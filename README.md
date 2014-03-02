nook-dictionary
===============

Create own dictionary for B&amp;N Nook Simple Touch and Nook GlowLight built-in Reader app

Tested on B&amp;N Nook Simple Touch with latest SW (1.2.1) and rooted by [NookManager](http://forum.xda-developers.com/showthread.php?t=2040351)


## Usage

Convert dictionary to Nook format
```shell
python nook-dictionary.py
```

Remount /system directory to enable write access
```shell
adb shell mount -o rw,remount /dev/block/mmcblk0p5 /system
```

Backup original dictionary (Merriam-Webster's Collegiate Dictionary, 11th Edition)
```shell
adb pull /system/media/reference/basewords.db ./
```

Create a symlink
```shell
adb shell ln -s /system/media/reference/basewords.db /media/dict/basewords.db
```

Copy created dictionary to the device
```shell
adb push basewords.db /media/dict/basewords.db
```

Reboot the Nook (/system directory will be mounted as read-only automatically)
```shell
adb shell reboot
```

## Sources
Info about SQLite3 DB's and BLOBs<br/>
http://nookdevs.com/Nook_Simple_Touch_dictionary

Great thread about Nook dictionaries and hacking built-in Reader app<br/>
http://forum.xda-developers.com/showthread.php?t=1477918

Renate NST's page with usefull tools<br/>
http://www.temblast.com/android.htm
