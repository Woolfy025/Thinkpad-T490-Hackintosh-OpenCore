# Lenovo ThinkPad T490 Hackintosh OpenCore
[![OpenCore](https://img.shields.io/badge/OpenCore-0.9.7-cyan.svg)](https://github.com/acidanthera/OpenCorePkg/releases/latest) [![Clover Version](https://img.shields.io/badge/Clover-r5155-apple.svg)](https://github.com/CloverHackyColor/CloverBootloader/releases/latest) [![macOS Sonoma](https://img.shields.io/badge/macOS-14.2b-white.svg)](https://www.apple.com/macos/sonoma/) [![release](https://github.com/5T33Z0/Thinkpad-T490-Hackintosh-OpenCore/releases/latest)<br>![10053604](https://github.com/5T33Z0/Thinkpad-T490-Hackintosh-OpenCore/assets/76865553/ed932a1a-8205-4b81-a4e2-f68d7d8a7178)

**TABLE of CONTENTS**

- [About](#about)
  - [Notable Features](#notable-features)
  - [Future Developments](#future-developments)
- [Issues](#issues)
- [Specs](#specs)
- [BIOS Settings](#bios-settings)
- [EFI Folder Content](#efi-folder-content)
- [Preparations](#preparations)
  - [Config adjustments](#config-adjustments)
    - [AirportItlwm.kext vs. itlwm.kext](#airportitlwmkext-vs-itlwmkext)
- [Deployment](#deployment)
  - [If macOS is installed already](#if-macos-is-installed-already)
  - [If macOS is not installed](#if-macos-is-not-installed)
- [Post-Install](#post-install)
- [For OCAT Users](#for-ocat-users)
- [Credits and Thank Yous](#credits-and-thank-yous)

## About
OpenCore EFI folder and config for running macOS Monterey and newer on the Lenovo ThinkPad T490. Read the documentation carefully in order to boot macOS successfully.

| :warning: Important Updates |
|:----------------------------|
| Uninstall Intel Power Gadget before upgrading to macOS Sonoma – use the Uninstaller included in the app's folder. The `EnergyDriver.kext` that comes with Intel Power Gadget causes all CPU cores to run at 100% in macOS Sonoma 14.2 beta 3!

### Notable Features
- Compatible with macOS Sonoma
- MicroSD Card Reader is working
- Clamshell mode working (when connected to A/C and external display)
- Optimized Framebuffer Patch for smoother handshake with external display
- Lean EFI folder with slimmed kexts (20 mb instead of 70) :
	- **AppleALC** (87 kB instead of 2.2 mb). Only contains layout `97`.
	- **AirportItlwm** (1.7 mb instead of 16 mb). Only Contains Firmware for Intel AC 9560.
	- **itlwm** (1.5 mb instead of 16 mb). Only Contains Firmware for Intel AC 9560.
- YogaSMC support for additional features like CPU fan control, performance bias, all <kbd>Fn</kbd> Key shortcuts working, additional OSD overlays, etc.
- No injection of `PlatformInfo` data into Windows.
- 3D globe view in Maps (macOS 12+)

### Future Developments
- Mapping USB Ports via ACPI instead of using USBMap.kext
- Creating AppleALC Layout for Docking Station

## Issues
- Audio Jack creates an unpleasant buzz/noise during driver initialization. So it's best to connect Headphones to it *after* booting.

> [!IMPORTANT]
> 
> Before reporting any issues, ensure that your system uses the latest UEFI and EC Firmware as I do. I have no time trying to fix issues which are not caused by my EFI but rather by running the system on outdated firmware! 

## Specs
Category | Description
:-------:|------------
**Model** | Lenovo ThinkPad T490 
**Variant** | [**20N3**](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-t-series-laptops/thinkpad-t490-type-20n2-20n3/document-userguide)
**BIOS** | **UEFI**: v1.80 (2023-06-21) <br> **Embedded Controller**: v1.27
**CPU** | Intel [**Intel Core i5 8265U**](https://ark.intel.com/content/www/us/en/ark/products/149088/intel-core-i58265u-processor-6m-cache-up-to-3-90-ghz.html) (Quad Core)
**RAM** | 16 GB: <ul> <li> 8 GB Samsung DDR 4 @2666 Mhz (soldered) <li> 8 GB Samsung DDR 4 @2666 Mhz (RAM Slot)
**Storage** | ~~Samsung PM981 NVMe~~ ([**unusable**](https://dortania.github.io/Anti-Hackintosh-Buyers-Guide/Storage.html)) <br> 250 GB M.2 Crucial MX 500 SATA SSD
**Display** | Full HD (1080p) (Non-Touch)
**iGPU** | Intel(R) Grpahics UHD 620 (spoofed as Iris 655, BusID: `2`)
**dGPU** | None
**Audio** | [**Realtek ALC257**](https://github.com/dreamwhite/ChonkyAppleALC-Build/blob/master/Realtek/ALC257.md) (using Layout `97`)
**Thunderbolt** | Titan Ridge Thunderbolt 3 Connector (USB-C)<br> (Reported working but I don't have any gear to test it)
**Ethernet** | Intel I219-V
**WiFi** | Intel AC-9560 <br> **Firmware**: [**`iwm-9000-46`**](https://www.intel.com/content/www/us/en/support/articles/000005511/wireless.html) ([Screenshot](https://github.com/5T33Z0/Thinkpad-T490-Hackintosh-OpenCore/blob/main/Additional_Files/Pics/wifi-firmware.png))
**Bluetooth** |**Device**: Intel Wireless Bluetooth <br> **VID**: `0x8087`, **PID**: `0x0aaa` <br> **Firmware**: `ibt-17-16-1.sfi`, `ibt17-16-1.ddc` <br>**USB Port**: `HS10`
**Trackpad** | Synaptics <br>**Device-id**: `pci8086,9de8`. Controlled via SMBus.
**SD Card Reader** | Realtek MicroSD Card Reader
**Dock** | [**ThinkPad Ultra Docking Station**](https://support.lenovo.com/us/en/solutions/pd500173-thinkpad-ultra-docking-station-overview-and-service-parts)

## BIOS Settings
After powering on the machine, spam <kbd>F1</kbd> until you hear a beep to enter the BIOS. Change the following settings:

Category | Setting
:-------:|------------
**Config** | **Display** <ul> <li>Shared Display Priority: `HDMI` <li> Total Graphics Memory: `256 MB` </ul> **CPU** <ul> <li> Intel Hyperthreading Technology: `ON`
**Security** | **Fingerprint** <ul><li>Predesktop Authentication: `OFF` </ul>**Security Chip** <ul><li>Security Chip`ON` or `OFF` (enable for Windows 11) </ul> **Memory Protection** <ul> <li> Execution Prevention: `ON`</ul></ul> **Virtualization** <ul><li> Kernel DMA Protection: `ON` (enables `VT-D` by design)</ul> **I/O Port Access** <ul> <li> Ethernet LAN: `ON` <li> Wireless LAN: `ON` <li> Bluetooth: `ON` <li> USB Port: `ON` <li> Memory Card Slot: `ON` <li> Smart Card Slot: `OFF` <li> Integrated Camera: `ON` <li> Integrated Audio: `ON` <li> Microphone: `ON` <li> Fingerprint Reader: `OFF` <li> Thunderbolt 3: `ON` </ul> **Absolute Persistance Module** <ul><li> Absolute Persistance Module Activation: `Disabled`</ul> **Secure Boot Configuration** <ul><li> Secure Boot: `OFF` </ul> **Intel SGX** <ul><li> Intel SGX Control: `Disabled`
**Startup** | <ul> <li> **UEFI/ Legacy Boot**: `UEFI Only` <li> **Boot Mode**: `Quick` (Skips Diagnostics)

## EFI Folder Content

<details>
<summary><strong>Click to reveal</strong></summary>

```
EFI
├── BOOT
│   └── BOOTx64.efi
├── OC
│   ├── ACPI
│   │   ├── DMAR.aml
│   │   ├── SSDT-ALS0.aml
│   │   ├── SSDT-AWAC.aml
│   │   ├── SSDT-ECRW.aml
│   │   ├── SSDT-EXT1-FixShutdown.aml
│   │   ├── SSDT-EXT3-LedReset-TP.aml
│   │   ├── SSDT-EXT4-WakeScreen.aml
│   │   ├── SSDT-GPRW.aml
│   │   ├── SSDT-MCHC.aml
│   │   ├── SSDT-PLUG.aml
│   │   ├── SSDT-PNLF.aml
│   │   ├── SSDT-PTSWAK.aml
│   │   ├── SSDT-THINK.aml
│   │   └── SSDT-USBX.aml
│   ├── Drivers
│   │   ├── AudioDxe.efi
│   │   ├── HfsPlus.efi
│   │   ├── OpenCanopy.efi
│   │   ├── OpenRuntime.efi
│   │   └── ResetNvramEntry.efi
│   ├── Kexts (Loading managed by MinKernel/MaxKernel settings)
│   │   ├── AMFIPass.kext
│   │   ├── AdvancedMap.kext
│   │   ├── AirportItlwm_Monterey.kext
│   │   ├── AirportItlwm_Sonoma.kext
│   │   ├── AirportItlwm_Ventura.kext
│   │   ├── AppleALC.kext
│   │   ├── BlueToolFixup.kext
│   │   ├── BrightnessKeys.kext
│   │   ├── CPUFriend.kext
│   │   ├── CPUFriendDataProvider.kext
│   │   ├── ECEnabler.kext
│   │   ├── IntelBTPatcher.kext
│   │   ├── IntelBluetoothFirmware.kext
│   │   ├── IntelBluetoothInjector.kext
│   │   ├── IntelMausiEthernet.kext
│   │   ├── itlwm.kext (disabled)
│   │   ├── Lilu.kext
│   │   ├── NVMeFix.kext
│   │   ├── RealtekCardReader.kext
│   │   ├── RealtekCardReaderFriend.kext
│   │   ├── RestrictEvents.kext
│   │   ├── SMCBatteryManager.kext
│   │   ├── SMCProcessor.kext
│   │   ├── SMCSuperIO.kext
│   │   ├── USBMap_MBP152.kext
│   │   ├── VirtualSMC.kext
│   │   ├── VoodooPS2Controller.kext
│   │   │   └── Contents
│   │   │       └── PlugIns
│   │   │           ├── VoodooInput.kext (disabled)
│   │   │           ├── VoodooPS2Keyboard.kext
│   │   │           ├── VoodooPS2Mouse.kext (disabled)
│   │   │           └── VoodooPS2Trackpad.kext
│   │   ├── VoodooRMI.kext
│   │   │       └── PlugIns
│   │   │           ├── RMII2C.kext (disabled)
│   │   │           ├── RMISMBus.kext
│   │   │           └── VoodooInput.kext
│   │   ├── VoodooSMBus.kext
│   │   ├── WhateverGreen.kext
│   │   └── YogaSMC.kext
│   ├── OpenCore.efi
│   ├── Resources
│   │   ├── Audio
│   │   │   └── OCEFIAudio_VoiceOver_Boot.mp3
│   │   ├── Font
│   │   │   ├── Font_1x.bin
│   │   │   ├── Font_1x.png
│   │   │   ├── Font_2x.bin
│   │   │   └── Font_2x.png
│   │   ├── Image
│   │   │   ├── Acidanthera (removed icons from tree view)
│   │   │   │   ├── Chardonnay
│   │   │   │   ├── GoldenGate
│   │   │   │   └── Syrah 
│   │   │   └── Blackosx
│   │   │       └── BsxM1 (removed icons from tree view)
│   │   └── Label (removed files from tree view)
│   └── Config.plist
└── OC Changelog.md
```
</details>

## Preparations

### Config Adjustments
- Download the [**latest Release**](https://github.com/5T33Z0/Thinkpad-T490-Hackintosh-OpenCore/releases/latest) of my EFI folder and unzip it
- Open the config.plist with a plist editor (e.g. ProperTree or OCAT) and adjust the following settings based on the used version of macOS and personal preferences:
	- **Graphics**
		- `Devices/Properties/Add/PciRoot(0x0)/Pci(0x2,0x0)`: For macOS 13.3 and older, disable/delete `enable-backlight-registers-alternative-fix` and use `enable-backlight-registers-fix` instead to fix backlight issues.
		- If other issues occur, try a different [Framebuffer Patch ](https://github.com/5T33Z0/Thinkpad-T490-Hackintosh-OpenCore/blob/main/Additional_Files/Framebuffer_Patches/UHD620_Framebuffer_Patches.plist)
	- **Wi-Fi**: Decide, which Wi-Fi kext you want to use (&rarr; see [**AirportItlwm vs itlwm**](#airportitlwmkext-vs-itlwmkext))
	- `Platforminfo/Generic`: Generate an MLB, Serial and ROM for `MacBookPro15,2` with GenSMBIOS or OCAT. :warning: Don't change the SMBIOS or the `USBMap.kext` won't work anymore!
	- Add `boot-args` for debugging if you have installation issues: `-v`, `debug=0x100` and `keepsyms=1`
	- `UEFI/APFS` section: change `MinVersion` and `MinDate` to `-1` if you want to use macOS Catalina or older.
- Save the changes

> [!IMPORTANT]
> 
> If your T490 model has a different WiFi/BT card than Intel AC-9560, use the official itlwm.kext because mine only contains the firmware for the 9560 so it won't work with other cards.

#### AirportItlwm.kext vs. itlwm.kext
Although the Intel AC-9560 Card is compatible with both kexts (use either one or the other), there are Pros and Cons to both of them (check the [**FAQs**](https://openintelwireless.github.io/itlwm/FAQ.html#features) for other differences):

- **AirportItlwm**: (default)
	- **Pro**: Can be used during macOS installation which is not possible with `itlwm.kext`
	- **Pro**: Supports Location Services and Find My Mac
	- **Con**: Doesn't perform as well as itlwm.kext and takes a bit longer to connect to access points
	- **Con**: Can't connect to hidden WiFi Networks
	- **Con**: Requires using the correct kext per macOS version, so running multiple version of macOS requires multiple versions of this kext controlled via `MinKernel` and `MaxKernel` settings
	- ~~**Con**: Not compatible with macOS Sonoma [**yet**](https://github.com/OpenIntelWireless/itlwm/issues/883)~~

- **itlwm.kext** (disabled)
	- **Pro**: Connects much faster to WiFi hotspots and loading times feel slightly better than with `AirportItlwm` 
	- **Pro**: Supports macOS Sonoma already
	- **Pro**: Can connect to hidden WiFi Networks
	- **Pro**: Only one kext to cover WiFi across multiple versions of macOS
	- **Con**: Requires [**HeliPort**](https://github.com/OpenIntelWireless/HeliPort) app to connect to WiFi Hotspots so it can't be used during macOS installation
	- **Con**: Doesn't support Location Services

> [!NOTE]
> 
> My config uses `AirportItlw.kext` by default since it allows accessing the internet during macOS installation (unlike `itlwm.kext` which requires an additional app to do so). Currently, AirportItlwm kexts for macOS Monterey, Ventura and Sonoma are included. My `itlwm.kext` is a slimmed-down version only containing the firmware for the Intel AC-9560 (1,5 MB instead of 16,1 MB). If you want to use itlwm, disable AirportItlwm (all variants) and enable itlwm in the config.plist instead. Next, download the Helipad app, run it and add it to "Login Items" (in System Settings) so that it starts automatically with macOS.

#### Enabling Hibernation
- In config.plist:
	- Change `HibernateMode` to `Auto`
- On the System:
	- Disable Power Nap (in System Settings)
	- In Terminal: `sudo pmset -a hibernatemode 25` 
	- In Terminal: `sudo pmset -a standby 1` 

## Deployment
### If macOS is installed already
- Put the EFI folder on a FAT32 formatted USB flash drive
- Reboot from said USB flash drive for testing
- If it works, mount your system's ESP, replace the BOOT and OC folders in the EFI folder
- Continue with Post-Install 

### If macOS is not installed
- Follow Dortania's [**OpenCore Install Guide**](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/#making-the-installer) to prepare a USB Installer
- Once the USB  has been created, download the latest version of [**HeliPort**](https://github.com/OpenIntelWireless/HeliPort) and copy the .dmg to your USB Installer (only required when `itlwm.kext` is used for Wi-Fi)
- Next, mount the ESP of the USB Installer (you can use [**MountEFI**](https://github.com/corpnewt/MountEFI) for this)
- Put the EFI folder on the EFI partition of the USB installer
- Reboot from the USB installer 
- Install macOS
- Once that is completed, continue with Post-Install

## Post-Install
- **Disable Gatekeeper**: `sudo spctl --master-disable` because it is annoying and wants to stop you from running scripts from github etc.
- **Wi-Fi** (`itlwm.kext` users only): 
	- Mount **HeliPort.dmg**, drag the app into the "Programs" folder and run it.
	- Use it to connect to your WiFI Hotspot.
	- Add HeliPort to "Login Items", so it stars with macOS and connects to your WiFi Hotspot automatically.
- **YogaSMC**:
	- Download [**YogaSMC-App**](https://github.com/zhen-zen/YogaSMC/releases) and mount it
	- Double-click the YogaSMC **prefPane** to install it
	- Drag the `YogaSMC` app into the "Programs" folder and run it
	- Click on the incon (⌥) in the menu bar and select "Start at Login"
	- Now you can control performance profiles, fan speed and other settings
- Use [**CPUFriendFriend**](https://github.com/corpnewt/CPUFriendFriend) to generate your own `CPUFriendDataProvider.kext` to optimize CPU Power Management if your T490 uses a different CPU than mine.

## Understanding YogaSMC Settings

- Enable the checkbox `PSC` Support in the YogaSMC preference pane. I think it refers to `Power State Current` in the `DSDT` and is used to control different levels of performance via the slider.
- `DYTC` controls the DYTC Performance mode. 3 profiles are available: "Quiet", "Balanced", and "Performance"

> [!NOTE]
>
> I've disabled YogaSMC in my latest EFI. Because after some time, CPU Power Management starts acting really weird. I was watching a YT video and suddenly the video playback stopped, but audio continued and the system felt super sluggish. The read-outs in Intel Powwer Gadget reflected this: the "Core Max" and "Core Avg" values were really low, while "Core Req" was through the roof. The performance slider doesn't seem to do anything either and after rebooting, it's resets again. I have to digg into it.

## For OCAT Users
Add the following entries to the "Kext URL Upgrade" list accessible via "Settings" from the "Sync" window (if not present already), so kext which are marked in grey in the Sync window will be downloaded when checking for updates:

Kext Name | Source URL
----------|-----------
**AdvancedMap.kext** | https://github.com/notjosh/AdvancedMap
**IntelBluetoothFirmware.ketx** | https://github.com/OpenIntelWireless/IntelBluetoothFirmware
**IntelMausiEthernet.kext** | https://github.com/CloverHackyColor/IntelMausiEthernet
**itlwm.kext** | https://github.com/OpenIntelWireless/itlwm 
**RealtekCardReader.kext** | https://github.com/0xFireWolf/RealtekCardReader
**RealtekCardReaderFriend.kext** | https://github.com/0xFireWolf/RealtekCardReaderFriend

> [!IMPORTANT]
> 
> Don't update `AppleALC.kext` and `itlwm.kext` via OCAT because then you lose the slimmed versions of these kexts!

## Credits and Thank Yous
- [**Acidanthera**](https://github.com/acidanthera) for OpenCore, Kexts and maciASL
- [**CorpNewt**](https://github.com/corpnewt) for ProperTree, CPUFriendFriend and SSDTTime
- Dreamwhite for slimmed versions of [**itlwm.kext**](https://github.com/dreamwhite/Chonky-itlwm-Build/releases)
- [**ic005k**](https://github.com/ic005k/OCAuxiliaryTools) for OpenCore Auxiliary Tools
- [**benbaker76**](https://github.com/benbaker76/Hackintool) for Hackintool
- [**zxystd**](https://github.com/zxystd/BrcmPatchRAM) for Sonoma-compatible BrcmPatchRAM kext
- **Special Thx to**:
	- [1Revenger1](https://github.com/1Revenger1/) for VoodooRMI and fixing issues with the TrackPad
	-  deeveedee for advice when trying to optimize the framebuffer patch for connecting to my external display. 
- **T490 OpenCore Repos** used for referencing and ACPI hotfixes:
	- [yusifsalam](https://github.com/yusifsalam/t490-macos)
	- [Krissh-C ](https://github.com/Krissh-C/T490-macOS)
	- [ganyuanzhen](https://github.com/ganyuanzhen/T490-Hackintosh-Opencore)
	- [laserdyke](https://github.com/laserdyke/t490-opencore)
	- [ZoR3oL](https://github.com/ZoR3oL/t490-hackintosh)
