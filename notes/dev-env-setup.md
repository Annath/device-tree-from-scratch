# Setting up a development environment

Tools I know I want to use:

1. 1bitsy dev board or similar STM32 dev kit
2. Black Magic Probe debugger
3. Ubuntu 22.04 host
4. `arm-none-eabi-` GNU toolchain with GDB

## ARM Toolchain Issues

Right off the bat, the ARM toolchain I downloaded directly from ARM seems to be broken on later Ubuntu releases. Looks
like it's compiled against an old version of libcursesw and possibly requires some python configuration. I'm abandoning
the ARM pre-built binary approach and trying crosstool-NG.

## Crosstool-NG

ct-ng has required a decent amount of setup but I think it's going to work. I had to add several apt packages and
complete a few unepxected steps in configuring it, but I was finally able to get it to build:

### Install apt-packages

I installed the following apt packages:

```bash
sudo apt install build-essential git ia32-libs wget python3 autoconf gperf \
    bison flex texinfo help2man awk gawk libtool libtool-bin \
    python3-distutils 
```

Some of these are likely left-overs from my attempts to get the pre-built binaries from arm working, so I'm going to
make a clean VM shortly to see which ones I actually need.

### ct-ng fixes

I ran the following commands to get ct-ng working:

```bash
# assuming you already installed all apt packages

git clone https://github.com/crosstool-ng/crosstool-ng
cd crosstool-ng/
git checkout crosstool-ng-1.25.0
./bootstrap && ./configure --enable-local && make -j$(nproc)
./configure --enable-local && make -j$(nproc)
./ct-ng arm-nano-eabi
# ./ct-ng menuconfig to change any settings
# Add GDB support via menuconfig
./ct-ng upgradeconfig # something was goofy with the GDB config and this fixed it
```

After those, I needed to update the config to allow it to download an older version of ZLIB. The following patch shows
my changes:

```
--- .config.backup	2022-11-29 23:10:55.987363105 -0600
+++ .config	2022-11-29 23:11:03.331460322 -0600
@@ -681,7 +681,7 @@
 CT_ZLIB_PATCH_ORDER="global"
 CT_ZLIB_V_1_2_12=y
 CT_ZLIB_VERSION="1.2.12"
-CT_ZLIB_MIRRORS="http://downloads.sourceforge.net/project/libpng/zlib/${CT_ZLIB_VERSION} https://www.zlib.net/"
+CT_ZLIB_MIRRORS="http://downloads.sourceforge.net/project/libpng/zlib/${CT_ZLIB_VERSION} https://www.zlib.net/ https://www.zlib.net/fossils"
 CT_ZLIB_ARCHIVE_FILENAME="@{pkg_name}-@{version}"
 CT_ZLIB_ARCHIVE_DIRNAME="@{pkg_name}-@{version}"
 CT_ZLIB_ARCHIVE_FORMATS=".tar.xz .tar.gz"
```

Finally:

```
./ct-ng build
```

## Building 1bitsy examples