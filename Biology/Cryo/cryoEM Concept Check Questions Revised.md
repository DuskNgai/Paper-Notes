# 0 Introduction - Why electrons?

1. What are the advantages and disadvantages of electrons compared to photons for microscopy? Neutrons?

    > Advantages to photons: shorter wavelengths than visible light, easier to focus than X-ray.
    >
    > Disadvantages to photons: damaging sample compare to visible light, poor penetration than X-ray.
    >
    > Advantages to neutrons: being easier to produce and focus.
    >
    > Disadvantages to neutrons: damaging sample.

2. What structural biological technologies give higher resolution information than cryo-EM, and what kinds of samples and questions can they address?

    > X-ray crystallography and Nuclear Magnetic Resonance spectroscopy. Small proteins and domains.

3. What structural biological technologies complement cryo-EM at lower resolutions, and what kinds of questions and samples do they address?

    > cryo-ET. Multi-protein reactions.

# 1 Basic Anatomy of the EM

## 1-1 Electron guns

1. Where do the imaging electrons in an electron microscope come from?

    > Electron gun, which is a bent wire line with current.

2. What part of the gun is called the "cathode"? What should be called the "anode"?

    > Filament. Ground.

3. What is the accelerator stack a "stack" of?

    > A series of perforated disks are connected to a series of resistors. Voltage is increased from -300 kV to 0 V disk by disk.

4. What voltages are typically used in transmission electron microscopes? What kinds of electron wavelengths does this correspond to?

    > 300 kV.
    > $$
    > \lambda=h/p=h/\sqrt{2mUe}\approx2\mathrm{pm}
    > $$

5. What does it mean to "condition" the gun?

    > Increase the normal operating voltage when discharging.

6. What is the difference between spatial and temporal coherence?

    > Spatial coherence: All the electrons come from the same direction.
    >
    > Temporal coherence: All the electrons have the same speed.

7. What are the three main types of electron guns? What are the advantages and disadvantages of each?

    > 1. Tungsten filaments: cheap, not coherent (Volcano-like).
    > 2. Lanthanum hexaboride: more coherent.
    > 3. Field emission gun: expensive, greatly coherent (Child-slide-like).

## 1-2 Electron lenses 

1. What is the defining property of a "lens"?

    > Bent the light and let them focus.

2. Why/how do optical lenses focus light?

    > Huygens' Principle: When a wave encounters atoms, the atoms perform as scattering centers. As a wave encounters dense material, the wave slows down, and wavelength compresses. Waves in different paths have the same optical path lengths means they are all in phase. So they form the image.

3. Draw a diagram that shows how a lens can be used to form a magnified image. What parameters determine the magnification?

    > Pass.
    >
    > Focal length & Object distance. If the object distance is between 1 and 2 times the focal length, the image distance is greater than 2 times the focal length.

4. How do electron lenses focus electrons?

    > The electrons along the axis will go straight ahead. Other electrons will be focused by the magnetic field excited by the alternating current.

5. Why do electron images rotate in an electron lens?

    > Electrons that are off-axis will rotate in a magnetic field. As a result, the image will also rotate.

6. What are the four main components of an electron lens system? What does each do?

    > *Deflector*: Adjust the electrons produced by the electron gun going along the axis.
    >
    > *Lens*: Focus the electrons.
    >
    > *Stigmator*: Compensate for the asymmetric magnetic field of the lens to pull the electrons to the optical axis.
    >
    > *Aperture*: Select the electrons near the optical axis.

## 1-3 The column

1. What are the three main lens systems in an electron microscope called?

    > - Condenser lens system;
    > - Objective lens system;
    > - Projector lens system.

2. What is meant by a "conjugate plane"?

    > The images on conjugate planes have the same wave function with different magnified factors.

3. What are the special names given to the three independent sets of deflectors?

    > - "gun" deflector
    > - "beam" deflector
    > - "image" deflector
    > - "projector" deflector

4. What current is controlled by the "filament"? "emission"? "spot size"? "intensity"? "focus"? "magnification"?

    > Filament: Current passing through the filament.
    >
    > Emission: Voltage between Wehnelt cylinder and accelerator stack.
    >
    > Spot size: Current passing through the upper condenser lenses.
    >
    > Intensity: Current passing through the lower condenser lenses, affecting the crossover position.
    >
    > Focus: Current passing through the objective lenses.
    >
    > Magnification: Current passing through the intermediate lenses.

5. What is controlled by the "high tension" knob?

    > The voltage drop in the accelerator stack.

6. What is a "crossover"?

    > The position that the electrons pass through the condenser lens and be focused above the specimen. 

7. Which knob controls whether the microscope is in "LM," "M," or "SA" mode? What currents change?

    > LM: Only the projector lens is on.
    >
    > M: The intermediate lens and the projector lens are on.
    >
    > SA: All three lenses are on.

8. What are "pivot points"?

    > Each deflector set has a pivot point. At the pivot point, the electrons tilt without shift or shift without tilt.

9. What does it mean to "align" the microscope?

    > Set deflectors and let the electrons along the optical axis form a good image.

10. What is "hysteresis"?

    > As one increases the current magnification, the magnetic field magnification increases. Meanwhile, the material of the coils gets magnetized temporally, so as you decrease the current magnification, the magnetic field magnification will be higher for a fixed magnification of the current.

11. What does the "normalize" button do?

    > It increases the current to the maximum, then decreases it to the latest, and then increases it again to the initial state to eliminate the hysteresis effect.

## 1-4 The sample chamber

1. In what directions/ways can the sample be moved while in the microscope?

    > Move in & out, rise & fall.

2. Where does the sample rest with respect to the objective lens?

    > Between pole pieces gap of objective lenses.

3. What is the "pole piece gap"?

    > Gap between two lenses (magnetic poles).

4. What is a "cryo-box"?

    > The tiny box in EM wraps around the sample to block the water molecules.

5. What is "eucentric height"? Is it different for every grid?

    > When rotating the sample inside the EM around the center axis of the insertion tube, a horizontal axis, the eucentric height is the height of the rotating axis. It is different for every grid.

## 1-5 Energy filters

1. Why are EM energy filters used?

    > Filter the inelastically scattered electrons which have lower energy.

2. How are "post-column" filters different from "in-column" filters?

    > one bent (C-shape) / three bent ($\Omega$-shape)

3. What is a typical slit width for cryo-EM?

    > 20 electronic voltage.

4. What is a "zero-loss" peak?

    > A peak that no energy loss in the energy-amount graph.

5. How could an energy filter allow you to image where a particular element was in the sample?

    > Use electrons with a certain amount of energy and get information from an image.

## 1-6 Electron detectors

1. Name five different types of electron detectors.

    > Fluorescent screens, TVs, Photographic film, CCDs, "Direct" detectors.

2. What is a "CCD"? How do they work?

    > The top layer of the CCD is a scintillator, which produces photons in all directions. When the photons hit the second layer of fiber optics, they are captured by the fiber optics and carried down to specific pixels in the CCD layer. The CCD layer can be read.

3. What are the advantages and disadvantages of film versus CCDs?

    >  Photons produced by a CCD may be captured in a distant and irrelevant place, creating noise. Photons generated by electrons are different in amounts.

4. What is meant by "direct" detector?

    > The electrons are detected directly by the CMOS detector.

5. What new capabilities do direct detectors provide?

    > Real-time and each burst in the image can be viewed as a single electron hit.

## 1-7 Vacuum systems

1. Name four different types of vacuum pumps. How does each work?

    > - Mechanical pump: Use a mechanic rotary rotor to push the air out of the column.
    > - Oil-diffusion pump: Use oil molecules to hit the air molecules and move them to the mechanical pump.
    > - Turbo-molecular pump: Use stator vanes and rotor vanes to push the air molecules down out.
    > - Ion getter pump: Ionize the air molecules.

2. Why are so many different types of pumps needed?

    > They work under different ranges of air pressure and are used one after one.

3. What is a "backing" pump?

    > Mechanical, Oil diffusion, and Ion getter pump are working one after one.

## 1-8 Summary, safety

1. What is the purpose of the heavy lead shielding covering electron microscopes?

    > As the electrons hit the material, they release X-rays.

2. Why do electron microscopes need chilled water? 

    > Maintaining a thermodynamic equilibrium.

3. Name three lethal and at least one more non-lethal hazards associated with electron cryo-microscopes

    > - X-rays.
    > - Sulfur hexafluoride leakage.
    > - Liquid nitrogen evaporating.
    > - Freezing burns.

# 2 Fourier Transform for Beginners

## 2-1 One-dimensional sine waves and their sums

1. What four parameters define a sine wave?

    > Amplitude, Wavelength/Frequency, Phase shift, Direction.

2. What is the difference between a temporal and a spatial frequency?

    > Temporal: how long of a time.
    >
    > Spatial: how long of length.

3. What in essence is a "Fourier transform"?

    > Decompose a function that is the sum of a sequence of sine and cosine functions. Express functions from the spatial domain to the frequency domain.

4. How can the amplitude of each Fourier component of a waveform be found?

    > $$
    > \begin{align*}
    > f(x)&=\frac{a_0}{2}+\sum_{n=1}^{\infty}\left[a_n\cos\left(\frac{2\pi nx}{\lambda}\right)+b_n\sin\left(\frac{2\pi nx}{\lambda}\right)\right]\\
    > &=\frac{a_0}{2}+\sum_{n=1}^{\infty}A_n\sin\left(\frac{2\pi nx}{\lambda}+\phi_n\right)\\
    > a_n&=\frac{2}{T}\int^{\lambda}_{0}f(x)\cos\left(\frac{2\pi nx}{\lambda}\right)dx\\
    > b_n&=\frac{2}{T}\int^{\lambda}_{0}f(x)\sin\left(\frac{2\pi nx}{\lambda}\right)dx\\
    > \end{align*}
    > $$

## 2-2 One-dimensional reciprocal space

1. What is the difference between an "analog" and a "digital" image?

    > Analog is a continuous signal, and digital is a discrete signal.

2. What is the "fundamental" frequency? A "harmonic"? "Nyquist" frequency?

    > - Fundamental: the minimal common frequency.
    > - Harmonic: 2 times of fundamental frequency.
    > - Nyquist: Highest frequency, oscillating every pixel in the digital image.

3. What is "reciprocal" space? What are the axes?

    > The Fourier space. Amplification - Spatial Frequency.

4. What does a plot of the Fourier transform of a function in reciprocal space tell you?

    > The frequency and corresponding amplification.

## 2-3 Two-dimensional waves and images

1. What does a two-dimensional sine wave look like?

    > Similar to a one-dimensional sine wave with one dimension extended without changing amplification.

2. What do the "Miller" indices "h" and "k" indicate?

    > h: the number of oscillations in the X-axis.
    >
    > k: the number of oscillations in the Y-axis.

## 2-4 Two-dimensional transforms and filters

1. In the Fourier transform of a real image, how much of reciprocal space (positive and negative values of "h" and "k") is unique?

    > 2

2. If an image "I" is the sum of several component images, what is the relationship of its Fourier transform to the Fourier transforms of the component images?

    > Sum.

3. What part of a Fourier transform is not displayed in a power spectrum?

    > Phase.

4. What does the "resolution" of a particular pixel in reciprocal space refer to? What is a "low pass" filter? "High pass"? "Band pass"?

    > Pixels near the origin in reciprocal space represent low frequencies, while pixels near the edge represent high frequencies.
    >
    > Low frequency carries approximate information.
    >
    > High frequency carries detailed information.

## 2-5 Three-dimensional waves and transforms 

1. What does a three-dimensional sine wave look like?

    > Easy to imagine.

2. What does the third "Miller" index "l" represent?

    > l: the number of oscillations in the Z-axis.

## 2-6 Convolution and cross-correlation

1. What is a "convolution"?

    > The sum of every pixel response to the original image. 

2. What is the "convolution theorem"?

    > Convolution of two signals is the production of their Fourier transforms.

3. What is a "point spread function"?

    > Impulse response in signal and system.

4. What is "cross-correlation"?

    > Find the most correlation between two signals.

5. How might cross-correlations be used in cryo-EM?

    > Template matching: Find cells or proteins.


# 3 Image Formation

## 3-1 Amplitude and phase contrast 

1. In what ways are inelastic and elastic scattering different? What causes them?

    > The path of an electron and its energy after it interacts with an atom. If the electron does not lose energy after interacting with the atom, it is elastic scattering.

2. What signals emerge from scattering events in the electron microscope that can be measured, and how do they lead to the three main types of electron microscopy?

    > - SEM: Detect back-scattered primaries charged particle.
    > - EXMA: Detect X-rays and visible lights emitted by electrons.
    > - Primaries STEM: Detect inelastic scattered electrons.
    > - Primaries TEM: Detect unscattered electrons that form the images.

3. How does amplitude contrast arise?

    > Electrons hit the sample and are scattered. As they propagate to the detector, a portion of them are unfocused or removed by apertures and energy filters. The electrons carry energy that can be detected by the detector. So fewer electrons hit in the area of the high scattering object than in the vicinity.

4. Why is it easier to explain amplitude contrast if we envision imaging electrons as particles?

    > We only receive the electron energy affected by the amplitude.

5. Why does phase contrast require us to think of imaging electrons as waves?

    > Interference is affected by both amplitude and phase. An electron wave hits a sample and changes its phase, then reaches the detector and interferes with others, thus changing the probability that each electron will be detected at any position in the image.

6. What is a "plane wave"? What about a plane wave changes as it travels through a vacuum?

    > A wave propagates as a plane. The phase increases. In the form of:
    > $$
    > f(\mathbf{x}, t)=Ae^{i(\mathbf{k}\cdot\mathbf{x}\mp\omega t)}
    > $$

7. Explain how/why atoms scatter X-rays.

    > X-rays are a constantly changing electromagnetic wave. Therefore, it excites electrons to produce an electromagnetic field, which in turn produces electromagnetic waves. Macroscopically, the X-rays appear to be scattered in all directions.

8. Why are there discrete peaks in the scattering from crystals?

    >  N-slit interference or so-called Fraunhofer Diffraction.

9. What information is delivered by each peak?

    > Each peak represents a Fourier component identified by $(h, k, l)$ indices. Each has an amplitude and a phase.

## 3-2 Wave propagation and phase shifts

1. How is the scattering from an object converted into an image in a microscope?

    > Focal Plane Ray Tracing: All scattered electrons scattered in the same parallel direction are focused on the same focal point. All scattered electrons from the same sample point will converge at the same location on the detector.

2. What is the relationship between the density of the sample and the wave function present on the back focal plane of the objective lens? The image plane? Can you draw a picture showing why?

    > A Fourier transform of the wave function occurs at the back focal plane. The electron waves hit the sample and are decomposed by the lens into the back focal plane, which is the Fourier transform. They are then synthesized again to form an image, which is the inverse Fourier transform. The image is a magnified replica of the sample.

3. How are plane waves represented in an "Argand" diagram? What are the axes?

    > Real and imagine axes. The wave is represented as a vector. The length represents amplitude, and the angle represents the phase.

4. Why were Argand diagrams introduced (how do they help us understand wave propagation and interference)?

    > Since waves can aggregate, the vector sum of two wave vectors represents the aggregated wave. 

5. How does the phase difference between two waves of identical frequency affect their interference?

    > Follow the rule of the vector sum. $\Delta\theta=0$, will enhance, $\Delta\theta=90$, small inference, $\Delta\theta=180$, will diminish.

6. What property of an electron wave gives the probability of its detection at each position?

    > The square of the electron wave gives the probability of its detection at each position.

## 3-3 The contrast transfer function

1. What two factors make the phase of a scattered component of a wave different from that of an unscattered component?

    > 1. The scattered wave suffers a 90-degree phase behind the unscattered wave.
    > 2. The scattered wave travels a longer optical path length than the unscattered wave.
    >
    > When they reach the image plane, they interfere with each other and produce a phase shift.

2. The contrast transfer function is typically plotted as a sinusoidally-varying function of what variable (what is the horizontal axis)? What quantity is plotted on the vertical axis?

    > CTF is a band pass filter.
    >
    > The horizontal axis is the spatial frequency in the posterior focal plane, representing the different components of the Fourier transform.
    > Vertical axis is contrast.

3. What is the CTF's domain and range?

    > Domain: $[0, \infty)$.
    > Range: $[-1, 1]$.

4. What does a "contrast transfer" of 1.0 mean? -1? 0?

    > - 1.0 means that the spatial frequency fully contributes to the final image.
    > - -1.0 means that the spatial frequency contributes exactly the opposite to the final image.
    > - 0 means that the spatial frequency does not contribute to the final image.

5. Why does the CTF oscillate sinusoidally?

    > This is because the optical path length of a scattered wave increases with its spatial frequency. This causes the phase shift to increase. Since the phase shift is periodic, the contrast transfer function oscillates as a sine wave.

6. What four variables appear in the argument of the sine function?

    > $$
    > \text{CTF}(\omega)=\sin\left(-\pi\cdot\Delta z\cdot\lambda\cdot\omega^2+\frac{\pi\cdot C\cdot\lambda^3\cdot\omega^4}{2}\right)
    > $$
    >
    > - $\Delta z$ is the defocus.
    > - $\lambda$ is the wavelength.
    > - $\omega$ is the frequency.
    > - $C$ is the spherical abbreviation.

## 3-4 Defocus and its effects

1. What is a "Thon" ring?

    > The graph of the square of the CTF.

2. How can the defocus of a TEM image be determined?

    > The distance between the predetermined image plane and the sample plane. And the focal length of the objective lens.

3. Why is defocus part of the argument of the CTF sine function?

    > It changes the optical path length, which changes the phase. So the CTF will be shifted. The more defocus, the higher frequency of the CTF sine function.

4. Does increasing the current in the objective lens make the image more or less defocussed?

    > Less defocused.

5. What is "over-focus"?

    > If the image is formed above the intended image plane, we call it overfocus.

6. How do heavily defocussed images look different than "closer-to-focus" images?

    > The specified spatial frequency on a heavily defocused image has a higher CTF value than on a "closer to focus" image.

7. What are the advantages of taking pictures far from focus? close to focus?

    > Far: easy to get particle position (low frequency) but hard to get high-frequency details.

## 3-5 Envelopes

1. What effect does partial spatial coherence have on the CTF? Why?

    > The electrons are not focused exactly on the same point. The low spatial frequncies in the CTF are not affected, but the high spatial frequencies are diminished. So the envelope of the CTF looks like a low-pass filter.

2. What effect does partial temporal coherence have on the CTF? Why?

    > The electons get both over-focused and defocused. So the CTF value is diminished.

3. What is their combined effect?

    > The net effect is the product of the two effects. The high spatial frequencies are diminished.

4. How do these effects depend on defocus?

    > Since the CTF depends on the defocus, the higher the defocus, the faster the CTF value diminishes.

## 3-6 CTF Correction

1. What is a "point spread function"?

    > A function that describes the relationship between a point before and after a image formation process.

2. How is the point spread function related to the CTF?

    > The point spread function is the Fourier transform of the CTF.

3. What is the relationship between the wave function that exists on the back focal plane of the microscope and the Fourier transform of the recorded image?

    > Image is formed by the convolution of the point spread function and the object function.

4. How (conceptually) can EM images be "CTF-corrected"?

    > We can deconvolute the image by the point spread function to get the object function. We can do it in frequency domain: by dividing the Fourier transform of the image by the CTF, then do the inverse Fourier transform.

5. How can the CTF of a TEM image be determined?

    > Calculate the Fourier transform of the image, and then take radiL means of the power spectrum. The result is the CTF.

6. What special issue arises at CTF-zeros? How can it be handled?

    > Zero division. Exclude the points near the CTF-zeros from the calculation and set the result to zero.

7. What would it mean if someone said they "CTF-corrected by phase-flipping only"?

    > Just take the absolute value of the CTF, which means shift the phase 180 degrees of the frequency components with negative CTF values.

8. How can the information loss at CTF-zeros be overcome?

    > Take the image at different defocuses and combine them.

# 4 Fundamental Challenges in Biological TEM

## 4-1 Sample prep - Room temperature methods

1. Why can't cells just be inserted into the microscope and imaged (without any special preparation)?

    > The environment in the microscope is vacuum, which will cause the liquid in the cell to boil and distroy the cell.

2. What is "chemical fixation", what agents are used to do it, and what are its advantages and disadvantages?

    > One way is using aldehydes to form bonds to chemical moieties on biological macromolecules. Another way is using osmium to make the structues to become more rigid and better preserved.

3. Once a cell or tissue is chemically fixed, what else is typically done in preparation for traditional "thin section" EM?

    > - Dehydration.
    > - Plastic embedment.
    > - Sectioning.
    > - Staining.

4. What metal stains are typically used for thin-section EM, and how do they affect the visibility of sample structures?

    > - Uranyl acetate: decorating the protein components of the cell.
    > - Osmium tetroxide: being collected inside the membrane.

5. How does "metal shadowing" work?

    > Fix the sample put it on a rotating grid. Then the metal partical will drop from the top. Since the grid is rotating, the metal partial will only be collected on one side of the sample, forming a shadow.

6. How is metal shadowing different from "negative staining"?

    > Metal shadowing uses a heated rod to produce metal partical, while negative staning uses metal partical solution.

## 4-2 Sample prep - Methods involving freezing

1. What problem does high pressure freezing solve?

    > Traditional thin section EM requires the sample to be dehydrated, which will cause the cell to shrink. High pressure freezing can freeze the sample so fast that the water molecules will not form ice crystals.

2. What is "low-temperature" embedding?

    > The block of tissue dehydrated and infiltrated by an organic solvent with a plastic resin will polymerizen at low temperature.

3. What is "cryo-sectioning", and what artifacts (3) and challenges (several) are associated with it?

    > First high presure freezing the sample, and then sectioning it in the frozen state. Finally the section is transferred to a grid and imaged.
    > - Artifacts:
    >      - Sections are not flat.
    >      - Knife marks are visible.
    >      - During sectioning, the stress forces the sample to erupt up out of the surface creating a series of crevasse-like cracks.

4. What kinds of samples can be "plunge-frozen"?

    > Any samples that can be suspended in a liquid.

5. How can focussed ion beams be used to prepare cryo-EM samples?

    > The focussed ion beams can be used to cut the sample into thin slices, a.k.a. "lamella".

## 4-3 Sample prep - Grid

1. What are the most common materials used to make grids?

    > Copper, gold, and nickel.

2. If you wanted to culture cells on grids, which grids would be better - copper or gold?

    > Gold. Copper will be oxidized.

3. What does "250 mesh" mean?

    > The number of holes per inch in the grid.

4. What is a "slot" grid? A "finder" grid?

    > - "slot grid": A large hole in the center of the grid.
    > - "finder" grid: A number of symbols, a asymetric center hole, and a large arrow, which helps to identify the orientation of the grid.

5. What is formvar?

    > A form of plastic that can be used to coat the grid.

6. What is the difference between "holey" carbon and "Quantifoil" coatings?

    > The holes made by "Quantifoil" can be controlled to be the same size and evenly spaced.

7. What is a carbon evaporator, and how does it work?

    > To coat the grid with carbon, the grid is placed in a vacuum chamber and a carbon rod is heated to evaporate the carbon.

8. What is "glow discharging," and why is it done?

    > As the layer of carbon is deposited, it is hydrophilic. After storing the grid for weeks, the carbon layer will become hydrophobic. Glow discharging can make the carbon layer hydrophilic again. Glow discharging is is similar to carbon evaporation, this time the grid is placed between two electrodes and a high voltage is applied. The air between the electrodes will be ionized and the ions will bombard the grid, creating hydropilic groups on the carbon layer.

9. What is "cryo-crinkling", and what are some ways to reduce it?

    > Cryo-crinkling is the crinkling of the carbon layer due to the difference in thermal expansion between the carbon layer and the copper grid in freezing. One way to reduce it is to use a gold grid.

## 4-4 3-D reconstruction

1. How can 3-D reconstructions be calculated from 2-D projections in real space?

    > Back project the 2-D projections to the 3-D space to find the intersection.

2. What is the "projection theorem"? Draw it.

    > Refer to the Fourier central slice theorem. The Fourier transform of the projection of an object is the central slice of the Fourier transform of the object.

3. How are 3-D reconstructions calculated from 2-D projections in reciprocal space?

    > The Fourier transform of the 2-D projections are combined in the orientation of the projections that passes the center of the 3-D object to form the Fourier transform of the 3-D object. Then the inverse Fourier transform is applied to the Fourier transform of the 3-D object to get the 3-D object.

## 4-5 Dose limitation

1. How do imaging electrons damage biological samples?

    > The electrons will ionize the molecule bonds, which create small gas molecules. The gas molecules can be seen as bubbles in the image.

2. How can radiation damage be recognized in images?

    > The image becomes fuzzer and the structure becomes less clear.

3. How can the rate of this damage be assessed quantitatively?

    > By measuring the intensity of high spatial frequency spots in the diffraction pattern under different doses.

4. What is the effect of temperature on the rate of radiation damage?

    > The higher the temperature, the faster the radiation damage.

5. What disadvantage is there to imaging at temperatures less than 40K?

    > Under 40K, the water molecules will form ice crystals, as the electrons pass through the ice crystals, the water molecules obtain energy from the electrons and re-arrange themselves into a more dense vitreous form. There density is close to the density of the protein, which makes low contrast.

6. For what kinds of samples can radiation damage be overcome? How?

    > By imaging a large number of identical copies of the same sample and averaging the images.

7. What are the three basic modalities of cryo-EM? How are they different? What kinds of resolutions can be expected from each? Why?

    > - Tomography: take a series of images of the same sample at different angles and reconstruct the 3-D structure. The resolution is about 4 nm restricted by the dose.
    > - Single particle analysis: take a large number of images of the same sample and average them. The resolution is about atomic resolution.
    > - 2-D crystallography: crystallize the sample and take X-ray diffraction images. The resolution is highest.

# 5 Tomography

## 5-1 Intro

1. What is a "tilt-series"?

    > A set of images of the same sample at different angles.

2. What range of angles is typically imaged?

    > -60 to 60 degrees.

3. What is the "missing wedge"?

    > The sample get thicker as the tilt angle goes higher. So high tilt angle images are meaningless and discarded, which causes a missing wedge in the 3-D reconstruction.

4. How are interpolations involved in the calculation of a tomogram?

    > The image is digitized into pixels, which means the image is a discrete function. The Fourier transform of the image is also a discrete function. In the Fourier space, the value of a lattice points is interpolated from the values of the surrounding points of 2D signals.

5. What does each word in "serial section montage tomography" signify?

    > Serial section: Sectioning a large tissue block into smaller slice.
    > Montage: Imaging a small area inside the slice and stacking along the slice.

6. How large or small a sample can be imaged by electron tomography?

    > From a cell to a macro-moleculecular complex.

7. How is "volume rendering" different than showing an "isosurface" or single slice?

    > Allowing you to see through the whole volume instead of just a surface or a piece of slice.

## 5.2 Data collection and reconstruction

1. What is "eucentric height"? Is it different for every sample?

    > See [the sample chamber](#1-4-the-sample-chamber).

2. What is the "tilt axis offset"? Is it different for every sample?

    > The gonionmeter is not perfectly aligned with the optical axis. The tilt axis offset is the distance between the tilt axis and the optical axis. It is different for every sample.

3. When speaking about automatic sequential tilt-series acquisition, what is "tracking"? What is "targeting"?

    > Tracking: 
    > 
    > Targeting:

4. How is the "predictive" tracking method different from the "focus position" method? Which is faster? Why would the slower method ever be used?

    > 

5. What is a "low mag atlas" and why would one be recorded?

    > 

6. What microscope operations are used to set specimen height automatically? Why?

    > 

7. What microscope operations are used to focus objects automatically? Why?

    > 

8. What about each image has to be determined to "align" a tilt-series? How are these parameters found?

    > 

9. What steps of data collection and 3-D reconstruction have to be done by the investigator, and which are typically automated?

    > 

