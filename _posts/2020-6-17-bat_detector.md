---
layout: post
title: Heterodyne Ultrasonic Bat Detector
---
<script src="http://api.html5media.info/1.1.8/html5media.min.js"></script>

![](https://upload.wikimedia.org/wikipedia/commons/9/96/Kleine_Hufeisennase.jpg)

*Lesser Horseshoe Bat*, © F. C. Robiller/naturlichter.de, [CC BY-SA](https://creativecommons.org/licenses/by-sa/3.0)


This project is about builduing a so called bat detector, i.e. a device that lets you listen to and record the sounds emitted by bats. As you probably know, bats emit ultrasonic sounds for the purpose of echolocation. These sounds lie in a frequency range above the human audible range and thus can't be heard directly. A bat detector uses a special microphone to capture these high-frequency sounds and convert them to a sound within the human audio frequency range.

There are three basic types of bat detectors (see [[1]](https://en.wikipedia.org/wiki/Bat_detector#Bat_detector_types)), in this case I developed a *heterodyne* detector. The advantage of this type is that you can listen to the bats in real-time. The disadvantage is, that using a heterodyne detector it is more difficult to distinguish different bat species compared to a high-frequency recording. I will explain how the heterodyne detector works later, first let's take a look at the microphone needed to detect ultrasounds.

While searching for a suitable microphone, I considered the following options:

* an electret capsule microphone: all of which I found are only specified in a frequency range beneath 10 or 20kHz, but according to some resources, some types are also somewhat sensible in the ultrasonic range. But since this is not specified in the datasheets, it would need some trial-and-error picking various brands and types and testing them. Therefore I chose not to use this type of microphone.

* a piezo transducer: they can be found for example in the ubiquitous *HC-SR04* rangefinder modules which can be found everywhere really cheap. The drawback of these is, that there sensitivity is centered at their resonant frequency somewhere around 40kHz. Above and beyond the resonant frequency, the sensitivity drops sharply. Therefore, those are not ideal either.

Instead, I found the *Knowles SPU0410LR5H* Microphone ([datasheet pdf](https://www.knowles.com/docs/default-source/model-downloads/spu0410lr5h-qb-revh.pdf)) which is a MEMS-type microphone. According to the datasheet, this microphone has a relatively flat frequency response up to 80kHz, so it is very well suited for this application. Additionally, it has an internal preamplifier, which achieves a high SNR. The main drawback of this microphone is that the package is by no means hobbyist-friendly. The microphone is **tiny** (only 3.76 x 3mm!) and has the connection pads *beneath* the package. While browsing around the web, I found [this page](https://hackaday.io/project/165081-blue-board-01/details) by *hackaday.io*-user [Alan Green](https://hackaday.io/alang) who also uses the same microphone in his project. He had the great idea of creating a special PCB footprint for the component that allows it to be soldered by hand. The trick is to elongate the pad so they reach out of the component. Then the pads can be heated up with the soldering iron and transfer the heat beneath the component. So I took this idea and went on designing a small PCB that contains the microphone and a dual op-amp. The latter provides a buffered virtual ground (at half the supply voltage) as well as an amplifier stage with 20dB of gain (i.e. x10). At first I was a little sceptic if soldering this microphone by hand, but it turned out to work really well, out of the 6 PCBs I soldered, all worked fine. I recorded the soldering process, you can watch the video [here](https://vimeo.com/430343841). As always, the design files for the PCB are open source and can be grabbed [here](https://github.com/MarcelMG/MEMS_Microphone_SPU0410_PCB/).

<p float="center">
  <img src="https://github.com/MarcelMG/MEMS_Microphone_SPU0410_PCB/raw/master/pcb_soldered_image2.jpg" width="280" />
  <img src="https://github.com/MarcelMG/MEMS_Microphone_SPU0410_PCB/raw/master/front_view.jpg" width="280" /> 
  <img src="https://github.com/MarcelMG/MEMS_Microphone_SPU0410_PCB/raw/master/back_view.jpg" width="280" />
</p>

So now I will explain the working principle of the heterodyne bat detector. To understand how it works, we need to take a look at the signals emitted by bats. We can model the bat calls as an amplitude-modulated signal with an ultrasonic carrier frequency and an envelope signal that looks like a short "chirp" signal.

![bat_signal](https://github.com/MarcelMG/marcelmg.github.io/raw/master/images/bat_signal.jpg)

In the above diagram you can see the ultrasonic carrier in red, the "chirp" envelope signal in green and the resulting product of both signals in blue. So now to make this signal audible to the human, we need to transform it in such way that we retain the envelope signal (the "chirp" sound) but modulate it with a lower-frequency carrier signal (e.g. in the 1-5kHz range). So how can we achieve this?

To understand how it works mathemetically, we use the known trigonometric identities [[2]](https://en.wikipedia.org/wiki/List_of_trigonometric_identities#Product-to-sum_and_sum-to-product_identities)

$$2sin(x)\cdot sin(y)=cos(x-y)-cos(x+y)$$

$$2cos(x)\cdot cos(y)=cos(x-y)+cos(x+y)$$

$$2sin(x)\cdot cos(y)=sin(x-y)+sin(x+y)$$

$$2cos(x)\cdot sin(y)=-sin(x-y)+sin(x+y)$$

So if we consider the bat call signal as explained above, we can model it as follows:

$$carrier(t)=sin(2\pi f_c t)\\
chirp(t)=sin(2\pi f_{chirp} t)\\
bat(t)=carrier(t)\cdot chirp(t)$$

with the ultrasonic carrier frequency $$f_c$$ and the frequency of the "chirp"-signal $$f_{chirp}$$. For this example, let's assume

$$f_c=40\text{kHz}$$ and $$f_chirp=1\text{kHz}$$.

Now if we apply the trigonometric identity, we get:

$$bat(t)=sin(2\pi f_c t)\cdot sin(2\pi f_{chirp} t)=\frac{1}{2}cos\bigl(2\pi (f_c-f_{chirp})t\bigr)-\frac{1}{2}cos\bigl(2\pi (f_c+f_{chirp})t\bigr)$$

So the modulated signal is composed of two frequencies symmetrically spaced around the carrier frequency, in this example at

$$40\text{kHz}-1\text{kHz}=39\text{kHz}$$ and $$40\text{kHz}+1\text{kHz}=41\text{kHz}$$.

These are called the lower and upper side bands (*LSB* and *USB*):

$$f_{LSB}=f_c-f_{chirp}$$

$$f_{USB}=f_c+f_{chirp}$$

Now we can use the same principle to down-convert the bat-signal to a lower (audible) frequency range. To do this, we have to multiply the signal with a so called local oscillator (LO) frequency that is the difference between the signal's carrier frequency and the desired target carrier frequency (in this case, the audible frequency at which we want to hear the bat calls). This technique is called [Heterodyning](https://en.wikipedia.org/wiki/Heterodyne). In our example, let's say we want to hear the bat calls at a frequency of 5kHz, which is well audible for humans. Then, we have:


$$f_{target}=5\text{kHz}$$


$$f_{LO}=f_c-f_{target}=35kHz$$


$$LO(t)=sin(2\pi f_{LO}t)$$


If we plug everything together, we can rewrite the terms using again the trigonometric identity as follows:


$$bat(t)\cdot LO(t)=\biggl(\frac{1}{2}cos(2\pi f_{LSB}t)-\frac{1}{2}cos(2\pi f_{USB}t)\biggr)\cdot sin(2\pi f_{LO}t)$$


$$=\frac{1}{2}cos(2\pi f_{LSB}t)\cdot sin(2\pi f_{LO}t)-\frac{1}{2}cos(2\pi f_{USB}t)\cdot sin(2\pi f_{LO}t)$$


$$=\frac{1}{4}\biggl(sin\bigl(2\pi(f_{LO}-f_{LSB})t\bigr)+sin\bigl(2\pi(f_{LO}+f_{LSB})t\bigr)-sin\bigl(2\pi(f_{LO}-f_{USB})t\bigr)-sin\bigl(2\pi(f_{LO}+f_{USB})t\bigr)\biggr)$$

(using $$sin(-x)=-sin(x)$$)

$$=\frac{1}{2}\biggl(-\frac{1}{2}sin\bigl(2\pi(\underbrace{f_{LSB}}_{=f_c-f_{chirp}}-f_{LO})t\bigr)+\frac{1}{2}sin\bigl(2\pi(\underbrace{f_{USB}}_{=f_c+f_{chirp}}-f_{LO})t\bigr)+\frac{1}{2}sin\bigl(2\pi(\underbrace{f_{LSB}}_{=f_c-f_{chirp}}+f_{LO})t\bigr)-\frac{1}{2}sin\bigl(2\pi(\underbrace{f_{USB}}_{=f_c+f_{chirp}}+f_{LO})t\bigr)\biggr)$$


$$=\frac{1}{2}\biggl(-\frac{1}{2}sin\bigl(2\pi(\underbrace{f_c-f_{LO}}_{=f_{target}}-f_{chirp})t\bigr)+\frac{1}{2}sin\bigl(2\pi(\underbrace{f_c-f_{LO}}_{=f_{target}}+f_{chirp})t\bigr)+\frac{1}{2}sin\bigl(2\pi(\underbrace{f_c+f_{LO}}_{:=f_2}-f_{chirp})t\bigr)-\frac{1}{2}sin\bigl(2\pi(\underbrace{f_c+f_{LO}}_{:=f_2}+f_{chirp})t\bigr)\biggr)$$


$$=\frac{1}{2}cos(2\pi f_{target}t)\cdot sin(2\pi f_{chirp}t)+\frac{1}{2}cos(2\pi f_{2}t)\cdot sin(2\pi f_{chirp}t)$$


As we can see, the first term $$cos(2\pi f_{target}t)\cdot sin(2\pi f_{chirp}t)$$ is exactly what we wanted, the envelope "chirp"-signal modulated with our audible target frequency of in this case, 5kHz. The second term is again the "chirp"-envelope signal, but modulated with a much higher frequency of $$f_2=f_c+f_{LO}$$ which is in this case 75kHz. This second component is well outside the human hearing range and can easily be filtered out using a low-pass filter in the bat detector circuit.


So now that we know in theory how the heterodyning principle can be used to build a bat detector, how can we realize it in a practical electronic circuit? The crucial part is the *multiplication* of the incoming signal with the LO-signal, which as it turns out is not trivial in practical electronics. There are in fact circuits that achieve analog multiplication (like the e.g. [Gilbert cell](https://en.wikipedia.org/wiki/Gilbert_cell)) and we could use an appropriate IC (like the [NE612](https://en.wikipedia.org/wiki/NE612)) together with a sine wave generator circuit (e.g. [Wien bridge oscillator](https://en.wikipedia.org/wiki/Wien_bridge_oscillator)). But this solution quite complicated and the Analog Multiplier ICs like the NE612 or similar are expensive and difficult to find.

As it turns out, there is another simpler approach that can be built with ubiquitous standard components. We can build a [frequency mixer](https://en.wikipedia.org/wiki/Frequency_mixer) with an [Analog Switch](https://en.wikipedia.org/wiki/Analogue_switch). Although it is not an *ideal* mixer since it doesn't actually multiply both signals, we will see that it will work fine nonetheless. 

This time, let's first take a look at the schematic and from there on figure out how it works.

![schematic](https://github.com/MarcelMG/Heterodyne_Bat_Detector/raw/master/schematic_small.jpg)

At the very left of the schematic, the microphone output is fed into a [2nd order LC high-pass filter](https://en.wikipedia.org/wiki/RLC_circuit#High-pass_filter) that filters out the audible frequency range beneath approximately 20kHz and let's only pass the ultrasonic signals (since it's these we're interested in). The resistor R1 parallel to the inductor is critical to dampen the LC-circuit and avoid resonance.

After the high-pass filter, the signal is fed into two op-amp buffer stages, one with a gain of 1 and the other with a gain of -1 (i.e. it *inverts* the signal). So now we have both the (bat-)signal and it's inverted counterpart.

If we take a look at the bottom left of the schematic, we recognize an [astable multivibrator](https://en.wikipedia.org/wiki/555_timer_IC#Astable) circuit with the venerable [555 IC](https://en.wikipedia.org/wiki/555_timer_IC). With the potentiometer P1 we can control the circuit and create a square wave with a duty cycle of ~50% and a variable frequency between ~20kHz and ~400kHz. This signal is in this case our local oscillator (LO) signal, but in contrast to our mathematical model it is a square wave and not a sine wave. Again, the LO signal is fed into an inverting stage formed by the analog switch U3D and the resistor R6. Here an analog switch is used as a digital inverter, since the IC *CD4066* that is used contains 4 analog switches we have two to spare and can use one as an inverter to save on components. How it works is simple: if the LO-signal is low, the switch is open and R6 pulls the output of the switch high. If the LO-signal is high, the switch is closed and the output of the switch is connected to ground. This way, we create an inverted LO-signal $$\overline{LO}$$.

So now let's look at the part of the schematic labeled *balanced mixer*. We see, that the non-inverted bat-signal is fed into an analog switch which is controlled by the $$LO$$-signal. The inverted bat-signal is fed into another switch which is in turn controlled by the inverted $$\overline{LO}$$-signal. The outputs of both switches are connected. So what does this part of the circuit do?

Let's consider the first case, when the $$LO$$-signal is 0 (low) and therefore the $$\overline{LO}$$-signal is 1 (high). In this case the lower switch *U3B* is closed and the upper switch *U3A* is open. Therefore, the inverted bat-signal is fed through. In the opposite case, when the $$LO$$-signal is 1 (high) and therefore the $$\overline{LO}$$-signal is 0 (low), the lower switch *U3B* is open and the upper switch *U3A* is closed. This time, the non-inverted bat-signal is fed through. Let's remember that this alternating switching procedure is happening at the frequency $$f_{LO}$$ determined by the 555-timer circuit.

Now, how can we model this behaviour mathematically? If we think about what this mixer circuit does, it actually multiplies the signal with a square-wave that alternates between +1 and -1. This is equivalent to the output being switched alternately between the non-inverted and inverted input signal. We can describe such a waveform (let's call it $$r(t)$$) using the so called signum-function $$sign(x)$$:

$$r(t):=sign\bigl(sin(2\pi ft)\bigr) \hspace{3mm} \text{with} \hspace{3mm}sign(x)=\left\{
\begin{array}{ll}
1 & x \geq 0 \\
-1 & x < 0 \\
\end{array}
\right.$$

So the output of the mixer is the product $$bat(t)\cdot r(t)$$. But that alone doesn't explain anything yet, to understand how the frequency mixing happens we have to apply some mathematical wizardry called the [Fourier series decomposition](https://mathworld.wolfram.com/FourierSeries.html). This time I will spare you the derivation and present you the result. Basically, using the Fourier series, we can show that

$$r(t)=\sum\limits_{k=0}^{\infty} \frac{4}{\pi} sin\bigl(2\pi (2k+1)f_{LO}t\bigr)=\frac4\pi\bigl(sin(2\pi f_{LO}t)+sin(2\pi\cdot3f_{LO}t)+sin(2\pi \cdot5f_{LO}t)+\ldots \bigr)$$.

So our +1/-1 square wave is composed of an infinite number of sine-waves with the main frequency $$f_{LO}$$ and it's odd multiples. So what can we make with that information? If we for now ignore the constant factor $$\frac{4}{\pi}$$, we see that $$r(t)$$ is the sum of $$LO(t)$$ and other additional higher frequencies $${LO}_{3}(t)$$, $${LO}_{5}(t)$$ and so on. From the previous calculations, we have shown that

$$bat(t)\cdot LO(t)=\frac{1}{2}cos(2\pi f_{target}t)\cdot sin(2\pi f_{chirp}t)+\frac{1}{2}cos(2\pi f_{2}t)\cdot sin(2\pi f_{chirp}t)$$

So we can calculate

$$bat(t)\cdot r(t)=bat(t)\cdot\frac{4}{\pi}\biggl(LO(t)+{LO}_{3}(t)+{LO}_{5}(t)+\ldots$$

$$=\frac4\pi\bigl(sin(2\pi f_{LO}t)+sin(2\pi\cdot3f_{LO}t)+sin(2\pi \cdot5f_{LO}t)+\ldots \bigr)$$

$$=\frac{2}{\pi}cos(2\pi f_{target}t)\cdot sin(2\pi f_{chirp}t)+\frac{1}{2}cos(2\pi f_{2}t)\cdot sin(2\pi f_{chirp}t)$$

$$+\frac{1}{2}cos(2\pi (3f_{LO}-f_c)t)\cdot sin(2\pi f_{chirp}t)+\frac{1}{2}cos(2\pi (3f_{LO}+f_c)t)\cdot sin(2\pi f_{chirp}t)$$

$$+\frac{1}{2}cos(2\pi (5f_{LO}-f_c)t)\cdot sin(2\pi f_{chirp}t)+\frac{1}{2}cos(2\pi (5f_{LO}+f_c)t)\cdot sin(2\pi f_{chirp}t)$$

$$+\ldots$$

So again, we see that the first term $$\frac{2}{\pi}cos(2\pi f_{target}t)\cdot sin(2\pi f_{chirp}t)$$ is the result we want, but now with the non-ideal analog switch mixer, we get many more terms that we don't want. But the great thing is, that all these other unwanted frequency components are of much higher frequencies that our target frequency $$f_{target}$$. So we can use a low-pass filter to filter those components out, so that the resulting signal is approximately $$\frac{2}{\pi}cos(2\pi f_{target}t)\cdot sin(2\pi f_{chirp}t)$$.



$$bat(t)\cdot r(t)=bat(t)\cdot\frac4\pi\bigl(sin(2\pi f_{LO}t)+sin(2\pi\cdot3f_{LO}t)+sin(2\pi \cdot5f_{LO}t)+\ldots \bigr)$$
$$=\biggl(\frac{1}{2}cos\bigl(2\pi (f_c-f_{chirp})t\bigr)-\frac{1}{2}cos\bigl(2\pi (f_c+f_{chirp})t\bigr)\biggr)\cdot\frac4\pi\bigl(sin(2\pi f_{LO}t)+sin(2\pi\cdot3f_{LO}t)+sin(2\pi \cdot5f_{LO}t)+\ldots \bigr)$$



<audio src="https://github.com/MarcelMG/marcelmg.github.io/raw/master/misc/bat_sample.mp3" controls preload></audio>



$$r(t):=sign\bigl(sin(2\pi ft)\bigr) \hspace{3mm} \text{with} \hspace{3mm}sign(x)=\left\{
\begin{array}{ll}
1 & x \geq 0 \\
-1 & x < 0 \\
\end{array}
\right.$$



$$r(t)$$ is an odd function, since $$r(-t)=-r(t)$$

r(t)=\sum\limits_{k=0}^{\infty} \frac{4}{\pi} sin\bigl(2\pi (2k+1)f_{LO}t\bigr)=\frac4\pi\bigl(sin(2\pi f_{LO}t)+sin(2\pi\cdot3f_{LO}t)+sin(2\pi \cdot5f_{LO}t)+\ldots \bigr)



