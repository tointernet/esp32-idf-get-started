# Esp32 IDF Get Started 

This project will be the famous hello world for esp32 beginners

## Table of Content
- [Setup Linux Environment](#setup-linux-environment)
  - [Prerequisites](#step-1---prerequisites)
  - [Get ESP-IDF](#step-2---get-esp-idf)
  - [Install Tools](#step-3---install-tools)
  - [Configure Environment](#step-4---configure-environment)
  - [Configure Serial Port](#step-5---configure-serial-port)
- [Hello World Project](#hello-world-project)
  - [Configure the project](#step-1---configure-the-project)
  - [Build And flash the project](#step-2---build-and-write-the-project)

## Setup Linux Environment

### Step 1 - Prerequisites

```bash
sudo apt-get install git wget flex bison gperf python3 python3-venv cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
```

### Step 2 - Get ESP-IDF

For linux we need to clone the esp-idf project, it's recommended to clone the idf in some root folder

```
mkdir -p ~/idf
cd ~/idf
git clone --recursive https://github.com/espressif/esp-idf.git
cd esp-idf
```

### Step 3 - Install tools

```
./install.sh esp32
```

### Step 4 - Configure Environment

Open the bash profile, in my case .zshrc, in fill that line

```
alias get_idf='. $HOME/idf/esp-idf/export.sh'
```

If you will use the IDF frequently, maybe It's a better option to configure to load idf each time the bash open

```
. $HOME/idf/esp-idf/export.sh
```

### Step 5 - Configure Serial Port

Develop board comes to USB-to-UART Bridge. The USB-UAT-Bridge need drive to stablish the communication, the newears Linux distributions already came with the drive, but for older version, wee need to install by our selfs. We can just plug our ESP in the USB port and check if everting is right. To do that we can run the following commands:

- Verify if the usb recognize the ESP Development Board:
```
lsusb
```
Should show one dive with the name like this: *Silicon Labs CP210x UART Bridge*

- Verify the serial port
```
dmesg | grep tty
```
Should show something like this: *usb 1-9: cp210x converter now attached to ttyUSB0*

```
dmesg | grep serial
```
Should show something like this: *usbserial: USB Serial support registered for cp210x*

If something went wrong probably you need to install the driver manually. To know which driver wee need to install check the board user guide for specific USB-to-UART bridge chip used.

- [Silicon Labs - CP210x](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers?tab=downloads)
- [FTDI](https://ftdichip.com/drivers/vcp-drivers/)

- Adding the user to dialout group to grant the user the permission to read and writ over usb
```
sudo usermod -a -G dialout $USER
```

- Adding the permission to the usb port. Attention where, you must change the permission for the usb port that your device connect

```
chmod 0777 /dev/ttyUSB0
```

## Hello World project

Inside the esp-idf repository there are a lot of exemplos, we will copy the hello_world exemplo for some folder in our computer

```
cp -r $IDF_PATH/examples/get-started/hello_world SOME_FOLDER/
```

### Step 1 - Configure the project

With everting was installed write, inside the hello_world copied directory, run the following command:

```
idf.py set-target esp32
```

And the next command should show to you a nice terminal menu to configure our build.
```
idf.py menuconfig
```

But for the hello_world project we do not need to change anything.

### Step 2 - Build and write the project

- Build

```
idf.py build
```

- Flash
You should change the -p argument to your own usb port

```
idf.py -p /dev/ttyUSB0 flash
```

- To check if the example is running

```
idf.py -p /dev/ttyUSB0 monitor
```