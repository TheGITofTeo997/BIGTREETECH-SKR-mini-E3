# Ender 3 - Marlin 2.0.9.3 - SKR Mini E3 V3.0 + CR-Touch
This is my personal configuration for the Ender 3 printer running Marlin. Since I've made many hardware upgrades I decided to compile myself Marlin for my needs, starting with this fork. If you have a similar hardware configuration, this firmware *should* suit your needs.

I will show you a detailed list of the upgrades I made (that are worth noting) and how the firmware has been accordingly modified. The list will be kept updated for as long as I can, so will be the firmware.

## Main features
The main feature on this firmware, is that it runs on the silent board SKR Mini E3 V3 with the probe CR-Touch (genuine BL-Touch works aswell) inserted into the *5-pin* probe slot, *without* any rewiring (yes, it Z-Homes). For this, and many other tweaks, we have to thank the main author of the fork [Seelenkind](https://github.com/Seelenkind/BIGTREETECH-SKR-mini-E3). Thanks!

I've also tried to keep an eye on security (i.e. Z positions) including some tweaks.

## My upgrades
- [Ender Metal Bed Clips](https://www.amazon.it/gp/product/B08PZKGJTR/ref=ppx_yo_dt_b_asin_title_o03_s00?psc=1):
These are smaller than included ones, so we can use a wider printable area. Two are mounted on the front, and two on the back
- [MGN12 X Axis Linear Rail](https://www.amazon.it/gp/product/B08G157G7C/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1):
Some notes on this one: for the Ender 3, I purchased the **300mm** one and I used the provided mounting kit with the **stock** plate and duct. Some rubber pins are included to prevent the rail to slide out. You need to remove the one *near* the X endstop and we want to **keep** the other one so the rail won't slide out. 
To prevent the block to hurt this rubber pin, the bed area has been defined as 225x225: this is still wider than the original one (220x220) and prevents grinding and/or hurting parts of the linear rail (remember to tell your **slicer** too!). In my opinion, defining it as 235x235 is useless since we are not able to use the full space.
Also, this rail mount shifts a bit forward the Y axis, that's why I increased the **FRONT** probing margin, so that the probe wont't probe out of bed (or on the bed clips).

``` 
// The size of the printable area
#define X_BED_SIZE 225
#define Y_BED_SIZE 225 
```

- [Silicone Bed Spacers](https://www.amazon.it/gp/product/B092V92JKS/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1):
Make a new bed tramming cycle after installing these ones, and then do the Z Probe Wizard again.
- [CR-Touch](https://www.amazon.it/gp/product/B097LD78NT/ref=ppx_yo_dt_b_asin_title_o03_s01?ie=UTF8&psc=1):
Installed on the default left side. The Probe offset have been defined accordingly to the hardware changes mentioned above. I left the Z Probe to a known 0, so before you start printing it is **required** to run the Z Probe Wizard (bed tramming is **suggested** before this).

`
#define NOZZLE_TO_PROBE_OFFSET { -48, -8.8, 0 }
`

If you are too lazy to do the Z probe wizard, I will share my Z offset with this setup, which is **-0,76ish**. However it is **strongly** not advised to use the same offset as me without having done the wizard before.

- [X Axis Belt Tender](https://www.amazon.it/gp/product/B08DRHFJ7V/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1):
This specific one has no issues working with the linear rail and with the defined bed size.
- [Y Axis Belt Tender](https://www.amazon.it/gp/product/B08JH9XVF4/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1):
This one had some Y axis grinding issues, because it covered part of the rail. Those has been fixed with the [thing:5167921](https://www.thingiverse.com/thing:5167921), designed by me, also with the defined bed size there won't be any issue.
- [Dual Z Axis Upgrade Kit](https://www.amazon.it/gp/product/B094F2LXS6/ref=ppx_yo_dt_b_asin_image_o00_s01?psc=1):
On this motherboard there are two slots for the Z steppers, ZA and ZB, which are driven by the same driver. In fact, I did not use the Y cable splitter provided in the kit but I built a new one which is identical to the stock one, to have a cleaner setup. Since now the driver has to drive two stepper motor instead of one, I increased the Z_CURRENT to a little bit more than the stock, just to make sure the steppers won't skip:

`
#define Z_CURRENT 750
`

This value looked like a safe spot without giving to much heat on the components but still having the steppers working correctly.

## Installation & assembly
To install this firmware you can just put on your SD card the [compiled firmware.bin](https://github.com/TheGITofTeo997/BIGTREETECH-SKR-mini-E3/releases/latest), otherwise if you want to compile it, just drag the configuration files into the Marlin zipped folder provided and then build with [Marlin Auto Build](https://marlinfw.org/docs/basics/auto_build_marlin.html).

<img src="https://github.com/TheGITofTeo997/BIGTREETECH-SKR-mini-E3/blob/6128b8d25788506257fd31cc91030b20c0038248/skr3.jpg" width="400" height="600">

For the **fans** you will to crimp a JST connector for the hotend one. Then assembly as follows:
- Part cooling fan in **FAN0**
- Hotend fan in **FAN1**
- Board fan in **FAN2**

They are **not** always on because the firmware controlles them, so don't worry if they don't spin up as soon as you power on the system after assembling.

## Final notes
Thank you for using this firmware, I will try to keep it up do date with Marlin releases as much as possible. Many time has been spent to make tweaks and adjustments, however there may still be some issues. This is a very specific configuration, so please make sure to use it only if you have the same or a very similar setup.

