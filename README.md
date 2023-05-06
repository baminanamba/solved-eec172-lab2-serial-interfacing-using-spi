Download Link: https://assignmentchef.com/product/solved-eec172-lab2-serial-interfacing-using-spi
<br>
<strong>Objective: </strong>In this lab, you will use SPI to interface the CC3200 LaunchPad to a color OLED display.​

<strong>New Equipment Needed  </strong>

2 Adafruit OLED Breakout Board – 16-bit Color 1.5″ (product id: 1431) on small breadboards

1 Saleae USB Logic Analyzer <strong>(</strong>​ <strong>optional)  </strong>Jumper wires

<strong>Note: </strong>Do not lose the shorting blocks (jumpers) that come with the board. They are used to configure​      different hardware functions on the board and may be needed for this and future labs. Unused shorting blocks should be kept on the board attached to a single header pin.

<strong>Resources (Technical Data Sheets)  </strong>

Refer to the following technical documents.

<ul>

 <li>CC3200 Technical Reference Manual​</li>

 <li>CC3200 LaunchPad User Guide​</li>

 <li>CC3200 LaunchPad Schematic​</li>

 <li>CC3200 Datasheet​</li>

 <li>SSD1351 128 RGB x 128 Dot Matrix OLED/Common Driver with Controller Advance Information ​ ​• OLED Display Module Product Specification (Doc No. SAS1-D038-A)</li>

</ul>

<strong>Part I. Interfacing to the OLED using the Serial Peripheral Interface (SPI)  </strong>

In this part, you will interface to the 128×128 color organic light-emitting diode (OLED) display to the CC3200 LaunchPad using SPI.

<h1>1.  Implement SPI_demo project on CC3200 LaunchPads​</h1>

As a starting point, you should study, import, build, and test the modified version of spi_demo project on one CC3200 LaunchPads. Please, replace the main file provided for you with the original main file in your SDK project. Make sure to have a copy of original main file for yourself!

The project is documented at <u>http://processors.wiki.ti.com/index.php/CC32xx_SPI_Demo</u>​             and​ in the CC3200 SDK (C:tiCC3200SDK_1.4.0cc3200-sdkexamplesSPI_Demo). You will need to import the project. In this task you are using only master mode. For the SPI master, you should set MASTER_MODE to 1 in main.c:

<strong>#define </strong>​MASTER_MODE 1

For the SPI slave, you should set MASTER_MODE to 0, but in this lab, you do not need to do this:

<strong>#define </strong>​MASTER_MODE ​0

As described in the documentation, “a​ lways execute master application followed by slave application to avoid slave SPI receiving garbage”.

1

<h1>2.  Implement OLED interface using SPI​</h1>

For the SPI interface to the OLED, you will need to use the interface signals shown in the table below.

Note that you will use GPIO signals configured as outputs to control the DC (Data/Command), the

OLEDCS (OC) and the RESET signals on the OLED. You can choose any unused GPIO signals on the LaunchPad headers P1 or P2. However, check the LaunchPad schematic to verify that the signals are truly available. For example, the GPIO on P2.9 (Dev Pin#55) is the UART0_TX signal and is not available for GPIO when the console window is used. Do not use the LaunchPad’s RESET_OUT signal for the OLED RESET since you should be able to reset the OLED independently from the CC3200 LaunchPad. (NOTE: we are using a GPIO signal to control the OLEDCS instead of the dedicated SPI_CS signal because we plan multiple SPI devices on the CC3200’s general-purpose SPI port.). In this task, you only need to use the master_main program. OLED will not send back any data. You just need to send a couple of commands to the OLED.

<table width="297">

 <tbody>

  <tr>

   <td width="134"><strong>OLED signal  </strong></td>

   <td width="163"><strong>LaunchPad interface </strong></td>

  </tr>

  <tr>

   <td width="134">MOSI (SI)</td>

   <td width="163">SPI_DOUT ( MOSI)</td>

  </tr>

  <tr>

   <td width="134">SCK (CL)</td>

   <td width="163">SPI_CLK (SCLK)</td>

  </tr>

  <tr>

   <td width="134">DC</td>

   <td width="163">GPIO</td>

  </tr>

  <tr>

   <td width="134">RESET (R)</td>

   <td width="163">GPIO</td>

  </tr>

  <tr>

   <td width="134">OLEDCS (OC)</td>

   <td width="163">GPIO</td>

  </tr>

  <tr>

   <td width="134">SDCS (SC)</td>

   <td width="163">n.c. (no connection)</td>

  </tr>

  <tr>

   <td width="134">MISO (SO)</td>

   <td width="163">n.c. (no connection)</td>

  </tr>

  <tr>

   <td width="134">CD</td>

   <td width="163">n.c. (no connection)</td>

  </tr>

  <tr>

   <td width="134">3V</td>

   <td width="163">n.c. (no connection)</td>

  </tr>

  <tr>

   <td width="134">Vin (+)</td>

   <td width="163">3.3V</td>

  </tr>

  <tr>

   <td width="134">GND (G)</td>

   <td width="163">GND</td>

  </tr>

 </tbody>

</table>







Adafruit provides a link to an open source graphics library for this display. (See

<u>http://www.adafruit.com/product/1431 </u>for details.) The code is written for Arduino so we need to port it<u>​          </u> to the CC3200. We will provide modified library functions with the Arduino-specific code removed or commented out. Your task will be to write low-level functions WriteData() and WriteCommand() that use the SPI port to write a data or command byte to the OLED. You should study the SPI example program that you tested in Part 1. The WriteData() and WriteCommand() functions allow you to make

use of the higher-level OLED library functions. Look for comments with “TODO” in the Adafruit_OLED.c and test.c files for specific tasks.

Write a test program that mimics the Arduino example test.ino found at

<u>https://github.com/adafruit/Adafruit-SSD1351-library/blob/master/examples/test/test.ino</u> Your test program for the OLED display should do the following:

<ul>

 <li>Print the full character-set found in the font table in glcdfont.h.​</li>

 <li>Print the string “your full name!” to the display​</li>

 <li>Display a pattern of 8 bands of different colors ​ <em>horizontally </em>​    ​across the full OLED display. ​• Display a pattern of 8 bands of different colors <em>vertically </em>​ ​across the full OLED display.</li>

</ul>

2

<ul>

 <li>Call the testlines() function to display diagonal lines.​</li>

 <li>Call the testfastlines() function to display a rectangular grid pattern.​</li>

 <li>Call the testdrawrects() function to draw a pattern of rectangles.​</li>

 <li>Call the testfillrects() function.​</li>

 <li>Call the testfillcircles() and testdrawcircles() functions.​</li>

 <li>Call the testroundrects() function.​</li>

 <li>Call the testtriangles() function.​</li>

</ul>

There should be a small delay between calling each of these functions to allow each pattern to be clearly seen. Your program should be an infinite loop that continually cycles through the specified test patterns.

<strong>Verification  </strong>

Execute all these functions and demonstrate them in your video.

<strong>Debugging  </strong>

To debug your display, you can capture the SPI waveform using the Saleae logic program. It is optional! For this purpose, you should capture the very first packets transmitted from the micro controller. To be more specific, the first few lines of codes in the Adafruit_Init program are the WriteData() and WriteCommand(), and the Data/Command is sent through the SPI to the OLED  board. You should confirm these with you code and check how the captured waveform relates to these instructions.

<h1>Capture SPI waveforms using a Saleae logic</h1>

To use the logic analyzer on your own computer you will need to install the application (version 1.1.15) from <em><u>http://www.saleae.com/download</u></em><u>​          </u><u>​</u><u>s</u>. The Saleae User Guide can be found at<u>​      </u>  <em><u>http://downloads.saleae.com/Logic+Guide.pdf</u></em>​.

<ul>

 <li>When installing the wire harness into the logic analyzer pod, notice that the orientation is shown on the​ bottom of the pod. <strong><u>Make sure that the ground symbol on the bottom of the Saleae module is aligned</u></strong><u>​   </u><strong>  <u>with the grey wire</u>. </strong><u>​ </u>Black is data Channel 0, Brown is Channel 1 and so on (the colors follow the color​  code for resistor values). <strong>You should make the signal connections with both the CC3200 LaunchPad</strong>​    <strong> and the Saleae logic probe <em>unpowered</em></strong>​       ​<strong>!  </strong></li>

</ul>

<strong> </strong>

<ul>

 <li>The Saleae application includes various serial protocol analyzer, including SPI. Turn on the SPI ​ To configure the SPI analyzer, click on the gear icon and select Enable Settings from the menu  options.</li>

 <li>Capture an SPI waveform using the Saleae logic probe. It should look like this:​</li>

</ul>

3




<strong>Deliverables:  </strong>