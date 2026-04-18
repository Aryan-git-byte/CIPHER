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
- Power Board $\to$ GPIO Breakout Board through a 01x03(3.3V, GND, VBAT) JST-XH connector
- Power Board $\to$ Instrument Board through a 01x03(3.3V, GND, 5V) JST-XH connector

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