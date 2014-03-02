nook-dictionary
===============

Create own dictionary for B&amp;N Nook Simple Touch and Nook GlowLight built-in Reader app

Tested on B&amp;N Nook Simple Touch with latest SW (1.2.1) and rooted by [NookManager](http://forum.xda-developers.com/showthread.php?t=2040351)

![Nook Simple Touch with modified basewords.db](https://github.com/geoRG77/nook-dictionary/raw/master/wiki/preview.gif "Nook Simple Touch with modified basewords.db")

## Input format

My goal was to convert GNU/FDL Engligh-Czech dictionary from [GNU/FDL anglicko-český slovník](http://slovnik.zcu.cz) which is using following format:

```
english word [TAB] translation [TAB] notes [TAB] extra notes [TAB] author
```
It should be easy to customize the script to whatever source format you need.

## Output

Output (basewords.db) SQLite3 DB
```sql
CREATE TABLE android_metadata
(locale TEXT);

INSERT INTO android_metadata VALUES('en_US');

CREATE TABLE tblWords
(_id INTEGER PRIMARY KEY AUTOINCREMENT,
 term TEXT,
 description BLOB);

CREATE INDEX term_index on tblWords (term ASC);
```

with following BLOB (zipped HTML description)
```HTML
<div class="entry">
  <sup>6</sup><b><span class="searchterm-headword">a</span></b><br/>
  <i>abbreviation</i>
  <div class="definitions">
    <b>1</b> absent <br/>
    <b>2</b> acceleration <br/>
    <b>3</b> acre <br/>
    <b>4</b> adult <br/>
    [...]
  </div>
</div>
```

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
