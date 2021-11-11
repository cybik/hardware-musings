# Personal notes on running Linux on the Eluktronics Prometheus XVI

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
