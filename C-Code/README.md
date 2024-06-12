# midi2usbhost C Code Software
This project depends on the usb\_midi\_host, midi\_uart\_lib, and ring\_buffer\_lib projects. They are included as git
submodules.

This project also uses the the Raspberry Pi Pico SDK and it uses the TinyUSB
library for the USB stack. At the time of this writing, the version of the
TinyUSB library that ships with the pico-sdk version 1.5.1 is not able to use
application USB Host drivers like usb\_midi\_host.  You will need a version of
TinyUSB from 15-Aug-2023 or later that calls `usbh_app_driver_get_cb()` to
install application USB Host drivers. See below for instructions on how to
update the TinyUSB library.

## Setting Up Your Build and Debug Environment
This is where my project deviates from the main project. I built my project on a 
Raspberry PI4 2GB CM using the latest bookworm. 

I have been programming all my life but linux is quite a new platform. 
I found that the linux instructions through up some errors. 
For reference I set the PI user name as Home but otherwise the build is a totally 
standard Raspbian build (bookworm) from June 2024. 

I didn't use Visual Studio Code so have removed those instructions. 
I copied the midi2usbhost.uf2 manually not through the debugging prob. 

I build the standard examples first but then had some issues with when building, so I 
went back and repeated the TinyUSB copy step again, rebooted and somehow through magic 
it worked. 

So here is how you do it through the command line... 

The file structure should look like 

pico + midi2usbhost
     + pico-examples
     + pico-sdk

## Updating the TinyUSB library
The Pico SDK uses the main repository for TinyUSB as a git submodule. Earlier revisions of this project
replaced the TinyUSB library with a forked version. This is no longer necessary. You will need to check
the version of TinyUSB you are using and update it to a more recent version if it is older than 15-Aug-2023.

Assuming your pico-sdk is installed in `${PICO_SDK_PATH}`, then follow these steps to force your version
of TinyUSB to the latest version. The `git remote` command is only required if you previously changed
it per the instructions in older revisions of this project.

```
git remote set-url origin https://github.com/hathach/tinyusb.git
cd ${PICO_SDK_PATH}/lib/tinyusb
git checkout master
git pull
```

## Get the project code
Clone the midiusb2host project to a directory at the same level as the pico-sdk directory.

```
cd ${PICO_SDK_PATH}/..
git clone --recurse-submodules https://github.com/rppicomidi/midi2usbhost.git
```
## Command Line Build (skip if you want to use Visual Studio Code)

Enter this series of commands (assumes you installed the pico-sdk
and the midid2usbhost project in the $HOME/foo directory)

```
export PICO_SDK_PATH=$HOME/pico/pico-sdk/
cd $HOME/pico/midi2usbhost/
mkdir build
cd build
cmake ..
make
```
The build should complete with no errors. The build output is in the build directory you created in the steps above.


