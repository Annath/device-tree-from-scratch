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

I needed the following apt packages:

```bash
sudo apt install TODO
```

### ct-ng fixes

TODO write me

## Building 1bitsy examples