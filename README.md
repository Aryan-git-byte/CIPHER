# CIPHER — Journal Export

- Exported at: 2026-05-15T09:21:32Z
- Project ID: 8
- Entries: 12

## Entry 1
- ID: 206
- Author: TheMudGuy
- Created At: 2026-03-19T09:36:23Z

### Content

# CIPHER
## Compact Integrated Platform for Hardware Engineering and Research


### Objective:
An integrated portable workstation with 8 gigabytes of ram, plenty of storage (target - 256 gb ssd), Exposed GPIO headers(SPI, I2C, UART, etc) to test sensors, components directly. 
The communication protocols of the workstation are LTE, LoRa, WiFi, and BLE.
It will have the following ports:
- 4x USB 3.0 A port
- 1x USB-C for Charging
- 1x USB 3.0 C for Data
- 1x Ethernet Port
- 1x 3.5 mm Audio Jack
- 1x Barrel jack for backup power to power the workstation directly from wall adaptor.
- 1x SD card Slot
- 1x self bouncing Nano SIM card slot
- 1x HDMI port


**For the GPIO pins we will have:**
- GP0 - GP11 : Digital IO/ PWM/ ADC
- I2C(SDA, SCL)
- SPI (MOSI, MISO, SCK, CS)
- 2x 3V3 pin
- 2x GND pin
- 2x 5V pin


## Modularity:


The Cyberdeck will be divided into 1 main board and 5 daughterboards(this number can be changed as we develop further) that will communicate with each other using protocols such as SPI,UART etc.
The main board will have :
- The CM5108000module itself
- And all the ports


**Now the Daughterboards are:**
1. Power Board:
 - it is responsible to lookup for the battery
 - Charging
 - Powering all the components in CIPHER
 - and exposing power outlet on the panel too
 - charge the batteries
 - protect from overcharge & over discharge, implement cell balancing, and obv prevent it from blowing up
 - supply 2  channels of 5v at 6A peak current and 3.3v at 5A peak with low power loss & high efficiency
 - monitor batteries and report it to CM5
 -8 18650 cells @ 2600 mah

2. Instrument Board
 - This board will have the Logic analyzer and a oscilloscope
 - the main mcu of this board will be a Pi pico 2, it will be placed there through female socket, making it replacable
3. GPIO Board
 - This board is responsible to expose the GPIO pins out the laptop
 - The main MCU of this board will be a  Pi pico 2, it will be placed there through female socket, making it replacable
 - It will be programmable through USB, which will be routed inside of the CIPHER
4. Radio Board
 - This will have all the communication modules such as LoRa, LTE, Sim slot, Antenna conectors.
 - This board will be operateed directly by the Main Board
5. Keyboard & Trackpad
 - This board will have a low profile 60-75% keyboard(tbd)
 - and a trackpad
 - The main mcu of this board is a  Pi pico 1, it will also be placed there through female socket, making it replacable

also i m using the devboard instead of IC , cuz using devboard can make it replacable and not waste the whole thing just bcz of a mcu if it blow. and it can also save up SMT cost.
currently i have started working on the power board
here how it went 
firstly i checked some ustom cyberdeck by others and how they are powered, read datasheet of cm5 about how much power it needs to work reliably. then  i searched differnet batteries & cells avaialble , from which i chose 
https://robu.in/product/dmegc-inr18650-26e-3-7v-2600mah-li-ion-battery/#tab-product_download_66939_tab
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NDY0LCJwdXIiOiJibG9iX2lkIn19--99e9d318f2b64b0493b13fdc75b7b92952fdb117/image.png)

read the datasheet of that exact batteries, searched reviews, studied abt it, then i started looking for things such as buck converter ics, charging ics, which were pretty hard to find in india so at last i decided to use LCSC.
then i read the datasheets of the following components to be used, and implemented their application in my schematic, and i also implemented battery protection features, and fuel gauge, 
this whole process was too long, from reading datasheets, searching hrs for a single ic to found that it has to be imported, now here are some pics of the schematics that i 've created as of now:

![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NDY1LCJwdXIiOiJibG9iX2lkIn19--65c0fc5126e3a5c03335227eda1e33f79321d981/image.png)
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NDY2LCJwdXIiOiJibG9iX2lkIn19--4674a02e0429b6660c441deef469eb08d2e472f2/image.png)
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NDY3LCJwdXIiOiJibG9iX2lkIn19--7d3c498c4ec95eb38647da86ffb6e6ab2b0144f6/image.png)
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NDY4LCJwdXIiOiJibG9iX2lkIn19--e217cde775419f40a3df447f4fbdd221689827eb/image.png)
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NDY5LCJwdXIiOiJibG9iX2lkIn19--33cf828c42bfbdc255074bd6105c50856cc283d4/image.png)
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NDcwLCJwdXIiOiJibG9iX2lkIn19--642e02fcd7e447f9c0324c02101e9da68f9779d0/image.png)


also, i forgot to timelapse this cm5 carrier board work, although its not much so i'll just ignore the hrs of it, it was smthg around 1-2 hrs too, it was mostly taken reefernce from the official cm5 carrier board thats why it was quick


### Recording Links

- https://public.lapse-hackclub.link/timelapses/pCE9Pr_62jN5/timelapse-pCE9Pr_62jN5.mp4

## Entry 2
- ID: 320
- Author: TheMudGuy
- Created At: 2026-03-22T14:30:19Z

### Content

today, first i started by implemting chargin feature in my schematic with ip2368, but i found out that the symbol of this ic didnt existed for kicad, so had to drew it too, and then started implementing it in the board , and overall completed the whole power board except the connectors that are to be placed:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NzMzLCJwdXIiOiJibG9iX2lkIn19--dd6c132050b324550d7d831310431dc2c5d12e43/image.png)

![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NzM0LCJwdXIiOiJibG9iX2lkIn19--d8e7242757265bec05ba607a5c5f591c683ccc91/image.png)

after that i started working on the navigation board which had, a tft, an oled, low profile mx style switches, rotary encoders and a touch panel for trackpad.
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NzM1LCJwdXIiOiJibG9iX2lkIn19--1155d266f372d7c8a0c44d8fc96cce2601b867ff/image.png)
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NzM2LCJwdXIiOiJibG9iX2lkIn19--d487cc2937718f4d682e722507185c2846942ef8/image.png)
the problem that i faced while making the navigation board was that i couldnt find a easy to use modular trackpad or touchpad at first, which i overcame by using this touch panel for evelta:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NzM3LCJwdXIiOiJibG9iX2lkIn19--d07c7a7ac7d138d015a84e13aa09b5aa7e4cbafe/image.png)

after that i started working on the breakout board which is still kinda incomplete, i spent most of my time finding the best ICs for integrated power supply, but i realized that even a basic buck and separte boost can do the job, so i did that, and implemented both buck & boost parrallely:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NzM4LCJwdXIiOiJibG9iX2lkIn19--bd4bb754c9c3a8cc45fee8e41b6b6c848b4eeec3/image.png)
thats all i did  on this day, see yaa

### Recording Links

- https://public.lapse-hackclub.link/timelapses/NV78D3_SI4x4/timelapse-NV78D3_SI4x4.mp4
- https://public.lapse-hackclub.link/timelapses/kjuWi_eqHoEB/timelapse-kjuWi_eqHoEB.mp4
- https://public.lapse-hackclub.link/timelapses/5uAo6Y--3Oyj/timelapse-5uAo6Y--3Oyj.mp4

## Entry 3
- ID: 1218
- Author: TheMudGuy
- Created At: 2026-04-05T07:55:29Z

### Content

hiii, today i started by organizing the CM5 carrier board schematic, which previously looked like a mess is now a clean schematic:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6MjY4NywicHVyIjoiYmxvYl9pZCJ9fQ==--b0629d4bed1bdc8df935fa9f700eb4c5b1a9ae7f/image.png)
and the changes i did are as follows:
- i replaced stacked USB_A with 1x USB3-A and 1x USB3-C.the takeoff the USB-C is that it will behave as usb3 only one way and when inserted the other way it will behave as a normal USB2.0:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6MjY4OCwicHVyIjoiYmxvYl9pZCJ9fQ==--804f9c0d27bc157e94d95d26223e9b369746c749/image.png)

- i also added a USB2.0 hub ic , to connect the navigation board and GPIO breakout board with the CM5 through USB:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6MjY4OSwicHVyIjoiYmxvYl9pZCJ9fQ==--f0ed5c4a755b2c197268ed97bff6f1912bfe7916/image.png)
- i also replaced 2x HDMI-A with 1x HDMI-A and 1x HDMI-C.:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6MjY5MCwicHVyIjoiYmxvYl9pZCJ9fQ==--01b6dd6b43ce2487b99330b3182d33fafd1a0434/image.png)
- and simplified the NVMe port:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6MjY5MSwicHVyIjoiYmxvYl9pZCJ9fQ==--64f8e2c65c5ece7079897507e53d0dfa6c552333/image.png)
after that i started researching about the Monitor to use and selected this one:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6MjY5MiwicHVyIjoiYmxvYl9pZCJ9fQ==--fd478a76cad30f9eab16e754b4c6620a720c74c8/image.png)
this is all the things i did, and as always chosing Display is not easy man, u have to like scroll a lot and then hop on one.

### Recording Links

- https://public.lapse-hackclub.link/timelapses/9NkPjEwhZABB/timelapse-9NkPjEwhZABB.mp4
- https://public.lapse-hackclub.link/timelapses/8x_S-KY60xsu/timelapse-8x_S-KY60xsu.mp4
- https://public.lapse-hackclub.link/timelapses/LRWA-5fO69hY/timelapse-LRWA-5fO69hY.mp4

## Entry 4
- ID: 3183
- Author: TheMudGuy
- Created At: 2026-04-21T10:12:32Z

### Content

Hiiii, so today i decided to complete the schematic of the CIPHER - main board so first  i placed a DAC IC PCM5102A for high quality sound in my CyberDeck and then i connected it along with a headphone jack:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYwNywicHVyIjoiYmxvYl9pZCJ9fQ==--a8054fb62250cacdf5d21e3cd89292033a61fcb0/image.png)
after that i made the connection of the NvMe Slot for the ssd
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYwOSwicHVyIjoiYmxvYl9pZCJ9fQ==--d34a88d368248a1b31b249647bbce57a9954f1ca/image.png)
and after completing this i started working on the ethernet and made its schematic too 
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYxMCwicHVyIjoiYmxvYl9pZCJ9fQ==--c1e4acbe115d5b823157e66f1f4ecf42e7c8b1de/image.png)


### Recording Links

- https://public.lapse-hackclub.link/timelapses/tmAsFf3opQUc/timelapse-tmAsFf3opQUc.mp4

## Entry 5
- ID: 3184
- Author: TheMudGuy
- Created At: 2026-04-21T10:21:03Z

### Content

Hiiiii, so today i started by arranging my messed up schematic, and i wired the 40 pins header in the main board for future use or if smtimes i need em in case
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYxMSwicHVyIjoiYmxvYl9pZCJ9fQ==--43b5f0f3301a5b4c2a8c88a4f03b2643a96c3eeb/image.png)
 by referencing the actual CM5IO Board, so that it is standardized.
after that i completed the SD card schematic and organized it.:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYxMiwicHVyIjoiYmxvYl9pZCJ9fQ==--c571bf94b39f3deb9f95c923eb74f071177696aa/image.png)
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYxMywicHVyIjoiYmxvYl9pZCJ9fQ==--12e931cb2c4157d8730af5ec0a93f062bb13330b/image.png)
with a power load switch.

after that i added misc. such as leds , missed out labels, headers etc based on the actual CM5IO board and referncing it .

i even put dozens of test points here and there so that i can be safe in the future with my test points if smthg goes wrong 

then i added DSI/CSi connectors in the schematic:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYxNSwicHVyIjoiYmxvYl9pZCJ9fQ==--93dc9349015a119cfe8ccf7ae88140f72ace0cfb/image.png)
 and after that i organized it in a hierarchial sheet and labelled em and conencted em neatly:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYxNywicHVyIjoiYmxvYl9pZCJ9fQ==--fb81ff6f4cbd3feca4cf060d7336e09722803566/image.png)
after that i doubled whole main board and ensured everythign is right and connected left out lables or pins.

### Recording Links

- https://public.lapse-hackclub.link/timelapses/-HCTVPgyLOj2/timelapse--HCTVPgyLOj2.mp4

## Entry 6
- ID: 3193
- Author: TheMudGuy
- Created At: 2026-04-21T11:01:51Z

### Content

Hi yall, so i wanted to add LTE to my Cyberdeck, because thats cool, + like i can maybe attend my calls with a cyberdeck too ,and access internet from anywhere. so i started my hunting by searching for some modules and stumbled upon SIM7670-G and sim7600e-h, both of which are very popular options. then i started to dig more about em , and found out that sim7600e-h is superior to the SIM7670-G in terms of download & upload speed.
but also it has some downsides that it is a bit more expensive and it has more current requirement, but its features justify them.
so after that i started looking for its schematic and refernce designs to follow. and imported the symbol to the kicad and placed it.

after that  i started by adding another usb hub in the Schematic bcz my other board were in need of more"
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYyMSwicHVyIjoiYmxvYl9pZCJ9fQ==--fb30dfa1f4e256e564d0515cf95b4f3fe7005107/image.png)
and fighting with wires & neatness in the hierarchial sheet nets.

then i morphed one power supply from 5v to 4.2v then 3.8v for the radio board.then i read its datasheet again and watched some reference designs to connect it.

![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYyNywicHVyIjoiYmxvYl9pZCJ9fQ==--90eb44ac0f999aee38a75b1f2352c3c4a1cb40a7/image.png)


### Recording Links

- https://public.lapse-hackclub.link/timelapses/MTlOoLgT0HFM/timelapse-MTlOoLgT0HFM.mp4
- https://public.lapse-hackclub.link/timelapses/xy6RJpzRv5JQ/timelapse-xy6RJpzRv5JQ.mp4

## Entry 7
- ID: 3195
- Author: TheMudGuy
- Created At: 2026-04-21T11:05:27Z

### Content

hiii, this time i added some missing components in the schematic and added bunch of testpoints. and tehn i started researching for other modules and radio communications to  have on my deck, and i finalized LoRa as one of them, and i researched bout it added it to schematic and wried it all :
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYyOCwicHVyIjoiYmxvYl9pZCJ9fQ==--6e9c0c642d6c31016adf0f72552f0764f47df23a/image.png)
and also i m using this specific LoRa module:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYzMSwicHVyIjoiYmxvYl9pZCJ9fQ==--26a7c67d26d5714ffe13f7481ca050f01f51b63c/image.png)
SX1262S915N0S1

### Recording Links

- https://public.lapse-hackclub.link/timelapses/B6GcOzRcl61j/timelapse-B6GcOzRcl61j.mp4

## Entry 8
- ID: 3199
- Author: TheMudGuy
- Created At: 2026-04-21T11:19:14Z

### Content

today, my goal was to complete the whole schematic .
first i polished the main board and then i made the instrument board that consist of  a logic analyzer and an oscilloscope:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYzMywicHVyIjoiYmxvYl9pZCJ9fQ==--fd5fa50d4150bbb663b7e1d7dfba5215e87ec97f/image.png)
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjYzNCwicHVyIjoiYmxvYl9pZCJ9fQ==--2a7ac0b53db21b0080257552a358f0044f336cdc/image.png)
this board is connected to the main board through USB data lines.
which will communicate over serial communication to the main board.


### Recording Links

- https://public.lapse-hackclub.link/timelapses/2C-y4Fbgew83/timelapse-2C-y4Fbgew83.mp4

## Entry 9
- ID: 3200
- Author: TheMudGuy
- Created At: 2026-04-21T11:32:17Z

### Content

Hi, today i started by reading ERCs, and trying to fix them. then i started adding connectors in boards, writing its documentations, doing minor changes allover the schematic, sorry i dont remember much bcz im journaling after like 6 days, but i will consistent starting from today.
soo yhh i did all that, start writing documentations 
```
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
```

okay so after this i faced a sudden power cut and my all files + a 2 hr recording,, so i was very depressed after this. and stopped working on CIPHER for 1-2 days , then i retur back on 18th april. the main issue was that the files in the git history were too old. and the kicad only had backup of overall schematic, not others  hierarchial sheets,
 so  regarding this i came and undo whatever i had in my git history and with a pdf i saved last time, i started restoring the schematic bit by bit

### Recording Links

- https://public.lapse-hackclub.link/timelapses/ZGyMXbbkUDuW/timelapse-ZGyMXbbkUDuW.mp4
- https://public.lapse-hackclub.link/timelapses/A-Lmj8DnW7kE/timelapse-A-Lmj8DnW7kE.mp4

## Entry 10
- ID: 3201
- Author: TheMudGuy
- Created At: 2026-04-21T11:41:46Z

### Content

Hi today i started again by restoring the schematic and writing the docs of the Schematic connections:
```
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

```

and this is the complete schematic as of now:
![output3_page-0001.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY0NiwicHVyIjoiYmxvYl9pZCJ9fQ==--c6f011b0f974c242ebba0e087a1cdff2cb717ee4/output3_page-0001.jpg)
![output3_page-0002.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY0NywicHVyIjoiYmxvYl9pZCJ9fQ==--e478a5cdae515389885dd7ca97fc4887f57ef337/output3_page-0002.jpg)
![output3_page-0003.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY0OCwicHVyIjoiYmxvYl9pZCJ9fQ==--f3770530b317271dcee3b65b10dfbf76696d2bfb/output3_page-0003.jpg)
![output3_page-0004.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY0OSwicHVyIjoiYmxvYl9pZCJ9fQ==--fb55b2ea8913519341a115b7305474e3d2d43728/output3_page-0004.jpg)
![output3_page-0005.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY1MCwicHVyIjoiYmxvYl9pZCJ9fQ==--6d42e7e964e842e4a3cbf5ad2d0894299b99df57/output3_page-0005.jpg)
![output3_page-0006.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY1MSwicHVyIjoiYmxvYl9pZCJ9fQ==--1fe082f544b4e0615d369523e904031f49b29df4/output3_page-0006.jpg)
![output3_page-0007.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY1MiwicHVyIjoiYmxvYl9pZCJ9fQ==--51f1a84dea48f91b6543d4be1fa045e8a58b56f4/output3_page-0007.jpg)
![output3_page-0008.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY1MywicHVyIjoiYmxvYl9pZCJ9fQ==--cadcb92da8a719edbb72386aef70fc31df661b10/output3_page-0008.jpg)
![output3_page-0009.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY1NCwicHVyIjoiYmxvYl9pZCJ9fQ==--f1358eaf8c13682bffddea875690fed1cca3ecc0/output3_page-0009.jpg)
![output3_page-0010.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6NjY1NSwicHVyIjoiYmxvYl9pZCJ9fQ==--38a69010fd38efb0ebf79c6b11173bb82f87e282/output3_page-0010.jpg)
and after that i started journaling my projects

### Recording Links

- https://public.lapse-hackclub.link/timelapses/LeHkxJ-kFLGI/timelapse-LeHkxJ-kFLGI.mp4
- https://public.lapse-hackclub.link/timelapses/Tcjf3Nbry5ca/timelapse-Tcjf3Nbry5ca.mp4

## Entry 11
- ID: 6212
- Author: TheMudGuy
- Created At: 2026-05-09T05:51:37Z

### Content

Hi, so today i started by importing some symbol to kicad and then changed the buck converter ic in the power board to make it more power-efficient and previous buck converters were overkill so fixed it then i also edited the ic symbol to make it easier to use and designed the whole schematic:
![image.png](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6MTMxNzEsInB1ciI6ImJsb2JfaWQifX0=--f2ae491665c92e5715a06aecd4c9f8d6850b4785/image.png)


### Recording Links

- https://public.lapse-hackclub.link/timelapses/xkmwdyNnk9PY/timelapse-xkmwdyNnk9PY.mp4

## Entry 12
- ID: 6589
- Author: TheMudGuy
- Created At: 2026-05-11T13:45:11Z

### Content

Today i changed my charging circuitory from those complex IC to simple charging circuit with a barrel jack 
![Screenshot_2026-05-11-19-13-13-431_com.android.chrome-edit.jpg](/user-attachments/blobs/redirect/eyJfcmFpbHMiOnsiZGF0YSI6MTQyMDEsInB1ciI6ImJsb2JfaWQifX0=--d4d05697bc35552d35e85d08e6599cd9cde38c19/Screenshot_2026-05-11-19-13-13-431_com.android.chrome-edit.jpg)
Because I didn't wanted to make the BOM and project anymore complex I stripped down that much complexity and I may strip down even more functionalities for V1 in future.

### Recording Links

- https://public.lapse-hackclub.link/timelapses/vfAe_d3sIZTV/timelapse-vfAe_d3sIZTV.mp4
