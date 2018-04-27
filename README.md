# Disk-image-scripts

Scripts for macOS (tested on High Sierra) for manipulating disk image files for 8-bit computers.

**imd2hfe** - convert [ImageDisk](http://www.classiccmp.org/dunfield/img/index.htm) (IMD) to HxC ([HFE](http://hxc2001.com/download/floppy_drive_emulator/SDCard_HxC_Floppy_Emulator_HFE_file_format.pdf)) for use by HxC or FlashFloppy USB emulators

**td02hfe** - convert [Teledisk](http://www.classiccmp.org/dunfield/img/index.htm) (TD0) to HxC ([HFE](http://hxc2001.com/download/floppy_drive_emulator/SDCard_HxC_Floppy_Emulator_HFE_file_format.pdf)) for use by HxC or FlashFloppy USB emulators

**hfe2cpm** - convert HxC ([HFE](http://hxc2001.com/download/floppy_drive_emulator/SDCard_HxC_Floppy_Emulator_HFE_file_format.pdf)) to CPM raw (IMG) for use by [cpmtools](https://github.com/lipro/cpmtools) ([home page](http://www.moria.de/~michael/cpmtools/))

**cpm2hfe** - convert from CPM raw (IMG) to HxC ([HFE](http://hxc2001.com/download/floppy_drive_emulator/SDCard_HxC_Floppy_Emulator_HFE_file_format.pdf)) for use by HxC or FlashFloppy USB emulators

The above require **dsktrans** (part of [libdsk](https://github.com/lipro/libdsk)) ([home page](http://www.seasip.info/Unix/LibDsk/)) and **disk-analyse** (part of [Disk-Utilities](https://github.com/keirf/Disk-Utilities))


**hxc_eject** - unmount a USB drive, remove all the extraneous files that macOS installs on it, and sort the FAT filesytem into alphabetical order (for use by HxC or FlashFloppy USB emulators)

requires [FATSort](https://sourceforge.net/projects/fatsort/files/) ([home page](http://fatsort.sourceforge.net/))


More details at https://www.richardloxley.com/2018/04/27/osborne-restoration-part-16-transferring-cpm-software

Example workflow:

```
hfe2cpm /Volumes/OSBORNE/Games.hfe Games.cpm

cpmls -l Games.cpm

0:
-rwxrwxrwx 5632 Jan 01 1970 setup80.com
-rwxrwxrwx 8704 Jan 01 1970 zork1.com
-rw-rw-rw- 84992 Jan 01 1970 zork1.dat

cpmcp Games.cpm MONOPLY.BAS 0:MONOPLY.BAS

cpmls -l Games.cpm

0:
-rw-rw-rw- 26752 Jan 01 1970 monoply.bas
-rwxrwxrwx 5632 Jan 01 1970 setup80.com
-rwxrwxrwx 8704 Jan 01 1970 zork1.com
-rw-rw-rw- 84992 Jan 01 1970 zork1.dat

cpm2hfe Games.cpm /Volumes/OSBORNE/Games.hfe

hxc_eject

Usage: /Users/Richard/bin/hxc_eject <volume name>

Volumes are:
"2TB"
"Black Backup"
"Boot SSD"
"OSBORNE"
"com.apple.TimeMachine.localsnapshots"

hxc_eject "OSBORNE"
* Turning off Spotlight indexing *
Password:
2018-04-27 18:19:47.662 mdutil[23284:737639] mdutil disabling Spotlight: /Volumes/OSBORNE -&gt; kMDConfigSearchLevelFSSearchOnly
* Deleting OSX-specific files *
* Unmounting disk *
Volume OSBORNE on disk4s1 unmounted
* Sorting disk *
*** If no errors appear above, it is now safe to remove the USB stick "OSBORNE" ***```
