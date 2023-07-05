# Welcome!
This is a repository for running macOS using OpenCore on a Lenovo ThinkPad p51.

> You may have noticed there aren't any files in the repository. That is because **you're gonna do this from scratch**, this repository is just gonna help guide you with the whole process, and get all the device-specific issues out of the way.
---
**Important Note**:
- As of 2023,  `macOS Ventura` is the final supported version of macOS for this model. More information in the FAQ section at the bottom of this page.

----
*Now that all of the blah blah is out of the way, let's get straight to business.*

-----
## 1- Know your hardware.
You and you only are responsible to know your hardware. You and me may share a model number, but that alone does not guarantee equal methods. Please be sure to visit the [Dortania Guide](https://dortania.github.io/OpenCore-Install-Guide/find-hardware.html) to know the exact specs of your machine.

My build contains the following:
|Part|Specific Model|Status|Notes|
|-----|-----------|----|-|
| CPU| i7-7820HQ| ✅ | |
| iGPU| Intel HD 630| ✅ | |
| dGPU| Nvidia Quadro M1200|❌*|Will not work without Nvidia Web Drivers, which only work on `macOS High Sierra`|
| SSD 1| Samsung pm981 512GB|❌|Common OEM Drive. **Refuses to work with macOS.** I recommend a WD or Crucial SSD instead. |
| SSD 2|Crucial 256GB|✅|Works perfectly.
| HDDs| |✅|Not tested, but should work as expected|
| Trackpad|Synaptics PS2 Trackpad|✅|
| Trackpoint|IBM PS2 Trackpoint|✅|
| Keyboard|PS2 Keyboard|✅|
| Wi-Fi|Intel Wireless-N 8265|✅|
| Bluetooth|Intel Wireless-N 8265|✅*|Noticed some issues with connecting to Xbox Wireless Controllers.|
| Ethernet|Intel I219-LM|✅|
| HDMI and DP|NOT FUNCTIONAL.|✅|This specific model routes all HDMI and DP ports through the dGPU, which cannot be used.
| Audio|ALC298|✅|
| Fingerprint|NOT FUNCTIONAL.|✅|There are currently no methods to emulating touch ID on macOS.|
| Webcam| |✅|
| Colour Sensor|NOT FUNCTIONAL|❌|Not tested and most likely not functional.
| SD Card Reader|Realtek SD Card Reader|✅|
| Touch Screen|Not tested.|❓|
| WWAN Card|Not tested.|❓|

- **PLEASE DO NOT USE THIS LIST AS A SPEC SHEET FOR YOUR MACHINE. YOU SHOULD 100% CHECK *YOUR* SPECS**
- If you have any of the optional hardware labeled `Not tested`, consider trying stuff out, and if you get any results, feel free to add it to the "Issues" tab!
- If you have a different CPU (a XEON, for example), please go follow the [Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html#prerequisites) (check under "Configs" in the sidebar.)
---

## 2- Collecting Files.
__
### a- Setting up your USB Drive.
This section differs based on your operating system.

If you have a seperate macOS device, follow "[Making the installer in macOS](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/mac-install.html)"

If you're running on Windows (the majority), follow "[Making the installer in Windows](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/windows-install.html)"

If you're running Linux on your machine, follow "[Making the installer in Linux](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/linux-install.html)"

### b- Adding the base OpenCore files, gathering Kexts, SSDTs and other files and config.plist setup.
Refer to [this page](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/opencore-efi.html)

## Notes about this section of the guide:
- If SSDTTime refuses to work, citing an error relating to a missing `iASL`, you will need to download `iasl-win-NUMBERS.zip` from [this website](https://acpica.org/downloads/binary-tools). Extract all of the contents to `/path/to/SSDTTime/Scripts/`.
*If this link doesn't work, just look for iASL for Windows online.*
---
## 3- Building your config.plist
This one definetly will take a while, so *be patient.*

For this part of the guide, method will differ based on the CPU you have in your PC.

You should know your CPU's **codename**.

For instance, I run an i7-7820HQ, so I have a **Kaby Lake** CPU. This means I'll go to the [Kaby Lake segment of the Intel Laptop config.plist in the Dortania Guide](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/kaby-lake.html).

You should refer to only the page relating to your CPU though. Following a guide for a CPU you do not have will most definitely NOT WORK.

---
If you followed the dortania guide properly, you should have a clean and ready boot drive! This process is full of trial and error, so don't be discouraged that your setup did not work the first time.

---
## 4- Installation
Make sure your USB Drive is PLUGGED IN and NOT EJECTED.

Reboot your machine and press F12 as soon as you see a Lenovo logo.

>### Something useful you can do here:
>Put your USB Drive higher on the Boot priority list. This will remove the need to press F12 and select it every time you reboot. You can do this by:
>1. Make sure your USB Drive is inserted.
>2. Pressing F1 on boot to open the BIOS Setup Utility
>3. Navigating to the `Startup` tab
>4. Selecting Boot
>5. Navigating to your USB Device, and increase its priority by highlighting it and pressing `shift + =` to increase its priority until it is at the top of the list.

Select your USB Drive from the list.

Select your EFI partition from the list.

**COMMON ISSUE:**
> "I don't even get to verbose, the screen just goes blank and does nothing"

>If this happens to you, you need an offline installer. For now, I cannot provide it. You'll have to look for a drive image for a flash drive, EtcherIO for example. If I am able to, I will provide it as soon as possible.

You'll see a bunch of lines (verbose) flying past the screen, then, at last, the macOS recovery environment.

Select disk utility. In the top of the window, Select the button labeled `View` and select `Show All Devices`. Select the Device you want to install macOS on and select the button labeled `Erase`. Your drive should be formatted as `APFS` and have a `GPT` partition scheme. Name it whatever you'd like.

**COMMON ISSUE:**
>"The erase keeps failing for absolutely no reason"

>This might mean you're running out of space on your EFI. Try expanding the size of your EFI partition. You can do this using GParted on linux.

After you're done, close the window.

You should see the menu you saw when you had just entered the recovery environment. Select `Install macOS [version]`. 

Go through the install.

***WARNING:** DO NOT TURN OF YOUR LAPTOP AT ANY POINT DURING THE INSTALL. IT WILL CORRUPT YOUR MACOS INSTALL AND YOU'LL HAVE TO START THIS SECTION OVER.*

After a while, your laptop will reboot to the OpenCore bootpicker, and automatically select a new entry labeled `macOS Installer`. This is normal. Leave it to do its thing.

After a while, you'll be looking at your working macOS install. Congrats!

---
## Now that you're done, follow the [Dortania OpenCore Post-Install Guide](https://dortania.github.io/OpenCore-Post-Install/)

---
# After you're done with that, let's get to our own Post-Install stuff.
## 1. Fixing SD Card.
To do this, all you have to do is add [`RealtekSDCardReader`](https://github.com/0xFireWolf/RealtekCardReader/releases/) and [`RealtekSDCardReaderFriend`](https://github.com/0xFireWolf/RealtekCardReaderFriend/releases) from each of their repositories (linked)

## 2. Fixing Bluetooth
If you're running macOS 12.3 or higher and bluetooth has stopped working, download the latest commit of `BrcmPatchRAM` from [here](https://dortania.github.io/builds/?product=BrcmPatchRAM&viewall=true&version=2.6.8&sha=2305aaa145a0021559f444d33a5adaacb6469050). Copy `BlueToolFixup.kext` to your kexts folder.

### Please do not forget to do an OC Clean Snapshot in ProperTree before attempting a boot.
---
Great job! You should now have a fully functional hackintosh!
If you don't, then you can get some help from others by:
- Checking out the [Hackintosh Subreddit](https://www.reddit.com/r/hackintosh/)
- Asking on the [Hackintosh Discord Server](https://discord.gg/Wxam8aH)

If you have any recommendations for this specific repository, please put them on the **Issues** section of this repository.

---
## Regarding macOS support
`macOS Ventura` is the final version of macOS to support this device, meaning no `macOS Sonoma`, and definetley not any other version of macOS afterwards.

That is unless you decide to use [OpenCore Legacy Patcher](https://dortania.github.io/OpenCore-Legacy-Patcher/). I personally have not tried this method yet, and probably won't but you will be doing this at your own risk. Nobody, and i mean literally nobody, will offer support this. Not me, not the hackintosh subreddit or discord server.

If you're gonna try this, you're on your own.