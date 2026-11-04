# Introduction
Modified fastboot sources to unlock vivo phones

# How to use

Lock vivo bootloader   :  fastboot bbk lock_vivo 

Unlock vivo bootloader :  fastboot bbk unlock_vivo

This binary can be compiled using any toolchain (on linux only) or using the official method by downloading whole AOSP source.
If you want to compile for windows, you probably have to use the whole AOSP source.

# How to compile

## Details
Suggested toolchain : https://web.archive.org/web/20200718202000/http://downloads.yoctoproject.org/releases/yocto/yocto-1.5/toolchain/

Depending on your host machine select either x86_64 or i686 on the page, and then choose the file appropriate for your target hardware. If you are on a 64bit computer then download poky-eglibc-x86_64-core-image-sato-x86_64-toolchain-1.5.sh, since we want to run  fastboot on an x86_64 architecture later on.

Assuming it was downloaded into Downloads, execute the script

   chmod a+x ~/Downloads/poky-eglibc-x86_64-core-image-sato-x86_64-toolchain-1.5.sh ~/Downloads/poky-eglibc-x86_64-core-image-sato-x86_64-toolchain-1.5.sh

You will be asked where you want to install the toolchain (default location is /opt/poky).

Now clone the following aosp sources using git from android.googlesource.com (android-4.4_r**)

platform/system/core, platform/system/extras, /platform/external/libselinux, /platform/external/zlib, /platform/external/openssl. 

Now navigate to platform/system/core/ directory 

type git clone https://github.com/Naveen3Singh/vivo_fastboot.git fastboot

Now the vivo_fastboot source will be downloaded in fastboot directory with a custom makefile for our toolchain.

Now type make within fastboot directory to compile the fastboot binary.

if you get compile error, it's most likely due to toolchain, edit makefile to point to your toolchain.
If you want to add support for more commands, just search "vivo_bsp" in fastboot.c, & replace it with your desired commmand.

## TL;DR

~~~sh
git clone --depth=1 -b android-4.4_r1.2 https://android.googlesource.com/platform/system/core/ platform/system/core
git clone --depth=1 -b android-4.4_r1.2 https://android.googlesource.com/platform/system/extras/ platform/system/extras
git clone --depth=1 -b android-4.4_r1.2 https://android.googlesource.com/platform/external/libselinux platform/external/libselinux
git clone --depth=1 -b android-4.4_r1.2 https://android.googlesource.com/platform/external/zlib platform/external/zlib
git clone --depth=1 -b android-4.4_r1.2 https://android.googlesource.com/platform/external/openssl platform/external/openssl
cd platform/system/core
git clone https://github.com/brianpow/vivo_fastboot
cd vivo_fastboot
sed -i 's/gettid(/selinux_gettid(/g' ../../../external/libselinux/src/*
sed -i '35i #include "output_file.h"' ../../core/libsparse/sparse_read.c
make TOOLCHAIN= CFLAGS='-std=gnu99'
~~~

# Known issues
## Phones *NOT* working

vivo V21 5G (V2050)
vivo Y91c
vivo U10
vivo Y20s
vivo Y15 (2019)
vivo Y51L
vivo X20
vivo Y81i (Model 1812)
vivo Y81
vivo Z3
vivo X9
vivo iQOO 855 neo
