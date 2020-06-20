---
layout: post
title: Heterodyne Ultrasonic Bat Detector
---
<script src="http://api.html5media.info/1.1.8/html5media.min.js"></script>

<a title="F. C. Robiller / naturlichter.de / CC BY-SA (https://creativecommons.org/licenses/by-sa/3.0)" href="https://commons.wikimedia.org/wiki/File:Kleine_Hufeisennase.jpg"><img width="512" alt="Kleine Hufeisennase" src="https://upload.wikimedia.org/wikipedia/commons/9/96/Kleine_Hufeisennase.jpg"></a>

This project is about builduing a so called bat detector, i.e. a device that lets you listen to and record the sounds emitted by bats. As you probably know, bats emit ultrasonic sounds for the purpose of echolocation. These sounds lie in a frequency range above the human audible range and thus can't be heard directly. A bat detector uses a special microphone to capture these high-frequency sounds and convert them to a sound within the human audio frequency range.

There are three basic types of bat detectors (see [[1]](https://en.wikipedia.org/wiki/Bat_detector#Bat_detector_types)), in this case I developed a *heterodyne* detector. The advantage of this type is that you can listen to the bats in real-time. The disadvantage is, that using a heterodyne detector it is more difficult to distinguish different bat species compared to a high-frequency recording. I will explain how the heterodyne detector works later, first let's take a look at the microphone needed to detect ultrasounds.

While searching for a suitable microphone, I considered the following options:

* an electret capsule microphone: all of which I found are only specified in a frequency range beneath 10 or 20kHz, but according to some resources, some types are also somewhat sensible in the ultrasonic range. But since this is not specified in the datasheets, it would need some trial-and-error picking various brands and types and testing them. Therefore I chose not to use this type of microphone.

* a piezo transducer: they can be found for example in the ubiquitous *HC-SR04* rangefinder modules which can be found everywhere really cheap. The drawback of these is, that there sensitivity is centered at their resonant frequency somewhere around 40kHz. Above and beyond the resonant frequency, the sensitivity drops sharply. Therefore, those are not ideal either.

Instead, I found the *Knowles SPU0410LR5H* Microphone ([datasheet pdf](https://www.knowles.com/docs/default-source/model-downloads/spu0410lr5h-qb-revh.pdf)) which is a MEMS-type microphone. According to the datasheet, this microphone has a relatively flat frequency response up to 80kHz, so it is very well suited for this application. Additionally, it has an internal preamplifier, which achieves a high SNR. The main drawback of this microphone is that the package is by no means hobbyist-friendly. The microphone is **tiny** (only 3.76 x 3mm!) and has the connection pads *beneath* the package. While browsing around the web, I found [this page](https://hackaday.io/project/165081-blue-board-01/details) by *hackaday.io*-user [Alan Green](https://hackaday.io/alang) who also uses the same microphone in his project. He had the great idea of creating a special PCB footprint for the component that allows it to be soldered by hand. The trick is to elongate the pad so they reach out of the component. Then the pads can be heated up with the soldering iron and transfer the heat beneath the component. So I took this idea and went on designing a small PCB that contains the microphone and a dual op-amp. The latter provides a buffered virtual ground (at half the supply voltage) as well as an amplifier stage with 20dB of gain (i.e. x10). At first I was a little sceptic if soldering this microphone by hand, but it turned out to work really well, out of the 6 PCBs I soldered, all worked fine. I recorded the soldering process, you can watch the video [here](https://vimeo.com/430343841). As always, the design files for the PCB are open source and can be grabbed [here](https://github.com/MarcelMG/MEMS_Microphone_SPU0410_PCB/).
 

<audio src="https://github.com/MarcelMG/marcelmg.github.io/raw/master/misc/bat_sample.mp3" controls preload></audio>



r(t):=sign\bigl(sin(2\pi ft)\bigr) \hspace{3mm} \text{with} \hspace{3mm}sign(x)=\left\{
\begin{array}{ll}
1 & x \geq 0 \\
-1 & x < 0 \\
\end{array}
\right.
\\
r(t)\text{ is an odd function, since }r(-t)=-r(t)\\
r(t)=\sum\limits_{k=1}^{\infty} \frac{4}{\pi(2k-1)} sin(2\pi kft)=\frac4\pi\bigl(sin(2\pi ft)+\frac{1}{3}sin(2\pi\cdot2ft)+\frac{1}{5}sin(2\pi\cdot3ft) + \ldots \bigr)


[[2]](https://en.wikipedia.org/wiki/List_of_trigonometric_identities#Product-to-sum_and_sum-to-product_identities)
$$2sin(x)sin(y)=cos(x-y)-cos(x+y)$$
