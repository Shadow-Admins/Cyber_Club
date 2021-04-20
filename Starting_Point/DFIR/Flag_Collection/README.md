<H1>Flag Collection</H1>
<hr>

<p></p>
Flag Collection was a series of challenges from CSC 2020. It revolves around file carving and stenography to a point. I would classify the first challenge as very easy and the second requiring some research. Below are walkthroughs for both challenges.
<hr>
<p></p>
<H2>Challenges</H2>
<details>
    <summary></summary>
<p></p>
I recommend you attempt these challenges on your own prior to looking through the walkthrough. Answers are at the end of the walkthroughs.
<p></p>
<details>
    <summary>Challenge 1</summary>
<p></p>
The first challenge we are given is:
<p></p>
Have you seen my flag collection? I could have sworn it was around here
somewhere.
<p></p>
<details>
    <summary>Hint</summary>
<p></p>
File carving is pretty cool.
</details>
<p></p>
<details>
    <summary>Hint</summary>
<p></p>
Foremost is my favourite.
</details>
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1MRxLoCJQTiKhUys-G_vBCAjkOgz07HKQ/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
<details>
    <summary>Extracting the File</summary>
<p></p>
The file comes compressed as a .7z IOT extract it we first need to ensure we have p7zip installed on our system by using the following command:
<p></p>

```
sudo apt install p7zip
```

<p></p>
Once p7zip is installed we run the following command to extract the challenge file:
<p></p>

```
❯ p7zip -d collection.7z

7-Zip (a) [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_AU.UTF-8,Utf16=on,HugeFiles=on,64 bits,8 CPUs Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz (806EA),ASM,AES-NI)

Scanning the drive for archives:
1 file, 3658662 bytes (3573 KiB)

Extracting archive: collection.7z
--
Path = collection.7z
Type = 7z
Physical Size = 3658662
Headers Size = 138
Method = LZMA2:26
Solid = -
Blocks = 1

Everything is Ok     

Size:       133169152
Compressed: 3658662
```

<p></p>
</details>
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
Now we have a file to work with I always start with determining what type of file it is using the <kbd>file</kbd> command. (file tests each argument in an attempt to classify it.  There are three sets of tests, performed in this order: filesystem tests, magic tests, and language tests.  The first test that succeeds causes the file type to be printed.)
<p></p>

```
file collection.img
```

<p></p>
Whaich returns:
<p></p>

```
❯ file collection.img
collection.img: Linux rev 1.0 ext4 filesystem data, UUID=e1b1a8d2-f026-443c-94f0-6f799837d5ba (needs journal recovery) (extents) (large files) (huge files)
```

<p></p>
So from this we can see that it is "filesystem data" and it is formatted as ext4 (The ext4 journaling file system or fourth extended filesystem is a journaling file system for Linux). 
<p></p>
You can attempt to open it through file explorer but it cant mount, this is when we turn to the hints which points us to the program intended for use <kbd>foremost</kbd>.
<p></p>
<details>
    <summary>What is foremost?</summary>
Foremost is a forensic data recovery program for Linux used to recover files using their headers, footers, and data structures through a process known as file carving. Although written for law enforcement use, it is freely available and can be used as a general data recovery tool.
<p></p>
Foremost is designed to ignore the type of underlying filesystem and directly read and copy portions of the drive into the computer's memory. It takes these portions one segment at a time, and using a process known as file carving searches this memory for a file header type that matches the ones found in Foremost's configuration file. When a match is found, it writes that header and the data following it into a file, stopping when either a footer is found, or until the file size limit is reached.
<p></p>
Foremost is used from the command-line interface, with no graphical user interface option available. It is able to recover specific filetypes, including jpg, gif, png, bmp, avi, exe, mpg, wav, riff, wmv, mov, pdf, ole, doc, zip, rar, htm, and cpp. There is a configuration file (usually found at /usr/local/etc/foremost.conf) which can be used to define additional file types.
<p></p>
Foremost can be used to recover data from image files, or directly from hard drives that use the ext3, ext4, NTFS, or FAT filesystems. Foremost can also be used via a computer to recover data from iPhones.
<p></p>
<H2>Usage</H2>
<p></p>
Below is the basic help file for foremost.
<p></p>

```
❯ foremost -h
foremost version 1.5.7 by Jesse Kornblum, Kris Kendall, and Nick Mikus.
$ foremost [-v|-V|-h|-T|-Q|-q|-a|-w-d] [-t <type>] [-s <blocks>] [-k <size>] 
        [-b <size>] [-c <file>] [-o <dir>] [-i <file] 

-V  - display copyright information and exit
-t  - specify file type.  (-t jpeg,pdf ...) 
-d  - turn on indirect block detection (for UNIX file-systems) 
-i  - specify input file (default is stdin) 
-a  - Write all headers, perform no error detection (corrupted files) 
-w  - Only write the audit file, do not write any detected files to the disk 
-o  - set output directory (defaults to output)
-c  - set configuration file to use (defaults to foremost.conf)
-q  - enables quick mode. Search are performed on 512 byte boundaries.
-Q  - enables quiet mode. Suppress output messages. 
-v  - verbose mode. Logs all messages to screen
```

<p></p>
Below is the description for all flags.
<p></p>


```
        -h     Show a help screen and exit.

       -V     Show copyright information and exit.

       -d     Turn on indirect block detection, this works well for Unix file systems.

       -T     Time stamp the output directory so you don't have to delete the output dir when running multiple times.

       -v     Enables verbose mode. This causes more information regarding the current state of the program to be displayed on the screen, and is highly recommended.

       -q     Enables quick mode. In quick mode, only the start of each sector is searched for matching headers. That is, the header is searched only up to the  length  of  the  longest
              header. The rest of the sector, usually about 500 bytes, is ignored. This mode makes foremost run considerably faster, but it may cause you to miss files that are embedded
              in other files. For example, using quick mode you will not be able to find JPEG images embedded in Microsoft Word documents.

              Quick mode should not be used when examining NTFS file systems. Because NTFS will store small files inside the Master File Table, these files will be missed  during  quick
              mode.

       -Q     Enables Quiet mode. Most error messages will be suppressed.

       -w     Enables write audit only mode.  No files will be extracted.

       -a     Enables write all headers, perform no error detection in terms of corrupted files.

       -b number
              Allows you to specify the block size used in foremost.  This is relevant for file naming and quick searches.  The default is 512.       ie.  foremost -b 1024 image.dd

       -k number
              Allows  you  to  specify  the chunk size used in foremost.  This can improve speed if you have enough RAM to fit the image in.  It reduces the checking that occurs between
              chunks of the buffer.  For example if you had > 500MB of RAM.       ie.  foremost -k 500 image.dd

       -i file
              The file is used as the input file.  If no input file is specified or the input file cannot be read then stdin is used.
        
        -o directory
              Recovered files are written to the directory directory.

       -c file
              Sets the configuration file to use. If none is specified, the file "foremost.conf" from the current directory is used, if that doesn't exist then  "/etc/foremost.conf"  is
              used.  The  format  for the configuration file is described in the default configuration file included with this program. See the CONFIGURATION FILE section below for more
              information.

       -s number
              Skips number blocks in the input file before beginning the search for headers.       ie.  foremost -s 512 -t jpeg -i /dev/hda1
```

</details>
<p></p>
So now that we know what foremost is we can run it against collection.img the command looks like this:
<p></p>

```
foremost collection.img
```

<p></p>
which outputs:
<p></p>

```
❯ foremost collection.img                                                                                                                                                                     
Processing: collection.img                                                                                                                                                                    
|**|                                                                                                                                                                                          
```

<p></p>
Not much information right! but if we look at our current working directory we can see that a directory called output has been created (thats unless you used the <kbd>-o</kbd> flag and specified a directory). If we go into that directory we can see that there is an audit.txt file and 2 new directories. Running <kbd>cat</kbd> on the audit.txt file outputs the following:
<p></p>

```
❯ cat audit.txt                                                                                                                                                                               
Foremost version 1.5.7 by Jesse Kornblum, Kris Kendall, and Nick Mikus                                                                                                                        
Audit File

Foremost started at Tue Apr 20 11:52:20 2021
Invocation: foremost collection.img 
Output directory: /home/parrot/ctf/csc/Preseason/Flag_Collection/output
Configuration file: /etc/foremost.conf
------------------------------------------------------------------
File: collection.img
Start: Tue Apr 20 11:52:20 2021
Length: 127 MB (133169152 bytes)
  
Num      Name (bs=512)         Size      File Offset     Comment 

0:      00016456.jpg         128 KB         8425472      
1:      00016720.jpg          45 KB         8560640      
2:      00016816.jpg          33 KB         8609792      
3:      00016888.jpg          41 KB         8646656      
4:      00016976.jpg         107 KB         8691712      
5:      00017192.jpg          98 KB         8802304      
6:      00017392.jpg          27 KB         8904704      
7:      00018072.jpg          42 KB         9252864      
8:      00017448.png         309 KB         8933376       (1200 x 900)
9:      00253952.jpg           1 MB       130023424      
10:     00258008.png         702 KB       132100096       (931 x 524)
Finish: Tue Apr 20 11:52:24 2021

11 FILES EXTRACTED

jpg:= 9
png:= 2
------------------------------------------------------------------

Foremost finished at Tue Apr 20 11:52:24 2021
```

<p></p>
So we can see that 11 files have been carved from collection.img from here if we look at the images in file explorer we should be able to find the flag within one of the images.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Flag_Collection/images/00016720.jpg" width="600"><br>
</div>
FLAG{fl4g5_4r3_fun}
</details>
</details>
</details>
</details>
<p></p>
<details>
    <summary>Challenge 2</summary>




</details>