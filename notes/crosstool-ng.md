## Install


1. Install app packages:
	```bash
	sudo apt install build-essential gperf flex bison texinfo help2man gawk \
		libtool-bin libncurses-dev
	```
1. Download crosstool-ng 1.26.0:
	```bash
	wget "http://crosstool-ng.org/download/crosstool-ng/crosstool-ng-1.26.0.tar.bz2"
	tar -xf crosstool-ng-1.26.0.tar.bz2
	cd crosstool-ng-1.26.0
	```
1. Build ct-ng:  
	```bash
	./configure
	make
	sudo make install
	```

## Creating the toolchain

