---
layout: post
title: W25Q64JV SPI flash memory chip as external memory
---

In the last blog post, I showed how we can create audio sginals using PWM on a STM32F1 microcontroller. But since the memory space available on the microcontroller is really small, we can't store a significant length of audio recordings. Thus I decided to use an external meory and went for a SPI flash memory chip, the Winbond W25Q64JV. In this post, I'll explain how to use it and read/write it with a microcontroller, in my case I use the STM32F103.

This memory chip has 8 Megabytes of space available and communicates via SPI (Serial Peripheral Interface). It also supports Dual and Quad SPI modes which allow for faster data throughoutput, but here I'll stick to simple SPI.

To prototype easily, I decided to design a small breakout board for the chip. I used the open source software "KiCad" to layout the board.
These days there are many PCB fabs that provide cheap prototypes, most of them in China or the US. I went for "AISLER", which is a provider based in Germany. They charge only per board space (so not per amount of vias etc.) and have free shipping, so especially for small PCBs I think the price is very good. Another advantage is, that together with the PCB you can order parts from Digikey. Of course the parts are much more expensive than on Digikey itself, but first of all Digikey doesn't sell (to my knowledge) to private consumers and second, you avoid the extra shipping fees.
I also did a small breakout board for a SPI DAC chip, the MCP49x1 since I planned to use it in another project. I found out, that there is a minimum price per PCB, so I added a second breakout board on the same PCB and separated them with mousebites, which is much less expensive than ordering two separate PCBs.
You can find the schematic and layout files [here](https://github.com/MarcelMG/PCB/tree/master/SPI_Flash_and_DAC).
A nice feature of KiCad is the integrated 3D preview of the board. It looks like this:
![3d_board_preview](https://github.com/MarcelMG/PCB/raw/master/SPI_Flash_and_DAC/3d_board_preview.jpeg)
After about a week, the boards together with the ICs arrived and it was time to solder them.

![spi_flash_mem_breakout_back](https://github.com/MarcelMG/marcelmg.github.io/raw/master/images/spi_flash_mem_breakout_back.jpg){:height="240px" width="400px"} ![spi_flash_mem_breakout_front](https://github.com/MarcelMG/marcelmg.github.io/raw/master/images/spi_flash_mem_breakout_front.jpg){:height="240px" width="400px"}

The ICs are in SOIC package and I used 0805 sizes for the passives (decoupling caps), so hand-soldering was quite easy. One thing I did was elongate the IC pads in the layout to make hand-soldering easier. I also choose special hand-soldering footprints for the passives which are also a bit larger.
But as it almost always is the case, there had to be a (stupid) mistake in my PCB: I forgot to set the space between the header connectors, so that it can fit in a breadboard. So now I have to connect them with jumper-wires, but that's not such a big deal.

The next step was to write some functions to read and write to the memory chip. The procedure is actually quite simple once understood: the microcontroller sends an instruction via SPI and then (depending on the instruction), reads or writes the data. The commands are explained in the datasheet. An important thing to consider when using flash memory is that you can't just overwrite data with new data. That means, that in order to write into a specific location in the memory, the section has to be erased before. This is because the physical principle of toggling a bit from 0 to 1 and from 1 to 0 is different in flash memory technology.

After having written the driver for the SPI flash memory, I wrote small tool that allows to erase, read and write the memory chip via a serial terminal. I use the terminal program "Cutecom", which is small but has many features e.g. sending or receiving a file.
![spi_flash_tool_screenshot](https://github.com/MarcelMG/marcelmg.github.io/raw/master/images/spi_flash_tool_screenshot.png)
When writing a file to the flash memory, I did not implement any buffering, thus it works only if the baud rate of the serial communication is much smaller than the SPI communication speed with the flash memory. Else, the PC would send data faster than it can be written to the SPI flash memory and data would get lost. You can find the source code for this [here](https://github.com/MarcelMG/STM32F103C8T6/tree/master/W25Q64JV_SPI_FLASH_MEMORY).

When finally everything worked, I combined the SPI flash memory with the PWM audio-player that I presented in the last post.
Now I can store an audio file on the SPI flash chip and play it using PWM. You can check out the source code [here](https://github.com/MarcelMG/STM32F103C8T6/tree/master/FLASH_PWM_AUDIO_PLAYER). Since the chip I use has 8 Megabytes of space, using 8bit resolution and a sampling rate of 44.1kHz for the audio recordings, we can store about 3 min of audio in the chip.

Now all that's left to do is find a (semi-)useful application for this... But that's something for the next post!

