# AntRunner-Pro
 * [Introduce](#introduce)
 * [Feature](#feature)
 * [Specification](#specification)
 * [How to Use](#how-to-use)
 * [System Detail](#system-detail)
    * [Gpredict](#gpredict)
    * [Hamlib](#hamlib)
    * [Look4Sat](#look4sat)
    * [SDR#](#sdr)
 * [How to Get One](#how-to-get-one)
 * [Reference](#reference)

## Introduce
AntRunner-Pro is pro verion of [AntRunner Rotator](https://github.com/wuxx/AntRunner) made by Muse Lab. It can be used for real-time automatic racking of satellites with corresponding open source software which is available on Windows/Linux/Mac/RaspberryPi/Android. support 360-degree azimuth and 90-degree elevation control, waterproof, waterproof, powered by a 12V power supply. controlled via Ethernet with the open source Gpredict. It can install various types of antennas (usually Yagi antennas), and can support antennas up to 10KG. It's perfectly suited for fixed installation on a rooftop for radio communication operations. 

![1](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/1.JPEG)
![2](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/2.JPEG)
![4](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/4.JPEG)

## Feature
- Full azimuth and elevation angle tracking
- Built-in slip ring
- Support Windows/Linux/RaspberryPi
- Control via ethernet 
- DC 12V power supply

## Specification
- Rotation Limit: AZ: 0 - 360°; EL: 0-90°
- Max Load: 10KG
- Backlash: AZ:1° EL:1°
- Weight: 6.25KG

## How to Use

### Windows

#### 0 Antenna installation
Prepare a circular tube with a diameter of 35cm, fix it on the support base, and then use a U-shaped buckle to fix the antenna on the circular tube. When fixing the antenna, make sure it is level with the ground

#### 1 Rotator adjustment
Return the rotator to its default horizontal state and face it towards the north, This step can be adjusted through the mobile compass APP.

#### 2 Power up
Use the 12V power supply to power on the rotator, after the rotator is powered on, it will perform a rotation self-test; connect the rotator to the router through a RJ45 cable, and get the rotator's IP address in the router's backend management interface.

#### 3 Start Gpredict
Double-click gpredict.exe to open the Gpredict program

![Gpredict-1a](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-1a.png)

##### 4.1 Longitude and latitude configuration
First, you need to configure the latitude and longitude of your region, select Edit -> General -> Ground Stations -> Add new, and configure your local latitude, longitude and altitude on the page that comes out.
![Gpredict-1c](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-1c.png)

##### 4.2 Satellite configuration
You need to add the satellites you want to track to the list, click the inverted triangle on the upper right, select Configure, and add the satellites you want to track to the small window on the right in the window that appears.
![Gpredict-1b](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-1b.png)


##### 4.3 Rotator configuration
Create a new rotator device, select Edit -> Interfaces -> Rotators -> Add New, and configure it as shown
![Gpredict-1d](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-1d.png)

##### 4.4 Rotator test
After configuring the rotator device, you can perform a preliminary test on the rotator. Click the inverted triangle on the upper right and select Antenna Control to enter the antenna control page.
select the newly created pelcodtrk, click Engage to initialize the rotator, and then configure the azimuth and pitch angles. After configuring the rotator, it will respond immediately, however, please note that the rotator can only accept the next command after it has moved to the specified position, so the current angle cannot be queried during the movement.
![Gpredict-2a](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-2a.png)
![Gpredict-2b](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-2b.png)
![Gpredict-2c](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-2c.png)
![Gpredict-2d](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-2d.png)

##### 4.5 satellite tracking
Select the satellite to be tracked in Target, such as ISS, click Track, the rotator will start tracking the satellite in real time, if the satellite is not in entry, it will adjust the pitch angle to 0 degrees, and the azimuth angle to the angle when the satellite enters, wait for the satellite to enter, The radar map on the left also shows the satellite's inbound trajectory and the current rotator position.
![Gpredict-2e](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-2e.png)
![Gpredict-2f](https://github.com/wuxx/AntRunner-Pro/blob/main/doc/Gpredict-2f.png)

### Linux/Raspberry
Since Gpredict is also supported under Linux, it can be directly run and used in the Linux/Raspberry Pi. The operation steps are basically the same, and will not be repeated here.

### Android
#### Look4Sat 
TODO

## System Detail
This section is a detailed description of the technical principle. Those who are not interested can ignore the description in this section and go directly to the actual operation chapter.

### Gpredict 
Gpredict (https://github.com/csete/gpredict) is lead by Alexandru Csete (call sign oz9aec) is an open source real-time satellite tracking and orbit forecasting software that can track an unlimited number of satellites and display their positions and other data in lists, tables, maps, radars, etc. It can also predict the future time through a satellite and provide you with detailed information. In addition, it can connect to a variety of commonly used radio stations, SDR equipment and a variety of antenna rotators. The main use scenario of AntRunner is to cooperate with Gpredict. Real-time tracking control.
Since it is an open source project based on GPL, Gpredict is still developing under the impetus of the community, and it is believed that more functions and more convenient features will be implemented in the future.

### Hamlib 
Hamlib (https://hamlib.github.io/) is a control library for radios and rotators based on the LGPL open source protocol, supporting Windows/Linux. Gpredict mentioned above can control various types of radio equipment and rotators. The above are all controlled by Hamlib. It can be understood that Hamlib is the middle layer between Gpredict and the actual hardware. Hamlib provides a unified control interface to Gpredict, and itself realizes the operation of complex hardware devices. In actual operation, Hamlib runs in the background as a separate task, which receives requests and sends responses through TCP port 4533. For example: Gpredict sends "p 30 60" through TCP port 4533, which means to adjust the current rotator azimuth to 30 degrees, the pitch angle is adjusted to 60 degrees, and the actual hardware operation is performed by Hamlib. Gpredict does not need to care what type of rotator is used, just specify the model of the rotator when Hamlib starts.
Note: AntRunner-Pro's driver is implemented in Hamlib, and has been incorporated into the official Hamlib repository (https://github.com/Hamlib/Hamlib), merge node: https://github.com/Hamlib/Hamlib/pull/1032. Just take the latest version of Hamlib or any release [version 4.5](https://github.com/Hamlib/Hamlib/releases/tag/4.5) or later.

### Look4Sat
Look4Sat (https://github.com/rt-bishop/Look4Sat) is an Android-based open source satellite tracking software implemented by Arty Bishop. The page is concise and easy to use. The latest submission also supports the control of the rotator. The real-time display of the gyroscope is supported during the tracking process, which can easily align the satellite. At present, other similar satellite prediction software does not support this simple use method, and it is widely used in the current HAM.
Look4Sat's control of the rotator is sent over the network, for example "\set_pos 30.0 60.0" means to adjust the current spinner azimuth to 30 degrees and pitch to 60 degrees.

### SDR#
SDR# is a popular SDR control application. It is simple and convenient to use. It supports a variety of common SDR devices, supports a variety of plug-in functions, and can be linked with Gpredict. At the same time, Gpredict can also support wireless devices and rotators. : Use SDR equipment such as RTL-SDR, and use Gpredict to track satellites at the same time, gpredict can send Doppler frequency to SDR# through the corresponding plug-in in SDR# for real-time adjustment, and Gpredict can control the rotator in real time for tracking. Other plug-ins in SDR# can analyze and record the received signal waveform to realize the linkage of the whole system.

### How to Get One
Some guys ask me if they could get a AntRunner-Pro rotator, so I made some AntRunner prototypes. Friends interested can place an order in my tindie store: https://www.tindie.com/products/johnnywu/the-antrunner-pro-rotator/

## Reference
- Gpredict (https://github.com/csete/gpredict) 
- Hamlib (https://hamlib.github.io/)
- Look4Sat (https://github.com/rt-bishop/Look4Sat)
- sdr# (https://airspy.com/download/)
- Look4Sat-AntRunner-Controller (https://github.com/wuxx/Look4Sat-AntRunner-Controller)
