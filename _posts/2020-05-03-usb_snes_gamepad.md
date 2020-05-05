---
layout: post
title: SNES gamepad USB converter with STM32F103
---
Playing classic games on the PC or tablet etc. is great fun, but without the real gamepad the experience is not authentic. In this post, I will show how to build a simple converter using the STM32F103 microcontroller that allows to connect a Super Nintendo (SNES) gamepad to the PC as a generic USB HID (Human Interface Device) gamepad. This has the huge advantage that the solution is plug-and-play, no drivers are needed on any OS.


In my case I use the cheap "generic STM32F103 development board" a.k.a. "Blue Pill"-Board, board you could use virtually any STM32 microcontroller that has an USB peripheral. In case of the cheap blue STM32F103 boards, these have a design flaw that often prevents the correct function of the USB. The resistor R10 on the backside of the PCB has a wrong value and is wrongly connected, so it needs to be removed. After the removal, solder a 1.5kΩ resistor between +3.3V and pin PA12 (USB D+ line). 


For the software, ST's *CubeHAL* will be used, which greatly simplifies the development especially when using USB which is quite complicated. To set up the project, the graphical tool *CubeMX* is used.
First, we configure the main clock in the **RCC** section. By selecting "Crystal/Ceramic Resonator" as HSE clock source, we use the 8MHz crystal present on the development board. When using USB it is crucial to use a crystal as the clock source, because the internal oscillator of the STM32 is not accurate enough for the precise timing needed for the USB connection. Under the **SYS** section, we select Debug->Serial Wire to use an ST-Link for flashing the device.
By ticking the box "Device (FS)" in the **Connectivity**->USB section we configure the USB peripheral. Under "Middleware->USB_DEVICE" we can select the Class of the USB device. Choose "Human Interface Device Class (HID)". Now we're ready to let the tool generate the code basis.

Afterwards, we need to apply some changes to the generated code. First, we need to make some changes in the files "usb_device.h" and "usb_device.c". We need to access the handle variable *hUsbDeviceFS* from within our main(), but by default this is not possible with ST's library. Thus we can implement a simple get-function that can be called in our main() and returns the wanted variable's pointer.

In *"usb_device.h"*, add:

{% highlight c %}

/* Private function prototypes -----------------------------------------------*/
/* USER CODE BEGIN PFP */
 USBD_HandleTypeDef* get_usbd_handle_ptr();
/* USER CODE END PFP */

{% endhighlight %}


And in *"usb_device.h"*, add:

{% highlight c %}

/* USER CODE BEGIN 1 */
USBD_HandleTypeDef* get_usbd_handle_ptr(){
	return &hUsbDeviceFS;
}
/* USER CODE END 1 */

{% endhighlight %}


By default, the generated code implements a Mouse Input Device, so we need to make some changes to create our own custom Gamepad Input Device. The properties of a HID Input device are stored in a so called *USB HID report descriptor*. You can find a good explanation about it at [[1]](https://eleccelerator.com/tutorial-about-usb-hid-report-descriptors/). To create our own HID descriptor, we can use the *HID Descriptor Tool* provided by the USB Implementers Forum, you can find it ![here](https://www.usb.org/document-library/hid-descriptor-tool).
In case of the SNES gamepad, it has 12 buttons that provide a logical value (i.e. either pressed or not pressed) and no analog joysticks. The appropriate report descriptor looks like this:

{% highlight c %}

__ALIGN_BEGIN static uint8_t HID_SNES_GAMEPAD_ReportDesc[HID_SNES_GAMEPAD_REPORT_DESC_SIZE]  __ALIGN_END =
{
  0x05, 0x01, // USAGE_PAGE (Generic Desktop)
  0x09, 0x05, // USAGE (Game Pad)
  0xa1, 0x01, // COLLECTION (Application)
  0xa1, 0x00, // COLLECTION (Physical)
  0x05, 0x09, // USAGE_PAGE (Button)
  0x19, 0x01, // USAGE_MINIMUM (Button 1)
  0x29, 0x0c, // USAGE_MAXIMUM (Button 12)
  0x15, 0x00, // LOGICAL_MINIMUM (0)
  0x25, 0x01, // LOGICAL_MAXIMUM (1)
  0x75, 0x01, // REPORT_SIZE (1)
  0x95, 0x10, // REPORT_COUNT (16)
  0x81, 0x02, // INPUT (Data,Var,Abs)
  0xc0,       // END_COLLECTION
  0xc0        // END_COLLECTION
};

{% endhighlight %} 

In the file "usbd_hid.c", replace *HID_MOUSE_ReportDesc\[HID_MOUSE_REPORT_DESDC_SIZE\]* with our new *HID_SNES_GAMEPAD_ReportDesc\[HID_SNES_GAMEPAD_REPORT_DESC_SIZE\]* from above. Additionally, replace the *#define HID_MOUSE_REPORT_DESDC_SIZE* with *HID_SNES_GAMEPAD_REPORT_DESC_SIZE* in "usbd_hid.h" and change the value (in this case, the size of the report descriptor above is 26).

The last thing to do is replace all occurences of *HID_MOUSE_ReportDesc* and *HID_MOUSE_REPORT_DESDC_SIZE* in the project with *HID_SNES_GAMEPAD_ReportDesc* and *HID_SNES_GAMEPAD_REPORT_DESC_SIZE* respectively. You can do this easily by using the Search&Replace function in your IDE.

Now we can jump to the "main.c" and write a simple test code to test our USB gamepad inside the *while(1)*-loop of *main()*: 


{% highlight c %}

/* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */
    /* USER CODE BEGIN 3 */
	uint8_t gamepad_buttons[2] = {0x00, 0x00};
	uint16_t test_buttons = 0;
	for(uint8_t i=0; i<12; ++i){
	  test_buttons ^= (1<<i);
	  gamepad_buttons[0] = (uint8_t) (test_buttons & 0xFF);
	  gamepad_buttons[1] = (uint8_t) (test_buttons >> 8);
	  USBD_HID_SendReport(get_usbd_handle_ptr(), gamepad_buttons, sizeof(gamepad_buttons));
	  HAL_Delay(300);
	}
  }
  /* USER CODE END 3 */
  
{% endhighlight %} 


This simple test code should simulate buttonpresses on all 12 buttons, so we can verify that everything works fine. On Windows 10, you should get something like this:


![usb_snes_gamepad_windows](https://raw.githubusercontent.com/MarcelMG/marcelmg.github.io/master/images/usb_snes_gamepad_windows.png)


Once the USB part of the project works correctly, we can go on and work out the connection with the SNES gamepad. A great source of information about the hardware pinout and communication protocol of the SNES gamepad can be found at [[2]](http://www.repairfaq.org/REPAIR/F_SNES.html). The communication protocol for the SNES gamepad is very simple: The SNES (or in our case, the microcontroller) toggles the *LATCH*-pin once by pushing the line high for 12µs and then pulling it low again. 6µs after the falling edge, the controller outputs 16bits of data serially via the *CLOCK*- and *DATA*-lines. To realize this communication, we can use the microcontroller's SPI peripheral. The connection is as follows:

<style>
.tablelines table, .tablelines td, .tablelines th {
        border: 1px solid black;
        }
</style>

|SNES  | STM32F103      |
|------|----------------|
|+5V   | +3.3V          |
|CLOCK | PA5 (SPI1 SCK) |
|DATA  | PA6 (SPI1 MISO)|
|LATCH | PA4            |
{: .tablelines}

Note that the SNES gamepad is normally powered with a +5V-supply by the SNES, but I found that mine works also with +3.3V. If you want to use a +5V supply for the SNES gamepad, you should use the alternate pins for SPI1 or SPI2 and use another GPIO-pin for LATCH. This is because on the STM32F103, the pins PA4, PA5 and PA6 are not 5V-tolerant (other pins are).

Since I didn't want to modify my SNES gamepad, I 3D-printed a connector. Luckily, I found a [model](https://www.thingiverse.com/thing:1753655) on *thingiverse* so I didn't have to measure and model it from scratch (Thanks to thingiverse-user *shantigilbert*).

Now that the hardware connections are made, let's take a look at the software. To configure the SPI peripheral, I will again use the CubeMX tool. Be careful when using it, because if we re-generate code, CubeMX will overwrite all the changes we have previously made to the USB-related files (usbd_hid.c/.h etc.). So back-them up before re-opening CubeMX and re-copy them later. To configure the SPI1-peripheral (under *Connectivity->SPI1*) select the mode "Receive Only Master". This means that the microcontroller acts as the Master, that is initiates the communication. Since we only need unidirectional communication for reading the SNES gamepad, we choose "Receive Only" mode (though we could also use Full-Duplex-Mode, it actually doesn't matter much here). We also disable the "Hardware NSS Signal", since here we don't use a Chip Select (CS a.k.a. NSS) signal. The appropriate parameters for communicating with the SNES gamepad are:

* Data Size: 16bits, LSB First
* Clock Polarity: High (CPOL=1)
* Clock Phase: 1st edge (CPHA=0)
* Prescaler: 256

The SNES uses a clock with a period of 12µs for the communication with the gamepad (i.e. 83.3kHz clock frequency). With the STM32F103's main clock frequency configured to 72MHz and the highest prescaler of 256 for SPI1, the clock frequency of SPI1 will be 281.25kHz. Although it is much higher, it still works flawlessly. The last thing to do in CubeMX is configure a GPIO pin (in my case I used pin PA4) as output and name it "LATCH_GPIO" (User Label) for convenience.

After generating code, we can go back to our main.c and implement the readout of the SNES gamepad. First we need to implement the toggling of the *LATCH*-line that triggers a new communication transfer. To create the precisely timed delays of 12µs and 6µs, I used the [*DWT_delay* library](https://github.com/keatis/dwt_delay/). We simply need to initialize the delay functionality like this:


{% highlight c %}
/* USER CODE BEGIN Init */
  DWT_Init();
/* USER CODE END Init */
{% endhighlight %} 


Then we can use *DWT_Delay()* to create µs-delays and implement the LATCH-toggle-sequence:


{% highlight c %}

HAL_GPIO_WritePin(LATCH_GPIO_GPIO_Port, LATCH_GPIO_Pin, GPIO_PIN_SET);
DWT_Delay(12); // 12µs delay
HAL_GPIO_WritePin(LATCH_GPIO_GPIO_Port, LATCH_GPIO_Pin, GPIO_PIN_RESET);
DWT_Delay(6); // 6µs delay

{% endhighlight %} 

We declare a two-element byte-array which will hold the 16bits of data from the gamepad:

{% highlight c %}

/* USER CODE BEGIN 1 */
  uint8_t gamepad_buttons[2] = {0x00, 0x00};
/* USER CODE END 1 */

{% endhighlight %}


With this, we can read out the data from the SNES gamepad:

{% highlight c %}

HAL_SPI_Receive(&hspi1, gamepad_buttons, 2, 50);

{% endhighlight %}

The 4th argument to the function is a timeout in ms, i.e. if there's an error in the SPI communication. E.g. if we receive no data, the function would return after 50ms in this case.

The first 12 bits that we received represent the state of the respective button of the gamepad. If the button is pressed, the value is 0, else 1. This is inverse to the format expected by the USB HID function, so we need to apply bit-wise inversion:

{% highlight c %}

gamepad_buttons[0] = ~gamepad_buttons[0]; // invert all bits
gamepad_buttons[1] = ~gamepad_buttons[1]; // invert all bits

{% endhighlight %}

Now we can finally send the button states via USB using the library's *USBD_HID_SendReport()* function. As first argument, it expects a pointer to the USB handle variable for which we wrote our get-function earlier on. The 2nd and 3rd arguments are the data and it's length respectively. But let's take a closer look at this function as it is implemented in "usbd_hid.c":


{% highlight c %}

/**
  * @brief  USBD_HID_SendReport
  *         Send HID Report
  * @param  pdev: device instance
  * @param  buff: pointer to report
  * @retval status
  */
uint8_t USBD_HID_SendReport(USBD_HandleTypeDef  *pdev,
                            uint8_t *report,
                            uint16_t len)
{
  USBD_HID_HandleTypeDef *hhid = (USBD_HID_HandleTypeDef *)pdev->pClassData;

  if (pdev->dev_state == USBD_STATE_CONFIGURED)
  {
    if (hhid->state == HID_IDLE)
    {
      hhid->state = HID_BUSY;
      USBD_LL_Transmit(pdev,
                       HID_EPIN_ADDR,
                       report,
                       len);
    }
  }
  return USBD_OK;
}

{% endhighlight %}


As we can see, the function always returns *USBD_OK* (which is a #define for 0), regardless of whether the USB transmission was sucessfull or not. The actual transmission is launched by *USBD_LL_Transmit()*, which returns either *USBD_OK*, *USBD_FAIL* (a #define for 2) or *USBD_BUSY* (a #define for 1). So it makes sense to modify USBD_HID_SendReport() so that it's return value represents the success of the transmission:

{% highlight c %}

uint8_t USBD_HID_SendReport(USBD_HandleTypeDef  *pdev,
                            uint8_t *report,
                            uint16_t len)
{
  USBD_HID_HandleTypeDef *hhid = (USBD_HID_HandleTypeDef *)pdev->pClassData;

  if (pdev->dev_state == USBD_STATE_CONFIGURED)
  {
    if (hhid->state == HID_IDLE)
    {
      hhid->state = HID_BUSY;
      return USBD_LL_Transmit(pdev,
                       HID_EPIN_ADDR,
                       report,
                       len);
    }
  }
  return USBD_BUSY;
}

{% endhighlight %}

Now the function returns 0 (*USBD_OK*) only if the transmission was sucessful, so we can use the return value to repeat the function call until it is sucessfull:

{% highlight c %}

while( USBD_HID_SendReport(get_usbd_handle_ptr(), gamepad_buttons, sizeof(gamepad_buttons)) != USBD_OK);

{% endhighlight %}

So at last, the main-loop looks like this:

{% highlight c %}

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
	  // toggle LATCH-pin on SNES-gamepad
	  HAL_GPIO_WritePin(LATCH_GPIO_GPIO_Port, LATCH_GPIO_Pin, GPIO_PIN_SET);
	  DWT_Delay(12); // 12µs delay
	  HAL_GPIO_WritePin(LATCH_GPIO_GPIO_Port, LATCH_GPIO_Pin, GPIO_PIN_RESET);
	  DWT_Delay(6); // 6µs delay
	  HAL_SPI_Receive(&hspi1, gamepad_buttons, 2, 50); // read 16bits (2 bytes) from gamepad
	  gamepad_buttons[0] = ~gamepad_buttons[0]; // invert all bits
	  gamepad_buttons[1] = ~gamepad_buttons[1]; // invert all bits
	  // send keypress states via USB, busy waiting until successfull
	  while( USBD_HID_SendReport(get_usbd_handle_ptr(), gamepad_buttons, sizeof(gamepad_buttons)) != USBD_OK);
	  HAL_Delay(1);
  }
  /* USER CODE END 3 */
  
{% endhighlight %}

Now we can compile the program (select "Release" target) and flash it to the microcontroller. 


*SIDE NOTE:
Don't launch the program in debug mode. If you use the debugger and enter a breakpoint, the USB connection will break down. This is because the microcontroller which acts as a USB device always has to be capable at any time to respond to the USB Host (the PC). If the program stops, it won't respond and the connection will fail. So compile with Target=Release and flash it using an external tool, e.g. the *STM32 ST-Link Utility*.
*


If everything works as expected, we can now enjoy playing some classic games using an authentic SNES gamepad! By the way, this works also on Android smartphones/tablets with USB-OTG functionality if you have the appropriate cable. I tested it successfully with the emulator-suite [RetroArch](https://www.retroarch.com/).


![usb_snes_gamepad_foto](https://raw.githubusercontent.com/MarcelMG/marcelmg.github.io/master/images/usb_snes_gamepad_foto.jpg)

You can find the source code for this project [here](https://github.com/MarcelMG/usb_hid_snes_gamepad).


Literature:

[1] [Tutorial about USB HID Report Descriptors](https://eleccelerator.com/tutorial-about-usb-hid-report-descriptors/)

[2] [Super Nintendo Entertainment System: pinouts & protocol](http://www.repairfaq.org/REPAIR/F_SNES.html)














