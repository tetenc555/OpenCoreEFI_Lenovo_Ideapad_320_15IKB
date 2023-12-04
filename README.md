# OpenCore EFI for the Lenovo Ideapad 320 (15IKB)

## Disclaimer
This EFI was built with my own laptop, and targeting at making all of its components functional. Please be aware that the same model of laptop can have differents components and im not responsible for any damage or data lost in the process of installing macOS. 

## My Hardware and What's working 
| Component  | Behavior |
| :-------------: | :-------------: |
| Intel Core i5-7200U  | Fully working with normal and turbo clock speeds  |
| SSD on main SATA + HDD on disk caddy  | Fully working  |
| Intel HD 620 | Fully working with external display support and 3GB VRAM  |
| Trackpad (Synaptics)[^1]  | Fully working with gestures and proper Force-Touch emulation  |
| Intel AC3160 Wi-Fi + Bluetooth Module | Fully working without AirDrop and Continuity Features |
| Apple BCM94360CS2 Wi-Fi + Bluetooth Module[^2] | Fully working with AirDrop and Continuity Features[^3]  |
| Realtek Audio (ALC230) | Fully working with proper in and out |
| Realtek Ethernet | Fully working |
| USB 3.0 and Type-C | Fully working |
| Fn Lenovo combos + battery conservation mode | Fully working (install YogaSMC prefpane to use them) |

[^1]:An common issue with this build is that some models came with an ELAN trackpad. You probably just need an different kext, as some people tried this EFI and it worked fine with these patches.
[^2]:I changed the wifi combo and i recommend doing this if you want AirDrop and Continuity Features. You can use this by following the tutorial on the Wi-Fi configuration section
[^3]:Althought there is an fix developed by OCLP, it requires some patches and the SIP to be disabled. These patches are not in this EFI and this WiFi will work natively on every version since the older macOS this laptop can handle until Ventura. Follow the instructions on the Wi-Fi configuration section to have this working.

## What's not working
1. If you're using the Intel Wi-FI:
    - Most continuity features will not work. Some like handoff can work but they arent that reliable. Last version i tested this combo was on version 2.0.0 so im not really sure if major changes had been made (this combo is realiable for normal wifi/bluethooth activies tho!!)
2. Hardware based DRM doesnt work. This isnt fixable ;(( , so you cant watch Netflix on safari. Just use an chromium based browser and the issue should be fixed as they have an software DRM decoder
3. Power button doesn't make the power off popup, but using Ctrl+Insert make it show. Not really sure how to fix it and wasnt that bothered by it so its probably staying this way...

## Before installing
Go to your bios and make this changes, **otherwise the system will not boot**:
* Disable Secure Boot
* Switch from Intel RST Premium to AHCI  

## When you finish installing...
Go to your EFI and change the Serial Number and info under PlatformInfo section. Im packing an public Serial Number so this is for security reasons... You can use [this](https://github.com/corpnewt/GenSMBIOS) to help changing it!! Im currently using the MacBookAir9,1 SMBIOS  
Also, go to your EFI and under Misc change the LauncherOption to Short. This will keep your EFI safier from any damage that can be caused by other systems or even the bios itself <333  
And, by the end, if you want full security (in Apple Security standarts), set an ApECID. Follow this [tutorial](https://dortania.github.io/OpenCore-Post-Install/universal/security/applesecureboot.html#special-notes-with-securebootmodel) to do it!!

## Wifi Configuration
This EFI comes with Intel Kexts correctly configured for macOS Sonoma and Monterey. Follow these tutorials if you are using the Intel card on an older macOS or if you are using the Apple Wi-Fi Card on Sonoma.

### Intel Wi-Fi on older macOS
This process is very simple, just disable BluetoolFixUp and enable IntelBluetoothInjector in Kernel section and it should work fine! :3

### Apple Native Wi-Fi on macOS Sonoma
This process is a bit complicated but at the end your OTA Updates should still be working, even thought SIP will be disabled.
1. Disable all Intel Kexts including BlueToolFixUp
2. Enable IOSkyFamily, the two IO80211 kexts and the AMFIPass kext
3. Enable IOSkyFamily in the Exclude section
4. Add the boot arg into your nvram: -amfipassbeta
5. Change your csr-active-config to 03080000 in the nvram section
6. Restart and reset your nvram
7. Install OpenCore Legacy Patcher and run the Post-Install Root Patches
Now your wifi and bluetooth should be working with all Continuity Features!! <33

## Misc

### Running systems older then Big Sur
If you need to run systems older then Big Sur, you will need to get the 1.0.0 release, as it is configured to use the MacBook Pro 2017 SMBIOS. It will run from 10.12.5 all the way to 13.2 (latest tested, probably it will run all Ventura versions) with no problems. The SMBIOS just had to be updated to an MacBook Air 2020 cause apple isnt natively supporting 2017 Macs anymore </3

### Disabling boot sound
There is an way to disable it via the EFI but you can just go to the settings app and disable it under Audio Preferences!!

### Disabling boot picker
Use OpenCoreAuxiliaryTools, mount your EFI and open your config.plist. You should be able to disable the option ShowPicker under the Misc section!!

### Bootcamp and Windows Dualboot status
You can easily install Windows and use Bootcamp to switch between systems without using the picker. Keep in mind you will need an flash drive with the Opencore EFI on it to boot onto Mac after installing Windows, just so you can restore the EFI. Just don't delete the Microsoft Folder and copy the boot folder from the usb. It should work just like an real Mac after this! 
OBS: Remember to activate LauncherOption into the short option so the EFI dont get damaged by Windows updates!!
