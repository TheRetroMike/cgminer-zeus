# cgminer 4.8.0-scrypt (zeus / gridseed / lketc support) #

CGMiner 4.8.0 with GridSeed, Zeus and Lketc with Scrypt ASIC support.

This file describes Lketc-specific settings and options.

For general CGMiner information refer to doc/README.

##Quick RPI Installation
```
curl -o- -k https://raw.githubusercontent.com/TheRetroMike/cgminer-zeus/main/rpi-installer.sh | bash
```

### LKETC usb miner ###
This code is forked from original cgminer-dmaxl-zeus.

I made a custom driver for LKETC usb miner that you can find on ebay, like this:

![](https://raw.githubusercontent.com/wareck/cgminer-lketc/master/docs/lketc.jpg)


My code is base on Zeus scrypt Asic, but I made some changes to enable possibility to use Zeus and LKETC as same time (with tuning for each kind of miner)

Why a separate driver instead of using zeus driver ?

because lketc stick , they are clone of zeus but with cheaper component.

In most case, they need a lower frequency to run properly. If you have a mix of zeus and lketc in your rig, it will interresting to separate zeus and lketc setup. 

to build this specific code:

	sudo apt-get update
	sudo apt-get install build-essential autoconf automake libtool pkg-config libcurl4-openssl-dev libudev-dev \
	libjansson-dev libncurses5-dev git libzip-dev
	
	git clone https://github.com/wareck/cgminer-lketc.git
	cd cgminer-lketc
	
	sudo usermod -a -G dialout,plugdev $USER
	sudo cp 01-cgminer.rules /etc/udev/rules.d/
	
	CFLAGS="-O2 -march=native" ./autogen.sh
	./configure --enable-scrypt
	make
	sudo reboot

### Option Summary ###

```
  --lketc-clock <clock>   Default chip clock speed (MHz)
  --lketc-options <ID>,<chips>,<clock>[;<ID>,<chips>,<clock>...]
                         Set chips and clock speed for individual devices

  --lketc-nocheck-golden  Skip golden nonce verification during initialization (serial mode only)
  --lketc-debug           Enable extra Lketc driver debugging output in verbose mode
```

The following three examples are equivalent assuming two miners are connected:

	# Using libusb
	./cgminer --scrypt --lketc-clock 280
	
	# Direct serial I/O, manual port specification
	./cgminer --scrypt --lketc-clock 280 --scan-serial /dev/ttyUSB0 \
		--scan-serial /dev/ttyUSB1 --scan-serial /dev/ttyUSB2
	
	# Direct serial I/O, auto-detect ports (Linux only)
	./cgminer --scrypt --lketc-clock 280 --scan-serial lketc:auto

Exemple If you use Lketc and a Gaw Fury :

	./cgminer --scrypt --lketc-clock 280 --zeus-chips 6 --zeus-clock 328

![](https://raw.githubusercontent.com/wareck/cgminer-lketc/master/docs/mining.png)

