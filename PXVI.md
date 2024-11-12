# Running Linux on the Eluktronics Prometheus XVI

This is my personal notes about bringing up Linux on an Eluktronics Prometheus XVI laptop.

## Table of Contents
* [Prep work](#prep-work)
* [Observed behaviours](#observed-behaviours)
* [Observed Nits](#observed-nits)
* [Things I want to do](#things-i-want-to-do)
   * [Getting LEDs working](#getting-leds-working)
* [Things I criticize](#things-i-criticize)
* [Misc](#misc)
   * [General Info](#general-info)
   * [Hardware Info](#hardware-info)

## Prep work

* Used an NVMe-to-USB adapter to prep a 2tb drive with Pop_OS 21.10
    * Issue encountered: SDDM refused to pop up on the "right" monitor, likely showing up on the HDMI out - this is a laptop with a hardware MUX but without MUX functionality active at this point, autoselect was wonked.
        * Fix: revert to GDM3 for the time being. Need to futz with pam shenanigans to autounlock the keychain properly, thanks for nothing GNOME eugh.

## Observed behaviours

* CPU runs at max out of the box, essentially.
    * Issue likely due to "ootb optimisation" setting up the NVMe in a VM. playing with power packages seems to have "jigged" system76-power into working. Call it "software percussive maintenance"
    * Temps are surprisingly nice, powerscaling goes as low as 400mhz
* With 5.15, out of the box sensors are fine
* Screen backlight works as designed!
* Keyboard shortcuts are fine
* Touchegg works OOTB!
* ~~No sound! The Renoir/Césanne sound out isn't recognized!~~
    * My dumb person and Pipewire. This was caused by missing parts in my Pipewire forced-activation setup missing a few packages for the pulseaudio fake-out to work. Once that was done, just worked!
    * Further proof of this: a full "reinstall" (made to optimize partition setup and data arrangement, because I'm dumb) had sound working before shenanigans with audio
    * Further further proof of this: with additional package pulls in my "bootstrap script", Pipewire worked fine
* Trackpad is fine
* Bluetooth just works, I paired a Dualshock 4 without effort and could play Hades with it.
* OpenRGB has no idea about anything here.
* VRR is crap on Linux, panel refresh rate is being a butt made of butt butts.

## Observed Nits

* Boot options
   * The Aptio/AMI setup kinda sucks a bit because I can only see "Boot Type Order" in the UEFIBIOS menu, with no hints that "Boot Type Order" is not the same as "Boot Selection" 
      * Put another way, I can't see multiple boot entries from the EFIBIOS, only the boot media types.
* SDDM/GDM oddities
   * Unrelated to Eluktronics. This is a screen selection hiccup, I'm assuming, where SDDM will select the nVidia screen if the computer is running in nVidia mode and not Hybrid, whereas GDM3 selects the internal DisplayPort panel just fine. Dropping to console works, so this is wholely on SDDM.
* 45-degree shenanigans
   * If holding the laptop at a 45-degree angle (like the front of it on a surface, the back "up", resulting in a 45 degree angle), the touchpad feels like it's hovering off its surface; putting the laptop back down on an even surface gets things back to normal.
   * This is probably within expected tolerances.
* Mainboard Mislabeling
   * The NVMe slots were marked 1 and 2, I put my Samsung in 1 and intel in 2. The UEFIBIOS had Intel in 1 and Samsung in 2.
      * Known "issue". It's actually a running joke at this point: https://twitter.com/BobTrades/status/1457821763413422084

## Things I want to do

### Getting LEDs working
* Need to reach out to Eluktronics about how to poke the right things to get LED control
    * Hi Bob, if you're reading this, the only way I'm seeing to poke the LEDs right now is two SMBus devices (this thing: https://www.i2c-bus.org/smbus/), but as I don't know how to poke them (and am steadfast in my refusal to run Windows, damn it!), I may need some wee help if you happen to have any information.
    * Devices: 
      ```
      i2c-1   smbus           SMBus PIIX4 adapter port 0 at ff00      SMBus adapter
      i2c-2   smbus           SMBus PIIX4 adapter port 2 at ff00      SMBus adapter
      ```
* Barring that, give up, install dumb windows in a VM, device-assign the SMBus i2cs into the VM and see if the driver signaling can be intercepted?

## Things I criticize

* Honestly just the UEFIBIOS configuration being barren. The Prometheus XVII's instruction manual shows more menus and options, the XVI looks really oversimplified in comparison. Maybe that's a choice? I dunno.

## Misc

### General Info

At time of writing, I'm installing Pop! on this computer, specifically the 21.10 version. Other specific details:
* Custom partition schema with a boot partition separate from /
  * This is to work around casefold format issues - Grub, rEFInd and gummiboot can't/won't access a casefold ext4 partition yet.
* Manually did /recovery but configured it at install in the partition setup, and it actually set the recovery partition up. Imagine my surprise.

### Hardware Info

#### lspci
```
00:00.0 Host bridge: Advanced Micro Devices, Inc. [AMD] Renoir/Cezanne Root Complex
00:00.2 IOMMU: Advanced Micro Devices, Inc. [AMD] Renoir/Cezanne IOMMU
00:01.0 Host bridge: Advanced Micro Devices, Inc. [AMD] Renoir PCIe Dummy Host Bridge
00:01.1 PCI bridge: Advanced Micro Devices, Inc. [AMD] Renoir PCIe GPP Bridge
00:02.0 Host bridge: Advanced Micro Devices, Inc. [AMD] Renoir PCIe Dummy Host Bridge
00:02.1 PCI bridge: Advanced Micro Devices, Inc. [AMD] Renoir/Cezanne PCIe GPP Bridge
00:02.2 PCI bridge: Advanced Micro Devices, Inc. [AMD] Renoir/Cezanne PCIe GPP Bridge
00:02.3 PCI bridge: Advanced Micro Devices, Inc. [AMD] Renoir/Cezanne PCIe GPP Bridge
00:02.4 PCI bridge: Advanced Micro Devices, Inc. [AMD] Renoir/Cezanne PCIe GPP Bridge
00:08.0 Host bridge: Advanced Micro Devices, Inc. [AMD] Renoir PCIe Dummy Host Bridge
00:08.1 PCI bridge: Advanced Micro Devices, Inc. [AMD] Renoir Internal PCIe GPP Bridge to Bus
00:14.0 SMBus: Advanced Micro Devices, Inc. [AMD] FCH SMBus Controller (rev 51)
00:14.3 ISA bridge: Advanced Micro Devices, Inc. [AMD] FCH LPC Bridge (rev 51)
00:18.0 Host bridge: Advanced Micro Devices, Inc. [AMD] Device 166a
00:18.1 Host bridge: Advanced Micro Devices, Inc. [AMD] Device 166b
00:18.2 Host bridge: Advanced Micro Devices, Inc. [AMD] Device 166c
00:18.3 Host bridge: Advanced Micro Devices, Inc. [AMD] Device 166d
00:18.4 Host bridge: Advanced Micro Devices, Inc. [AMD] Device 166e
00:18.5 Host bridge: Advanced Micro Devices, Inc. [AMD] Device 166f
00:18.6 Host bridge: Advanced Micro Devices, Inc. [AMD] Device 1670
00:18.7 Host bridge: Advanced Micro Devices, Inc. [AMD] Device 1671
01:00.0 VGA compatible controller: NVIDIA Corporation GA104M [GeForce RTX 3080 Mobile / Max-Q 8GB/16GB] (rev a1)
01:00.1 Audio device: NVIDIA Corporation GA104 High Definition Audio Controller (rev a1)
02:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8125 2.5GbE Controller (rev 05)
03:00.0 Network controller: Intel Corporation Wi-Fi 6 AX200 (rev 1a)
04:00.0 Non-Volatile memory controller: Intel Corporation SSD 660P Series (rev 03)
05:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller SM981/PM981/PM983
06:00.0 VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] Cezanne (rev c4)
06:00.1 Audio device: Advanced Micro Devices, Inc. [AMD/ATI] Renoir Radeon High Definition Audio Controller
06:00.2 Encryption controller: Advanced Micro Devices, Inc. [AMD] Family 17h (Models 10h-1fh) Platform Security Processor
06:00.3 USB controller: Advanced Micro Devices, Inc. [AMD] Renoir/Cezanne USB 3.1
06:00.4 USB controller: Advanced Micro Devices, Inc. [AMD] Renoir/Cezanne USB 3.1
06:00.5 Multimedia controller: Advanced Micro Devices, Inc. [AMD] Raven/Raven2/FireFlight/Renoir Audio Processor (rev 01)
06:00.6 Audio device: Advanced Micro Devices, Inc. [AMD] Family 17h (Models 10h-1fh) HD Audio Controller
```

#### lsusb

```
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 002: ID 0408:2094 Quanta Computer, Inc. USB webcam
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 002: ID 8087:0029 Intel Corp. AX200 Bluetooth
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

#### lspcmcia

Nothing. This is 2021 you numpties.

#### Interesting bits

* This is pretty much a Quanta Computer design. Nothing bad about it, just good to know where it comes from.
* Eluktronics did populate the Serial Number fields with the right values!
* The "version" stuffs say Type1Version and Type2Version, wonder what's up with that
* The BIOS software is recent as hell.
* Variable Refresh Rate on the panel is weird and I'm "locked" to 60hz somehow. Not Eluktronics' fault.
