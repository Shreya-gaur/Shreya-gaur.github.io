---
layout: post
title: Load Value Prediction
---

<style>body {text-align: justify}</style>

### Introduction

Load value locality is the concept of recurring values over time from particular static load instructions. If a load can be identified as consistently returning the same value, the CPU can exploit this pattern by predicting the result before the load instruction even executes, thus saving time and potentially memory bandwidth. Such value speculation is like branch prediction except with a whole word instead of one bit. Methods are required to verify load value predictions and also to correct mispredictions. In this report, we review prior work on load value prediction, discuss our designs and experiments to implement the concept in gem5, and examine our statistical results measuring rates of locality and final performance changes. The implementation for this project can be found on [this repository.](https://github.com/BullPointe/ECE565Project)

##### Related Work

The benefit of value locality is two-fold: First, the early availability of predicted load values enables dependent instructions to execute sooner and, second, accurate predictions relieve the memory system from fetching redundant data. In 1995, the first of these concepts was introduced to perform loads very early and verify in parallel to achieve zero-cycle loads in some cases [Aus95](https://web.eecs.umich.edu/~taustin/papers/MICRO28-zcl.pdf). The second concept is that the CPU can bypass the data caches altogether, thus acquiring data that may not even exist in cache. Both concepts were explored in the seminal paper on value locality from 1996. Their basic structures were the load value prediction table (LVPT), load classification table (LCT), and constant verification unit (CVU) [Lip96](https://course.ece.cmu.edu/~ece740/f10/lib/exe/fetch.php?media=valuelocalityandloadvalueprediction.pdf). The LVPT caches prior loaded data values. When it predicts a value, it forwards it to younger instruction. The LCT classifies the confidence as “unpredictable”, “predictable”, or “constant”. In the first two cases, the load must execute fully on data cache, then compare the true load value with the prediction. If correct, the LCT increases its confidence; otherwise, it decreases. “Constant” loads do not access memory at all. In 1998, researchers noted that mispredictions do not require all younger instructions to squash, but only those with directly-dependent data [Bur98](https://userweb.cs.txstate.edu/~burtscher/papers/tr98.pdf). Unfortunately, this selective squashing technique was considered too hard at the time [Pai98](https://core.ac.uk/download/pdf/54846841.pdf). Other methods to increase prediction accuracy included tracking prediction history and using statistical inference.

### Implementation details

#### 1. Load Value Prediction and Classification Table

It was unnecessary to have separate LVPT and LCT tables, due to the only additional payload being the data stored in the LCT. The new table LVPCT contains all the functions that were implemented in their respective objects. These functions include: lookup, using an instruction address to identify a classification and corresponding value; upgrade/downgrade classification, which updates the LVPCT based on whether the value matches the current value and modifying the classification. The structure of an LVPCT entry looks like this:

| TID | Classification (Can be Unpredictable 1, Unpredictable 2, Predictable or Constant) | Value (uint64_t) |

No valid bit is required as the entries marked with Unpredictable 1 and 2 are considered invalid. The table has 1024 entries and is direct-mapped without tags.


#### 2. Constant Verification Unit

The CVU is implemented as specified in the original paper. The replacement policy used is FIFO. It is hardcoded with 32 entries, since the constant loads will be few. The complexity of a CAM search could not be coded so we chose a simpler structure, a vector. These functions were included: insertConstantLoad, checkForStore, checkForConstantLoad, and isFull, which inserts the constant load into CVU, checks for presence of a store’s data address in CVU, and checks for the presence of an executed constant load for validation purpose. The checkForStore also invalidates entries in the CVU, which entails entry deletion. The structure of a CVU entry looks like this:

| TID | Data Address | Index in the LVPCT table |

#### 3. Forwarding and Verification

![lvp01](.\images\lvp01.png)

In the dispatch stage, the load gets its classification using lookup on the LVPCT table. The value from the table is copied into the destination register. The instruction is, then, inserted into the instruction queue with LSQ::insertLoad and the dependent instructions are notified of the available value. To notify dependents, we follow the pattern of existing dependence-management code with some modifications (see InstructionQueue::wakeLoadDependents). The dependent instructions are removed from a waiting queue (or dependency graph) and the corresponding register scoreboard entry is marked as ready. Thus, the predicted value is forwarded early. The purpose of “value forwarding” in our context is to send predicted load values to dependent instructions early on in execution, such that dependent instructions can operate sooner than they otherwise would have. 

To verify predictable loads, it needs to execute normally in the LSQ and get the true value from memory. The verification is done by IEW::checkMisprediction called in LSQUnit::writeback (see Fig.1). Once the true value writes-back to the destination register, it is compared with the predicted value. If they are equal, then the load commits normally. If they are unequal, the load still commits normally as the value for the load is corrected by usual execution of LSQUnit::executeLoad. All subsequent instructions, though, are squashed. We achieve this squashing by marking the instructions as a false branch prediction and allowing the branch squashing mechanism to operate. It is not necessary to squash mispredicted predictable loads because they are self-correcting after the writeback stage. 

Constant loads, however, cannot be corrected in this same way because they are supposed to skip the memory system altogether. Thus, mispredicted constant loads must be squashed along with all subsequent instructions. Constant loads are verified in the writeback stage by checking the CVU for existence of a matching entry (see earlier section on CVU). If a match is found, then it commits normally. Otherwise, it must squash. We have had many difficulties making constant load verification successful. See the appendix for more details on this. The CVU is kept coherent with memory by invalidating entries whose data address matches incoming stores. 

### Benchmarks
The benchmarks used to evaluate the LVPT were the following: deep sjeng, lbm, leela, mcf, imgick. Unfortunately bwaves could not run on the x86 system (segmentation fault). So, we replaced it with leela. We evaluated the implementation with the number of cycles for the benchmark. It can be identified whether the LVPT is optimizing the O3 processor based on the improvement in number of cycles. In addition, metrics involving the unit’s prediction quality were correctly/incorrectly predicted values, and time spent within each classification.

### Results

![lvp02](.\images\lvp02.png)

The figure 2 demonstrates the accuracy of the LVPT, with higher percentages the unit is able to upgrade an instruction to a “constant” classification where its value can be forwarded effectively. Unfortunately as seen in lbm we are seeing higher percentages of incorrect predictions, 80%, therefore it can be deemed to take longer to reach said level as it will thrash between “unpredictable” states. However, for the other benchmarks we are seeing percentage levels of near 40% to 50% which is significantly better than lbm. For these benchmarks, the data is more predictable and could be optimized better than the rest.

![lvp03](.\images\lvp03.png)

The figure 3 above shows that during the run of each benchmark the “unpredictable” state was the most seen state for the load instructions. What this represents is that the behavior for each instruction is thrashing between values and a result cannot be considered constant for more than 50% of the time. While for the lbm benchmark it was clear that there was a significant difference comparatively where the majority of the instructions were held at constant. This figure provides insight on how much work could have been optimized in relation to each benchmark. Benchmarks with a high percentage of “constant” are expected to see higher possible improvements to execution.

![lvp04](.\images\lvp04.png)

The figure 4 demonstrates the optimization done by the Load Value Prediction Unit for each benchmark. There is minimal optimization to the benchmarks; this is due to the inability to accurately predict constant values. As discussed before this hinders the unit's ability to fully optimize the processor. Some of the number of cycles are larger with the LVPT due to numerous incorrect predictions. However, with the prediction for “predictable” loads, the figure 4 accurately displays slight optimizations and reductions in the number of cycles. Therefore the data provides inconclusive results on how effective the load value prediction unit is with the selected benchmarks.

### Conclusion
Based on our working implementation of predicting load value, it would be seen that the simulations took very short time due to early forwarding of values. The squashing in case of misprediction of predictable loads was also performed effectively leading. In case of constant loads, the performance could not be observed even after rigorous testing of our approach due to a core dump. More on that can be read about in the appendix.

### Appendix
#### Constant Load Prediction
* Value Forwarding
<p>
	The value prediction for the Constant Loads is conducted in the same way as that of the Predictable loads. The dependent instructions are removed from a waiting queue (or dependency graph) and the corresponding register scoreboard entry is marked as ready. Thus, the predicted value is forwarded early. 
</p>
* Verification
<p>
	For Constant Loads the checkMispredicton function calls the badConstantLoadPrediction function which verifies whether a constant load exists in the CVU or not. Furthermore, when the LSQ executes a store instruction, it searches for matching data addresses in the CVU and invalidates all found entries.
</p> 
* Bypassing Memory
<p>
	We were unable to successfully figure out how to bypass the memory. We tried several methods including bypassing the LSQUnit::initiateAcc method in the LSQUnit::executeLoad method in LSQ and the completeAcc call in LSQ::writeBack. We also tried preventing constant loads from being inserted into the LSQ at all, but this caused problems with the load instruction never reaching the verification stage or commit. We tried spoofing constant load execution by allowing them to complete in memory and then restoring the prediction afterward. At least then, we could determine if the verification logic was correct.
</p>
* Squashing on Misprediction
<p>
	We tried squashing the instructions using the squashDueToMemOrder methods in IEW stage. Function squashDueToMemOrder squashed both the load instructions and the dependents. This resulted in the program hanging because of bad squashing. We next tried to test using the squashDueToBranch which does not squash the load instruction but only the dependents. In order to do so, we provided the correct value to the load instruction using the getStashedValue method of DynInsts and then used squashDueToBranch. This caused a Core Dumped error with the fault “Tried to execute unmapped address 0”.
</p>