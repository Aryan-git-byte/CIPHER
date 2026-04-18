# Power Supply:

1x5v@8A peak
1x3.3@8A peak
1x3.8v@8A peak

# Power Consumption:

### 5V rail:

1. CM5108000 - 2.5A(ref.[link](https://pip-assets.raspberrypi.com/categories/944-raspberry-pi-compute-module-5/documents/RP-008180-DS-6-cm5-datasheet.pdf?disposition=inline), Section B.3. Power budget )

2. The 3007 Cooling Fan For Raspberry Pi Compute Module 5 - 0.5A (ref. [link](https://www.waveshare.com/cm4-fan-3007-b.htm?srsltid=AfmBOoo_jx59p4GUA53h08jMqbzjG3b7E_QtJ08EYZFVpmIr8XMdigU6#:~:text=Table_content:%20header:%20%7C%20Model%20%7C%203007%2DB%2D5V%20%7C,3007%2DB%2D5V:%20125%20%C2%B1%2010mm%20%7C%203007%2DB%2D12V:%20%7C))

3. HDMI connectors - 200 mA at max

4. C2684433 - 200 mA at max

5. USBs - 3A at peak, if every port is draining the most current.

6. 3x Raspberry Pi Picos - 100 mA each, so around 0.4A max

### 3v8 rail:

1. SIM7600E-H - 2A at max

### 3v3 rail:

1. 