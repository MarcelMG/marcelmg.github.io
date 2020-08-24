---
layout: post
title: the speaking compass
---
<script src="http://api.html5media.info/1.1.8/html5media.min.js"></script>

In the previous posts I showed how to store and playback audio files with the STM32F1 microcontroller and a SPI flash memory chip. Then I kept thinking about building something that uses audio output. Digging through my parts bin I came upon a QMC5883L magnetometer board that I bought long time ago. So I had the idea to build a speaking compass, that tells you the current orientation using voice audio output.

<!--excerpt-->

I had already written code to use the QMC5883L magnetometer using the I²C protocol and read raw values, I only had to write another function to calculate the orientation. The magnetometer measures the magnetic flux along three axes. If there are no other strong magnetic fields nearby, we can measure the earth's magnetic field. When the sensor is placed horizontally, we can approximately calculate the orientation by computing the angle of the vector of x-y-flux, i.e. the arctangens of the y- and x-values. In reality, there are deviations because of two reasons:
* the earth's magnetic field is not exactly horizontal, depending on your location, this is called [magnetic inclination](https://en.wikipedia.org/wiki/Magnetic_dip)
* the earth's magnetic north differs from the geographic north depending on your location, this is called [magnetic declination](https://en.wikipedia.org/wiki/Magnetic_declination)
For this simple fun project I neglect both, since I only need very course values.

The next step was to generate audio files to tell the user the orientation. I decided to create an audio snippet for every 45° 
 segment, i.e. e.g. north, north-east, east etc. I used an online [text-to-speech generator](https://www.text2speech.org/) to create the audio snippets, then I converted them into 8bit mono raw format using Audacity. After that, I programmed the audio files to the SPI Flash memory using my terminal tool (see [last post](https://marcelmg.github.io/spi_flash_memory/)) and noted their addresses and lengths manually, since there is no filesystem that takes care of that.
 
 The main program is very simple, it reads the orientation from the magnetometer and checks in which section it lies. Then it plays the corresponding audio snippet. You can see a quick demo here:
<video src="https://github.com/MarcelMG/talking-compass/raw/master/speaking_compass_demo.mp4" width="480" height="320" controls preload></video>

The circuit is built on a breadboard, it consists of the STM32F103 microcontroller board, the active low-pass filter for the PWM audio playback (see [this post](https://marcelmg.github.io/pwm_dac_sound/)) and the board with the W25Q64JV SPI flash memory. I connected a tiny speaker to the opamp's output, but the volume is really low. It could use another amplifier like e.g. the LM386, but I even though I had one I was too lazy to use it and there was no room left on the breadboard.

Overall I think it's a fun little project, though one can really argue about it's usefulness...
