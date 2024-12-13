[![GitHub issues](https://img.shields.io/github/issues/TheGITofTeo997/BIGTREETECH-SKR-mini-E3)](https://github.com/TheGITofTeo997/BIGTREETECH-SKR-mini-E3/issues)
[![GitHub closed issues](https://badgen.net/github/closed-issues/TheGITofTeo997/BIGTREETECH-SKR-mini-E3?color=green)](https://github.com/TheGITofTeo997/BIGTREETECH-SKR-mini-E3/issues?q=is%3Aissue+is%3Aclosed)
![GitHub repo size](https://img.shields.io/github/repo-size/TheGITofTeo997/BIGTREETECH-SKR-mini-E3)
![GitHub](https://img.shields.io/github/license/TheGITofTeo997/BIGTREETECH-SKR-mini-E3?color=blue)
![GitHub Maintained](https://img.shields.io/badge/Open%20Source-Yes-green)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/TheGITofTeo997/BIGTREETECH-SKR-mini-E3)
![GitHub last commit](https://img.shields.io/github/last-commit/TheGITofTeo997/BIGTREETECH-SKR-mini-E3)
![GitHub Maintained](https://img.shields.io/badge/maintained-yes-green)
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FTheGITofTeo997%2FBIGTREETECH-SKR-mini-E3&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)


# Ender 3 - Marlin 2.1.2.5 - SKR Mini E3 V3.0 + CR-Touch + MK8
This is my personal configuration for the Ender 3 printer running Marlin. Since I've made many hardware upgrades I decided to compile myself Marlin for my needs, starting with this fork. If you have a similar hardware configuration, this firmware *should* suit your needs.

I will show you a detailed list of the upgrades I made (that are worth noting) and how the firmware has been accordingly modified. The list will be kept updated for as long as I can, so will be the firmware.

## Main features
The main feature on this firmware, is that it runs on the silent board **SKR Mini E3 V3** with the probe **CR-Touch** (genuine BL-Touch works as well) inserted into the *5-pin* probe slot, *without* any rewiring (yes, it Z-Homes). For this, and many other tweaks, we have to thank the main author of the fork [Seelenkind](https://github.com/Seelenkind/BIGTREETECH-SKR-mini-E3). Thanks!

This configuration also runs with the **[MK8 24V Premium Hotend](https://www.amazon.it/Aggiornamento-Creality-Premium-Capricorn-stampante/dp/B08B3DRMFN/ref=sr_1_15)**

I've also tried to keep an eye on security (i.e. Z positions) including some tweaks.

## My upgrades
- [MK8 24V Premium Hotend](https://www.amazon.it/Aggiornamento-Creality-Premium-Capricorn-stampante/dp/B08B3DRMFN/ref=sr_1_15):
Some firmware tweaks were needed for the type of thermistor that this hotend uses. This has been bundled with the premium Capricorn tube, which appears to be the best bowden tube out here.
After installing this hotend, **PID** Tuning is required, that's why I have enabled the PID autotune menu with 5 cycles.
```
#define TEMP_SENSOR_0 0
```
- [SUNLU PEI Bed](https://www.amazon.it/gp/product/B08PZCX7H8/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1):
In my opinion, there's nothing better than a PEI bed. I ditched the old glass one in favor of this one and I personally would never go back. No clips and other stuff needed, it's all magnetic.

- [MGN12 X Axis Linear Rail](https://www.amazon.it/gp/product/B08G157G7C/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1):
Some notes on this one: for the Ender 3, I purchased the **300mm** one and I used the provided mounting kit with the **stock** plate. Some rubber pins are included to prevent the rail to slide out. You need to remove the one *near* the X endstop and you want to **keep** the other one so the rail won't slide out.
To prevent the block to hurt this rubber pin, the bed area has been defined as 225x225: this is still wider than the original one (220x220) and prevents grinding and/or hurting parts of the linear rail (remember to tell your **slicer** too!). In my opinion, defining it as 235x235 is useless since we are not able to use the full space for how this configuration is made.
Also, this rail mount shifts a bit forward the Y axis, that's why I increased the probing margins, so that the probe won't probe out of bed (or on the bed clips).

```
// The size of the printable area
#define X_BED_SIZE 225
#define Y_BED_SIZE 225
```

- [Silicone Bed Spacers](https://www.amazon.it/gp/product/B092V92JKS/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1):
Make a new bed tramming cycle after installing these ones, and then do the Z Probe Wizard again.
- [CR-Touch](https://www.amazon.it/gp/product/B097LD78NT/ref=ppx_yo_dt_b_asin_title_o03_s01?ie=UTF8&psc=1):
Installed on the default left side. The Probe offset have been defined accordingly to the hardware changes mentioned above. 
- [X Axis Belt Tender](https://www.amazon.it/gp/product/B08DRHFJ7V/ref=ppx_yo_dt_b_asin_title_o03_s00?ie=UTF8&psc=1):
This specific one has no issues working with the linear rail and with the defined bed size.
- [Y Axis Belt Tender](https://www.amazon.it/gp/product/B08JH9XVF4/ref=ppx_yo_dt_b_asin_title_o02_s00?ie=UTF8&psc=1):
This one had some Y axis grinding issues, because it covered part of the rail. Those has been fixed with the [thing:5167921](https://www.thingiverse.com/thing:5167921), designed by me, also with the defined bed size there won't be any issue.
- [Dual Z Axis Upgrade Kit](https://www.amazon.it/gp/product/B094F2LXS6/ref=ppx_yo_dt_b_asin_image_o00_s01?psc=1):
On this motherboard there are two slots for the Z steppers, ZA and ZB, which are driven by the same driver. In fact, I did not use the Y cable splitter provided in the kit but I built a new one which is identical to the stock one, to have a cleaner setup. The linear rail on the X axis increases its weight, so if you don't do this dual Z upgrade, your axis might fall when the printer is off, an anti-backlash system is also suggested; however with just two axis you should be fine. Since now the driver has to drive two stepper motor instead of one, I increased the Z_CURRENT to a little bit more than the stock, just to make sure the steppers won't skip. This value looked like a safe spot without giving to much heat on the components but still having the steppers working correctly:

`
#define Z_CURRENT 750
`

- [Petsfang Blokhead Fan Duct](https://www.dpetsel.com/because-you-asked.html):
This is an improved prtinable fan duct, you will find instructions on the dedicated website. This has been paired with a [5015 Fan](https://www.amazon.it/gp/product/B079BPS9Q8/ref=ppx_yo_dt_b_asin_title_o03_s01?ie=UTF8&psc=1) and a [Noctua 4010](https://www.amazon.it/gp/product/B009NQLT0M/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1) for the hotend cooling.
Since this fan duct is so massive, to prevent hurting the clips, I defined the *Y_MIN_POS* as **-25**. This also fixes an annoying issue from creality that makes your prints not Y centered. With this fix you should gain some space that was being wasted on the front.
Anyway this is still an in-development thing as I will surely find better margins/values.

## Installation & assembly
To install this firmware you can just put on your SD card the [compiled firmware.bin](https://github.com/TheGITofTeo997/BIGTREETECH-SKR-mini-E3/releases/latest), otherwise if you want to compile it, just drag the configuration files into the Marlin zipped folder provided and then build with [Marlin Auto Build](https://marlinfw.org/docs/basics/auto_build_marlin.html).

| Board photo | Schematic |
| :---: | :---: |
| ![](img/skr3.jpg) | ![](img/skr_schema.jpg) |

For the **fans** you will to crimp a JST connector for the hotend one. Then assembly as follows:
- Part cooling fan in **FAN0**
- Hotend fan in **FAN1** (with a [Buck Converter](https://www.amazon.it/gp/product/B0823P6PW6/ref=ppx_yo_dt_b_asin_title_o02_s01?ie=UTF8&psc=1) if you use a [Noctua 4010](https://www.amazon.it/gp/product/B009NQLT0M/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1))
- Board fan in **FAN2** (with a [Buck Converter](https://www.amazon.it/gp/product/B0823P6PW6/ref=ppx_yo_dt_b_asin_title_o02_s01?ie=UTF8&psc=1) if you use a [Noctua 4010](https://www.amazon.it/gp/product/B009NQLT0M/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1))

They are **not** always on because the firmware controls them, so don't worry if they don't spin up as soon as you power on the system after assembling.

## Values
Since I didn't want to lose all my calibrations at every flash, what I did with this firmware is hard-wiring all my values to be the default ones. If you don't like this practice you are free to revert/modify them as you wish. This is more of a personal note for me to remember what I calibrated and what I did.

```
NOZZLE_TO_PROBE_OFFSET { -47, -2, 0.30 }
A bed tramming cycle is always suggested after this

DEFAULT_Kp  44.40
DEFAULT_Ki  6.10
DEFAULT_Kd  80.7

DEFAULT_bedKp 96.4
DEFAULT_bedKi 17.99
DEFAULT_bedKd 344.3
These are the Bed and Extruder PID Tuning values, maybe not a good idea to default this, so a PID Tuning cycle may still be necessary.

DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 386.9 }
```
I will try to keep this section as updated as I can, to keep trace of my calibrations with this setup.

## Cura Profiles
I use the Ender 3 Cura Profiles from [3D Printscape](https://3dprintscape.com/cura-profiles/) which are pretty reliable and I made no modification to them, except for something to tune such as temperature and flow.
You can find them on the website or in the [Cura Profiles Folder](https://github.com/TheGITofTeo997/BIGTREETECH-SKR-mini-E3/tree/master/Cura%20Profiles)

## Start G-CODE
```
; Ender 3 Custom Start G-code

;Heating Part
M117 Heating Bed...
M140 S60 ; Set Heat Bed temperature
M190 S60 ; Wait for Heat Bed temperature
M117 Pre-heating Hotend...
M104 S160; start warming extruder to 160

;Probing Part
G28 ; Home all axes
M117 Bed-Leveling...
G29 ; Auto bed-level (CR-Touch)
M420 S1 ; Using Probed Bed
M900 K0.3 ; Linear Advance
G92 E0 ; Reset Extruder
M117 Final Heating Hotend...
M104 S200 ; Set Extruder temperature
M109 S200 ; Wait for Extruder temperature
G1 Z5.0 F3000 ; Move z up little to prevent scratching of surface
G1 X0.1 Y20 Z0.3 F5000.0 ; move to start-line position

;Starting Part
M117 Purging...
G1 X0.1 Y200.0 Z0.3 F1500.0 E15 ; Draw 1st line
G1 X0.4 Y200.0 Z0.3 F5000.0 ; Move to side a little
;G1 X0.4 Y20 Z0.3 F1500.0 E30 ; Draw 2nd line (??)
G92 E0 ; Reset extruder
G1 Z3.0 F3000 ; Move z up little to prevent scratching of surface
M117 Printing...
; End of custom start GCode

G92 E0 ;Reset Extruder
G1 Z2.0 F3000 ;Move Z Axis up
G1 X10.1 Y20 Z0.28 F5000.0 ;Move to start position
G1 X10.1 Y200.0 Z0.28 F1500.0 E15 ;Draw the first line
G1 X10.4 Y200.0 Z0.28 F5000.0 ;Move to side a little
G1 X10.4 Y20 Z0.28 F1500.0 E30 ;Draw the second line
G92 E0 ;Reset Extruder
G1 Z2.0 F3000 ;Move Z Axis up
M117 Printing...
```

## End G-CODE
```
G91 ; Relative positionning
G1 E-2 F2700 ; Retract a bit
G1 E-2 Z0.2 F2400 ; Retract and raise Z
G1 X5 Y5 F3000 ; Wipe out
G1 Z10 ; Raise Z more
G90 ; Absolute positionning

G1 X0 Y0 ; Present print
G1 Y200 F9000 ;
M106 S0 ; Turn-off fan
M104 S0 ; Turn-off hotend
M140 S0 ; Turn-off bed

M84 X Y E ; Disable all steppers but Z

M220 S100 ; Reset Feedrate
M221 S100 ; Reset Flowrate
M82 ; Absolute extrusion mode
M104 S0
;End of Gcode
```

## Final notes
Thank you for using this firmware, I will try to keep it up do date with Marlin releases as much as possible. Much time has been spent to make tweaks and adjustments, however there may still be some issues. This is a very specific configuration, so please make sure to use it only if you have the same or a very similar setup.
