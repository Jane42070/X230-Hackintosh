# MacOS on ThinkPad X230

<img align="right" src="/Other/Pics/X230.png" alt="Lenovo Thinkpad X230 macOS Hackintosh OpenCore" width="300">

[![ThinkPad](https://img.shields.io/badge/ThinkPad-X230-blue.svg)](https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_X230.pdf)
[![MacOS Catalina](https://img.shields.io/badge/Catalina-10.15-red.svg)](https://www.apple.com/)
[![MacOS Big Sur](https://img.shields.io/badge/Big_Sur-11.4-blue.svg)](https://www.apple.com/macos/big-sur/)
[![MacOS Monterey](https://img.shields.io/badge/Monterey-12.0-purple.svg)](https://www.apple.com/macos/monterey-preview/)

## READ THE ENTIRE README.MD BEFORE YOU START

### I am not responsible for any damages you may cause

- Complete EFI packs are available in the releases page.
- I will try my best to keep the repo updated with the latest kexts and OpenCore version.
- Please **do not clone or download** the main branch for daily use: it may include **unstable code** just because it is my repository.
- This EFI is configured with Catalina and Big Sur. (Not completed for Monterey).
- With every EFI update you retrieve from here please remember to go through the post install guide.

#### Status : WIP

<details>
<summary><strong> SUMMARY </strong></summary>
<br>

> ### Non-Fuctional

| Feature                              | Status | Dependency          |
| :----------------------------------- | ------ | ------------------- |
| Fingerprint Reader                   | ❌   | `DISABLED` in BIOS to save power. |
| Wireless WAN                         | ❌   | `DISABLED` in BIOS to save power. |
| VGA Port                             | ❌   | Does not exist on real apple computers. |

> ### Video and Audio

| Feature                              | Status | Dependency          |
| :----------------------------------- | ------ | ------------------- |
| Full Graphics Accleration (QE/CI)    | ✅   | `WhateverGreen.kext`  |
| Audio Recording                      | ✅   | `AppleALC.kext` with Layout ID = 55 and `SSDT-HPET.aml`   |
| Audio Playback                       | ✅   | `AppleALC.kext` with Layout ID = 55 and `SSDT-HPET.aml`   |
| Automatic Headphone Output Switching | ✅   | `AppleALC.kext` with Layout ID = 55 and `SSDT-HPET.aml`   |
| Dock Audio Port                      | ✅   | `AppleALC.kext` with Layout ID = 55 and `SSDT-HPET.aml`   |

> ### Power, Charge, Sleep and Hibernation

| Feature                              | Status | Dependency          |
| :----------------------------------- | ------ | ------------------- |
| Battery Percentage Indication        | ✅   | `ECEnabler.kext`            | 
| iGPU Power Management                | ✅   | `XCPM`, enabled by [`SSDT-PM.aml`](https://github.com/Piker-Alpha/ssdtPRGen.sh) |
| S3 Sleep/ Hibernation Mode 3         | ✅   | `SSDT-PWTK.aml` |  |   
| Custom Charge Threshold              | ✅   | `SSDT-EC.aml`, [YogaSMC.kext](https://github.com/zhen-zen/YogaSMC), and [YogaSMCPane](https://github.com/zhen-zen/YogaSMC)|
| Fan Control                          | ✅   | `SSDT-EC.aml`, [YogaSMC.kext](https://github.com/zhen-zen/YogaSMC), and [YogaSMCPane](https://github.com/zhen-zen/YogaSMC)|
| Battery Life                         | ✅   | Native, comparable to Windows/Linux. |

> ### Input/ Output

| Feature                              | Status | Dependency          |
| :----------------------------------- | ------ | ------------------- |
| WiFi                                 | ✅   | `AirportItlwm.kext`  |
| Bluetooth                            | ✅   | `IntelBluetoothFirmware.kext`  |
| Ethernet                             | ✅   | `IntelMausi.kext`  |
| USB 2.0, USB 3.0                     | ✅   | `USBPorts.kext`    |
| USB Power Properties in macOS        | ✅   | `SSDT-EC-USBX.aml` |

> ### Display, TrackPad, TrackPoint, and Keyboard

| Feature                              | Status | Dependency          |
| :----------------------------------- | ------ | ------------------- |
| Brightness Adjustments | ✅  | `WhateverGreen.kext`, `SSDT-PNLF.aml` and `BrightnessKeys.kext`|
| TrackPoint             | ✅  | `VoodooPS2Controller.kext` |
| TrackPad               | ✅  | `VoodooPS2Controller.kext` |
| Built-in Keyboard      | ✅  | `VoodooPS2Controller.kext` |
| Multimedia Keys        | ✅  | `BrightnessKeys.kext` and [YogaSMC](https://github.com/zhen-zen/YogaSMC) |

> ### macOS Continuity

| Feature                              | Status | Dependency          |
| :----------------------------------- | ------ | ------------------- |
| iCloud, iMessage, FaceTime           | ✅   | Whitelisted Apple ID, Valid SMBIOS  |
| AirDrop                              | ✅   | Not tested  |
| Time Machine                         | ✅   | Native  |

</details>

<details>
<summary><strong> REQUIREMENTS </strong></summary>
<br>

- A macOS machine(optional): to create the macOS installer.
- Flash drive, 12GB or more, for the above purpose.  
- Xcode works fine for editing plist files on macOS, but I prefer [PlistEdit Pro](https://www.fatcatsoftware.com/plisteditpro/).  
- [ProperTree](https://github.com/corpnewt/ProperTree) if you need to edit plist files on Windows.  
- [MaciASL](https://github.com/acidanthera/MaciASL), for patching ACPI tables and editing ACPI patches.
- [MountEFI](https://github.com/corpnewt/MountEFI) to quickly mount EFI partitions.  
- [IORegistryExplorer](https://developer.apple.com/downloads), for diagnosis.  
- [Hackintool](https://www.insanelymac.com/forum/topic/335018-hackintool-v286/), for diagnostic ONLY, Hackintool should not be used for patching, it is outdated.
- Patience and time, especially if this is your first time Hackintosh-ing.

</details>

<details>
<summary><strong> HARDWARE </strong></summary>
<br>

| Category  | THINKPAD X230            | THINKPAD T530            | THINKPAD T430s            |
| --------- | ------------------------ | ------------------------ | ------------------------- |
| CPU       | Intel Core i5-3380M      | Intel Core i5-3320M      | Intel Core i7-3520M       |
| SSD       | Kingston 480GB           | SanDisk A400 256GB       | Samsung 870 Evo 256GB     |
| Display   | 12.5' TN HD (1366x1768)  | 15.6' TN FHD (1920x1080) | 14' TN HD+ (1600x900)     |
| WiFi & BT | Intel Wireless-AC 6205   | DW1550 (BCM94352HMB)     | Intel Centrino N-6205     |

- Refer to [X230-Platform_Specifications](https://psref.lenovo.com/syspool/Sys/PDF/withdrawnbook/ThinkPad_X230.pdf) for possible stock ThinkPad X230 configurations.

</details>

<details>
<summary><strong> GETTING STARTED </strong></summary>
<br>

Before you do anything, please familiarize yourself with basic Hackintosh terminologies and the basic Hackintosh process by throughly reading Dortania guides as linked in `REFERENCES`

- Creating a macOS installer: refer to [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/)
- [**README-HARDWARE**](/Other/README_HARDWARE.md): Requirements before installing.
- [**README-OTHERS**](/Other/README_OTHERS.md): for post installation settings and other remarks.

</details>

<details>
<summary><strong> BENCHMARKS </strong></summary>
</br>

- macOS 12.7.5, EFI OpenCore 0.9.9

| CPU            | Single-Core | Multi-Core |
| :------------- | ----------: | ---------: |
| Cinebench R23  |             |       1576 |
| Geekbench 5    |         732 |       1529 |

| GPU            | OpenCL      | Metal      |
| :------------- | ----------: | ---------: |
| Geekbench 5    |        1028 |        193 |

</details>
