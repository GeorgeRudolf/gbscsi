# gbscsi
From parts unknown... A Cheap SCSI Disk Emulator board

This thing is poorly documented. Everything in this repo was, if still somewhat carefully, hastily done.
The first version posted to github on Aug-24-2022 had one major issue (despite being still usable), so
the board got respun after just one day. Nice job, George.

It's free. Do whatever the hell you want with it. Just keep attribution.
Unfortunately the actual design files are in proprietary format, but I may redo it in KiCad someday.
If that ever happens, I'll push it here.

Included is a somewhat workable STEP file you can use for modeling an enclosure.

This really is aimed at pre-assembled ordering. While populating by hand is possible, I don't see why you would. It won't be cheaper
or faster. It's possible some people will resell this, but you can easily order your own without hassle, and resell the spares if you don't need 5 of them.
The included instructions target JLCPCB's PCBA service, but you can use others as well. You're on your own there, though.
Last I tested, BlueSCSI firmware does run on the APM32F103C8T6 microcontroller used as default (you can opt for a regular STM32 though, see the .txt file).

Everything else is pretty self-explanatory. You don't need too big a grasp of electronics to use the contents, just
a willingness to learn - some of the files provided guarantee most of the work is already done.

![image](https://user-images.githubusercontent.com/24400566/186554213-0b1e5d74-8df3-4cb5-a4b8-6f6f7c0e0e3d.png)

As usual, suggestions welcome.
