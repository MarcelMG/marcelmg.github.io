---
layout: post
title: create audio signals with PWM
---

In this post I'm going to show how we can use PWM to playback audio on the STM32F103 microcontroller. In the [previous post](https://marcelmg.github.io/pwm_left_vs_centered/) I explained some theory about how to generate analog signals with PWM, now we'll see an example of how to realize it.

I am using the STM32F103 microcontroller, which has some nice features that are useful for this application. You can find very cheap boards by searching for "STM32F103C8T6" e.g. on ebay. They are about the size of a Arduino Nano and cost less than 5€. 

Basically, the setup consists of 3 steps:
* setting up a timer to generate the PWM signal
* setting up the DMA controller to copy the audio samples
* configuring a second timer to periodically trigger the DMA

For the PWM signal generation, I am using timer 2. It is clocked with the maximal possible frequency, 72MHz (the main clock). Now as I explained in the previous post, there is a tradeoff between the resolution of the PWM and the frequency. Audio is usually recorded with 16bit resolution (CD quality), but then the maximum PWM frequency would be 72MHz/2^16=1098 Hz which is way too low. Therefore I am using 8 bit resolution, which gives a PWM of 72MHz/2^18=281250 Hz (in left-adjusted mode), which is very good since it's much higher than the audible range. Another advantage is, that with 8bit resolution one sample fits exactly in one byte, which makes programming easier. If our samples would be e.g. 10bit and we store them in a 16bit variable, we waste memory space.

After setting up the timer for PWM generation, the DMA controller is configured. DMA stands for *direct memory access*. The DMA controller is a peripheral device in the microcontroller. It's purpose is basically to copy data from a source to a destination. The big advantage is, that is does that independently from the CPU, so it reduces the CPU load. The DMA controller basically works as follows: we have to tell it where it should fetch the data (the source address) and where to copy it to (the destination address). Then we must configure *when* it should transfer the data, i.e. a trigger.

In this case, the source from where to fetch the data is the array containing our audio samples. The destination is the register of timer 2 which contains the PWM compare value. This is the value which determines the duty-cycle of the PWM signal. The DMA transfer has to be triggered with the sample rate of the recording. In this case, I use the standard of 44.1kHz. So every 1/44kHz, a new sample is output by the PWM-"DAC". You can find the whole source code [here](https://github.com/MarcelMG/STM32F103C8T6/tree/master/PWM_DAC_SOUND). You might notice that I'm not using any Hardware Abstraction Layer / Libraries. I want to teach myself how to work with these STM32 microcontrollers by programming it "bare-bones" for educational purpose. 

So what's nice about using DMA here is, that we can play the sound without loading the CPU at all! As you can see in the source code, the main while-loop is empty.

To test it, I created two audio files. One is a simple lookup-table to create a 5kHz sine wave. To test a real voice audio, I took a short test recording of me saying "Hello test". I used the well-known software *Audacity* to export the audio snippet as a "8bit unsigned RAW" file. This results in a binary file containing the 8bit large samples. To program this file into the microcontroller, I created a small C++ program, which converts the file into a convenient C-header file which contains the data as an array. Then it is only a matter of including the *.h file in my microcontroller-project. You can find the tool [here](https://github.com/MarcelMG/Miscellaneous/blob/master/8bit_raw_audio_2_c_header_converter/main.cpp).

Now to filter the PWM signal, I built a simple two stage RC low-pass filter circuit on a breadboard. The op-amp I used is a MCP6002 which has the advantage of working with rail-to-rail input & output even at low voltages (here 3.3V from the microcontroller board). The output is connected to some headphones.
[pwm_dac_filter_schematic](https://raw.githubusercontent.com/MarcelMG/marcelmg.github.io/master/images/pwm_dac_filter_schematic.png)

So now let's take a look at the test results! First, the 5kHz sine wave: This is a snapshot from my oscilloscope (FYI i have a PicoScope 2204A).
[280kHz_PWM_5kHz_sine](https://raw.githubusercontent.com/MarcelMG/marcelmg.github.io/master/images/280kHz_PWM_5kHz_sine.png)
In red we can see the raw PWM signal, the blue one is the output after the low-pass filter. It looks pretty good, even more so considering that I used only 9 samples in a period. If we inspect the spectrum of the signal, we can though clearly see the PWM harmonics:
[280kHz_PWM_5kHz_sine_red_pwm_blue_filtered16kHz](https://raw.githubusercontent.com/MarcelMG/marcelmg.github.io/master/images/280kHz_PWM_5kHz_sine_red_pwm_blue_filtered16kHz.png)
The main peak is naturally at 5kHz, the frequency of the sine wave. But the next biggest peak is at 280kHz, which is the frequency of the PWM. There is another peak at 560kHz, twice the PWM frequency. In this application these harmonics are not a problem, because they are way outside the audible range or the range a normal speaker could reproduce.

Ok, so now how about the voice sample? I have to say that the results are way better than I expected considering the resolution of only 8bit. The voice audio is of course no HiFi, but it is clearly understandable. I recorded the sound with my PC soundcard, you can listen to it [here](https://raw.githubusercontent.com/MarcelMG/marcelmg.github.io/master/misc/PWM_DAC_test_sound.mp3). The recording is not perfect because the sound card input went into saturation, but It still gives a good impression of the audio quality.

The results of this project were really encouraging, but one limit is the available memory space on the microcontroller: with my 2s long audio snippet, the 64kB of internal flash memory were almost full. For this reason I've been looking for external memory to connect to the microcontroller and decided to go for a Flash memory chip. But more about that in the next post...
