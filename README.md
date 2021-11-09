# Personal notes on running Linux on the Eluktronics Prometheus XVI

## Prep work

* Used an NVMe-to-USB adapter to prep a 2tb drive with Pop_OS 21.10
    * Issue encountered: SDDM refused to pop up on the "right" monitor, likely showing up on the HDMI out - this is a laptop with a hardware MUX but without MUX functionality active at this point, autoselect was wonked.
        * Fix: revert to GDM3 for the time being. Need to futz with pam shenanigans to autounlock the keychain properly, thanks for nothing GNOME eugh.

## Observed behaviours

* CPU runs at max out of the box, essentially.
* With 5.15, out of the box sensors are fine
* Screen backlight works as designed!
* No sound! The Renoir/Césanne sound out isn't recognized!
* Trackpad is fine