---
layout: post
title: Deep Double Descent in Convolutional Neural Network
---

<style>body {text-align: justify}</style>

### Introduction to Deep Double Descent

The phenomenon of deep double descent was first postulate by Belkin et. al(2018). Belkin worked with the
neural network and observed that when one increases the width of a network(i.e., increase number of
neurones), the test error appears to increase for a range of width and then decreases and saturates. This
observation was reported continuously for different kinds of network architectures by even the predecessors
of Belkin including Opper (1995; 2001), Advani & Saxe (2017), Spigler et al. (2018), and Geiger et al.
(2019b).

The reason behind such an increase and then, decrease in the test error can be understood by combining two
conventional thought processes for the question ‘Whether larger models are better or worse?’. The two
conventional viewpoints can be understood as such,
* Bias Variance trade-off - The bias is a measure of how well the data performs on training dataset, whereas,
the variance is a measure of how well the model performs on test dataset given it trained upon a given
training dataset. The concept of Bias-Variance trade off is a fundamental concept of classical statistical
theory. According to the concept, after certain threshold width, the model starts to ‘overfit’ the training
dataset hence it performs poorly on the test data and it’s generalisation capability decreases. Therefore the
understanding is that the ‘larger models are worse’.
* The modern neural networks do not exhibit any such circumstance. The larger models have more
parameters hence are able to fit even random labels. Hence, the understanding becomes that ‘larger models
are better’.

![dd01]( \images\dd01.png)

The double descent is not only observed in case of varying model width but also in case of varying number
of epochs as discussed in the paper by Preetum Nakkiran et al.(2019). The paper also describes a measure of
complexity of model called effective model complexity and describes in detail the regions of the width versus
test error graph as shown in figure 1.

### Experimental Setup
#### Dataset
A study was conducted initially to understand which dataset to work with considering the training time taken 
to train upon CIFAR10 and CIFAR100. Run times were compared for the required architectures with 
CIFAR10 and CIFAR100. It was observed that working on CIFAR10, gives better results in terms of time 
and accuracy considering it has 10 classes. The performance on CIFAR100 data proved to be comparable to 
that on CIFAR10 in terms of runtime but accuracy suffers as it has 100 classes.

![dd02]( \images\dd02.png)

Note: Originally, in the paper by Preetam Nakkiran et al.(2019), the model was trained with Adam Optimiser for 
4000 epochs. At the current speed of training, training CNN on CIFAR100 for 4000 steps would take 15 hours 
approximately.

#### Architecture
Two of the architectures tested upon are based on the architectures used in the paper by Preetum 
Nakkiran(2019), where as, BCNN were added in the later part of the study. The following are the 
architectures considered:
1. Convolutional Neural Network - The CNN5 was used with layer width parameter as c, where the layer 
width varies [c, 2c, 4c, 5c] for the Convolutional layers. It was observed the base line model with c equal 
to 60 gives 80% accuracy on the CIFAR10 dataset.
2. Residual Neural Network - The ResNet18 architecture was employed with each ResNet block has it’s 
width varying in the same fashion i.e, [c, 2c, 4c, 8c] for the Convolutional layers. 
3. Binarised CNN - The BCNN was introduced later in the study to try and observe the interpolation 
effect(double descent) phenomenon. The basic structure of network is exactly the same as that of CNN5 
but with quantised convolutional layers introduced. Also, the BCNN unlike CNN5 is trained using Keras, 
not Pytorch.

#### Number of Epochs
The models of CNN and ResNet architectures are all trained for 40 epochs and mini-batch size of 128 due 
to computational constraint as only Google Colab’s Tesla GPU was used. The random seed of 0 is added in 
order to make sure the output remains constant for a value of c. It was still observed that there was certain 
variability between the values of test error obtained for each width parameter value. 
BCNN on the other hand has been analysed for two cases epoch 40 and epoch 400, as training of BCNN is 
considerably faster than CNN5. Understandably so, it is because of the better memory utilisation and use of 
bit wise operations instead of arithmetic ones that are used in CNN5. For better understanding, kindly refer 
to the paper [2] by Benjio et al.

#### Optimization
The standard adam optimiser is utilised with a learning rate of 10-4 for all the architectures.

#### Regularisation
No regularisation technique was utilised. Further study is required to see the impact of regularisation upon 
double descent phenomenon.

#### Experiments and Results
The main objective of each experiment conducted is to observe the interpolation phenomenon in CNN5, 
ResNet18 and BCNN. The experiment conducted in case of each architecture is explained below,
##### CNN5 
The Pytorch platform was used in order to try and observe the double descent phenomenon after the plotting 
of test errors for values of width parameter c. The c values were chosen at an interval of 5, starting with 1, 
up till 65. For each of c values the test error and train error attained, is plotted on the graph. Double descent 
phenomenon was observed only in one out of five tries with the complete width value set. There was greater 
variability between each experiment try result. And nothing could be predicted conclusively with respect why 
such a thing is happening, as double descent issue should be observed in all tried. It was however assumed 
that this issue may be due to the fact that the model is trained for very less number of epochs(40 epochs) due 
to the computational constraint. However, it was observed that the test error while trying to approach zero, 
saturates near the value of 0.15(15%) test error as can be seen in the Figure 2.

![dd03]( \images\dd03.png)

##### ResNet18
The Pytorch platform was used in order to try and observe the double descent phenomenon after the plotting 
of test errors and train errors for values of width parameter c. The c values were chosen at an interval of 5, 
starting with 1, up till 40. For each of c values the test error and train error attained, is plotted on the graph. 
There was no instance where double descent presented itself out of the 7 trials made. The graph showing 
width parameter on the x axis and the test and train errors on the y axis for one of the trials can be observed 
in figure 4 given

![dd04]( \images\dd04.png)

##### BCNN
There are two experiments conducted with respect to Binarise CNN. The experiment unlike the previous 
ones before is conducted on Keras instead on Pytorch. The change in choice was made because of the 
availability of larq library which can be used to get the quantised convolution layers and maxpool layers. The 
experiments conducted are:

a. Varying the width parameter
This experiment is same as the one conducted for the CNN5 and ResNet18. The width parameters are 
varied and the test and train errors are noted. The experiment is conducted for 40 and 400 epochs. The 
experiment is conducted for 40 epochs to compare the performance with the CNN5 and ResNet18 
performances for the same number of epochs. The figure 5 and figure 6 show the performance with respect 
to 40 and 400 epochs respectively, where width varies from 1 to 40. As one can observe here, there was no 
concrete appearance of double descent even after 6 trials.

![dd05]( \images\dd05.png)

b. Varying the number of CNN layer
Here, we try to understand how the test and train errors vary with respect to the addition of more CNN 
layers. The layers added are added with “same” padding and of the same width equal to 4c. The number of 
epochs used is 400. The minimum number of CNN layers is 3 with widths [c, 2c, 4c]. The performance of 
the model with respect to varying number of CNN layers is shown in the figure 7.

![dd06]( \images\dd06.png)

Given in the figure 8 is the table summarising what values of test and train error do the models saturate at 
when the width parameter is varied.

![dd07]( \images\dd07.png)

### Inference
To, draw any concrete conclusions from this study would be too soon. But several inferences can be drawn 
including:
* Double descent does not show for lower number of epochs.
* The test error doesn’t follow any known trend when number of CNN layers is increased.
* BNN performed very effectively with the training set but not so much with the test one.

### Future Work
The next stage of the study involves a comparative study between CNN and BCNN to formulate a 
benchmark to predict the performance of a CNN architecture by training it’s corresponding BCNN on the 
same datasets. For people with limited computational resources, it can be a good way of evaluating 
architecture performance on their dataset in less amount of time and higher number of epochs. This study 
would be based upon the observation the BCNN takes very less time in training for higher number of epochs 
as compared to CNN.

### References
1. Preetum Nakkiran et al. (2019), Deep Double Descent: where bigger model and more data hurt?
2. Itay Hubara et al. (2016), Binarized Neural Network