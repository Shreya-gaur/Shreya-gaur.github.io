---
layout: post
title: Fabrication of SAW Devices
---

<style>body {text-align: justify}</style>

### Understanding Surface acoustic Waves

The surface acoustic wave(SAW) is an acoustic wave traveling along the surface of a material exhibiting elasticity. These waves are the modes of propagation of elastic energy along the surface of the solid, whose displacement amplitude undergoes exponential decay. The SAW devices were first discovered by Lord Rayleigh in 1885, hence, SAW is also called Rayleigh waves. The Rayleigh waves have longitudinal and vertical shear components that can couple with any media in contact with the surface. This coupling strongly affects the amplitude and velocity of the wave, allowing SAW sensors to directly sense mass and mechanical properties.

Here, in SAW devices we will confine ourselves to the surface acoustic waves observed in case of a piezoelectric solid. A piezoelectric solid is the one which possesses the unique ability to transduce mechanical stress applied on it, into the form of a current and vice versa. Transduction refers to the conversion of one energy into another. Here, this transduction of mechanical stress into electrical current in the piezoelectric material is called the piezoelectric effect.

##### Structure & Working of SAW Device
If one considers the basic structure of a SAW device, it constitutes a substrate topped by a layer of piezoelectric solid and then, two IDTs, i.e., the input IDT and output IDT. The structure is clearly illustrated in the figure given below.

<!-- Image here for SAW Device-->

![Fab01]( \images\Fab01.jpg)

IDT stands for inter digital transducer. The IDTs are made using mono photolithography, which is discussed in detail later in the report. When an AC voltage is provided to the input IDT. The input AC voltage is converted to a form of surface acoustic wave, as the IDT is mounted on piezoelectric layer. The SAW propagates through the delay region to eventually reach the output IDT. The output IDT does the opposite of input IDT and converts the vibration applied to itself into an AC voltage. This voltage received is recorded and made sense out of in many of the SAW device applications.

The finger-like structure in the IDTs are actually electrodes made out of a metal. The metal used in the study for this purpose is Aluminum.

In the fabrication of SAW devices, ZnO is the piezoelectric solid used. One might ask why ZnO is used, there are other piezoelectric materials available like quartz, LiTaO3, LiNbO3, etc, as well. The answer lies in the layered structure of ZnO, which allows relatively higher SAW propagation velocity (5000 m/s). The other piezoelectric materials like quartz, LiTaO3, etc, show less SAW propagation velocity, i.e., in the range of 3000 to 4000 m/s. 

In all honesty, diamond materials possess the highest SAW propagation velocity of about 11000m/s, but the poly-crystalline diamond layers grown by hot filament CVD process usually have very large grain size and rough surface, which requires a huge amount of mechanical polishing to improve surface smoothness to a level good enough to be used in the fabrication process of SAW devices (Ra~5nm). Such processes are very time consuming and expensive. Hence, we can strike off diamond materials from the list of piezoelectric solids that can be used. But if you are really not under the constraint of budget, there are nanocrystalline diamond materials which don’t require such post-polishing processes and can be used for fabrication of SAW devices. In the study mentioned, however, we stick with ZnO.

### Fabrication Processes

A number of processes are carried out on a given substrate to fabricate a SAW device. In our study, as mentioned above we use two substrates. First is passivated silicon wafer. By the term “passivated”, we mean that there is a layer of Silicon oxide (SiO2) on the wafer, 2-4µm thick. The layer of SiO2 is said to be the ‘sacrificial’ layer.

The second substrate used is corning glass. Corning is the name of the company which produces this unique glass. The specialty of this glass is that it can withstand a temperature of 600°C, whereas other glasses can’t possibly tolerate such a high temperature, they might melt.

The processes involved in fabrication of a SAW devices are mentioned below sequentially:
1. Sputtering
2. Electron beam deposition
3. Mono photo-lithography
4. Developing
5. Etching
6. IV characteristics check
7. Packaging

#### SPUTTERING

Sputtering, by definition, is the deposition of metal or a target material on a surface by using fast ions to eject particles of it from the target. It can also be seen as the plasma etching of target material. As we know from the design, we need a thin layer of ZnO on the substrate. This layer is achieved by using the process of sputtering. The sputtering is of various kind based upon how the fast moving ions are achieved. However, we have used RF sputtering to achieve the required results, i.e., 4µm thick layer of ZnO.

##### Theory of RF sputtering
Plasma is one of the four fundamental states of matter. It is formed when an inert gas, like Argon, helium, argon, etc., get ionized upon being subjected to a very high intensity electric field. In sputtering, plasma plays the major role of removing molecules from target material’s surface by transferring their own momentum into them. Ar+ is the ion used in the sputtering process.

Radio frequency is used to form the plasma. It is used to ionize the Argon, introduced into the system, which is a very significant step in the process of sputtering. These Ar+ ions then bombard the target material, getting attracted to the cathode(target). The cathode is generally cooled because sputtering generates heat. The target molecules then float around, and some of it lands on the wafer(anode), hence, the thin layer of the target material on the wafer is attained. The anode can be cooled or heated.

<!-- Image for Sputtering -->

![Fab02]( \images\Fab02.jpg )

##### Process
The substrates used, must firstly undergo the cleaning process in order to remove most of the impurities from the substrates’ surface. The substrate is attached to the substrate holders using pins as shown in fig 1. This substrate holder is then placed inside the cylindrical sputtering chamber in an upright position. The target is labeled in the figure shown. The distance between the target and the substrate should optimally be 11.5cm. The chamber is then closed and the all the gas inlets are off.

<!-- Two process images -->

![Fab01]( \images\Fab03.jpg "Substrates on substrate holder")

![Fab01]( \images\Fab04.jpg "Chamber in RF sputtering machine")

All the air is vented out of the cylindrical sputtering equipment. The rotary pump is turned on in order to create a vacuum of the order of 10-2. Upon achieving the vacuum of the mentioned order, the turbo pump is turned on. The pumps are allowed to work till the vacuum of about 10-6 order is attained. This is done to assure that no air remains inside the chamber, which further ensures that no impurity pertains which may be able to strand or interfere with the process in any way. 

One must know that it takes a significant amount of time to attain the vacuum level of 10-6 order. The time is generally 6 to 8 hours. So, one must equip patience while engaging in the process. 

When the atmosphere inside the chamber is clean, the Argon inlet is opened, so as to allow the formation of plasma. The process takes place at about vacuum of order 10-2. The plasma once formed initiates deposition, as ionized argon molecules transfer their momentum to the target material and remove molecules. These molecules once freed roam freely in the chamber and some of them may attach to the substrate’s surface. Therefore, a thin layer of ZnO is deposited on the surface of the substrate. The time taken after the formation of plasma estimates to somewhere between 6 to 7 hours for proper deposition to occur.

#### ELECTRON BEAM DEPOSITION

##### Theory of E. Beam Deposition
The electron beam is concentrated on a target metal. This raises the temperature of the metal to the point of vaporization of the metal. Once the metal vaporizes it is made to conveniently condense on the wafer placed in vicinity. Hence, a thin film of metal is formed on the substrate’s surface.

##### Process
The substrate, after ZnO deposition, is loaded in an electron beam machine. Vacuum is created in the chamber to remove the chances of encountering impurities. The vacuum is created by using the backing and roughing valve. A very high vacuum is created, of the order 10-6 and a temperature of 100°C is attained. 

The metal of choice is aluminum (Al). A thin film of aluminum is formed on the surface coated with ZnO. The metal is placed on a boat. The electron beam is focused on the metal. The metal first melts and then vaporizes. The vapor condenses on the substrate and hence a thin film of Aluminum is formed.

<!-- Image of Electron Beam Machine -->
![Fab05]( \images\Fab05.jpg "Electron Beam Machine")

#### MONO PHOTO-LITHOGRAPHY
 
From this point forth in the fabrication of SAW, the substrate coated with ZnO and Aluminum will be called “work piece” for convenience, in the following report. The substrate surface is spin coated with photo-resist. The spin coating machine is effectively run for 1 minute at 1000 rpm. This forms a uniform coat of photo-resist on the surface of the substrate. One must remember that the process must be done in yellow light. The bonds of photo-rest are weaker with the substrate in yellow light. 

<!-- Image of Spin Coating Machine and SUbstrate caoted -->
![Fab01]( \images\Fab06.jpg "Spin Coating Machine")
![Fab01]( \images\Fab07.jpg "Substrate Spin Coated with photo resist")

After spin coating, the workpiece, already coated with photo-resist, is kept in the heater at a temperature of 90°C for half an hour. This is “pre-baking”. The heated workpiece is then placed under a mask aligner such that parts of the workpiece coated with photo-resist can selectively undergo UV exposure. The photoresist when exposed to UV rays, strengthens its bonds with the work piece. The mask plays a major role in the above process. The parts which are not supposed to be left covered with the photo-resist are not exposed to UV rays. The UV exposure is done for about 13 seconds. Note that the mask used is unique to the SAW device.

<!-- Mask Machine Microscope -->
![Fab08]( \images\Fab08.jpg "Mask for SAW Device")
![Fab09]( \images\Fab09.png "Microscope")


##### DEVELOPING

The workpiece, after being selectively exposed to UV rays, is then straightaway transferred to a developing solution. It is kept in the solution for 2 minutes. The developing solution is used to remove the photoresist from the parts of the workpiece, not exposed to UV rays. For example, the delay region. This is a crucial step because if not done carefully, the parts exposed to the UV ray will also have to part with their protective photoresist. The work piece is transferred to a microscope for checking, such that one makes sure that the sample is developed properly.

<!-- Contact Pad Image under microscope -->

![Fab10]( \images\Fab10.jpg "Contact Pad seen under microscope")

![Fab11]( \images\Fab11.jpg "Image under Microscope")

After development of the sample, it is kept at a temperature of 120°C for 20 minutes and then taken out for etching.

##### ETCHING

Etching is the process which can be described as the opposite of depositing. The Aluminum is supposed to be etched from the surface except from the electrodes of IDT. This is where the photoresist plays a major role. It protects the aluminum that forms the electrodes from getting etched.
The etchant is a solution containing potassium ferricyanide, potassium chloride and deionized water in the ratio 10:1:100. The work piece is dipped with the etchant and then, cleaned with deionized water. Etching is again a crucial process and should be done carefully. Aluminum is used to make the electrodes, as it is a very stable and conductive metal. Gold and Platinum have better properties than Aluminum, but they are highly expensive. 

<!-- Work Dipped in Etchant -->

![Fab12]( \images\Fab12.png "Work Dipped in Etchant")

##### PACKAGING 

The work piece is diced in order to isolate the devices on the wafer or the corning glass substrate. The device that resulted is of dimensions 1 cm and 0.5 cm and has passed the scrutiny of the checking process is then placed on a header and the contact pads on the device are connected to the contact pads of the header. This is done to establish an internal connection between the pins of the packaging and the actual device.

The device after the cap is put on the header, is input into a Vector Network Analyzer. The resultant is the graph depicted in the image given below.

<!-- SAW on header Vector Network Analyser -->
![Fab13]( \images\Fab13.jpg "SAW on header")

![Fab14]( \images\Fab14.jpg "Vector Network Analyser")

In a Vector Network Analyzer, an electromagnetic wave is input into the device. The input electromagnetic wave is of the frequency range 50 to 150 MHz. The output wave is observed. The graph of gain and frequency is made and a number of observations are derived from it. 

The frequency which is allowed to pass through our filter is given by the formulae,

<div style="text-align: center;">
F=v/λ 
</div>

where, F = frequency allowed to pass, 
	   λ = acoustic wavelength
	and v is given by the property of substrates.

<!-- Substrate Info to VNA -->
<!-- ![Fab15]( \images\Fab15.jpg "Substrate input to VNA") -->

One of the most significant observations is that our filter is able to filter a frequency of 98.18Mhz, as from the graph the loss is minimum in this case. 

### Conclusion
 
The fabrication of an Integrated SAW Device filter is complete. The filter is able to let an AC voltage of frequency 98.18 MHz pass through it. The SAW filters find their applications in a number of communication systems, for example, mobile/wireless transceivers, radio frequency and intermediate frequency filters, etc.