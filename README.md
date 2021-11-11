# Personal notes on running Linux on the Eluktronics Prometheus XVI

## Table of Contents
* [Prep work](#prep-work)
* [Observed behaviours](#observed-behaviours)
* [Observed Nits](#observed-nits)
* [Things I want to do](#things-i-want-to-do)

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
* OpenRGB has no idea about anything here.

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

* Reach out to Eluktronics about how to poke the right things to get LED control
    * Hi Bob, if you're reading this, the only way I'm seeing to poke the LEDs right now is two SMBus devices (this thing: https://www.i2c-bus.org/smbus/), but as I don't know how to poke them (and am steadfast in my refusal to run Windows, damn it!), I may need some wee help if you happen to have any information.
    * Devices: 
      ```
      i2c-1   smbus           SMBus PIIX4 adapter port 0 at ff00      SMBus adapter
      i2c-2   smbus           SMBus PIIX4 adapter port 2 at ff00      SMBus adapter
      ```
* Barring that, give up, install dumb windows in a VM, device-assign the SMBus i2cs into the VM and see if the driver signaling can be intercepted?
