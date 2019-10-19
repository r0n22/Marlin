<<<<<<< HEAD
# (Un-Official) Sunhokey 2015 Prusa i3 Marlin 3D Printer Firmware
<img align="right" src="Documentation/Logo/Marlin%20Logo%20GitHub.png" />

 Documentation has moved to [marlinfirmware.org](http://www.marlinfirmware.org).

## About this firmware

Important: This code is intended to follow latest stable branch from official Marlin
firmware, feel free to fork, but be aware I might force push this branch to keep the
Sunhokey patches on top of latest stable Marlin firmware.

 This branch has changes for targeting [Sunhokey 2015 Prusa i3](https://www.3dprintersonlinestore.com/full-acrylic-reprap-prusa-i3-kit). Refer to 
 below changes section for details.

Instructions here assume you are familiar with using your OS command line tools, 
you know how to use git (as you'd be using a git repository), build Arduino projects,
and have somehow managed to already print some usable 3D models on your machine with
stock Sunhokey firmware.

### Firmware backup

Before trying to use this firmware first backup your stock firmware (hopefully you
already tested your machine is working properly after fighting with calibration).

In order to download your firmware install Avrdure (I'm using a Mac and Homebrew). This is how you'd back up your firmware flash code, only adjust your serial port:

```
avrdude -p m2560 -c stk500v2 -P /dev/tty.usbserial-AL00X197 -b 115200 -F -U flash:r:prusa_i3_backup.hex
```

And your EEPROM (persistent data), in case new firmware seems to support it:

```
avrdude -p m2560 -c stk500v2 -P /dev/tty.usbserial-AL00X197 -b 115200 -F -U eeprom:r:prusa_i3_backup.eep
```

In order to confirm the applied setting patches apply 'as is' to your machine, confirm
the Configuration.h patches against your machine reading the configuration. The configuration
would show in Repetier Host when you connect directly to your Prusa i3.

Alternatively, just create a .gcode file with a text editor with the `M503' text on in and and empty line following it. Then just read your machine response. Here was mine (as shown by Repetier Host):

```
< 8:04:47 PM: start
< 8:04:47 PM: echo: External Reset
< 8:04:47 PM: 1.0.0
< 8:04:47 PM: echo: Last Updated: May 26 2015 16:38:13 | Author: (none, default config)
< 8:04:47 PM: Compiled: May 26 2015
< 8:04:47 PM: echo: Free Memory: 3800  PlannerBufferBytes: 1232
< 8:04:47 PM: echo:Hardcoded Default Settings Loaded
< 8:04:47 PM: echo:Steps per unit:
< 8:04:47 PM: echo:  M92 X80.50 Y80.50 Z405.60 E80.50
< 8:04:47 PM: echo:Maximum feedrates (mm/s):
< 8:04:47 PM: echo:  M203 X100.00 Y100.00 Z5.00 E50.00
< 8:04:47 PM: echo:Maximum Acceleration (mm/s2):
< 8:04:47 PM: echo:  M201 X5000 Y5000 Z90 E10000
< 8:04:47 PM: echo:Acceleration: S=acceleration, T=retract acceleration
< 8:04:47 PM: echo:  M204 S950.00 T950.00
< 8:04:47 PM: echo:Advanced variables: S=Min feedrate (mm/s), T=Min travel feedrate (mm/s), B=minimum segment time (ms), X=maximum XY jerk (mm/s),  Z=maximum Z jerk (mm/s),  E=maximum E jerk (mm/s)
< 8:04:47 PM: echo:  M205 S0.00 T0.00 B20000 X20.00 Z0.40 E5.00
< 8:04:47 PM: echo:Home offset (mm):
< 8:04:47 PM: echo:  M206 X0.00 Y0.00 Z0.00
< 8:04:47 PM: echo:PID settings:
< 8:04:47 PM: echo:   M301 P22.20 I1.08 D114.00
< 8:04:48 PM: https://www.sunhokey.com Machine:Prusa I3
```

So stock firmware was 1.0.0 as displayed above, built on May 26th 2015 (just before
shipping!).

## Useful links

 * [RepRap Marlin wiki page](http://reprap.org/wiki/Marlin)

 * [Hiboson's Sunhokey Prusa i3 videos](https://youtu.be/OwwfR-UDEFc). There are few
differences on the machine he builds compared to May/2015 Sunhokey's. Anyway, the 
videos shipped with the machine skipped some important parts like how to mount the
control board (anyway, get some PCB spacers for this) or how to attach the Y axis belt.

 * [Thingiverse Sunhokey 3D printer owners](http://www.thingiverse.com/groups/sunhokey-3d-printer-owners). Keep an eye on the improvements other guys are doing.

### Some tips

From my build experience (start of Jun/2015), first 3d printer contact actually:

* Sunhokey videos are hard to follow for correct pieces orientation, take a good look
to shape features.

* Missing MKS v1.1 board mounting instructions, check Hibobon's videos and forums. GIYF.

* It's likely your aluminum bed would be bent, if you find a way to avoid a glass bed 
please let me know. Initially I'm using a two dollars frame glass from Home Depot (+6 USD
for a glass cutter because I could not get my desired), anyway, using glass is really
tricky, there are some good ideas on Thingiverse if you want to avoid holding clips.

* My extruder motor gear was about 3.5mm off (inside position), this was causing the 
filament to be released during printing.

* Use stock firmware to get you started, I'm sure that if anything gets changed the 
Sunhokey guys would adapt it, googling around, seems customer support is not bad.

* The shipped SD card is not compattible (speed, timing, etc) with the stock firmware,
I had to use an 'old 4GB Sandisk' I had (speed 4).

* Get a extension cord with a switch, keep the switch near you when testing to shutdown
the unit quickly before anything breaks (e.g. had you missed to connect any axis stop
switch?).

* IMPORTANT: SD card is really convenient, but something I notticed is that stopping a 
print without first pausing it would leave the unit in a non recoverable state (motion into out of limits). Both pause or stop won't shutdown the printing immediately though (might take
2~20 seconds).

* Get Gym mats before attempting to level your printer, this is to dampen the conducted 
noise from your printer, respect your relatives (and neighbors if you live in an apartment).

* Leveling is so damn tricky!!! This don't even attempt to print if your bed is not flat
and leveled (use a metal ruler in perpendicular size position to 'see' your deformation).

* Printing is slow, simple stuff takes couple of hours, more below. Be sure to have 
time before you get started.

* Use latest 'best' slicer, there are frequent slic3r and Cura release. Sunhokey settings
seem non optimal for slic3r with outdated repetier on Mac.  Download latest Cura and adapt
the settings from Sunhokey's configuration snapshots for Repetier, this will pay off.

* Start with PLA filament. It sticks nicely to a 40 ~ 50 C glass bed and to blue tape.

* Cura is awesome, start with 0.25mm layers (or maybe Repetier/Slic3r on Mac OS X is 
really bad, I don't know). Cura you can quicly show a printing estimate, add multiple
parts (automatically positioned), and get decent results without many tweaks 
(use suggested defaults when in doubt, e.g. brims).

* Autodesk Meshmixer is a nice .obj to .stl conveter.

* My extruder steps per mm are way off the shipped values, this is not reflected in
this branch, I'm keeping in my personal settings branch, but check this in your printer:
if the Bowden extrusion gear diameter is 10 mm is likely your setting is about 101 ~ 102.5
steps per mm.


## Over Upstream Marlin Changes

### Jun/8th/2015

Changes done on top of official 1.0.2 stable branch.

Using settings from M503 gcode response (which actually match to those ones uploaded
to forums for Sunhokey 2015 Prusa i3 machines.

 * MKS v1.1, (RAMPS 1.4 + Arduino ATMEGA2560)

 * Single extruder

 * Temperature thermistor sensors enabled for both extruder and bed

 * Horizontal size limited extra 5mm to account for some glass bed platform restrictions.
   While the shipped bed is aluminum it is not flat enough.

 * Stepper settigs matching reported configuration, along other forum posted settings
   (e.g. REPRAP_DISCOUNT_SMART_CONTROLLER defined, inverted Y direction, but EEPROM 
   storage not enabled).

## Contact

Refer to main Marlin firmware for core project contact information.

## Credits

The current Marlin dev team consists of:

 - Scott Lahteine [@thinkyhead] - English
 - Andreas Hardtung [@AnHardt] - Deutsch, English
 - [@Wurstnase] - Deutsch, English
 - [@fmalpartida] - English, Spanish
 - [@CONSULitAS] - Deutsch, English
 - [@maverikou]
 - Chris Palmer [@nophead]
 - [@paclema]
 - [@epatel]
 - Erik van der Zalm [@ErikZalm]
 - David Braam [@daid]
 - Bernhard Kubicek [@bkubicek]

More features have been added by:
  - Lampmaker,
  - Bradley Feldman,
  - and others...

## License

Marlin is published under the [GPL license](/Documentation/COPYING.md) because We believe in open development.
Do not use this code in products (3D printers, CNC etc) that are closed source or are crippled by a patent.

[![Flattr this git repo](http://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=ErikZalm&url=https://github.com/MarlinFirmware/Marlin&title=Marlin&language=&tags=github&category=software)
=======
# Marlin 3D Printer Firmware
<img align="right" src="../../raw/1.1.x/buildroot/share/pixmaps/logo/marlin-250.png" />

Marlin is the world's most popular open source firmware for Replicating Rapid Prototyper (RepRap) machines, commonly referred to as "3D printers." Marlin Firmware is highly efficient, running even on modest 16MHz embedded AVR processors. While Marlin 1.1 only supports ATmega AVR (Arduino, etc.) and AT90USB (Teensy++ 2.0), [Marlin 2.0](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x) also adds support for several ARM processors, including the SAM3X8E (Arduino Due), NXP LPC1768/LPC1769 ARM Cortex-M3 (Re-Arm, MKS SBASE, Smoothieboard), and ARM Cortex-M4 (Teensy 3.5/3.6, STM32F1/4/7).

A monumental amount of talent and effort goes into Marlin production, and thanks are due to many volunteers around the world. We work closely with the community, contributors, vendors, host and library developers, etc. to improve the quality, configurability, and compatibility of Marlin Firmware with a [huge variety](http://marlinfw.org/docs/configuration/configuration.html#motherboard) of boards. For the final 1.1 release we focused on code quality, performance, stability, and overall user experience. Several new features were added, many of which require no extra hardware.

## Documentation

- Visit [marlinfw.org](http://marlinfw.org/) for complete documentation on [configuration](http://marlinfw.org/docs/configuration/configuration.html), [installation](http://marlinfw.org/docs/basics/install.html), [features](http://marlinfw.org/meta/features/), and the many [G-codes](http://marlinfw.org/meta/gcode/) that Marlin supports. We will continue to expand the site to include in-depth articles, tutorials, and how-to videos on all of Marlin's features.
- See the [Releases](https://github.com/MarlinFirmware/Marlin/releases) page for Release Notes on all current and previous versions of Marlin.
- Check out the [RepRap.org Marlin Page](http://reprap.org/wiki/Marlin) for an overview of Marlin and its role in the RepRap project.

## Marlin 1.1.x

The 1.1.x branch is home to all tagged releases of Marlin 1.1.

Marlin 1.1.9 is the final release of the AVR-only flat version of Marlin Firmware, so there will be no further 1.1.x releases. However [`bugfix-1.1.x`](https://github.com/MarlinFirmware/Marlin/tree/bugfix-1.1.x) will continue to receive patches for critical bugs, so be sure to test it (or [`bugfix-2.0.x`](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x)) before reporting any bugs you find in 1.1.9.

## Marlin 2.0.x

[Marlin 2.0](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x) is the future, featuring a much-improved hierarchical file structure and full [32-bit support](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x) via a Hardware Access Layer (HAL). Marlin 2.0 continues to work with [Arduino IDE](https://www.arduino.cc/en/Main/Software) for the platforms it supports, and the excellent [PlatformIO IDE](https://platformio.org/platformio-ide) is recommended for the next generation of ARM-based boards. If you're looking for the very best that Marlin has to offer and aren't bothered by a few rough edges, give version 2.0 a try!

## Contributing to Marlin

Click on the [Issue Queue](https://github.com/MarlinFirmware/Marlin/issues) and [Pull Requests](https://github.com/MarlinFirmware/Marlin/pulls) links above at any time to see what we're currently working on.

To submit patches and new features for Marlin 2.0 check out the [bugfix-2.0.x](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x) branch, add your commits, and submit a Pull Request back to the `bugfix-2.0.x` branch. Once 2.0.x has been certified for a critical mass of common 32-bit boards, it will become the next major release and will be the basis for all future major and minor releases.

Note that our "bugfix" branches always contain the latest patches and new code. These patches may not be widely tested. As always, when using "nightly" builds of Marlin, proceed with full caution.

## Marlin Resources

- [Marlin Home Page](http://marlinfw.org/) - The Marlin Documentation Project. [Help us](https://github.com/MarlinFirmware/MarlinDocumentation) expand this site!
- [@MarlinFirmware](https://twitter.com/MarlinFirmware) on Twitter - Follow for news, release alerts, and tips & tricks. (Maintained by [@thinkyhead](https://github.com/thinkyhead).)

## Marlin User Support

Looking for help? Our GitHub Issue Queue is only for development-related issues, feature requests, and bug reports. But there are several places where you can get help from experienced users:

- [RepRap.org Marlin Forum](http://forums.reprap.org/list.php?415)
- ["Marlin Firmware" Facebook Group](https://www.facebook.com/groups/1049718498464482/)
- [Tom's 3D Forums](https://discuss.toms3d.org/)
- [Marlin on Discord](https://discord.gg/n5NJ59y)

## Credits

Marlin Admins:
 - Erik van der Zalm [[@ErikZalm](https://github.com/ErikZalm)]
 - Scott Lahteine [[@thinkyhead](https://github.com/thinkyhead)]
 - Roxanne Neufeld [[@Roxy-3D](https://github.com/Roxy-3D)]
 - Bob Kuhn [[@Bob-the-Kuhn](https://github.com/Bob-the-Kuhn)]

Notable contributors:
 - Alberto Cotronei [[@MagoKimbra](https://github.com/MagoKimbra)]
 - Andreas Hardtung [[@AnHardt](https://github.com/AnHardt)]
 - Bernhard Kubicek [[@bkubicek](https://github.com/bkubicek)]
 - Bob Cousins [[@bobc](https://github.com/bobc)]
 - Chris Palmer [[@nophead](https://github.com/nophead)]
 - Chris Pepper [[@p3p](https://github.com/p3p)]
 - David Braam [[@daid](https://github.com/daid)]
 - Éduardo Tagle [[@ejtagle](https://github.com/ejtagle)]
 - Edward Patel [[@epatel](https://github.com/epatel)]
 - Ernesto Martinez [[@emartinez167](https://github.com/emartinez167)]
 - F. Malpartida [[@fmalpartida](https://github.com/fmalpartida)]
 - Giuliano Zaro [[@GMagician](https://github.com/GMagician)]
 - Jochen Groppe [[@CONSULitAS](https://github.com/CONSULitAS)]
 - João Brazio [[@jbrazio](https://github.com/jbrazio)]
 - Kai [[@Kaibob2](https://github.com/Kaibob2)]
 - Luc Van Daele[[@LVD-AC](https://github.com/LVD-AC)]
 - Nico Tonnhofer [[@Wurstnase](https://github.com/Wurstnase)]
 - Petr Zahradnik [[@clexpert](https://github.com/clexpert)]
 - Thomas Moore [[@tcm0116](https://github.com/tcm0116)]
 - [[@alexxy](https://github.com/alexxy)]
 - [[@android444](https://github.com/android444)]
 - [[@benlye](https://github.com/benlye)]
 - [[@bgort](https://github.com/bgort)]
 - [[@Grogyan](https://github.com/Grogyan)]
 - [[@marcio-ao](https://github.com/marcio-ao)]
 - [[@maverikou](https://github.com/maverikou)]
 - [[@oysteinkrog](https://github.com/oysteinkrog)]
 - [[@p3p](https://github.com/p3p)]
 - [[@paclema](https://github.com/paclema)]
 - [[@paulusjacobus](https://github.com/paulusjacobus)]
 - [[@psavva](https://github.com/psavva)]
 - [[@Tannoo](https://github.com/Tannoo)]
 - [[@teemuatlut](https://github.com/teemuatlut)]
 - ...and you!

## License

Marlin is published under the [GPL license](https://github.com/COPYING.md) because we believe in open development. The GPL comes with both rights and obligations. Whether you use Marlin firmware as the driver for your open or closed-source product, you must keep Marlin open, and you must provide your compatible Marlin source code to end users upon request. The most straightforward way to comply with the Marlin license is to make a fork of Marlin on Github, perform your modifications, and direct users to your modified fork.

While we can't prevent the use of this code in products (3D printers, CNC, etc.) that are closed source or crippled by a patent, we would prefer that you choose another firmware or, better yet, make your own.

---

<!-- [![Coverity Scan Build Status](https://scan.coverity.com/projects/2224/badge.svg)](https://scan.coverity.com/projects/2224) -->
- [![Travis Build Status](https://travis-ci.org/MarlinFirmware/Marlin.svg)](https://travis-ci.org/MarlinFirmware/Marlin)
- [![Flattr Scott Lahteine](http://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=thinkyhead&url=https://github.com/MarlinFirmware/Marlin&title=Marlin&language=&tags=github&category=software) - Flattr Scott Lahteine
- [![Flattr Erik van der Zalm](http://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=ErikZalm&url=https://github.com/MarlinFirmware/Marlin&title=Marlin&language=&tags=github&category=software) - Flattr Erik van der Zalm
>>>>>>> 1.1.9
