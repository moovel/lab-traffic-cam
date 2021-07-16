## Jetson Nano

- [Jetson Nano](#jetson-nano)
  - [Limitations](#limitations)
  - [Shopping list](#shopping-list)
  - [Setup Opendatacam](#setup-opendatacam)
    - [1. Flash Jetson Nano:](#1-flash-jetson-nano)
    - [2. Set correct Powermode according to your Power supply](#2-set-correct-powermode-according-to-your-power-supply)
      - [Using microUSB](#using-microusb)
      - [Using barrel jack (5V - 4A)](#using-barrel-jack-5v---4a)
    - [3. Setup a swap partition:](#3-setup-a-swap-partition)
    - [4. Install Opendatacam](#4-install-opendatacam)
    - [5. (optional) Run on USB Camera](#5-optional-run-on-usb-camera)
    - [6. Test Opendatacam](#6-test-opendatacam)
    - [7. Access Opendatacam via Wifi hotspot](#7-access-opendatacam-via-wifi-hotspot)
    - [8. Tips](#8-tips)
    - [9. Build a case](#9-build-a-case)

### Limitations

Jetson Nano has two power mode, 5W and 10W.

Once Opendatacam is installed and **running without a monitor**, it runs perfectly fine on 5W powermode _(which is nice because you can power it with a powerbank)_. If you use it with a monitor connected, the display will be a bit laggy but it should work.

We recommend you to do the setup with a monitor connected and then make your Jetson nano available as a Wifi hotspot to operate it from another device.

The 10W Power mode of the Jetson won't bring much performance improvement for Opendatacam.

### Shopping list

The minimum setup for 5W power mode is:

- 1 Jetson nano
- 1 Camera [USB compatible camera](https://elinux.org/Jetson_Nano#Cameras) or [Raspberrycam module v2](https://www.raspberrypi.org/products/camera-module-v2/)
- 1 Wifi dongle, [this one is compatible](https://www.edimax.com/edimax/merchandise/merchandise_detail/data/edimax/in/wireless_adapters_n150/ew-7811un/) out of the box, or [see compatibility list](https://elinux.org/Jetson_Nano#Wireless).
- 1 MicroSD card (at least 32 GB and 100 MB/s)
- 1 Power supply: either a [5V⎓2A Micro-USB adapter](https://www.adafruit.com/product/1995) or a Powerbank with min 2A output.

For 10W power mode _(this is good for desktop use when you plug the screen, the mouse, the keyboard, it draws powers from the peripherics)_

- Power supply: [5V⎓4A DC barrel jack adapter, 5.5mm OD x 2.1mm ID x 9.5mm length, center-positive](https://www.adafruit.com/product/1466)
- 1x [2.54mm Standard Computer Jumper](https://www.amazon.com/2-54mm-Standard-Computer-Jumper-100pack/dp/B00N552DWK/) This is used on the J48 Pin when supplying power from the jack entry instead of the microUSB. It tells the Jetson to by-pass the microUSB power entry.

For setup:

- 1 usb mouse
- 1 usb keyboard
- 1 screen (HDMI or Displayport)
- And for faster connexion, a ethernet cable to your router

Learn more about Jetson Nano ecosystem: [https://elinux.org/Jetson_Nano#Ecosystem_Products_and_Sensors](https://elinux.org/Jetson_Nano#Ecosystem_Products_and_Sensors)

### Setup Opendatacam

#### 1. Flash Jetson Nano:

Follow [Flashing guide](FLASH_JETSON.md#Jetson-Nano) (don't forget to verify if CUDA is in your PATH)

#### 2. Set correct Powermode according to your Power supply
##### Using microUSB

Using microUSD with a powerbank or a 5V⎓2A power supply, you just need to plug-in and the Jetson Nano will start when connected to it.

When started, we advise you to set the powermode of the Jetson Nano to 5W so it won't crash, to do so, open a terminal and run:

```
sudo nvpmodel -m 1
```

To switch back to 10W power mode (default)

```
sudo nvpmodel -m 0
```

##### Using barrel jack (5V - 4A)

When working with the Jetson Nano with the monitor connected, we advise to use the barrel jack power. In order to do so you need first to put a jumper on the J48 pin (more details on Jetson Nano power supply)

![jumper](https://user-images.githubusercontent.com/533590/60701138-edca9500-9efa-11e9-8c51-6e2b421ed44b.png)

By default, the Jetson Nano will already run on the 10W power mode, but you can make sure it is by running:

```
sudo nvpmodel -m 0
```

#### 3. Setup a swap partition:

In order to reduce memory pressure (and crashes), it is a good idea to setup a 6GB swap partition. _(Nano has only 4GB of RAM)_

```bash
git clone https://github.com/JetsonHacksNano/installSwapfile
cd installSwapfile
chmod 777 installSwapfile.sh
./installSwapfile.sh
```

Reboot the Jetson nano

#### 4. Install Opendatacam

You need to install [Docker compose](https://blog.hypriot.com/post/nvidia-jetson-nano-install-docker-compose/) (no official installer available for ARM64 devices)

```bash
sudo apt install python3-pip

sudo apt-get install -y libffi-dev
sudo apt-get install -y python-openssl
sudo apt-get install libssl-dev
python3 -m pip install -U pip setuptools

sudo pip3 install docker-compose
```

And then install OpenDataCam

```bash
# Download install script
wget -N https://raw.githubusercontent.com/opendatacam/opendatacam/v3.0.1/docker/install-opendatacam.sh

# Give exec permission
chmod 777 install-opendatacam.sh

# NB: You will be asked for sudo password when installing the docker container

# Install command for Jetson Nano
./install-opendatacam.sh --platform nano
```

#### 5. (optional) Run on USB Camera

By default, OpenDataCam will start on a demo file, but if you want to run from an usbcam you should

- Verify an USB Camera is connected

```bash
ls /dev/video*
# Output should be: /dev/video1
```

- Change `"VIDEO_INPUT"` in `config.json`

```json
"VIDEO_INPUT": "usbcam"
```

- Change `"usbcam"` device in `config.json` depending on the result of `ls /dev/video*`

For example:

```json
"v4l2src device=/dev/video1 ..."
```

- Restart docker

```
sudo docker-compose restart
```

_N.B : there is some issue to support out of the box (docker install) run from the CSI cam (raspberry pi cam), please see: https://github.com/opendatacam/opendatacam/blob/master/documentation/CONFIG.md#run-from-raspberry-pi-cam-jetson-nano for more info, you need to do a manual install for this_

#### 6. Test Opendatacam

Open `http://localhost:8080`.

This will be super slow if you are using this directly on the monitor connected to the Jetson nano, see next step to access Opendatacam from an external device.

#### 7. Access Opendatacam via Wifi hotspot

N.B: you need a wifi dongle for this.

- [Make jetson available as a WiFi hotspot](../WIFI_HOTSPOT_SETUP.md)
- Reboot
- Click on the network icon on the top right > Connection Information

![connectioninformatio](https://user-images.githubusercontent.com/533590/60710337-bf58b400-9f12-11e9-8056-987f0b5ea583.png)

Take note somewhere of the Jetson IP Address, in this case 10.42.0.1

- Unplug monitor / ethernet / keyboard / mouse and reboot

- Connect with another device to this Wifi network, and open <IP_OF_JETSON>:8080 in your browser

_After rebooting the Jetson Nano may takes 1-5 min to start the docker container, so if your browser say "Page not found", just retry after a few minutes_

You should be able to operate Opendatacam without lag issues.

#### 8. Tips

- You'll notice there are no button to power on / off button your Jetson Nano. When you plug the power supply it will power on immediately. If you want to restart you can just un-plug / re-plug if you are not connected via a Monitor or SSH. There is a way to add power buttons via the J40 pins, [see nvidia forum](https://devtalk.nvidia.com/default/topic/1050888/jetson-nano/power-and-suspend-buttons-for-jetson-nano/post/5333577/#5333577).

- You can connect your Jetson to ethernet and SSH into it to do all the setup without having to connect a monitor (after having setup a fixed IP)

#### 9. Build a case

[Here are the steps to set up the Jetson NANO in the Wildlife Cam Casing from Naturebytes.](HOUSING.md)
