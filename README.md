rtl8192du
=========

Source code for RTL8192DU device, forked from https://github.com/lwfinger/rtl8192du.git

Install Instructions:
# For Fedora
    sudo dnf install -y dkms git gcc gcc-c++ kernel-headers kernel-devel make 
# Get the source and build the driver
    git clone https://github.com/lwfinger/rtl8192du.git
    cd rtl8192du
    make
    sudo make install
    sudo modprobe 8192du
    

# Aimed for Sunxi 8i H3
I have a H3 board, so need to compile for it. Do what is list below for the cross compiling.

- Change Makefile
- Change os_dep/osdep_service.c


## Makefile
```
// 1. add platform choice
CONFIG_PLATFORM_ARM_SUN8I = y

// 2. modify the KRC and CROSS_COMPILE accordingly 
// between
ifeq ($(CONFIG_PLATFORM_ARM_SUN8I), y)
ARCH=arm 
CROSS_COMPILE := 
KSRC :=
endif


```

## osdep_service.c
```
osdep_service.c:69:28: error: ‘const struct file_operations’ has no member named ‘read_iter’

A: It is just because Linux 3.4 won't support in inlcude/linux/fs.h; So remove it in osdep_service.c where read_iter is used and what is related.

```

## Compile

```
$ make
$ file 8192du.ko
8192du.ko: ELF 32-bit LSB relocatable, ARM, EABI5 version 1 (SYSV), BuildID[sha1]=7060805f4ef75ae046bdab899455446212bc21ba, with debug_info, not stripped

```