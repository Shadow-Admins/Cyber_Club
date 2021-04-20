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
<p></p>
<hr>
<p></p>
<details>
    <summary>Challenge 2</summary>
<p></p>
I got sent a new flag for my collection but something isn't right with
it. Can you open it?
<p></p>
<details>
    <summary>Hint</summary>
Your Head-er can GIF you the answers.
</details>
<p></p>
<details>
    <summary>Hint</summary>
Magic numbers can tell you a lot.
</details>
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/18bv2QWGmy8zuRvTzFJptfvN1f7k9ts4M/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>

<details>
    <summary>Walkthrough</summary>
The first thing I start with is attempting to open and view china.gif through the file explorer however it returns an error message stating it couldn't open the file and it appears it isn't a .gif file.
<br>
From here we should run the <kbd>file</kbd> command to ascertain what the file type is, which returns:
<p></p>

```
❯ file china.gif
china.gif: data
```

<p></p>
Interesting, running <kbd>strings</kbd> or <kbd>cat</kbd> on the file returns nothing of interest either.
<p></p>
Its time to look at the hints now.
<br>
Lets start with the second hint pointing towards the first hint. A quick google returns:
<br>
"File magic numbers are the first bits of a file which uniquely identify the type of file."
<br>
"Magic numbers/File signatures are typically not visible to the user but can be seen by using a hex editor or by using the <kbd>xxd</kbd> command"
<p></p>
Ok so we know what magic numbers are lets have a look at this files magic number, we will first use <kbd>xxd</kbd> to view the magic number:
<p></p>
<details>
    <summary>What is xxd?</summary>
xxd creates a hex dump of a given file or standard input.  It can also convert a hex dump back to its original binary form.  Like uuencode(1) and uudecode(1) it allows the transmission of binary data in a `mail-safe' ASCII representation, but has the advantage of decoding to standard output.  Moreover, it can be used to perform binary file patching.
<p></p>
This is the help file for <kbd>xxd</kbd>
<p></p>

```
❯ xxd -h
Usage:
       xxd [options] [infile [outfile]]
    or
       xxd -r [-s [-]offset] [-c cols] [-ps] [infile [outfile]]
Options:
    -a          toggle autoskip: A single '*' replaces nul-lines. Default off.
    -b          binary digit dump (incompatible with -ps,-i,-r). Default hex.
    -C          capitalize variable names in C include file style (-i).
    -c cols     format <cols> octets per line. Default 16 (-i: 12, -ps: 30).
    -E          show characters in EBCDIC. Default ASCII.
    -e          little-endian dump (incompatible with -ps,-i,-r).
    -g          number of octets per group in normal output. Default 2 (-e: 4).
    -h          print this summary.
    -i          output in C include file style.
    -l len      stop after <len> octets.
    -o off      add <off> to the displayed file position.
    -ps         output in postscript plain hexdump style.
    -r          reverse operation: convert (or patch) hexdump into binary.
    -r -s off   revert with <off> added to file positions found in hexdump.
    -d          show offset in decimal instead of hex.
    -s [+][-]seek  start at <seek> bytes abs. (or +: rel.) infile offset.
    -u          use upper case hex letters.
    -v          show version: "xxd V1.10 27oct98 by Juergen Weigert".
```

<p></p>
Bellow is the description of all flags.
<p></p>

```
        If no infile is given, standard input is read.  If infile is specified as a `-' character, then input is taken from standard input.  If no outfile is given (or a `-' character is
        in its place), results are sent to standard output.

       Note that a "lazy" parser is used which does not check for more than the first option letter, unless the option is followed by a parameter.  Spaces between a single option letter
       and its parameter are optional.  Parameters to options can be specified in decimal, hexadecimal or octal notation.  Thus -c8, -c 8, -c 010 and -cols 8 are all equivalent.

       -a | -autoskip
              Toggle autoskip: A single '*' replaces nul-lines.  Default off.

       -b | -bits
              Switch to bits (binary digits) dump, rather than hexdump.  This option writes octets as eight digits "1"s and "0"s instead of a normal hexadecimal dump. Each line is  pre‐
              ceded by a line number in hexadecimal and followed by an ascii (or ebcdic) representation. The command line switches -r, -p, -i do not work with this mode.

       -c cols | -cols cols
              Format <cols> octets per line. Default 16 (-i: 12, -ps: 30, -b: 6). Max 256.

       -C | -capitalize
              Capitalize variable names in C include file style, when using -i.

       -E | -EBCDIC
              Change the character encoding in the righthand column from ASCII to EBCDIC.  This does not change the hexadecimal representation. The option is meaningless in combinations
              with -r, -p or -i.

       -e     Switch to little-endian hexdump.  This option treats byte groups as words in little-endian byte order.  The default grouping of 4 bytes may be changed using -g.  This  op‐
              tion only applies to hexdump, leaving the ASCII (or EBCDIC) representation unchanged.  The command line switches -r, -p, -i do not work with this mode.

       -g bytes | -groupsize bytes
              Separate  the  output  of  every <bytes> bytes (two hex characters or eight bit-digits each) by a whitespace.  Specify -g 0 to suppress grouping.  <Bytes> defaults to 2 in
              normal mode, 4 in little-endian mode and 1 in bits mode.  Grouping does not apply to postscript or include style.

       -h | -help
              Print a summary of available commands and exit.  No hex dumping is performed.

       -i | -include
              Output in C include file style. A complete static array definition is written (named after the input file), unless xxd reads from stdin.

       -l len | -len len
              Stop after writing <len> octets.

       -o offset
              Add <offset> to the displayed file position.

       -p | -ps | -postscript | -plain
              Output in postscript continuous hexdump style. Also known as plain hexdump style.

       -r | -revert
              Reverse operation: convert (or patch) hexdump into binary.  If not writing to stdout, xxd writes into its output file without truncating it. Use the combination -r  -p  to
              read plain hexadecimal dumps without line number information and without a particular column layout. Additional Whitespace and line-breaks are allowed anywhere.

       -seek offset
              When used after -r: revert with <offset> added to file positions found in hexdump.

       -s [+][-]seek
              Start at <seek> bytes abs. (or rel.) infile offset.  + indicates that the seek is relative to the current stdin file position (meaningless when not reading from stdin).  -
              indicates that the seek should be that many characters from the end of the input (or if combined with +: before the current stdin file position).  Without -s  option,  xxd
              starts at the current file position.

       -u     Use upper case hex letters. Default is lower case.

       -v | -version
              Show version string.
```

<p></p>
</details>
<p></p>

```
❯ xxd china.gif | head
00000000: 0000 0000 0000 2003 9001 f700 0000 0000  ...... .........
00000010: 0000 3300 0066 0000 9900 00cc 0000 ff00  ..3..f..........
00000020: 2b00 002b 3300 2b66 002b 9900 2bcc 002b  +..+3.+f.+..+..+
00000030: ff00 5500 0055 3300 5566 0055 9900 55cc  ..U..U3.Uf.U..U.
00000040: 0055 ff00 8000 0080 3300 8066 0080 9900  .U......3..f....
00000050: 80cc 0080 ff00 aa00 00aa 3300 aa66 00aa  ..........3..f..
00000060: 9900 aacc 00aa ff00 d500 00d5 3300 d566  ............3..f
00000070: 00d5 9900 d5cc 00d5 ff00 ff00 00ff 3300  ..............3.
00000080: ff66 00ff 9900 ffcc 00ff ff33 0000 3300  .f.........3..3.
00000090: 3333 0066 3300 9933 00cc 3300 ff33 2b00  33.f3..3..3..3+.
```

<p></p>
You notice that i have piped (|) the output of xxd to <kbd>head</kbd> as magic numbers are located in the first bytes of a file so we only need to see the start.
<br>
But this just looks like a stack of numbers and we have nothing to compare it to so lets put it side by side with a working .gif image.
<p></p>

china.gif
``` 
00000000: 0000 0000 0000 2003 9001 f700 0000 0000  ...... .........
00000010: 0000 3300 0066 0000 9900 00cc 0000 ff00  ..3..f..........
00000020: 2b00 002b 3300 2b66 002b 9900 2bcc 002b  +..+3.+f.+..+..+
00000030: ff00 5500 0055 3300 5566 0055 9900 55cc  ..U..U3.Uf.U..U.
00000040: 0055 ff00 8000 0080 3300 8066 0080 9900  .U......3..f....
00000050: 80cc 0080 ff00 aa00 00aa 3300 aa66 00aa  ..........3..f..
00000060: 9900 aacc 00aa ff00 d500 00d5 3300 d566  ............3..f
00000070: 00d5 9900 d5cc 00d5 ff00 ff00 00ff 3300  ..............3.
00000080: ff66 00ff 9900 ffcc 00ff ff33 0000 3300  .f.........3..3.
00000090: 3333 0066 3300 9933 00cc 3300 ff33 2b00  33.f3..3..3..3+. 
```
Working.gif
```
00000000: 4749 4638 3961 0002 0002 8000 0000 0000  GIF89a..........
00000010: 0000 0021 ff0b 4e45 5453 4341 5045 322e  ...!..NETSCAPE2.
00000020: 3003 0100 0000 21f9 0405 0500 ff00 2c00  0.....!.......,.
00000030: 0000 0000 0200 0287 0000 002b 606c 4605  ...........+`lF.
00000040: b007 b33d 1b1d c1d5 1b09 0769 8801 010a  ...=.......i....
00000050: a74d 0508 e807 2b1e af06 07ec de15 0606  .M....+.........
00000060: 9060 7621 5f25 ca07 6f44 4575 3053 c013  .`v!_%..oDEu0S..
00000070: 2424 3e96 8e1e 4ce6 090c 064e a408 d01e  $$>...L....N....
00000080: 3e6b 4f3a 12af 0546 aebf 0732 b036 147c  >kO:...F...2.6.|
00000090: 3a43 0412 e207 27c9 1306 e214 499a 1b08  :C....'.....I...
```

<p></p>
Ok so what can we see here, if we look at the first lines of the two images we can see there is a difference, china.gif has 3 octets of 0's where as working.gif has numbers. This can also be seen in the right hand column with the absence of GIF in china.gif where at it is present in the working.gif file.
<p></p>
This site provides a very good <a href="https://www.file-recovery.com/gif-signature-format.htm" rel="nofollow">Explanation</a> of what is happening with .gif headers/magic numbers.
<p></p>
So now that we know what is wrong with our image how do we fix it? by adding the magic numbers to our broken .gif file using a tool called <kbd>hexedit</kbd> (or any other hex editing tool you may be familiar with).
<p></p>
<details>
    <summary>What is hexedit?</summary>
Hexedit allows you to view and edit files in hexadecimal or in ASCII
<p></p>
Bellow is the basic help file for hexedit:
<p></p>

```
❯ hexedit -h
usage: hexedit [-s | --sector] [-m | --maximize] [-l<n> | --linelength <n>] [--color] [-h | --help] filename
```

<p></p>
Bellow is the list of commands for hexedit:
<p></p>

```
COMMANDS (quickly)
   Moving
       <, > :  go to start/end of the file
       Right:  next character
       Left:   previous character
       Down:   next line
       Up:     previous line
       Home:   beginning of line
       End:    end of line
       PUp:    page forward
       PDown:  page backward

   Miscellaneous
       F2:     save
       F3:     load file
       F1:     help
       Ctrl-L: redraw
       Ctrl-Z: suspend
       Ctrl-X: save and exit
       Ctrl-C: exit without saving

       Tab:    toggle hex/ascii
       Return: go to
       Backspace: undo previous character
       Ctrl-U: undo all
       Ctrl-S: search forward
       Ctrl-R: search backward

Cut&Paste
       Ctrl-Space: set mark
       Esc-W:  copy
       Ctrl-Y: paste
       Esc-Y:  paste into a file
       Esc-I:  fill

```

<p></p>
More information can be attained using man hexedit
</details>
<p></p>
First we need to ensure hexedit is installed:
<p></p>

```
sudo apt install hexedit
```

<p></p>
Now that we have hexedit installed we can use it to add the .gif magic numbers to china.gif using <kbd>F2</kbd> to save before using <kbd>Ctrl + c</kbd> to exit.
<br>
We will be adding "4749 4638 3961" to the first 3 octets and our file should look like this after modification:
<p></p>

```
❯ xxd china.gif | head
00000000: 4749 4638 3961 2003 9001 f700 0000 0000  GIF89a .........
00000010: 0000 3300 0066 0000 9900 00cc 0000 ff00  ..3..f..........
00000020: 2b00 002b 3300 2b66 002b 9900 2bcc 002b  +..+3.+f.+..+..+
00000030: ff00 5500 0055 3300 5566 0055 9900 55cc  ..U..U3.Uf.U..U.
00000040: 0055 ff00 8000 0080 3300 8066 0080 9900  .U......3..f....
00000050: 80cc 0080 ff00 aa00 00aa 3300 aa66 00aa  ..........3..f..
00000060: 9900 aacc 00aa ff00 d500 00d5 3300 d566  ............3..f
00000070: 00d5 9900 d5cc 00d5 ff00 ff00 00ff 3300  ..............3.
00000080: ff66 00ff 9900 ffcc 00ff ff33 0000 3300  .f.........3..3.
00000090: 3333 0066 3300 9933 00cc 3300 ff33 2b00  33.f3..3..3..3+.
```

<p></p>
You can see that GIF89a has now appeared in the right column and if you open the file using file explorer we can now open the image and get the flag.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Flag_Collection/images/china.gif" width="600"><br>
</div>
FLAG{4h34d_0f_th3_curv3}
</details>
</details>
</details>
</details>