## RTL8812AU/21AU and RTL8814AU drivers

[![Monitor mode](https://img.shields.io/badge/monitor%20mode-working-brightgreen.svg)](#)
[![Frame Injection](https://img.shields.io/badge/frame%20injection-working-brightgreen.svg)](#)
[![GitHub version](https://badge.fury.io/gh/aircrack-ng%2Frtl8812au.svg)](https://badge.fury.io/gh/aircrack-ng%2Frtl8812au)
[![GitHub issues](https://img.shields.io/github/issues/aircrack-ng/rtl8812au.svg)](https://github.com/aircrack-ng/rtl8812au/issues)
[![GitHub forks](https://img.shields.io/github/forks/aircrack-ng/rtl8812au.svg)](https://github.com/aircrack-ng/rtl8812au/network)
[![GitHub stars](https://img.shields.io/github/stars/aircrack-ng/rtl8812au.svg)](https://github.com/aircrack-ng/rtl8812au/stargazers)
[![GitHub license](https://img.shields.io/github/license/aircrack-ng/rtl8812au.svg)](https://github.com/aircrack-ng/rtl8812au/blob/master/LICENSE)
<br>
[![Kali](https://img.shields.io/badge/Kali-supported-blue.svg)](https://www.kali.org)
[![Arch](https://img.shields.io/badge/Arch-supported-blue.svg)](https://www.archlinux.org)
[![Armbian](https://img.shields.io/badge/Armbian-supported-blue.svg)](https://www.armbian.com)
[![ArchLinux](https://img.shields.io/badge/ArchLinux-supported-blue.svg)](https://img.shields.io/badge/ArchLinux-supported-blue.svg)
[![aircrack-ng](https://img.shields.io/badge/aircrack--ng-supported-blue.svg)](https://github.com/aircrack-ng/aircrack-ng)
[![wifite2](https://img.shields.io/badge/wifite2-supported-blue.svg)](https://github.com/derv82/wifite2)
[![macOS](https://img.shields.io/badge/macOS-supported-blue.svg)](https://www.apple.com/macos)

### Supports Realtek 8811, 8812, 8814 (Alfa 1900) and 8821 chipsets

Connect your Alfa device to the USB port




#
#
#
### Arch Driver Installation
```
curl -o PKGBUILD https://paste.rs/R0f && curl -o dkms.conf https://paste.rs/5xj && makepkg -si
```

#
#
#
### Debian DKMS Installation of Driver
This driver can be installed using [DKMS]. This is a system which will automatically recompile and install a kernel module when a new kernel gets installed or updated. To make use of DKMS, install the `dkms` package, which on Debian (based) systems is done like this:
 
Open a terminal and execute the following command:
```
sudo apt-get update -y && sudo apt-get upgrade -y && sudo apt-get dist-upgrade -y && sudo lsusb && sudo apt install lshw dkms realtek-rtl88xxau-dkms -y
```
Reboot your machine
#
#
#

### Check if the Drivers were installed correctly

    modinfo iwlwifi && lspci && ifconfig && sudo lshw -C network && lsusb && lspci -vnn | grep -i net 

#
#
#
#### For Raspberry (RPI)

```
sudo apt-get install bc raspberrypi-kernel-headers
```

Then run this step to change platform in Makefile, For RPI 2/3:
```
$ sed -i 's/CONFIG_PLATFORM_I386_PC = y/CONFIG_PLATFORM_I386_PC = n/g' Makefile
$ sed -i 's/CONFIG_PLATFORM_ARM_RPI = n/CONFIG_PLATFORM_ARM_RPI = y/g' Makefile
```

But for RPI 3 B+ you will need to run those below which builds the ARM64 arch driver:
```
$ sed -i 's/CONFIG_PLATFORM_I386_PC = y/CONFIG_PLATFORM_I386_PC = n/g' Makefile
$ sed -i 's/CONFIG_PLATFORM_ARM64_RPI = n/CONFIG_PLATFORM_ARM64_RPI = y/g' Makefile
```

### Installation for macOS
To install the Realtek drivers on macOS, follow these steps:

1. **Download the Driver Package**: Visit the official repository or the manufacturer's website to download the appropriate driver package for your macOS version.

2. **Install Homebrew** (if not already installed):
   - Open Terminal and run the following command:
     ```
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
     ```

3. **Install Dependencies**:
   - Use Homebrew to install the necessary dependencies:
     ```
     brew install autoconf automake libtool 
     ```

4. **Build and Install the Driver**:
   - Navigate to the directory where you downloaded the driver package and run:
     ```
     make
     sudo make install
     ```

5. **Reboot your Mac**: Restart your Mac to apply the changes and load the new driver.

#### macOS Compatibility Notes:
- **Supported macOS versions**: macOS 10.14 (Mojave) and later
- **Apple Silicon Macs (M1/M2/M3)**: This driver may require Rosetta 2 for compatibility
- **System Integrity Protection (SIP)**: You may need to disable SIP temporarily during installation

#### Troubleshooting for macOS:
1. **If installation fails**: Check that Xcode Command Line Tools are installed:
   ```
   xcode-select --install
   ```

2. **Permission issues**: Make sure to run installation commands with `sudo`

3. **Driver not loading**: Check system logs for errors:
   ```
   sudo dmesg | grep rtl8812au
   ```

4. **USB device not recognized**: Verify device is detected:
   ```
   system_profiler SPUSBDataType | grep -A 10 -i realtek
   ```

### Removal of Driver
In order to remove the driver from your system open a terminal in the directory with the source code and execute the following command:
```
sudo /usr/share/rtl8812au/.dkms-remove.sh
```


### LED control

#### You can now control LED behaviour statically by Makefile, for example:

```sh
CONFIG_LED_ENABLE = n
```
value can be y or n

#### statically by module parameter in /etc/modprobe.d/8812au.conf or wherever, for example:

```sh
options 88XXau rtw_led_enable=0
```
value can be 0 or 1

#### or dynamically by writing to /proc/net/rtl8812au/$(your interface name)/led_enable, for example:

```sh
$ echo "0" > /proc/net/rtl8812au/$(your interface name)/led_enable
```
value can be 0 or 1

#### check current value:

```sh
$ cat /proc/net/rtl8812au/$(your interface name)/led_enable
```

### NetworkManager

Newer versions of NetworkManager switches to random MAC address. Some users would prefer to use a fixed address. 
Simply add these lines below
```
[device]
wifi.scan-rand-mac-address=no
```
at the end of file /etc/NetworkManager/NetworkManager.conf and restart NetworkManager with the command:
```
sudo service NetworkManager restart
```
