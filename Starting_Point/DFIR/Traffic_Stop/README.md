<H1>Traffic Stop</H1>
<hr>

<p></p>
Traffic Stop was a series of challenges from CSC 2020. The challenges revolve around file carving and data recovery. I would classify the challenges as medium to hard and requiring some research. Below are walkthroughs for the three challenges.
<hr>
<p></p>
<H2>Challenges</H2>
<details>
    <summary></summary>
<p></p>
I recommend you attempt these challenges on your own prior to looking through the walkthrough. Answers are at the end of the walkthroughs.
<p></p>
All three challenges us the usb.zip file located below.
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1MMLbIp_GT-RTojR38AFXm91lnehNf3Rf/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
<details>
    <summary>Extracting the File</summary>
<p></p>
All you need to do is run the following command on the file:

```
unzip usb.zip
```

<p></p>
Which outputs:
<p></p>

```
❯ unzip usb.zip
Archive:  usb.zip
  inflating: usb.raw                 
```

<p></p>
You now have a file to work with usb.raw
<p></p>
</details>
<p></p>
<hr></hr>
<p></p>
<details>
    <summary>Challenge 1</summary>
<p></p>
The first challenge we are given is:
<p></p>
We have an acquired a USB from a suspect believed to be associated with
human trafficking. Can you find any evidence to support this claim?
<p></p>
<details>
    <summary>Hint</summary>
If you find an account number use `echo <account> | xxd -r -p`
</details>
<p></p>
<details>
    <summary>Hint</summary>
I spent all this time configuring IRC but everyone is using slack these days.
</details>
<p></p>
<details>
    <summary>Walkthrough</summary>
The first thing I like to do is run <kbd>file</kbd> on the file IOT determine what type of file it is.
<p></p>

```
❯ file usb.raw
usb.raw: DOS/MBR boot sector; partition 1 : ID=0x7, start-CHS (0x11,2,5), end-CHS (0x3e9,51,38), startsector 67584, 3907584 sectors, extended partition table (last)
```

<p></p>
We can see the DOS/MBR boot sector but what does that mean?
<br>
"A master boot record (MBR) is a special type of boot sector at the very beginning of partitioned computer mass storage devices like fixed disks or removable drives intended for use with IBM PC-compatible systems and beyond.
<p></p>
The MBR holds the information on how the logical partitions, containing file systems, are organized on that medium. The MBR also contains executable code to function as a loader for the installed operating system—usually by passing control over to the loader's second stage, or in conjunction with each partition's volume boot record (VBR). This MBR code is usually referred to as a boot loader."
<p></p>
We can also see that it contains 1 partition. So this starts to confirm for us that this file is actually a USB but we can use another command to further gain information on a drive and that is <kbd>fdisk</kbd>. This tool allows us to view or manipulate partition tables.
<p></p>
<details>
    <summary>fdisk</summary>
<p></p>
Below is the basic help file for <kbd>fdisk</kbd>:
<p></p>

```
❯ sudo fdisk -h                                                                                                                                                                               
                                                                                                                                                                                              
Usage:                                                                                                                                                                                        
 fdisk [options] <disk>         change partition table                                                                                                                                        
 fdisk [options] -l [<disk>...] list partition table(s)                                                                                                                                       
                                                                                                                                                                                              
Display or manipulate a disk partition table.                                                                                                                                                 
                                                                                                                                                                                              
Options:
 -b, --sector-size <size>      physical and logical sector size
 -B, --protect-boot            don't erase bootbits when creating a new label
 -c, --compatibility[=<mode>]  mode is 'dos' or 'nondos' (default)
 -L, --color[=<when>]          colorize output (auto, always or never)
                                 colors are enabled by default
 -l, --list                    display partitions and exit
 -x, --list-details            like --list but with more details
 -n, --noauto-pt               don't create default partition table on empty devices
 -o, --output <list>           output columns
 -t, --type <type>             recognize specified partition table type only
 -u, --units[=<unit>]          display units: 'cylinders' or 'sectors' (default)
 -s, --getsz                   display device size in 512-byte sectors [DEPRECATED]
     --bytes                   print SIZE in bytes rather than in human readable format
     --lock[=<mode>]           use exclusive device lock (yes, no or nonblock)
 -w, --wipe <mode>             wipe signatures (auto, always or never)
 -W, --wipe-partitions <mode>  wipe signatures from new partitions (auto, always or never)

 -C, --cylinders <number>      specify the number of cylinders
 -H, --heads <number>          specify the number of heads
 -S, --sectors <number>        specify the number of sectors per track

 -h, --help                    display this help
 -V, --version                 display version

Available output columns:
 gpt: Device Start End Sectors Size Type Type-UUID Attrs Name UUID
 dos: Device Start End Sectors Cylinders Size Type Id Attrs Boot End-C/H/S Start-C/H/S
 bsd: Slice Start End Sectors Cylinders Size Type Bsize Cpg Fsize
 sgi: Device Start End Sectors Cylinders Size Type Id Attrs
 sun: Device Start End Sectors Cylinders Size Type Id Flags

For more details see fdisk(8).
```

<p></p>
Bellow is a description for each flag:
<p></p>

```
       -b, --sector-size sectorsize
              Specify the sector size of the disk.  Valid values are 512, 1024, 2048, and 4096.  (Recent kernels know the sector size.  Use this option only on old kernels or  to  over‐
              ride the kernel's ideas.)  Since util-linux-2.17, fdisk differentiates between logical and physical sector size.  This option changes both sector sizes to sectorsize.

       -B, --protect-boot
              Don't erase the beginning of the first disk sector when creating a new disk label.  This feature is supported for GPT and MBR.

       -c, --compatibility[=mode]
              Specify the compatibility mode, 'dos' or 'nondos'.  The default is non-DOS mode.  For backward compatibility, it is possible to use the option without the mode argument --
              then the default is used.  Note that the optional mode argument cannot be separated from the -c option by a space, the correct form is for example '-c=dos'.

       -h, --help
              Display a help text and exit.

       -L, --color[=when]
              Colorize the output.  The optional argument when can be auto, never or always.  If the when argument is omitted, it defaults to auto.  The colors can be disabled; for  the
              current built-in default see the --help output.  See also the COLORS section.

       -l, --list
              List the partition tables for the specified devices and then exit.  If no devices are given, those mentioned in /proc/partitions (if that file exists) are used.

       -x, --list-details
              Like --list, but provides more details.

       --lock[=mode]
              Use  exclusive  BSD lock for device or file it operates.  The optional argument mode can be yes, no (or 1 and 0) or nonblock.  If the mode argument is omitted, it defaults
              to "yes".  This option overwrites environment variable $LOCK_BLOCK_DEVICE.  The default is not to use any lock at all, but it's recommended to avoid collisions with  udevd
              or other tools.

       -n, --noauto-pt
              Don't automatically create a default partition table on empty device.  The partition table has to be explicitly created by user (by command like 'o', 'g', etc.).

       -o, --output list
              Specify which output columns to print.  Use --help to get a list of all supported columns.

              The default list of columns may be extended if list is specified in the format +list (e.g., -o +UUID).

       -s, --getsz
              Print the size in 512-byte sectors of each given block device.  This option is DEPRECATED in favour of blockdev(8).

       -t, --type type
              Enable support only for disklabels of the specified type, and disable support for all other types.

       -u, --units[=unit]
              When  listing partition tables, show sizes in 'sectors' or in 'cylinders'.  The default is to show sizes in sectors.  For backward compatibility, it is possible to use the
              option without the unit argument -- then the default is used.  Note that the optional unit argument cannot be separated from the -u option by a space, the correct form  is
              for example '-u=cylinders'.

       -C, --cylinders number
              Specify the number of cylinders of the disk.  I have no idea why anybody would want to do so.

       -H, --heads number
              Specify the number of heads of the disk.  (Not the physical number, of course, but the number used for partition tables.)  Reasonable values are 255 and 16.

       -S, --sectors number
              Specify the number of sectors per track of the disk.  (Not the physical number, of course, but the number used for partition tables.) A reasonable value is 63.

       -w, --wipe when
              Wipe  filesystem,  RAID  and partition-table signatures from the device, in order to avoid possible collisions.  The argument when can be auto, never or always.  When this
              option is not given, the default is auto, in which case signatures are wiped only when in interactive mode.  In all cases detected signatures are reported by warning  mes‐
              sages before a new partition table is created.  See also wipefs(8) command.

       -W, --wipe-partitions when
              Wipe  filesystem,  RAID and partition-table signatures from a newly created partitions, in order to avoid possible collisions.  The argument when can be auto, never or al‐
              ways.  When this option is not given, the default is auto, in which case signatures are wiped only when in interactive mode and after confirmation by user.  In  all  cases
              detected signatures are reported by warning messages before a new partition is created.  See also wipefs(8) command.

       -V, --version
              Display version information and exit.
```

<p></p>
</details>
<p></p>
We will now run <kbd>fdisk</kbd> on usb.raw to inspect the partition tables and confirm that this is in fact a usb. we will use the <kbd>-l</kbd> flag to list the partition tables.
<p></p>

```
❯ sudo fdisk -l usb.raw
Disk usb.raw: 1.93 GiB, 2068840448 bytes, 4040704 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x3157f404

Device     Boot Start     End Sectors  Size Id Type
usb.raw1        67584 3975167 3907584  1.9G  7 HPFS/NTFS/exFAT
```

<p></p>
This confirms for me that this is a USB device and the next step will be to mount it IOT investigate further.
</details>


</details>