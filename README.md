# Ender 3 - Marlin 2.0.9.3 - SKR Mini E3 V3.0 + CR-Touch
This is my personal configuration for the Ender 3 printer running Marlin. Since I've made many hardware upgrades I decided to compile myself Marlin for my needs, starting with this fork. If you have a similar hardware configuration, this firmware *should* suit your needs.

I will show you a detailed list of the upgrades I made (that are worth noting) and how the firmware has been accordingly modified. The list will be ketp updated for as long as I can, so will be the firmware.

# Main features
The main feature on this firmware, is that it runs on the silent board SKR Mini E3 V3 with the probe CR-Touch (genuine BL-Touch works aswell) inserted into the *5-pin* probe slot, *without* any rewiring (yes, it Z-Homes). For this, and many other tweaks, we have to thank the main author of the fork [Seelekind](https://github.com/Seelenkind/BIGTREETECH-SKR-mini-E3). Thanks!

I've also tried to keep an eye on security (i.e. Z positions) including some tweaks.

# My upgrades
- [Ender Metal Bed Clips](https://www.amazon.it/gp/product/B08PZKGJTR/ref=ppx_yo_dt_b_asin_title_o03_s00?psc=1) 
These are smaller than included ones, so we can use a wider printable area. Two are mounted on the front, and two on the back
- [MGN12 X Axis Linear Rail](https://www.amazon.it/gp/product/B08G157G7C/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1)
Some notes on this one: for the Ender 3, I purchased the **300mm** one and I used the provided mounting kit with the **stock** plate and duct. Some rubber pins are included to prevent the rail to slide out. You need to remove the one *near* the X endstop and we want to **keep** the other one so the rail won't slide out. 
To prevent the block to hurt this rubber pin, the bed area has been defined as 225x225: this is still wider than the original one (220x220) and prevents grinding and/or hurting parts of the linear rail (remember to tell your **slicer** too!). In my opinion, defining it as 235x235 is useless since we are not able to use the full space.
Also, this rail mount shifts a bit forward the Y axis, that's why I increased the **FRONT** probing margin, so that the probe wont't probe out of bed (or on the bed clips).

``` 
// The size of the printable area
#define X_BED_SIZE 225
#define Y_BED_SIZE 225 
```

- [Silicone Bed Spacers](https://www.amazon.it/gp/product/B092V92JKS/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1)
Make a new bed tramming cycle after installing these ones, and then do the Z Probe Wizard again.
- [CR-Touch](https://www.amazon.it/gp/product/B097LD78NT/ref=ppx_yo_dt_b_asin_title_o03_s01?ie=UTF8&psc=1)
Installed on the default left side. The Probe offset have been defined accordingly to the hardware changes mentioned above. I left the Z Probe to a known 0, so before you start printing it is **required** to run the Z Probe Wizard (bed tramming is **suggested** before this).

`
#define NOZZLE_TO_PROBE_OFFSET { -48, -8.8, 0 }
`

If you are too lazy to do the Z probe wizard, I will share my Z offset with this setup, which is **-0,89**. However it is **strongly** not advised to use the same offset as me without having done the wizard before.

- [X Axis Belt Tender](https://www.amazon.it/gp/product/B08DRHFJ7V/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1)
This specific one has no issues working with the linear rail and with the defined bed size
- [Y Axis Belt Tender](https://www.amazon.it/gp/product/B08JH9XVF4/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1)
This one had some Y axis grinding issues, because it covered part of the rail. Those has been fixed with the [thing:5167921](https://www.thingiverse.com/thing:5167921), designed by me, also with the defined bed size there won't be any issue.

# Final notes
To install this firmware you can just put on your SD card the compiled firmware.bin, otherwise if you want to compile it, just drag the configuration files into the Marlin zipped folder provided and then build with Marlin Auto Build.

Thank you for using this firmware, I will try to keep it up do date with Marlin releases as much as possible. Many time has been spent to make tweaks and adjustments, however there may still be some issues. This is a very specific configuration, so please make sure to use it only if you have the same or a very similar setup.

