# CIPHER - A Detailed OverView of the Schematic:

This is CIPHER, A Compact Integrated Platform For Hardware & Engineering Research.  
And in this doc i will discuss about the schematic of the CIPHER and how each Module (referred as Daughterboard) will communicate and interact with each other. Also before reading this if u havent read [link](About-CIPHER.md).  
Starting off with the Overall Schematic, in this page i wanted to demonstrate how the Daughter Boards, will be connected to each other through connectors such as JST-XH, JST-SH, WS-90 etc.

# 1 Overall Schematic:

Figure.1:
![Schematic-1](images\Schematic\Schematic_page-0001.jpg)

In the Above Figure showing the Overall Schematic page, we are seeing 6 Hierarchial sheets which are as follows:

1. Main Board
2. Power Board
3. Navigation Board
4. GPIO Breakout Board
5. Instrument Board
6. Radio Board

The Connection between these 6 boards will be implemented in this way:

- Power Board $\to$ Main Board through 2(3.3V, 5V) 01x02 WS-90 Connectors
- Power Board $\to$ Radio Board through 3(3.3V, 3.8V, 5V) 01x02 WS-90 Connectors
- Power Board $\to$ Navigation Board through a 01x02 JST-XH connector for 5V and GND
- Power Board $\to$ GPIO Breakout Board through a 01x03(5V, GND, VBAT) JST-XH connector
- Power Board $\to$ Instrument Board through a 01x02(5V and GND) JST-XH connector

therefore total number of connectors dedicated for power are as follows:

- 5x 01x02 WS-90 Connectors
- 2x 01x02 JST-XH Connectors
- 1x 01x03 JST-XH Connectors

where

- 2x WS-90 for 3.3v rail
- 2x WS-90 for 5v rail
- 1x WS-90 for 3.8v rail
- 1x 01x02 JST-XH for 3.3v rail
- 1x 01x02 JST-XH for 5v rail
- 1x 01x03 JST-XH for VBAT and 5v rail

after completing the power rail , we can move on to the Signals Connectors.

here we'll first encounter the Navigation Board with a 01x04 JST-XH connector having (SCL, SDA, USB_DP, USB_DM) going to CM5 in the same order.

Next we have the GPIO Breakout Board connected to the Main Board through a 01x02 JST-XH Connector in the order (USB_DM, USB_DP) And same for the Instrument Board ASWELL,

and for the Connection between Radio Board and Main Board we will use a 01x08 JST-XH connector in the order (CEO, GPIO5, GPIO22, GPIO17, GPIO17, GPIO27, MOSI, MISO, SCK)

In conclusion:

1. 1x 01x04 JST-XH connector
2. 2x 01x02 JST-XH connector
3. 1x 01x08 JST-XH connector

And now with this he whole Overall Schematic is done connecting all the DaughterBoards with the Main Board.

# 2 NAVIGATION BOARD:

Figure.2:
![Schematic-2](images\Schematic\Schematic_page-0005.jpg)

In this Schematic Shown above, We have our 75% Keyboard, A MCU(RP2040), A GPIO Expander, 2 Rotaries with Switches, 1 TouchScreen TFT for Macros, 1 OLED for System Info and 1 Touch Panel For Trackpad.

## Details:

- **SW2 - SW85** Are Switches of the Keyboard forming a Matrix along with **D9 - D92**. Matrix Formed are **R1-R6** and **C1-C16**. where Rs are connected to the A2 (RP Pi Pico) in the following Connection:

| Row No. | Pin No. | Pin Name |
| ------- | ------- | -------- |
| R1      | 4       | GPIO2    |
| R2      | 5       | GPIO3    |
| R3      | 6       | GPIO4    |
| R4      | 7       | GPIO5    |
| R5      | 9       | GPIO6    |
| R6      | 10      | GPIO7    |

And C1-C16 are connected to the U16 (PCA9555D) in the following connections:
|Row No.| Pin No.|Pin Name|
|-------|--------|--------|
| C1 |4|IO0_0|
| C2 |5|IO0_1|
| C3 |6|IO0_2|
| C4 |7|IO0_3|
| C5 |8|IO0_4|
| C5 |9|IO0_5|
| C6 |10|IO0_6|
| C7 |11|IO0_7|
| C8 |13|IO01_0|
| C9 |14|IO01_1|
| C10 |15|IO01_2|
| C11 |16|IO01_3|
| C12 |17|IO01_4|
| C13 |18|IO01_5|
| C14 |19|IO01_6|
| C15 |20|IO01_7|

- Then , We Have SW86 and SW87 which are EC11 connected to the A2 (RP pi pico) in following connections:

| Pin No. Of Rotary Encoders | Pin No. of RP pi pico | Pin Name |
| -------------------------- | --------------------- | -------- |
| SW86_A                     | 32                    | GPIO27   |
| SW86_B                     | 34                    | GPIO28   |
| SW87_A                     | 12                    | GPIO9    |
| SW87_B                     | 14                    | GPIO10   |

and the switch side of these encoders are connected in the matrix in this way:

- SW86_S1 $\to$ C14 net
- SW86_S2 $\to$ R2 net
- SW87_S1 $\to$ C14 net
- SW87_S2 $\to$ R3 net

Next, we Have

- J28 (GME0831B), which is a 5" I2C touch panel whose SDA and SCL are connected directly to the CM5, and Int and RST pin to Pi Pico connected like:
- GME0831B_1 $\to$ P11 of pi pico
- GME0831B_2 $\to$ P12 of pi pico
- GME0831B_4 $\to$ SDA of pi pico
- GME0831B_5 $\to$ SCL of pi pico
  Then we have,
- two displays in which one of them is a 2.8" J25 TFT(used for Macros) and one is 0.96" J26 OLED (used for system info)

| TFT   | Pi pico |
| ----- | ------- |
| CS    | P17     |
| RST   | P21     |
| DC    | P20     |
| MOSI  | P19     |
| SCK   | P18     |
| LED   | P8      |
| MISO  | P16     |
| T_CLK | P18     |
| T_CS  | P26     |
| T_DIN | P19     |
| T_DO  | P16     |
| T_IRQ | P22     |

| OLED | Pi pico |
| ---- | ------- |
| SCL  | P14     |
| SDA  | P15     |

- And at last we have the Mcu A2 & GPIO U15 GPIO expander, Where GPIO expander (PCA9555D), Is connected to the pi pico on pin 14 & 15. using I2C. and pi pico is powered by the 5v lane.
