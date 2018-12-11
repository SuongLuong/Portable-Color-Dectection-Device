
# TCS34725 RGB Color Sensor & the Raspberry Pi
## TABLE OF CONTENT
I.   [Introduction](#i-introduction)

II.  [Thing used in this Project](#ii-things-used-in-this-project)

III. [Project Schedule](#iii-overheat-sensor-schedule)

IV.  [Setting Up](#iv-setting-up)

V.   [Printed Circuit Board PCB and Soldering](#v-printed-circuit-board-pcb-and-soldering)

VI.  [Power Up and Unit Testing](#vi-power-up-and-unit-testing)

VII. [Production Testing](#vii-production-testing)

### I. Introduction
The TCS34725 RGB color sensor board can be used to detect the color of objects. It works with the Raspberry Pi using an I2C connection.  When used to detect the color of things, the led should be on and the object should be put on the top of the enclosure closely. The theory of sensing the color of objects is Reflective Sensing Theory as below.

![d](https://user-images.githubusercontent.com/43184936/49826532-1f102780-fd55-11e8-8695-47e925f3c31f.png)
(retrieved from http://home.roboticlab.eu/en/examples/sensor/color)

- An System Diagram for the Project. It explains the connection between Raspberry Pi and the sensor.

![image](https://user-images.githubusercontent.com/43184936/49833644-25f46580-fd68-11e8-9849-7612f7e7980d.png)

### II. Things used in this project
- The TCS34725 Color Sensor
- Rasberry Pi 3 B+ module
- 6-pin Headers
- 2x20-pin Header
- Essential Softwares: Remote Desktop Connection / VNC Viewer, Fritzing(PCB Designing), CorelDraw(Enclosure Design), Product Printer in Prototype Lab.

The TCS3472 device provides a digital return of red, green, blue (RGB), and clear light sensing values. An IR blocking filter, integrated on-chip and localized to the color sensing photodiodes, allows color measurements to be made accurately. The TCS3472 an ideal color sensor solution for use under varying lighting conditions and through attenuating materials. 

![sensor](https://user-images.githubusercontent.com/43184936/49824796-e2dac800-fd50-11e8-8d7f-00377f148ccd.jpg)
(retrieved from: https://www.adafruit.com/product/1334)

![raspberry](https://user-images.githubusercontent.com/43184936/49824862-0a319500-fd51-11e8-9716-404b64f69bb5.jpg)
(retrieved from https://www.google.com/search?q=raspberry+pi+3&source=lnms&tbm=isch&sa=X&ved=0ahUKEwiz-YLm5pjfAhWE_YMKHWn6C_8Q_AUIDigB&biw=1440&bih=720#imgrc=7EPdukg2I5VbWM:)

- The pin header from Prototype Lab:
 
![2x20pins](https://user-images.githubusercontent.com/43184936/49826920-12d89a00-fd56-11e8-876b-23ddb2447342.jpeg)

![6pinsstackableheader](https://user-images.githubusercontent.com/43184936/49826921-13713080-fd56-11e8-9ddc-2c08ae6f2c13.jpg)

- Bill of Materials/Budgets: Total for hardware components. More detail <a href="https://github.com/SuongLuong/Portable-Color-Dectection-Device/files/2669320/Buget317.xlsx">Budget</a>

![image](https://user-images.githubusercontent.com/43184936/49828283-7dd7a000-fd59-11e8-9aae-924e75a76793.png)


### III. Project Schedule

The schedule for my whole project. 

![image](https://user-images.githubusercontent.com/43184936/49830020-06f0d600-fd5e-11e8-9068-03cf894a4bb1.png)

This project can be done in about 1 week time if all components are ready. The ordering process may be longer if you order outside Canada. Some websites to purchase: <a href="https://elmwoodelectronics.ca/">ElmWoodElectronic</a>

### IV. Setting Up
#### Student Raspberry Pi Image Creation and Test Code

1. Download, unzip, and copy the folder contents of http://downloads.raspberrypi.org/NOOBS/images/NOOBS-2017-08-17/NOOBS_v2_4_3.zip into the SD card (at least 8GB).

2. Insert the SD Card into the Raspberry Pi. For first time usage, a HDMI cable, a wireless mouse and a separate screen is required. 

3. Perform necessary update as instructed by the Pi steps by steps.

4. Open the terminal in the top left corner of the screen and input the following lines (this takes quite a long time):

wget https://raw.githubusercontent.com/six0four/StudentSenseHat/master/firmware/hshcribv01.sh \  
-O /home/pi/hshcribv01.sh  
chmod u+x /home/pi/hshcribv01.sh  
/home/pi/hshcribv01.sh

5. Reboot the RPI

#### Enterprise Wi-Fi
Connect to the Raspberry Pi through Wi-Fi will be easier. This is the instruction for connection the Raspberry Pi to the Humber Wifi:

1.  In /etc/network/interfaces:
	```
	auto lo
	iface lo inet loopback
	iface eth0 inet dhcp
	allow-hotplug wlan0
	iface wlan0 inet manual
	wpa-roam /etc/wpa_supplicant/wpa_supplicant.conf
	iface default inet dhcp
	```

2.  In /etc/wpa_supplicant/wpa_supplicant.conf:

    ```
	sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
	```

    ```
	# sample network configuration for Humber College myWi-Fi
	# change text in <> to your account values (remove the < and > while doing this )
	network={
		ssid="myWi-Fi@Humber"
		key_mgmt=WPA-EAP
		auth_alg=OPEN
		eap=PEAP
		identity="<YourHCNetID>"
		password="<YourHCnetPassword>"
		phase1="peaplabel=0"
		phase2="auth=MSCHAPV2"
		priority=999
		proactive_key_caching=1
	}

	# Sample network configuration for joining Eduroam Wi-Fi network
	# change text in <> to your account values (remove the < and > while doing this )
	network={
		ssid="eduroam"
		scan_ssid=1
		key_mgmt=WPA-EAP
		eap=PEAP
		identity="<YourHCnetID>@humber.ca"
		password="<YourHCnetPassword>"
		phase1="peaplabel=0"
		phase2="auth=MSCHAPV2"
		proactive_key_caching=1

	}

	# Sample network configuration for joining a home wi-fi network.
	# change text in <> to your network values (remove the < and > while doing this )
	network={
		ssid="<yourNetworkSSID"
		psk="<yourNetworkPassword>"
		proto=RSN
		key_mgmt=WPA-PSK
		pairwise=CCMP
		auth_alg=OPEN
	}

	#Sample networtk configuration for joining open, unsecured network
	network={
		ssid="<yourNetworkSSID>"
		key_mgmt=NONE
	}
	```

3.  Download Humber Certificate (For HumberSecure).cer from https://its.humber.ca/wireless/humbersecure/

4.  Reboot


### V. Printed Circuit Board PCB and Soldering
- The Breadboard view of the connection:

![image](https://user-images.githubusercontent.com/43184936/49830355-ee34f000-fd5e-11e8-82f9-a9ec99e9e771.png)

- The design for PCB on Fritzing: <a href="https://github.com/SuongLuong/Portable-Color-Dectection-Device/files/2669423/gerberfiles.zip">FritzingGerberFiles</a>

![image](https://user-images.githubusercontent.com/43184936/49830428-250b0600-fd5f-11e8-8b99-ff5a83d2960c.png)

- Soldering Process: Get started by reviewing the [soldering videos](https://radiojove.gsfc.nasa.gov/telescope/soldering.htm). By using equipment in the Lab, it may take 1 hour. Be carefull with the PCB sides to be connected properly.

![image](https://user-images.githubusercontent.com/43184936/49834731-23dfd600-fd6b-11e8-9508-3d3ccbf8ef65.png)

![image](https://user-images.githubusercontent.com/43184936/49830749-107b3d80-fd60-11e8-8114-dd1dacf86bb6.png)

![solder](https://user-images.githubusercontent.com/43184936/49830819-3ef91880-fd60-11e8-87f6-ffeedf95c4a3.jpeg)

![48100858-f8f5e580-e1f2-11e8-9daf-02c232f5b149](https://user-images.githubusercontent.com/43184936/49830858-618b3180-fd60-11e8-88bf-d1194a70eeca.jpeg)

#### Enclosure (Case for the whole hardware]

For this part, we need to work on CorelDraw. All the hardware dimensions should be measured carefully to fit the case. The default file for RPI case can be found on CENG317 folder. We need to modify this case and send the design files to Prototype Lab (prototypelab@humber.ca) to get the case. This case can be laser cut just in 5 minitues but you need to wait in line for other people.

My design file in the same directory with the name Pi2CaseX6.cdr. 

![image](https://user-images.githubusercontent.com/43184936/49835303-c8aee300-fd6c-11e8-8941-ad3ff21ddc0d.png)

The Fritzing file is available here: https://github.com/six0four/StudentSenseHat/tree/master/electronics/StudentSenseHatV06.fzz

The complete product:

![image](https://user-images.githubusercontent.com/43184936/49831439-e62a7f80-fd61-11e8-8572-3d28de42fa03.png)

![image](https://user-images.githubusercontent.com/43184936/49831448-eb87ca00-fd61-11e8-98b0-95c825fcbdde.png)
      
### VI. Power Up and Unit Testing
- Power Up: Through Ethernet cable, and Remote Desktop Control, you can connect and check your I2C address.

![a](https://user-images.githubusercontent.com/43184936/49832728-7c13d980-fd65-11e8-9d9b-92f88a5a5269.jpeg)


![ab](https://user-images.githubusercontent.com/43184936/49832748-9057d680-fd65-11e8-81ba-8b61d6444e3c.jpeg)

- Output: The code used to detect

sudo i2cdetect -y 1


![image](https://user-images.githubusercontent.com/43184936/49832819-c1d0a200-fd65-11e8-855b-d6973e42e7b2.png)

![image](https://user-images.githubusercontent.com/43184936/49832828-c5fcbf80-fd65-11e8-8ea6-eaaeb3163a8e.png)

Everytime we put the object near the sensor, the RGB value wil changed.

- The source code <a href="https://github.com/SuongLuong/Portable-Color-Dectection-Device/files/2669619/pythoncode.txt">Code</a>


### VII. Production Testing and Tip
- We need to make sure all the connection are correct:

TCS34725  RPi
SDA       P3 GPIO 0 (SDA)
SCL       P5 GPIO 1 (SCL)
3V3       P1 3V3
GND       P9

- Tip:
For the connection between RPI and Remote Desktop Control: We need to modify the network sharing first so the RPI will be recognized by the computer. IP Advanced Scanner can be used to scan your RPI IP address and make it easier. 

For the case: We better need an printed paper first to fit the hardware platform before printing the real laser-cut case. It will save time.
