<H1>Volatility</H1>
<hr>

<H2>What is volatility?</H2>
<details>
    <summary></summary>
<p>
Volatility is an extremely powerful program that allows us to analyse runtime state of a system using the data found in volatile storage (RAM). 
<br>
Volatility is one of the best open source software programs for analyzing RAM in 32 bit/64 bit systems. It supports analysis for Linux, Windows, Mac, and Android systems. It is based on Python and can be run on Windows, Linux, and Mac systems. It can analyze raw dumps, crash dumps, VMware dumps (.vmem), virtual box dumps, and many others.
</p>
</details>
<hr>
<H2>Installing Volatility</H2>
<details>
<summary></summary>
<p></p>
How to install volatility... currently the volatility website is down so I have uploaded the binary executable (linux) to the github.
<br>
The first thing you need to do is download the executable from <a href="https://github.com/Shadow-Admins/Cyber_Club/blob/06ad32bf1a36fa370da8b7fb637dec66fdcc54df/Starting_Point/DFIR/Memory_Forensics/Volatility/Files/volatility" rel="nofollow">here</a>.
<p></p>
Next you need to give it execute permissions using this command:
<p></p>

```
sudo chmod +x volatility
```

<p></p>
Finally you need to move it to the /bin directory using this command:
<p></p>

```
sudo mv volatility /bin
```

<p></p>
And then check that it is working using this command:
<p></p>

```
sudo volatility -h
```

<p></p>
This will set you up with a working executable of volatility ready for memory analysis.
</details>
<hr>

<H2>Where do I Start?</H2>
<details>
    <summary></summary>
Volatility has a ton of options (flags) that can be used and can be overwhelming at the start.
But once you work out a general road-map you can work through an analysis methodically. Once you have your initial foothold you can start searching for the information you need.
<p></p>
If at any stage you don't know what command to run you can use the <kbd>-h</kbd> flag to print a list of commands you can use. This doesn't show all the commands and doesn't give the best explanation so I would recommend using this <a href="https://github.com/volatilityfoundation/volatility/wiki/Command-Reference" rel="nofollow">site</a> for command reference.
<p></p>
We will use a series of challenges from different CTF's presented as a write ups IOT practice the use of volatility.
</details>
<hr>

<H2>Getting your Foothold</H2>
<details>
    <summary></summary>
Assuming you already have a memory file (we will use 'bob.vmem' for this exercise [you can get this file from challenge A Bob's Life located in challenges] as obtaining a memory file is another lesson) we need to identify what type of system (or profile) the memory is from.

```
sudo volatility -f bob.vmem imageinfo
```

Looking at this command we will unpack it and go through the basic syntax.
<br>
<kbd>sudo</kbd> we will be running the program as a root user.
<br>
<kbd>volatility</kbd> the program we want to run.
<br>
<kbd>-f bob.vmem</kbd> the <kbd>-f</kbd> flag points the program to the file we are looking to analyse.
<br>
<kbd>imageinfo</kbd> is the analysis option we want to use on the file.
<p>
    So what does this command give us?
</p>
<details>
    <summary>Click the <kbd>arrow</kbd> for the answer.</summary>

```
    ❯ sudo volatility -f bob.vmem imageinfo
    Volatility Foundation Volatility Framework 2.6
    INFO    : volatility.debug    : Determining profile based on KDBG search...
              Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_23418
                         AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                         AS Layer2 : FileAddressSpace (/home/parrot/ctf/csc/Preseason/bob.vmem)
                          PAE type : No PAE
                               DTB : 0x187000L
                              KDBG : 0xf80002c080a0L
              Number of Processors : 1
         Image Type (Service Pack) : 1
                    KPCR for CPU 0 : 0xfffff80002c09d00L
                 KUSER_SHARED_DATA : 0xfffff78000000000L
               Image date and time : 2020-08-20 03:34:17 UTC+0000
         Image local date and time : 2020-08-20 13:34:17 +1000
    
```
</details>
<br>
This one command gave us alot of info however we are looking for one specific piece of information, that is the suggested profiles.

```
Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_23418
```

<br>
Most of the time the first profile will be the correct one however if commands aren't working we may need to try another profile.
<br>
This single command is what gives us our foothold to start analysis of the memory file from here our syntax stays basically the same except one flag is added.

```
sudo volatility -f bob.vmem --profile=Win7SP1x64 'Option to use'
```
<br>
The <kbd>--profile=</kbd> flag will always be present now so that volatility knows what OS it is running commands against. I find it easiest to have the <kbd>Options to use</kbd> at the end of the command so that you can easily run different command by pressing the <kbd>up arrow</kbd>.
<br>
We now have a foothold and can begin analysis on the file.
</details>
<hr>

<H2>Challenges</H2>
<p></p>
<details>
    <summary></summary>
<p></p>
For the challenges I recommend you attempt them yourself and only go through the walk throughs when you are stuck. This way you will get used to searching for the commands and try a lot of different commands.
<p></p>
<hr>
<p></p>
<H3>A Bob's Life Challenges</H3>
<p></p>
This series of challenges is from the 2020 Cyber Skills Challenge, these challenges will use the file bob.vmem
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1F0QgNZoBZvIr-0Z3j4B3hdCQT5wvVj4a/view?usp=sharing" rel="nofollow">Google Drive</a>
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
❯ p7zip -d  bob.7z

7-Zip (a) [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_AU.UTF-8,Utf16=on,HugeFiles=on,64 bits,8 CPUs Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz (806EA),ASM,AES-NI)

Scanning the drive for archives:
1 file, 134831177 bytes (129 MiB)

Extracting archive: bob.7z
--
Path = bob.7z
Type = 7z
Physical Size = 134831177
Headers Size = 122
Method = LZMA2:26
Solid = -
Blocks = 1

Everything is Ok

Size:       1073741824
Compressed: 134831177
```

<p></p>
</details>
<details>
    <summary>Challenges</summary>
<p></p>
<details>
    <summary>A Bob's Life 1</summary>
<p></p>
<p>
The first challenge we are given is:
</p>
<p>
    Bob has returned from a lunch break and thinks someone might have tampered with
    <br> 
    his PC but he is usually overly dramatic about these things. Can you make sure 
    <br>
    he actually has a network connection? What is his IPv4 address?
</p>
<p>
    The flag format is  FLAG{IP.Address.Here}
</p>

<details>
    <summary>Walkthrough</summary>
<p></p>
<p>
    So what information can we pull from the hint?
<P>
    <details>
        <summary>Spoilers</summary>
<p></p>
            - We are going to have to look for network related analysis options
    </details>
</p>
<hr>
<p>
Now that we know what we are looking for what are the possible options we can use?
</p>
<details>
    <summary>Spoilers</summary>

<p></p>

Option | Description
------ | -----------------------------------------------------------------
connections | To view TCP connections that were active at the time of the memory acquisition, use the connections command. This walks the singly-linked list of connection structures pointed to by a non-exported symbol in the tcpip.sys module.<br><b><u>This command is for x86 and x64 Windows XP and Windows 2003 Server only.</u></b>
connscan | To find _TCPT_OBJECT structures using pool tag scanning, use the connscan command. This can find artifacts from previous connections that have since been terminated, in addition to the active ones. For example, a Pid field may be 0 (not valid), but all other fields are still in tact. Thus, while it may find false positives sometimes, you also get the benefit of detecting as much information as possible.<br><b><u>This command is for x86 and x64 Windows XP and Windows 2003 Server only.</u></b>
sockets | To detect listening sockets for any protocol (TCP, UDP, RAW, etc), use the sockets command. This walks a singly-linked list of socket structures which is pointed to by a non-exported symbol in the tcpip.sys module.<br><u><b>This command is for x86 and x64 Windows XP and Windows 2003 Server only.</b></u>
sockscan | To find _ADDRESS_OBJECT structures using pool tag scanning, use the sockscan command. As with connscan, this can pick up residual data and artifacts from previous sockets.<br><b><u>This command is for x86 and x64 Windows XP and Windows 2003 Server only.</u></b>
netscan | To scan for network artifacts in 32- and 64-bit Windows Vista, Windows 2008 Server and Windows 7 memory dumps, use the netscan command. This finds TCP endpoints, TCP listeners, UDP endpoints, and UDP listeners. It distinguishes between IPv4 and IPv6, prints the local and remote IP (if applicable), the local and remote port (if applicable), the time when the socket was bound or when the connection was established, and the current state (for TCP connections only). <p>There are at least 2 alternate ways to enumerate connections and sockets on Vista+ operating systems. One of them is using partitions and dynamic hash tables, which is how the netstat.exe utility on Windows systems works. The other involves bitmaps and port pools.</p>


<p>
Looking at our 5 options we see that netscan is the only one that will work because the other 4 will not work with the memory profile we have.
</p>
</details>

<hr>
<br>

We can now run that command.

<details>
    <summary>Spoilers</summary>
<br>
The command we will run is:

```
sudo volatility -f bob.vmem --profile=Win7SP1x64 netscan
```

<br>
Which gives us:
<br>

```
❯ sudo volatility -f bob.vmem --profile=Win7SP1x64 netscan                                                                                                                                    
[sudo] password for parrot:                                                                                                                                                                   
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x3e07f530         UDPv4    0.0.0.0:52906                  *:*                                   952      svchost.exe    2020-08-20 03:34:17 UTC+0000                                         
0x3e07f530         UDPv6    :::52906                       *:*                                   952      svchost.exe    2020-08-20 03:34:17 UTC+0000                                         
0x3e0f2ec0         UDPv6    fe80::4919:a193:81e5:6027:546  *:*                                   740      svchost.exe    2020-08-20 03:34:06 UTC+0000                                         
0x3e10a5e0         UDPv4    127.0.0.1:59815                *:*                                   2808     iexplore.exe   2020-08-20 03:30:31 UTC+0000                                         
0x3e10da00         UDPv4    127.0.0.1:64777                *:*                                   2728     iexplore.exe   2020-08-20 03:30:31 UTC+0000                                         
0x3e21e550         UDPv4    192.168.128.130:138            *:*                                   4        System         2020-08-20 03:34:10 UTC+0000                                         
0x3e4cd2e0         UDPv4    127.0.0.1:58862                *:*                                   1700     iexplore.exe   2020-08-20 03:31:27 UTC+0000                                         
0x3e5e6960         UDPv4    0.0.0.0:5355                   *:*                                   952      svchost.exe    2020-08-20 03:34:17 UTC+0000                                         
0x3e6676a0         UDPv4    0.0.0.0:5355                   *:*                                   952      svchost.exe    2020-08-20 03:34:17 UTC+0000                                         
0x3e6676a0         UDPv6    :::5355                        *:*                                   952      svchost.exe    2020-08-20 03:34:17 UTC+0000                                         
0x3e702010         UDPv4    0.0.0.0:68                     *:*                                   740      svchost.exe    2020-08-20 03:34:06 UTC+0000                                         
0x3e7256b0         UDPv4    0.0.0.0:65018                  *:*                                   952      svchost.exe    2020-08-20 03:34:17 UTC+0000                                         
0x3e270160         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        500      lsass.exe                                                                           
0x3e3806a0         TCPv4    192.168.128.130:139            0.0.0.0:0            LISTENING        4        System                                                                              
0x3e4d46d0         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        484      services.exe                                                                        
0x3e4d46d0         TCPv6    :::49155                       :::0                 LISTENING        484      services.exe   
0x3e4d52b0         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        484      services.exe   
0x3e60c440         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        740      svchost.exe    
0x3e60d010         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        740      svchost.exe    
0x3e60d010         TCPv6    :::49153                       :::0                 LISTENING        740      svchost.exe    
0x3e72d6f0         TCPv4    0.0.0.0:445                    0.0.0.0:0            LISTENING        4        System         
0x3e72d6f0         TCPv6    :::445                         :::0                 LISTENING        4        System         
0x3e755820         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        880      svchost.exe    
0x3e755820         TCPv6    :::49154                       :::0                 LISTENING        880      svchost.exe    
0x3e11f9d0         TCPv4    192.168.128.130:49188          104.18.102.194:443   CLOSED           2808     iexplore.exe   
0x3e1521b0         TCPv4    192.168.128.130:49160          111.221.29.254:443   CLOSED           2808     iexplore.exe   
0x3e1a9880         TCPv4    192.168.128.130:49177          204.79.197.200:443   CLOSED           2808     iexplore.exe   
0x3e1d4520         TCPv4    192.168.128.130:49173          144.2.0.5:80         CLOSED           2808     iexplore.exe   
0x3e1d9660         TCPv4    192.168.128.130:49170          111.221.29.254:443   CLOSED           -1                      
0x3e9b66b0         UDPv4    192.168.128.130:137            *:*                                   4        System         2020-08-20 03:34:10 UTC+0000
0x3eaf7260         UDPv4    192.168.128.130:68             *:*                                   740      svchost.exe    2020-08-20 03:34:13 UTC+0000
0x3e8a8890         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        688      svchost.exe    
0x3e8c2230         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        688      svchost.exe    
0x3e8c2230         TCPv6    :::135                         :::0                 LISTENING        688      svchost.exe    
0x3e94b1c0         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        384      wininit.exe    
0x3e9d5c90         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        384      wininit.exe    
0x3e9d5c90         TCPv6    :::49152                       :::0                 LISTENING        384      wininit.exe    
0x3e9da4a0         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        500      lsass.exe      
0x3e9da4a0         TCPv6    :::49156                       :::0                 LISTENING        500      lsass.exe      
0x3e9cd540         TCPv6    -:0                            68b0:3a02:80fa:ffff:68b0:3a02:80fa:ffff:0 CLOSED           1        ??0????       
0x3e9cdaf0         TCPv4    -:0                            104.176.58.2:0       CLOSED           424                     
0x3e9cf860         TCPv6    -:0                            68b0:3a02:80fa:ffff:68b0:3a02:80fa:ffff:0 CLOSED           1        ??0????       
0x3f2e80e0         UDPv4    0.0.0.0:0                      *:*                                   952      svchost.exe    2020-08-20 03:34:10 UTC+0000
0x3f2e80e0         UDPv6    :::0                           *:*                                   952      svchost.exe    2020-08-20 03:34:10 UTC+0000
0x3f20f760         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        880      svchost.exe    
```
<br>
Looking through this output we are looking for an IPv4 address and are looking for the local side.
<br>
We should also make sure the it is "LISTENING" and not "CLOSED", and finally we need to see what the "Owner" is.
<br>
What we end up finding is:
<p>

<details>
    <summary>Answer</summary>

```
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                         
0x3e3806a0         TCPv4    192.168.128.130:139            0.0.0.0:0            LISTENING        4        System
```

<br>
From this we see Bob has an internal IPv4 address of <kbd>192.168.128.130</kbd> the <kbd>:139</kbd> is the port that the connection is on.
<br>
Giving us the answer of: <kbd>192.168.128.130</kbd>
<br>
FLAG{192.168.128.130}
</details>
</details>
</details>

</details>

<hr>

<details>
    <summary>A Bob's Life 2</summary>
<p></p>
The second challenge we are given is:
<P></P>
So much for being air gapped. Maybe someone did use his machine after
<br>
all? Bob keeps notes in his "Blog", Can you make sure no one has found it?
<p></p>

<details>
    <summary>Walkthrough</summary>
<p></p>

So what information can we pull from the hint?
<p></p>

<details>
    <summary>Spoilers</summary>
<p></p>
- We are looking for something called "Blog"
<br>
- We need to make sure it hasn't been changed.
<br>
- thinking about blogs they are usually located on the internet
<p></p>
<p>So with that information we can start our analysis.</p>
</details>

<hr>

<p></p>
Now that we know what we are looking for what are the possible options we can use?
<p>
<details>
    <summary>Spoilers</summary>
<p></p>

Options | Description
--------|-----------------------------
iehistory | This plugin recovers fragments of IE history index.dat cache files. It can find basic accessed links (via FTP or HTTP), redirected links (--REDR), and deleted entries (--LEAK). It applies to any process which loads and uses the wininet.dll library, not just Internet Explorer. Typically that includes Windows Explorer and even malware samples.

<p>
This plugin will let us look at the internet history, you can look for files on the system called blog however this wont return anything.



</details>
<p>
We can now run that command.
<p>
<details>
    <summary>Spoilers/Answer</summary>
<p>
The command we will run is:

```
sudo volatility -f bob.vmem --profile=Win7SP1x64 iehistory
```
which gives us:

```
❯ sudo volatility -f bob.vmem --profile=Win7SP1x64 iehistory
Volatility Foundation Volatility Framework 2.6
**************************************************
Process: 1328 explorer.exe
Cache type "DEST" at 0x423ed3
Last modified: 2020-08-20 13:31:21 UTC+0000
Last accessed: 2020-08-20 03:31:22 UTC+0000
URL: Bob@http://192.168.128
**************************************************
Process: 1328 explorer.exe
Cache type "DEST" at 0x47c25d3
Last modified: 2020-08-20 13:34:07 UTC+0000
Last accessed: 2020-08-20 03:34:08 UTC+0000
URL: Bob@file:///C:/Users/Bob/Desktop/hacked.gif
**************************************************
Process: 2808 iexplore.exe
Cache type "DEST" at 0x73a07bb
Last modified: 2020-08-20 13:31:21 UTC+0000
Last accessed: 2020-08-20 03:31:22 UTC+0000
URL: Bob@http://192.168.128.128:8000/SecretBlog.html
Title: Bob's Secret Blog - FLAG{encoded_secrets}
```
<p>
Looking through the output we can see:

```
Process: 2808 iexplore.exe
Cache type "DEST" at 0x73a07bb
Last modified: 2020-08-20 13:31:21 UTC+0000
Last accessed: 2020-08-20 03:31:22 UTC+0000
URL: Bob@http://192.168.128.128:8000/SecretBlog.html
Title: Bob's Secret Blog - FLAG{encoded_secrets}
```

<p>
Giving us the flag.
<p>
FLAG{encoded_secrets}
<p></p>
There is an alternate way to do this without using the <kbd>iehistory</kbd> option, the alternate way is the same as the method for A Bob's Life 3.

</details>
</details>
</details>
<hr>

<details>
    <summary>A Bob's Life 3</summary>
<p></p>
The third challenge we are given is:
<p></p>
This could be serious. It looks like the attacker left something for
<br>
Bob on his Desktop. Was this someone sinister? Or some kind of prank? Can you take
<br>
a look at it?
<p></p>

<details>
    <summary>Walkthrough</summary>

<p></p>
So what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- We need to use an option to look at what is on the desktop.
<p></p>
So with that information we can start our analysis.
</details>

<hr>

Now that we know what we are looking for what options can we use?
<p></p>

<details>
    <summary>Spoilers</summary>
<p></p>
Looking at our options there are a few steps we need to go through.
<p></p>
First.
<br>
We need to locate all the files on the desktop.
<br>
Second.
<br>
We need to either read the file through volatility or we need to dump it onto our system.
<p></p>

Options | Description
--------|----------------
filescan | To find FILE_OBJECTs in physical memory using pool tag scanning, use the filescan command. This will find open files even if a rootkit is hiding the files on disk and if the rootkit hooks some API functions to hide the open handles on a live system. The output shows the physical offset of the FILE_OBJECT, file name, number of pointers to the object, number of handles to the object, and the effective permissions granted to the object.
dumpfiles | An important concept that every computer scientist, especially those who have spent time doing operating system research, is intimately familiar with is that of caching. Files are cached in memory for system performance as they are accessed and used. This makes the cache a valuable source from a forensic perspective since we are able to retrieve files that were in use correctly, instead of file carving which does not make use of how items are mapped in memory. Files may not be completely mapped in memory (also for performance), so missing sections are zero padded. Files dumped from memory can then be processed with external tools.

<p></p>
Now that we know what commands we can run we will look at some of the problems we will run into.

</details>
<p></p>
We can now run those commands.
<p></p>
<details>
    <summary>Spoilers</summary>

<p></p>
The first command we will run is <kbd>filescan</kbd>
<p></p>

``` shell
sudo volatility -f bob.vmem --profile=Win7SP1x64 filescan
```

<p></p>
However we are presented with a ton of output, we could look through line by line or we can use a built in linux tool <kbd>grep</kbd> to narrow down our results.
<p></p>

```
sudo volatility -f bob.vmem --profile=Win7SP1x64 filescan | grep -i '\desktop'
```

<p></p>
Which gives us:
<p></p>

```
❯ sudo volatility -f bob.vmem --profile=Win7SP1x64 filescan | grep -i '\Desktop'                                                                                                              
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
0x000000003e0cb360      1      1 R--rw- \Device\HarddiskVolume1\Users\Bob\Desktop                                                                                                             
0x000000003e0de070     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Favorites\desktop.ini                                                                                               
0x000000003e0eb6a0      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Favorites\Links\desktop.ini                                                                                         
0x000000003e1688e0      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Contacts\desktop.ini                                                                                                
0x000000003e179070     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Local\Microsoft\Windows\History\desktop.ini                                                                 
0x000000003e1b08a0      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Videos\desktop.ini                                                                                                  
0x000000003e1d34d0      1      1 R--rw- \Device\HarddiskVolume1\Users\Bob\Desktop                                                                                                             
0x000000003e1d8070      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Searches\desktop.ini                                                                                                
0x000000003e1dd2d0      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Saved Games\desktop.ini                                                                                             
0x000000003e1dd9e0      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Downloads\desktop.ini                                                                                               
0x000000003e1dedd0      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Links\desktop.ini                                                                                                   
0x000000003e2cca40      2      1 R--rwd \Device\HarddiskVolume1\Users\Bob\Desktop                                                                                                             
0x000000003e2eee80      2      0 R--rwd \Device\HarddiskVolume1\Program Files\desktop.ini                                                                                                     
0x000000003e329f20      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Start Menu\desktop.ini                                                            
0x000000003e3416f0      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Local\Microsoft\Windows\Burn\Burn\desktop.ini                                                               
0x000000003e342dd0     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Desktop\desktop.ini                                                                                                 
0x000000003e3435a0     16      0 R--rwd \Device\HarddiskVolume1\Users\Public\desktop.ini                                                                                                      
0x000000003e344660     16      0 R--rwd \Device\HarddiskVolume1\Users\Public\Desktop\desktop.ini
0x000000003e347070      2      1 R--rwd \Device\HarddiskVolume1\Users\Public\Desktop
0x000000003e347b70      2      1 R--rwd \Device\HarddiskVolume1\Users\Public\Desktop
0x000000003e348f20      2      1 R--rwd \Device\HarddiskVolume1\Users\Bob\Desktop
0x000000003e349dc0      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch\desktop.ini
0x000000003e354ab0      2      0 R--rwd \Device\HarddiskVolume1\Program Files (x86)\desktop.ini
0x000000003e35a070     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\desktop.ini
0x000000003e35af20     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\desktop.ini
0x000000003e35b740     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\desktop.ini
0x000000003e35cdd0     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Accessories\Desktop.ini
0x000000003e35d070     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Administrative Tools\desktop.ini
0x000000003e35d960     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Maintenance\Desktop.ini
0x000000003e35e070     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\desktop.ini
0x000000003e35e810     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Accessories\Accessibility\Desktop.ini
0x000000003e35fd10     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Accessories\System Tools\Desktop.ini
0x000000003e360290     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\Games\desktop.ini
0x000000003e360b90     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\Accessories\Desktop.ini
0x000000003e361dd0     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\Administrative Tools\desktop.ini
0x000000003e3621f0     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\Maintenance\Desktop.ini
0x000000003e362650     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\Accessories\Tablet PC\Desktop.ini
0x000000003e363260     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\Accessories\System Tools\Desktop.ini
0x000000003e364b30     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\Accessories\Accessibility\Desktop.ini
0x000000003e366f20     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\Accessories\Windows PowerShell\desktop.ini
0x000000003e36d7e0      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch\User Pinned\TaskBar\desktop.ini
0x000000003e37f070      2      0 R--rw- \Device\HarddiskVolume1\Users\Public\Desktop\Google Chrome.lnk
0x000000003e383920      2      0 R--rwd \Device\HarddiskVolume1\$Recycle.Bin\S-1-5-21-2085463377-1443911893-2077917805-1000\desktop.ini
0x000000003e38a290      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Libraries\desktop.ini
0x000000003e39af20     16      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Documents\desktop.ini
0x000000003e39c9e0     16      0 R--rwd \Device\HarddiskVolume1\Users\Public\Documents\desktop.ini
0x000000003e39cf20      2      0 R--rwd \Device\HarddiskVolume1\Users\Public\Pictures\desktop.ini
0x000000003e39d300      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Music\desktop.ini
0x000000003e39da80      2      0 R--rwd \Device\HarddiskVolume1\Users\Public\Music\desktop.ini
0x000000003e39ef20      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\Pictures\desktop.ini
0x000000003e3b74b0      6      0 R--r-d \Device\HarddiskVolume1\Program Files\VMware\VMware Tools\plugins\vmusr\desktopEvents.dll
0x000000003e3c4860      2      0 R--rw- \Device\HarddiskVolume1\ProgramData\VMware\VMware Tools\Unity Filters\googledesktop.txt
0x000000003e465070     16      0 R--rwd \Device\HarddiskVolume1\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\desktop.ini
0x000000003e5b34d0     16      0 R--rwd \Device\HarddiskVolume1\Users\desktop.ini
0x000000003e6b2a50      1      1 R--rw- \Device\HarddiskVolume1\Users\Bob\Desktop
0x000000003e895480      2      0 R--rwd \Device\HarddiskVolume1\Users\Bob\AppData\Roaming\Microsoft\Windows\Recent\desktop.ini
0x000000003e8cbd10      1      1 R--rw- \Device\HarddiskVolume1\Users\Bob\Desktop
0x000000003e9b6550     16      0 R--rw- \Device\HarddiskVolume1\Users\Bob\Desktop\hacked.gif

```
<p></p>
That gives us a much smaller list, and a file should stick out...
<p></P>

```
0x000000003e9b6550     16      0 R--rw- \Device\HarddiskVolume1\Users\Bob\Desktop\hacked.gif
```

<p></p>
So we can see the file but how do we access it?
<br>
The first thing we need to do is make a new directory (im calling mine out) so that we can dump the file onto our system
<p></p>

```
mkdir out
```
<p></p>

And now we'll use the dumpfiles option. It does come with more flags that aren't shown when you use <kbd>-h</kbd> so I'll post them bellow.
<p></p>

```
  -r REGEX, --regex=REGEX
                        Dump files matching REGEX
  -i, --ignore-case     Ignore case in pattern match
  -o OFFSET, --offset=OFFSET
                        Dump files for Process with physical address OFFSET
  -Q PHYSOFFSET, --physoffset=PHYSOFFSET
                        Dump File Object at physical address PHYSOFFSET
  -D DUMP_DIR, --dump-dir=DUMP_DIR
                        Directory in which to dump extracted files
  -S SUMMARY_FILE, --summary-file=SUMMARY_FILE
                        File where to store summary information
  -p PID, --pid=PID     Operate on these Process IDs (comma-separated)
  -n, --name            Include extracted filename in output file path
  -u, --unsafe          Relax safety constraints for more data
  -F FILTER, --filter=FILTER
                        Filters to apply (comma-separated)
```
<p></p>
We will use the <kbd>-Q</kbd> <kbd>-D</kbd><kbd>-n</kbd> flags. But what goes where?
<br>
Thinking back to the hacked.gif file we found we need to get one piece of information and that is the physical offset (if you want to know more about physical and virtual offsets I'd recommend google). The Physical offset is the number in the first column being <kbd>0x000000003e9b6550</kbd> the next thing we need is where we want to dump the file to <kbd>out/</kbd>. So what does this command look like?

<p></p>

```
sudo volatility -f bob.vmem --profile=Win7SP1x64 dumpfiles -D out/ -n -Q 0x000000003e9b6550
```

<p></p>
Running it we get:
<p></p>

```
❯ sudo volatility -f bob.vmem --profile=Win7SP1x64 dumpfiles -D out/ -n -Q 0x000000003e9b6550

Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x3e9b6550   None   \Device\HarddiskVolume1\Users\Bob\Desktop\hacked.gif
```

<p></p>
Not much information but if we look at the file in our file explorer we can see that it has dumped the .gif image which we can view and get the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/file.None.0xfffffa80023b6d00.hacked.gif.dat" width="600"><br>
</div>
<p></p>
Giving us the flag: FLAG{my_name_gif}
</details>
</details>
</details>
</details>
<hr>
<p></p>
<details>
    <summary>A Bob's Life 4</summary>
<p></p>
The fourth challenge we are given is:
<p></p>
How did the attacker get Bob's password? He insists he has ALWAYS had very 
<br>
secure passwords. Let's find out. Can you check what was the default password 
<br>
Bob used when he set up his computer?
<p></p>

<details>
    <summary>Walkthrough</summary>

<p></p>
So what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- We need to find the systems 'default password' from initial set up.
<p></p>
With that information we can start our analysis.
</details>
<hr>
<p></p>
Now that we know what we are looking for what options can we use?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
Looking at our options there are two aimed at password recovery.
<p></p>

Options | Details
--------|----------
hashdump | To extract and decrypt cached domain credentials stored in the registry, use the hashdump command.
lsadump | To dump LSA secrets from the registry, use the lsadump command. This exposes information such as the default password (for systems with autologin enabled), the RDP public key, and credentials used by DPAPI.

<p></p>
Looking at both we can see that the <kbd>lsadump</kbd> is optimal for our scenario as it dumps the default password.
<p></p>
Now that we know that we can start our analysis.
</details>
<p></p>
We can now run that command.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
The command we will run is:
<p></p>

```
sudo volatility -f bob.vmem --profile=Win7SP1x64 lsadump
```

<p></p>
Which gives us:
<p></p>

```
❯ sudo volatility -f bob.vmem --profile=Win7SP1x64 lsadump

Volatility Foundation Volatility Framework 2.6
DefaultPassword
0x00000000  10 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x00000010  57 00 65 00 6c 00 63 00 6f 00 6d 00 65 00 32 00   W.e.l.c.o.m.e.2.
0x00000020  c6 f2 ea 0c 76 c8 ef f0 fd de f8 3b 0d cd 25 c7   ....v......;..%.

NL$KM
0x00000000  40 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   @...............
0x00000010  94 bb 72 9a 14 c7 4a 92 85 6c d7 7f b9 67 e7 d4   ..r...J..l...g..
0x00000020  05 25 b8 87 f0 1c ac 3c 23 84 4a a9 45 71 e1 f0   .%.....<#.J.Eq..
0x00000030  36 59 a9 ff 26 2a cd 80 f1 79 27 83 11 18 40 5c   6Y..&*...y'...@\
0x00000040  c7 29 15 d3 75 da ca 0d 4e f0 51 41 14 08 d8 b7   .)..u...N.QA....
0x00000050  42 5f 1a 93 50 c7 67 cd ca f1 03 3a 6e 00 9f 75   B_..P.g....:n..u

DPAPI_SYSTEM
0x00000000  2c 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ,...............
0x00000010  01 00 00 00 f0 7a 35 76 16 46 e9 84 87 ee d3 a9   .....z5v.F......
0x00000020  f4 f4 46 91 89 c7 a3 9d 9b a9 78 93 8c 71 e8 f1   ..F.......x..q..
0x00000030  0b 6d 43 76 f7 3d f9 d5 9d ef 15 b8 00 00 00 00   .mCv.=..........

```

<p></p>

We can see that the default password has been dumped.

```
DefaultPassword
0x00000000  10 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00   ................
0x00000010  57 00 65 00 6c 00 63 00 6f 00 6d 00 65 00 32 00   W.e.l.c.o.m.e.2.
0x00000020  c6 f2 ea 0c 76 c8 ef f0 fd de f8 3b 0d cd 25 c7   ....v......;..%.
```

<p></p>
<details>
    <summary>Answer</summary>
<p></p>
Giving us the flag: FLAG{Welcome2}
</details>
</details>
</details>
</details>
<hr>
<p></p>

<details>
    <summary>A Bob's Life 5</summary>
<p></p>
The fifth challenge we're given is:
<p></p>
Well that doesn't look like a very secure password, Bob. Let's hope he has improved since then.
<br>
Can you check what his current password is?
<p></p>
Hint: Hashtags crack me up.
<p></p>

<details>
    <summary>Walkthrough</summary>

<p></p>
So what can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- We need to find the current password.
<br>
- Windows passwords are stored as NTLM hashes within the SAM file in System32.
</details>
<hr>
<p></p>
Now that we know what we are looking for what options can we use?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
From our last challenge we can now use the hashdump option but we need to use one other option first.
<p></p>

Options | Description
--------|-------------
hivelist | To locate the virtual addresses of registry hives in memory, and the full paths to the corresponding hive on disk, use the hivelist command. If you want to print values from a certain hive, run this command first so you can see the address of the hives.
hashdump | To extract and decrypt cached domain credentials stored in the registry, use the hashdump command. To use hashdump, pass the virtual address of the SYSTEM hive as <kbd>-y</kbd> and the virtual address of the SAM hive as <kbd>-s</kbd>.

<p></p>
Now that we know the commands we need to use we can start our analysis.
</details>
<p></p>
We can now run those commands.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
The first command we will run is:
<p></p>

```
sudo volatility -f bob.vmem --profile=Win7SP1x64 hivelist
```

<p></P>
Which gives us:
<p></p>

```
❯ sudo volatility -f bob.vmem --profile=Win7SP1x64 hivelist

Volatility Foundation Volatility Framework 2.6
Virtual            Physical           Name
------------------ ------------------ ----
0xfffff8a00000f010 0x0000000026fc1010 [no name]
0xfffff8a000024010 0x000000002714c010 \REGISTRY\MACHINE\SYSTEM
0xfffff8a000057010 0x000000002747f010 \REGISTRY\MACHINE\HARDWARE
0xfffff8a0008fc010 0x00000000210b3010 \Device\HarddiskVolume1\Boot\BCD
0xfffff8a001553010 0x0000000020440010 \SystemRoot\System32\Config\SOFTWARE
0xfffff8a00177f010 0x000000001e852010 \SystemRoot\System32\Config\SECURITY
0xfffff8a0017fb010 0x000000001e2fb010 \SystemRoot\System32\Config\SAM
0xfffff8a0018d1010 0x000000001ddd8010 \??\C:\Windows\ServiceProfiles\NetworkService\NTUSER.DAT
0xfffff8a001959410 0x000000003bba7410 \??\C:\Windows\ServiceProfiles\LocalService\NTUSER.DAT
0xfffff8a001ef2010 0x000000000f7b7010 \??\C:\Users\Bob\ntuser.dat
0xfffff8a001f02010 0x0000000009907010 \??\C:\Users\Bob\AppData\Local\Microsoft\Windows\UsrClass.dat
0xfffff8a004342010 0x000000001fbbf010 \SystemRoot\System32\Config\DEFAULT
```

<p></p>
This gives us some important information we need for our next command being the virtual addresses of the SYSTEM and SAM hives. Now that we have this information we can dump the NTLM hashes IOT get the password.
<p></p>

```
Virtual            Physical           Name
------------------ ------------------ ----
0xfffff8a000024010 0x000000002714c010 \REGISTRY\MACHINE\SYSTEM
```

<p></p>

```
Virtual            Physical           Name
------------------ ------------------ ----
0xfffff8a0017fb010 0x000000001e2fb010 \SystemRoot\System32\Config\SAM
```

<p></p>
Now that we know what the SYSTEM virtual address is <kbd>-y 0xfffff8a000024010</kbd> and the SAM virtual address <kbd>-s 0xfffff8a0017fb010</kbd> the next command we will run is:
<p></p>

```
sudo volatility -f bob.vmem --profile=Win7SP1x64 hashdump -y 0xfffff8a000024010 -s 0xfffff8a0017fb010
```
<p></p>
Which gives us:
<p></p>

```
❯ sudo volatility -f bob.vmem --profile=Win7SP1x64 hashdump -y 0xfffff8a000024010 -s 0xfffff8a0017fb010

Volatility Foundation Volatility Framework 2.6
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Bob:1000:aad3b435b51404eeaad3b435b51404ee:21bc7dcd88ee195ecf3728677a47815b:::
```

<p></p>
So we can see that the hashes are there but we cant understand them so we will need to use another program to decrypt the hash, enter john the ripper.
<p></p>
To enable us to decrypt the hash we will need to put them into a file the easiest way to do this is to append them into a text file when we run volitility like this:
<p></p>

```
sudo volatility -f bob.vmem --profile=Win7SP1x64 hashdump -y 0xfffff8a000024010 -s 0xfffff8a0017fb010 > hash.hash
```

<p></p>
We now have a file that we can pass to john IOT crack the hashes. Using john is another lesson but you can see all the flags by using the <kbd>-h</kbd> flag.
<br>
The command we will use is:
<p></p>

```
sudo john --format=NT --show hash.hash
```

<p></p>
This gives us:
<p></p>

```
❯ sudo john --format=NT hash.hash                                                                                                                                                             
Using default input encoding: UTF-8                                                                                                                                                           
Loaded 3 password hashes with no different salts (NT [MD4 256/256 AVX2 8x3])                                                                                                                  
Warning: no OpenMP support for this hash type, consider --fork=4                                                                                                                              
Proceeding with single, rules:Single                                                                                                                                                          
Press 'q' or Ctrl-C to abort, almost any other key for status                                                                                                                                 
Warning: Only 10 candidates buffered for the current salt, minimum 24 needed for performance.                                                                                                 
Warning: Only 9 candidates buffered for the current salt, minimum 24 needed for performance.                                                                                                  
Almost done: Processing the remaining buffered candidate passwords, if any.                                                                                                                   
Warning: Only 22 candidates buffered for the current salt, minimum 24 needed for performance.                                                                                                 
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist                                                                                                                         
                 (Administrator)                                                                                                                                                              
                 (Guest)                                                                                                                                                                      
Hunter2          (Bob)                                                                                                                                                                        
3g 0:00:00:00 DONE 2/3 (2021-04-15 17:15) 100.0g/s 2968Kp/s 2968Kc/s 3158KC/s Pentium2..Pizza2                                                                                                
Use the "--show --format=NT" options to display all of the cracked passwords reliably                                                                                                         
Session completed              
```

<p></p>
It may also show:
<p></p>

```
❯ sudo john --format=NT --show hash2.hash
Administrator::500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest::501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Bob:Hunter2:1000:aad3b435b51404eeaad3b435b51404ee:21bc7dcd88ee195ecf3728677a47815b:::

3 password hashes cracked, 0 left
```

<p></p>
In both outputs we can see the password we are looking for.
<p></p>
<details>
    <summary>Answer</summary>
The password is Hunter2 so the flag is: FLAG{Hunter2}
</details>
</details>
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<H3>Memory is RAM</H2>
<p></p>
This series of challenges is from the 2020 Cyber Skills Challenge, These challenges will use the file dump.vmem
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1qCShCAP7KE2y1rGR0-b2j3lT2vLa4QG5/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
<details>
    <summary>Challenges</summary>
<p></p>
<details>
    <summary>Starting Point</summary>
<p></p>
When you download it the file comes as dump.7z you can either extract it through an archive manager or through CLI which I will explain now.
<p></p>
The first thing you will need is to install p7zip-full if you haven't already. To do this run this command:
<p></p>

```
sudo apt install p7zip-full
```

Now that's installed you can run this command on the file:

```
p7zip -d dump.7z
```

Which will output this and extract dump.vmem:

```
❯ p7zip -d dump.7z

7-Zip (a) [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_AU.UTF-8,Utf16=on,HugeFiles=on,64 bits,8 CPUs Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz (806EA),ASM,AES-NI)

Scanning the drive for archives:
1 file, 479938046 bytes (458 MiB)

Extracting archive: dump.7z
--
Path = dump.7z
Type = 7z
Physical Size = 479938046
Headers Size = 122
Method = LZMA2:20
Solid = -
Blocks = 1

Everything is Ok

Size:       2147483648
Compressed: 479938046

```

<p></p>
Once we determine the image profile using <kbd>imageinfo</kbd> (refer to Getting your Foothold ) we can begin our analysis on dump.vmem
<p></p>
</details>
<p></p>
<hr>
<p></p>

<details>
    <summary>Memory is RAM 1</summary>
<p></p>
The first challenge we are given is:
<p></p>
We've recovered a memory dump of an attackers VM and need you to perform
<br>
some analysis. First up, what VPN provider was the attacker using?
<p></p>
Flag Format: FLAG{somevpncompany} (provider name with no whitespace)
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
So what information can we pull from the hint?
<p></p>

<details>
    <summary>Spoilers</summary>
<p></p>
- What is a VPN? A virtual private network (VPN) gives you online privacy and anonymity by creating a private network from a public internet connection. VPNs mask your internet protocol (IP) address so your online actions are virtually untraceable. Most important, VPN services establish secure and encrypted connections to provide greater privacy than even a secured Wi-Fi hotspot.
  <br>
- So from that we can conclude that a VPN has something to do with the internet and will most likely be a program or process running locally on the system IOT create the tunnel.
<p></p>
With that information we can begin our analysis.
</details>
<hr>
<p></p>
Now that we know what we are looking for what are the possible options we can use?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>

Options | Description
--------|-------------
pslist | To list the processes of a system, use the <kbd>pslist</kbd> command. This walks the doubly-linked list pointed to by <kbd>PsActiveProcessHead</kbd> and shows the offset, process name, process ID, the parent process ID, number of threads, number of handles, and date/time when the process started and exited. <p></p> This plugin does not detect hidden or unlinked processes (but psscan can do that). <p></p> If you see processes with 0 threads, 0 handles, and/or a non-empty exit time, the process may not actually still be active. <p></p> Also note the two processes <kbd>System</kbd> and <kbd>smss.exe</kbd> will not have a Session ID, because System starts before sessions are established and <kbd>smss.exe</kbd> is the session manager itself.
psscan | To enumerate processes using pool tag scanning (<kbd>_POOL_HEADER</kbd>), use the <kbd>psscan</kbd> command. This can find processes that previously terminated (inactive) and processes that have been hidden or unlinked by a rootkit. The downside is that rootkits can still hide by overwriting the pool tag values (though not commonly seen in the wild).
pstree | To view the process listing in tree form, use the <kbd>pstree</kbd> command. This enumerates processes using the same technique as <kbd>pslist</kbd>, so it will also not show hidden or unlinked processes. Child process are indicated using indention and periods.
cmdline | This will output all process command-line outputs.

<p></p>
All of these will show us information that will be usefull, so now that we know which options we can use we can start our analysis.
<p></p>
We can now run those commands.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
While all commands will give us the answer they all give slightly different outputs, lets look at them now.
<p></p>
First we will look at <kbd>pslist</kbd>.
<p></p>
The command we will run is:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 pslist
```

<p></p>

Which gives us:

<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 pslist                                                                                                                           [30/4523]
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                                                                       
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------                                             
0xfffffa80018b0960 System                    4      0     97      541 ------      0 2020-08-02 02:21:39 UTC+0000
0xfffffa80031c75d0 smss.exe                272      4      2       30 ------      0 2020-08-02 02:21:39 UTC+0000
0xfffffa8003c2f060 csrss.exe               360    344      9      555      0      0 2020-08-02 02:21:40 UTC+0000
0xfffffa800350d060 wininit.exe             400    344      3       81      0      0 2020-08-02 02:21:41 UTC+0000
0xfffffa8003df0b00 csrss.exe               412    392     11      558      1      0 2020-08-02 02:21:41 UTC+0000
0xfffffa8003e679a0 winlogon.exe            456    392      3      117      1      0 2020-08-02 02:21:41 UTC+0000
0xfffffa8003db3b00 services.exe            504    400      7      214      0      0 2020-08-02 02:21:41 UTC+0000
0xfffffa8003e03b00 lsass.exe               512    400      9      643      0      0 2020-08-02 02:21:41 UTC+0000
0xfffffa8003ea8b00 lsm.exe                 524    400     11      167      0      0 2020-08-02 02:21:41 UTC+0000
0xfffffa8003fd49c0 svchost.exe             644    504     12      375      0      0 2020-08-02 02:21:42 UTC+0000
0xfffffa800403e2f0 svchost.exe             716    504      9      317      0      0 2020-08-02 02:21:42 UTC+0000
0xfffffa8003fdcb00 svchost.exe             784    504     25      560      0      0 2020-08-02 02:21:42 UTC+0000
0xfffffa80040a2b00 svchost.exe             860    504     22      479      0      0 2020-08-02 02:21:42 UTC+0000
0xfffffa80040f3060 svchost.exe             912    504     15      644      0      0 2020-08-02 02:21:42 UTC+0000                                 
0xfffffa800410d5f0 svchost.exe             952    504     47     1307      0      0 2020-08-02 02:21:43 UTC+0000                                 
0xfffffa80030d1500 svchost.exe            1032    504     35      607      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa80034b36e0 spoolsv.exe            1132    504     15      289      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa8003474b00 svchost.exe            1164    504     21      344      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa800440ab00 svchost.exe            1244    504     11      315      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa8004411b00 pia-service.ex         1348    504     18      312      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa800199a6e0 VGAuthService.         1428    504      3       86      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa80044d3b00 vmtoolsd.exe           1488    504     11      299      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa8004561b00 WmiPrvSE.exe           1876    644     11      230      0      0 2020-08-02 02:21:46 UTC+0000                                 
0xfffffa80019986d0 dllhost.exe            1960    504     14      196      0      0 2020-08-02 02:21:46 UTC+0000                                 
0xfffffa8004684b00 msdtc.exe              1984    504     12      146      0      0 2020-08-02 02:21:47 UTC+0000                                 
0xfffffa80041c2700 taskhost.exe           2900    504      8      199      1      0 2020-08-02 02:22:02 UTC+0000                                 
0xfffffa8004aad7f0 dwm.exe                1792    860      5      160      1      0 2020-08-02 02:22:03 UTC+0000                                 
0xfffffa8004ab7b00 explorer.exe           2252   2180     28      882      1      0 2020-08-02 02:22:03 UTC+0000                                 
0xfffffa80040d7410 vm3dservice.ex         2464   2252      2       42      1      0 2020-08-02 02:22:04 UTC+0000                                 
0xfffffa80041f9b00 vmtoolsd.exe           2468   2252      8      181      1      0 2020-08-02 02:22:04 UTC+0000                                 
0xfffffa80045d6b00 SearchIndexer.         2420    504     13      712      0      0 2020-08-02 02:22:10 UTC+0000                                 
0xfffffa8001c48b00 sppsvc.exe             3620    504      4      153      0      0 2020-08-02 02:23:46 UTC+0000                                 
0xfffffa8001b4b060 svchost.exe            3708    504     13      347      0      0 2020-08-02 02:23:46 UTC+0000    
0xfffffa8001ad5b00 cmd.exe                3692   2252      1       22      1      0 2020-08-02 02:24:05 UTC+0000                                 
0xfffffa8001c3eb00 conhost.exe            3568    412      2       53      1      0 2020-08-02 02:24:05 UTC+0000                                 
0xfffffa8001ab1b00 chrome.exe             3704   2252     34      793      1      0 2020-08-02 02:24:08 UTC+0000                                 
0xfffffa8001bc9b00 chrome.exe             3696   3704      8       75      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c0a060 chrome.exe             1724   3704     10      230      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c3cb00 chrome.exe             3896   3704     33      469      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c6fb00 chrome.exe             1516   3704     16      272      1      0 2020-08-02 02:24:11 UTC+0000                                 
0xfffffa8001c71b00 pia-client.exe         2408   2252     13      181      1      0 2020-08-02 02:24:20 UTC+0000                                 
0xfffffa80024a0b00 wuauclt.exe            3152    952      4       97      1      0 2020-08-02 02:24:58 UTC+0000                                 
0xfffffa8004794b00 svchost.exe             848    504     10      142      0      0 2020-08-02 02:25:23 UTC+0000                                 
0xfffffa8002550360 pidgin.exe             4060   2252     13      262      1      1 2020-08-02 03:40:36 UTC+0000                                 
0xfffffa80024a4870 chrome.exe             3172   3704     15      394      1      0 2020-08-02 03:40:49 UTC+0000                                 
0xfffffa8002499b00 audiodg.exe             616    784      4      126      0      0 2020-08-02 03:52:48 UTC+0000                                 
0xfffffa8002f4eb00 SearchProtocol         3996   2420      7      326      0      0 2020-08-02 06:47:09 UTC+0000                                 
0xfffffa8004525b00 firefox.exe            3412   4012     55      813      1      0 2020-08-02 06:57:58 UTC+0000                                 
0xfffffa8002483820 firefox.exe            3424   3412     12      209      1      0 2020-08-02 06:57:58 UTC+0000                                 
0xfffffa80040ad910 tor.exe                2640   3412      6       77      1      0 2020-08-02 06:57:59 UTC+0000                                 
0xfffffa8004ac2930 SearchFilterHo         1904   2420      6      111      0      0 2020-08-02 06:57:59 UTC+0000                                 
0xfffffa8001d113e0 firefox.exe            2728   3412     28      352      1      0 2020-08-02 06:58:08 UTC+0000                                 
0xfffffa800250e770 firefox.exe            3380   3412     26      342      1      0 2020-08-02 06:58:09 UTC+0000                                 
0xfffffa8001aaeb00 firefox.exe            3756   3412     23      312      1      0 2020-08-02 06:58:13 UTC+0000                                 
0xfffffa8001ab9060 pia-openvpn.ex         1872   1348      2       78      0      0 2020-08-02 06:58:40 UTC+0000                                 
0xfffffa8002f84b00 conhost.exe            2832    360      1       33      0      0 2020-08-02 06:58:41 UTC+0000                                 
0xfffffa8001ceeb00 mobsync.exe             208    644      8      165      1      0 2020-08-02 06:58:45 UTC+0000                                 
0xfffffa80030155f0 cmd.exe                2064   1488      0 --------      0      0 2020-08-02 06:58:50 UTC+0000   2020-08-02 06:58:50 UTC+0000  
0xfffffa800256f770 conhost.exe            3392    360      0       30      0      0 2020-08-02 06:58:50 UTC+0000                                 
0xfffffa8002f88640 ipconfig.exe           3128   2064      0 --------      0      0 2020-08-02 06:58:50 UTC+0000   2020-08-02 06:58:50 UTC+0000  
```

<p></p>
Here we can see multiple processes pointing us to the answer:
<p></p>

```
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                                                                       
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------                                             
0xfffffa8004411b00 pia-service.ex         1348    504     18      312      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa8001c71b00 pia-client.exe         2408   2252     13      181      1      0 2020-08-02 02:24:20 UTC+0000                                 
0xfffffa8001ab9060 pia-openvpn.ex         1872   1348      2       78      0      0 2020-08-02 06:58:40 UTC+0000                                 
```

<p></p>
Next we will look at <kbd>psscan</kbd>. 
<p></p>
The command we will run is:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 psscan
```

<p></p>
However this gives us no output so we will move to the next command being <kbd>pstree</kbd>
<p></p>
The command we will run is:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 pstree
```

<p></p>
Which gives us:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 pstree                                                                                                                                    
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
 0xfffffa800350d060:wininit.exe                       400    344      3     81 2020-08-02 02:21:41 UTC+0000                                                                                   
. 0xfffffa8003e03b00:lsass.exe                        512    400      9    643 2020-08-02 02:21:41 UTC+0000                                                                                   
. 0xfffffa8003ea8b00:lsm.exe                          524    400     11    167 2020-08-02 02:21:41 UTC+0000                                                                                   
. 0xfffffa8003db3b00:services.exe                     504    400      7    214 2020-08-02 02:21:41 UTC+0000                                                                                   
.. 0xfffffa8004794b00:svchost.exe                     848    504     10    142 2020-08-02 02:25:23 UTC+0000                                                                                   
.. 0xfffffa80030d1500:svchost.exe                    1032    504     35    607 2020-08-02 02:21:44 UTC+0000                                                                                   
.. 0xfffffa800199a6e0:VGAuthService.                 1428    504      3     86 2020-08-02 02:21:45 UTC+0000                                                                                   
.. 0xfffffa8003fd49c0:svchost.exe                     644    504     12    375 2020-08-02 02:21:42 UTC+0000                                                                                   
... 0xfffffa8004561b00:WmiPrvSE.exe                  1876    644     11    230 2020-08-02 02:21:46 UTC+0000                                                                                   
... 0xfffffa8001ceeb00:mobsync.exe                    208    644      8    165 2020-08-02 06:58:45 UTC+0000                                                                                   
.. 0xfffffa80019986d0:dllhost.exe                    1960    504     14    196 2020-08-02 02:21:46 UTC+0000                                                                                   
.. 0xfffffa800440ab00:svchost.exe                    1244    504     11    315 2020-08-02 02:21:44 UTC+0000                                                                                   
.. 0xfffffa800410d5f0:svchost.exe                     952    504     47   1307 2020-08-02 02:21:43 UTC+0000                                                                                   
... 0xfffffa80024a0b00:wuauclt.exe                   3152    952      4     97 2020-08-02 02:24:58 UTC+0000                                                                                   
.. 0xfffffa8004684b00:msdtc.exe                      1984    504     12    146 2020-08-02 02:21:47 UTC+0000                                                                                   
.. 0xfffffa8004411b00:pia-service.ex                 1348    504     18    312 2020-08-02 02:21:45 UTC+0000                                                                                   
... 0xfffffa8001ab9060:pia-openvpn.ex                1872   1348      2     78 2020-08-02 06:58:40 UTC+0000                                                                                   
.. 0xfffffa8003474b00:svchost.exe                    1164    504     21    344 2020-08-02 02:21:44 UTC+0000                                                                                   
.. 0xfffffa800403e2f0:svchost.exe                     716    504      9    317 2020-08-02 02:21:42 UTC+0000                                                                                   
.. 0xfffffa80044d3b00:vmtoolsd.exe                   1488    504     11    299 2020-08-02 02:21:45 UTC+0000                                                                                   
... 0xfffffa80030155f0:cmd.exe                       2064   1488      0 ------ 2020-08-02 06:58:50 UTC+0000                                                                                   
.... 0xfffffa8002f88640:ipconfig.exe                 3128   2064      0 ------ 2020-08-02 06:58:50 UTC+0000                                                                                   
.. 0xfffffa8001c48b00:sppsvc.exe                     3620    504      4    153 2020-08-02 02:23:46 UTC+0000                                                                                   
.. 0xfffffa80040a2b00:svchost.exe                     860    504     22    479 2020-08-02 02:21:42 UTC+0000
... 0xfffffa8004aad7f0:dwm.exe                       1792    860      5    160 2020-08-02 02:22:03 UTC+0000
.. 0xfffffa80040f3060:svchost.exe                     912    504     15    644 2020-08-02 02:21:42 UTC+0000
.. 0xfffffa80034b36e0:spoolsv.exe                    1132    504     15    289 2020-08-02 02:21:44 UTC+0000
.. 0xfffffa80045d6b00:SearchIndexer.                 2420    504     13    712 2020-08-02 02:22:10 UTC+0000
... 0xfffffa8002f4eb00:SearchProtocol                3996   2420      7    326 2020-08-02 06:47:09 UTC+0000
... 0xfffffa8004ac2930:SearchFilterHo                1904   2420      6    111 2020-08-02 06:57:59 UTC+0000
.. 0xfffffa80041c2700:taskhost.exe                   2900    504      8    199 2020-08-02 02:22:02 UTC+0000
.. 0xfffffa8001b4b060:svchost.exe                    3708    504     13    347 2020-08-02 02:23:46 UTC+0000
.. 0xfffffa8003fdcb00:svchost.exe                     784    504     25    560 2020-08-02 02:21:42 UTC+0000
... 0xfffffa8002499b00:audiodg.exe                    616    784      4    126 2020-08-02 03:52:48 UTC+0000
 0xfffffa8003c2f060:csrss.exe                         360    344      9    555 2020-08-02 02:21:40 UTC+0000
. 0xfffffa800256f770:conhost.exe                     3392    360      0     30 2020-08-02 06:58:50 UTC+0000
. 0xfffffa8002f84b00:conhost.exe                     2832    360      1     33 2020-08-02 06:58:41 UTC+0000
 0xfffffa80018b0960:System                              4      0     97    541 2020-08-02 02:21:39 UTC+0000
. 0xfffffa80031c75d0:smss.exe                         272      4      2     30 2020-08-02 02:21:39 UTC+0000
 0xfffffa8004ab7b00:explorer.exe                     2252   2180     28    882 2020-08-02 02:22:03 UTC+0000
. 0xfffffa8001ad5b00:cmd.exe                         3692   2252      1     22 2020-08-02 02:24:05 UTC+0000
. 0xfffffa8002550360:pidgin.exe                      4060   2252     13    262 2020-08-02 03:40:36 UTC+0000
. 0xfffffa80040d7410:vm3dservice.ex                  2464   2252      2     42 2020-08-02 02:22:04 UTC+0000
. 0xfffffa80041f9b00:vmtoolsd.exe                    2468   2252      8    181 2020-08-02 02:22:04 UTC+0000
. 0xfffffa8001ab1b00:chrome.exe                      3704   2252     34    793 2020-08-02 02:24:08 UTC+0000
.. 0xfffffa8001c3cb00:chrome.exe                     3896   3704     33    469 2020-08-02 02:24:09 UTC+0000
.. 0xfffffa8001bc9b00:chrome.exe                     3696   3704      8     75 2020-08-02 02:24:09 UTC+0000
.. 0xfffffa8001c0a060:chrome.exe                     1724   3704     10    230 2020-08-02 02:24:09 UTC+0000
.. 0xfffffa8001c6fb00:chrome.exe                     1516   3704     16    272 2020-08-02 02:24:11 UTC+0000
.. 0xfffffa80024a4870:chrome.exe                     3172   3704     15    394 2020-08-02 03:40:49 UTC+0000
. 0xfffffa8001c71b00:pia-client.exe                  2408   2252     13    181 2020-08-02 02:24:20 UTC+0000
 0xfffffa8004525b00:firefox.exe                      3412   4012     55    813 2020-08-02 06:57:58 UTC+0000
. 0xfffffa8001d113e0:firefox.exe                     2728   3412     28    352 2020-08-02 06:58:08 UTC+0000
. 0xfffffa80040ad910:tor.exe                         2640   3412      6     77 2020-08-02 06:57:59 UTC+0000
. 0xfffffa8001aaeb00:firefox.exe                     3756   3412     23    312 2020-08-02 06:58:13 UTC+0000
. 0xfffffa800250e770:firefox.exe                     3380   3412     26    342 2020-08-02 06:58:09 UTC+0000
. 0xfffffa8002483820:firefox.exe                     3424   3412     12    209 2020-08-02 06:57:58 UTC+0000
 0xfffffa8003df0b00:csrss.exe                         412    392     11    558 2020-08-02 02:21:41 UTC+0000
. 0xfffffa8001c3eb00:conhost.exe                     3568    412      2     53 2020-08-02 02:24:05 UTC+0000
 0xfffffa8003e679a0:winlogon.exe                      456    392      3    117 2020-08-02 02:21:41 UTC+0000
```

<p></p>
This command also points us towards the answer:
<p></p>

```
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
.. 0xfffffa8004411b00:pia-service.ex                 1348    504     18    312 2020-08-02 02:21:45 UTC+0000
... 0xfffffa8001ab9060:pia-openvpn.ex                1872   1348      2     78 2020-08-02 06:58:40 UTC+0000
. 0xfffffa8001c71b00:pia-client.exe                  2408   2252     13    181 2020-08-02 02:24:20 UTC+0000
```

<p></p>
The final command we will look at is <kbd>cmdline</kbd>.
<p></p>
The command we will run is:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 cmdline
```

<p></p>
Which gives us a huge output however we are only interested in these lines:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
************************************************************************
pia-service.ex pid:   1348
Command line : "C:\Program Files\Private Internet Access\pia-service.exe"
************************************************************************
pia-client.exe pid:   2408
Command line : "C:\Program Files\Private Internet Access\pia-client.exe" 
************************************************************************
pia-openvpn.ex pid:   1872
Command line : 
************************************************************************
```

<p></p>
Now that we have looked at the different ways we can get the information we need a simple google search will show that pia is a vpn provider.
<p></p>

<details>
    <summary>Answer</summary>
<p></p>
FLAG{privateinternetaccess}
</details>
</details>
</details>
</details>
</details>
<hr>
<p></p>
<details>
    <summary>Memory is RAM 2</summary>
<p></p>
The second challenge we are given is:
<p></p>
It is believed the attacker attempted to identify and exploit a SQL injection vulnerability in a victim's website, can you identify the domain they were targeting?
<br>
Flag Format: FLAG{domain}
<p></p>

<details>
    <summary>Walkthrough</summary>
<p></p>
So what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- the SQL injection was likely performed in a terminal.
<p></p>
With that information we can now begin our analysis.
</details>
<hr>
 Now that we know what we are looking for what are the possible options we can use?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>

Options | Description
--------|--------------
cmdscan | The cmdscan plugin searches the memory of csrss.exe on XP/2003/Vista/2008 and conhost.exe on Windows 7 for commands that attackers entered through a console shell (cmd.exe). This is one of the most powerful commands you can use to gain visibility into an attackers actions on a victim system, whether they opened cmd.exe through an RDP session or proxied input/output to a command shell from a networked backdoor. <p></p> In addition to the commands entered into a shell, this plugin shows: <p></p> - The name of the console host process (csrss.exe or conhost.exe) <br> - The name of the application using the console (whatever process is using cmd.exe) <br> - The location of the command history buffers, including the current buffer count, last added command, and last displayed command <br> - The application process handle <p></p> Due to the scanning technique this plugin uses, it has the capability to find commands from both active and closed consoles.
consoles | Similar to cmdscan the consoles plugin finds commands that attackers typed into cmd.exe or executed via backdoors. However, instead of scanning for COMMAND_HISTORY, this plugin scans for CONSOLE_INFORMATION. The major advantage to this plugin is it not only prints the commands attackers typed, but it collects the entire screen buffer (input and output). For instance, instead of just seeing "dir", you'll see exactly what the attacker saw, including all files and directories listed by the "dir" command. <p></p> Additionally, this plugin prints the following: <p></p> - The original console window title and current console window title <br> - The name and pid of attached processes (walks a LIST_ENTRY to enumerate all of them if more than one) <br> - Any aliases associated with the commands executed. For example, attackers can register an alias such that typing "hello" actually executes "cd system" <br> - The screen coordinates of the cmd.exe console

<p></p>
Both of these options will show us terminal CLI information but with slightly different outputs, lets look at them now.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
The first command we will run is <kbd>cmdscan</kbd> which looks like this:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 cmdscan
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 cmdscan
Volatility Foundation Volatility Framework 2.6
**************************************************
CommandProcess: conhost.exe Pid: 3568
CommandHistory: 0xff2f0 Application: cmd.exe Flags: Allocated, Reset
CommandCount: 12 LastAdded: 11 LastDisplayed: 11
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
Cmd #0 @ 0xf3b50: cd Downloads
Cmd #1 @ 0xfde30: cd sqlmap
Cmd #2 @ 0xdbe50: cd sqlmapproject-sqlmap-b719b96
Cmd #3 @ 0xfdbe0: ls
Cmd #4 @ 0xfdc80: dir
Cmd #5 @ 0xfbbd0: python3 sqlmap.py --help
Cmd #6 @ 0xfde50: py -3
Cmd #7 @ 0xfbd50: py -3 sqlmap.py --help
Cmd #8 @ 0xff8e0: sqlmap -u "http://platypus-corp.com.au" --tor --check-tor --dbs --random-agent
Cmd #9 @ 0xff4d0: py -3 sqlmap.py -u "http://platypus-corp.com.au" --tor --check-tor --dbs --random-agent
Cmd #10 @ 0x105690: py -3 sqlmap.py -u "http://platypus-corp.com.au/"  --crawl --tor --check-tor --dbs --random-agent
Cmd #11 @ 0x105760: py -3 sqlmap.py -u "http://platypus-corp.com.au/"  --crawl=5 --tor --check-tor --dbs --random-agent
Cmd #15 @ 0xb0158: 
Cmd #16 @ 0xfdc80: dir
```

<p></p>
From this command we can see the information we were looking for. We will also look at the <kbd>consoles</kbd> command as it outputs the information in a different way.
<p></p>
The consoles command will look like this:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 consoles
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 consoles                                                                                                                                  
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
**************************************************                                                                                                                                            
ConsoleProcess: conhost.exe Pid: 3568                                                                                                                                                         
Console: 0xff146200 CommandHistorySize: 50                                                                                                                                                    
HistoryBufferCount: 4 HistoryBufferMax: 4                                                                                                                                                     
OriginalTitle: %SystemRoot%\system32\cmd.exe                                                                                                                                                  
Title: C:\Windows\system32\cmd.exe                                                                                                                                                            
AttachedProcess: cmd.exe Pid: 3692 Handle: 0x60                                                                                                                                               
----                                                                                                                                                                                          
CommandHistory: 0x100c70 Application: cmd.exe Flags:                                                                                                                                          
CommandCount: 0 LastAdded: -1 LastDisplayed: -1                                                                                                                                               
FirstCommand: 0 CommandCountMax: 50                                                                                                                                                           
ProcessHandle: 0x0                                                                                                                                                                            
----                                                                                                                                                                                          
CommandHistory: 0xffa90 Application: python.exe Flags: Reset                                                                                                                                  
CommandCount: 2 LastAdded: 1 LastDisplayed: 1                                                                                                                                                 
FirstCommand: 0 CommandCountMax: 50                                                                                                                                                           
ProcessHandle: 0x0                                                                                                                                                                            
Cmd #0 at 0xfdeb0: exit()                                                                                                                                                                     
Cmd #1 at 0xff890: n                                                                                                                                                                          
----                                                                                                                                                                                          
CommandHistory: 0xff600 Application: py.exe Flags:                                                                                                                                            
CommandCount: 0 LastAdded: -1 LastDisplayed: -1                                                                                                                                               
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x0
----
CommandHistory: 0xff2f0 Application: cmd.exe Flags: Allocated, Reset
CommandCount: 12 LastAdded: 11 LastDisplayed: 11
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
Cmd #0 at 0xf3b50: cd Downloads
Cmd #1 at 0xfde30: cd sqlmap
Cmd #2 at 0xdbe50: cd sqlmapproject-sqlmap-b719b96
Cmd #3 at 0xfdbe0: ls
Cmd #4 at 0xfdc80: dir
Cmd #5 at 0xfbbd0: python3 sqlmap.py --help
Cmd #6 at 0xfde50: py -3
Cmd #7 at 0xfbd50: py -3 sqlmap.py --help
Cmd #8 at 0xff8e0: sqlmap -u "http://platypus-corp.com.au" --tor --check-tor --dbs --random-agent
Cmd #9 at 0xff4d0: py -3 sqlmap.py -u "http://platypus-corp.com.au" --tor --check-tor --dbs --random-agent
Cmd #10 at 0x105690: py -3 sqlmap.py -u "http://platypus-corp.com.au/"  --crawl --tor --check-tor --dbs --random-agent
Cmd #11 at 0x105760: py -3 sqlmap.py -u "http://platypus-corp.com.au/"  --crawl=5 --tor --check-tor --dbs --random-agent
----
Screen 0xdf970 X:80 Y:300
Dump:                                                                                                                                                                                         
07/31/2020  06:28 PM            16,703 .pylintrc                                                                                                                                              
07/31/2020  06:28 PM               402 .travis.yml                                                                                                                                            
07/31/2020  06:28 PM             2,092 COMMITMENT                                                                                                                                             
07/31/2020  06:28 PM    <DIR>          data                                                                                                                                                   
07/31/2020  06:28 PM    <DIR>          doc                                       
07/31/2020  06:28 PM    <DIR>          extra                                     
07/31/2020  06:28 PM    <DIR>          lib                                       
07/31/2020  06:28 PM            18,886 LICENSE                                   
07/31/2020  06:28 PM    <DIR>          plugins                                   
07/31/2020  06:28 PM             5,005 README.md                                
07/31/2020  06:28 PM            21,310 sqlmap.conf                              
07/31/2020  06:28 PM            21,263 sqlmap.py                                
07/31/2020  06:28 PM             2,783 sqlmapapi.py                             
07/31/2020  06:28 PM    <DIR>          tamper                                    
07/31/2020  06:28 PM    <DIR>          thirdparty                               
              10 File(s)         88,796 bytes                                    
              10 Dir(s)  14,491,189,248 bytes free                              
                                                                                 
C:\Users\User\Downloads\sqlmap\sqlmapproject-sqlmap-b719b96>python3 sqlmap.py --
help                                                                             
'python3' is not recognized as an internal or external command,                 
operable program or batch file.                                                  
                                                                                 
C:\Users\User\Downloads\sqlmap\sqlmapproject-sqlmap-b719b96>py -3               
Python 3.8.5 (tags/v3.8.5:580fbb0, Jul 20 2020, 15:43:08) [MSC v.1926 32 bit (In
tel)] on win32                  
    -g GOOGLEDORK       Process Google dork results as target URLs              
                                                                                 
  Request:                                                                       
    These options can be used to specify how to connect to the target URL       
                                                                                 
    --data=DATA         Data string to be sent through POST (e.g. "id=1")       
    --cookie=COOKIE     HTTP Cookie header value (e.g. "PHPSESSID=a8d127e..")   
    --random-agent      Use randomly selected HTTP User-Agent header value      
    --proxy=PROXY       Use a proxy to connect to the target URL                
    --tor               Use Tor anonymity network                               
    --check-tor         Check to see if Tor is used properly                    
                                                                                 
  Injection:                                                                     
    These options can be used to specify which parameters to test for,          
    provide custom injection payloads and optional tampering scripts            
                                                                                 
    -p TESTPARAMETER    Testable parameter(s)                                    
    --dbms=DBMS         Force back-end DBMS to provided value                   
                                                                                 
  Detection:                                                                     
    These options can be used to customize the detection phase                  
                                                                                 
    --level=LEVEL       Level of tests to perform (1-5, default 1)              
    --risk=RISK         Risk of tests to perform (1-3, default 1)               
                                                                                 
  Techniques:                                    
    system underlying operating system                                           
                                                                                 
    --os-shell          Prompt for an interactive operating system shell        
    --os-pwn            Prompt for an OOB shell, Meterpreter or VNC             
                                                                                 
  General:                                                                       
    These options can be used to set some general working parameters            
                                                                                 
    --batch             Never ask for user input, use the default behavior      
    --flush-session     Flush session files for current target                  
                                                                                 
  Miscellaneous:                                                                 
    These options do not fit into any other category                            
                                                                                 
    --sqlmap-shell      Prompt for an interactive sqlmap shell                  
    --wizard            Simple wizard interface for beginner users              
                                                                                 
Press Enter to continue...                                                       
                                                                                 
C:\Users\User\Downloads\sqlmap\sqlmapproject-sqlmap-b719b96>sqlmap -u "http://pl
atypus-corp.com.au" --tor --check-tor --dbs --random-agent                      
'sqlmap' is not recognized as an internal or external command,                  
operable program or batch file.                                                  
                                                                                 
C:\Users\User\Downloads\sqlmap\sqlmapproject-sqlmap-b719b96>py -3 sqlmap.py -u "
http://platypus-corp.com.au" --tor --check-tor --dbs --random-agent             
        ___                                                                      
       __H__                                                                     
 ___ ___[']_____ ___ ___  {1.4.7.25#dev}                                         
|_ -| . [']     | .'| . |                                                        
|___|_  [']_|_|_|__,|  _|                                                        
      |_|V...       |_|   http://sqlmap.org                                      
                                                                                 
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual
 consent is illegal. It is the end user's responsibility to obey all applicable 
local, state and federal laws. Developers assume no liability and are not respon
sible for any misuse or damage caused by this program                           
                                                                                 
[*] starting @ 13:50:27 /2020-08-02/                                             
                                                                                 
[13:50:27] [WARNING] increasing default value for option '--time-sec' to 10 beca
use switch '--tor' was provided                                                  
[13:50:27] [INFO] setting Tor SOCKS proxy settings                              
[13:50:28] [INFO] fetched random HTTP User-Agent header value 'Opera/9.80 (Windo
ws NT 6.1; U; ja) Presto/2.5.22 Version/10.50' from file 'C:\Users\User\Download
s\sqlmap\sqlmapproject-sqlmap-b719b96\data\txt\user-agents.txt'                 
[13:50:28] [INFO] checking Tor connection                                        
[13:50:31] [INFO] Tor is properly being used                                     
[13:50:32] [INFO] testing connection to the target URL                          
[13:50:35] [CRITICAL] unable to connect to the target URL or proxy. sqlmap is go
ing to retry the request(s)                                                      
[13:50:35] [WARNING] please make sure that you have Tor installed and running so
 you could successfully use switch '--tor' (e.g. 'https://www.torproject.org/dow
nload/download.html.en')                                                         
                                                                                 
                                                                                 
[*] ending @ 13:50:40 /2020-08-02/                                               
                                                                                 
^C                                                                               
C:\Users\User\Downloads\sqlmap\sqlmapproject-sqlmap-b719b96>py -3 sqlmap.py -u "
http://platypus-corp.com.au/"  --crawl --tor --check-tor --dbs --random-agent   
        ___                                                                      
       __H__                                                                     
 ___ ___[)]_____ ___ ___  {1.4.7.25#dev}                                         
|_ -| . ["]     | .'| . |                                                        
|___|_  [(]_|_|_|__,|  _|                                                        
      |_|V...       |_|   http://sqlmap.org                                      
                                                                                 
Usage: sqlmap.py [options]                                                       
                                                                                 
sqlmap.py: error: option --crawl: invalid integer value: '--tor'                
                                                                                 
Press Enter to continue...exit()                                                 
                                                                                 
C:\Users\User\Downloads\sqlmap\sqlmapproject-sqlmap-b719b96>py -3 sqlmap.py -u "
http://platypus-corp.com.au/"  --crawl=5 --tor --check-tor --dbs --random-agent 
        ___                                                                      
       __H__                                                                     
 ___ ___[)]_____ ___ ___  {1.4.7.25#dev}                                         
|_ -| . [(]     | .'| . |                                                        
|___|_  [.]_|_|_|__,|  _|                                                        
      |_|V...       |_|   http://sqlmap.org                                      
                                                                                 
[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual
 consent is illegal. It is the end user's responsibility to obey all applicable 
local, state and federal laws. Developers assume no liability and are not respon
sible for any misuse or damage caused by this program                           
                                                                                 
[*] starting @ 13:51:07 /2020-08-02/                                             
                                                                                 
[13:51:07] [WARNING] increasing default value for option '--time-sec' to 10 beca
use switch '--tor' was provided                                                  
[13:51:07] [INFO] setting Tor SOCKS proxy settings                              
[13:51:08] [INFO] fetched random HTTP User-Agent header value 'Mozilla/5.0 (Wind
ows NT 6.2; rv:21.0) Gecko/20130326 Firefox/21.0' from file 'C:\Users\User\Downl
oads\sqlmap\sqlmapproject-sqlmap-b719b96\data\txt\user-agents.txt'              
[13:51:08] [INFO] checking Tor connection                                        
[13:51:10] [INFO] Tor is properly being used                                     
do you want to check for the existence of site's sitemap(.xml) [y/N] n          
[13:51:12] [INFO] starting crawler for target URL 'http://platypus-corp.com.au/'
                                                                                 
[13:51:12] [INFO] searching for links with depth 1                              
[13:51:14] [CRITICAL] unable to connect to the target URL or proxy. sqlmap is go
ing to retry the request(s)                                                      
[13:51:14] [WARNING] please make sure that you have Tor installed and running so
 you could successfully use switch '--tor' (e.g. 'https://www.torproject.org/dow
nload/download.html.en')                                                         
                                                                                 
[13:51:22] [WARNING] user aborted during crawling. sqlmap will use partial list 
[13:51:22] [WARNING] no usable links found (with GET parameters)                
                                                                                 
                                                                                 
[*] ending @ 13:51:23 /2020-08-02/                                               
                                                                                 
                                                                                 
C:\Users\User\Downloads\sqlmap\sqlmapproject-sqlmap-b719b96>                    
```

<p></p>
The <kbd>consoles</kbd> command outputs alot more information however it allows us to see exactly what is being returned within the terminal.
<p></p>
Both of these methods pointed us to the correct answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
FLAG{platypus-corp.com.au}
<p></p>
</details>
</details>
</details>
</details>
</details>

<hr>

<details>
    <summary>Memory is RAM 3</summary>
<p></p>
The third challenge we are given is:
<p></p>
It appears the attacker was utilizing Tor, what is the IP address of
<br>
the entry node the attacker was going through?
<p></p>
Flag Format: FLAG{IP}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
So what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- The attacker was using Tor, but what is Tor? 
    - Tor is free and open-source software for enabling anonymous communication by directing Internet traffic through a free, worldwide, volunteer overlay network consisting of more than seven thousand relays[5] in order to conceal a user's location and usage from anyone conducting network surveillance or traffic analysis. Using Tor makes it more difficult to trace the Internet activity to the user: this includes "visits to Web sites, online posts, instant messages, and other communication forms".
<p></p>
    - Onion routing is implemented by encryption in the application layer of a communication protocol stack, nested like the layers of an onion. Tor encrypts the data, including the next node destination IP address, multiple times and sends it through a virtual circuit comprising successive, random-selection Tor relays. Each relay decrypts a layer of encryption to reveal the next relay in the circuit to pass the remaining encrypted data on to it. The final relay decrypts the innermost layer of encryption and sends the original data to its destination without revealing or knowing the source IP address. Because the routing of the communication was partly concealed at every hop in the Tor circuit, this method eliminates any single point at which the communicating peers can be determined through network surveillance that relies upon knowing its source and destination.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/how_tor_works_1-100763523-large.jpg" width="600"><br>
</div>
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/tor-2-100763518-large.jpg" width="600"><br>
</div>
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/tor-3-100763520-large.jpg" width="600"><br>
</div>
<p></p>
So now that we know what Tor is and that it has to do with the internet and routing, we can start looking for options to analyse our file.
<p></p>
<hr>
<p></p>
Now that we know what we are looking for what options can we use for analysis?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>

Options | Description
--------|-------------------
pslist | To list the processes of a system, use the pslist command. This walks the doubly-linked list pointed to by PsActiveProcessHead and shows the offset, process name, process ID, the parent process ID, number of threads, number of handles, and date/time when the process started and exited. As of 2.1 it also shows the Session ID and if the process is a Wow64 process (it uses a 32 bit address space on a 64 bit kernel). <p></p> This plugin does not detect hidden or unlinked processes (but psscan can do that). <p></p> If you see processes with 0 threads, 0 handles, and/or a non-empty exit time, the process may not actually still be active.
netscan | To scan for network artifacts in 32- and 64-bit Windows Vista, Windows 2008 Server and Windows 7 memory dumps, use the netscan command. This finds TCP endpoints, TCP listeners, UDP endpoints, and UDP listeners. It distinguishes between IPv4 and IPv6, prints the local and remote IP (if applicable), the local and remote port (if applicable), the time when the socket was bound or when the connection was established, and the current state (for TCP connections only).

<p></p>
With these two options we can begin our analysis.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
The first command we will runs is <kbd>pslist</kbd> this will do two things for us, first it will show that Tor is running and second it will give us the PID of the running process.
<p></p>
The command will look like this:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 pslist
```

<p></p>
And gives us the following output:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 pslist                                                                                                                                    
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                                                                       
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------                                             
0xfffffa80018b0960 System                    4      0     97      541 ------      0 2020-08-02 02:21:39 UTC+0000                                                                              
0xfffffa80031c75d0 smss.exe                272      4      2       30 ------      0 2020-08-02 02:21:39 UTC+0000                                                                              
0xfffffa8003c2f060 csrss.exe               360    344      9      555      0      0 2020-08-02 02:21:40 UTC+0000                                                                              
0xfffffa800350d060 wininit.exe             400    344      3       81      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003df0b00 csrss.exe               412    392     11      558      1      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003e679a0 winlogon.exe            456    392      3      117      1      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003db3b00 services.exe            504    400      7      214      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003e03b00 lsass.exe               512    400      9      643      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003ea8b00 lsm.exe                 524    400     11      167      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003fd49c0 svchost.exe             644    504     12      375      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa800403e2f0 svchost.exe             716    504      9      317      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa8003fdcb00 svchost.exe             784    504     25      560      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa80040a2b00 svchost.exe             860    504     22      479      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa80040f3060 svchost.exe             912    504     15      644      0      0 2020-08-02 02:21:42 UTC+0000                                 
0xfffffa800410d5f0 svchost.exe             952    504     47     1307      0      0 2020-08-02 02:21:43 UTC+0000                                 
0xfffffa80030d1500 svchost.exe            1032    504     35      607      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa80034b36e0 spoolsv.exe            1132    504     15      289      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa8003474b00 svchost.exe            1164    504     21      344      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa800440ab00 svchost.exe            1244    504     11      315      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa8004411b00 pia-service.ex         1348    504     18      312      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa800199a6e0 VGAuthService.         1428    504      3       86      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa80044d3b00 vmtoolsd.exe           1488    504     11      299      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa8004561b00 WmiPrvSE.exe           1876    644     11      230      0      0 2020-08-02 02:21:46 UTC+0000                                 
0xfffffa80019986d0 dllhost.exe            1960    504     14      196      0      0 2020-08-02 02:21:46 UTC+0000                                 
0xfffffa8004684b00 msdtc.exe              1984    504     12      146      0      0 2020-08-02 02:21:47 UTC+0000                                 
0xfffffa80041c2700 taskhost.exe           2900    504      8      199      1      0 2020-08-02 02:22:02 UTC+0000                                 
0xfffffa8004aad7f0 dwm.exe                1792    860      5      160      1      0 2020-08-02 02:22:03 UTC+0000                                 
0xfffffa8004ab7b00 explorer.exe           2252   2180     28      882      1      0 2020-08-02 02:22:03 UTC+0000                                 
0xfffffa80040d7410 vm3dservice.ex         2464   2252      2       42      1      0 2020-08-02 02:22:04 UTC+0000                                 
0xfffffa80041f9b00 vmtoolsd.exe           2468   2252      8      181      1      0 2020-08-02 02:22:04 UTC+0000                                 
0xfffffa80045d6b00 SearchIndexer.         2420    504     13      712      0      0 2020-08-02 02:22:10 UTC+0000                                 
0xfffffa8001c48b00 sppsvc.exe             3620    504      4      153      0      0 2020-08-02 02:23:46 UTC+0000                                 
0xfffffa8001b4b060 svchost.exe            3708    504     13      347      0      0 2020-08-02 02:23:46 UTC+0000                                 
0xfffffa8001ad5b00 cmd.exe                3692   2252      1       22      1      0 2020-08-02 02:24:05 UTC+0000                                 
0xfffffa8001c3eb00 conhost.exe            3568    412      2       53      1      0 2020-08-02 02:24:05 UTC+0000                                 
0xfffffa8001ab1b00 chrome.exe             3704   2252     34      793      1      0 2020-08-02 02:24:08 UTC+0000                                 
0xfffffa8001bc9b00 chrome.exe             3696   3704      8       75      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c0a060 chrome.exe             1724   3704     10      230      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c3cb00 chrome.exe             3896   3704     33      469      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c6fb00 chrome.exe             1516   3704     16      272      1      0 2020-08-02 02:24:11 UTC+0000                                 
0xfffffa8001c71b00 pia-client.exe         2408   2252     13      181      1      0 2020-08-02 02:24:20 UTC+0000                                 
0xfffffa80024a0b00 wuauclt.exe            3152    952      4       97      1      0 2020-08-02 02:24:58 UTC+0000                                 
0xfffffa8004794b00 svchost.exe             848    504     10      142      0      0 2020-08-02 02:25:23 UTC+0000                                 
0xfffffa8002550360 pidgin.exe             4060   2252     13      262      1      1 2020-08-02 03:40:36 UTC+0000                                 
0xfffffa80024a4870 chrome.exe             3172   3704     15      394      1      0 2020-08-02 03:40:49 UTC+0000                                 
0xfffffa8002499b00 audiodg.exe             616    784      4      126      0      0 2020-08-02 03:52:48 UTC+0000                                 
0xfffffa8002f4eb00 SearchProtocol         3996   2420      7      326      0      0 2020-08-02 06:47:09 UTC+0000                                 
0xfffffa8004525b00 firefox.exe            3412   4012     55      813      1      0 2020-08-02 06:57:58 UTC+0000                                 
0xfffffa8002483820 firefox.exe            3424   3412     12      209      1      0 2020-08-02 06:57:58 UTC+0000                                 
0xfffffa80040ad910 tor.exe                2640   3412      6       77      1      0 2020-08-02 06:57:59 UTC+0000                                 
0xfffffa8004ac2930 SearchFilterHo         1904   2420      6      111      0      0 2020-08-02 06:57:59 UTC+0000                                 
0xfffffa8001d113e0 firefox.exe            2728   3412     28      352      1      0 2020-08-02 06:58:08 UTC+0000                                 
0xfffffa800250e770 firefox.exe            3380   3412     26      342      1      0 2020-08-02 06:58:09 UTC+0000                                 
0xfffffa8001aaeb00 firefox.exe            3756   3412     23      312      1      0 2020-08-02 06:58:13 UTC+0000                                 
0xfffffa8001ab9060 pia-openvpn.ex         1872   1348      2       78      0      0 2020-08-02 06:58:40 UTC+0000                                 
0xfffffa8002f84b00 conhost.exe            2832    360      1       33      0      0 2020-08-02 06:58:41 UTC+0000                                 
0xfffffa8001ceeb00 mobsync.exe             208    644      8      165      1      0 2020-08-02 06:58:45 UTC+0000                                 
0xfffffa80030155f0 cmd.exe                2064   1488      0 --------      0      0 2020-08-02 06:58:50 UTC+0000   2020-08-02 06:58:50 UTC+0000  
0xfffffa800256f770 conhost.exe            3392    360      0       30      0      0 2020-08-02 06:58:50 UTC+0000                                 
0xfffffa8002f88640 ipconfig.exe           3128   2064      0 --------      0      0 2020-08-02 06:58:50 UTC+0000   2020-08-02 06:58:50 UTC+0000  
```

<p></p>
This showes us that Tor definitely was running and gives us the PID aswell, this will help us in our next step.
<p></p>

```
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                                                                       
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------                                             
0xfffffa80040ad910 tor.exe                2640   3412      6       77      1      0 2020-08-02 06:57:59 UTC+0000                                 
```

<p></p>
The next command we will run is <kbd>netscan</kbd> to see all network connections from the system.
<br>
The command will look like this:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 netscan
```

<p></p>
Which gives us the following output:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 netscan                                                                                                                                   
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x7ccc6b00         TCPv4    127.0.0.1:9150                 127.0.0.1:49976      ESTABLISHED      -1                                                                                           
0x7d0389f0         UDPv4    0.0.0.0:55079                  *:*                                   1032     svchost.exe    2020-08-02 06:58:46 UTC+0000                                         
0x7d0acec0         UDPv4    0.0.0.0:55393                  *:*                                   1032     svchost.exe    2020-08-02 06:58:49 UTC+0000                                         
0x7d0d8a30         UDPv4    0.0.0.0:60491                  *:*                                   1032     svchost.exe    2020-08-02 06:58:49 UTC+0000                                         
0x7d1129e0         UDPv4    0.0.0.0:5355                   *:*                                   1032     svchost.exe    2020-08-02 06:58:50 UTC+0000                                         
0x7d15bbf0         UDPv4    192.168.72.156:138             *:*                                   4        System         2020-08-02 06:57:12 UTC+0000                                         
0x7d1ce160         UDPv4    10.18.10.10:138                *:*                                   4        System         2020-08-02 06:58:42 UTC+0000
0x7d235d70         UDPv4    0.0.0.0:49279                  *:*                                   1032     svchost.exe    2020-08-02 06:58:46 UTC+0000
0x7d306a70         UDPv4    0.0.0.0:0                      *:*                                   1032     svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7d306a70         UDPv6    :::0                           *:*                                   1032     svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7d30bbe0         UDPv4    0.0.0.0:62691                  *:*                                   1032     svchost.exe    2020-08-02 06:58:49 UTC+0000
0x7d700820         UDPv4    0.0.0.0:64450                  *:*                                   1872     pia-openvpn.ex 2020-08-02 06:58:41 UTC+0000
0x7d70dc60         UDPv4    0.0.0.0:53051                  *:*                                   1032     svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7d70dc60         UDPv6    :::53051                       *:*                                   1032     svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7d93d240         UDPv4    0.0.0.0:5353                   *:*                                   3704     chrome.exe     2020-08-02 06:58:47 UTC+0000
0x7d93d240         UDPv6    :::5353                        *:*                                   3704     chrome.exe     2020-08-02 06:58:47 UTC+0000
0x7d9b32e0         UDPv4    0.0.0.0:62238                  *:*                                   1032     svchost.exe    2020-08-02 06:58:49 UTC+0000
0x7d0d5b50         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        512      lsass.exe      
0x7d0db8a0         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        512      lsass.exe      
0x7d0db8a0         TCPv6    :::49156                       :::0                 LISTENING        512      lsass.exe      
0x7d65ac80         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        716      svchost.exe    
0x7d65ac80         TCPv6    :::135                         :::0                 LISTENING        716      svchost.exe    
0x7d6633c0         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        400      wininit.exe    
0x7d665870         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        716      svchost.exe    
0x7d673ee0         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        400      wininit.exe    
0x7d673ee0         TCPv6    :::49152                       :::0                 LISTENING        400      wininit.exe    
0x7d6c90f0         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        784      svchost.exe    
0x7d6c9390         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        784      svchost.exe    
0x7d6c9390         TCPv6    :::49153                       :::0                 LISTENING        784      svchost.exe    
0x7d028970         TCPv4    127.0.0.1:49991                127.0.0.1:49990      ESTABLISHED      -1                      
0x7d112cd0         TCPv4    10.18.10.10:50006              104.97.252.112:443   CLOSED           -1                      
0x7d3d7010         TCPv4    127.0.0.1:9150                 127.0.0.1:49985      ESTABLISHED      -1                      
0x7d8332b0         TCPv4    127.0.0.1:9150                 127.0.0.1:49983      ESTABLISHED      -1                      
0x7da14010         UDPv6    ::1:1900                       *:*                                   848      svchost.exe    2020-08-02 06:58:42 UTC+0000
0x7e32b850         UDPv4    127.0.0.1:56109                *:*                                   848      svchost.exe    2020-08-02 06:58:42 UTC+0000
0x7e2948e0         TCPv4    0.0.0.0:445                    0.0.0.0:0            LISTENING        4        System         
0x7e2948e0         TCPv6    :::445                         :::0                 LISTENING        4        System         
0x7e29cc80         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        952      svchost.exe    
0x7e29fee0         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        952      svchost.exe    
0x7e29fee0         TCPv6    :::49154                       :::0                 LISTENING        952      svchost.exe    
0x7e2d3ac0         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        504      services.exe   
0x7e2d9ee0         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        504      services.exe   
0x7e2d9ee0         TCPv6    :::49155                       :::0                 LISTENING        504      services.exe   
0x7e2d9ee0         TCPv6    :::49155                       :::0                 LISTENING        504      services.exe   
0x7e3258b0         TCPv4    127.0.0.1:49954                127.0.0.1:49953      ESTABLISHED      -1                      
0x7e33acd0         TCPv4    127.0.0.1:49985                127.0.0.1:9150       ESTABLISHED      -1                      
0x7e370210         TCPv4    192.168.72.156:49963           160.16.212.196:5222  ESTABLISHED      -1                      
0x7e7e9010         UDPv4    10.18.10.10:137                *:*                                   4        System         2020-08-02 06:58:42 UTC+0000
0x7e9826a0         UDPv4    0.0.0.0:64976                  *:*                                   1032     svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7e992380         UDPv4    192.168.72.156:137             *:*                                   4        System         2020-08-02 06:57:12 UTC+0000
0x7e60f730         TCPv4    127.0.0.1:9150                 127.0.0.1:49986      ESTABLISHED      -1                      
0x7e686cd0         TCPv4    127.0.0.1:49986                127.0.0.1:9150       ESTABLISHED      -1                      
0x7e6e8050         TCPv4    127.0.0.1:49992                127.0.0.1:50005      CLOSED           -1                      
0x7e971500         TCPv4    127.0.0.1:49982                127.0.0.1:9150       ESTABLISHED      -1                      
0x7e983960         TCPv4    127.0.0.1:49972                127.0.0.1:49971      ESTABLISHED      -1                      
0x7ec85cd0         TCPv4    127.0.0.1:9150                 127.0.0.1:49973      ESTABLISHED      -1                      
0x7ed9dcd0         TCPv4    127.0.0.1:49983                127.0.0.1:9150       ESTABLISHED      -1                      
0x7ee75e40         UDPv4    0.0.0.0:5355                   *:*                                   1032     svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7ee75e40         UDPv6    :::5355                        *:*                                   1032     svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7ef0b7a0         UDPv6    fe80::19bc:868a:ebe:e82c:546   *:*                                   784      svchost.exe    2020-08-02 06:58:42 UTC+0000
0x7f2b36c0         UDPv6    ::1:56108                      *:*                                   848      svchost.exe    2020-08-02 06:58:42 UTC+0000
0x7f2ed010         UDPv4    10.18.10.10:68                 *:*                                   784      svchost.exe    2020-08-02 06:58:45 UTC+0000
0x7f2f8ec0         UDPv6    fe80::19bc:868a:ebe:e82c:1900  *:*                                   848      svchost.exe    2020-08-02 06:58:42 UTC+0000
0x7f393460         UDPv4    0.0.0.0:0                      *:*                                   952      svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7f393460         UDPv6    :::0                           *:*                                   952      svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7f3d4ec0         UDPv4    0.0.0.0:52187                  *:*                                   1032     svchost.exe    2020-08-02 06:58:46 UTC+0000
0x7f3d8380         UDPv4    0.0.0.0:57768                  *:*                                   1032     svchost.exe    2020-08-02 06:58:49 UTC+0000
0x7f3d8590         UDPv4    0.0.0.0:52769                  *:*                                   1032     svchost.exe    2020-08-02 06:58:46 UTC+0000
0x7f3e30e0         UDPv4    127.0.0.1:53236                *:*                                   4060     pidgin.exe     2020-08-02 06:58:11 UTC+0000
0x7f6548b0         UDPv4    0.0.0.0:55679                  *:*                                   1032     svchost.exe    2020-08-02 06:58:49 UTC+0000
0x7f681710         UDPv4    192.168.72.156:1900            *:*                                   848      svchost.exe    2020-08-02 06:58:42 UTC+0000
0x7f2ac3b0         TCPv4    127.0.0.1:9150                 127.0.0.1:49979      ESTABLISHED      -1                      
0x7f2afcd0         TCPv4    127.0.0.1:9150                 127.0.0.1:49975      ESTABLISHED      -1                      
0x7f2ca7c0         TCPv4    127.0.0.1:49951                127.0.0.1:49950      ESTABLISHED      -1                      
0x7f310cd0         TCPv4    127.0.0.1:49962                127.0.0.1:49961      ESTABLISHED      -1                      
0x7f32a730         TCPv4    127.0.0.1:49971                127.0.0.1:49972      ESTABLISHED      -1                      
0x7f63bcd0         TCPv4    127.0.0.1:9150                 127.0.0.1:49977      ESTABLISHED      -1                      
0x7f657b80         TCPv4    127.0.0.1:49973                127.0.0.1:9150       ESTABLISHED      -1                      
0x7f658cd0         TCPv4    127.0.0.1:9151                 127.0.0.1:49956      ESTABLISHED      -1                      
0x7f6d5cd0         TCPv4    127.0.0.1:49977                127.0.0.1:9150       ESTABLISHED      -1                      
0x7f6eecd0         TCPv4    127.0.0.1:49992                127.0.0.1:50003      CLOSED           -1                      
0x7f7ec7a0         TCPv4    127.0.0.1:49959                127.0.0.1:9151       ESTABLISHED      -1                      
0x7faee240         UDPv4    0.0.0.0:50941                  *:*                                   1032     svchost.exe    2020-08-02 06:58:46 UTC+0000
0x7fb40c40         UDPv4    0.0.0.0:63963                  *:*                                   1032     svchost.exe    2020-08-02 06:58:46 UTC+0000
0x7fb5e850         UDPv4    10.18.10.10:1900               *:*                                   848      svchost.exe    2020-08-02 06:58:42 UTC+0000
0x7fb6a860         UDPv4    10.18.10.10:58903              *:*                                   3704     chrome.exe     2020-08-02 06:58:47 UTC+0000
0x7fbf2ca0         UDPv4    0.0.0.0:52416                  *:*                                   1032     svchost.exe    2020-08-02 06:58:46 UTC+0000
0x7fc09ec0         UDPv4    192.168.72.156:68              *:*                                   784      svchost.exe    2020-08-02 06:58:50 UTC+0000
0x7fc0ce00         UDPv4    192.168.72.156:58904           *:*                                   3704     chrome.exe     2020-08-02 06:58:47 UTC+0000
0x7fc0d010         UDPv4    0.0.0.0:5353                   *:*                                   3704     chrome.exe     2020-08-02 06:58:47 UTC+0000
0x7fc45820         UDPv4    0.0.0.0:5353                   *:*                                   3704     chrome.exe     2020-08-02 06:58:47 UTC+0000
0x7fc45820         UDPv6    :::5353                        *:*                                   3704     chrome.exe     2020-08-02 06:58:47 UTC+0000
0x7fcb0190         UDPv4    0.0.0.0:5353                   *:*                                   3704     chrome.exe     2020-08-02 06:58:47 UTC+0000
0x7fd1dd70         UDPv4    127.0.0.1:53237                *:*                                   4060     pidgin.exe     2020-08-02 06:58:11 UTC+0000
0x7fd5e450         UDPv6    fe80::e1ba:8eee:519f:dce6:1900 *:*                                   848      svchost.exe    2020-08-02 06:58:42 UTC+0000
0x7fe80830         UDPv4    127.0.0.1:1900                 *:*                                   848      svchost.exe    2020-08-02 06:58:42 UTC+0000
0x7fa717b0         TCPv4    127.0.0.1:9150                 0.0.0.0:0            LISTENING        2640     tor.exe        
0x7faa2c50         TCPv4    192.168.72.156:139             0.0.0.0:0            LISTENING        4        System         
0x7fc5c620         TCPv4    127.0.0.1:49992                0.0.0.0:0            LISTENING        1348     pia-service.ex 
0x7fca7b00         TCPv4    127.0.0.1:9151                 0.0.0.0:0            LISTENING        2640     tor.exe        
0x7fa19010         TCPv4    127.0.0.1:49975                127.0.0.1:9150       ESTABLISHED      -1                      
0x7fa3d500         TCPv4    127.0.0.1:49952                127.0.0.1:9151       ESTABLISHED      -1                      
0x7fa74460         TCPv4    127.0.0.1:49990                127.0.0.1:49991      ESTABLISHED      -1                      
0x7fa806e0         TCPv4    127.0.0.1:9151                 127.0.0.1:49959      ESTABLISHED      -1                      
0x7faa6010         TCPv4    192.168.72.156:49955           149.56.185.56:9001   ESTABLISHED      -1                      
0x7faca010         TCPv4    127.0.0.1:9150                 127.0.0.1:49984      ESTABLISHED      -1                      
0x7fb799a0         TCPv4    127.0.0.1:49953                127.0.0.1:49954      ESTABLISHED      -1                      
0x7fc028b0         TCPv4    127.0.0.1:49950                127.0.0.1:49951      ESTABLISHED      -1                      
0x7fc515a0         TCPv4    127.0.0.1:49976                127.0.0.1:9150       ESTABLISHED      -1                      
0x7fc8b9d0         TCPv4    127.0.0.1:49966                127.0.0.1:49965      ESTABLISHED      -1                      
0x7fcaccd0         TCPv4    127.0.0.1:49961                127.0.0.1:49962      ESTABLISHED      -1                      
0x7fcadac0         TCPv4    127.0.0.1:49956                127.0.0.1:9151       ESTABLISHED      -1                      
0x7fcc3cd0         TCPv4    127.0.0.1:50005                127.0.0.1:49992      CLOSED           -1                      
0x7fd097d0         TCPv4    192.168.72.156:49949           192.241.189.207:443  ESTABLISHED      -1                      
0x7fd6ccd0         TCPv4    127.0.0.1:49965                127.0.0.1:49966      ESTABLISHED      -1                      
0x7fd74010         TCPv4    127.0.0.1:49979                127.0.0.1:9150       ESTABLISHED      -1                      
0x7fdce540         TCPv4    127.0.0.1:9150                 127.0.0.1:49982      ESTABLISHED      -1                      
0x7fe65500         TCPv4    127.0.0.1:9151                 127.0.0.1:49952      ESTABLISHED      -1                      
0x7fe821b0         TCPv4    127.0.0.1:49984                127.0.0.1:9150       ESTABLISHED      -1                      
```

<p></p>
If we look through the output we can see the Tor.exe with the PID of 2640 which we found when we ran <kbd>pslist</kbd>.

```
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x7fca7b00         TCPv4    127.0.0.1:9151                 0.0.0.0:0            LISTENING        2640     tor.exe        
0x7fa19010         TCPv4    127.0.0.1:49975                127.0.0.1:9150       ESTABLISHED      -1                      
0x7fa3d500         TCPv4    127.0.0.1:49952                127.0.0.1:9151       ESTABLISHED      -1                      
0x7fa74460         TCPv4    127.0.0.1:49990                127.0.0.1:49991      ESTABLISHED      -1                      
0x7fa806e0         TCPv4    127.0.0.1:9151                 127.0.0.1:49959      ESTABLISHED      -1                      
0x7faa6010         TCPv4    192.168.72.156:49955           149.56.185.56:9001   ESTABLISHED      -1                      
0x7faca010         TCPv4    127.0.0.1:9150                 127.0.0.1:49984      ESTABLISHED      -1                      
0x7fb799a0         TCPv4    127.0.0.1:49953                127.0.0.1:49954      ESTABLISHED      -1                      
0x7fc028b0         TCPv4    127.0.0.1:49950                127.0.0.1:49951      ESTABLISHED      -1                      
0x7fc515a0         TCPv4    127.0.0.1:49976                127.0.0.1:9150       ESTABLISHED      -1                      
0x7fc8b9d0         TCPv4    127.0.0.1:49966                127.0.0.1:49965      ESTABLISHED      -1                      
0x7fcaccd0         TCPv4    127.0.0.1:49961                127.0.0.1:49962      ESTABLISHED      -1                      
0x7fcadac0         TCPv4    127.0.0.1:49956                127.0.0.1:9151       ESTABLISHED      -1                      
0x7fcc3cd0         TCPv4    127.0.0.1:50005                127.0.0.1:49992      CLOSED           -1                      
0x7fd097d0         TCPv4    192.168.72.156:49949           192.241.189.207:443  ESTABLISHED      -1                      
0x7fd6ccd0         TCPv4    127.0.0.1:49965                127.0.0.1:49966      ESTABLISHED      -1                      
0x7fd74010         TCPv4    127.0.0.1:49979                127.0.0.1:9150       ESTABLISHED      -1                      
0x7fdce540         TCPv4    127.0.0.1:9150                 127.0.0.1:49982      ESTABLISHED      -1                      
0x7fe65500         TCPv4    127.0.0.1:9151                 127.0.0.1:49952      ESTABLISHED      -1                      
0x7fe821b0         TCPv4    127.0.0.1:49984                127.0.0.1:9150       ESTABLISHED      -1                      
```

<p></p>
Looking at the above excerpt we can see the initial process Tor.exe with PID of 2640, all connections bellow have a PID of -1 meaning they are child processes of Tor.exe. 
<p></p>
Since we are looking for the entry node (that is the first node that Tor communicates with) we know it will be an external address.
<p></p>
We also know that 127.0.0.1 is the local ip address of the system so we can exclude all those ip's leaving us with two external ip addresses.
<p></p>
A quick search will show us common Tor ports being:
<p></p>
"Transmission Control Protocol (TCP) and User Datagram Protocol (UDP) ports commonly affiliated with Tor include 9001, 9030, 9040, 9050, 9051, and 9150."
<p></p>
Providing us with the answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
FLAG{149.56.185.56}
</details>
</details>
</details>
</details>
</details>
</details>

<p></p>
<hr>
<p></p>
<details>
    <summary>Memory is RAM 4</summary>
<p></p>
The fourth challenge we are given is:
<p></p>
What .onion site was the attacker browsing?
<p></p>
Flag Format: FLAG{site.onion}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
So what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- From some research about the .onion extention we find:
<br>
".onion is a special-use top level domain name designating an anonymous onion service, which was formerly known as a "hidden service",[1] reachable via the Tor network. Such addresses are not actual DNS names, and the .onion TLD is not in the Internet DNS root, but with the appropriate proxy software installed, Internet programs such as web browsers can access sites with .onion addresses by sending the request through the Tor network."
<p></p>
- So we know that we need to look at Tor again, and we need to see the browsing history
<p></p>
With that information we can start the analysis.
<hr>
<p></p>
 Now that we know what we are looking for what options can we use for analysis?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>

Options | Description
--------|--------------
pslist | To list the processes of a system, use the pslist command. This walks the doubly-linked list pointed to by PsActiveProcessHead and shows the offset, process name, process ID, the parent process ID, number of threads, number of handles, and date/time when the process started and exited. As of 2.1 it also shows the Session ID and if the process is a Wow64 process (it uses a 32 bit address space on a 64 bit kernel). <p></p> This plugin does not detect hidden or unlinked processes (but psscan can do that). <p></p> If you see processes with 0 threads, 0 handles, and/or a non-empty exit time, the process may not actually still be active.
memdump | To extract all memory resident pages in a process into an individual file, use the memdump command. Supply the output directory with -D or --dump-dir=DIR.

<p></p>
With these two options we can begin our analysis.
<br>
note: I have tried both dumping all Tor related files and this memdump way, I couldn't find anything useable when I dumped all files.

<P></P>
<details>
    <summary>Spoilers</summary>
<p></p>
The first thing we will do is list all the processes running using <kbd>pslist</kbd> the command looks like this:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 pslist
```

<p></p>
Which gives the following output:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 pslist                                                                                                                                    
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                                                                       
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------                                             
0xfffffa80018b0960 System                    4      0     97      541 ------      0 2020-08-02 02:21:39 UTC+0000                                                                              
0xfffffa80031c75d0 smss.exe                272      4      2       30 ------      0 2020-08-02 02:21:39 UTC+0000                                                                              
0xfffffa8003c2f060 csrss.exe               360    344      9      555      0      0 2020-08-02 02:21:40 UTC+0000                                                                              
0xfffffa800350d060 wininit.exe             400    344      3       81      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003df0b00 csrss.exe               412    392     11      558      1      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003e679a0 winlogon.exe            456    392      3      117      1      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003db3b00 services.exe            504    400      7      214      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003e03b00 lsass.exe               512    400      9      643      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003ea8b00 lsm.exe                 524    400     11      167      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003fd49c0 svchost.exe             644    504     12      375      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa800403e2f0 svchost.exe             716    504      9      317      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa8003fdcb00 svchost.exe             784    504     25      560      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa80040a2b00 svchost.exe             860    504     22      479      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa80040f3060 svchost.exe             912    504     15      644      0      0 2020-08-02 02:21:42 UTC+0000                                 
0xfffffa800410d5f0 svchost.exe             952    504     47     1307      0      0 2020-08-02 02:21:43 UTC+0000                                 
0xfffffa80030d1500 svchost.exe            1032    504     35      607      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa80034b36e0 spoolsv.exe            1132    504     15      289      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa8003474b00 svchost.exe            1164    504     21      344      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa800440ab00 svchost.exe            1244    504     11      315      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa8004411b00 pia-service.ex         1348    504     18      312      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa800199a6e0 VGAuthService.         1428    504      3       86      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa80044d3b00 vmtoolsd.exe           1488    504     11      299      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa8004561b00 WmiPrvSE.exe           1876    644     11      230      0      0 2020-08-02 02:21:46 UTC+0000                                 
0xfffffa80019986d0 dllhost.exe            1960    504     14      196      0      0 2020-08-02 02:21:46 UTC+0000                                 
0xfffffa8004684b00 msdtc.exe              1984    504     12      146      0      0 2020-08-02 02:21:47 UTC+0000                                 
0xfffffa80041c2700 taskhost.exe           2900    504      8      199      1      0 2020-08-02 02:22:02 UTC+0000                                 
0xfffffa8004aad7f0 dwm.exe                1792    860      5      160      1      0 2020-08-02 02:22:03 UTC+0000                                 
0xfffffa8004ab7b00 explorer.exe           2252   2180     28      882      1      0 2020-08-02 02:22:03 UTC+0000                                 
0xfffffa80040d7410 vm3dservice.ex         2464   2252      2       42      1      0 2020-08-02 02:22:04 UTC+0000                                 
0xfffffa80041f9b00 vmtoolsd.exe           2468   2252      8      181      1      0 2020-08-02 02:22:04 UTC+0000                                 
0xfffffa80045d6b00 SearchIndexer.         2420    504     13      712      0      0 2020-08-02 02:22:10 UTC+0000                                 
0xfffffa8001c48b00 sppsvc.exe             3620    504      4      153      0      0 2020-08-02 02:23:46 UTC+0000                                 
0xfffffa8001b4b060 svchost.exe            3708    504     13      347      0      0 2020-08-02 02:23:46 UTC+0000                                 
0xfffffa8001ad5b00 cmd.exe                3692   2252      1       22      1      0 2020-08-02 02:24:05 UTC+0000                                 
0xfffffa8001c3eb00 conhost.exe            3568    412      2       53      1      0 2020-08-02 02:24:05 UTC+0000                                 
0xfffffa8001ab1b00 chrome.exe             3704   2252     34      793      1      0 2020-08-02 02:24:08 UTC+0000                                 
0xfffffa8001bc9b00 chrome.exe             3696   3704      8       75      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c0a060 chrome.exe             1724   3704     10      230      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c3cb00 chrome.exe             3896   3704     33      469      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c6fb00 chrome.exe             1516   3704     16      272      1      0 2020-08-02 02:24:11 UTC+0000                                 
0xfffffa8001c71b00 pia-client.exe         2408   2252     13      181      1      0 2020-08-02 02:24:20 UTC+0000                                 
0xfffffa80024a0b00 wuauclt.exe            3152    952      4       97      1      0 2020-08-02 02:24:58 UTC+0000                                 
0xfffffa8004794b00 svchost.exe             848    504     10      142      0      0 2020-08-02 02:25:23 UTC+0000                                 
0xfffffa8002550360 pidgin.exe             4060   2252     13      262      1      1 2020-08-02 03:40:36 UTC+0000                                 
0xfffffa80024a4870 chrome.exe             3172   3704     15      394      1      0 2020-08-02 03:40:49 UTC+0000                                 
0xfffffa8002499b00 audiodg.exe             616    784      4      126      0      0 2020-08-02 03:52:48 UTC+0000                                 
0xfffffa8002f4eb00 SearchProtocol         3996   2420      7      326      0      0 2020-08-02 06:47:09 UTC+0000                                 
0xfffffa8004525b00 firefox.exe            3412   4012     55      813      1      0 2020-08-02 06:57:58 UTC+0000                                 
0xfffffa8002483820 firefox.exe            3424   3412     12      209      1      0 2020-08-02 06:57:58 UTC+0000                                 
0xfffffa80040ad910 tor.exe                2640   3412      6       77      1      0 2020-08-02 06:57:59 UTC+0000                                 
0xfffffa8004ac2930 SearchFilterHo         1904   2420      6      111      0      0 2020-08-02 06:57:59 UTC+0000                                 
0xfffffa8001d113e0 firefox.exe            2728   3412     28      352      1      0 2020-08-02 06:58:08 UTC+0000                                 
0xfffffa800250e770 firefox.exe            3380   3412     26      342      1      0 2020-08-02 06:58:09 UTC+0000                                 
0xfffffa8001aaeb00 firefox.exe            3756   3412     23      312      1      0 2020-08-02 06:58:13 UTC+0000                                 
0xfffffa8001ab9060 pia-openvpn.ex         1872   1348      2       78      0      0 2020-08-02 06:58:40 UTC+0000                                 
0xfffffa8002f84b00 conhost.exe            2832    360      1       33      0      0 2020-08-02 06:58:41 UTC+0000                                 
0xfffffa8001ceeb00 mobsync.exe             208    644      8      165      1      0 2020-08-02 06:58:45 UTC+0000                                 
0xfffffa80030155f0 cmd.exe                2064   1488      0 --------      0      0 2020-08-02 06:58:50 UTC+0000   2020-08-02 06:58:50 UTC+0000  
0xfffffa800256f770 conhost.exe            3392    360      0       30      0      0 2020-08-02 06:58:50 UTC+0000                                 
0xfffffa8002f88640 ipconfig.exe           3128   2064      0 --------      0      0 2020-08-02 06:58:50 UTC+0000   2020-08-02 06:58:50 UTC+0000  
```

<p></p>
From this we can see that Tor.exe has a PID of 2640 which we will need for the next step.
<p></p>
The next thing we will do is dump the memory files associated with the process. to do this we will use the <kbd>memdump</kbd> command. First we need to make a directory to dump to (mine is called tor_dump) The command looks like this:
<p></p>


```
sudo volatility -f dump.vmem --profile=Win7SP1x64 memdump -p 2640 --dump-dir=tor_dump
```

<p></p>
which gives the output of:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 memdump -p 2640 --dump-dir=tor_dump
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing tor.exe [  2640] to 2640.dmp
```

<p></p>
The memdump will output a file for us, running <kbd>file 2640.dmp</kbd> lets us know this is a dat file. From here we can run strings on the file (The strings command returns each string of printable characters in files. Its main uses are to determine the contents of and to extract text from binary files) and since we know we are searching for a .onion file we can pipe (|) the output of strings into grep searching for .onion the command looks like This:
<p></p>

```
strings 2640.dmp | grep .onion
```

<p></p>
This command outputs a ton of information but at the tail of the output is the information we are looking for.
<p></p>

```
https://xsstorweb56srs3a.onion/threads/29059/
rs3a.onion/threa
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
b56srs3a.onion
^privateBrowsingId=1&firstPartyDomain=xsstorweb56srs3a.onion
https://xsstorweb56srs3a.onion
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/styles/favicon.png
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
https://xsstorweb56srs3a.onion/threads/29059/
.onion/t@
https://xsstorweb56srs3a.onion/forums/140/
xsstorweb56srs3a.onion
```

This output gives us our answer.

<p></p>
<details>
    <summary>Answer</summary>
<p></p>
FLAG{xsstorweb56srs3a.onion}
<p></p>
</details>
</details>
</details>
</details>
</details>
</details>

<p></p>
<hr>
<p></p>
<details>
    <summary>Memory is RAM 5</summary>
<p></p>
The fifth challenge we are given is:
<p></p>
It looks like the attacker was talking to someone over XMPP about buying
<br>
a database, can you recover the communications and find the domain the database
<br>
belongs to?
<p></p>
Flag format: FLAG{domain}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
So what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- First what is XMPP? 
<br>
"Extensible Messaging and Presence Protocol (XMPP, originally named Jabber) is an open communication protocol designed for instant messaging (IM), presence information, and contact list maintenance. Based on XML (Extensible Markup Language), it enables the near-real-time exchange of structured data between any two or more network entities."
- So we know that when we list the processes we need to look for a messaging app, (if you have played with linux before the app will stick out)
<p></p>
With this information we can begine our analysis.
<p></p>
<hr>
<p></p>
Now that we know what we are looking for what options can we use?
<p></p>

Options | Description
--------|-------------------
pslist | To list the processes of a system, use the pslist command. This walks the doubly-linked list pointed to by PsActiveProcessHead and shows the offset, process name, process ID, the parent process ID, number of threads, number of handles, and date/time when the process started and exited. As of 2.1 it also shows the Session ID and if the process is a Wow64 process (it uses a 32 bit address space on a 64 bit kernel). <p></p> This plugin does not detect hidden or unlinked processes (but psscan can do that). <p></p> If you see processes with 0 threads, 0 handles, and/or a non-empty exit time, the process may not actually still be active.
netscan | To scan for network artifacts in 32- and 64-bit Windows Vista, Windows 2008 Server and Windows 7 memory dumps, use the netscan command. This finds TCP endpoints, TCP listeners, UDP endpoints, and UDP listeners. It distinguishes between IPv4 and IPv6, prints the local and remote IP (if applicable), the local and remote port (if applicable), the time when the socket was bound or when the connection was established, and the current state (for TCP connections only).

<p></p>
With these two options we can now begin our analysis.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
The first thing we will do is lust all of the processes using <kbd>pslist</kbd> the command looks like this:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 pslist
```

<p></p>
Which gives an output of:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 pslist                                                                                                                                    
[sudo] password for parrot:                                                                                                                                                                   
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                                                                       
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------                                             
0xfffffa80018b0960 System                    4      0     97      541 ------      0 2020-08-02 02:21:39 UTC+0000                                                                              
0xfffffa80031c75d0 smss.exe                272      4      2       30 ------      0 2020-08-02 02:21:39 UTC+0000                                                                              
0xfffffa8003c2f060 csrss.exe               360    344      9      555      0      0 2020-08-02 02:21:40 UTC+0000                                                                              
0xfffffa800350d060 wininit.exe             400    344      3       81      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003df0b00 csrss.exe               412    392     11      558      1      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003e679a0 winlogon.exe            456    392      3      117      1      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003db3b00 services.exe            504    400      7      214      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003e03b00 lsass.exe               512    400      9      643      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003ea8b00 lsm.exe                 524    400     11      167      0      0 2020-08-02 02:21:41 UTC+0000                                                                              
0xfffffa8003fd49c0 svchost.exe             644    504     12      375      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa800403e2f0 svchost.exe             716    504      9      317      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa8003fdcb00 svchost.exe             784    504     25      560      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa80040a2b00 svchost.exe             860    504     22      479      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa80040f3060 svchost.exe             912    504     15      644      0      0 2020-08-02 02:21:42 UTC+0000                                                                              
0xfffffa800410d5f0 svchost.exe             952    504     47     1307      0      0 2020-08-02 02:21:43 UTC+0000                                                                              
0xfffffa80030d1500 svchost.exe            1032    504     35      607      0      0 2020-08-02 02:21:44 UTC+0000                                                                              
0xfffffa80034b36e0 spoolsv.exe            1132    504     15      289      0      0 2020-08-02 02:21:44 UTC+0000                                                                              
0xfffffa8003474b00 svchost.exe            1164    504     21      344      0      0 2020-08-02 02:21:44 UTC+0000                                                                              
0xfffffa800440ab00 svchost.exe            1244    504     11      315      0      0 2020-08-02 02:21:44 UTC+0000                                 
0xfffffa8004411b00 pia-service.ex         1348    504     18      312      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa800199a6e0 VGAuthService.         1428    504      3       86      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa80044d3b00 vmtoolsd.exe           1488    504     11      299      0      0 2020-08-02 02:21:45 UTC+0000                                 
0xfffffa8004561b00 WmiPrvSE.exe           1876    644     11      230      0      0 2020-08-02 02:21:46 UTC+0000                                 
0xfffffa80019986d0 dllhost.exe            1960    504     14      196      0      0 2020-08-02 02:21:46 UTC+0000                                 
0xfffffa8004684b00 msdtc.exe              1984    504     12      146      0      0 2020-08-02 02:21:47 UTC+0000                                 
0xfffffa80041c2700 taskhost.exe           2900    504      8      199      1      0 2020-08-02 02:22:02 UTC+0000                                 
0xfffffa8004aad7f0 dwm.exe                1792    860      5      160      1      0 2020-08-02 02:22:03 UTC+0000                                 
0xfffffa8004ab7b00 explorer.exe           2252   2180     28      882      1      0 2020-08-02 02:22:03 UTC+0000                                 
0xfffffa80040d7410 vm3dservice.ex         2464   2252      2       42      1      0 2020-08-02 02:22:04 UTC+0000                                 
0xfffffa80041f9b00 vmtoolsd.exe           2468   2252      8      181      1      0 2020-08-02 02:22:04 UTC+0000                                 
0xfffffa80045d6b00 SearchIndexer.         2420    504     13      712      0      0 2020-08-02 02:22:10 UTC+0000                                 
0xfffffa8001c48b00 sppsvc.exe             3620    504      4      153      0      0 2020-08-02 02:23:46 UTC+0000                                 
0xfffffa8001b4b060 svchost.exe            3708    504     13      347      0      0 2020-08-02 02:23:46 UTC+0000                                 
0xfffffa8001ad5b00 cmd.exe                3692   2252      1       22      1      0 2020-08-02 02:24:05 UTC+0000                                 
0xfffffa8001c3eb00 conhost.exe            3568    412      2       53      1      0 2020-08-02 02:24:05 UTC+0000                                 
0xfffffa8001ab1b00 chrome.exe             3704   2252     34      793      1      0 2020-08-02 02:24:08 UTC+0000                                 
0xfffffa8001bc9b00 chrome.exe             3696   3704      8       75      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c0a060 chrome.exe             1724   3704     10      230      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c3cb00 chrome.exe             3896   3704     33      469      1      0 2020-08-02 02:24:09 UTC+0000                                 
0xfffffa8001c6fb00 chrome.exe             1516   3704     16      272      1      0 2020-08-02 02:24:11 UTC+0000                                 
0xfffffa8001c71b00 pia-client.exe         2408   2252     13      181      1      0 2020-08-02 02:24:20 UTC+0000                                 
0xfffffa80024a0b00 wuauclt.exe            3152    952      4       97      1      0 2020-08-02 02:24:58 UTC+0000                                 
0xfffffa8004794b00 svchost.exe             848    504     10      142      0      0 2020-08-02 02:25:23 UTC+0000                                 
0xfffffa8002550360 pidgin.exe             4060   2252     13      262      1      1 2020-08-02 03:40:36 UTC+0000                                 
0xfffffa80024a4870 chrome.exe             3172   3704     15      394      1      0 2020-08-02 03:40:49 UTC+0000                                 
0xfffffa8002499b00 audiodg.exe             616    784      4      126      0      0 2020-08-02 03:52:48 UTC+0000                                 
0xfffffa8002f4eb00 SearchProtocol         3996   2420      7      326      0      0 2020-08-02 06:47:09 UTC+0000                                 
0xfffffa8004525b00 firefox.exe            3412   4012     55      813      1      0 2020-08-02 06:57:58 UTC+0000                                 
0xfffffa8002483820 firefox.exe            3424   3412     12      209      1      0 2020-08-02 06:57:58 UTC+0000                                 
0xfffffa80040ad910 tor.exe                2640   3412      6       77      1      0 2020-08-02 06:57:59 UTC+0000                                 
0xfffffa8004ac2930 SearchFilterHo         1904   2420      6      111      0      0 2020-08-02 06:57:59 UTC+0000                                 
0xfffffa8001d113e0 firefox.exe            2728   3412     28      352      1      0 2020-08-02 06:58:08 UTC+0000                                 
0xfffffa800250e770 firefox.exe            3380   3412     26      342      1      0 2020-08-02 06:58:09 UTC+0000                                 
0xfffffa8001aaeb00 firefox.exe            3756   3412     23      312      1      0 2020-08-02 06:58:13 UTC+0000                                 
0xfffffa8001ab9060 pia-openvpn.ex         1872   1348      2       78      0      0 2020-08-02 06:58:40 UTC+0000                                 
0xfffffa8002f84b00 conhost.exe            2832    360      1       33      0      0 2020-08-02 06:58:41 UTC+0000                                 
0xfffffa8001ceeb00 mobsync.exe             208    644      8      165      1      0 2020-08-02 06:58:45 UTC+0000                                 
0xfffffa80030155f0 cmd.exe                2064   1488      0 --------      0      0 2020-08-02 06:58:50 UTC+0000   2020-08-02 06:58:50 UTC+0000  
0xfffffa800256f770 conhost.exe            3392    360      0       30      0      0 2020-08-02 06:58:50 UTC+0000                                 
0xfffffa8002f88640 ipconfig.exe           3128   2064      0 --------      0      0 2020-08-02 06:58:50 UTC+0000   2020-08-02 06:58:50 UTC+0000  
```

<p></p>
Through some reasearch we find that pidgin is an XMPP application, we also need to note the PID of 4060.
<br>
"Pidgin is a chat program which lets you log into accounts on multiple chat networks simultaneously. This means that you can be chatting with friends on XMPP and sitting in an IRC channel at the same time."
<p></p>

```
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit                                                                       
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------                                             
0xfffffa8002550360 pidgin.exe             4060   2252     13      262      1      1 2020-08-02 03:40:36 UTC+0000                                 
```

<p></p>
Now that we know which application we are looking at we can now dump the memory associated with it IOT investigate further, so we will now use the <kbd>memdump</kbd> option after making a dump directory <kbd>mkdir pidgin_dump</kbd>, the command looks like this:
<p></p>

```
sudo volatility -f dump.vmem --profile=Win7SP1x64 memdump -D pidgin_dump -p 4060
```

<p></p>
Which gives an output of:
<p></p>

```
❯ sudo volatility -f dump.vmem --profile=Win7SP1x64 memdump -D pidgin_dump -p 4060
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing pidgin.exe [  4060] to 4060.dmp
```

<p></p>
Now that we have a file we can analyse we can again run strings on the file (my system lagged while running strings and pipping to another command so a tip is to run strings on 4060.dmp and append it to a text file <kbd>strings 4060.dmp > out.txt</kbd> we can then run other commands much quicker).
<br>
Now that we have a text file we can <kbd>cat</kbd> the file and pipe it to grep to search for the database we are looking for.
<br>
If you aren't familiar with <kbd>head</kbd> <kbd>tail</kbd> <kbd>less</kbd> or <kbd>more</kbd> you should start as it allows us to more managebly view large outputs of information try all on this file and see the differences (<kbd>q</kbd> will quit out of <kbd>less</kbd>). The command will look like this:
<p></p>

```
cat out.txt | grep database | less
```

<p></p>
Which gives and output of:
<p></p>

```
❯ cat out.txt | grep database | less                                                                                                                                                           
just looking to see if you had a copy of that database we spoke about a while back, i've seen its been circulating a bit but havent managed to grab a copy myself                             
just looking to see if you had a copy of that database we spoke about a while back, i've seen its been circulating a bit but havent managed to grab a copy myself                             
st looking to see if you had a copy of that database we spoke about a while back, i've seen its been circulating a bit but havent managed to grab a copy myself                               
just looking to see if you had a copy of that database we spoke about a while back, i've seen its been circulating a bit but havent managed to grab a copy myself                             
all good, it was the aus-offensive.com.au database, think it was the jan-2020 version? willing to drop 0.3 btc for a copy if you have it                                                      
all good, it was the aus-offensive.com.au database, think it was the jan-2020 version? willing to drop 0.3 btc for a copy if you have it                                                      
all good, it was the aus-offensive.com.au database, think it was the jan-2020 version? willing to drop 0.3 btc for a copy if you have it                                                      
all good, it was the aus-offensive.com.au database, think it was the jan-2020 version? willing to drop 0.3 btc for a copy if you have it                                                      
all good, it was the aus-offensive.com.au database, think it was the jan-2020 version? willing to drop 0.3 btc for a copy if you have it                                                      
:57:04 PM) yung_mendax@xmpp.jp/24175827678445536902550094050: all good, it was the aus-offensive.com.au database, think it was the jan-2020 version? willing to drop 0.3 btc for a copy if you
 have it                                                                                                                                                                                      
:55:38 PM) yung_mendax@xmpp.jp/24175827678445536902550094050: just looking to see if you had a copy of that database we spoke about a while back, i've seen its been circulating a bit but hav
ent managed to grab a copy myself                                                                                                                                                             
just looking to see if you had a copy of that database we spoke about a while back, i've seen its been circulating a bit but havent managed to grab a copy myself                             
st looking to see if you had a copy of that database we spoke about a while back, i've seen its been circulating a bit but havent managed to grab a copy myself b a copy myself               
no secret in database                                                                                                                                                                         
no secret in database                                                                                                                                                                         
transitioning user %s to auxprop database
file is encrypted or is not a database
auxiliary database format error
database schema has changed
unable to open database file
database or disk is full
database disk image is malformed
attempt to write a readonly database
database table is locked
database is locked
SQL logic error or missing database
database_list
database schema is locked: %s
database table is locked: %s
The certificate/key database is in an old, unsupported format.
You have no database of root certificates, so this certificate cannot be validated.
```

<p></p>
This shows us the answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
FLAG{aus-offensive.com.au}
</details>
</details>
</details>
</details>
</details>

<p></p>
<hr>
<p></p>
<details>
    <summary>Memory is RAM 6</summary>
<p></p>
The final challenge we are given is:
<p></p>
It seems like the attacker uses LastPass to store their credentials,
<br>
can you recover their password for the xss.is forum?
<p></p>
Flag format: FLAG{password}
<p></p>
<details>
    <summary>Walkthrough</summary>

<p></p>
So what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- We will need a way to extract passwords from LastPass but what is Last Pass?
<br>
"LastPass is a secure password manager that stores all of your usernames and passwords in one safe place, called a Vault. After you save a password to your Vault, LastPass always remembers it for you. When you need to log in to a website, LastPass enters your username and password for you!"
<p></p>
- The password we need is for xss.is
<p></p>
With that information we can start our analysis.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
So now we know what we are looking for what options can we use?
<p></p>
This is the first challenge where we will introduce the use of plugins to assist with our analysis.
<br>
A quick google for "lastpass volatility" will direct us to a github site that has the plugin we need, https://github.com/kevthehermit/volatility_plugins
<br>
So what does this plugin do?
<p></p>

Plugin | Description
-------|--------------
lastpass | Read browser memory space and attempt to recover any resident artefact's. <p></p> IMPORTANT <p></p> It is important to note that this process only works if a memory image was taken whilst the lastpass plugin was running in active browser. And will only find credentials for domains that are active in a tab

<p></p>
So now that we have found the plugin we will use we will now clone it so we can use it in our analysis.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
The first thing we need to do is clone the git repository containg the plugin so we can use it, the command looks like this:
<p></p>

```
git clone https://github.com/kevthehermit/volatility_plugins.git
```

<p></p>
Which gives us the output of:
<p></p>

```
❯ git clone https://github.com/kevthehermit/volatility_plugins.git
Cloning into 'volatility_plugins'...
remote: Enumerating objects: 90, done.
remote: Total 90 (delta 0), reused 0 (delta 0), pack-reused 90
Receiving objects: 100% (90/90), 20.28 KiB | 944.00 KiB/s, done.
Resolving deltas: 100% (47/47), done.
```

<p></p>
you need to get the location for the plugin, the easiest way is to go into the folder containing the plugin and use the command <kbd>pwd</kbd>, once you have the file path you will need to use the <kbd>-plugin=</kbd> flag (note this flag must come straight after the volatility command or it wont work) the command looks like this:
<p></p>


```
sudo volatility --plugins=/home/parrot/ctf/csc/Preseason/Memory_is_RAM/volatility_plugins/lastpass -f dump.vmem --profile=Win7SP1x64 lastpass
```

<p></p>
The output looks like this:
<p></p>

```
❯ sudo volatility --plugins=/home/parrot/ctf/csc/Preseason/Memory_is_RAM/volatility_plugins/lastpass -f dump.vmem --profile=Win7SP1x64 lastpass
Volatility Foundation Volatility Framework 2.6
Searching for LastPass Signatures
Found pattern in Process: chrome.exe (3704)
Found pattern in Process: chrome.exe (3704)
Found pattern in Process: chrome.exe (3704)
Found pattern in Process: chrome.exe (1516)
Found pattern in Process: chrome.exe (1516)
Found pattern in Process: chrome.exe (1516)
Found pattern in Process: chrome.exe (1516)
Found pattern in Process: chrome.exe (1516)
Found pattern in Process: chrome.exe (1516)
Found pattern in Process: chrome.exe (3172)
Found pattern in Process: chrome.exe (3172)
Found pattern in Process: chrome.exe (3172)
Found pattern in Process: chrome.exe (3172)

Found LastPass Entry for xss.is
UserName: yung_mendax
Pasword: 8QUGzXSd2WXTHCe1ZW
```

<p></p>
Which gives us the answer:
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
FLAG{8QUGzXSd2WXTHCe1ZW}
<p></p>
</details>
</details>
</details>
</details>
</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<H3>Memory Forensics</H3>
<p></p>
Is a series of challenges from the <a href="https://ctf.hackfest.tn/" rel="nofollow">Hackfest</a> from 2019, these challenges will use the file lab.raw
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1lPkhMQa4vFLyteN2p1yT27wVsZtMVFAI/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
The answers for these challenge do not have 'flag' infront of them and simply require subbmitting the answer into the answer box on the ctf site.
<p></p>
<b>Do the first challenge before extracting the file.</b>
<p></p>
<details>
    <summary>Extracting the File</summary>
<p></p>
<b>Do the first challenge before extracting the file.</b>
<p></p>
When you download it the file comes as memlab_c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6.7z you can either extract it through an archive manager or through CLI which I will explain now.
<p></p>
The first thing you will need is to install p7zip-full if you haven't already. To do this run this command:
<p></p>

```
sudo apt install p7zip-full
```

Now that's installed you can run this command on the file:

```
p7zip -d memlab_c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6.7z
```

Which will output this and extract lab.raw:

```
❯ p7zip -d memlab_c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6.7z

7-Zip (a) [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_AU.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-7920HQ CPU @ 3.10GHz (906E9),ASM,AES-NI)

Scanning the drive for archives:
1 file, 392775032 bytes (375 MiB)

Extracting archive: memlab_c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6.7z
--
Path = memlab_c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6.7z
Type = 7z
Physical Size = 392775032
Headers Size = 122
Method = LZMA2:24
Solid = -
Blocks = 1

Everything is Ok

Size:       2147418112
Compressed: 392775032
```

<p></p>
Once we determine the image profile using <kbd>imageinfo</kbd> (refer to Getting your Foothold ) we can begin our analysis on lab.raw
<p></p>
</details>

<details>
    <summary>Challenges</summary>
<p></p>
<details>
    <summary>Memory Forensics Lab - Question 1</summary>
<p></p>
<b>Overview:</b>
<br>
Memory forensic refers to the process of investigating a memory dump to locate malicious behaviors. The dump is a snapshot capture of RAM memory at a specific point of time; it can be a full physical memory dump, a crash dump or a hibernation file.
<br>
As investigator, this lab will guide you to extract useful artifacts from a given memory snapshot, including running processes, URLs, passwords, encryption keys, open sockets and active connections, open registry keys. That information can be accessed by obtaining and analyzing the attached memory dump.
Memory dump acquisition for this lab has been performed using "DumpIt" program which has been already installed on the system where the memory dump has been acquired.
<p></p>
<b>Scenario:</b>
<br>
A machine has been compromised by a malware and important files have been encrypted. Our job as memory forensics experts is to determine how the malware went into the machine, understand its internal and attempt to recover the important file.
<p></p>
<b>Question:</b>
<br>
For verification purpose, what is the SHA256 sum of the attached file ?
c5f377..
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
To do this we run <kbd>sha256sum</kbd> on memlab_c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6.7z the command and output looks like this:
<p></p>

```
❯ sha256sum memlab_c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6.7z                                                                                                        
c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6  memlab_c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6.7z                                                  
```

<p></p>
Which gives us the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
c5f3774eb6ce36405d9a2f8ecb45ef71b3a0a702660587a6b6a357b12d6171f6

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 2</summary>
<p></p>
 What is the Operating System of the machine being investigated?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
To get the flag for this challenge we weed to run the imageinfo option to discover what the image OS is. The command and output looks like this:
<p></p>

```
sudo volatility -f lab.raw imageinfo
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f lab.raw imageinfo
Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/parrot/ctf/hackfest/Memory_Forensics/lab.raw)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf80002846070L
          Number of Processors : 1
     Image Type (Service Pack) : 0
                KPCR for CPU 0 : 0xfffff80002847d00L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2019-12-06 19:54:14 UTC+0000
     Image local date and time : 2019-12-06 11:54:14 -0800
```

<p></p>
Looking through the output we go to the suggested profiles and use the first suggested profile. The flag is the whole name of the OS.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
Windows 7

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 3</summary>
<p></p>
 What is the computer Name ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
There are two methods to obtain the flag for this challenge, the first and simpler way is using the envars option and the second is pulling the registry sub key that contains the computer name (I have written a better explaination of pulling registry subkeys in the Rick-GeneralInfo challenge). I will go over the envars method first. The command looks like this:
<p></p>

```
sudo volatility -f lab.raw --profile=Win7SP1x64 envars | tee envars.txt
```

<p></p>
You can see that I piped (|) the output to tee and a .txt file, this is so I can see the output (So I know what the file looks like, and I can better structure my grep command). Now that we have a .txt file we can run <kbd>grep</kbd> against the file ans search for the computer name. The command looks like this:
<p></p>

```
cat envar.txt | grep -i computername
```

<p></p>
Which outputs:
<p></p>

```
❯ cat envars.txt| grep -i computername                                                                                                                                                        
     380 wininit.exe          0x0000000000299ae0 COMPUTERNAME                   LAB-VM-C57C                                                                                                   
     432 winlogon.exe         0x000000000036e490 COMPUTERNAME                   LAB-VM-C57C                                                                                                   
     476 services.exe         0x0000000000111320 COMPUTERNAME                   LAB-VM-C57C                                                                                                   
     484 lsass.exe            0x0000000000331320 COMPUTERNAME                   LAB-VM-C57C
     492 lsm.exe              0x0000000000261320 COMPUTERNAME                   LAB-VM-C57C
     596 svchost.exe          0x00000000002a1320 COMPUTERNAME                   LAB-VM-C57C
     656 VBoxService.ex       0x0000000000071320 COMPUTERNAME                   LAB-VM-C57C
     712 svchost.exe          0x0000000000211320 COMPUTERNAME                   LAB-VM-C57C
     768 svchost.exe          0x0000000000121320 COMPUTERNAME                   LAB-VM-C57C
     892 svchost.exe          0x0000000000441320 COMPUTERNAME                   LAB-VM-C57C
     920 svchost.exe          0x00000000003e1320 COMPUTERNAME                   LAB-VM-C57C
     340 svchost.exe          0x0000000000351320 COMPUTERNAME                   LAB-VM-C57C
     372 svchost.exe          0x0000000000271320 COMPUTERNAME                   LAB-VM-C57C
    1132 dwm.exe              0x0000000000371320 COMPUTERNAME                   LAB-VM-C57C
    1144 explorer.exe         0x0000000003970b10 COMPUTERNAME                   LAB-VM-C57C
    1200 spoolsv.exe          0x0000000000321320 COMPUTERNAME                   LAB-VM-C57C
    1256 taskhost.exe         0x0000000000281320 COMPUTERNAME                   LAB-VM-C57C
    1284 svchost.exe          0x0000000000291320 COMPUTERNAME                   LAB-VM-C57C
    1404 svchost.exe          0x0000000000261320 COMPUTERNAME                   LAB-VM-C57C
    1556 VBoxTray.exe         0x0000000000321320 COMPUTERNAME                   LAB-VM-C57C
    1564 BitTorrent.exe       0x0000000000291320 COMPUTERNAME                   LAB-VM-C57C
    1572 StikyNot.exe         0x0000000000151320 COMPUTERNAME                   LAB-VM-C57C
    1608 SearchIndexer.       0x00000000001ea9e0 COMPUTERNAME                   LAB-VM-C57C
    2160 bittorrentie.e       0x0000000000091320 COMPUTERNAME                   LAB-VM-C57C
    2200 bittorrentie.e       0x0000000000581320 COMPUTERNAME                   LAB-VM-C57C
    2268 GoogleCrashHan       0x0000000000161320 COMPUTERNAME                   LAB-VM-C57C
    2276 GoogleCrashHan       0x0000000000411320 COMPUTERNAME                   LAB-VM-C57C
    2488 wmpnetwk.exe         0x00000000000f3740 COMPUTERNAME                   LAB-VM-C57C
    3052 cmd.exe              0x0000000000278ab0 COMPUTERNAME                   LAB-VM-C57C
    2796 regedit.exe          0x0000000000111320 COMPUTERNAME                   LAB-VM-C57C
    2396 chrome.exe           0x0000000000bf4e20 COMPUTERNAME                   LAB-VM-C57C
    2876 chrome.exe           0x0000000000af1320 COMPUTERNAME                   LAB-VM-C57C
    2856 chrome.exe           0x0000000000a61320 COMPUTERNAME                   LAB-VM-C57C
    3032 chrome.exe           0x0000000000061320 COMPUTERNAME                   LAB-VM-C57C
    1092 chrome.exe           0x0000000000b61320 COMPUTERNAME                   LAB-VM-C57C
    3204 chrome.exe           0x0000000000b31320 COMPUTERNAME                   LAB-VM-C57C
    3492 chrome.exe           0x0000000000a61320 COMPUTERNAME                   LAB-VM-C57C
    3576 chrome.exe           0x0000000000941320 COMPUTERNAME                   LAB-VM-C57C
    3688 WmiPrvSE.exe         0x0000000000361320 COMPUTERNAME                   LAB-VM-C57C
    3960 sppsvc.exe           0x00000000002e1320 COMPUTERNAME                   LAB-VM-C57C
    1960 svchost.exe          0x000000000039c3c0 COMPUTERNAME                   LAB-VM-C57C
    1592 mirc.exe             0x0000000001631320 COMPUTERNAME                   LAB-VM-C57C
    3660 WmiApSrv.exe         0x0000000000251320 COMPUTERNAME                   LAB-VM-C57C
    1492 wuauclt.exe          0x0000000000291320 COMPUTERNAME                   LAB-VM-C57C
    2128 SearchProtocol       0x0000000000301320 COMPUTERNAME                   LAB-VM-C57C
    3424 crypt0r.exe          0x00000000003b1320 COMPUTERNAME                   LAB-VM-C57C
    1528 SearchFilterHo       0x0000000000381320 COMPUTERNAME                   LAB-VM-C57C
     376 notepad.exe          0x0000000000321320 COMPUTERNAME                   LAB-VM-C57C
    3820 notepad.exe          0x00000000002a1320 COMPUTERNAME                   LAB-VM-C57C
    3076 DumpIt.exe           0x0000000000251320 COMPUTERNAME                   LAB-VM-C57C
```

<p></p>
Here we can see the computer name and the flag for this challenge.
<p></p>
I will now go over pulling the registry sub key that contains the computer name. First we need to discover where in the registry the computer name is held. A quick google shows:
<p></p>
"This key shows computer name in all Windows versions – Windows 7, 8 and 10. HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\ComputerName\ActiveComputerName"
<p></p>
Now that we know where the registry key is located we need to list the registry hives using the hivelist option. The command looks like this:
<p></p>

```
sudo volatility -f lab.raw --profile=Win7SP1x64 hivelist
```

<p></p>
Which outputs this:
<p></p>

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 hivelist
Volatility Foundation Volatility Framework 2.6
Virtual            Physical           Name
------------------ ------------------ ----
0xfffff8a00000d010 0x000000002d5b3010 [no name]
0xfffff8a000024010 0x000000002d4d8010 \REGISTRY\MACHINE\SYSTEM
0xfffff8a00004e010 0x000000002d4c2010 \REGISTRY\MACHINE\HARDWARE
0xfffff8a00011f010 0x000000002b229010 \SystemRoot\System32\Config\SECURITY
0xfffff8a0001af010 0x000000002920a010 \SystemRoot\System32\Config\SOFTWARE
0xfffff8a000dcd420 0x000000001e5d8420 \??\C:\Windows\ServiceProfiles\NetworkService\NTUSER.DAT
0xfffff8a000e5a010 0x0000000021de6010 \??\C:\Windows\ServiceProfiles\LocalService\NTUSER.DAT
0xfffff8a00100e3f0 0x000000001d5eb3f0 \??\C:\Users\maro\AppData\Local\Microsoft\Windows\UsrClass.dat
0xfffff8a0014c7010 0x00000000211ba010 \??\C:\System Volume Information\Syscache.hve
0xfffff8a0036ab010 0x000000002827d010 \SystemRoot\System32\Config\DEFAULT
0xfffff8a0036bf010 0x00000000281ed010 \SystemRoot\System32\Config\SAM
0xfffff8a005d2f420 0x0000000018fc8420 \??\C:\Users\maro\ntuser.dat
0xfffff8a005dcc420 0x00000000290e8420 \Device\HarddiskVolume1\Boot\BCD
```

<p></p>
We know from our research that the key is located in the SYSTEM hive, we will use this information in the next step using the printkey option. The command looks like this:
<p></p>

```
sudo volatility -f lab.raw --profile=Win7SP1x64 printkey -o 0xfffff8a000024010
```

<p></p>
which outputs:
<p></p>

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 printkey -o 0xfffff8a000024010
Volatility Foundation Volatility Framework 2.6
Legend: (S) = Stable   (V) = Volatile

----------------------------
Registry: \REGISTRY\MACHINE\SYSTEM
Key name: CMI-CreateHive{2A7FB991-7BBE-4F9D-B91E-7CB51D4737F5} (S)
Last updated: 2019-12-06 19:47:06 UTC+0000

Subkeys:
  (S) ControlSet001
  (S) ControlSet002
  (S) MountedDevices
  (S) RNG
  (S) Select
  (S) Setup
  (S) WPA
  (V) CurrentControlSet

Values:
```

<p></p>
Here we can see the subkeys listed, we do this step so that we can see the ControlSet001 subkey which we need to use with the research we did earlier, the next command looks like this:
<p></p>

```
sudo volatility -f lab.raw --profile=Win7SP1x64 printkey -o 0xfffff8a000024010 -K 'ControlSet001\Control\ComputerName\ComputerName'
```

<p></p>
Which outputs this:
<p></p>

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 printkey -o 0xfffff8a000024010 -K 'ControlSet001\Control\ComputerName\ComputerName'
Volatility Foundation Volatility Framework 2.6
Legend: (S) = Stable   (V) = Volatile

----------------------------
Registry: \REGISTRY\MACHINE\SYSTEM
Key name: ComputerName (S)
Last updated: 2019-12-06 15:21:08 UTC+0000

Subkeys:

Values:
REG_SZ                        : (S) mnmsrvc
REG_SZ        ComputerName    : (S) LAB-VM-C57C
```

<p></p>
Here we can see the computername in the output, giving us the answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
LAB-VM-C57C

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 4</summary>
<p></p>
What is the user's system password ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 5</summary>
<p></p>
What is the IP address of the machine?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
IOT get the flag for this challenge we need to list all the network connection, we will do this using the netscan option. The command looks like this:
<p></p>

```
sudo volatility -f lab.raw --profile=Win7SP1x64 netscan
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 netscan                                                                                                                            [50/3469]
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x7dc293f0         UDPv6    fe80::a0e4:262f:80eb:3d68:1900 *:*                                   1404     svchost.exe    2019-12-06 19:47:15 UTC+0000                                         
0x7dc34d00         UDPv4    10.0.2.15:1900                 *:*                                   1404     svchost.exe    2019-12-06 19:47:15 UTC+0000                                         
0x7dc6b460         UDPv4    0.0.0.0:3702                   *:*                                   340      svchost.exe    2019-12-06 19:47:17 UTC+0000                                         
0x7dc6b460         UDPv6    :::3702                        *:*                                   340      svchost.exe    2019-12-06 19:47:17 UTC+0000                                         
0x7dc71d00         UDPv4    127.0.0.1:64509                *:*                                   1404     svchost.exe    2019-12-06 19:47:15 UTC+0000                                         
0x7dc782d0         UDPv4    10.0.2.15:64508                *:*                                   1404     svchost.exe    2019-12-06 19:47:15 UTC+0000                                         
0x7dc79a40         UDPv6    fe80::a0e4:262f:80eb:3d68:64506 *:*                                   1404     svchost.exe    2019-12-06 19:47:15 UTC+0000                                        
0x7dc7a380         UDPv4    127.0.0.1:1900                 *:*                                   1404     svchost.exe    2019-12-06 19:47:15 UTC+0000                                         
0x7dcee460         UDPv4    0.0.0.0:3702                   *:*                                   1404     svchost.exe    2019-12-06 19:47:17 UTC+0000                                         
0x7dcee460         UDPv6    :::3702                        *:*                                   1404     svchost.exe    2019-12-06 19:47:17 UTC+0000                                         
0x7dcef010         UDPv4    0.0.0.0:57684                  *:*                                   340      svchost.exe    2019-12-06 19:47:18 UTC+0000                                         
0x7dcf1550         UDPv4    0.0.0.0:63041                  *:*                                   340      svchost.exe    2019-12-06 19:47:16 UTC+0000                                         
0x7dcf3bb0         UDPv4    0.0.0.0:63042                  *:*                                   340      svchost.exe    2019-12-06 19:47:16 UTC+0000                                         
0x7dcf3bb0         UDPv6    :::63042                       *:*                                   340      svchost.exe    2019-12-06 19:47:16 UTC+0000                                         
0x7dcfb9e0         UDPv4    0.0.0.0:57685                  *:*                                   340      svchost.exe    2019-12-06 19:47:18 UTC+0000                                         
0x7dcfb9e0         UDPv6    :::57685                       *:*                                   340      svchost.exe    2019-12-06 19:47:18 UTC+0000                                         
0x7dcfbb30         UDPv4    0.0.0.0:3702                   *:*                                   1404     svchost.exe    2019-12-06 19:47:17 UTC+0000                                         
0x7dcfbcb0         UDPv4    0.0.0.0:3702                   *:*                                   340      svchost.exe    2019-12-06 19:47:17 UTC+0000                                         
0x7dd48360         UDPv4    0.0.0.0:64770                  *:*                                   2160     bittorrentie.e 2019-12-06 19:47:41 UTC+0000                                         
0x7dd48360         UDPv6    :::64770                       *:*                                   2160     bittorrentie.e 2019-12-06 19:47:41 UTC+0000                                         
0x7dd484b0         UDPv4    127.0.0.1:55621                *:*                                   2160     bittorrentie.e 2019-12-06 19:47:41 UTC+0000                                         
0x7dfd47e0         UDPv4    0.0.0.0:3702                   *:*                                   1404     svchost.exe    2019-12-06 19:47:17 UTC+0000                                         
0x7dfe9010         UDPv4    10.0.2.15:6771                 *:*                                   1564     BitTorrent.exe 2019-12-06 19:47:13 UTC+0000                                         
0x7dffd8f0         UDPv4    0.0.0.0:0                      *:*                                   372      svchost.exe    2019-12-06 19:47:13 UTC+0000                                         
0x7dffd8f0         UDPv6    :::0                           *:*                                   372      svchost.exe    2019-12-06 19:47:13 UTC+0000                                         
0x7e145b30         UDPv4    0.0.0.0:5355                   *:*                                   372      svchost.exe    2019-12-06 19:47:15 UTC+0000                                         
0x7e1dc820         UDPv4    10.0.2.15:137                  *:*                                   4        System         2019-12-06 19:47:13 UTC+0000                                         
0x7e27e240         UDPv4    0.0.0.0:46690                  *:*                                   1564     BitTorrent.exe 2019-12-06 19:47:11 UTC+0000                                         
0x7e296c20         UDPv4    0.0.0.0:0                      *:*                                   656      VBoxService.ex 2019-12-06 19:54:14 UTC+0000                                         
0x7e32a8b0         UDPv6    ::1:64507                      *:*                                   1404     svchost.exe    2019-12-06 19:47:15 UTC+0000                                         
0x7e3612b0         UDPv4    127.0.0.1:6771                 *:*                                   1564     BitTorrent.exe 2019-12-06 19:47:13 UTC+0000                                         
0x7e375b80         UDPv4    0.0.0.0:0                      *:*                                   1564     BitTorrent.exe 2019-12-06 19:47:31 UTC+0000                                         
0x7e3d36d0         UDPv4    0.0.0.0:5353                   *:*                                   2396     chrome.exe     2019-12-06 19:48:01 UTC+0000
0x7e3f75c0         UDPv4    0.0.0.0:3702                   *:*                                   1404     svchost.exe    2019-12-06 19:47:17 UTC+0000
0x7e3f75c0         UDPv6    :::3702                        *:*                                   1404     svchost.exe    2019-12-06 19:47:17 UTC+0000
0x7dc22010         TCPv4    0.0.0.0:49164                  0.0.0.0:0            LISTENING        484      lsass.exe      
0x7dc22010         TCPv6    :::49164                       :::0                 LISTENING        484      lsass.exe      
0x7dc47ce0         TCPv4    0.0.0.0:49164                  0.0.0.0:0            LISTENING        484      lsass.exe      
0x7e0008b0         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        920      svchost.exe    
0x7e0008b0         TCPv6    :::49154                       :::0                 LISTENING        920      svchost.exe    
0x7e219360         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        712      svchost.exe    
0x7e228d30         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        712      svchost.exe    
0x7e228d30         TCPv6    :::135                         :::0                 LISTENING        712      svchost.exe    
0x7e2322b0         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        380      wininit.exe    
0x7e234e60         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        380      wininit.exe    
0x7e234e60         TCPv6    :::49152                       :::0                 LISTENING        380      wininit.exe    
0x7e281770         TCPv4    0.0.0.0:46690                  0.0.0.0:0            LISTENING        1564     BitTorrent.exe 
0x7e2b8680         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        768      svchost.exe    
0x7e2b8680         TCPv6    :::49153                       :::0                 LISTENING        768      svchost.exe    
0x7e2bc480         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        768      svchost.exe    
0x7e3406f0         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        476      services.exe   
0x7e342a80         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        476      services.exe   
0x7e342a80         TCPv6    :::49155                       :::0                 LISTENING        476      services.exe   
0x7e365cd0         TCPv4    0.0.0.0:5357                   0.0.0.0:0            LISTENING        4        System         
0x7e365cd0         TCPv6    :::5357                        :::0                 LISTENING        4        System         
0x7e3fb280         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        920      svchost.exe    
0x7dc0c980         TCPv4    10.0.2.15:49206                94.125.182.252:6697  ESTABLISHED      1592     mirc.exe       
0x7dc49290         TCPv4    10.0.2.15:49219                193.70.38.49:80      CLOSE_WAIT       3424     crypt0r.exe    
0x7dd3b510         TCPv6    -:0                            387b:4803:80fa:ffff:387b:4803:80fa:ffff:0 CLOSED           1564     BitTorrent.exe 
0x7dd63010         TCPv4    -:0                            56.123.72.3:0        CLOSED           1564     BitTorrent.exe 
0x7df76cf0         TCPv6    -:0                            385b:3003:80fa:ffff:d063:a302:80fa:ffff:0 CLOSED           340      svchost.exe    
0x7df92cf0         TCPv4    -:0                            56.91.48.3:0         CLOSED           1564     BitTorrent.exe 
0x7dfe24f0         TCPv4    10.0.2.15:49221                216.58.213.163:443   ESTABLISHED      3032     chrome.exe     
0x7e113010         TCPv4    -:0                            56.123.72.3:0        CLOSED           3032     chrome.exe     
0x7e1f0940         TCPv4    10.0.2.15:49203                50.28.34.67:443      CLOSE_WAIT       1592     mirc.exe       
0x7e2d5010         TCPv4    10.0.2.15:49220                216.58.213.163:443   ESTABLISHED      3032     chrome.exe     
0x7e4a9370         UDPv4    0.0.0.0:5353                   *:*                                   2396     chrome.exe     2019-12-06 19:48:01 UTC+0000
0x7e4a9370         UDPv6    :::5353                        *:*                                   2396     chrome.exe     2019-12-06 19:48:01 UTC+0000
0x7e62c950         UDPv4    0.0.0.0:5355                   *:*                                   372      svchost.exe    2019-12-06 19:47:15 UTC+0000
0x7e62c950         UDPv6    :::5355                        *:*                                   372      svchost.exe    2019-12-06 19:47:15 UTC+0000
0x7e64eec0         UDPv4    0.0.0.0:3702                   *:*                                   340      svchost.exe    2019-12-06 19:47:17 UTC+0000
0x7e64eec0         UDPv6    :::3702                        *:*                                   340      svchost.exe    2019-12-06 19:47:17 UTC+0000
0x7e655e40         UDPv4    0.0.0.0:3702                   *:*                                   340      svchost.exe    2019-12-06 19:47:17 UTC+0000
0x7e705810         UDPv4    127.0.0.1:62228                *:*                                   1564     BitTorrent.exe 2019-12-06 19:47:12 UTC+0000
0x7e440700         TCPv6    -:0                            387b:4803:80fa:ffff:387b:4803:80fa:ffff:0 CLOSED           1564     BitTorrent.exe 
0x7ee02010         UDPv4    0.0.0.0:65081                  *:*                                   1404     svchost.exe    2019-12-06 19:47:11 UTC+0000
0x7ee02010         UDPv6    :::65081                       *:*                                   1404     svchost.exe    2019-12-06 19:47:11 UTC+0000
0x7ee967e0         UDPv6    ::1:1900                       *:*                                   1404     svchost.exe    2019-12-06 19:47:15 UTC+0000
0x7eeba730         UDPv4    0.0.0.0:1900                   *:*                                   1564     BitTorrent.exe 2019-12-06 19:47:12 UTC+0000
0x7eed1b70         UDPv4    10.0.2.15:138                  *:*                                   4        System         2019-12-06 19:47:13 UTC+0000
0x7f1fc880         UDPv4    0.0.0.0:65080                  *:*                                   1404     svchost.exe    2019-12-06 19:47:11 UTC+0000
0x7ee470f0         TCPv4    0.0.0.0:445                    0.0.0.0:0            LISTENING        4        System         
0x7ee470f0         TCPv6    :::445                         :::0                 LISTENING        4        System         
0x7fc88560         UDPv4    0.0.0.0:0                      *:*                                   656      VBoxService.ex 2019-12-06 19:54:29 UTC+0000
0x7fcb3220         UDPv4    0.0.0.0:0                      *:*                                   3424     crypt0r.exe    2019-12-06 19:53:46 UTC+0000
0x7fd10920         UDPv6    fe80::a0e4:262f:80eb:3d68:546  *:*                                   768      svchost.exe    2019-12-06 19:54:28 UTC+0000
0x7fd50550         UDPv4    0.0.0.0:0                      *:*                                   3424     crypt0r.exe    2019-12-06 19:53:46 UTC+0000
0x7fdadac0         UDPv4    0.0.0.0:0                      *:*                                   3424     crypt0r.exe    2019-12-06 19:53:46 UTC+0000
0x7fdadac0         UDPv6    :::0                           *:*                                   3424     crypt0r.exe    2019-12-06 19:53:46 UTC+0000
0x7fdafec0         UDPv4    0.0.0.0:0                      *:*                                   3424     crypt0r.exe    2019-12-06 19:53:46 UTC+0000
0x7fdafec0         UDPv6    :::0                           *:*                                   3424     crypt0r.exe    2019-12-06 19:53:46 UTC+0000
0x7fee1ce0         TCPv4    10.0.2.15:139                  0.0.0.0:0            LISTENING        4        System         
0x7fd50cf0         TCPv4    10.0.2.15:49222                216.239.34.117:443   ESTABLISHED      3032     chrome.exe     
0x7fd58cf0         TCPv4    10.0.2.15:49217                172.217.22.131:443   CLOSED           3032     chrome.exe     
```

<p></p>
Normally I go stright to system and look at the local ip address as this normally shows the system ip. we can see that in the output.
<p></p>

```
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x7e1dc820         UDPv4    10.0.2.15:137                  *:*                                   4        System         2019-12-06 19:47:13 UTC+0000                                         
```

<p></p>
Which gives us the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
10.0.2.15
</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 6</summary>
<p></p>
What is the IRC client Software used by the user ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
To get the flag for this challenge we will list all the processes running on the system and see if we can see anything, we will use pstree but any process listing option will work(again I pipe (|) to tee for later use). The command looks like this:
<p></p>

```
sudo volatility -f lab.raw --profile=Win7SP1x64 pstree | tee pstree.txt
```

<p></p>
We can see in the output:
<p></p>

```
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
. 0xfffffa80039879d0:mirc.exe                        1592   1144      9    424 2019-12-06 19:49:47 UTC+0000
```

<p></p>
If we google this:
<p></p>
"mIRC is a popular Internet Relay Chat client used by individuals and organizations to communicate, share, play and work with each other on IRC networks around the world."
<p></p>
Which proves this is the answer for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
mirc
</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 7</summary>
<p></p>
What is the user's IRC account Password ?
<br>
Hint: Clipboard contents are critical to forensic analysis. It often provides valuable forensic information, including user passwords.
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This challenge hint basically gives us the method to solve this question as there is a volatility optionaclled clipboard that prints out the clipboard history. The command and output looks like this:

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 clipboard
Volatility Foundation Volatility Framework 2.6
Session    WindowStation Format                         Handle Object             Data                                              
---------- ------------- ------------------ ------------------ ------------------ --------------------------------------------------
         1 WinSta0       CF_UNICODETEXT               0x2102fb 0xfffff900c23c6250 uFB646Vm9CscCwzR                                  
         1 WinSta0       CF_TEXT                          0x10 ------------------                                                   
         1 WinSta0       0x3306f1L              0x200000000000 ------------------                                                   
         1 WinSta0       CF_TEXT                           0x1 ------------------                                                   
         1 ------------- ------------------           0x3306f1 0xfffff900c2167350                                                   
```

<p></p>
The data column gives us the flag for this question.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
uFB646Vm9CscCwzR
</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 8</summary>
<p></p>
What is the IRC server and channel ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This one requires alot of searching using grep. A good starting place is to do a filescan and then grep for mIRC. the initial command looks like this:
<p></p>

```
sudo volatility -f lab.raw --profile=Win7SP1x64 filescan > filescan.txt
```

<p></p>
This gives us a file we can now grep multiple times. We will now grep for mIRC. The command and output looks like this:
<p></p>

```
❯ cat filescan.txt | grep -i mirc
0x00000000059fdc50     16      0 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\scripts\popups.ini
0x00000000059fdf20     16      0 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\scripts\aliases.ini
0x000000007dd51920      2      1 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC
0x000000007dea6a10     16      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\#infosec.freenode.log
0x000000007dea8840      1      1 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\bitsmasher.freenode.log
0x000000007dfa3940     16      0 R----- \Device\HarddiskVolume2\Windows\Prefetch\MIRC.EXE-6DA58AAF.pf
0x000000007e05e7e0     14      0 R--r-d \Device\HarddiskVolume2\Users\maro\Downloads\mirc.exe
0x000000007e111c20      2      0 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\#infosec.freenode.log
0x000000007e18d8d0     16      0 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\status.Freenode.log
0x000000007e1c39e0     12      0 R--r-- \Device\HarddiskVolume2\Program Files (x86)\mIRC\mirc.exe
0x000000007e277070     16      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\bitsmasher.freenode.log
0x000000007e524950      2      1 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC
0x000000007fc44f20     15      0 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\status.Freenode.log
0x000000007fc836a0      1      1 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\#infosec.freenode.log
0x000000007fcd61e0      9      0 R--r-d \Device\HarddiskVolume2\Program Files (x86)\mIRC\mirc.exe
0x000000007fcd6330     14      0 R--r-d \Device\HarddiskVolume2\Program Files (x86)\mIRC\uninstall.exe
0x000000007fcd69d0     16      0 R--r-- \Device\HarddiskVolume2\Program Files (x86)\mIRC\uninstall.exe
0x000000007fcd7730     15      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\cacert.pem
0x000000007fcd9490      2      1 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\scripts
0x000000007fd1f8a0      2      1 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs
0x000000007fd54f20      1      1 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\status.Freenode.log
0x000000007fee9470      1      1 R--rw- \Device\HarddiskVolume2\Program Files (x86)\mIRC
0x000000007fee9c50     16      0 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\urls.ini
0x000000007feef8f0      2      1 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs
```

<p></p>
The interesting thing we can see here is all the log files. Lets isolate the log files using this command:
<p></p>

```
❯ cat filescan.txt | grep -i mirc | grep -i log
0x000000007dea6a10     16      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\#infosec.freenode.log
0x000000007dea8840      1      1 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\bitsmasher.freenode.log
0x000000007e111c20      2      0 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\#infosec.freenode.log
0x000000007e18d8d0     16      0 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\status.Freenode.log
0x000000007e277070     16      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\bitsmasher.freenode.log
0x000000007fc44f20     15      0 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\status.Freenode.log
0x000000007fc836a0      1      1 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\#infosec.freenode.log
0x000000007fd1f8a0      2      1 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs
0x000000007fd54f20      1      1 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\status.Freenode.log
0x000000007feef8f0      2      1 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs
```

<p></p>
Now that we can see these log files we can dump them using the volatility option dumpfiles. The command looks like this:
<p></p>

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 dumpfiles -n -Q 0x000000007dea6a10,0x000000007dea8840,0x000000007e111c20,0x000000007e18d8d0,0x000000007e277070,0x000000007fc44f20,0x000000007fc836a0,0x000000007fd1f8a0,0x000000007fd54f20,0x000000007feef8f0 -D dump
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x7dea6a10   None   \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\#infosec.freenode.log
SharedCacheMap 0x7dea6a10   None   \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\#infosec.freenode.log
DataSectionObject 0x7dea8840   None   \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\bitsmasher.freenode.log
DataSectionObject 0x7e18d8d0   None   \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\status.Freenode.log
SharedCacheMap 0x7e18d8d0   None   \Device\HarddiskVolume2\Users\maro\AppData\Roaming\mIRC\logs\status.Freenode.log
```

<p></p>
In this command the <kbd>-n</kbd> flag tells volatility to add the name of the file to the output, the <kbd>-Q</kbd> flag points to the physical address of the file we want to dump and the <kbd>-D</kbd> flag points to the directory we want to dump the files to. We can now cat the files to see what information is contained within.
<p></p>
Looking through these, status.Freenode.log is designed to lead you down a rabbit hole as the server/channels are about authours 'Tolkein' etc. however the #infosec and bitsmasher logs look more like the information we require.
<p></p>

```
❯ cat file.None.0xfffffa8001a878f0.\#infosec.freenode.log.dat

Session Start: Fri Dec 06 04:44:41 2019
Session Ident: #infosec
03[04:44] * Now talking in #infosec
03[04:45] * bitsmasher (~bitdefeat@90.85.138.133) has joined #infosec
03[05:22] * bitsmasher (~bitdefeat@90.85.138.133) has left #infosec
Session Close: Fri Dec 06 05:23:09 2019

Session Start: Fri Dec 06 05:24:27 2019
Session Ident: #infosec
03[05:24] * Now talking in #infosec
Session Close: Fri Dec 06 05:25:15 2019

Session Start: Fri Dec 06 05:25:46 2019
Session Ident: #infosec
03[05:25] * Now talking in #infosec

Session Start: Fri Dec 06 06:42:42 2019
Session Ident: #infosec
03[06:42] * Now talking in #infosec
03[06:43] * bitsmasher (~bitdefeat@90.85.138.133) has joined #infosec
03[07:41] * drale2k (~drale2k@212-186-241-203.static.upcbusiness.at) has joined #infosec

Session Start: Fri Dec 06 11:24:10 2019
Session Ident: #infosec
03[11:24] * Now talking in #infosec
03[11:24] * bitsmasher (~bitdefeat@58.188.158.77.rev.sfr.net) has joined #infosec

Session Start: Fri Dec 06 11:51:54 2019
Session Ident: #infosec
03[11:51] * Now talking in #infosec
```

<p></p>

```
❯ cat file.None.0xfffffa8001b60260.bitsmasher.freenode.log.dat

Session Start: Fri Dec 06 04:45:49 2019
Session Ident: bitsmasher
[04:45] Session Ident: bitsmasher (freenode, codewaver) (~bitdefeat@90.85.138.133)
[04:45] <bitsmasher> Hi !
[04:46] <bitsmasher> Hello :)
Session Close: Fri Dec 06 05:23:06 2019

Session Start: Fri Dec 06 05:26:05 2019
Session Ident: bitsmasher
[05:26] Session Ident: bitsmasher (freenode, codewaver) (~bitdefeat@90.85.138.133)
[05:26] <bitsmasher> Hi !
01[05:26] <codewaver> Hi @bitsmasher 
[05:27] <bitsmasher> I saw your message in the chatroom
[05:27] <bitsmasher> Actually I have what you are looking for 
[05:28] <bitsmasher> I have a ton eBooks about CyberSecurity available for download ! I can share it with you ...
01[05:28] <codewaver> Ah That would be awesome 
01[05:28] <codewaver> Could you kindely share the download link with me ?
[05:29] <bitsmasher> sure ! here it is https://file.io/yBBkJc
01[05:29] <codewaver> Appreciated !

Session Start: Fri Dec 06 06:43:42 2019
Session Ident: bitsmasher
[06:43] <bitsmasher> :)

Session Start: Fri Dec 06 11:51:56 2019
Session Ident: bitsmasher
```

<p></p>
Here we can see connection to a channel and the conversation that occured. We can use this information to inform our next grep on the process memory dump. First we need to dump the process memory using the volatility option memdump. The command and output looks like this:
<p></p>

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 memdump -p 1592 -D .
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing mirc.exe [  1592] to 1592.dmp
```

<p></p>
In this command you can see the <kbd>-p</kbd> flag points to the Pid we found from our pstree earlier in the challenge, the <kbd>-D</kbd> flag points to the directory we want to dump the file to.
<p></p>
We can now run strings and grep for the information we found in the log files.
<p></p>
To start with I ran this (excerpt from the output):
<p></p>

```
strings -e l 1592.dmp | grep -B 5 -A 5 -i '#infosec'

--
"ircsupport
Beginner
IRCAddicts
Query: 1
Channel: 1
#infosec
^TeenParty
bitsmasher
RTeenWorld
bTeenLounge
fSpeakEasy
--
```

<p></p>
With this command strings uses the <kbd>-e l</kbd> flag to tell it to read unicode strings, the <kbd>-B</kbd> flag and <kbd>-A</kbd> flag tells grep to ooutput the 5 lines before and after #infosec and the <kbd>-i</kbd> flag tells grep to be case insensitive. This output lets us know we have found the channel. Now for the server, from here I changed my grep to different itterations of strings with and without the <kbd>-e</kbd> flag, and grep's for different itterations of infosec.freenode, .freenode etc.
<p></p>

```
strings 1592.dmp | grep -i freenode | grep "#"
```

<p></p>
Which outputs:
<p></p>

```
❯ strings 1592.dmp | grep -i freenode | grep "#"
dams.freenode.net 366 codewaver #infosec :End of /NAMES list.
:adams.freenode.net 372 codewaver :- and everyone else who made this year's freenode #live conference amazing.
:adams.freenode.net 366 codewaver #infosec :End of /NAMES list.
:adams.freenode.net 372 codewaver :- and everyone else who made this year's freenode #live conference amazing.
K&1:51] - #freenode and using the '/who freenode/staff/*' command. You may message
#infosec.freenode.lnk
#infosec.freenode.lnk
[04:44] CHANTYPES=# EXCEPTS INVEX CHANMODES=eIbq,k,flj,CFLMPQScgimnprstuz CHANLIMIT=#:120 PREFIX=(ov)@+ MAXLIST=bqeI:100 MODES=4 NETWORK=freenode STATUSMSG=@+ CALLERID=g CASEMAPPING=rfc1459 are supported by this server
[04:44] - #freenode and using the '/who freenode/staff/*' command. You may message
[11:24] - and everyone else who made this year's freenode #live conference amazing.
02[11:50] * Connect retry #1 chat.freenode.net (+6697) (dns pool)
02[11:51] * Connect retry #2 chat.freenode.net (+6697) (dns pool)
[11:51] CHANTYPES=# EXCEPTS INVEX CHANMODES=eIbq,k,flj,CFLMPQScgimnprstuz CHANLIMIT=#:120 PREFIX=(ov)@+ MAXLIST=bqeI:100 MODES=4 NETWORK=freenode STATUSMSG=@+ CALLERID=g CASEMAPPING=rfc1459 are supported by this server
[11:51] - #freenode and using the '/who freenode/staff/*' command. You may message
[11:51] - and everyone else who made this year's freenode #live conference amazing.
```

<p></p>
The output from these searches led me to thinking that the flag should look a certain way and upon trying I was correct.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
#infosec.freenode.com 1

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 9</summary>
<p></p>
What is the Nickname of the attacker ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This challenge is made easy for us from our last challenge and dumping the log files of the conversation, if we look at file.None.0xfffffa8001b60260.bitsmasher.freenode.log.dat we can see the two people in the conversation.
<p></p>

```
<bitsmasher>
<codewaver>
```

<p></p>
Entering these names gives us the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
bitsmasher

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 10</summary>
<p></p>
What is the suspicious URL visited by the victim ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
Again the log dump we did in question 8 gives us the answer.
<p></p>

```
❯ cat file.None.0xfffffa8001b60260.bitsmasher.freenode.log.dat

Session Start: Fri Dec 06 04:45:49 2019
Session Ident: bitsmasher
[04:45] Session Ident: bitsmasher (freenode, codewaver) (~bitdefeat@90.85.138.133)
[04:45] <bitsmasher> Hi !
[04:46] <bitsmasher> Hello :)
Session Close: Fri Dec 06 05:23:06 2019

Session Start: Fri Dec 06 05:26:05 2019
Session Ident: bitsmasher
[05:26] Session Ident: bitsmasher (freenode, codewaver) (~bitdefeat@90.85.138.133)
[05:26] <bitsmasher> Hi !
01[05:26] <codewaver> Hi @bitsmasher 
[05:27] <bitsmasher> I saw your message in the chatroom
[05:27] <bitsmasher> Actually I have what you are looking for 
[05:28] <bitsmasher> I have a ton eBooks about CyberSecurity available for download ! I can share it with you ...
01[05:28] <codewaver> Ah That would be awesome 
01[05:28] <codewaver> Could you kindely share the download link with me ?
[05:29] <bitsmasher> sure ! here it is https://file.io/yBBkJc
01[05:29] <codewaver> Appreciated !

Session Start: Fri Dec 06 06:43:42 2019
Session Ident: bitsmasher
[06:43] <bitsmasher> :)

Session Start: Fri Dec 06 11:51:56 2019
Session Ident: bitsmasher
```

<p></p>
Here we can see the download link the attacker gave.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
https://file.io/yBBkJc

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 11</summary>
<p></p>
What is the file that has been downloaded ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
For this challenge I started by running strings on the entire memory dump and appending it to a .txt file.
<p></p>

```
strings -a -td lab.raw > lab_strings.txt
```

<p></p>
Followed by:
<p></p>

```
strings -a -td -el lab.raw >> lab_strings.txt
```

<p></p>
You can see with these two commands that I used the -td flags to get the decimal offset and made a second pass with the -el flags in order to get (little endian) Unicode strings. Notice that the second pass appends (>>) to the existing file
<p></p>
Now that we have a strings file we can run <kbd>grep</kbd> on the file and search for the link previously given:
<p></p>

```
❯ cat lab_strings.txt| grep https://file.io/yBBkJc
597159888 https://file.io/yBBkJc
1000960500 https://file.io/yBBkJc
1000961060 https://file.io/yBBkJcor
1353483076 [05:29] <bitsmasher> sure ! here it is https://file.io/yBBkJc
1450428034 ure ! here it is https://file.io/yBBkJc
1450530858 https://file.io/yBBkJcapplication/x-bittorrentapplication/x-bittorrentp
1476252944 https://file.io/yBBkJc
1476253584 https://file.io/yBBkJc
1476253680 https://file.io/yBBkJc
1476253776 https://file.io/yBBkJc
1508595114 https://file.io/yBBkJc
1508595179 https://file.io/yBBkJc
1508595223 https://file.io/yBBkJc
1508595333 https://file.io/yBBkJc
1519366154 https://file.io/yBBkJc
1519366219 https://file.io/yBBkJc
1519366263 https://file.io/yBBkJc
1519366373 https://file.io/yBBkJc
1530919694 	8b670940-efa6-4223-a711-50a7470b6d13https://file.io/yBBkJchttps://file.io/yBBkJchttps://file.io/yBBkJchttps://file.io/yBBkJc0,1
1559127851 mhttps://file.io/yBBkJcyragee
1568852174 	8b670940-efa6-4223-a711-50a7470b6d13https://file.io/yBBkJchttps://file.io/yBBkJchttps://file.io/yBBkJchttps://file.io/yBBkJc0,1
1572020886 https://file.io/yBBkJc
1717557680 https://file.io/yBBkJc
1725194080 https://file.io/yBBkJc
1752996468 https://file.io/yBBkJc
1832815400 https://file.io/yBBkJc
1841253725 https://file.io/yBBkJc
1841253753 https://file.io/yBBkJc*
1841254309 https://file.io/yBBkJc
1841254337 https://file.io/yBBkJc*
1847009735 https://file.io/yBBkJc
1847009763 https://file.io/yBBkJc*
1851250250 https://file.io/yBBkJcapplication/x-bittorrentapplication/x-bittorrent
1995644858 https://file.io/yBBkJcapplication/x-bittorrentapplication/x-bittorrent
2008231913 9https://file.io/yBBkJc
5706888 <bitsmasher> sure ! here it is https://file.io/yBBkJc
381135264 <bitsmasher> sure ! here it is https://file.io/yBBkJc
397106088 <bitsmasher> sure ! here it is https://file.io/yBBkJc
725610818 05:29] <bitsmasher> sure ! here it is https://file.io/yBBkJc
934812504 <bitsmasher> sure ! here it is https://file.io/yBBkJc
1345086920 [05:29] <bitsmasher> sure ! here it is https://file.io/yBBkJc
1411580464 [05:29] <bitsmasher> sure ! here it is https://file.io/yBBkJc
1425714952 <bitsmasher> sure ! here it is https://file.io/yBBkJc
1442803464 [05:29] <bitsmasher> sure ! here it is https://file.io/yBBkJc
1573335680 https://file.io/yBBkJc
1573335744 https://file.io/yBBkJc
1573335808 https://file.io/yBBkJc
```

<p></p>
From this output we can see reference to bittorrent, this informs our next search where we use the filescan.txt file we made at the start of the challenges and grep for .torrent.
<p></p>

```
❯ cat filescan.txt | grep -i .torrent
0x000000007de17f20     16      0 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\dht_feed.dat.new
0x000000007de4f230     16      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\settings.dat
0x000000007deafca0      2      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\dht.dat
0x000000007deb64e0      2      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\dht_feed.dat
0x000000007df94c80     15      0 R--r-- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\updates\7.10.5_45272\bittorrentie.exe
0x000000007dfa1390      3      1 ------ \Device\NamedPipe\BitTorrent_1564_0394AC28_1532450063
0x000000007dfb08c0      3      1 ------ \Device\NamedPipe\BitTorrent_1564_0394ADF0_1564602137
0x000000007dfb5c20      3      1 ------ \Device\NamedPipe\BitTorrent_1564_0394ADF0_1564602137
0x000000007dfb9520     16      1 RW-r-- \Device\HarddiskVolume2\Users\maro\AppData\LocalLow\BitTorrent\BitTorrent_1564_0394ADF0_1564602137
0x000000007dfc8e60      2      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\rss.dat
0x000000007dfca430     16      0 R--rwd \Device\HarddiskVolume2\Users\maro\AppData\Roaming\Microsoft\Windows\Start Menu\BitTorrent.lnk
0x000000007dfce930      2      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\CyberSecurity_Books_Courses_Tutorials.exe.torrent
0x000000007e098d10     16      1 RW-r-- \Device\HarddiskVolume2\Users\maro\AppData\LocalLow\BitTorrent\BitTorrent_1564_0394AC28_1532450063
0x000000007e0d53b0     16      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\updates.dat
0x000000007e1c0f20     15      0 R--r-d \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\BitTorrent.exe
0x000000007e1c2f20     12      0 R--r-- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\BitTorrent.exe
0x000000007e1e9b40     16      0 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\settings.dat.new
0x000000007e25ff20      3      1 ------ \Device\NamedPipe\BitTorrent_1564_0394AC28_1532450063
0x000000007e261130     16      0 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\resume.dat.old
0x000000007e27ca20     10      0 R--r-d \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\updates\7.10.5_45272\bittorrentie.exe
0x000000007e43e3a0     15      0 R--r-d \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\maindoc.ico
0x000000007e63e3d0     15      0 R--rw- \Device\HarddiskVolume2\Users\maro\Desktop\BitTorrent.lnk
0x000000007fca4c40     15      0 R--rwd \Device\HarddiskVolume2\Users\maro\Downloads\BitTorrent.exe
0x000000007fced7b0     16      0 RW-rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\resume.dat.new
0x000000007fdaa4b0     14      0 R--r-- \Device\HarddiskVolume2\Windows\Prefetch\BITTORRENT.EXE-5495F912.pf
```

<p></p>
This output showes us a book however its a .exe file which is a bit strange, giving us the answer for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CyberSecurity_Books_Courses_Tutorials.exe.torrent

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 12</summary>
<p></p>
What is the torrent site ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This one took a while to figure out, to start with we grep our filescan.txt for "CyberSecurity_Books_Courses_Tutorials.exe.torrent"
<p></p>

```
❯ cat filescan.txt | grep CyberSecurity_Books_Courses_Tutorials.exe.torrent
0x000000007dfce930      2      0 R--rw- \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\CyberSecurity_Books_Courses_Tutorials.exe.torrent
```

<p></p>
Once we find the .torrent file we can use the volatility option dumpfile to dump the file.
<p></p>

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 dumpfiles -n -Q 0x000000007dfce930 -D .
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x7dfce930   None   \Device\HarddiskVolume2\Users\maro\AppData\Roaming\BitTorrent\CyberSecurity_Books_Courses_Tutorials.exe.torrent
```

<p></p>
We can now cat this file.
<p></p>

```
❯ cat file.None.0xfffffa80039ce230.CyberSecurity_Books_Courses_Tutorials.exe.torrent.dat
d10:created by33:kimbatt.github.io/torrent-creator13:creation datei1575576825e4:infod6:lengthi100685e4:name41:CyberSecurity_Books_Courses_Tutorials.exe12:piece lengthi16384e6:pieces140:��EP��VE�-����J4��Zɨ�����Q��/Ò�-E�.bܔ�W��T$t?�<�S�ڊW)]m��*��r�ŅR�R���
	������.n�a��C��7#�dZ
                            �H`R�s�q2s���e�H#^+��j��ee%                                                                                                       
```

<p></p>
From this output we can see a link to a github page, "kimbatt.github.io/torrent-creator" and if we navigate there we can see that this creates torrent files online.
<p></p>
Now that we know that we will dump the process memory of bittorrent that we saw running in pstree.
<p></p>

```
❯ cat pstree.txt | grep -i torrent
. 0xfffffa80037bf6c0:BitTorrent.exe                  1564   1144     22    494 2019-12-06 19:47:10 UTC+0000
.. 0xfffffa80039b1b30:bittorrentie.e                 2200   1564      9    129 2019-12-06 19:47:11 UTC+0000
.. 0xfffffa800347f060:bittorrentie.e                 2160   1564     14    323 2019-12-06 19:47:11 UTC+0000
```

<p></p>

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 memdump -p 1564,2200,2160 -D bittorrent
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing BitTorrent.exe [  1564] to 1564.dmp
************************************************************************
Writing bittorrentie.e [  2160] to 2160.dmp
************************************************************************
Writing bittorrentie.e [  2200] to 2200.dmp
```

<p></p>
We now have the BitTorrent process memory dumped we can use strings and grep for .torrent
<p></p>

```
strings bittorrent/* | grep .torrent 
```

<p></p>
If we look at the output at the tail end we can see:
<p></p>

```
application/x-bittorrent-appinst
application/x-bittorrent-key&
d10:created by33:bit.smasher.io/torrent-infected-k19:creation datei1575576825e4:infod6:lengthi100685e4:name41:CyberSecurity_Books_Courses_Tutorials.exe12:piece lengthi16384e6:pieces140:
J1CyberSecurity_Books_Courses_Tutorials.exe.torrentP
s/CyberSecurity_Books_Courses_Tutorials.exe.torrent
d10:created by33:kimbatt.github.io/torrent-creator13:creation datei1575576825e4:infod6:lengthi100685e4:name41:CyberSecurity_Books_Courses_Tutorials.exe12:piece lengthi16384e6:pieces140:
bittorrentie.exe
bittorrentie.exe
```

<p></p>
Here we can see the github we looked at before and above it we can see our attacker we found earlier. and the website which is the answer to this question.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
bit.smasher.io/torrent-infected-k19
</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 13</summary>
<p></p>
What is the malware process name ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
For this challenge if we take the challenge question and see "process name" we can go straight to our pstree.txt and look for any strange process names one sticks out.
<p></p>

```
❯ cat pstree.txt
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
 0xfffffa8001b9ab30:crypt0r.exe                      3424   3468     12    265 2019-12-06 19:53:45 UTC+0000
```

<p></p>
This process doesnt look right, lets investigate it. First we will dump the process using this command.
<p></p>

```
❯ sudo volatility -f lab.raw --profile=Win7SP1x64 procdump -p 3424 -D exe
[sudo] password for parrot: 
Volatility Foundation Volatility Framework 2.6
Process(V)         ImageBase          Name                 Result
------------------ ------------------ -------------------- ------
0xfffffa8001b9ab30 0x0000000001050000 crypt0r.exe          OK: executable.3424.exe
```

<p></p>
We can now run this through a program called thor-lite. The command looks like this:
<p></p>

```
~/Mytools/thor-lite/thor-lite-linux -p ~/ctf/hackfest/Memory_Forensics/exe --allreasons | tee thor.txt
```

<p></p>
The important part of the output is this:
<p></p>

```
> 3/3 > Running module 'Filesystem Checks' ------------------------------------
Info: Filescan Starting module
Info: Filescan The following paths will be scanned: /home/parrot/ctf/hackfest/Memory_Forensics/exe
Info: Filescan Scanning /home/parrot/ctf/hackfest/Memory_Forensics/exe RECURSIVE
Warning: Filescan Possibly Dangerous file found
FILE: /home/parrot/ctf/hackfest/Memory_Forensics/exe/executable.3424.exe EXT: .exe SCORE: 75
SIZE: 305664
CREATED: Tue May 11 09:13:47.011 2021 CHANGED: Tue May 11 09:13:47.051 2021 MODIFIED: Tue May 11 09:13:47.051 2021 ACCESSED: Tue May 11 09:13:47.011 2021 PERMISSIONS: -rw-r--r-- OWNER: root
MD5: 0633635245838db24ae8a34eeaa3d05d
SHA1: 1d9e2edebb4044a336ac71121d0932a5527cdb78
SHA256: d1b433eb609cdea1e16e02d50b18dcba1ad446fa0d29b6b5b00849d3f3b2bef7 TYPE: EXE FIRSTBYTES: 4d5a90000300000004000000ffff0000b8000000 / MZ
REASON_1: YARA rule MAL_RANSOM_COVID19_Apr20_1 / Detects ransomware distributed in COVID-19 theme SUBSCORE_1: 75 REF_1: https://unit42.paloaltonetworks.com/covid-19-themed-cyber-attacks-target-government-and-medical-organizations/ MATCHED_1: Str1: { 60 2e 2e 2e af 34 34 34 b8 34 34 34 b8 34 34 34 } Str2: { 1f 07 1a 37 85 05 05 36 83 05 05 36 83 05 05 34 } TAGS_1: EXE, FILE, T1136 RULEDATE_1: 2020-04-15 SIGTYPE_1: internal
Info: Filescan Finished module TOOK: 0 hours 0 mins 0 secs
```

<p></p>
Here we can see that it has detected this file as ransomware if we follow the <a href="https://unit42.paloaltonetworks.com/covid-19-themed-cyber-attacks-target-government-and-medical-organizations/" rel="nofollow">link</a> we can read up on the malware information and how it works.
<p></p>
We can use another method to see if this is malware and this is by using the website <a href="https://www.virustotal.com/gui/" rel="nofollow">VirusTotal</a> with this page we upload the .exe and we are returned this:
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/VirusTotal.jpg"><br>
</div>
<p></p>
This page tells us lots of information about the malware aswell. These results make me comfortable submitting this as the answer to this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
crypt0r.exe

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 14</summary>
<p></p>
What is the type of this malware ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
From our research from the previous question we have already found the answer to this question.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
ransomware

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 15</summary>
<p></p>
Is the malware known ? what is its name ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 16</summary>
<p></p>
What is the Bitcoin address of the attacker?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 17</summary>
<p></p>
What is the malware's control server ?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 18</summary>
<p></p>
What is the Encryption password?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Memory Forensics Lab - Question 19</summary>
<p></p>
Recover the secret
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
</details>
</details>








</details>










<p></p>
<hr>
<p></p>

<H3>Memory Forensics</H3>
<p></p>
Is a series of challenges from the <a href="https://defcon2019.ctfd.io/" rel="nofollow">Defcon DFIR CTF</a> from 2019, these challenges will use the file Triage-Memory.mem (the CTF site calles the file triage.mem)
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1osfmnlXkexk219fv83QvAs-q4I3deFFT/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
The answers for these challenges are in the form:

```
flag<ANSWER>
```

<p></p>
<details>
    <summary>Extracting the File</summary>
<p></p>
When you download it the file comes as Triage.7z you can either extract it through an archive manager or through CLI which I will explain now.
<p></p>
The first thing you will need is to install p7zip-full if you haven't already. To do this run this command:
<p></p>

```
sudo apt install p7zip-full
```

Now that's installed you can run this command on the file:

```
p7zip -d Triage.7z
```

Which will output this and extract Triage-Memory.mem:

```
❯ p7zip -d Triage.7z

7-Zip (a) [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_AU.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs Intel(R) Core(TM) i7-7920HQ CPU @ 3.10GHz (906E9),ASM,AES-NI)

Scanning the drive for archives:
1 file, 821288421 bytes (784 MiB)

Extracting archive: Triage.7z
--
Path = Triage.7z
Type = 7z
Physical Size = 821288421
Headers Size = 138
Method = LZMA2:26
Solid = -
Blocks = 1

Everything is Ok        

Size:       5368709120
Compressed: 821288421
```

<p></p>
Once we determine the image profile using <kbd>imageinfo</kbd> (refer to Getting your Foothold ) we can begin our analysis on Triage-Memory.mem
<p></p>
</details>
<p></p>
<details>
    <summary>Challenges</summary>
<p></p>
<details>
    <summary>get your volatility on</summary>
<p></p>
The first challenge we are given is:
<p></p>
What is the SHA1 hash of triage.mem?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This one is fairly easy, and should be done whenever downloading a file and the file sum is provided (normally md5sum) you can google how to calculate the sum of a file. We calculate the hash of files to ensure we are downloading the file we intended, if the sum is different to the advertised hash we can assume that the file has been altered. To calculate the sha1 hash of Triage-Memory.mem we will use a built in linux command <kbd>sha1sum</kbd>. The command looks like this and gives this output:
<p></p>

```
❯ sha1sum Triage-Memory.mem
c95e8cc8c946f95a109ea8e47a6800de10a27abd  Triage-Memory.mem
```

<p></p>
Here we can see the sha1sum of the file and our first answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<c95e8cc8c946f95a109ea8e47a6800de10a27abd>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>pr0file</summary>
<p></p>
The second challenge we are given is:
<p></p>
What profile is the most appropriate for this machine? (ex: Win10x86_14393)
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
We have done this multiple times in other challenges and I have written about it in getting a foothold. The volatility option we will use is imageinfo. the command looks like this:
<p></p>

```
sudo volatility -f Triage-Memory.mem imageinfo
```

<p></p>
Which outputs this:
<p></p>

```
❯ sudo volatility -f Triage-Memory.mem imageinfo                                                                                                                                              
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
INFO    : volatility.debug    : Determining profile based on KDBG search...                                                                                                                   
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_23418                                                            
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)                                                                                                                          
                     AS Layer2 : FileAddressSpace (/home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/Triage-Memory.mem)                                                                       
                      PAE type : No PAE                                                                                                                                                       
                           DTB : 0x187000L                                                                                                                                                    
                          KDBG : 0xf800029f80a0L                                                                                                                                              
          Number of Processors : 2                                                                                                                                                            
     Image Type (Service Pack) : 1                                                                                                                                                            
                KPCR for CPU 0 : 0xfffff800029f9d00L                                                                                                                                          
                KPCR for CPU 1 : 0xfffff880009ee000L                                                                                                                                          
             KUSER_SHARED_DATA : 0xfffff78000000000L                                                                                                                                          
           Image date and time : 2019-03-22 05:46:00 UTC+0000                                                                                                                                 
     Image local date and time : 2019-03-22 01:46:00 -0400                                                                                                                                    
```

<p></p>
As usual we will use the first suggested profile being our answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<Win7SP1x64>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>hey, write this down</summary>
<p></p>
The third challenge we are given is:
<p></p>
What was the process ID of notepad.exe?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
To find the Pid of notepad we will list all processes using the volatility option <kbd>pstree</kbd> I like to append it to a .txt file incase I need to look at the process tree later in the analysis. The command looks like this:
<p></p>

```
sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 pstree > pstree.txt
```

<p></p>
Which outputs this:
<p></p>

```
❯ sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 pstree > pstree.txt                                                                                                               
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
 0xfffffa8003de39c0:explorer.exe                     1432   1308     28    976 2019-03-22 05:32:07 UTC+0000                                                                                   
. 0xfffffa80042aa430:cmd.exe                         1408   1432      1     23 2019-03-22 05:34:12 UTC+0000                                                                                   
. 0xfffffa8005d067d0:StikyNot.exe                    1628   1432      8    183 2019-03-22 05:34:42 UTC+0000                                                                                   
. 0xfffffa80042dbb30:chrome.exe                      3248   1432     32    841 2019-03-22 05:35:14 UTC+0000                                                                                   
.. 0xfffffa8005442b30:chrome.exe                     4232   3248     14    233 2019-03-22 05:35:17 UTC+0000                                                                                   
.. 0xfffffa80047beb30:chrome.exe                     3244   3248      7     91 2019-03-22 05:35:15 UTC+0000                                                                                   
.. 0xfffffa80053306f0:chrome.exe                     1816   3248     14    328 2019-03-22 05:35:16 UTC+0000                                                                                   
.. 0xfffffa8005300b30:chrome.exe                     4156   3248     14    216 2019-03-22 05:35:17 UTC+0000                                                                                   
.. 0xfffffa8005419b30:chrome.exe                     4240   3248     14    215 2019-03-22 05:35:17 UTC+0000                                                                                   
.. 0xfffffa800540db30:chrome.exe                     4520   3248     10    234 2019-03-22 05:35:18 UTC+0000                                                                                   
.. 0xfffffa80052f0060:chrome.exe                     2100   3248      2     59 2019-03-22 05:35:15 UTC+0000                                                                                   
.. 0xfffffa80053cbb30:chrome.exe                     4688   3248     13    168 2019-03-22 05:35:19 UTC+0000                                                                                   
. 0xfffffa800474c060:OUTLOOK.EXE                     3688   1432     30   2023 2019-03-22 05:34:37 UTC+0000                                                                                   
. 0xfffffa8004798320:calc.exe                        3548   1432      3     77 2019-03-22 05:34:43 UTC+0000                                                                                   
. 0xfffffa80053d3060:POWERPNT.EXE                    4048   1432     23    765 2019-03-22 05:35:09 UTC+0000                                                                                   
. 0xfffffa8004905620:hfs.exe                         3952   1432      6    214 2019-03-22 05:34:51 UTC+0000                                                                                   
.. 0xfffffa8005a80060:wscript.exe                    5116   3952      8    312 2019-03-22 05:35:32 UTC+0000                                                                                   
... 0xfffffa8005a1d9e0:UWkpjFjDzM.exe                3496   5116      5    109 2019-03-22 05:35:33 UTC+0000                                                                                   
.... 0xfffffa8005bb0060:cmd.exe                      4660   3496      1     33 2019-03-22 05:35:36 UTC+0000                                                                                   
. 0xfffffa80054f9060:notepad.exe                     3032   1432      1     60 2019-03-22 05:32:22 UTC+0000
. 0xfffffa8005b49890:vmtoolsd.exe                    1828   1432      6    144 2019-03-22 05:32:10 UTC+0000
. 0xfffffa800474fb30:taskmgr.exe                     3792   1432      6    134 2019-03-22 05:34:38 UTC+0000
. 0xfffffa80053f83e0:EXCEL.EXE                       1272   1432     21    789 2019-03-22 05:33:49 UTC+0000
. 0xfffffa8004083880:FTK Imager.exe                  3192   1432      6    353 2019-03-22 05:35:12 UTC+0000
 0xfffffa8003c72b30:System                              4      0     87    547 2019-03-22 05:31:55 UTC+0000
. 0xfffffa8004616040:smss.exe                         252      4      2     30 2019-03-22 05:31:55 UTC+0000
 0xfffffa80050546b0:csrss.exe                         332    324     10    516 2019-03-22 05:31:58 UTC+0000
 0xfffffa8005259060:wininit.exe                       380    324      3     78 2019-03-22 05:31:58 UTC+0000
. 0xfffffa8005680910:services.exe                     476    380     12    224 2019-03-22 05:31:59 UTC+0000
.. 0xfffffa8005409060:dllhost.exe                    2072    476     13    194 2019-03-22 05:32:14 UTC+0000
.. 0xfffffa80055b0060:wmpnetwk.exe                   2628    476      9    210 2019-03-22 05:32:18 UTC+0000
.. 0xfffffa800583db30:svchost.exe                    1028    476     19    307 2019-03-22 05:32:05 UTC+0000
.. 0xfffffa8005775b30:svchost.exe                     796    476     15    368 2019-03-22 05:32:03 UTC+0000
... 0xfffffa80059e6890:dwm.exe                       1344    796      3     88 2019-03-22 05:32:07 UTC+0000
.. 0xfffffa8005508650:SearchIndexer.                 2456    476     13    766 2019-03-22 05:32:17 UTC+0000
.. 0xfffffa80057beb30:svchost.exe                     932    476     10    568 2019-03-22 05:32:03 UTC+0000
.. 0xfffffa800432f060:svchost.exe                    3300    476     13    346 2019-03-22 05:34:15 UTC+0000
.. 0xfffffa8005478060:msdtc.exe                      2188    476     12    146 2019-03-22 05:32:15 UTC+0000
.. 0xfffffa800577db30:svchost.exe                     820    476     33   1073 2019-03-22 05:32:03 UTC+0000
... 0xfffffa80059cc620:taskeng.exe                   1292    820      4     83 2019-03-22 05:32:07 UTC+0000
... 0xfffffa8004300620:taskeng.exe                   1156    820      4     93 2019-03-22 05:34:14 UTC+0000
.. 0xfffffa80059cb7c0:taskhost.exe                   1276    476      8    183 2019-03-22 05:32:07 UTC+0000
.. 0xfffffa8005b4eb30:vmtoolsd.exe                   1852    476     10    314 2019-03-22 05:32:11 UTC+0000
.. 0xfffffa800570d060:svchost.exe                     672    476      7    341 2019-03-22 05:32:02 UTC+0000
.. 0xfffffa8005a324e0:FileZilla Serv                 1476    476      9     81 2019-03-22 05:32:07 UTC+0000
.. 0xfffffa8005c4ab30:svchost.exe                    2888    476     11    152 2019-03-22 05:32:20 UTC+0000
.. 0xfffffa8005ba0620:ManagementAgen                 1932    476     10    102 2019-03-22 05:32:11 UTC+0000
.. 0xfffffa80056e1060:svchost.exe                     592    476      9    375 2019-03-22 05:32:01 UTC+0000
... 0xfffffa80054d2380:WmiPrvSE.exe                  2196    592     11    222 2019-03-22 05:32:15 UTC+0000
... 0xfffffa8005c8e440:WmiPrvSE.exe                  2436    592      9    245 2019-03-22 05:32:33 UTC+0000
... 0xfffffa80047cb060:iexplore.exe                  3576    592     12    403 2019-03-22 05:34:48 UTC+0000
.... 0xfffffa80047e9540:iexplore.exe                 2780   3576      6    233 2019-03-22 05:34:48 UTC+0000
.. 0xfffffa8005850a30:spoolsv.exe                     864    476     12    279 2019-03-22 05:32:04 UTC+0000
.. 0xfffffa80057e4560:svchost.exe                     232    476     15    410 2019-03-22 05:32:03 UTC+0000
.. 0xfffffa80058ed390:OfficeClickToR                 1136    476     23    631 2019-03-22 05:32:05 UTC+0000
.. 0xfffffa8005af24e0:VGAuthService.                 1768    476      3     89 2019-03-22 05:32:09 UTC+0000
.. 0xfffffa8004330b30:sppsvc.exe                     3260    476      4    149 2019-03-22 05:34:15 UTC+0000
.. 0xfffffa800575e5b0:svchost.exe                     764    476     20    447 2019-03-22 05:32:02 UTC+0000
. 0xfffffa80056885e0:lsass.exe                        484    380      7    650 2019-03-22 05:32:00 UTC+0000
. 0xfffffa8005696b30:lsm.exe                          492    380     10    155 2019-03-22 05:32:00 UTC+0000
 0xfffffa8005268b30:winlogon.exe                      416    364      3    110 2019-03-22 05:31:58 UTC+0000
 0xfffffa800525a9e0:csrss.exe                         372    364     11    557 2019-03-22 05:31:58 UTC+0000
. 0xfffffa80042ab620:conhost.exe                     1008    372      2     55 2019-03-22 05:34:12 UTC+0000
. 0xfffffa8005c1ab30:conhost.exe                     4656    372      2     49 2019-03-22 05:35:36 UTC+0000
 0xfffffa8005be12c0:FileZilla Serv                   1996   1860      3     99 2019-03-22 05:32:12 UTC+0000
```

<p></p>
If we look through these processes we can see notepad is running.
<p></p>

```
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
. 0xfffffa80054f9060:notepad.exe                     3032   1432      1     60 2019-03-22 05:32:22 UTC+0000
```

<p></p>
Which gives us the Pid and our answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<3032>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>wscript can haz children</summary>
<p></p>
The fourth challenge we are given is:
<p></p>
Name the child processes of wscript.exe
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
We can get this answer from the pstree.txt file we made in the previous challenge.
<p></p>

```
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
.. 0xfffffa8005a80060:wscript.exe                    5116   3952      8    312 2019-03-22 05:35:32 UTC+0000                                                                                   
... 0xfffffa8005a1d9e0:UWkpjFjDzM.exe                3496   5116      5    109 2019-03-22 05:35:33 UTC+0000                                                                                   
```

<p></p>
The bonus of using pstree over other process listing options is it showes child/parent process relationships the '.' '..' '...' show how the processes are related we can also visualise this with this command:
<p></p>

```
❯ sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 pstree --output=dot --output-file=pstree.dot
Volatility Foundation Volatility Framework 2.6
Outputting to: pstree.dot
```

<p></p>

```
dot -Tjpg pstree.dot -o pstree.jpg
```

<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/pstree_triage.jpg"><br>
</div>
<p></p>
These two methods give us our answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<UWkpjFjDzM.exe>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>tcpip settings</summary>
<p></p>
The fifth challenge we are given is:
<p></p>
What was the IP address of the machine at the time the RAM dump was created?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This challenge requires us to look at the network conditions of the system, we will use the netscan option to do this, the command looks like this (I like to append to a .txt file for later use, here you can see i pipe (|) the file to <kbd>tee</kbd> which allowes us to see the output while writting to a file at the same time):
<p></p>

```
sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 netscan | tee netscan.txt
```

<p></p>
Which outputs this:
<p></p>

```
❯ sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 netscan | tee netscan.txt                                                                                                         
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x13e057300        UDPv4    10.0.0.101:55736               *:*                                   2888     svchost.exe    2019-03-22 05:32:20 UTC+0000                                         
0x13e05b4f0        UDPv6    ::1:55735                      *:*                                   2888     svchost.exe    2019-03-22 05:32:20 UTC+0000                                         
0x13e05b790        UDPv6    fe80::7475:ef30:be18:7807:55734 *:*                                   2888     svchost.exe    2019-03-22 05:32:20 UTC+0000                                        
0x13e05d4b0        UDPv6    fe80::7475:ef30:be18:7807:1900 *:*                                   2888     svchost.exe    2019-03-22 05:32:20 UTC+0000                                         
0x13e05dec0        UDPv4    127.0.0.1:55737                *:*                                   2888     svchost.exe    2019-03-22 05:32:20 UTC+0000                                         
0x13e05e3f0        UDPv4    10.0.0.101:1900                *:*                                   2888     svchost.exe    2019-03-22 05:32:20 UTC+0000                                         
0x13e05eab0        UDPv6    ::1:1900                       *:*                                   2888     svchost.exe    2019-03-22 05:32:20 UTC+0000                                         
0x13e064d70        UDPv4    127.0.0.1:1900                 *:*                                   2888     svchost.exe    2019-03-22 05:32:20 UTC+0000                                         
0x13e02bcf0        TCPv4    -:49220                        72.51.60.132:443     CLOSED           4048     POWERPNT.EXE                                                                        
0x13e035790        TCPv4    -:49223                        72.51.60.132:443     CLOSED           4048     POWERPNT.EXE                                                                        
0x13e036470        TCPv4    -:49224                        72.51.60.132:443     CLOSED           4048     POWERPNT.EXE                                                                        
0x13e258010        UDPv4    127.0.0.1:55560                *:*                                   5116     wscript.exe    2019-03-22 05:35:32 UTC+0000                                         
0x13e305a50        UDPv4    0.0.0.0:5355                   *:*                                   232      svchost.exe    2019-03-22 05:32:09 UTC+0000                                         
0x13e360be0        UDPv4    0.0.0.0:63790                  *:*                                   504                     2019-03-22 05:45:47 UTC+0000                                         
0x13e490ec0        UDPv4    0.0.0.0:5355                   *:*                                   232      svchost.exe    2019-03-22 05:32:09 UTC+0000                                         
0x13e490ec0        UDPv6    :::5355                        *:*                                   232      svchost.exe    2019-03-22 05:32:09 UTC+0000                                         
0x13e5683e0        UDPv4    10.0.0.101:137                 *:*                                   4        System         2019-03-22 05:32:06 UTC+0000                                         
0x13e594250        UDPv4    10.0.0.101:138                 *:*                                   4        System         2019-03-22 05:32:06 UTC+0000                                         
0x13e597ec0        UDPv4    0.0.0.0:0                      *:*                                   232      svchost.exe    2019-03-22 05:32:06 UTC+0000                                         
0x13e597ec0        UDPv6    :::0                           *:*                                   232      svchost.exe    2019-03-22 05:32:06 UTC+0000                                         
0x13e61fb30        UDPv6    fe80::7475:ef30:be18:7807:546  *:*                                   764      svchost.exe    2019-03-22 05:46:23 UTC+0000                                         
0x13e918010        UDPv4    0.0.0.0:56372                  *:*                                   1816     chrome.exe     2019-03-22 05:45:51 UTC+0000                                         
0x13e9cd730        UDPv4    127.0.0.1:57374                *:*                                   1136     OfficeClickToR 2019-03-22 05:32:18 UTC+0000                                         
0x13ea8e6a0        UDPv4    127.0.0.1:61704                *:*                                   3688     OUTLOOK.EXE    2019-03-22 05:34:44 UTC+0000                                         
0x13ead0bf0        UDPv4    127.0.0.1:55614                *:*                                   4048     POWERPNT.EXE   2019-03-22 05:35:15 UTC+0000                                         
0x13ebc6c20        UDPv4    0.0.0.0:5353                   *:*                                   3248     chrome.exe     2019-03-22 05:35:17 UTC+0000                                         
0x13ebea890        UDPv4    0.0.0.0:5353                   *:*                                   3248     chrome.exe     2019-03-22 05:35:17 UTC+0000                                         
0x13ebea890        UDPv6    :::5353                        *:*                                   3248     chrome.exe     2019-03-22 05:35:17 UTC+0000                                         
0x13e2c6b10        TCPv4    0.0.0.0:21                     0.0.0.0:0            LISTENING        1476     FileZilla Serv                                                                      
0x13e2c6b10        TCPv6    :::21                          :::0                 LISTENING        1476     FileZilla Serv                                                                      
0x13e2c7850        TCPv6    ::1:14147                      :::0                 LISTENING        1476     FileZilla Serv                                                                      
0x13e2c96b0        TCPv4    127.0.0.1:14147                0.0.0.0:0            LISTENING        1476     FileZilla Serv                                                                      
0x13e2c9be0        TCPv4    0.0.0.0:21                     0.0.0.0:0            LISTENING        1476     FileZilla Serv                                                                      
0x13e3a1150        TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        484      lsass.exe                                                                           
0x13e3a1150        TCPv6    :::49155                       :::0                 LISTENING        484      lsass.exe                                                                           
0x13e3b2010        TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        484      lsass.exe                                                                           
0x13e430580        TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        820      svchost.exe                                                                         
0x13e430580        TCPv6    :::49154                       :::0                 LISTENING        820      svchost.exe                                                                         
0x13e431820        TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        820      svchost.exe    
0x13e57e010        TCPv4    10.0.0.101:139                 0.0.0.0:0            LISTENING        4        System         
0x13e71cef0        TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        672      svchost.exe    
0x13e720660        TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        672      svchost.exe    
0x13e720660        TCPv6    :::135                         :::0                 LISTENING        672      svchost.exe    
0x13e72f010        TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        380      wininit.exe    
0x13e72f6e0        TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        380      wininit.exe    
0x13e72f6e0        TCPv6    :::49152                       :::0                 LISTENING        380      wininit.exe    
0x13e770240        TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        764      svchost.exe    
0x13e772980        TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        764      svchost.exe    
0x13e772980        TCPv6    :::49153                       :::0                 LISTENING        764      svchost.exe    
0x13ebb3010        TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        476      services.exe   
0x13ebb3010        TCPv6    :::49156                       :::0                 LISTENING        476      services.exe   
0x13ebcdef0        TCPv4    0.0.0.0:80                     0.0.0.0:0            LISTENING        3952     hfs.exe        
0x13e2348a0        TCPv4    -:49366                        192.168.206.181:389  CLOSED           504                     
0x13e397190        TCPv4    10.0.0.101:49217               10.0.0.106:4444      ESTABLISHED      3496     UWkpjFjDzM.exe 
0x13e3986d0        TCPv4    -:49378                        213.209.1.129:25     CLOSED           504                     
0x13e3abae0        TCPv4    -:49226                        72.51.60.132:443     CLOSED           4048     POWERPNT.EXE   
0x13e3e7010        TCPv6    -:0                            38db:7705:80fa:ffff:38db:7705:80fa:ffff:0 CLOSED           1136     OfficeClickToR 
0x13e441830        TCPv6    -:0                            382b:c703:80fa:ffff:382b:c703:80fa:ffff:0 CLOSED           1        ?RK????       
0x13e4e4910        TCPv4    10.0.0.101:49208               52.109.12.6:443      CLOSED           504                     
0x13e55fae0        TCPv4    10.0.0.101:49209               52.96.44.162:443     CLOSED           504                     
0x13e71b540        TCPv4    -:0                            104.208.112.5:0      CLOSED           1        ?RK????       
0x13e73b560        TCPv4    -:49266                        35.190.69.156:443    CLOSED           504                     
0x13e7c6010        TCPv4    10.0.0.101:49204               172.217.6.195:443    CLOSED           1816     chrome.exe     
0x13ead7cf0        TCPv4    10.0.0.101:49202               172.217.10.68:443    CLOSED           1816     chrome.exe     
0x13f5898a0        TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        476      services.exe   
0x13f5899c0        TCPv4    0.0.0.0:445                    0.0.0.0:0            LISTENING        4        System         
0x13f5899c0        TCPv6    :::445                         :::0                 LISTENING        4        System         
0x13f4facf0        TCPv4    10.0.0.101:49262               52.109.12.6:443      ESTABLISHED      3688     OUTLOOK.EXE    
0x13f50a010        TCPv4    -:49265                        213.186.33.3:443     CLOSED           504                     
0x13f5289f0        TCPv4    -:49234                        72.51.60.133:80      CLOSED           3688     OUTLOOK.EXE    
0x13f7b4ec0        UDPv4    0.0.0.0:55707                  *:*                                   232      svchost.exe    2019-03-22 05:45:44 UTC+0000
0x13f7e8670        UDPv4    127.0.0.1:59411                *:*                                   3576     iexplore.exe   2019-03-22 05:34:49 UTC+0000
0x13fc6f1b0        UDPv4    0.0.0.0:55102                  *:*                                   232      svchost.exe    2019-03-22 05:45:36 UTC+0000
0x13fc78dc0        UDPv4    127.0.0.1:53361                *:*                                   1272     EXCEL.EXE      2019-03-22 05:34:03 UTC+0000
0x13f7ae010        TCPv4    10.0.0.101:49263               52.96.44.162:443     ESTABLISHED      3688     OUTLOOK.EXE    
0x13fa93cf0        TCPv4    -:49173                        72.51.60.132:443     CLOSED           1272     EXCEL.EXE      
0x13fa95cf0        TCPv4    -:49170                        72.51.60.132:443     CLOSED           1272     EXCEL.EXE      
0x13fa969f0        TCPv4    -:0                            56.219.119.5:0       CLOSED           1272     EXCEL.EXE      
0x13fbd07e0        TCPv4    -:49372                        212.227.15.9:25      CLOSED           504                     
0x13fc857e0        TCPv4    -:49167                        72.51.60.132:443     CLOSED           1272     EXCEL.EXE      
```

<p></p>
If we look in the local address column and go down to the system process we can see a local ip and the answer for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x13e5683e0        UDPv4    10.0.0.101:137                 *:*                                   4        System         2019-03-22 05:32:06 UTC+0000                                         
```

<p></p>

```
flag<10.0.0.101>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>intel</summary>
<p></p>
The sixth challenge we are given is:
<p></p>
Based on the answer regarding to the infected PID, can you determine what the IP of the attacker was?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
For this challenge we can have a look at the netscan.txt file we created and look for the file we found in 'wscript can haz children' (UWkpjFjDzM.exe) if we go through the network connections we look for that file and look in the foreign address column for our answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x13e397190        TCPv4    10.0.0.101:49217               10.0.0.106:4444      ESTABLISHED      3496     UWkpjFjDzM.exe 
```

<p></p>

```
flag<10.0.0.106>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>i <3 windows dependencies</summary>
<p></p>
The seventh challenge we are given is:
<p></p>
What process name is VCRUNTIME140.dll associated with?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
First what is a .dll?
<p></p>
<details>
    <summary>What is a .dll?</summary>
<p></p>
Dynamic-link library (DLL) is Microsoft's implementation of the shared library concept in the Microsoft Windows and OS/2 operating systems. These libraries usually have the file extension DLL, OCX (for libraries containing ActiveX controls), or DRV (for legacy system drivers). The file formats for DLLs are the same as for Windows EXE files – that is, Portable Executable (PE) for 32-bit and 64-bit Windows, and New Executable (NE) for 16-bit Windows. As with EXEs, DLLs can contain code, data, and resources, in any combination.
<p></p>
Data files with the same file format as a DLL, but with different file extensions and possibly containing only resource sections, can be called resource DLLs. Examples of such DLLs include icon libraries, sometimes having the extension ICL, and font files, having the extensions FON and FOT.
<p></p>
A DLL file is a library that contains a set of code and data for carrying out a particular activity in Windows. Apps can then call on those DLL files when they need that activity performed. DLL files are a lot like executable (EXE) files, except that DLL files cannot be directly executed in Windows. In other words, you can’t double-click a DLL file to run it the same way you would an EXE file. Instead, DLL files are designed to be called upon by other apps. In fact, they are designed to be called upon by multiple apps at once. The “link” part of the DLL name also suggests another important aspect. Multiple DLLs can be linked together so that when one DLL is called, a number of other DLLs are also called at the same time.
</details>
<p></p>
So now that we know what a .dll is we can use the dlllist option in volatility to list all the .dll's and their associated processes.

Option | Description
-------|-------------------
dlllist | To display a process's loaded DLLs, use the dlllist command. It walks the doubly-linked list of _LDR_DATA_TABLE_ENTRY structures which is pointed to by the PEB's InLoadOrderModuleList. DLLs are automatically added to this list when a process calls LoadLibrary (or some derivative such as LdrLoadDll) and they aren't removed until FreeLibrary is called and the reference count reaches zero. The load count column tells you if a DLL was statically loaded (i.e. as a result of being in the exe or another DLL's import table) or dynamically loaded. <p></p> To display the DLLs for a specific process instead of all processes, use the <kbd>-p</kbd>

<p></p>
The command we will use is bellow (note that I pipe (|) to tee and output to dlllist.txt for later use)

```
sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 dlllist | tee dlllist.txt
```

<p></p>
We can see from the tail end of the output that the process is followed by all the linked .dll's We can now <kbd>grep</kbd> for VCRUNTIME140.dll
<p></p>

```
❯ cat dlllist.txt | grep VCRUNTIME140.dll
0x000007fefa5c0000            0x16000             0xffff C:\Program Files\Common Files\Microsoft Shared\ClickToRun\VCRUNTIME140.dll
```

<p></p>
We can see we got a hit! Thinking back to the output we know that the .dll is located below the process we can see this by adding the <kbd>-B</kbd> flag and specifying lines before that we want to see.
<p></p>

```
❯ cat dlllist.txt | grep -B 35 VCRUNTIME140.dll
0x000007fef5300000             0xd000                0x1 C:\Windows\system32\wdiasqmmodule.dll
0x000007fefcb90000            0x22000                0x1 C:\Windows\system32\bcrypt.dll
************************************************************************
OfficeClickToR pid:   1136
Command line : "C:\Program Files\Common Files\Microsoft Shared\ClickToRun\OfficeClickToRun.exe" /service
Service Pack 1

Base                             Size          LoadCount Path
------------------ ------------------ ------------------ ----
0x000000013f420000           0xa9d000             0xffff C:\Program Files\Common Files\Microsoft Shared\ClickToRun\OfficeClickToRun.exe
0x0000000077260000           0x1a9000             0xffff C:\Windows\SYSTEM32\ntdll.dll
0x0000000077040000           0x11f000             0xffff C:\Windows\system32\kernel32.dll
0x000007fefd380000            0x6c000             0xffff C:\Windows\system32\KERNELBASE.dll
0x000007fefe970000            0xdb000             0xffff C:\Windows\system32\ADVAPI32.dll
0x000007fefd6b0000            0x9f000             0xffff C:\Windows\system32\msvcrt.dll
0x000007feff160000            0x1f000             0xffff C:\Windows\SYSTEM32\sechost.dll
0x000007fefe7a0000           0x12d000             0xffff C:\Windows\system32\RPCRT4.dll
0x000007fefef60000            0x67000             0xffff C:\Windows\system32\GDI32.dll
0x0000000077160000            0xfa000             0xffff C:\Windows\system32\USER32.dll
0x000007feff150000             0xe000             0xffff C:\Windows\system32\LPK.dll
0x000007fefe5e0000            0xc9000             0xffff C:\Windows\system32\USP10.dll
0x000007fefb140000            0x27000             0xffff C:\Windows\system32\IPHLPAPI.DLL
0x000007fefe790000             0x8000             0xffff C:\Windows\system32\NSI.dll
0x000007fefb100000             0xb000             0xffff C:\Windows\system32\WINNSI.DLL
0x000007feff180000           0x203000             0xffff C:\Windows\system32\ole32.dll
0x000007fefe6b0000            0xd7000             0xffff C:\Windows\system32\OLEAUT32.dll
0x000007feff3f0000            0x4d000             0xffff C:\Windows\system32\WS2_32.dll
0x000007fefa5e0000            0x1b000             0xffff C:\Windows\system32\Cabinet.dll
0x000007fefd2a0000            0x3b000             0xffff C:\Windows\system32\WINTRUST.dll
0x000007fefd3f0000           0x16d000             0xffff C:\Windows\system32\CRYPT32.dll
0x000007fefd250000             0xf000             0xffff C:\Windows\system32\MSASN1.dll
0x000007fefb2d0000            0x11000             0xffff C:\Windows\system32\WTSAPI32.dll
0x000007fefed80000           0x1d7000             0xffff C:\Windows\system32\SETUPAPI.dll
0x000007fefd260000            0x36000             0xffff C:\Windows\system32\CFGMGR32.dll
0x000007fefd560000            0x1a000             0xffff C:\Windows\system32\DEVOBJ.dll
0x000007fefa5c0000            0x16000             0xffff C:\Program Files\Common Files\Microsoft Shared\ClickToRun\VCRUNTIME140.dll
```

<p></p>
Here we can see the process and the answer to the challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
Now if we put in OfficeClickToRun.exe it will not accept, however if we go back to the netscan we can see what the process is listed as:
<p></p>

```
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x13e9cd730        UDPv4    127.0.0.1:57374                *:*                                   1136     OfficeClickToR 2019-03-22 05:32:18 UTC+0000                                         
```

<p></p>
Which gives us the flag.
<p></p>

```
flag<OfficeClickToR>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>mal-ware-are-you</summary>
<p></p>
The eighth challenge we are given is:
<p></p>
What is the md5 hash value the potential malware on the system?
<p></p>
<H3>Warning live malware</H3>
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
First we will need to extract the malware IOT run <kbd>md5sum</kbd> on the file so we will cat our pslist.txt and look for the Pid associated with the malware we found earlier.
<p></p>

```
❯ sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 pstree > pstree.txt                                                                                                               
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
 0xfffffa8003de39c0:explorer.exe                     1432   1308     28    976 2019-03-22 05:32:07 UTC+0000                                                                                   
. 0xfffffa80042aa430:cmd.exe                         1408   1432      1     23 2019-03-22 05:34:12 UTC+0000                                                                                   
. 0xfffffa8005d067d0:StikyNot.exe                    1628   1432      8    183 2019-03-22 05:34:42 UTC+0000                                                                                   
. 0xfffffa80042dbb30:chrome.exe                      3248   1432     32    841 2019-03-22 05:35:14 UTC+0000                                                                                   
.. 0xfffffa8005442b30:chrome.exe                     4232   3248     14    233 2019-03-22 05:35:17 UTC+0000                                                                                   
.. 0xfffffa80047beb30:chrome.exe                     3244   3248      7     91 2019-03-22 05:35:15 UTC+0000                                                                                   
.. 0xfffffa80053306f0:chrome.exe                     1816   3248     14    328 2019-03-22 05:35:16 UTC+0000                                                                                   
.. 0xfffffa8005300b30:chrome.exe                     4156   3248     14    216 2019-03-22 05:35:17 UTC+0000                                                                                   
.. 0xfffffa8005419b30:chrome.exe                     4240   3248     14    215 2019-03-22 05:35:17 UTC+0000                                                                                   
.. 0xfffffa800540db30:chrome.exe                     4520   3248     10    234 2019-03-22 05:35:18 UTC+0000                                                                                   
.. 0xfffffa80052f0060:chrome.exe                     2100   3248      2     59 2019-03-22 05:35:15 UTC+0000                                                                                   
.. 0xfffffa80053cbb30:chrome.exe                     4688   3248     13    168 2019-03-22 05:35:19 UTC+0000                                                                                   
. 0xfffffa800474c060:OUTLOOK.EXE                     3688   1432     30   2023 2019-03-22 05:34:37 UTC+0000                                                                                   
. 0xfffffa8004798320:calc.exe                        3548   1432      3     77 2019-03-22 05:34:43 UTC+0000                                                                                   
. 0xfffffa80053d3060:POWERPNT.EXE                    4048   1432     23    765 2019-03-22 05:35:09 UTC+0000                                                                                   
. 0xfffffa8004905620:hfs.exe                         3952   1432      6    214 2019-03-22 05:34:51 UTC+0000                                                                                   
.. 0xfffffa8005a80060:wscript.exe                    5116   3952      8    312 2019-03-22 05:35:32 UTC+0000                                                                                   
... 0xfffffa8005a1d9e0:UWkpjFjDzM.exe                3496   5116      5    109 2019-03-22 05:35:33 UTC+0000                                                                                   
.... 0xfffffa8005bb0060:cmd.exe                      4660   3496      1     33 2019-03-22 05:35:36 UTC+0000                                                                                   
. 0xfffffa80054f9060:notepad.exe                     3032   1432      1     60 2019-03-22 05:32:22 UTC+0000
. 0xfffffa8005b49890:vmtoolsd.exe                    1828   1432      6    144 2019-03-22 05:32:10 UTC+0000
. 0xfffffa800474fb30:taskmgr.exe                     3792   1432      6    134 2019-03-22 05:34:38 UTC+0000
. 0xfffffa80053f83e0:EXCEL.EXE                       1272   1432     21    789 2019-03-22 05:33:49 UTC+0000
. 0xfffffa8004083880:FTK Imager.exe                  3192   1432      6    353 2019-03-22 05:35:12 UTC+0000
 0xfffffa8003c72b30:System                              4      0     87    547 2019-03-22 05:31:55 UTC+0000
. 0xfffffa8004616040:smss.exe                         252      4      2     30 2019-03-22 05:31:55 UTC+0000
 0xfffffa80050546b0:csrss.exe                         332    324     10    516 2019-03-22 05:31:58 UTC+0000
 0xfffffa8005259060:wininit.exe                       380    324      3     78 2019-03-22 05:31:58 UTC+0000
. 0xfffffa8005680910:services.exe                     476    380     12    224 2019-03-22 05:31:59 UTC+0000
.. 0xfffffa8005409060:dllhost.exe                    2072    476     13    194 2019-03-22 05:32:14 UTC+0000
.. 0xfffffa80055b0060:wmpnetwk.exe                   2628    476      9    210 2019-03-22 05:32:18 UTC+0000
.. 0xfffffa800583db30:svchost.exe                    1028    476     19    307 2019-03-22 05:32:05 UTC+0000
.. 0xfffffa8005775b30:svchost.exe                     796    476     15    368 2019-03-22 05:32:03 UTC+0000
... 0xfffffa80059e6890:dwm.exe                       1344    796      3     88 2019-03-22 05:32:07 UTC+0000
.. 0xfffffa8005508650:SearchIndexer.                 2456    476     13    766 2019-03-22 05:32:17 UTC+0000
.. 0xfffffa80057beb30:svchost.exe                     932    476     10    568 2019-03-22 05:32:03 UTC+0000
.. 0xfffffa800432f060:svchost.exe                    3300    476     13    346 2019-03-22 05:34:15 UTC+0000
.. 0xfffffa8005478060:msdtc.exe                      2188    476     12    146 2019-03-22 05:32:15 UTC+0000
.. 0xfffffa800577db30:svchost.exe                     820    476     33   1073 2019-03-22 05:32:03 UTC+0000
... 0xfffffa80059cc620:taskeng.exe                   1292    820      4     83 2019-03-22 05:32:07 UTC+0000
... 0xfffffa8004300620:taskeng.exe                   1156    820      4     93 2019-03-22 05:34:14 UTC+0000
.. 0xfffffa80059cb7c0:taskhost.exe                   1276    476      8    183 2019-03-22 05:32:07 UTC+0000
.. 0xfffffa8005b4eb30:vmtoolsd.exe                   1852    476     10    314 2019-03-22 05:32:11 UTC+0000
.. 0xfffffa800570d060:svchost.exe                     672    476      7    341 2019-03-22 05:32:02 UTC+0000
.. 0xfffffa8005a324e0:FileZilla Serv                 1476    476      9     81 2019-03-22 05:32:07 UTC+0000
.. 0xfffffa8005c4ab30:svchost.exe                    2888    476     11    152 2019-03-22 05:32:20 UTC+0000
.. 0xfffffa8005ba0620:ManagementAgen                 1932    476     10    102 2019-03-22 05:32:11 UTC+0000
.. 0xfffffa80056e1060:svchost.exe                     592    476      9    375 2019-03-22 05:32:01 UTC+0000
... 0xfffffa80054d2380:WmiPrvSE.exe                  2196    592     11    222 2019-03-22 05:32:15 UTC+0000
... 0xfffffa8005c8e440:WmiPrvSE.exe                  2436    592      9    245 2019-03-22 05:32:33 UTC+0000
... 0xfffffa80047cb060:iexplore.exe                  3576    592     12    403 2019-03-22 05:34:48 UTC+0000
.... 0xfffffa80047e9540:iexplore.exe                 2780   3576      6    233 2019-03-22 05:34:48 UTC+0000
.. 0xfffffa8005850a30:spoolsv.exe                     864    476     12    279 2019-03-22 05:32:04 UTC+0000
.. 0xfffffa80057e4560:svchost.exe                     232    476     15    410 2019-03-22 05:32:03 UTC+0000
.. 0xfffffa80058ed390:OfficeClickToR                 1136    476     23    631 2019-03-22 05:32:05 UTC+0000
.. 0xfffffa8005af24e0:VGAuthService.                 1768    476      3     89 2019-03-22 05:32:09 UTC+0000
.. 0xfffffa8004330b30:sppsvc.exe                     3260    476      4    149 2019-03-22 05:34:15 UTC+0000
.. 0xfffffa800575e5b0:svchost.exe                     764    476     20    447 2019-03-22 05:32:02 UTC+0000
. 0xfffffa80056885e0:lsass.exe                        484    380      7    650 2019-03-22 05:32:00 UTC+0000
. 0xfffffa8005696b30:lsm.exe                          492    380     10    155 2019-03-22 05:32:00 UTC+0000
 0xfffffa8005268b30:winlogon.exe                      416    364      3    110 2019-03-22 05:31:58 UTC+0000
 0xfffffa800525a9e0:csrss.exe                         372    364     11    557 2019-03-22 05:31:58 UTC+0000
. 0xfffffa80042ab620:conhost.exe                     1008    372      2     55 2019-03-22 05:34:12 UTC+0000
. 0xfffffa8005c1ab30:conhost.exe                     4656    372      2     49 2019-03-22 05:35:36 UTC+0000
 0xfffffa8005be12c0:FileZilla Serv                   1996   1860      3     99 2019-03-22 05:32:12 UTC+0000
 ```

<p></p>
We can see the .exe that we identified before.
<p></p>

```
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
... 0xfffffa8005a1d9e0:UWkpjFjDzM.exe                3496   5116      5    109 2019-03-22 05:35:33 UTC+0000                                                                                   
```

<p></p>
From here we need to take the Pid and input it into the procdump option. The command looks like this:
<p></p>

```
❯ sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 procdump -p 3496 -D .
Volatility Foundation Volatility Framework 2.6
Process(V)         ImageBase          Name                 Result
------------------ ------------------ -------------------- ------
0xfffffa8005a1d9e0 0x0000000000400000 UWkpjFjDzM.exe       OK: executable.3496.exe
```

<p></p>
In this command the <kbd>-p</kbd> flag points to the Pid of the file and the <kbd>-D</kbd> flag points to the directory we want to dump to '.' being the local working directory.
<p></p>
We can now run <kbd>md5sum</kbd> on the file IOT get the hash. The command looks like this:
<p></p>

```
❯ md5sum executable.3496.exe
690ea20bc3bdfb328e23005d9a80c290  executable.3496.exe
```

<p></p>
Here we can see the md5sum of the .exe file and the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<690ea20bc3bdfb328e23005d9a80c290>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>lm-get bobs hash</summary>
<p></p>
The ninth challenge we are given is:
<p></p>
What is the LM hash of bobs account?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
First what is a LM Hash?
<p></p>
<details>
    <summary>What is a LM Hash?</summary>
<p></p>
LM Hash is used in many versions of Windows to store user passwords that are fewer than 15 characters long. It is a fairly weak security implementation can be easily broken using standard dictionary lookups. More modern versions of Windows use SYSKEY to encrypt passwords.
</details>
<p></p>
Ok now that we know what a LM hash is we can use the hashdump option to show us the hashes in the memory. The command looks like this (we pipe (|) to tee so we have the hashes for later):
<p></p>

```
sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 hashdump | tee hash.hash
Volatility Foundation Volatility Framework 2.6
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Bob:1000:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```

<p></p>
We can see bobs hash and the answer to this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<aad3b435b51404eeaad3b435b51404ee>
```


</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>vad the impaler</summary>
<p></p>
The tenth challenge we are given is:
<p></p>
What protections does the VAD node at 0xfffffa800577ba10 have?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
First what is a VAD?
<p></p>
<details>
    <summary>What is a VAD?</summary>
<p></p>
The Virtual Address Descriptor tree is used by the Windows memory manager to describe memory ranges used by a process as they are allocated. When a process allocates memory with VirutalAlloc, the memory manager creates an entry in the VAD tree. The corresponding page directory and page table entries are not created until the process tries to reference that memory page, which can provide signiﬁcant memory savings for processes that allocate a large amount of memory but access it sparsely.
</details>
<p></p>
So now we know what a VAD is we can use the vadinfo option in volatility.
<p></p>

Option | Description
-------|------------------
vadinfo | The vadinfo command displays extended information about a process's VAD nodes. In particular, it shows:<p></p>- The address of the MMVAD structure in kernel memory<br>- The starting and ending virtual addresses in process memory that the MMVAD structure pertains to<br>- The VAD Tag<br>- The VAD flags, control flags, etc<br>- The name of the memory mapped file (if one exists)<br>- The memory protection constant (permissions). Note there is a difference between the original protection and current protection. The original protection is derived from the flProtect parameter to VirtualAlloc. For example you can reserve memory (MEM_RESERVE) with protection PAGE_NOACCESS (original protection). Later, you can call VirtualAlloc again to commit (MEM_COMMIT) and specify PAGE_READWRITE (becomes current protection). The vadinfo command shows the original protection only. Thus, just because you see PAGE_NOACCESS here, it doesn't mean code in the region cannot be read, written, or executed.

<p></p>
The command looks like this (you can see I have appended to a .txt file again for later use and so I can run grep multiple times):
<p></p>

```
sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 vadinfo > vadinfo.txt
```

<p></p>
From here we can grep for the info in the hint, the command looks like this:
<p></p>

```
❯ cat vadinfo.txt | grep 0xfffffa800577ba10
VAD node @ 0xfffffa800577ba10 Start 0x0000000000030000 End 0x0000000000033fff Tag Vad 
```

<p></p>
So we can see we have found it but we need to see information above and below it so we will add the <kbd>-B</kbd> and <kbd>-A</kbd> flags.
<p></p>

```
❯ cat vadinfo.txt | grep -B 5 -A 5 0xfffffa800577ba10
NumberOfMappedViews:                2 NumberOfUserReferences:          3
Control Flags: Commit: 1
First prototype PTE: fffff8a001021f78 Last contiguous PTE: fffff8a001021ff0
Flags2: 

VAD node @ 0xfffffa800577ba10 Start 0x0000000000030000 End 0x0000000000033fff Tag Vad 
Flags: NoChange: 1, Protection: 1
Protection: PAGE_READONLY
Vad Type: VadNone
ControlArea @fffffa8005687a50 Segment fffff8a000c4f870
NumberOfSectionReferences:          1 NumberOfPfnReferences:           0
```

<p></p>
Here we can see the protection and the flag to this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<PAGE_READONLY>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>more vads?!</summary>
<p></p>
The eleventh challenge we are given is:
<p></p>
What protections did the VAD starting at 0x00000000033c0000 and ending at 0x00000000033dffff have?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This is the same as the last challenge we just need to change the grep. The command and output look like this:
<p></p>

```
❯ cat vadinfo.txt | grep -B 5 -A 5 -i "start 0x00000000033c0000 end 0x00000000033dffff"
VAD node @ 0xfffffa80058d9e00 Start 0x0000000003280000 End 0x000000000337ffff Tag VadS
Flags: CommitCharge: 4, PrivateMemory: 1, Protection: 4
Protection: PAGE_READWRITE
Vad Type: VadNone

VAD node @ 0xfffffa80052652b0 Start 0x00000000033c0000 End 0x00000000033dffff Tag VadS
Flags: CommitCharge: 32, PrivateMemory: 1, Protection: 24
Protection: PAGE_NOACCESS
Vad Type: VadNone

VAD node @ 0xfffffa8003f416d0 Start 0x00000000033a0000 End 0x00000000033bffff Tag VadS
```

<p></p>
We got the "start 0x00000000033c0000 end 0x00000000033dffff" from the last challenge when we searched the vadinfo.
<br>
The output gives us the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<PAGE_NOACCESS>
```


</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>vacation bible school</summary>
<p></p>
The twelvth challenge we are given is:
<p></p>
There was a VBS script run on the machine. What is the name of the script? (submit without file extension)
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
First what is a VBS script?
<p></p>
<details>
    <summary>What is a VBS Script?</summary>
<p></p>
VBScript ("Microsoft Visual Basic Scripting Edition") is an Active Scripting language developed by Microsoft that is modeled on Visual Basic. It allows Microsoft Windows system administrators to generate powerful tools for managing computers with error handling, subroutines, and other advanced programming constructs. It can give the user complete control over many aspects of their computing environment.
<p></p>
VBScript uses the Component Object Model to access elements of the environment within which it is running; for example, the FileSystemObject (FSO) is used to create, read, update and delete files. VBScript has been installed by default in every desktop release of Microsoft Windows since Windows 98; in Windows Server since Windows NT 4.0 Option Pack; and optionally with Windows CE (depending on the device it is installed on).
<p></p>
A VBScript script must be executed within a host environment, of which there are several provided with Microsoft Windows, including: Windows Script Host (WSH), Internet Explorer (IE), and Internet Information Services (IIS). Additionally, the VBScript hosting environment is embeddable in other programs, through technologies such as the Microsoft Script Control (msscript.ocx).
</details>
<p></p>
To see commands that are run using VBS we will use cmdline you can scroll through untill you have found what you are looking for however i have piped (|) to <kbd>grep</kbd> searching for vbs.
<p></p>

```
❯ sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 cmdline | grep -B 5 -A 5 vbs
Volatility Foundation Volatility Framework 2.6
************************************************************************
chrome.exe pid:   4688
Command line : "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --type=renderer --field-trial-handle=924,2132560186875629139,18153288653831290455,131072 --disable-gpu-compositing --service-pipe-token=2094820586012107996 --lang=en-US --enable-offline-auto-reload --enable-offline-auto-reload-visible-only --device-scale-factor=1 --num-raster-threads=1 --service-request-channel-token=2094820586012107996 --renderer-client-id=13 --no-v8-untrusted-code-mitigations --mojo-platform-channel-handle=3844 /prefetch:1
************************************************************************
wscript.exe pid:   5116
Command line : "C:\Windows\System32\wscript.exe" //B //NOLOGO %TEMP%\vhjReUDEuumrX.vbs
************************************************************************
UWkpjFjDzM.exe pid:   3496
Command line : "C:\Users\Bob\AppData\Local\Temp\rad93398.tmp\UWkpjFjDzM.exe" 
************************************************************************
cmd.exe pid:   4660
```

<p></p>
Here you can see wscript.exe is running and the .vbs script it has run giving us the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<vhjReUDEuumrX>
```


</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>thx microsoft</summary>
<p></p>
The thirteenth challenge we are given is:
<p></p>
An application was run at 2019-03-07 23:06:58 UTC, what is the name of the program? (Include extension)
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
For this we will use a volatility option called timeliner,
<p></p>

Option | Description
-------|--------------------------
timeliner | This timeliner plugin creates a timeline from various artifacts in memory from the following sources (items in parenthesis are filters that may be used with the <kbd>---type</kbd> flag in order to obtain only items of that artifact):<p></p>- System time (ImageDate)<br>- Processes (Process)<br>---    Create and Exit times<br>---    LastTrimTime (XP and 2003 only)<br>- DLLs (Process, LoadTime)<br>---    LoadTime (Windows 7 and 8 only)<br>- PE Timestamps (TimeDateStamp)<br>---    Modules/Processes/DLLs<br>---    _IMAGE_FILE_HEADER<br>---    _IMAGE_DEBUG_DIRECTORY<br>- Threads (Thread)<br>---    Create and Exit times<br>- Sockets (Socket)<br>---    Create time<br>- Event logs (XP and 2003 only) (EvtLog)<br>- IE History (IEHistory)<br>- Registry hives (_CMHIVE and _HBASE_BLOCK)<br>- Registry keys<br>---    LastWriteTime of registry keys in _CMHIVE (Registry)<br>---    LastWriteTime of registry key objects referenced in the handle table (_CM_KEY_BODY)<br>- Embedded registry (filters below)<br>---    Userassist<br>---    Shimcache<br>- Timers (Timer)<br>- Symbolic links (Symlink)<p></p>You can filter for any of the above options in order to have more focused output using the --type flag.

<p></p>
The command looks like this:
<p></p>

```
sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 timeliner | tee timeliner.txt
```

<p></p>
Now that we have a file we can run grep against it, the command and output looks like this:
<p></p>

```
❯ cat timeliner.txt| grep "2019-03-07 23:06:58"
2019-03-07 23:06:58 UTC+0000|[SHIMCACHE]| \??\C:\Program Files (x86)\Microsoft\Skype for Desktop\Skype.exe| 
```

<p></p>
Here we can see the answer and flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<skype.exe>
```

<p></p>
From the output we could have also run the option shimcache and obtained the same result.
</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>lightbulb moment</summary>
<p></p>
The fourtheenth challenge we are given is:
<p></p>
What was written in notepad.exe in the time of the memory dump?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
IOT start this challenge we need to dump the memory of the process running at the time of the memory dump. To do this we will use the memdump option, but to use this option we need the Pid of the notepad.exe, we will get this from the pstree.txt we made earlier. (I have included the header line from pstree)
<p></p>

```
❯ cat pstree.txt| grep notepad
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
. 0xfffffa80054f9060:notepad.exe                     3032   1432      1     60 2019-03-22 05:32:22 UTC+0000
```

<p></p>
So we have found the Pid, we will now use that in the memdump option.
<p></p>

```
❯ sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 memdump -p 3032 -D .
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing notepad.exe [  3032] to 3032.dmp
```

<p></p>
With this command the <kbd>-p</kbd> flag points the the Pid and the <kbd>-D</kbd> flag points points to the directory we want to dump to '.' being the current working directory.
<p></p>
Now that we have the process memory dumped we can start running <kbd>strings</kbd> and <kbd>grep</kbd> to search the file for our challenge flag. When we run strings we need to use the <kbd>-e l</kbd> flag to extract all the human-readable little eddian strings and then we can pipe (|) to <kbd>grep</kbd> and search for what we know is in the flag.
<p></p>

```
❯ strings -e l 3032.dmp | grep -i "flag<"
flag<REDBULL_IS_LIFE>
flag<Th>
flag<Th>
flag<TheK>
flag<TheK>
```

<p></p>
Here we can see the the output has returned the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<REDBULL_IS_LIFE>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>8675309</summary>
<p></p>
The fifteenth challenge we are given is:
<p></p>
What is the shortname of the file at file record 59045?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
Details about file records are held in the Master File Table (MFT) and can be extracted from the memory dump using the mftparser option.
<p></p>
<details>
    <summary>mftparser</summary>
<p></p>
This plugin scans for potential Master File Table (MFT) entries in memory (using "FILE" and "BAAD" signatures) and prints out information for certain attributes, currently: <kbd>$FILE_NAME</kbd> (<kbd>$FN</kbd>), <kbd>$STANDARD_INFORMATION</kbd> (<kbd>$SI</kbd>), <kbd>$FN</kbd> and <kbd>$SI</kbd> attributes from the <kbd>$ATTRIBUTE_LIST</kbd>, <kbd>$OBJECT_ID</kbd> (default output only) and resident <kbd>$DATA</kbd>.
<p></p>
 Options of interest include:
<p></p>
- <kbd>-machine</kbd> - Machine name to add to timeline header (useful when combining timelines from multiple machines)
<br>- <kbd>-D/--dump-dir</kbd> - Output directory to which resident data files are dumped
<br>- <kbd>-output=body</kbd> - print output in Sleuthkit 3.X body format
<br>- <kbd>-no-check</kbd> - Prints out all entries including those with null timestamps
<br>- <kbd>-E/--entry-size</kbd> - Changes the default 1024 byte MFT entry size.
<br>- <kbd>-o/--offset</kbd> - Prints out the MFT entry at a give offset (comma delimited)
</details>
<p></p>
The first thing we will do is run mftparser and pipe (|) it to tee so we have a .txt file we can run <kbd>grep</kbd> on multiple times. The command looks like this:
<p></p>

```
sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 mftparser | tee mftparser.txt
```

<p></p>
This is the final entry from mftparser.
<p></p>

```
***************************************************************************
***************************************************************************
MFT entry found at offset 0x13dbcdc78
Attribute: In Use & File
Record Number: 77271
Link count: 2


$STANDARD_INFORMATION
Creation                       Modified                       MFT Altered                    Access Date                    Type
------------------------------ ------------------------------ ------------------------------ ------------------------------ ----
2019-03-22 01:29:14 UTC+0000 2019-03-22 01:29:14 UTC+0000   2019-03-22 01:29:14 UTC+0000   2019-03-22 01:29:14 UTC+0000   Archive & Content not indexed

$FILE_NAME
Creation                       Modified                       MFT Altered                    Access Date                    Name/Path
------------------------------ ------------------------------ ------------------------------ ------------------------------ ---------
2019-03-22 01:29:14 UTC+0000 2019-03-22 01:29:14 UTC+0000   2019-03-22 01:29:14 UTC+0000   2019-03-22 01:29:14 UTC+0000   Users\Bob\AppData\Local\Temp\UIHLZO~1.PSM

$FILE_NAME
Creation                       Modified                       MFT Altered                    Access Date                    Name/Path
------------------------------ ------------------------------ ------------------------------ ------------------------------ ---------
2019-03-22 01:29:14 UTC+0000 2019-03-22 01:29:14 UTC+0000   2019-03-22 01:29:14 UTC+0000   2019-03-22 01:29:14 UTC+0000   Users\Bob\AppData\Local\Temp\uihlzovb.nhj.p좃m1

$DATA


***************************************************************************
```

<p></p>
Looking at this we can see:
<p></p>

```
***************************************************************************
***************************************************************************
MFT entry found at offset 0x13dbcdc78
Attribute: In Use & File
Record Number: 77271
Link count: 2
```

<p></p>
Here we can see the record number and then all the information comes post record number, this informs our grep so we know we need to include lines after our found record. The command looks like this:
<p></p>

```
cat mftparser.txt | grep -B 4 -A 20 "59045"
```

<p></p>
Which returns:
<p></p>

```
***************************************************************************
***************************************************************************
MFT entry found at offset 0x2193d400
Attribute: In Use & File
Record Number: 59045
Link count: 2


$STANDARD_INFORMATION
Creation                       Modified                       MFT Altered                    Access Date                    Type
------------------------------ ------------------------------ ------------------------------ ------------------------------ ----
2019-03-17 06:50:07 UTC+0000 2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:42 UTC+0000   Archive

$FILE_NAME
Creation                       Modified                       MFT Altered                    Access Date                    Name/Path
------------------------------ ------------------------------ ------------------------------ ------------------------------ ---------
2019-03-17 06:50:07 UTC+0000 2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:42 UTC+0000   Users\Bob\DOCUME~1\EMPLOY~1\EMPLOY~1.XLS

$FILE_NAME
Creation                       Modified                       MFT Altered                    Access Date                    Name/Path
------------------------------ ------------------------------ ------------------------------ ------------------------------ ---------
2019-03-17 06:50:07 UTC+0000 2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:43 UTC+0000   2019-03-17 07:04:42 UTC+0000   Users\Bob\DOCUME~1\EMPLOY~1\EmployeeInformation.xlsx

$OBJECT_ID
Object ID: 00fe50d2-4841-e911-8751-000c2958bc5f
```

<p></p>
Giving us the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<EMPLOY~1.XLS>
```

</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>whats-a-metasploit?</summary>
<p></p>
The sixteenth and final challenge we are given is:
<p></p>
This box was exploited and is running meterpreter. What PID was infected?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
Thinking back to our previous challenges we found a strance process running (UWkpjFjDzM.exe with a Pid of 3496) we can confirm this by dumping the process IOT upload it to <a href="https://www.virustotal.com/gui/" rel="nofollow">VirusTotal</a> to do this we will use the procdump option. The command looks like this:
<p></p>

```
❯ sudo volatility -f Triage-Memory.mem --profile=Win7SP1x64 procdump -p 3496 -D .
Volatility Foundation Volatility Framework 2.6
Process(V)         ImageBase          Name                 Result
------------------ ------------------ -------------------- ------
0xfffffa8005a1d9e0 0x0000000000400000 UWkpjFjDzM.exe       OK: executable.3496.exe
```

<p></p>
With this command the <kbd>-p</kbd> flag points to the Pid and the <kbd>-D</kbd> flag points to directory we want to dump the file to, '.' being the current working directory.
<br>
We can now upload this file to <a href="https://www.virustotal.com/gui/" rel="nofollow">VirusTotal</a>.
<p></p>
Which got a hit!
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/metasploit.jpg"><br>
</div>
<p></p>
After that I also ran it through a application called <a href="https://www.nextron-systems.com/thor-lite/" rel="nofollow">thor-lite</a>, it is a free APT and IOC scanner. I have included the output bellow (minus my pers details and some system information)
<p></p>

```
❯ sudo ./thor-lite-linux -p ~/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe --allreasons | tee thor.txt

   ###++++++   ________ ______  ___
   ###++++++  /_  __/ // / __ \/ _ \
   ###   +++   / / / _  / /_/ / , _/
   ###   +++  /_/ /_//_/\____/_/|_|  Lite
   ######+++ 
   ######+++  APT Scanner

> Scan Information ------------------------------------------------------------
Info: Startup Thor Version: 10.5.16
Info: Startup Thor Build: f4732fc (2021-05-05 13:21:43)
Info: Startup Run on system: parrot
Info: Startup Running as user: root
Info: Startup User has admin rights: yes
Info: Startup Working Directory: /home/parrot/MyTools
Info: Startup Thor Scan started TIME: Sat May  8 12:42:20 2021 HOSTNAME: parrot
Info: Startup Effective argument list: [--allreasons true --path /home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe --dbfile /var/lib/thor/thor10-lite.db]
Info: Startup Platform: Parrot OS 4.11
Info: Startup Platform DeepEval NAME: Parrot OS 4.11 KERNEL_NAME: Linux KERNEL_VERSION: 5.10.0-6parrot1-amd64 PROC: unknown ARCH: x86_64
Info: Startup Language: en_AU, Zone: AEST
Info: Startup System Uptime: 0.45 days
Info: Startup CPU Count: 4
Info: Startup Memory in Megabyte: 3900
Info: Startup Signature Database: 2021/05/05-092341
Info: Startup Successfully compiled 0 false positive filters TYPE: log filter
Info: Startup Writing report file to: parrot_thor_2021-05-08_1242.txt
Info: Startup Writing csv report file to: parrot_files_md5s.csv
Info: Startup No json report file will be written
Info: Startup Writing html report file to: parrot_thor_2021-05-08_1242.html
Info: Startup Syslog Export: off
Info: Startup IP Address 1: 192.168.234.131
Info: Startup ScanID: S-yQ1AKSfYPHM
Info: Startup System is not a domain controller
Info: Init Max. file size to be scanned is 12.0 MB, use --max_file_size to increase the limit
Info: Init Selected modules: Autoruns, EnvCheck, Filescan, Firewall, Hosts, LoggedIn, OpenFiles, ProcessCheck, ServiceCheck, UserDir, Users
Info: Init Deselected modules: DeepDive, Dropzone
Info: Init Selected features: Amcache, Archive, ArchiveScan, AtJobs, Bifrost2, C2, CPULimit, CheckString, DoublePulsar, EVTX, ExeDecompress, FilenameIOCs, Filescan, GroupsXML, KeywordIOCs, Lnk, LogScan, Prefetch, ProcessConnections, ProcessHandles, RegistryHive, Rescontrol, SHIMCache, SignalHandler, Stix, TeamViewer, ThorDB, Timestomp, VulnerabilityCheck, WER, WMIPersistence, WebdirScan, Yara
Info: Init Deselected features: Action, Bifrost, DumpScan, Sigma
Info: Init System Type: Server

> Reading YARA signatures and IOC files ... -----------------------------------
Info: Init Adding rule set from thor-lite-all.yas as 'default' type
Info: Init Adding rule set from thor-lite-keywords.yas as 'keyword' type
Info: Init Adding rule set from thor-lite-log-sigs.yas as 'log' type
Info: Init Adding rule set from thor-lite-process-memory-sigs.yas as 'process' type
Info: Init Ignoring rule set from thor-lite-registry.yas due to operating system type
Info: Init Successfully compiled 4115 default YARA rules TYPE: YARA
Info: Init Successfully compiled 7 log YARA rules TYPE: YARA
Info: Init Successfully compiled 0 keyword YARA rules TYPE: YARA
Info: Init Successfully compiled 1653 process YARA rules TYPE: YARA
Info: Init Successfully compiled 0 custom default YARA rules TYPE: YARA
Info: Init Skip sigma initialization, use '--sigma' flag to scan with sigma
Info: Init Reading iocs from /home/parrot/MyTools/signatures/iocs/c2-iocs.dat as 'domains' type
Info: Init Reading iocs from /home/parrot/MyTools/signatures/iocs/falsepositive-hashes.dat as false positive 'hash' type
Info: Init Reading iocs from /home/parrot/MyTools/signatures/iocs/filename-iocs.dat as 'filename' type
Info: Init Reading iocs from /home/parrot/MyTools/signatures/iocs/hash-iocs.dat as 'hash' type
Info: Init Reading iocs from /home/parrot/MyTools/signatures/iocs/keywords.dat as 'keyword' type
Info: Init Reading iocs from /home/parrot/MyTools/signatures/iocs/otx-hash-iocs.dat as 'hash' type
Info: Init Reading iocs from /home/parrot/MyTools/signatures/misc/file-type-signatures.dat as 'file-type-signatures' type
Info: Init Successfully compiled 25 keyword ioc strings TYPE: IOC
Info: Init Successfully compiled 2239 filename ioc strings and 428 filename ioc regexs TYPE: IOC
Info: Init Successfully compiled 49395 malware and 30 false positive hashes TYPE: IOC
Info: Init Successfully compiled 1548 malware domains TYPE: IOC
Info: Init Successfully compiled 0 malicious handles and 0 regex malicious handles TYPE: IOC
Info: Init Successfully compiled 0 named pipe ioc strings and 0 named pipe ioc regexs TYPE: IOC
Info: Init Successfully compiled 75 file type signatures TYPE: IOC
Info: Init No custom registry excludes defined
Info: Init No custom eventlog excludes defined
Info: Init Successfully opened ThorDB PATH: /var/lib/thor/thor10-lite.db ENTRIES: 44
Info: Init Adding 1548 malware domains to yara process memory and log scans
Info: Control Found last scan start with same context TIME: Sat May  8 02:38:38 2021 CONTEXT: -p /home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe --allreasons
Info: Control Found last scan finished with same context TIME: Sat May  8 02:38:45 2021 CONTEXT: -p /home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe --allreasons

> 1/3 > Running module 'Autoruns' ---------------------------------------------
Info: Autoruns Starting module
Info: Autoruns Finished module TOOK: 0 hours 0 mins 0 secs

> 2/3 > Running module 'ProcessCheck' -----------------------------------------

> 3/3 > Running module 'Filesystem Checks' ------------------------------------
Info: Filescan Starting module
Info: Filescan The following paths will be scanned: /home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe
Info: Filescan Scanning /home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe RECURSIVE
Warning: Filescan Possibly Dangerous file found
FILE: /home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe/executable.3496.exe EXT: .exe SCORE: 75
SIZE: 73728
CREATED: Fri May  7 18:58:19.889 2021 CHANGED: Sat May  8 12:34:30.802 2021 MODIFIED: Sat May  8 12:06:23.389 2021 ACCESSED: Fri May  7 18:58:19.889 2021 PERMISSIONS: -rw-r--r-- OWNER: root
MD5: 690ea20bc3bdfb328e23005d9a80c290
SHA1: ab120a232492dcfe8ff49e13f5720f63f0545dc2
SHA256: b6bdfee2e621949deddfc654dacd7bb8fce78836327395249e1f9b7b5ebfcfb1 TYPE: EXE FIRSTBYTES: 4d5a90000300000004000000ffff0000b8000000 / MZ
REASON_1: YARA rule Hunting_Rule_ShikataGaNai / - SUBSCORE_1: 75 REF_1: https://www.fireeye.com/blog/threat-research/2019/10/shikata-ga-nai-encoder-still-going-strong.html MATCHED_1: Str1: { d9 74 24 f4 bb be 7a 43 19 5a 29 c9 b1 47 31 5a 18 } TAGS_1:  RULEDATE_1: 1970-01-01 SIGTYPE_1: internal
Info: Filescan Finished module TOOK: 0 hours 0 mins 0 secs

Results -----------------------------------------------------------------------
Info: Report Results ALERTS: 0 WARNINGS: 1 ERRORS: 0
Info: Report For details see the log files written to ["parrot_thor_2021-05-08_1242.txt" "parrot_thor_2021-05-08_1242.html"]
Info: Report Begin Time: Sat May  8 12:42:23 2021
Info: Report End Time: Sat May  8 12:42:29 2021
Info: Report Scan took 0 hours 0 mins 6 secs
Notice: Report Thor Scan finished TIME: Sat May  8 12:42:29 2021 ALERTS: 0 WARNINGS: 1 NOTICES: 1 ERRORS: 0
Info: ThorDB Successfully closed ThorDB
```

<p></p>
If we look specifically at the 'Filesystem Checks' we can see a warning and a website.
<p></p>

```
> 3/3 > Running module 'Filesystem Checks' ------------------------------------
Info: Filescan Starting module
Info: Filescan The following paths will be scanned: /home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe
Info: Filescan Scanning /home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe RECURSIVE
Warning: Filescan Possibly Dangerous file found
FILE: /home/parrot/ctf/Defcon_DFIR_CTF/Memory_Forensics/exe/executable.3496.exe EXT: .exe SCORE: 75
SIZE: 73728
CREATED: Fri May  7 18:58:19.889 2021 CHANGED: Sat May  8 12:34:30.802 2021 MODIFIED: Sat May  8 12:06:23.389 2021 ACCESSED: Fri May  7 18:58:19.889 2021 PERMISSIONS: -rw-r--r-- OWNER: root
MD5: 690ea20bc3bdfb328e23005d9a80c290
SHA1: ab120a232492dcfe8ff49e13f5720f63f0545dc2
SHA256: b6bdfee2e621949deddfc654dacd7bb8fce78836327395249e1f9b7b5ebfcfb1 TYPE: EXE FIRSTBYTES: 4d5a90000300000004000000ffff0000b8000000 / MZ
REASON_1: YARA rule Hunting_Rule_ShikataGaNai / - SUBSCORE_1: 75 REF_1: https://www.fireeye.com/blog/threat-research/2019/10/shikata-ga-nai-encoder-still-going-strong.html MATCHED_1: Str1: { d9 74 24 f4 bb be 7a 43 19 5a 29 c9 b1 47 31 5a 18 } TAGS_1:  RULEDATE_1: 1970-01-01 SIGTYPE_1: internal
Info: Filescan Finished module TOOK: 0 hours 0 mins 0 secs
```

<p></p>
And if we visit that <a href="https://www.fireeye.com/blog/threat-research/2019/10/shikata-ga-nai-encoder-still-going-strong.html" rel="nofollow">website</a> we get a direct reference to Metasploit.
<p></p>
This was enough for me to enter this .exe's Pid as the flag and get our final answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
flag<3496>
```

</details>
</details>
</details>




</details>

<p></p>
<hr>
<p></p>



<H3>Rick (Memory Forensics)</H3>
<p></p>
Is a series of challenges from the <a href="https://otterctf.com/" rel="nofollow">OtterCTF</a> from 2019, these challenges will use the file OtterCTF.vmem
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1yy7wWcRhQOlJLQPkWEuVlajEJueKMACz/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
The answers for these challenges are 'CTF{ANSWER}'
<p></p>
<details>
    <summary>Extracting the File</summary>
<p></p>
When you download it the file comes as OtterCTF.7z you can either extract it through an archive manager or through CLI which I will explain now.
<p></p>
The first thing you will need is to install p7zip-full if you haven't already. To do this run this command:
<p></p>

```
sudo apt install p7zip-full
```

Now that's installed you can run this command on the file:

```
p7zip -d dump.7z
```

Which will output this and extract dump.vmem:

```
❯ p7zip -d OtterCTF.7z

7-Zip (a) [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_AU.UTF-8,Utf16=on,HugeFiles=on,64 bits,8 CPUs Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz (806EA),ASM,AES-NI)

Scanning the drive for archives:
1 file, 507206872 bytes (484 MiB)

Extracting archive: OtterCTF.7z
--
Path = OtterCTF.7z
Type = 7z
Physical Size = 507206872
Headers Size = 130
Method = LZMA2:24
Solid = -
Blocks = 1

Everything is Ok    

Size:       2147483648
Compressed: 507206872
```

<p></p>
Once we determine the image profile using <kbd>imageinfo</kbd> (refer to Getting your Foothold ) we can begin our analysis on OtterCTF.vmem
<p></p>
</details>
<p></p>
<details>
    <summary>Challenges</summary>
<p></p>

<p></p>
<details>
    <summary>What the Password</summary>
<p></p>
The first challenge we are given is [warning this challenge may be broken, it is unlikely you will be able to crack the hash and the plugin used to crack my require a custom python enviroment IOT run, feel free to skip to the next challenge]:
<p></p>
You got a sample of rick's PC's memory. can you get his user password?
<p></p>
format: CTF{FLAG}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
So what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- We need to get Rick's current password, we can either do it the same was as A Bob's Life 5 or we can use a custom plugin.
<p></p>
Since I have already explained how to do it via hash extraction I will go over the use of a custom plugin.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
So now we know what we are looking for how do we go about using a custom plugin?
<br>
The plugin we will use is called mimikatz which is a standalone program used to analyse memory on Windows machines however someone has written it as a plugin for volatility.
<p></p>
First we need to download the plugin:
<p></p>

```
cd /usr/share/volatility
```

<p></p>

```
mkdir plugins
```

<p></p>

```
wget https://raw.githubusercontent.com/dfirfpi/hotoloti/master/volatility/mimikatz.py
```

<p></p>

```
apt-get install python-crypto
```

<p></p>

Now that we have downloaded the plugin we can use it on the memory file.
<p></p>
We need to ensure that we use the <kbd>--plugins</kbd> flag as shown below.
<p></p>
The command we will use is:
<p></p>

```
sudo volatility --plugins=/usr/share/volatility/plugins --profile=Win7SP1x64 -f OtterCTF.vmem mimikatz
```

<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/image2.png"><br>
</div>
<p></p>

Which gives us the answers
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{MortyIsReallyAnOtter}
</details>
</details>
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>General Info</summary>
<p></p>
The second challenge we are given is:
<p></p>
Let's start easy - whats the PC's name and IP address?
<p></p>
format: CTF{FLAG}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
First What information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- We need to locate the computers IP address
<br>
- We need to find out what the computers name is
<p></p>
There are two seperate flags in this challenge, 1 is easy to locate the second requires more work and research.
<br>
We will start with finding the IP address followed by the hostname.
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
<H3>Finding the IP Address</H3>
<p></p>
First we need to find the IP address of the system, to do this we will need to use an option that looks at networking and TCP/UDP connections. There are multiple options for this however, because we know the system is Win7SP1x64 both connections and connscan will not work so we will use netscan. The command looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 netscan
```

<p></p>
Which returns:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 netscan                                                                                                                               
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x7d60f010         UDPv4    0.0.0.0:1900                   *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:17 UTC+0000                                         
0x7d62b3f0         UDPv4    192.168.202.131:6771           *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:22 UTC+0000                                         
0x7d62f4c0         UDPv4    127.0.0.1:62307                *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:17 UTC+0000                                         
0x7d62f920         UDPv4    192.168.202.131:62306          *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:17 UTC+0000                                         
0x7d6424c0         UDPv4    0.0.0.0:50762                  *:*                                   4076     chrome.exe     2018-08-04 19:33:37 UTC+0000                                         
0x7d6b4250         UDPv6    ::1:1900                       *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000                                         
0x7d6e3230         UDPv4    127.0.0.1:6771                 *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:22 UTC+0000                                         
0x7d6ed650         UDPv4    0.0.0.0:5355                   *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d71c8a0         UDPv4    0.0.0.0:0                      *:*                                   868      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d71c8a0         UDPv6    :::0                           *:*                                   868      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d74a390         UDPv4    127.0.0.1:52847                *:*                                   2624     bittorrentie.e 2018-08-04 19:27:24 UTC+0000                                         
0x7d7602c0         UDPv4    127.0.0.1:52846                *:*                                   2308     bittorrentie.e 2018-08-04 19:27:24 UTC+0000                                         
0x7d787010         UDPv4    0.0.0.0:65452                  *:*                                   4076     chrome.exe     2018-08-04 19:33:42 UTC+0000                                         
0x7d789b50         UDPv4    0.0.0.0:50523                  *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d789b50         UDPv6    :::50523                       *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d92a230         UDPv4    0.0.0.0:0                      *:*                                   868      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d92a230         UDPv6    :::0                           *:*                                   868      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d9e8b50         UDPv4    0.0.0.0:20830                  *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:15 UTC+0000                                         
0x7d9f4560         UDPv4    0.0.0.0:0                      *:*                                   3856     WebCompanion.e 2018-08-04 19:34:22 UTC+0000                                         
0x7d9f8cb0         UDPv4    0.0.0.0:20830                  *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:15 UTC+0000
0x7d9f8cb0         UDPv6    :::20830                       *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:15 UTC+0000
0x7d8bb390         TCPv4    0.0.0.0:9008                   0.0.0.0:0            LISTENING        4        System         
0x7d8bb390         TCPv6    :::9008                        :::0                 LISTENING        4        System         
0x7d9a9240         TCPv4    0.0.0.0:8733                   0.0.0.0:0            LISTENING        4        System         
0x7d9a9240         TCPv6    :::8733                        :::0                 LISTENING        4        System         
0x7d9e19e0         TCPv4    0.0.0.0:20830                  0.0.0.0:0            LISTENING        2836     BitTorrent.exe 
0x7d9e19e0         TCPv6    :::20830                       :::0                 LISTENING        2836     BitTorrent.exe 
0x7d9e1c90         TCPv4    0.0.0.0:20830                  0.0.0.0:0            LISTENING        2836     BitTorrent.exe 
0x7d42ba90         TCPv4    -:0                            56.219.196.26:0      CLOSED           2836     BitTorrent.exe 
0x7d6124d0         TCPv4    192.168.202.131:49530          77.102.199.102:7575  CLOSED           708      LunarMS.exe    
0x7d62d690         TCPv4    192.168.202.131:49229          169.1.143.215:8999   CLOSED           2836     BitTorrent.exe 
0x7d634350         TCPv6    -:0                            38db:c41a:80fa:ffff:38db:c41a:80fa:ffff:0 CLOSED           2836     BitTorrent.exe 
0x7d6f27f0         TCPv4    192.168.202.131:50381          71.198.155.180:34674 CLOSED           2836     BitTorrent.exe 
0x7d704010         TCPv4    192.168.202.131:50382          92.251.23.204:6881   CLOSED           2836     BitTorrent.exe 
0x7d708cf0         TCPv4    192.168.202.131:50364          91.140.89.116:31847  CLOSED           2836     BitTorrent.exe 
0x7d729620         TCPv4    -:50034                        142.129.37.27:24578  CLOSED           2836     BitTorrent.exe 
0x7d72cbe0         TCPv4    192.168.202.131:50340          23.37.43.27:80       CLOSED           3496     Lavasoft.WCAss 
0x7d7365a0         TCPv4    192.168.202.131:50358          23.37.43.27:80       CLOSED           3856     WebCompanion.e 
0x7d81c890         TCPv4    192.168.202.131:50335          185.154.111.20:60405 CLOSED           2836     BitTorrent.exe 
0x7d8fd530         TCPv4    192.168.202.131:50327          23.37.43.27:80       CLOSED           3496     Lavasoft.WCAss 
0x7d9cecf0         TCPv4    192.168.202.131:50373          173.239.232.46:2997  CLOSED           2836     BitTorrent.exe 
0x7d9d7cf0         TCPv4    192.168.202.131:50371          191.253.122.149:59163 CLOSED           2836     BitTorrent.exe 
0x7daefec0         UDPv4    0.0.0.0:0                      *:*                                   3856     WebCompanion.e 2018-08-04 19:34:22 UTC+0000
0x7daefec0         UDPv6    :::0                           *:*                                   3856     WebCompanion.e 2018-08-04 19:34:22 UTC+0000
0x7db83b90         UDPv4    0.0.0.0:0                      *:*                                   3880     WebCompanionIn 2018-08-04 19:33:30 UTC+0000
0x7db83b90         UDPv6    :::0                           *:*                                   3880     WebCompanionIn 2018-08-04 19:33:30 UTC+0000
0x7db9cdd0         UDPv4    0.0.0.0:0                      *:*                                   2844     WebCompanion.e 2018-08-04 19:30:05 UTC+0000
0x7db9cdd0         UDPv6    :::0                           *:*                                   2844     WebCompanion.e 2018-08-04 19:30:05 UTC+0000
0x7dc2dc30         UDPv4    0.0.0.0:50879                  *:*                                   4076     chrome.exe     2018-08-04 19:30:41 UTC+0000
0x7dc2dc30         UDPv6    :::50879                       *:*                                   4076     chrome.exe     2018-08-04 19:30:41 UTC+0000
0x7dc83810         UDPv4    0.0.0.0:5355                   *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7dc83810         UDPv6    :::5355                        *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7dd82c30         UDPv4    0.0.0.0:5355                   *:*                                   620      svchost.exe    2018-08-04 19:26:38 UTC+0000
0x7df00980         UDPv4    0.0.0.0:0                      *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7df00980         UDPv6    :::0                           *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7df04cc0         UDPv4    0.0.0.0:5355                   *:*                                   620      svchost.exe    2018-08-04 19:26:38 UTC+0000
0x7df04cc0         UDPv6    :::5355                        *:*                                   620      svchost.exe    2018-08-04 19:26:38 UTC+0000
0x7df5f010         UDPv4    0.0.0.0:55175                  *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7dfab010         UDPv4    0.0.0.0:58383                  *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7dfab010         UDPv6    :::58383                       *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7e12c1c0         UDPv4    0.0.0.0:0                      *:*                                   3880     WebCompanionIn 2018-08-04 19:33:27 UTC+0000
0x7e163a40         UDPv4    0.0.0.0:0                      *:*                                   3880     WebCompanionIn 2018-08-04 19:33:27 UTC+0000
0x7e163a40         UDPv6    :::0                           *:*                                   3880     WebCompanionIn 2018-08-04 19:33:27 UTC+0000
0x7e1cf010         UDPv4    192.168.202.131:137            *:*                                   4        System         2018-08-04 19:26:35 UTC+0000
0x7e1da010         UDPv4    192.168.202.131:138            *:*                                   4        System         2018-08-04 19:26:35 UTC+0000
0x7dc4ad30         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        500      lsass.exe      
0x7dc4ad30         TCPv6    :::49155                       :::0                 LISTENING        500      lsass.exe      
0x7dc4b370         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        500      lsass.exe      
0x7dd71010         TCPv4    0.0.0.0:445                    0.0.0.0:0            LISTENING        4        System         
0x7dd71010         TCPv6    :::445                         :::0                 LISTENING        4        System         
0x7ddca6b0         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        492      services.exe   
0x7ddcbc00         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        492      services.exe   
0x7ddcbc00         TCPv6    :::49156                       :::0                 LISTENING        492      services.exe   
0x7de09c30         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        396      wininit.exe    
0x7de09c30         TCPv6    :::49152                       :::0                 LISTENING        396      wininit.exe    
0x7de0d7b0         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        396      wininit.exe    
0x7de424e0         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        808      svchost.exe    
0x7de45ef0         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        808      svchost.exe    
0x7de45ef0         TCPv6    :::49153                       :::0                 LISTENING        808      svchost.exe    
0x7df3d270         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        868      svchost.exe    
0x7df3eef0         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        868      svchost.exe    
0x7df3eef0         TCPv6    :::49154                       :::0                 LISTENING        868      svchost.exe    
0x7e1f6010         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        712      svchost.exe    
0x7e1f6010         TCPv6    :::135                         :::0                 LISTENING        712      svchost.exe    
0x7e1f8ef0         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        712      svchost.exe    
0x7db000a0         TCPv4    -:50091                        93.142.197.107:32645 CLOSED           2836     BitTorrent.exe 
0x7db132e0         TCPv4    192.168.202.131:50280          72.55.154.81:80      CLOSED           3880     WebCompanionIn 
0x7dbc3010         TCPv6    -:0                            4847:d418:80fa:ffff:4847:d418:80fa:ffff:0 CLOSED           4076     chrome.exe     
0x7dc4bcf0         TCPv4    -:0                            104.240.179.26:0     CLOSED           3        ?4????       
0x7dc83080         TCPv4    192.168.202.131:50377          179.108.238.10:19761 CLOSED           2836     BitTorrent.exe 
0x7dd451f0         TCPv4    192.168.202.131:50321          45.27.208.145:51414  CLOSED           2836     BitTorrent.exe 
0x7ddae890         TCPv4    -:50299                        212.92.105.227:8999  CLOSED           2836     BitTorrent.exe 
0x7ddff010         TCPv4    192.168.202.131:50379          23.37.43.27:80       CLOSED           3856     WebCompanion.e 
0x7e0057d0         TCPv4    192.168.202.131:50353          85.242.139.158:51413 CLOSED           2836     BitTorrent.exe 
0x7e0114b0         TCPv4    192.168.202.131:50339          77.65.111.216:8306   CLOSED           2836     BitTorrent.exe 
0x7e042cf0         TCPv4    192.168.202.131:50372          83.44.27.35:52103    CLOSED           2836     BitTorrent.exe 
0x7e08a010         TCPv4    192.168.202.131:50374          89.46.49.163:20133   CLOSED           2836     BitTorrent.exe 
0x7e092010         TCPv4    192.168.202.131:50378          120.29.114.41:13155  CLOSED           2836     BitTorrent.exe 
0x7e094b90         TCPv4    192.168.202.131:50365          52.91.1.182:55125    CLOSED           2836     BitTorrent.exe 
0x7e09ba90         TCPv6    -:0                            68f0:181b:80fa:ffff:68f0:181b:80fa:ffff:0 CLOSED           2836     BitTorrent.exe 
0x7e0a8b90         TCPv4    192.168.202.131:50341          72.55.154.81:80      CLOSED           3880     WebCompanionIn 
0x7e0d6180         TCPv4    192.168.202.131:50349          196.250.217.22:32815 CLOSED           2836     BitTorrent.exe 
0x7e108100         TCPv4    192.168.202.131:50360          174.0.234.77:31240   CLOSED           2836     BitTorrent.exe 
0x7e124910         TCPv4    192.168.202.131:50366          89.78.106.196:51413  CLOSED           2836     BitTorrent.exe 
0x7e14dcf0         TCPv4    192.168.202.131:50363          122.62.218.159:11627 CLOSED           2836     BitTorrent.exe 
0x7e18bcf0         TCPv4    192.168.202.131:50333          191.177.124.34:21011 CLOSED           2836     BitTorrent.exe 
0x7e1f7ab0         TCPv4    -:0                            56.187.190.26:0      CLOSED           3        ?4????       
0x7e48d9c0         UDPv6    fe80::b06b:a531:ec88:457f:1900 *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
0x7e4ad870         UDPv4    127.0.0.1:1900                 *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
0x7e511bb0         UDPv4    0.0.0.0:60005                  *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7e5dc3b0         UDPv6    fe80::b06b:a531:ec88:457f:546  *:*                                   808      svchost.exe    2018-08-04 19:33:28 UTC+0000
0x7e7469c0         UDPv4    0.0.0.0:50878                  *:*                                   4076     chrome.exe     2018-08-04 19:30:39 UTC+0000
0x7e7469c0         UDPv6    :::50878                       *:*                                   4076     chrome.exe     2018-08-04 19:30:39 UTC+0000
0x7e77cb00         UDPv4    0.0.0.0:50748                  *:*                                   4076     chrome.exe     2018-08-04 19:30:07 UTC+0000
0x7e77cb00         UDPv6    :::50748                       *:*                                   4076     chrome.exe     2018-08-04 19:30:07 UTC+0000
0x7e79f3f0         UDPv4    0.0.0.0:5353                   *:*                                   4076     chrome.exe     2018-08-04 19:29:35 UTC+0000
0x7e7a0ec0         UDPv4    0.0.0.0:5353                   *:*                                   4076     chrome.exe     2018-08-04 19:29:35 UTC+0000
0x7e7a0ec0         UDPv6    :::5353                        *:*                                   4076     chrome.exe     2018-08-04 19:29:35 UTC+0000
0x7e7a3960         UDPv4    0.0.0.0:0                      *:*                                   3880     WebCompanionIn 2018-08-04 19:33:30 UTC+0000
0x7e7dd010         UDPv6    ::1:58340                      *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
0x7e413a40         TCPv4    -:0                            -:0                  CLOSED           708      LunarMS.exe    
0x7e415010         TCPv4    192.168.202.131:50346          89.64.10.176:10589   CLOSED           2836     BitTorrent.exe 
0x7e4202d0         TCPv4    192.168.202.131:50217          104.18.21.226:80     CLOSED           3880     WebCompanionIn 
0x7e45f110         TCPv4    192.168.202.131:50211          104.18.20.226:80     CLOSED           3880     WebCompanionIn 
0x7e4cc910         TCPv4    192.168.202.131:50228          104.18.20.226:80     CLOSED           3880     WebCompanionIn 
0x7e512950         TCPv4    192.168.202.131:50345          77.126.30.221:13905  CLOSED           2836     BitTorrent.exe 
0x7e521b50         TCPv4    -:0                            -:0                  CLOSED           708      LunarMS.exe    
0x7e5228d0         TCPv4    192.168.202.131:50075          70.65.116.120:52700  CLOSED           2836     BitTorrent.exe 
0x7e52f010         TCPv4    192.168.202.131:50343          86.121.4.189:46392   CLOSED           2836     BitTorrent.exe 
0x7e563860         TCPv4    192.168.202.131:50170          103.232.25.44:25384  CLOSED           2836     BitTorrent.exe 
0x7e572cf0         TCPv4    192.168.202.131:50125          122.62.218.159:11627 CLOSED           2836     BitTorrent.exe 
0x7e5d6cf0         TCPv4    192.168.202.131:50324          54.197.8.177:49420   CLOSED           2836     BitTorrent.exe 
0x7e71b010         TCPv4    192.168.202.131:50344          70.27.98.75:6881     CLOSED           2836     BitTorrent.exe 
0x7e71d010         TCPv4    192.168.202.131:50351          99.251.199.160:1045  CLOSED           2836     BitTorrent.exe 
0x7e74b010         TCPv4    192.168.202.131:50385          209.236.6.89:56500   CLOSED           2836     BitTorrent.exe 
0x7e78b7f0         TCPv4    192.168.202.131:50238          72.55.154.82:80      CLOSED           3880     WebCompanionIn 
0x7e7ae380         TCPv4    192.168.202.131:50361          5.34.21.181:8999     CLOSED           2836     BitTorrent.exe 
0x7e7b0380         TCPv6    -:0                            4847:d418:80fa:ffff:4847:d418:80fa:ffff:0 CLOSED           2836     BitTorrent.exe 
0x7e7b9010         TCPv4    192.168.202.131:50334          188.129.94.129:25128 CLOSED           2836     BitTorrent.exe 
0x7e94b010         TCPv4    192.168.202.131:50356          77.126.30.221:13905  CLOSED           2836     BitTorrent.exe 
0x7e9ad840         TCPv4    192.168.202.131:50380          84.52.144.29:56299   CLOSED           2836     BitTorrent.exe 
0x7e9bacf0         TCPv4    192.168.202.131:50350          77.253.242.0:5000    CLOSED           2836     BitTorrent.exe 
0x7eaac5e0         TCPv4    192.168.202.131:50387          93.184.220.29:80     CLOSED           3856     WebCompanion.e 
0x7eab4cf0         TCPv4    -:0                            56.219.196.26:0      CLOSED           2836     BitTorrent.exe 
0x7fb9cec0         UDPv4    192.168.202.131:1900           *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
0x7fb9d430         UDPv4    127.0.0.1:58341                *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
```

<p></p>
Looking through the output if we go to the local address column we can see 1 address that is repeating.
<p></p>

```
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x7d62b3f0         UDPv4    192.168.202.131:6771           *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:22 UTC+0000                                         
```

<p></p>
Giving us our first answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{192.168.202.131}
<p></p>
</details>
<p></p>
<H3>Finding the PC Name</H3>
<p></p>
We will now go on with finding what the PC's name is (There is more than 1 way we will do the more difficult version first), this is more difficult than running a single option and looking at the output and will require some research to complete.
<br>
To do this we will need to look in the registry files to locate the computer name.
<br>
First we need to know where in the registry the computer name is located, a quick google search gives us our answer.
<br>
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName
<br>
Now that we know where it is located in the Registry we can use volatility to look at this information. First we need to list the hives IOT find the SYSTEM hive. We will use the following command:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 hivelist
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 hivelist
Volatility Foundation Volatility Framework 2.6
Virtual            Physical           Name
------------------ ------------------ ----
0xfffff8a00377d2d0 0x00000000624162d0 \??\C:\System Volume Information\Syscache.hve
0xfffff8a00000f010 0x000000002d4c1010 [no name]
0xfffff8a000024010 0x000000002d50c010 \REGISTRY\MACHINE\SYSTEM
0xfffff8a000053320 0x000000002d5bb320 \REGISTRY\MACHINE\HARDWARE
0xfffff8a000109410 0x0000000029cb4410 \SystemRoot\System32\Config\SECURITY
0xfffff8a00033d410 0x000000002a958410 \Device\HarddiskVolume1\Boot\BCD
0xfffff8a0005d5010 0x000000002a983010 \SystemRoot\System32\Config\SOFTWARE
0xfffff8a001495010 0x0000000024912010 \SystemRoot\System32\Config\DEFAULT
0xfffff8a0016d4010 0x00000000214e1010 \SystemRoot\System32\Config\SAM
0xfffff8a00175b010 0x00000000211eb010 \??\C:\Windows\ServiceProfiles\NetworkService\NTUSER.DAT
0xfffff8a00176e410 0x00000000206db410 \??\C:\Windows\ServiceProfiles\LocalService\NTUSER.DAT
0xfffff8a002090010 0x000000000b92b010 \??\C:\Users\Rick\ntuser.dat
0xfffff8a0020ad410 0x000000000db41410 \??\C:\Users\Rick\AppData\Local\Microsoft\Windows\UsrClass.dat
```

<p></p>
The important information we need is this:
<p></p>

```
Virtual            Physical           Name
------------------ ------------------ ----
0xfffff8a000024010 0x000000002d50c010 \REGISTRY\MACHINE\SYSTEM
```

<p></p>
With this information (particularly the Virtual Offset) we can now use the <kbd>printkey</kbd> option to read the registry using the location we discovered before.
<br>

Option | Description
-------|-------------
printkey | Display the subkeys, values, data, and data types contained within a specified registry key, use the printkey command. By default, printkey will search all hives and print the key information (if found) for the requested key. Therefore, if the key is located in more than one hive, the information for the key will be printed for each hive that contains it. If you want to limit your search to a specific hive, printkey also accepts a virtual address to the hive. use the command with a <kbd>-o</kbd> flag specifying the virtual offset.

<br>
Since we know where the information we are looking for is located the command will look like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 printkey -o 0xfffff8a000024010 -K 'ControlSet\Control\ComputerName\ComputerName'
```

<p></p>
Which outputs this:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 printkey -o 0xfffff8a000024010 -K 'ControlSet\Control\ComputerName\ComputerName'
Volatility Foundation Volatility Framework 2.6
Legend: (S) = Stable   (V) = Volatile

The requested key could not be found in the hive(s) searched
```

<p></p>
This wasn't our expected output, the issue is the ControlSet we need to start with the first one but how can we find this information out? We can either run the <kbd>printkey</kbd> command only using the <kbd>-o</kbd> flag or by using the <kbd>handles</kbd> option: 
<br>

Option | Description
-------|--------------
handles | To display the open handles in a process, use the handles command. This applies to files, registry keys, mutexes, named pipes, events, window stations, desktops, threads, and all other types of securable executive objects. As of 2.1, the output includes handle value and granted access for each object. <p></p> You can display handles for a particular process by specifying --pid=PID or the physical offset of an _EPROCESS structure (--physical-offset=OFFSET). You can also filter by object type using -t or --object-type=OBJECTTYPE.

<br>
You can try using handles however I am going to use the <kbd>printkey</kbd> option only using the <kbd>-o</kbd> flag. The handles command looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 handles -t Key
```

<p></p>
We used the <kbd>-t Key</kbd> flag to return only the key values, which still outputs a stack of data but you can see that ControlSet001 is repeated throughout the output. Now that we have this information we can repeat the <kbd>printkey</kbd> option.
<p></p>
The <kbd>printkey</kbd> option looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 printkey -o 0xfffff8a000024010
```

<p></p>
We got the value for the <kbd>-o</kbd> flag from the <kbd>hivelist</kbd> output earlier.
<br>
This outputs the following:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 printkey -o 0xfffff8a000024010
Volatility Foundation Volatility Framework 2.6
Legend: (S) = Stable   (V) = Volatile

----------------------------
Registry: \REGISTRY\MACHINE\SYSTEM
Key name: CMI-CreateHive{2A7FB991-7BBE-4F9D-B91E-7CB51D4737F5} (S)
Last updated: 2018-08-04 19:25:54 UTC+0000

Subkeys:
  (S) ControlSet001
  (S) ControlSet002
  (S) MountedDevices
  (S) RNG
  (S) Select
  (S) Setup
  (S) Software
  (S) WPA
  (V) CurrentControlSet

Values:
```

<p></p>
This output shows us the ControlSet being listed as ControlSet001, we can now use this information to run the printkey command again giving us this output and the answer:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 printkey -K 'ControlSet001\Control\ComputerName\ComputerName'
Volatility Foundation Volatility Framework 2.6
Legend: (S) = Stable   (V) = Volatile

----------------------------
Registry: \REGISTRY\MACHINE\SYSTEM
Key name: ComputerName (S)
Last updated: 2018-06-02 19:23:00 UTC+0000

Subkeys:

Values:
REG_SZ                        : (S) mnmsrvc
REG_SZ        ComputerName    : (S) WIN-LO6FAF3DTFE
```

<p></p>
Giving us our Answer.
<p></p>
<details>
    <summary>Answer and Alternate method</summary>
<p></p>
CTF{WIN-LO6FAF3DTFE}
<p></p>
The alternate method is much easier however you wouldn't have learned how to interogate registry files, it uses the option <kbd>envars</kbd>. (We append it to a text file because of the size of the output and the increased speed using <kbd>grep</kbd> from a text file, I then use double ampersands (&&) to run a command on completion of the volatility output in this case we want to cat and then grep the output file). The command looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 envars > envars.txt && cat envars.txt | grep -i computername
```

<p></p>
Which outputs this:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 envars > envars.txt && cat envars.txt| grep -i computername                                                                  [20/4909]
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
     396 wininit.exe          0x00000000002abae0 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     432 winlogon.exe         0x00000000001afd70 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     492 services.exe         0x0000000000091320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     500 lsass.exe            0x0000000000481320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     508 lsm.exe              0x00000000003d1320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     604 svchost.exe          0x0000000000251320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     668 vmacthlp.exe         0x0000000000421320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     712 svchost.exe          0x00000000002a1320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     808 svchost.exe          0x0000000000261320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     844 svchost.exe          0x00000000001b1320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     868 svchost.exe          0x0000000000341320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
    1012 svchost.exe          0x0000000000241320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
     620 svchost.exe          0x00000000002c1320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
    1120 spoolsv.exe          0x0000000000131320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
    1164 svchost.exe          0x0000000000291320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
    1356 VGAuthService.       0x00000000002c1320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
    1428 vmtoolsd.exe         0x0000000000341320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
    1800 WmiPrvSE.exe         0x0000000000191320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
    1948 svchost.exe          0x00000000003f1320 COMPUTERNAME                   WIN-LO6FAF3DTFE                                                                                               
    1324 dllhost.exe          0x0000000000441320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    1436 msdtc.exe            0x0000000000321320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2136 WmiPrvSE.exe         0x00000000001e1320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2344 taskhost.exe         0x0000000000201320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2500 sppsvc.exe           0x0000000000341320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2704 dwm.exe              0x00000000001c1320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2728 explorer.exe         0x00000000025e1a90 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2804 vmtoolsd.exe         0x0000000000341320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2836 BitTorrent.exe       0x00000000001f1320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3064 SearchIndexer.       0x00000000001bf170 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2308 bittorrentie.e       0x0000000000321320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2624 bittorrentie.e       0x0000000000531320 COMPUTERNAME                   WIN-LO6FAF3DTFE
     708 LunarMS.exe          0x00000000002f1320 COMPUTERNAME                   WIN-LO6FAF3DTFE
     724 PresentationFo       0x00000000001a1320 COMPUTERNAME                   WIN-LO6FAF3DTFE
     412 mscorsvw.exe         0x0000000000371320 COMPUTERNAME                   WIN-LO6FAF3DTFE
     164 svchost.exe          0x0000000000241320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3124 mscorsvw.exe         0x0000000000361320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3196 svchost.exe          0x00000000003aeaa0 COMPUTERNAME                   WIN-LO6FAF3DTFE
    4076 chrome.exe           0x0000000005f61240 COMPUTERNAME                   WIN-LO6FAF3DTFE
    4084 chrome.exe           0x0000000000341320 COMPUTERNAME                   WIN-LO6FAF3DTFE
     576 chrome.exe           0x0000000000471320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    1808 chrome.exe           0x00000000003b1320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3924 chrome.exe           0x00000000004a1320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    2748 chrome.exe           0x0000000000571320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3820 Rick And Morty       0x0000000000331320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3720 vmware-tray.ex       0x0000000000331320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3880 WebCompanionIn       0x0000000000401320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3648 chrome.exe           0x0000000000541320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    1796 chrome.exe           0x00000000002a1320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3496 Lavasoft.WCAss       0x0000000000311320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3856 WebCompanion.e       0x0000000000151320 COMPUTERNAME                   WIN-LO6FAF3DTFE
    3304 notepad.exe          0x0000000000301320 COMPUTERNAME                   WIN-LO6FAF3DTFE
```

<p></p>
Resulting in us obtaining the same answer although much easier.
</details>
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Play Time</summary>
<p></p>
The third challenge we are given is:
<p></p>
Rick just loves to play some good old videogames. can you tell which game is he playing? whats the IP address of the server?
<p></p>
There are two flags for this challenge, this first being the name of the game and the second being the IP of the server.
<p></p> 
format: CTF{flag}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
First what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- He is playing a game and it has internet connectivity meaning we will need to investigate network connections
<p></p>
Now that we know what we are looking for we can begine our analysis.
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
The first thing we will do is analyse the network connectivity searching for the game and IP address' to do this we will use the netscan option as connection and connscan wont work as the system is running Win7SP1x64. The command will look like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 netscan
```

<p></p>
Which gives us the output:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 netscan                                                                                                                               
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x7d60f010         UDPv4    0.0.0.0:1900                   *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:17 UTC+0000                                         
0x7d62b3f0         UDPv4    192.168.202.131:6771           *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:22 UTC+0000                                         
0x7d62f4c0         UDPv4    127.0.0.1:62307                *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:17 UTC+0000                                         
0x7d62f920         UDPv4    192.168.202.131:62306          *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:17 UTC+0000                                         
0x7d6424c0         UDPv4    0.0.0.0:50762                  *:*                                   4076     chrome.exe     2018-08-04 19:33:37 UTC+0000                                         
0x7d6b4250         UDPv6    ::1:1900                       *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000                                         
0x7d6e3230         UDPv4    127.0.0.1:6771                 *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:22 UTC+0000                                         
0x7d6ed650         UDPv4    0.0.0.0:5355                   *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d71c8a0         UDPv4    0.0.0.0:0                      *:*                                   868      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d71c8a0         UDPv6    :::0                           *:*                                   868      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d74a390         UDPv4    127.0.0.1:52847                *:*                                   2624     bittorrentie.e 2018-08-04 19:27:24 UTC+0000                                         
0x7d7602c0         UDPv4    127.0.0.1:52846                *:*                                   2308     bittorrentie.e 2018-08-04 19:27:24 UTC+0000                                         
0x7d787010         UDPv4    0.0.0.0:65452                  *:*                                   4076     chrome.exe     2018-08-04 19:33:42 UTC+0000                                         
0x7d789b50         UDPv4    0.0.0.0:50523                  *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d789b50         UDPv6    :::50523                       *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d92a230         UDPv4    0.0.0.0:0                      *:*                                   868      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d92a230         UDPv6    :::0                           *:*                                   868      svchost.exe    2018-08-04 19:34:22 UTC+0000                                         
0x7d9e8b50         UDPv4    0.0.0.0:20830                  *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:15 UTC+0000                                         
0x7d9f4560         UDPv4    0.0.0.0:0                      *:*                                   3856     WebCompanion.e 2018-08-04 19:34:22 UTC+0000                                         
0x7d9f8cb0         UDPv4    0.0.0.0:20830                  *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:15 UTC+0000
0x7d9f8cb0         UDPv6    :::20830                       *:*                                   2836     BitTorrent.exe 2018-08-04 19:27:15 UTC+0000
0x7d8bb390         TCPv4    0.0.0.0:9008                   0.0.0.0:0            LISTENING        4        System         
0x7d8bb390         TCPv6    :::9008                        :::0                 LISTENING        4        System         
0x7d9a9240         TCPv4    0.0.0.0:8733                   0.0.0.0:0            LISTENING        4        System         
0x7d9a9240         TCPv6    :::8733                        :::0                 LISTENING        4        System         
0x7d9e19e0         TCPv4    0.0.0.0:20830                  0.0.0.0:0            LISTENING        2836     BitTorrent.exe 
0x7d9e19e0         TCPv6    :::20830                       :::0                 LISTENING        2836     BitTorrent.exe 
0x7d9e1c90         TCPv4    0.0.0.0:20830                  0.0.0.0:0            LISTENING        2836     BitTorrent.exe 
0x7d42ba90         TCPv4    -:0                            56.219.196.26:0      CLOSED           2836     BitTorrent.exe 
0x7d6124d0         TCPv4    192.168.202.131:49530          77.102.199.102:7575  CLOSED           708      LunarMS.exe    
0x7d62d690         TCPv4    192.168.202.131:49229          169.1.143.215:8999   CLOSED           2836     BitTorrent.exe 
0x7d634350         TCPv6    -:0                            38db:c41a:80fa:ffff:38db:c41a:80fa:ffff:0 CLOSED           2836     BitTorrent.exe 
0x7d6f27f0         TCPv4    192.168.202.131:50381          71.198.155.180:34674 CLOSED           2836     BitTorrent.exe 
0x7d704010         TCPv4    192.168.202.131:50382          92.251.23.204:6881   CLOSED           2836     BitTorrent.exe 
0x7d708cf0         TCPv4    192.168.202.131:50364          91.140.89.116:31847  CLOSED           2836     BitTorrent.exe 
0x7d729620         TCPv4    -:50034                        142.129.37.27:24578  CLOSED           2836     BitTorrent.exe 
0x7d72cbe0         TCPv4    192.168.202.131:50340          23.37.43.27:80       CLOSED           3496     Lavasoft.WCAss 
0x7d7365a0         TCPv4    192.168.202.131:50358          23.37.43.27:80       CLOSED           3856     WebCompanion.e 
0x7d81c890         TCPv4    192.168.202.131:50335          185.154.111.20:60405 CLOSED           2836     BitTorrent.exe 
0x7d8fd530         TCPv4    192.168.202.131:50327          23.37.43.27:80       CLOSED           3496     Lavasoft.WCAss 
0x7d9cecf0         TCPv4    192.168.202.131:50373          173.239.232.46:2997  CLOSED           2836     BitTorrent.exe 
0x7d9d7cf0         TCPv4    192.168.202.131:50371          191.253.122.149:59163 CLOSED           2836     BitTorrent.exe 
0x7daefec0         UDPv4    0.0.0.0:0                      *:*                                   3856     WebCompanion.e 2018-08-04 19:34:22 UTC+0000
0x7daefec0         UDPv6    :::0                           *:*                                   3856     WebCompanion.e 2018-08-04 19:34:22 UTC+0000
0x7db83b90         UDPv4    0.0.0.0:0                      *:*                                   3880     WebCompanionIn 2018-08-04 19:33:30 UTC+0000
0x7db83b90         UDPv6    :::0                           *:*                                   3880     WebCompanionIn 2018-08-04 19:33:30 UTC+0000
0x7db9cdd0         UDPv4    0.0.0.0:0                      *:*                                   2844     WebCompanion.e 2018-08-04 19:30:05 UTC+0000
0x7db9cdd0         UDPv6    :::0                           *:*                                   2844     WebCompanion.e 2018-08-04 19:30:05 UTC+0000
0x7dc2dc30         UDPv4    0.0.0.0:50879                  *:*                                   4076     chrome.exe     2018-08-04 19:30:41 UTC+0000
0x7dc2dc30         UDPv6    :::50879                       *:*                                   4076     chrome.exe     2018-08-04 19:30:41 UTC+0000
0x7dc83810         UDPv4    0.0.0.0:5355                   *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7dc83810         UDPv6    :::5355                        *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7dd82c30         UDPv4    0.0.0.0:5355                   *:*                                   620      svchost.exe    2018-08-04 19:26:38 UTC+0000
0x7df00980         UDPv4    0.0.0.0:0                      *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7df00980         UDPv6    :::0                           *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7df04cc0         UDPv4    0.0.0.0:5355                   *:*                                   620      svchost.exe    2018-08-04 19:26:38 UTC+0000
0x7df04cc0         UDPv6    :::5355                        *:*                                   620      svchost.exe    2018-08-04 19:26:38 UTC+0000
0x7df5f010         UDPv4    0.0.0.0:55175                  *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7dfab010         UDPv4    0.0.0.0:58383                  *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7dfab010         UDPv6    :::58383                       *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7e12c1c0         UDPv4    0.0.0.0:0                      *:*                                   3880     WebCompanionIn 2018-08-04 19:33:27 UTC+0000
0x7e163a40         UDPv4    0.0.0.0:0                      *:*                                   3880     WebCompanionIn 2018-08-04 19:33:27 UTC+0000
0x7e163a40         UDPv6    :::0                           *:*                                   3880     WebCompanionIn 2018-08-04 19:33:27 UTC+0000
0x7e1cf010         UDPv4    192.168.202.131:137            *:*                                   4        System         2018-08-04 19:26:35 UTC+0000
0x7e1da010         UDPv4    192.168.202.131:138            *:*                                   4        System         2018-08-04 19:26:35 UTC+0000
0x7dc4ad30         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        500      lsass.exe      
0x7dc4ad30         TCPv6    :::49155                       :::0                 LISTENING        500      lsass.exe      
0x7dc4b370         TCPv4    0.0.0.0:49155                  0.0.0.0:0            LISTENING        500      lsass.exe      
0x7dd71010         TCPv4    0.0.0.0:445                    0.0.0.0:0            LISTENING        4        System         
0x7dd71010         TCPv6    :::445                         :::0                 LISTENING        4        System         
0x7ddca6b0         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        492      services.exe   
0x7ddcbc00         TCPv4    0.0.0.0:49156                  0.0.0.0:0            LISTENING        492      services.exe   
0x7ddcbc00         TCPv6    :::49156                       :::0                 LISTENING        492      services.exe   
0x7de09c30         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        396      wininit.exe    
0x7de09c30         TCPv6    :::49152                       :::0                 LISTENING        396      wininit.exe    
0x7de0d7b0         TCPv4    0.0.0.0:49152                  0.0.0.0:0            LISTENING        396      wininit.exe    
0x7de424e0         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        808      svchost.exe    
0x7de45ef0         TCPv4    0.0.0.0:49153                  0.0.0.0:0            LISTENING        808      svchost.exe    
0x7de45ef0         TCPv6    :::49153                       :::0                 LISTENING        808      svchost.exe    
0x7df3d270         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        868      svchost.exe    
0x7df3eef0         TCPv4    0.0.0.0:49154                  0.0.0.0:0            LISTENING        868      svchost.exe    
0x7df3eef0         TCPv6    :::49154                       :::0                 LISTENING        868      svchost.exe    
0x7e1f6010         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        712      svchost.exe    
0x7e1f6010         TCPv6    :::135                         :::0                 LISTENING        712      svchost.exe    
0x7e1f8ef0         TCPv4    0.0.0.0:135                    0.0.0.0:0            LISTENING        712      svchost.exe    
0x7db000a0         TCPv4    -:50091                        93.142.197.107:32645 CLOSED           2836     BitTorrent.exe 
0x7db132e0         TCPv4    192.168.202.131:50280          72.55.154.81:80      CLOSED           3880     WebCompanionIn 
0x7dbc3010         TCPv6    -:0                            4847:d418:80fa:ffff:4847:d418:80fa:ffff:0 CLOSED           4076     chrome.exe     
0x7dc4bcf0         TCPv4    -:0                            104.240.179.26:0     CLOSED           3        ?4????       
0x7dc83080         TCPv4    192.168.202.131:50377          179.108.238.10:19761 CLOSED           2836     BitTorrent.exe 
0x7dd451f0         TCPv4    192.168.202.131:50321          45.27.208.145:51414  CLOSED           2836     BitTorrent.exe 
0x7ddae890         TCPv4    -:50299                        212.92.105.227:8999  CLOSED           2836     BitTorrent.exe 
0x7ddff010         TCPv4    192.168.202.131:50379          23.37.43.27:80       CLOSED           3856     WebCompanion.e 
0x7e0057d0         TCPv4    192.168.202.131:50353          85.242.139.158:51413 CLOSED           2836     BitTorrent.exe 
0x7e0114b0         TCPv4    192.168.202.131:50339          77.65.111.216:8306   CLOSED           2836     BitTorrent.exe 
0x7e042cf0         TCPv4    192.168.202.131:50372          83.44.27.35:52103    CLOSED           2836     BitTorrent.exe 
0x7e08a010         TCPv4    192.168.202.131:50374          89.46.49.163:20133   CLOSED           2836     BitTorrent.exe 
0x7e092010         TCPv4    192.168.202.131:50378          120.29.114.41:13155  CLOSED           2836     BitTorrent.exe 
0x7e094b90         TCPv4    192.168.202.131:50365          52.91.1.182:55125    CLOSED           2836     BitTorrent.exe 
0x7e09ba90         TCPv6    -:0                            68f0:181b:80fa:ffff:68f0:181b:80fa:ffff:0 CLOSED           2836     BitTorrent.exe 
0x7e0a8b90         TCPv4    192.168.202.131:50341          72.55.154.81:80      CLOSED           3880     WebCompanionIn 
0x7e0d6180         TCPv4    192.168.202.131:50349          196.250.217.22:32815 CLOSED           2836     BitTorrent.exe 
0x7e108100         TCPv4    192.168.202.131:50360          174.0.234.77:31240   CLOSED           2836     BitTorrent.exe 
0x7e124910         TCPv4    192.168.202.131:50366          89.78.106.196:51413  CLOSED           2836     BitTorrent.exe 
0x7e14dcf0         TCPv4    192.168.202.131:50363          122.62.218.159:11627 CLOSED           2836     BitTorrent.exe 
0x7e18bcf0         TCPv4    192.168.202.131:50333          191.177.124.34:21011 CLOSED           2836     BitTorrent.exe 
0x7e1f7ab0         TCPv4    -:0                            56.187.190.26:0      CLOSED           3        ?4????       
0x7e48d9c0         UDPv6    fe80::b06b:a531:ec88:457f:1900 *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
0x7e4ad870         UDPv4    127.0.0.1:1900                 *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
0x7e511bb0         UDPv4    0.0.0.0:60005                  *:*                                   620      svchost.exe    2018-08-04 19:34:22 UTC+0000
0x7e5dc3b0         UDPv6    fe80::b06b:a531:ec88:457f:546  *:*                                   808      svchost.exe    2018-08-04 19:33:28 UTC+0000
0x7e7469c0         UDPv4    0.0.0.0:50878                  *:*                                   4076     chrome.exe     2018-08-04 19:30:39 UTC+0000
0x7e7469c0         UDPv6    :::50878                       *:*                                   4076     chrome.exe     2018-08-04 19:30:39 UTC+0000
0x7e77cb00         UDPv4    0.0.0.0:50748                  *:*                                   4076     chrome.exe     2018-08-04 19:30:07 UTC+0000
0x7e77cb00         UDPv6    :::50748                       *:*                                   4076     chrome.exe     2018-08-04 19:30:07 UTC+0000
0x7e79f3f0         UDPv4    0.0.0.0:5353                   *:*                                   4076     chrome.exe     2018-08-04 19:29:35 UTC+0000
0x7e7a0ec0         UDPv4    0.0.0.0:5353                   *:*                                   4076     chrome.exe     2018-08-04 19:29:35 UTC+0000
0x7e7a0ec0         UDPv6    :::5353                        *:*                                   4076     chrome.exe     2018-08-04 19:29:35 UTC+0000
0x7e7a3960         UDPv4    0.0.0.0:0                      *:*                                   3880     WebCompanionIn 2018-08-04 19:33:30 UTC+0000
0x7e7dd010         UDPv6    ::1:58340                      *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
0x7e413a40         TCPv4    -:0                            -:0                  CLOSED           708      LunarMS.exe    
0x7e415010         TCPv4    192.168.202.131:50346          89.64.10.176:10589   CLOSED           2836     BitTorrent.exe 
0x7e4202d0         TCPv4    192.168.202.131:50217          104.18.21.226:80     CLOSED           3880     WebCompanionIn 
0x7e45f110         TCPv4    192.168.202.131:50211          104.18.20.226:80     CLOSED           3880     WebCompanionIn 
0x7e4cc910         TCPv4    192.168.202.131:50228          104.18.20.226:80     CLOSED           3880     WebCompanionIn 
0x7e512950         TCPv4    192.168.202.131:50345          77.126.30.221:13905  CLOSED           2836     BitTorrent.exe 
0x7e521b50         TCPv4    -:0                            -:0                  CLOSED           708      LunarMS.exe    
0x7e5228d0         TCPv4    192.168.202.131:50075          70.65.116.120:52700  CLOSED           2836     BitTorrent.exe 
0x7e52f010         TCPv4    192.168.202.131:50343          86.121.4.189:46392   CLOSED           2836     BitTorrent.exe 
0x7e563860         TCPv4    192.168.202.131:50170          103.232.25.44:25384  CLOSED           2836     BitTorrent.exe 
0x7e572cf0         TCPv4    192.168.202.131:50125          122.62.218.159:11627 CLOSED           2836     BitTorrent.exe 
0x7e5d6cf0         TCPv4    192.168.202.131:50324          54.197.8.177:49420   CLOSED           2836     BitTorrent.exe 
0x7e71b010         TCPv4    192.168.202.131:50344          70.27.98.75:6881     CLOSED           2836     BitTorrent.exe 
0x7e71d010         TCPv4    192.168.202.131:50351          99.251.199.160:1045  CLOSED           2836     BitTorrent.exe 
0x7e74b010         TCPv4    192.168.202.131:50385          209.236.6.89:56500   CLOSED           2836     BitTorrent.exe 
0x7e78b7f0         TCPv4    192.168.202.131:50238          72.55.154.82:80      CLOSED           3880     WebCompanionIn 
0x7e7ae380         TCPv4    192.168.202.131:50361          5.34.21.181:8999     CLOSED           2836     BitTorrent.exe 
0x7e7b0380         TCPv6    -:0                            4847:d418:80fa:ffff:4847:d418:80fa:ffff:0 CLOSED           2836     BitTorrent.exe 
0x7e7b9010         TCPv4    192.168.202.131:50334          188.129.94.129:25128 CLOSED           2836     BitTorrent.exe 
0x7e94b010         TCPv4    192.168.202.131:50356          77.126.30.221:13905  CLOSED           2836     BitTorrent.exe 
0x7e9ad840         TCPv4    192.168.202.131:50380          84.52.144.29:56299   CLOSED           2836     BitTorrent.exe 
0x7e9bacf0         TCPv4    192.168.202.131:50350          77.253.242.0:5000    CLOSED           2836     BitTorrent.exe 
0x7eaac5e0         TCPv4    192.168.202.131:50387          93.184.220.29:80     CLOSED           3856     WebCompanion.e 
0x7eab4cf0         TCPv4    -:0                            56.219.196.26:0      CLOSED           2836     BitTorrent.exe 
0x7fb9cec0         UDPv4    192.168.202.131:1900           *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
0x7fb9d430         UDPv4    127.0.0.1:58341                *:*                                   164      svchost.exe    2018-08-04 19:28:42 UTC+0000
```

<p></p>
Looking through the output the only application that sticks out is LunarMS.exe and if we google that we find out that it is infact a game.
<p></p>

```
Offset(P)          Proto    Local Address                  Foreign Address      State            Pid      Owner          Created                                                              
0x7d6124d0         TCPv4    192.168.202.131:49530          77.102.199.102:7575  CLOSED           708      LunarMS.exe    
```

<p></p>
Giving us our answer:
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{LunarMS}
<br>
CTF{77.102.199.102}
</details>
</details>
</details>
</details>
</details>

<p></p>
<hr>
<p></p>


<details>
    <summary>Name Game</summary>
<p></p>
The fourth challenge we are given is:
<p></p>
We know that the account was logged in to a channel called Lunar-3. what is the account name?
<p></p>
format: CTF{flag}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
First we will locate the LunarMS.exe process, to do this we will use pstree however you can use pstree or any other process listing option. The command looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 pstree 
```

<p></p>
Which gives the following output:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 pstree                                                                                                                                
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
 0xfffffa801b27e060:explorer.exe                     2728   2696     33    854 2018-08-04 19:27:04 UTC+0000                                                                                   
. 0xfffffa801b486b30:Rick And Morty                  3820   2728      4    185 2018-08-04 19:32:55 UTC+0000                                                                                   
.. 0xfffffa801a4c5b30:vmware-tray.ex                 3720   3820      8    147 2018-08-04 19:33:02 UTC+0000                                                                                   
. 0xfffffa801b2f02e0:WebCompanion.e                  2844   2728      0 ------ 2018-08-04 19:27:07 UTC+0000                                                                                   
. 0xfffffa801a4e3870:chrome.exe                      4076   2728     44   1160 2018-08-04 19:29:30 UTC+0000                                                                                   
.. 0xfffffa801a4eab30:chrome.exe                     4084   4076      8     86 2018-08-04 19:29:30 UTC+0000                                                                                   
.. 0xfffffa801a5ef1f0:chrome.exe                     1796   4076     15    170 2018-08-04 19:33:41 UTC+0000                                                                                   
.. 0xfffffa801aa00a90:chrome.exe                     3924   4076     16    228 2018-08-04 19:29:51 UTC+0000                                                                                   
.. 0xfffffa801a635240:chrome.exe                     3648   4076     16    207 2018-08-04 19:33:38 UTC+0000                                                                                   
.. 0xfffffa801a502b30:chrome.exe                      576   4076      2     58 2018-08-04 19:29:31 UTC+0000                                                                                   
.. 0xfffffa801a4f7b30:chrome.exe                     1808   4076     13    229 2018-08-04 19:29:32 UTC+0000                                                                                   
.. 0xfffffa801a7f98f0:chrome.exe                     2748   4076     15    181 2018-08-04 19:31:15 UTC+0000                                                                                   
. 0xfffffa801b5cb740:LunarMS.exe                      708   2728     18    346 2018-08-04 19:27:39 UTC+0000                                                                                   
. 0xfffffa801b1cdb30:vmtoolsd.exe                    2804   2728      6    190 2018-08-04 19:27:06 UTC+0000                                                                                   
. 0xfffffa801b290b30:BitTorrent.exe                  2836   2728     24    471 2018-08-04 19:27:07 UTC+0000                                                                                   
.. 0xfffffa801b4c9b30:bittorrentie.e                 2624   2836     13    316 2018-08-04 19:27:21 UTC+0000                                                                                   
.. 0xfffffa801b4a7b30:bittorrentie.e                 2308   2836     15    337 2018-08-04 19:27:19 UTC+0000                                                                                   
 0xfffffa8018d44740:System                              4      0     95    411 2018-08-04 19:26:03 UTC+0000                                                                                   
. 0xfffffa801947e4d0:smss.exe                         260      4      2     30 2018-08-04 19:26:03 UTC+0000                                                                                   
 0xfffffa801a2ed060:wininit.exe                       396    336      3     78 2018-08-04 19:26:11 UTC+0000                                                                                   
. 0xfffffa801ab377c0:services.exe                     492    396     11    242 2018-08-04 19:26:12 UTC+0000                                                                                   
.. 0xfffffa801afe7800:svchost.exe                    1948    492      6     96 2018-08-04 19:26:42 UTC+0000                                                                                   
.. 0xfffffa801ae92920:vmtoolsd.exe                   1428    492      9    313 2018-08-04 19:26:27 UTC+0000                                                                                   
... 0xfffffa801a572b30:cmd.exe                       3916   1428      0 ------ 2018-08-04 19:34:22 UTC+0000                                                                                   
.. 0xfffffa801ae0f630:VGAuthService.                 1356    492      3     85 2018-08-04 19:26:25 UTC+0000                                                                                   
.. 0xfffffa801abbdb30:vmacthlp.exe                    668    492      3     56 2018-08-04 19:26:16 UTC+0000                                                                                   
.. 0xfffffa801aad1060:Lavasoft.WCAss                 3496    492     14    473 2018-08-04 19:33:49 UTC+0000
.. 0xfffffa801a6af9f0:svchost.exe                     164    492     12    147 2018-08-04 19:28:42 UTC+0000
.. 0xfffffa801ac2e9e0:svchost.exe                     808    492     22    508 2018-08-04 19:26:18 UTC+0000
... 0xfffffa801ac753a0:audiodg.exe                    960    808      7    151 2018-08-04 19:26:19 UTC+0000
.. 0xfffffa801ae7f630:dllhost.exe                    1324    492     15    207 2018-08-04 19:26:42 UTC+0000
.. 0xfffffa801a6c2700:mscorsvw.exe                   3124    492      7     77 2018-08-04 19:28:43 UTC+0000
.. 0xfffffa801b232060:sppsvc.exe                     2500    492      4    149 2018-08-04 19:26:58 UTC+0000
.. 0xfffffa801abebb30:svchost.exe                     712    492      8    301 2018-08-04 19:26:17 UTC+0000
.. 0xfffffa801ad718a0:svchost.exe                    1164    492     18    312 2018-08-04 19:26:23 UTC+0000
.. 0xfffffa801ac31b30:svchost.exe                     844    492     17    396 2018-08-04 19:26:18 UTC+0000
... 0xfffffa801b1fab30:dwm.exe                       2704    844      4     97 2018-08-04 19:27:04 UTC+0000
.. 0xfffffa801988c2d0:PresentationFo                  724    492      6    148 2018-08-04 19:27:52 UTC+0000
.. 0xfffffa801b603610:mscorsvw.exe                    412    492      7     86 2018-08-04 19:28:42 UTC+0000
.. 0xfffffa8018e3c890:svchost.exe                     604    492     11    376 2018-08-04 19:26:16 UTC+0000
... 0xfffffa8019124b30:WmiPrvSE.exe                  1800    604      9    222 2018-08-04 19:26:39 UTC+0000
... 0xfffffa801b112060:WmiPrvSE.exe                  2136    604     12    324 2018-08-04 19:26:51 UTC+0000
.. 0xfffffa801ad5ab30:spoolsv.exe                    1120    492     14    346 2018-08-04 19:26:22 UTC+0000
.. 0xfffffa801ac4db30:svchost.exe                     868    492     45   1114 2018-08-04 19:26:18 UTC+0000
.. 0xfffffa801a6e4b30:svchost.exe                    3196    492     14    352 2018-08-04 19:28:44 UTC+0000
.. 0xfffffa801acd37e0:svchost.exe                     620    492     19    415 2018-08-04 19:26:21 UTC+0000
.. 0xfffffa801b1e9b30:taskhost.exe                   2344    492      8    193 2018-08-04 19:26:57 UTC+0000
.. 0xfffffa801ac97060:svchost.exe                    1012    492     12    554 2018-08-04 19:26:20 UTC+0000
.. 0xfffffa801b3aab30:SearchIndexer.                 3064    492     11    610 2018-08-04 19:27:14 UTC+0000
.. 0xfffffa801aff3b30:msdtc.exe                      1436    492     14    155 2018-08-04 19:26:43 UTC+0000
. 0xfffffa801ab3f060:lsass.exe                        500    396      7    610 2018-08-04 19:26:12 UTC+0000
. 0xfffffa801ab461a0:lsm.exe                          508    396     10    148 2018-08-04 19:26:12 UTC+0000
 0xfffffa801a0c8380:csrss.exe                         348    336      9    563 2018-08-04 19:26:10 UTC+0000
. 0xfffffa801a6643d0:conhost.exe                     2420    348      0     30 2018-08-04 19:34:22 UTC+0000
 0xfffffa80198d3b30:csrss.exe                         388    380     11    460 2018-08-04 19:26:11 UTC+0000
 0xfffffa801aaf4060:winlogon.exe                      432    380      3    113 2018-08-04 19:26:11 UTC+0000
 0xfffffa801b18f060:WebCompanionIn                   3880   1484     15    522 2018-08-04 19:33:07 UTC+0000
. 0xfffffa801aa72b30:sc.exe                          3504   3880      0 ------ 2018-08-04 19:33:48 UTC+0000
. 0xfffffa801aeb6890:sc.exe                           452   3880      0 ------ 2018-08-04 19:33:48 UTC+0000
. 0xfffffa801a6268b0:WebCompanion.e                  3856   3880     15    386 2018-08-04 19:34:05 UTC+0000
. 0xfffffa801b08f060:sc.exe                          3208   3880      0 ------ 2018-08-04 19:33:47 UTC+0000
. 0xfffffa801ac01060:sc.exe                          2028   3880      0 ------ 2018-08-04 19:33:49 UTC+0000
 0xfffffa801b1fd960:notepad.exe                      3304   3132      2     79 2018-08-04 19:34:10 UTC+0000
```

<p></p>
We can see LunarMS.exe in the output.
<p></p>

```
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
. 0xfffffa801b5cb740:LunarMS.exe                      708   2728     18    346 2018-08-04 19:27:39 UTC+0000                                                                                   
```

<p></p>
The important information we need is the Pid so that we can dump the memory of the process (I attempted finding the LunarMS.exe files and searching within the actual files for the information we need however it didn't return anything, hence we will dump the process memory itself). Now that we have the PID we can do a <kbd>memdump</kbd> IOT analyse the process further. The command looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 memdump -p 708 -D . 
```

<p></p>
With this command the <kbd>-p</kbd> flag is pointing to the PID, the <kbd>-D</kbd> flag is pointing to the current working directory '.' to dump the files, however you can specify any directory to dump the file.
<p></p>
This gives a fairly basic output.
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 memdump -p 708 -D .
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing LunarMS.exe [   708] to 708.dmp
```

<p></p>
But it gives us a file that we can begin analysing. From here we will run strings (to print the ASCII strings) and grep for the information that we know Lunar-3.
<p></p>

```
❯ strings 708.dmp | grep -i lunar-3
Lunar-3
Lunar-3
```

<p></p>
That showes that Lunar-3 is within the file however it is only printing the single line so we need to add some flags to <kbd>grep</kbd> IOT see the lines following Lunar-3. To do this we use the <kbd>-C</kbd> specifying '4' as the amount of lines following the found 'Lunar-3'
<p></p>

```
❯ strings 708.dmp | grep Lunar-3 -C 4
\b+Y
c+Yt
tb+Y4c+Y
b+YLc+Y
Lunar-3
Lunar-4
L(dNVxdNV
L|eNV
{qf8
--
pressed
disabled
mouseOver
keyFocused
Lunar-3
0tt3r8r33z3
Sound/UI.img/
BtMouseClick
Lunar-4
```

<p></p>
Here we can see the user name straight after Lunar-3
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{0tt3r8r33z3}
<p></p>
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Name Game 2</summary>
<p></p>
The fifth challenge we are given is:
<p></p>
From a little research we found that the username of the logged on character is always after this signature: 0x64 0x??{6-8} 0x40 0x06 0x??{18} 0x5a 0x0c 0x00{2} What's rick's character's name? 
<p></p>
format: CTF{...}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This challenge is a bit more obscure and there is a slow and fast way to go about it. The first thing we need to recognise is that the "signature" we are given is hex. This is our big clue on how we much progress. Now the first way you can go about this is by using <kbd>hexedit</kbd> and manually search through the file looking for something that resembles a username (takes forever!) the second and quicker/less painfull way is to utilise <kbd>xxd</kbd> and <kbd>grep</kbd> using the last part of the signature we are given. We will look at the <kbd>xxd</kbd> method now. 
<p></p>
First the final part of the signature looks like this "5a0c 0000" this is what we will grep for.
<p></p>
The command we will use is:
<p></p>

```
xxd 708.dmp | grep "5a0c 0000"
```

<p></p>
Which outputs:
<p></p>

```
❯ xxd 708.dmp | grep "5a0c 0000"                                                                                                                                                              
003ef0a0: 5a0c 0000 684e fe41 00e8 00b6 2900 59c3  Z...hN.A....).Y.                                                                                                                           
0062fcc0: 5a0c 0000 0000 0000 63ae 0200 0000 0000  Z.......c.......                                                                                                                           
01147d10: 880c 0000 5a0c 0000 4c00 0000 4d00 0000  ....Z...L...M...                                                                                                                           
01148d50: b30b 0000 b30b 0000 5a0c 0000 a50c 0000  ........Z.......                                                                                                                           
01479d80: 0000 ffff 00ff 0d09 5a0c 0000 1b2c 0000  ........Z....,..                                                                                                                           
0210f4e0: 0704 0000 0000 ffff 00ff 0d09 5a0c 0000  ............Z...                                                                                                                           
084ae150: 5a0c 0000 04f8 3e1f 1000 0000 0035 c150  Z.....>......5.P                                                                                                                           
0871dd10: 5a0c 0000 2000 0000 0000 0000 a033 f949  Z... ........3.I                                                                                                                           
08751740: 1000 0000 0035 c150 0000 0000 5a0c 0000  .....5.P....Z...                                                                                                                           
0ac724c0: 1000 0000 0035 c150 0000 0000 5a0c 0000  .....5.P....Z...                                                                                                                           
0b04a330: 5a0c 0000 700f 0320 1000 0000 0035 c150  Z...p.. .....5.P                                                                                                                           
0b04ac60: 0000 0000 5a0c 0000 64c5 221e 1000 0000  ....Z...d.".....                                                                                                                           
0c33a4a0: 9a23 3223 0b00 0001 5a0c 0000 4d30 7274  .#2#....Z...M0rt                                                                                                                           
0e85cba0: 1000 0000 0035 c150 0000 0000 5a0c 0000  .....5.P....Z...                                                                                                                           
0ecfa5c0: 0035 c150 0000 0000 5a0c 0000 b460 9047  .5.P....Z....`.G                                                                                                                           
0ecfad80: 0000 0000 5a0c 0000 40f3 9047 1000 0000  ....Z...@..G....                                                                                                                           
0ed06370: 1000 0000 0035 c150 b033 b046 5a0c 0000  .....5.P.3.FZ...                                                                                                                           
0ed25c60: 1000 0000 0035 c150 0000 0000 5a0c 0000  .....5.P....Z...                                                                                                                           
107ee770: 1000 0000 0035 c150 0000 0000 5a0c 0000  .....5.P....Z...                                                                                                                           
109397e0: 5a0c 0000 3100 3000 3500 3200 3000 3000  Z...1.0.5.2.0.0.                                                                                                                           
10b3ca20: 5a0c 0000 2000 0000 0000 0000 40a0 cd49  Z... .......@..I                                                                                                                           
10f27d50: 0000 0000 5a0c 0000 50a9 df48 1000 0000  ....Z...P..H....                                                                                                                           
111e3e60: 5a0c 0000 6d00 6100 7000 2f00 6f00 6200  Z...m.a.p./.o.b.
122f9a00: 5a0c 0000 5300 7400 7200 6900 6e00 6700  Z...S.t.r.i.n.g.
12cf83d0: 5a0c 0000 5300 6800 6100 7000 6500 3200  Z...S.h.a.p.e.2.
12d183d0: 5a0c 0000 4500 7400 6300 2f00 4300 6f00  Z...E.t.c./.C.o.
12d383d0: 5a0c 0000 5300 6800 6100 7000 6500 3200  Z...S.h.a.p.e.2.
12d786f0: 5a0c 0000 4500 7400 6300 2f00 4300 6f00  Z...E.t.c./.C.o.
12f38650: 5a0c 0000 7400 7200 6100 6400 6500 4100  Z...t.r.a.d.e.A.
131582d0: 5a0c 0000 4500 7400 6300 2f00 4300 6f00  Z...E.t.c./.C.o.
133c9870: 174c c31e 0000 0080 5a0c 0000 4500 7400  .L......Z...E.t.
20b05fc0: b0e5 af00 5a0c 0000 4d30 7274 794c 304c  ....Z...M0rtyL0L
21059040: 1800 0000 0000 0000 a8e5 985a 5a0c 0000  ...........ZZ...
212437c0: 6918 9553 0000 0080 5a0c 0000 4500 6600  i..S....Z...E.f.
212c3500: 5a0c 0000 4500 7400 6300 2f00 4300 6f00  Z...E.t.c./.C.o.
21445530: 5a0c 0000 5300 6800 6100 7000 6500 3200  Z...S.h.a.p.e.2.
21462b30: 788c 8150 0000 0000 30cb 354d 5a0c 0000  x..P....0.5MZ...
21549530: d730 9b53 0000 0080 5a0c 0000 5300 6800  .0.S....Z...S.h.
23f3a1a0: 5a0c 0000 3900 3900 3000 3100 3100 3100  Z...9.9.0.1.1.1.
240161c0: 5a0c 0000 6900 6e00 6300 5300 5400 5200  Z...i.n.c.S.T.R.
244ea4f0: 5a0c 0000 2000 0000 0000 0000 408d 0c5a  Z... .......@..Z
25539020: 10e8 5a0c 0000 e947 edff ffff 75cc 56ff  ..Z....G....u.V.
25d7e450: 6359 5a0c 0000 0080 2161 720d 0000 0080  cYZ.....!ar.....
25d7e690: 63b9 5a0c 0000 60e6 63c9 8e0c 0000 50e6  c.Z...`.c.....P.
25d7f050: 6319 5a0c 0000 f0e0 6329 b20c 0000 00e1  c.Z.....c)......
25f93930: 5a0c 0000 da0e 0200 0000 0000 0000 0000  Z...............
2891d760: 379c 5a0c 0000 0000 0000 0000 0000 0000  7.Z.............
29238210: 4489 b424 540c 0000 66c7 8424 5a0c 0000  D..$T...f..$Z...
2a01c5f0: 580c 0000 2e06 0000 5a0c 0000 2f06 0000  X.......Z.../...
2dafe010: 0207 2801 480c 0001 5a0c 0000 0000 0100  ..(.H...Z.......
2db054f0: 5a0c 0000 0000 0100 0000 0000 2003 0000  Z........... ...
2edc74e0: 8003 0000 60b0 4f20 5a0c 0000 0000 0000  ....`.O Z.......
2ee233d0: 0d01 0000 5a0c 0000 0100 0301 0000 0000  ....Z...........
2eef6b80: 5a0c 0000 0000 0000 0000 0000 0000 0000  Z...............
2f846960: 404b 4801 a0f8 ffff 5a0c 0000 0000 0040  @KH.....Z......@
2f849ee0: 404b 4801 a0f8 ffff 5a0c 0000 0000 0040  @KH.....Z......@
2f84d190: 404b 4801 a0f8 ffff 5a0c 0000 0000 0040  @KH.....Z......@
2f886ad0: b902 0000 0000 0000 5a0c 0000 0000 0040  ........Z......@
2f89c410: 40b1 6b01 a0f8 ffff 5a0c 0000 0000 0040  @.k.....Z......@
2f8d8790: 40b1 6b01 a0f8 ffff 5a0c 0000 0000 0040  @.k.....Z......@
2f8fcd50: 4021 aa01 a0f8 ffff 5a0c 0000 0000 0040  @!......Z......@
2f9285b0: 70cc 6b01 a0f8 ffff 5a0c 0000 0000 0040  p.k.....Z......@
2f9517a0: 40b1 6b01 a0f8 ffff 5a0c 0000 0000 0040  @.k.....Z......@
2f959080: 7065 8703 a0f8 ffff 5a0c 0000 0000 0040  pe......Z......@
2f9ebdd0: 7065 8703 a0f8 ffff 5a0c 0000 0000 0040  pe......Z......@
2fa0abe0: 7065 8703 a0f8 ffff 5a0c 0000 0000 0040  pe......Z......@
```

<p></p>
Giving us our answer:
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{M0rtyL0L}
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Silly Rick</summary>
<p></p>
The sixth challenge we are given is:
<p></p>
Silly rick always forgets his email's password, so he uses a Stored Password Services online to store his password. He always copy and paste the password so he will not get it wrong. whats rick's email password?
<p></p>
format: CTF{ANSWER}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This is a very easy challenge and only requires using one option and looking at the standard output that is the clipboard command. The command looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 clipboard
```

<p></p>
Which gives the following output:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 clipboard
Volatility Foundation Volatility Framework 2.6
Session    WindowStation Format                         Handle Object             Data                                              
---------- ------------- ------------------ ------------------ ------------------ --------------------------------------------------
         1 WinSta0       CF_UNICODETEXT                0x602e3 0xfffff900c1ad93f0 M@il_Pr0vid0rs                                    
         1 WinSta0       CF_TEXT                          0x10 ------------------                                                   
         1 WinSta0       0x150133L              0x200000000000 ------------------                                                   
         1 WinSta0       CF_TEXT                           0x1 ------------------                                                   
         1 ------------- ------------------           0x150133 0xfffff900c1c1adc0                                                   
```

<p></p>
Giving us the answer:
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{M@il_Pr0vid0rs}
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Hide and Seek</summary>
<p></p>
The seventh challenge we are given is:
<p></p>
The reason that we took rick's PC memory dump is because there was a malware infection. Please find the malware process name (including the extension)
<p></p>
BEAWARE! There are only 3 attempts to get the right flag!
<p></p>
format: CTF{flag}
<p></p>
<H2>WARNING THIS IS LIVE RANSOMEWARE</H2>
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
I have done a bit of a bigger write up on this challenge as malware forensics is a whole area in itself. I have tried to explaine the thought process and method of getting to an answer.
<p></p>
From the hint we can see that we need to find a process and we are looking for malware, so lets start with that, we will use <kbd>pstree</kbd> however any process listing option will work. The command looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 pstree
```

<p></p>
Which outputs this:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 pstree                                                                                                                                
[sudo] password for parrot: 
Volatility Foundation Volatility Framework 2.6
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
 0xfffffa801b27e060:explorer.exe                     2728   2696     33    854 2018-08-04 19:27:04 UTC+0000
. 0xfffffa801b486b30:Rick And Morty                  3820   2728      4    185 2018-08-04 19:32:55 UTC+0000
.. 0xfffffa801a4c5b30:vmware-tray.ex                 3720   3820      8    147 2018-08-04 19:33:02 UTC+0000
. 0xfffffa801b2f02e0:WebCompanion.e                  2844   2728      0 ------ 2018-08-04 19:27:07 UTC+0000
. 0xfffffa801a4e3870:chrome.exe                      4076   2728     44   1160 2018-08-04 19:29:30 UTC+0000
.. 0xfffffa801a4eab30:chrome.exe                     4084   4076      8     86 2018-08-04 19:29:30 UTC+0000
.. 0xfffffa801a5ef1f0:chrome.exe                     1796   4076     15    170 2018-08-04 19:33:41 UTC+0000
.. 0xfffffa801aa00a90:chrome.exe                     3924   4076     16    228 2018-08-04 19:29:51 UTC+0000
.. 0xfffffa801a635240:chrome.exe                     3648   4076     16    207 2018-08-04 19:33:38 UTC+0000
.. 0xfffffa801a502b30:chrome.exe                      576   4076      2     58 2018-08-04 19:29:31 UTC+0000
.. 0xfffffa801a4f7b30:chrome.exe                     1808   4076     13    229 2018-08-04 19:29:32 UTC+0000
.. 0xfffffa801a7f98f0:chrome.exe                     2748   4076     15    181 2018-08-04 19:31:15 UTC+0000
. 0xfffffa801b5cb740:LunarMS.exe                      708   2728     18    346 2018-08-04 19:27:39 UTC+0000
. 0xfffffa801b1cdb30:vmtoolsd.exe                    2804   2728      6    190 2018-08-04 19:27:06 UTC+0000
. 0xfffffa801b290b30:BitTorrent.exe                  2836   2728     24    471 2018-08-04 19:27:07 UTC+0000
.. 0xfffffa801b4c9b30:bittorrentie.e                 2624   2836     13    316 2018-08-04 19:27:21 UTC+0000
.. 0xfffffa801b4a7b30:bittorrentie.e                 2308   2836     15    337 2018-08-04 19:27:19 UTC+0000
 0xfffffa8018d44740:System                              4      0     95    411 2018-08-04 19:26:03 UTC+0000
. 0xfffffa801947e4d0:smss.exe                         260      4      2     30 2018-08-04 19:26:03 UTC+0000
 0xfffffa801a2ed060:wininit.exe                       396    336      3     78 2018-08-04 19:26:11 UTC+0000
. 0xfffffa801ab377c0:services.exe                     492    396     11    242 2018-08-04 19:26:12 UTC+0000
.. 0xfffffa801afe7800:svchost.exe                    1948    492      6     96 2018-08-04 19:26:42 UTC+0000
.. 0xfffffa801ae92920:vmtoolsd.exe                   1428    492      9    313 2018-08-04 19:26:27 UTC+0000
... 0xfffffa801a572b30:cmd.exe                       3916   1428      0 ------ 2018-08-04 19:34:22 UTC+0000
.. 0xfffffa801ae0f630:VGAuthService.                 1356    492      3     85 2018-08-04 19:26:25 UTC+0000
.. 0xfffffa801abbdb30:vmacthlp.exe                    668    492      3     56 2018-08-04 19:26:16 UTC+0000
.. 0xfffffa801aad1060:Lavasoft.WCAss                 3496    492     14    473 2018-08-04 19:33:49 UTC+0000
.. 0xfffffa801a6af9f0:svchost.exe                     164    492     12    147 2018-08-04 19:28:42 UTC+0000
.. 0xfffffa801ac2e9e0:svchost.exe                     808    492     22    508 2018-08-04 19:26:18 UTC+0000
... 0xfffffa801ac753a0:audiodg.exe                    960    808      7    151 2018-08-04 19:26:19 UTC+0000
.. 0xfffffa801ae7f630:dllhost.exe                    1324    492     15    207 2018-08-04 19:26:42 UTC+0000
.. 0xfffffa801a6c2700:mscorsvw.exe                   3124    492      7     77 2018-08-04 19:28:43 UTC+0000
.. 0xfffffa801b232060:sppsvc.exe                     2500    492      4    149 2018-08-04 19:26:58 UTC+0000
.. 0xfffffa801abebb30:svchost.exe                     712    492      8    301 2018-08-04 19:26:17 UTC+0000
.. 0xfffffa801ad718a0:svchost.exe                    1164    492     18    312 2018-08-04 19:26:23 UTC+0000
.. 0xfffffa801ac31b30:svchost.exe                     844    492     17    396 2018-08-04 19:26:18 UTC+0000
... 0xfffffa801b1fab30:dwm.exe                       2704    844      4     97 2018-08-04 19:27:04 UTC+0000
.. 0xfffffa801988c2d0:PresentationFo                  724    492      6    148 2018-08-04 19:27:52 UTC+0000
.. 0xfffffa801b603610:mscorsvw.exe                    412    492      7     86 2018-08-04 19:28:42 UTC+0000
.. 0xfffffa8018e3c890:svchost.exe                     604    492     11    376 2018-08-04 19:26:16 UTC+0000
... 0xfffffa8019124b30:WmiPrvSE.exe                  1800    604      9    222 2018-08-04 19:26:39 UTC+0000
... 0xfffffa801b112060:WmiPrvSE.exe                  2136    604     12    324 2018-08-04 19:26:51 UTC+0000
.. 0xfffffa801ad5ab30:spoolsv.exe                    1120    492     14    346 2018-08-04 19:26:22 UTC+0000
.. 0xfffffa801ac4db30:svchost.exe                     868    492     45   1114 2018-08-04 19:26:18 UTC+0000
.. 0xfffffa801a6e4b30:svchost.exe                    3196    492     14    352 2018-08-04 19:28:44 UTC+0000
.. 0xfffffa801acd37e0:svchost.exe                     620    492     19    415 2018-08-04 19:26:21 UTC+0000
.. 0xfffffa801b1e9b30:taskhost.exe                   2344    492      8    193 2018-08-04 19:26:57 UTC+0000
.. 0xfffffa801ac97060:svchost.exe                    1012    492     12    554 2018-08-04 19:26:20 UTC+0000
.. 0xfffffa801b3aab30:SearchIndexer.                 3064    492     11    610 2018-08-04 19:27:14 UTC+0000
.. 0xfffffa801aff3b30:msdtc.exe                      1436    492     14    155 2018-08-04 19:26:43 UTC+0000
. 0xfffffa801ab3f060:lsass.exe                        500    396      7    610 2018-08-04 19:26:12 UTC+0000
. 0xfffffa801ab461a0:lsm.exe                          508    396     10    148 2018-08-04 19:26:12 UTC+0000
 0xfffffa801a0c8380:csrss.exe                         348    336      9    563 2018-08-04 19:26:10 UTC+0000
. 0xfffffa801a6643d0:conhost.exe                     2420    348      0     30 2018-08-04 19:34:22 UTC+0000
 0xfffffa80198d3b30:csrss.exe                         388    380     11    460 2018-08-04 19:26:11 UTC+0000
 0xfffffa801aaf4060:winlogon.exe                      432    380      3    113 2018-08-04 19:26:11 UTC+0000
 0xfffffa801b18f060:WebCompanionIn                   3880   1484     15    522 2018-08-04 19:33:07 UTC+0000
. 0xfffffa801aa72b30:sc.exe                          3504   3880      0 ------ 2018-08-04 19:33:48 UTC+0000
. 0xfffffa801aeb6890:sc.exe                           452   3880      0 ------ 2018-08-04 19:33:48 UTC+0000
. 0xfffffa801a6268b0:WebCompanion.e                  3856   3880     15    386 2018-08-04 19:34:05 UTC+0000
. 0xfffffa801b08f060:sc.exe                          3208   3880      0 ------ 2018-08-04 19:33:47 UTC+0000
. 0xfffffa801ac01060:sc.exe                          2028   3880      0 ------ 2018-08-04 19:33:49 UTC+0000
 0xfffffa801b1fd960:notepad.exe                      3304   3132      2     79 2018-08-04 19:34:10 UTC+0000
```

<p></p>
Looking through the output the first thing we want to make sure is that only system has the PID of 4, this is a give away for malware if it has a PID of 4 as it is  default allocated to SYSTEM.
<br>
After that we need to make sure no process has the same start time as SYSTEM or wininit, csrss or winlogon as this would be another give away for malware (these are all windows initialisation process').
<br>
Since we have confirmed the above are not occuring we can begin looking at the running process' for anything that stands out as odd. My attention is drawn to one thing:
<p></p>

```
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
 0xfffffa801b27e060:explorer.exe                     2728   2696     33    854 2018-08-04 19:27:04 UTC+0000
. 0xfffffa801b486b30:Rick And Morty                  3820   2728      4    185 2018-08-04 19:32:55 UTC+0000
.. 0xfffffa801a4c5b30:vmware-tray.ex                 3720   3820      8    147 2018-08-04 19:33:02 UTC+0000
```

<p></p>
What we can see here is 'explorer' as the parent process which then spawned 'Rick And Morty' as a child process that finally spawned 'vmware-tray' as a child process (the dots to the left signify parent and child process', PPid [Parent Process ID] also shows this relationship) we can visualise this using the following commands:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 pstree --output=dot --output-file=pstree.dot
Volatility Foundation Volatility Framework 2.6
Outputting to: pstree.dot
```

<p></p>

```
dot -Tjpg pstree.dot -o pstree.jpg
```

<p></p>
Which outputs a .jpg file for us to look at mapping the relationships of process'. (I have only included the explorer,Rick And Morty,vmware-tray part of the image).
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/pstree.jpg"><br>
</div>
<p></p>
Here we can see the physical relationship between the process' and how they have spawned.
<p></p>
Ok so we have a suspect lets use the <kbd>malfind</kbd> option (this finds hidden or injected code) to see if this is running any hidden code. The command looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 malfind
```

<p></p>
Which outputs this (I have only included the vmware-tray parts as it has a large output):
<p></p>

```
Process: vmware-tray.ex Pid: 3720 Address: 0x670000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 1, PrivateMemory: 1, Protection: 6

0x00670000  9c 4c 5b ae 8c 8a 00 01 ee ff ee ff 00 00 00 00   .L[.............
0x00670010  a8 00 67 00 a8 00 67 00 00 00 67 00 00 00 67 00   ..g...g...g...g.
0x00670020  40 00 00 00 88 05 67 00 00 00 6b 00 3f 00 00 00   @.....g...k.?...
0x00670030  01 00 00 00 00 00 00 00 f0 0f 67 00 f0 0f 67 00   ..........g...g.

0x00670000 9c               PUSHF
0x00670001 4c               DEC ESP
0x00670002 5b               POP EBX
0x00670003 ae               SCASB
0x00670004 8c8a0001eeff     MOV [EDX-0x11ff00], CS
0x0067000a ee               OUT DX, AL
0x0067000b ff00             INC DWORD [EAX]
0x0067000d 0000             ADD [EAX], AL
0x0067000f 00a8006700a8     ADD [EAX-0x57ff9900], CH
0x00670015 006700           ADD [EDI+0x0], AH
0x00670018 0000             ADD [EAX], AL
0x0067001a 670000           ADD [BX+SI], AL
0x0067001d 006700           ADD [EDI+0x0], AH
0x00670020 40               INC EAX
0x00670021 0000             ADD [EAX], AL
0x00670023 008805670000     ADD [EAX+0x6705], CL
0x00670029 006b00           ADD [EBX+0x0], CH
0x0067002c 3f               AAS
0x0067002d 0000             ADD [EAX], AL
0x0067002f 0001             ADD [ECX], AL
0x00670031 0000             ADD [EAX], AL
0x00670033 0000             ADD [EAX], AL
0x00670035 0000             ADD [EAX], AL
0x00670037 00f0             ADD AL, DH
0x00670039 0f6700           PACKUSWB MM0, [EAX] 
0x0067003c f00f6700         PACKUSWB MM0, [EAX] 

Process: vmware-tray.ex Pid: 3720 Address: 0x510000                                                                                                                                           
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE                                                                                                                                              
Flags: CommitCharge: 23, PrivateMemory: 1, Protection: 6

0x00510000  f0 26 16 8e 6a 2f 00 01 ee ff ee ff 00 00 00 00   .&..j/..........
0x00510010  a8 00 51 00 a8 00 51 00 00 00 51 00 00 00 51 00   ..Q...Q...Q...Q.
0x00510020  40 00 00 00 88 05 51 00 00 00 55 00 29 00 00 00   @.....Q...U.)...
0x00510030  01 00 00 00 00 00 00 00 f0 6f 52 00 f0 6f 52 00   .........oR..oR.

0x00510000 f02616           PUSH SS
0x00510003 8e6a2f           MOV GS, [EDX+0x2f]
0x00510006 0001             ADD [ECX], AL
0x00510008 ee               OUT DX, AL
0x00510009 ff               DB 0xff
0x0051000a ee               OUT DX, AL
0x0051000b ff00             INC DWORD [EAX]
0x0051000d 0000             ADD [EAX], AL
0x0051000f 00a8005100a8     ADD [EAX-0x57ffaf00], CH
0x00510015 005100           ADD [ECX+0x0], DL
0x00510018 0000             ADD [EAX], AL
0x0051001a 51               PUSH ECX
0x0051001b 0000             ADD [EAX], AL
0x0051001d 005100           ADD [ECX+0x0], DL
0x00510020 40               INC EAX
0x00510021 0000             ADD [EAX], AL
0x00510023 008805510000     ADD [EAX+0x5105], CL
0x00510029 005500           ADD [EBP+0x0], DL
0x0051002c 2900             SUB [EAX], EAX
0x0051002e 0000             ADD [EAX], AL
0x00510030 0100             ADD [EAX], EAX
0x00510032 0000             ADD [EAX], AL
0x00510034 0000             ADD [EAX], AL
0x00510036 0000             ADD [EAX], AL
0x00510038 f06f             OUTS DX, DWORD [ESI]
0x0051003a 52               PUSH EDX
0x0051003b 00f0             ADD AL, DH
0x0051003d 6f               OUTS DX, DWORD [ESI]
0x0051003e 52               PUSH EDX
0x0051003f 00               DB 0x0

Process: vmware-tray.ex Pid: 3720 Address: 0xc00000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 1, PrivateMemory: 1, Protection: 6

0x00c00000  33 d2 3c f0 3c cf 00 01 ee ff ee ff 00 00 00 00   3.<.<...........
0x00c00010  a8 00 c0 00 a8 00 c0 00 00 00 c0 00 00 00 c0 00   ................
0x00c00020  40 00 00 00 88 05 c0 00 00 00 c4 00 3f 00 00 00   @...........?...
0x00c00030  01 00 00 00 00 00 00 00 f0 0f c0 00 f0 0f c0 00   ................

0x00c00000 33d2             XOR EDX, EDX
0x00c00002 3cf0             CMP AL, 0xf0
0x00c00004 3ccf             CMP AL, 0xcf
0x00c00006 0001             ADD [ECX], AL
0x00c00008 ee               OUT DX, AL
0x00c00009 ff               DB 0xff
0x00c0000a ee               OUT DX, AL
0x00c0000b ff00             INC DWORD [EAX]
0x00c0000d 0000             ADD [EAX], AL
0x00c0000f 00a800c000a8     ADD [EAX-0x57ff4000], CH
0x00c00015 00c0             ADD AL, AL
0x00c00017 0000             ADD [EAX], AL
0x00c00019 00c0             ADD AL, AL
0x00c0001b 0000             ADD [EAX], AL
0x00c0001d 00c0             ADD AL, AL
0x00c0001f 004000           ADD [EAX+0x0], AL
0x00c00022 0000             ADD [EAX], AL
0x00c00024 8805c0000000     MOV [0xc0], AL
0x00c0002a c400             LES EAX, [EAX]
0x00c0002c 3f               AAS
0x00c0002d 0000             ADD [EAX], AL
0x00c0002f 0001             ADD [ECX], AL
0x00c00031 0000             ADD [EAX], AL
0x00c00033 0000             ADD [EAX], AL
0x00c00035 0000             ADD [EAX], AL
0x00c00037 00f0             ADD AL, DH
0x00c00039 0fc000           XADD [EAX], AL
0x00c0003c f00fc000         LOCK XADD [EAX], AL 

Process: vmware-tray.ex Pid: 3720 Address: 0xa10000
Vad Tag: VadS Protection: PAGE_EXECUTE_READWRITE
Flags: CommitCharge: 1, PrivateMemory: 1, Protection: 6

0x00a10000  12 d2 1f 89 e6 fd 00 01 ee ff ee ff 00 00 00 00   ................
0x00a10010  a8 00 a1 00 a8 00 a1 00 00 00 a1 00 00 00 a1 00   ................
0x00a10020  40 00 00 00 88 05 a1 00 00 00 a5 00 3f 00 00 00   @...........?...
0x00a10030  01 00 00 00 00 00 00 00 f0 0f a1 00 f0 0f a1 00   ................

0x00a10000 12d2             ADC DL, DL
0x00a10002 1f               POP DS
0x00a10003 89e6             MOV ESI, ESP
0x00a10005 fd               STD
0x00a10006 0001             ADD [ECX], AL
0x00a10008 ee               OUT DX, AL
0x00a10009 ff               DB 0xff
0x00a1000a ee               OUT DX, AL
0x00a1000b ff00             INC DWORD [EAX]
0x00a1000d 0000             ADD [EAX], AL
0x00a1000f 00a800a100a8     ADD [EAX-0x57ff5f00], CH
0x00a10015 00a1000000a1     ADD [ECX-0x5f000000], AH
0x00a1001b 0000             ADD [EAX], AL
0x00a1001d 00a100400000     ADD [ECX+0x4000], AH
0x00a10023 008805a10000     ADD [EAX+0xa105], CL
0x00a10029 00a5003f0000     ADD [EBP+0x3f00], AH
0x00a1002f 0001             ADD [ECX], AL
0x00a10031 0000             ADD [EAX], AL
0x00a10033 0000             ADD [EAX], AL
0x00a10035 0000             ADD [EAX], AL
0x00a10037 00f0             ADD AL, DH
0x00a10039 0fa1             POP FS
0x00a1003b 00f0             ADD AL, DH
0x00a1003d 0fa1             POP FS
0x00a1003f 00               DB 0x0
```

<p></p>
The output we can see is called assembly language, there are some other process' that output from this command however some are system related and some are spawned from the malware we just can't prove/see that yet. So now that we know that this is doing something in the background se should definitely look at this further.
<p></p>
Now that we are honning in on our suspected file we should now dump the file and process memory to investigate further. First we will dump the process memory using the following command:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 memdump -p 3720,3820 -D .
```

<p><p>
In this command the <kbd>-p</kbd> flag points to the process ID (PID) and the <kbd>-D</kbd> flag points to the directory we want to dump the files to '.' is our current working directory.
<p></p>
We can then run <kbd>strings 3820.dmp | less</kbd> to look through the file. Bellow are the two interesting things that pop out:
<p></p>

```
"C:\Torrents\Rick And Morty season 1 download.exe"

vmware-tray.exe
```

<p></p>
So first we can see that Rick was trying to download season 1 of Rick And Morty however it's a .exe file (not a folder with videos in it) second we can see that vmware-tray.exe is mentioned within the file. Now we can look at vmware-tray.exe using this command <kbd>strings 3720.dmp | less</kbd> and this time we find some very interesting information.
<p></p>

```
C:\Users\Tyler\Desktop\hidden-tear-master\hidden-tear\hidden-tear\obj\Debug\VapeHacksLoader.pdb                                                                                               
_CorExeMain                                                                                                                                                                                   
mscoree.dll                                                                                                                                                                                   
<?xml version="1.0" encoding="utf-8"?>                                                                                                                                                        
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">                                                                                                                     
  <assemblyIdentity version="1.0.0.0" name="MyApplication.app"/>                                                                                                                              
  <trustInfo xmlns="urn:schemas-microsoft-com:asm.v2">                                                                                                                                        
    <security>                                                                                                                                                                                
      <requestedPrivileges xmlns="urn:schemas-microsoft-com:asm.v3">                                                                                                                          
        <!-- UAC Manifest Options                                                                                                                                                             
             If you want to change the Windows User Account Control level replace the                                                                                                         
             requestedExecutionLevel node with one of the following.                                                                                                                          
        <requestedExecutionLevel  level="asInvoker" uiAccess="false" />                                                                                                                       
        <requestedExecutionLevel  level="requireAdministrator" uiAccess="false" />                                                                                                            
        <requestedExecutionLevel  level="highestAvailable" uiAccess="false" />                                                                                                                
            Specifying requestedExecutionLevel element will disable file and registry virtualization.                                                                                         
            Remove this element if your application requires this virtualization for backwards                                                                                                
            compatibility.                                                                                                                                                                    
        -->                                                                                                                                                                                   
        <requestedExecutionLevel level="requireAdministrator" uiAccess="false" />                                                                                                             
      </requestedPrivileges>                                                                                                                                                                  
    </security>                                                                                                                                                                               
  </trustInfo>                                                                                                                                                                                
  <compatibility xmlns="urn:schemas-microsoft-com:compatibility.v1">                                                                                                                          
    <application>                                                                                                                                                                             
      <!-- A list of the Windows versions that this application has been tested on and is                                                                                                     
           is designed to work with. Uncomment the appropriate elements and Windows will                                                                                                      
           automatically selected the most compatible environment. -->                                                                                                                        
      <!-- Windows Vista -->                                                                                                                                                                  
      <!--<supportedOS Id="{e2011457-1546-43c5-a5fe-008deee3d3f0}" />-->                                                                                                                      
      <!-- Windows 7 -->
      <!--<supportedOS Id="{35138b9a-5d96-4fbd-8e2d-a2440225f93a}" />-->
      <!-- Windows 8 -->
      <!--<supportedOS Id="{4a2f28e3-53b9-4441-ba9c-d69d4a4a6e38}" />-->
     <!-- Windows 8.1 -->
      <!--<supportedOS Id="{1f676c76-80e1-4239-95bb-83d0f6d0da78}" />-->
      <!-- Windows 10 -->
      <!--<supportedOS Id="{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}" />-->
    </application>
  </compatibility>
  <!-- Indicates that the application is DPI-aware and will not be automatically scaled by Windows at higher
       DPIs. Windows Presentation Foundation (WPF) applications are automatically DPI-aware and do not need 
       to opt in. Windows Forms applications targeting .NET Framework 4.6 that opt into this setting, should 
       also set the 'EnableWindowsFormsHighDpiAutoResizing' setting to 'true' in their app.config. -->
  <!--
  <application xmlns="urn:schemas-microsoft-com:asm.v3">
    <windowsSettings>
      <dpiAware xmlns="http://schemas.microsoft.com/SMI/2005/WindowsSettings">true</dpiAware>
    </windowsSettings>
  </application>
  -->
  <!-- Enable themes for Windows common controls and dialogs (Windows XP and later) -->
  <!--
  <dependency>
    <dependentAssembly>
      <assemblyIdentity
          type="win32"
          name="Microsoft.Windows.Common-Controls"
          version="6.0.0.0"
          processorArchitecture="*"
          publicKeyToken="6595b64144ccf1df"
          language="*"
        />
    </dependentAssembly>
  </dependency>
  -->
</assembly>
```

<p></p>

```
Your Files are locked. They are locked because you downloaded something with this file in it.
This is Ransomware. It locks your files until you pay for them. Before you ask, Yes we will
give you your files back once you pay and our server confrim that you pay.
```

<p></p>
It looks like we found the file we are looking for, but lets go one step further to confirm this, now we will dump the actual file and upload it to virus total. To dump the file we will use the following commands.
<p></p>
First we do a file scan to locate the file (I appended it to a text file as it is quicker to grep from, I did this in one line using double ampersands [&&] to fd a follow on command)
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 filescan > filescan.txt && cat filescan.txt | grep -i vmware-tray 
```

<p></p>
Which returns:
<p></p>

```
❯ cat filescan.txt | grep -i vmware-tray
0x000000007daad840     16      0 -W-r-- \Device\HarddiskVolume1\Users\Rick\AppData\Local\Temp\RarSFX0\vmware-tray.exe
0x000000007dc6cf20     13      0 R--r-d \Device\HarddiskVolume1\Users\Rick\AppData\Local\Temp\RarSFX0\vmware-tray.exe
```

<p></p>
Next we will dump these files using the following command (the <kbd>-Q</kbd> flag points to the physical offsets, the <kbd>-D</kbd> flag points to directory we wish to dump the files to with '.' being the current working directory):
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 dumpfiles -Q 0x000000007daad840,0x000000007dc6cf20 -D .
```

<p></p>
We are now left with two files (file.None.0xfffffa801ab15890.dat and file.None.0xfffffa801b494c30.img) you can run strings on these two which will give much the same output as when we ran strings on the process memory dump. But we are going to upload the .dat file to <a href="https://www.virustotal.com/gui/" rel="nofollow">VirusTotal</a>, and when we do that we see this:
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/Screenshot_2021-05-05%20VirusTotal_landing.png"><br>
</div>
<p></p>
You can look through the page and see what registry keys it changes and deletes, what files it creates and the additional process' that spawned that we couldn't see were linked.
<p></p>
We have now confirmed what the ransomware is so we now have the answer. If we got to the end of this process and nothing looked malicious then we would move to another process that was running that may have been the malware untill we found the file.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{vmware-tray.exe}
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Path To Glory</summary>
<p></p>
The eighth challenge we are given is:
<p></p>
How did the malware got to rick's PC? It must be one of rick old illegal habits...
<p></p>
format: CTF{...}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
If you completed the previous challenge this will be easy as we looked at the initial file that contained vmware-tray.exe that file was "Rick And Morty.exe" so lets go back and look at it. Last time we looked through the process memory dump of 'Rick And Morty' but this time we will dump the file and look through the actual file. To do this we will use the filescan.txt file we created and use <kbd>grep</kbd> to search for 'Rick And Morty':
<p></p>

```
cat filescan.txt | grep -i 'rick and morty'
```

<p></p>
Which outputs this (I have included the header lines so you can see what everything means):
<p></p>

```
❯ cat filescan.txt | grep -i 'rick and morty'
Offset(P)            #Ptr   #Hnd Access Name
------------------ ------ ------ ------ ----
0x000000007d63dbc0     10      0 R--r-d \Device\HarddiskVolume1\Torrents\Rick And Morty season 1 download.exe
0x000000007d6b3a10     11      1 R--rw- \Device\HarddiskVolume1\Torrents\Rick and Morty - Season 3 (2017) [1080p]\Rick.and.Morty.S03E07.The.Ricklantis.Mixup.1080p.Amazon.WEB-DL.x264-Rapta.mkv
0x000000007d7adb50     17      1 R--rw- \Device\HarddiskVolume1\Torrents\Rick and Morty - Season 3 (2017) [1080p]\Rick.and.Morty.S03E06.Rest.and.Ricklaxation.1080p.Amazon.WEB-DL.x264-Rapta.mkv
0x000000007d8813c0      2      0 RW-rwd \Device\HarddiskVolume1\Users\Rick\Downloads\Rick And Morty season 1 download.exe.torrent
0x000000007da56240      2      0 RW-rwd \Device\HarddiskVolume1\Torrents\Rick And Morty season 1 download.exe
0x000000007dae9350      2      0 RWD--- \Device\HarddiskVolume1\Users\Rick\AppData\Roaming\BitTorrent\Rick And Morty season 1 download.exe.1.torrent
0x000000007dcbf6f0      2      0 RW-rwd \Device\HarddiskVolume1\Users\Rick\AppData\Roaming\BitTorrent\Rick And Morty season 1 download.exe.1.torrent
0x000000007e5f5d10      3      1 R--rw- \Device\HarddiskVolume1\Torrents\Rick and Morty Season 2 [WEBRIP] [1080p] [HEVC]\[pseudo] Rick and Morty S02E03 Auto Erotic Assimilation [1080p] [h.265].mkv
0x000000007e710070      8      0 R--rwd \Device\HarddiskVolume1\Torrents\Rick And Morty season 1 download.exe
0x000000007e7ae700      3      1 R--rw- \Device\HarddiskVolume1\Torrents\Rick and Morty Season 2 [WEBRIP] [1080p] [HEVC]\Sample\Screenshot 08.png
```

<p></p>
We can see that there are other seasons etc in there but we are only interested in the season 1 dowload.exe, we can improve our grep to only show these results.
<p></p>

```
❯ cat filescan.txt | grep -i 'rick and morty season 1'
Offset(P)            #Ptr   #Hnd Access Name
------------------ ------ ------ ------ ----
0x000000007d63dbc0     10      0 R--r-d \Device\HarddiskVolume1\Torrents\Rick And Morty season 1 download.exe
0x000000007d8813c0      2      0 RW-rwd \Device\HarddiskVolume1\Users\Rick\Downloads\Rick And Morty season 1 download.exe.torrent
0x000000007da56240      2      0 RW-rwd \Device\HarddiskVolume1\Torrents\Rick And Morty season 1 download.exe
0x000000007dae9350      2      0 RWD--- \Device\HarddiskVolume1\Users\Rick\AppData\Roaming\BitTorrent\Rick And Morty season 1 download.exe.1.torrent
0x000000007dcbf6f0      2      0 RW-rwd \Device\HarddiskVolume1\Users\Rick\AppData\Roaming\BitTorrent\Rick And Morty season 1 download.exe.1.torrent
0x000000007e710070      8      0 R--rwd \Device\HarddiskVolume1\Torrents\Rick And Morty season 1 download.exe
```

<p></p>
That's better, so you can see we have both the .exe and the .exe.torrent files.
<br>
But what is torrenting?
<p></p>
<details>
    <summary>What is Torrenting?</summary>
<p></p>
Torrenting is the act of downloading and uploading files through the BitTorrent network. Instead of downloading files to a central server, torrenting involves downloading files from other users’ devices on the network. Conversely, users upload files from their own devices for other users to download.
<p></p>
Torrenting is the most popular form of peer-to-peer (P2P) file-sharing, and it requires torrent management software to connect to the BitTorrent network. Such software can be downloaded for free for a number of different devices.
<p></p>
Everyone downloading or uploading the same file is called a peer, and collectively they are known as a swarm. Because of how BitTorrent works, a peer can download a file from several other users at once, or upload a file to multiple other users simultaneously.
<p></p>
Torrenting is often associated with piracy because it’s frequently used to share files that are protected by copyright, including movies, games, music, and software. However, torrenting has many legitimate uses as well, such as lessening the load on centralized servers by distributing the hosting burden among users.
</details>
<p></p>
We will look at the .exe file first. The first thing we need to do is dump the file using this command:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 dumpfiles -n -Q 0x000000007d63dbc0 -D .
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 dumpfiles -n -Q 0x000000007d63dbc0 -D .
Volatility Foundation Volatility Framework 2.6
ImageSectionObject 0x7d63dbc0   None   \Device\HarddiskVolume1\Torrents\Rick And Morty season 1 download.exe
DataSectionObject 0x7d63dbc0   None   \Device\HarddiskVolume1\Torrents\Rick And Morty season 1 download.exe
```

<p></p>
We can now run strings on these two files however it doesn't turn up anything of importance. So we will look at he .torrent files as we know this is how the files got onto Rick's computer in the first place. We will now dump the .torrent files using this command:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 dumpfiles -n -Q 0x000000007d8813c0,0x000000007dae9350,0x000000007dcbf6f0 -D .
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 dumpfiles -n -Q 0x000000007d8813c0,0x000000007dae9350,0x000000007dcbf6f0 -D .
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x7d8813c0   None   \Device\HarddiskVolume1\Users\Rick\Downloads\Rick And Morty season 1 download.exe.torrent
DataSectionObject 0x7dae9350   None   \Device\HarddiskVolume1\Users\Rick\AppData\Roaming\BitTorrent\Rick And Morty season 1 download.exe.1.torrent
DataSectionObject 0x7dcbf6f0   None   \Device\HarddiskVolume1\Users\Rick\AppData\Roaming\BitTorrent\Rick And Morty season 1 download.exe.1.torrent
```

<p></p>
We now have 3 files we can analyse. We will start with the first one by running strings on the file, which returns:
<p></p>

```
❯ strings file.None.0xfffffa801af10010.Rick\ And\ Morty\ season\ 1\ download.exe.torrent.dat
[ZoneTransfer]
ZoneId=3
```

<p></p>
What does this mean?
<br>
zone 3 – Internet Zone, for Web sites on the Internet that do not belong to another zone;
<br>
So this means it was downloaded from the internet. Nothing else here so we will move to the next file.
<p></p>

```
❯ strings file.None.0xfffffa801b42c9e0.Rick\ And\ Morty\ season\ 1\ download.exe.1.torrent.dat
d8:announce44:udp://tracker.openbittorrent.com:80/announce13:announce-listll44:udp://tracker.openbittorrent.com:80/announceel42:udp://tracker.opentrackr.org:1337/announceee10:created by17:BitTorrent/7.10.313:creation datei1533150595e8:encoding5:UTF-84:infod6:lengthi456670e4:name36:Rick And Morty season 1 download.exe12:piece lengthi16384e6:pieces560:\I
!PC<^X
B.k_Rk
0<;O87o
!4^"
3hq,
&iW1|
K68:o
w~Q~YT
o9p
bwF:u
e7:website19:M3an_T0rren7_4_R!cke
```

<p></p>
Looking at this output is a bit more information and what looks like a flag.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{M3an_T0rren7_4_R!ck}
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Path To Glory 2</summary>
<p></p>
The ninth challenge we are given is:
<p></p>
Continue the search after the way that malware got in.
<p></p>
format: CTF{...}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
So from the last challenge we know that Rick downloaded the file from the internet, we can search his internet history using a volatility option <kbd>iehistory</kbd> the command looks like this:
<p></p>

```
sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 iehistory
```

<p></p>
And outputs this:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 iehistory
Volatility Foundation Volatility Framework 2.6
**************************************************
Process: 2728 explorer.exe
Cache type "DEST" at 0x6b80317
Last modified: 2018-08-04 22:34:11 UTC+0000
Last accessed: 2018-08-04 19:34:12 UTC+0000
URL: Rick@file:///C:/Users/Rick/Desktop/Flag.txt.WINDOWS
**************************************************
Process: 2728 explorer.exe
Cache type "DEST" at 0x6bb7227
Last modified: 2018-08-04 22:34:11 UTC+0000
Last accessed: 2018-08-04 19:34:12 UTC+0000
URL: Rick@file:///C:/Users/Rick/Desktop/Flag.txt.WINDOWS
```

<p></p>
Nothing useful for our current challenge here [This could definitely throw you off as this is the answer for a later challenge] but this information can definitely be used in a later challenge likely. Since this didn't help us lets look at the browsers that are running using <kbd>pstree</kbd>.
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 pstree                                                                                                                                
[sudo] password for parrot: 
Volatility Foundation Volatility Framework 2.6
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
 0xfffffa801b27e060:explorer.exe                     2728   2696     33    854 2018-08-04 19:27:04 UTC+0000
. 0xfffffa801b486b30:Rick And Morty                  3820   2728      4    185 2018-08-04 19:32:55 UTC+0000
.. 0xfffffa801a4c5b30:vmware-tray.ex                 3720   3820      8    147 2018-08-04 19:33:02 UTC+0000
. 0xfffffa801b2f02e0:WebCompanion.e                  2844   2728      0 ------ 2018-08-04 19:27:07 UTC+0000
. 0xfffffa801a4e3870:chrome.exe                      4076   2728     44   1160 2018-08-04 19:29:30 UTC+0000
.. 0xfffffa801a4eab30:chrome.exe                     4084   4076      8     86 2018-08-04 19:29:30 UTC+0000
.. 0xfffffa801a5ef1f0:chrome.exe                     1796   4076     15    170 2018-08-04 19:33:41 UTC+0000
.. 0xfffffa801aa00a90:chrome.exe                     3924   4076     16    228 2018-08-04 19:29:51 UTC+0000
.. 0xfffffa801a635240:chrome.exe                     3648   4076     16    207 2018-08-04 19:33:38 UTC+0000
.. 0xfffffa801a502b30:chrome.exe                      576   4076      2     58 2018-08-04 19:29:31 UTC+0000
.. 0xfffffa801a4f7b30:chrome.exe                     1808   4076     13    229 2018-08-04 19:29:32 UTC+0000
.. 0xfffffa801a7f98f0:chrome.exe                     2748   4076     15    181 2018-08-04 19:31:15 UTC+0000
. 0xfffffa801b5cb740:LunarMS.exe                      708   2728     18    346 2018-08-04 19:27:39 UTC+0000
. 0xfffffa801b1cdb30:vmtoolsd.exe                    2804   2728      6    190 2018-08-04 19:27:06 UTC+0000
. 0xfffffa801b290b30:BitTorrent.exe                  2836   2728     24    471 2018-08-04 19:27:07 UTC+0000
.. 0xfffffa801b4c9b30:bittorrentie.e                 2624   2836     13    316 2018-08-04 19:27:21 UTC+0000
.. 0xfffffa801b4a7b30:bittorrentie.e                 2308   2836     15    337 2018-08-04 19:27:19 UTC+0000
 0xfffffa8018d44740:System                              4      0     95    411 2018-08-04 19:26:03 UTC+0000
. 0xfffffa801947e4d0:smss.exe                         260      4      2     30 2018-08-04 19:26:03 UTC+0000
 0xfffffa801a2ed060:wininit.exe                       396    336      3     78 2018-08-04 19:26:11 UTC+0000
. 0xfffffa801ab377c0:services.exe                     492    396     11    242 2018-08-04 19:26:12 UTC+0000
.. 0xfffffa801afe7800:svchost.exe                    1948    492      6     96 2018-08-04 19:26:42 UTC+0000
.. 0xfffffa801ae92920:vmtoolsd.exe                   1428    492      9    313 2018-08-04 19:26:27 UTC+0000
... 0xfffffa801a572b30:cmd.exe                       3916   1428      0 ------ 2018-08-04 19:34:22 UTC+0000
.. 0xfffffa801ae0f630:VGAuthService.                 1356    492      3     85 2018-08-04 19:26:25 UTC+0000
.. 0xfffffa801abbdb30:vmacthlp.exe                    668    492      3     56 2018-08-04 19:26:16 UTC+0000
.. 0xfffffa801aad1060:Lavasoft.WCAss                 3496    492     14    473 2018-08-04 19:33:49 UTC+0000
.. 0xfffffa801a6af9f0:svchost.exe                     164    492     12    147 2018-08-04 19:28:42 UTC+0000
.. 0xfffffa801ac2e9e0:svchost.exe                     808    492     22    508 2018-08-04 19:26:18 UTC+0000
... 0xfffffa801ac753a0:audiodg.exe                    960    808      7    151 2018-08-04 19:26:19 UTC+0000
.. 0xfffffa801ae7f630:dllhost.exe                    1324    492     15    207 2018-08-04 19:26:42 UTC+0000
.. 0xfffffa801a6c2700:mscorsvw.exe                   3124    492      7     77 2018-08-04 19:28:43 UTC+0000
.. 0xfffffa801b232060:sppsvc.exe                     2500    492      4    149 2018-08-04 19:26:58 UTC+0000
.. 0xfffffa801abebb30:svchost.exe                     712    492      8    301 2018-08-04 19:26:17 UTC+0000
.. 0xfffffa801ad718a0:svchost.exe                    1164    492     18    312 2018-08-04 19:26:23 UTC+0000
.. 0xfffffa801ac31b30:svchost.exe                     844    492     17    396 2018-08-04 19:26:18 UTC+0000
... 0xfffffa801b1fab30:dwm.exe                       2704    844      4     97 2018-08-04 19:27:04 UTC+0000
.. 0xfffffa801988c2d0:PresentationFo                  724    492      6    148 2018-08-04 19:27:52 UTC+0000
.. 0xfffffa801b603610:mscorsvw.exe                    412    492      7     86 2018-08-04 19:28:42 UTC+0000
.. 0xfffffa8018e3c890:svchost.exe                     604    492     11    376 2018-08-04 19:26:16 UTC+0000
... 0xfffffa8019124b30:WmiPrvSE.exe                  1800    604      9    222 2018-08-04 19:26:39 UTC+0000
... 0xfffffa801b112060:WmiPrvSE.exe                  2136    604     12    324 2018-08-04 19:26:51 UTC+0000
.. 0xfffffa801ad5ab30:spoolsv.exe                    1120    492     14    346 2018-08-04 19:26:22 UTC+0000
.. 0xfffffa801ac4db30:svchost.exe                     868    492     45   1114 2018-08-04 19:26:18 UTC+0000
.. 0xfffffa801a6e4b30:svchost.exe                    3196    492     14    352 2018-08-04 19:28:44 UTC+0000
.. 0xfffffa801acd37e0:svchost.exe                     620    492     19    415 2018-08-04 19:26:21 UTC+0000
.. 0xfffffa801b1e9b30:taskhost.exe                   2344    492      8    193 2018-08-04 19:26:57 UTC+0000
.. 0xfffffa801ac97060:svchost.exe                    1012    492     12    554 2018-08-04 19:26:20 UTC+0000
.. 0xfffffa801b3aab30:SearchIndexer.                 3064    492     11    610 2018-08-04 19:27:14 UTC+0000
.. 0xfffffa801aff3b30:msdtc.exe                      1436    492     14    155 2018-08-04 19:26:43 UTC+0000
. 0xfffffa801ab3f060:lsass.exe                        500    396      7    610 2018-08-04 19:26:12 UTC+0000
. 0xfffffa801ab461a0:lsm.exe                          508    396     10    148 2018-08-04 19:26:12 UTC+0000
 0xfffffa801a0c8380:csrss.exe                         348    336      9    563 2018-08-04 19:26:10 UTC+0000
. 0xfffffa801a6643d0:conhost.exe                     2420    348      0     30 2018-08-04 19:34:22 UTC+0000
 0xfffffa80198d3b30:csrss.exe                         388    380     11    460 2018-08-04 19:26:11 UTC+0000
 0xfffffa801aaf4060:winlogon.exe                      432    380      3    113 2018-08-04 19:26:11 UTC+0000
 0xfffffa801b18f060:WebCompanionIn                   3880   1484     15    522 2018-08-04 19:33:07 UTC+0000
. 0xfffffa801aa72b30:sc.exe                          3504   3880      0 ------ 2018-08-04 19:33:48 UTC+0000
. 0xfffffa801aeb6890:sc.exe                           452   3880      0 ------ 2018-08-04 19:33:48 UTC+0000
. 0xfffffa801a6268b0:WebCompanion.e                  3856   3880     15    386 2018-08-04 19:34:05 UTC+0000
. 0xfffffa801b08f060:sc.exe                          3208   3880      0 ------ 2018-08-04 19:33:47 UTC+0000
. 0xfffffa801ac01060:sc.exe                          2028   3880      0 ------ 2018-08-04 19:33:49 UTC+0000
 0xfffffa801b1fd960:notepad.exe                      3304   3132      2     79 2018-08-04 19:34:10 UTC+0000
```

<p></p>
So here we can see that chrome is the only browser that is running, lets dump all of the process memory and analyse that to see if we can find the torrent file.
<p></p>
To do this we will use <kbd>memdump</kbd>. Because there is a parent and many child process' we will output the dump to a directory. The command looks like this:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 memdump -p4084,1796,3924,3648,576,1808,2748 -D chrome_dump
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing chrome.exe [  4084] to 4084.dmp
************************************************************************
Writing chrome.exe [   576] to 576.dmp
************************************************************************
Writing chrome.exe [  1808] to 1808.dmp
************************************************************************
Writing chrome.exe [  3924] to 3924.dmp
************************************************************************
Writing chrome.exe [  2748] to 2748.dmp
************************************************************************
Writing chrome.exe [  3648] to 3648.dmp
************************************************************************
Writing chrome.exe [  1796] to 1796.dmp                 
```

<p></p>
You can see that I outputed this using <kbd>-D</kbd> to a dump directory 'chrome_dump' that allows me to run strings on all files at the same time using the following command. We will search for the file extention we know was downloaded. "download.exe.torrent"
<p></p>

```
❯ strings chrome_dump/* | grep "download.exe.torrent"
Rick And Morty season 1 download.exe.torrent
```

<p></p>
So you can see the command strings is run agains the 'chrome_dump' directory and the wildcard '*' is added at the end to specify all files. We can see that we have found the file we are looking for but <kbd>grep</kbd> only prints the single line so lets modify our <kbd>grep</kbd> command to print the lines before and after. The command looks like this:
<p></p>

```
strings chrome_dump/* | grep -B 15 -A 15 "download.exe.torrent"
```

<p></p>
Here you can see we are using the <kbd>-B</kbd> which will print 15 lines before our found string and the <kbd>-A</kbd> flag which will print 15 lines after our found string. This command returns the following output:
<p></p>

```
❯ strings chrome_dump/* | grep -B 15 -A 15 "download.exe.torrent"                                                                                     [5/4786]
M8.81 5h2.4l-.18 7H8.98l-.17-7zM9 14h2v2H9z=                                                                                                                  
simple-icon_mail-classification-feedbackmKw=                                                                                                                  
form-composite-switchable-content_condition                                                                                                                   
form-composite-addresschooser_textfieldc.com                                                                                                                  
SPnvideo-label video-title trc_ellipsis  ]"sAE=                                                                                                               
display:inline;width:56px;height:200px;m>
Hum@n_I5_Th3_Weak3s7_Link_In_Th3_Ch@inYear
//sec-s.uicdn.com/nav-cdn/home/preloader.gif
simple-icon_toolbar-change-view-horizontal
 nnx-track-sec-click-communication-inboxic.com
nx-track-sec-click-dashboard-hide_smileyable
Nftd-box stem-north big fullsize js-focusable
js-box-flex need-overlay js-componentone
Jhttps://search.mail.com/web [q origin ]Year
ntrack-and-trace__delivery-info--has-iconf
Rick And Morty season 1 download.exe.torrent
tbl_1533411035475_7.0.1.40728_2033115181
panel-mail-display-table-mail-default35"
Cnpanel-mail-display-table-mail-horizontal.js
trc_rbox text-links-a trc-content-sponsored 
identity_OjpwcmVsb2FkZXIuaHRtbC50d2ln
Move the widget to its desired position.3c8=
Set-Cookie, no-store, proxy-revalidateHxRKw=
Set-Cookie, no-store, proxy-revalidate143/
tbl_1533411035475_7.0.9.40728_2033115181
"mail.com Update" <service@corp.mail.com>e
F'?Mhttps://www.mail.com/shareFeedback.htmluUAE=
Ynapplication/javascript; charset=UTF-835".js
nsection col-1 flexible component ui-sortable
link-item navigation-item js-componentne
m//www.googletagservices.com/tag/js/gpt.js80=
```

<p></p>
From this return some 1337 speak should stick out which is our next flag.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{Hum@n_I5_Th3_Weak3s7_Link_In_Th3_Ch@in}
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Bit 4 Bit</summary>
<p></p>
The tenth challenge we are given is:
<p></p>
We've found out that the malware is a ransomware. Find the attacker's bitcoin address.
<p></p>
format: CTF{...}
<p></p>
<H2>WARNING THIS IS LIVE RANSOMEWARE</H2>
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
Lets have a look at that file that was on the desktop from Path to Glory we will cat the filescan.txt that we made and <kbd>grep</kbd> for "desktop". The command looks like this:
<p></p>

```
cat filescan.txt | grep -i desktop
```

<p></p>
This returns two files of interest.
<p></p>

```
Offset(P)            #Ptr   #Hnd Access Name
------------------ ------ ------ ------ ----
0x000000007e410890     16      0 R--r-- \Device\HarddiskVolume1\Users\Rick\Desktop\Flag.txt
0x000000007d660500      2      0 -W-r-- \Device\HarddiskVolume1\Users\Rick\Desktop\READ_IT.txt                                                                
```

<p></p>
From here we can dump these two files to analyse them. The command looks like this:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 dumpfiles -n -Q 0x000000007e410890,0x000000007d660500 -D .
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x7e410890   None   \Device\HarddiskVolume1\Users\Rick\Desktop\Flag.txt
DataSectionObject 0x7d660500   None   \Device\HarddiskVolume1\Users\Rick\Desktop\READ_IT.txt
```

<p></p>
We can now <kbd>cat</kbd> these files.
<p></p>

```
❯ cat file.None.0xfffffa801b0532e0.Flag.txt.dat
{$V\C(l1Tr~{ƍШn>G
                 %  
```

<p></p>

```
❯ cat file.None.0xfffffa801b2def10.READ_IT.txt.dat
Your files have been encrypted.
Read the Program for more information
read program for more information.
```

<p></p>
The first file doesn't make much sense but the second file READ_IT.txt looks like it could be pointing us somewhere.
<p></p>
Back in Hide and Seek we did a memdump of the vmware-tray process (Pid 3720) we can run <kbd>strings</kbd> on this file and pain stakingly go through the 1589147 lines or we can modify our <kbd>strings</kbd> command and use <kbd>grep</kbd>.
<p></p>
We need to modify our <kbd>strings</kbd> command so that it reads unicode we do this by using the flag <kbd>-e l</kbd> before piping to <kbd>grep</kbd>, the command looks like this:
<p></p>

```
strings -e l 3720.dmp | grep -B 15 -A 15 -i "ransom" 
```

<p></p>
But why do we have to make <kbd>strings</kbd> read unicode? We do this because we are running <kbd>strings</kbd> on a program (we have previously seen the assembely code from challenge Hide and Seek) if we run strings without telling it to run unicode it wont show our answer.
<p></p>
The output is (only showing the tail end of the output):
<p></p>

```
Your Files are locked. They are locked because you downloaded something with this file in it.
This is Ransomware. It locks your files until you pay for them. Before you ask, Yes we will
give you your files back once you pay and our server confrim that you pay.
Send 0.16 to the address below.
e al
I paid, Now give me back my files.
1MmpEmebJkqXG8nQv4cjJSmxZQFVmFo63M
he program you want to use to open this file:
Flag.txt.WINDOWS
MSCTFIME UI
Cancel
OleMainThreadWndName
Rick And Morty season 1 download.exe - Add New Torrent
hortcut
&Medium icons
cons
arge icons
```

<p></p>
That string after "I paid, Now give me back my files." looks interesting and is the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{1MmpEmebJkqXG8nQv4cjJSmxZQFVmFo63M}
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Graphic's For The Weak</summary>
<p></p>
The eleventh challenge we are given is:
<p></p>
There's something fishy in the malware's graphics.
<p></p>
format: CTF{...}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
From the challenge hint we know that we  are searching for some type of graphic or image within the malware file (vmware-tray.exe) whenever a forensic challenge calls for image/graphic recovery the two go to tools are binwalk and foremost (foremost soley for image recovery/extraction). First we will dump the entire executable using <kbd>procdump</kbd>. The command looks like this:
<p></p>

```
❯ sudo volatility -f OtterCTF.vmem --profile=Win7SP1x64 procdump -p3720 -D .
Volatility Foundation Volatility Framework 2.6
Process(V)         ImageBase          Name                 Result
------------------ ------------------ -------------------- ------
0xfffffa801a4c5b30 0x0000000000ec0000 vmware-tray.ex       OK: executable.3720.exe
```

<p></p>
We can now start extracting files from the executable, we will first use binwalk. The command looks like this:
<p></p>

```
❯ binwalk executable.3720.exe

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             Microsoft executable, portable (PE)
9178          0x23DA          Copyright string: "CopyrightAttribute"
116288        0x1C640         PNG image, 4800 x 1454, 8-bit/color RGBA, non-interlaced
116416        0x1C6C0         Zlib compressed data, compressed
344098        0x54022         PNG image, 800 x 600, 8-bit colormap, non-interlaced
344511        0x541BF         Zlib compressed data, best compression
420575        0x66ADF         XML document, version: "1.0"
```

<p></p>
This prints out what files can be extracted from the .exe and we can see that there are 2 .png images we will run it with flags IOT extract, (I usually use <kbd>-BAMe</kbd> when I am using binwalk to extract files), the command looks like this:
<p></p>

```
❯ binwalk -BAMe executable.3720.exe                                                                                                                           
                                                                                                                                                              
Scan Time:     2021-05-06 12:22:27                                                                                                                            
Target File:   /home/parrot/ctf/OtterCTF/Memory_Forensics/executable.3720.exe                                                                                 
MD5 Checksum:  b40d8f9f7457d1e5adafb35e8063b100                                                                                                               
Signatures:    423                                                                                                                                            
                                                                                                                                                              
DECIMAL       HEXADECIMAL     DESCRIPTION                                                                                                                     
--------------------------------------------------------------------------------                                                                              
0             0x0             Microsoft executable, portable (PE)
9178          0x23DA          Copyright string: "CopyrightAttribute"
116288        0x1C640         PNG image, 4800 x 1454, 8-bit/color RGBA, non-interlaced
116416        0x1C6C0         Zlib compressed data, compressed
344098        0x54022         PNG image, 800 x 600, 8-bit colormap, non-interlaced
344511        0x541BF         Zlib compressed data, best compression
420575        0x66ADF         XML document, version: "1.0"


Scan Time:     2021-05-06 12:22:27
Target File:   /home/parrot/ctf/OtterCTF/Memory_Forensics/_executable.3720.exe-0.extracted/1C6C0
MD5 Checksum:  d41d8cd98f00b204e9800998ecf8427e
Signatures:    423

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------


Scan Time:     2021-05-06 12:22:27
Target File:   /home/parrot/ctf/OtterCTF/Memory_Forensics/_executable.3720.exe-0.extracted/541BF
MD5 Checksum:  c8046134f762f06d01ac6ec959db8769
Signatures:    423

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------



```

<p></p>
This creates a directory called "_executable.3720.exe.extracted" however when we look in the directory the PNG images weren't extracted, time to try <kbd>foremost</kbd> the command looks like this:
<p></p>

```
❯ foremost executable.3720.exe
Processing: executable.3720.exe
|*|
```

<p></p>
This creates a directory called "output" with a subdirectory called png with an image named 00000672.png.
<p></p>


```
❯ tree output

 ├──     audit.txt  
 └──     exe/ 
 │  └────     00000000.exe  
 └──     png/ 
 │  └────     00000672.png  
```

<p></p>
If we look at this image using an image viewer we get the flag for this challenge.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/00000672.png"><br>
</div>
<p></p>
CTF{S0_Just_M0v3_Socy}
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Recovery</summary>
<p></p>
The twelvth challenge we are given is:
<p></p>
Rick got to have his files recovered! What is the random password used to encrypt the files?
<p></p>
format: CTF{...}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This challenge takes some research, I first started by running strings and grepping for password against the files that we mem and proc dumped. But had no returns, I then thought back to the last challenge and the text file on the desktop:
<p></p>

```
❯ cat file.None.0xfffffa801b2def10.READ_IT.txt.dat
Your files have been encrypted.
Read the Program for more information
read program for more information.
```

<p></p>
So I figured I would look at the actual .exe in Ghidra (Ghidra is a free and open source reverse engineering tool developed by the National Security Agency of United States of America.) This is the import screen of Ghidra:
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/GhidraImportSummary.png"><br>
</div>
<p></p>
Which when I looked through the program I found a function called CreatePassword.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/ghidra_create_pass.png"><br>
</div>
<p></p>
And I could see what the password would look like.
<p></p>

```
CreatePassword-707-8848(uint param_1,uint *param_2)
```

<p></p>
But looking through the program I could not find what unit param_1 or 2 were, this was the same on IDA and R2. I know I am in the right area I just need to find out what those parameters are. So I went back to researching, I now googled from the last chalenge $ucylocker from the image we dumped and found out that this ransomware is written in .NET so I would need a decompiler for .NET which is where I turned to "JetBrains dotPeek" running on a Windows VM (remember this is live ransomware).
<p></p>
dotPeek loaded the program and showes us some other functions that weren't apparent in other decompilers
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/1cf7b3ef2b5f78409d711586855621e394a20b0c/Starting_Point/DFIR/Memory_Forensics/Volatility/images/dotPeek_load.png"><br>
</div>
<p></p>
We have seen "hidden_tear" while we were looking through the files using strings etc. previously but hadn't seen it as a function, opening this function up and looking through we find the function "CreatePassword" and can now see what the password looks like:
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/1cf7b3ef2b5f78409d711586855621e394a20b0c/Starting_Point/DFIR/Memory_Forensics/Volatility/images/dotPeek_CreatePassword.png"><br>
</div>
<p></p>
So we can see that the password looks like this "COMPUTERNAME-USERNAME PASSWORD" this looks like something we can search for within the file as we already know what the computer name (WIN-LO6FAF3DTFE) and username (Rick) is from our previous challenges.
<p></p>
From here we return to the memory dump of vmware-tray we extracted previously and run this command:
<p></p>

```
❯ strings -e l 3720.dmp | grep -B 5 -A 5 WIN-LO6FAF3DTFE-Rick
\Desktop\
abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890*!=&?&/
aDOBofVYUNVnmp7
aDOBofVYUNVnmp7
C:\Users\Rick\Desktop\
WIN-LO6FAF3DTFE-Rick aDOBofVYUNVnmp7
.txt
.doc
.docx
.xls
.xlsx
```

<p></p>
And here we can see from the information we pulled from "CreatePassword" the answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
CTF{aDOBofVYUNVnmp7}
</details>
</details>
</details>

<p></p>
<hr>
<p></p>

<details>
    <summary>Closure</summary>
<p></p>
The thirteenth and final challenge we are given is:
<p></p>
Now that you extracted the password from the memory, could you decrypt rick's files?
<p></p>
format: CTF{ANSWER}
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
For this challenge we need to decrypt Rick's files using the password we found from the last challenge (aDOBofVYUNVnmp7) our best bet is to find a decryptor that has already been built for this ransomware, searching google for $ucylocker, vapehacksloader and hidden_tear turns up a <a href="https://www.bleepingcomputer.com/download/hidden-tear-decrypter/" rel="nofollow">page</a> with a decryptor.
<p></p>
From our previous reasearch on $ucyLocker we know that it only encrypts files located on the desktop and remembering back to when we looked on the desktop there were 2 files there, the "READ_ME.txt" that we used for the previous challenge and the "Flag.txt" that we saw.
<p></p>
First I renamed the file to make it easier to work with:
<p></p>

```
❯ cp file.None.0xfffffa801b0532e0.Flag.txt.dat flag.txt
```

<p></p>

```
❯ cat flag.txt
{�$V�\�C(��Ń�l1����T�r���~�{ƍШ���n>�G�
                                      ��%   
```

<p></p>

```
❯ xxd flag.txt
00000000: 7be6 2456 9e5c 0fef 8e43 28f7 e4c5 83ff  {.$V.\...C(.....
00000010: 6c31 d7e6 1cda ea54 cf72 ddd6 ec7e b07b  l1.....T.r...~.{
00000020: c68d d0a8 ccc2 ce6e 3eee 0347 c10b b3e8  .......n>..G....
00000030: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000040: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000050: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000060: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000070: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000080: 0000 0000 0000 0000 0000 0000 0000 0000  ................
00000090: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000a0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000b0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
000000c0: 0000 0000 0000 0000 0000 0000 0000 0000  ................
```

<p></p>
Here you can see that I have run both <kbd>cat</kbd> and <kbd>xxd</kbd> on the flag.txt
<br>
When we ran <kbd>cat</kbd> on the file it is completely unreadable, and when we ran xxd to look at the individual bites we can see that there is only information at the start and the rest is buffered with null bytes. We can get rid of that empty space so we are only left with the actual bytes we need (If you attempt to decrypt with the null bytes present it wont decrypt) we do this by using <kbd>dd</kbd>, the command looks like this:
<p></p>

```
❯ dd bs=1 count=48 if=flag.txt of=flag_dd.txt
48+0 records in
48+0 records out
48 bytes copied, 0.000145241 s, 330 kB/s
```

<p></p>
Unpacking this command <kbd>bs=1</kbd> tells dd to read 1 byte at a time, <kbd>if=</kbd> tells dd the input file, <kbd>of=</kbd> tells dd the uptput file to write to. You can see that it read 48 bytes and outputted them to the new file. If we re-run xxd you can see what the new file looks like this:
<p></p>

```
❯ xxd flag_dd.txt
00000000: 7be6 2456 9e5c 0fef 8e43 28f7 e4c5 83ff  {.$V.\...C(.....
00000010: 6c31 d7e6 1cda ea54 cf72 ddd6 ec7e b07b  l1.....T.r...~.{
00000020: c68d d0a8 ccc2 ce6e 3eee 0347 c10b b3e8  .......n>..G....
```

<p></p>
We have now eliminated the null bytes and can now use the decoder (if you did this in your linux box like I did you will need a way of moving it to your Windows box, I used a python HTML server).
<p></p>
Now that the flag.txt is in our Windows box we can run decryptor on it.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/decryptor.png"><br>
</div>
<p></p>
Once we have decrypted the file we can open it in a text reader to get our flag.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/DFIR/Memory_Forensics/Volatility/images/flag.png"><br>
</div>
<p></p>
CTF{Im_Th@_B3S7_RicK_0f_Th3m_4ll}
<p></p>
After completing the series of Rick Challenges <a href="https://malwaretips.com/threads/the-ucylocker-ransomware.78330/" rel="nofollow">this page</a> is worth looking at,
it is a write up on the $ucyLocker ransomware and explains what it is very well.
<p></p>
This series of challenges was very interesting and required a significant amount of research IOT solve each stage, it started easy and progressed to very difficult.
</details>
</details>
</details>

</details>



<p></p>
<hr>
<p></p>





<H3>MEMLABS</H3>
<p></p>
<details>
    <summary>Challenges</summary>
<p></p>
This series of challenges was made by P. Abhiram Kumar (https://github.com/stuxnet999/MemLabs) and are hosted on github for free. They vary in their range of difficulty from begginer to hard, one thing to note is that the scenarios dont really provide much direction and it can be laborious searching through a massive dump for a needle in a haystack. Feel free to try and find the flags with the little information you are given, otherwise I will create scenario pointers to at least guide your analysis.
<p></p>
Each challenge will use a different memory file which can be obtained from https://github.com/stuxnet999/MemLabs
<p></p>
The challenges are as follows:
<p></p>

Directory | Challenge Name | Level Of Difficulty
----------|----------------|----------------------
Lab 0 | Never Too Late Mister | Sample challenge
Lab 1 | Beginner's Luck | Easy
Lab 2 | A New World | Easy
Lab 3 | The Evil's Den | Easy - Medium
Lab 4 | Obsession | Medium
Lab 5 | Black Tuesday | Medium - Hard
Lab 6 | The Reckoning | Hard

<p></p>
<hr>
<p></p>

<details>
    <summary>Lab 0 - Never Too Late Mister</summary>
<p></p>
MEMLABS provides a write up for LAB 0 as it is designed as an introduction so I will post a link direct to that walkthrough.
<p></p>
Challenge File: <a href="https://github.com/stuxnet999/MemLabs/tree/master/Lab%200" rel="nofollow">MEMLABS 0</a>
<p></p>
The only thing I will discuss is how to extract the file. First you will need to ensure you have xz-utils installed:
<p></p>

```
sudo apt-get install xz-utils
```

<p></p>
Once you have this installed you use the following command to extract the .tar files
<p></p>

```
unxz Challenge.tar.xz
```

<p></p>

you will then need to untar the challenge file:

<p></p>

```
unxz Challenge.tar.xz
```

<p></p>
This will leave you with the file to start analysing.
<p></p>
</details>
<p></p>
<hr>

<p></p>
<details>
    <summary>Lab 1 - Beginner's Luck</summary>
<p></p>
The first challenge we are given is:
<p></p>
My sister's computer crashed. We were very fortunate to recover this memory dump. Your job is get all her important files from the system. From what we remember, we suddenly saw a black window pop up with some thing being executed. When the crash happened, she was trying to draw something. Thats all we remember from the time of crash.
<p></p>
Note: This challenge is composed of 3 flags.
<p></p>
Challenge File: <a href="https://github.com/stuxnet999/MemLabs/tree/master/Lab%201" rel="nofollow">MEMLABS 1</a>
<p></p>
<hr>
<p></p>
<details>
    <summary>Getting Started</summary>
<p></p>
This file is given with MD5 hashes (MD5 hashes are used to ensure the data integrity of files. Because the MD5 hash algorithm always produces the same output for the same given input, users can compare a hash of the source file with a newly created hash of the destination file to check that it is intact and unmodified.)
<p></p>
The commpressed archive
<p></p>
    MD5 hash: 919a0ded944c427b7f4e5c26a6790e8d
<p></p>
The memory dump
<p></p>
    MD5 hash: b9fec1a443907d870cb32b048bda9380
<p></p>
To test the file to enusre no changes have been made we simply run the following command and compare it to the one provided:
<p></p>

```
md5sum MemLabs-Lab1.7z
```

<p></p>
and
<p></p>

```
md5sum MemoryDump_Lab1.raw
```

<p></p>
<hr>
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
❯ p7zip -d MemLabs-Lab1.7z

7-Zip (a) [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_AU.UTF-8,Utf16=on,HugeFiles=on,64 bits,8 CPUs Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz (806EA),ASM,AES-NI)

Scanning the drive for archives:
1 file, 158197742 bytes (151 MiB)

Extracting archive: MemLabs-Lab1.7z
--
Path = MemLabs-Lab1.7z
Type = 7z
Physical Size = 158197742
Headers Size = 146
Method = LZMA2:24
Solid = -
Blocks = 1

Everything is Ok          

Size:       1073676288
Compressed: 158197742
```


</details>
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
<details>
    <summary>Scenario Hints</summary>
<p></p>
- The black window that popped up may have been running a command.
<br>
- She was drawing something so try looking for an image program.
<br>
- I wonder if there are any "important" files on her computer.
<p></p>
</details>
<p></p>
<details>
    <summary>Starting Point</summary>
<p></p>
The first thing we need to do is figure out what the <kbd>imageinfo</kbd> is IOT get our profile.
<br>
The command looks like this:
<p></p>

```
sudo volatility -f MemoryDump_Lab1.raw imageinfo
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f MemoryDump_Lab1.raw imageinfo
Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/parrot/ctf/MEMLABS/MEMLABS_1/MemoryDump_Lab1.raw)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf800028100a0L
          Number of Processors : 1
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff80002811d00L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2019-12-11 14:38:00 UTC+0000
     Image local date and time : 2019-12-11 20:08:00 +0530
```

<p></p>
From this output we can see the suggested profiles and will use Win7SP1x64 for this challenge.
<p></p>
</details>
<details>
    <summary>Walkthrough Flag 1</summary>
<p></p>
The first thing we want to look at is the the black box that popped up, this likely ran a command so we will investigate this.
<p></p>
The options we will use for this are:
<p></p>

Option | Description
-------|--------------
pstree | To view the process listing in tree form, use the <kbd>pstree</kbd> command. This enumerates processes using the same technique as <kbd>pslist</kbd>, so it will also not show hidden or unlinked processes. Child process are indicated using indention and periods.
cmdline | This will output all process command-line outputs.
consoles | Similar to <kbd>cmdscan</kbd> the <kbd>consoles</kbd> plugin finds commands that attackers typed into cmd.exe or executed via backdoors. However, instead of scanning for COMMAND_HISTORY, this plugin scans for CONSOLE_INFORMATION. The major advantage to this plugin is it not only prints the commands attackers typed, but it collects the entire screen buffer (input and output). For instance, instead of just seeing "dir", you'll see exactly what the attacker saw, including all files and directories listed by the "dir" command. <p></p> Additionally, this plugin prints the following: <p></p> - The original console window title and current console window title <br> - The name and pid of attached processes (walks a LIST_ENTRY to enumerate all of them if more than one) <br> - Any aliases associated with the commands executed. For example, attackers can register an alias such that typing "hello" actually executes "cd system" <br> - The screen coordinates of the cmd.exe console

<p></p>
Now we have the options we will use we can begin our analysis.
<p></p>
The first thing we will do is see what processes are currently running. To do that we will use the <kbd>pstree</kbd> command:
<p></p>

```
sudo volatility -f MemoryDump_Lab1.raw --profile=Win7SP1x64 pstree
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f MemoryDump_Lab1.raw --profile=Win7SP1x64 pstree                                                                                                                          
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
Name                                                  Pid   PPid   Thds   Hnds Time                                                                                                           
-------------------------------------------------- ------ ------ ------ ------ ----                                                                                                           
 0xfffffa8000f4c670:explorer.exe                     2504   3000     34    825 2019-12-11 14:37:14 UTC+0000                                                                                   
. 0xfffffa8000f9a4e0:VBoxTray.exe                    2304   2504     14    144 2019-12-11 14:37:14 UTC+0000
. 0xfffffa8001010b30:WinRAR.exe                      1512   2504      6    207 2019-12-11 14:37:23 UTC+0000
 0xfffffa8001c5f630:wininit.exe                       424    312      3     75 2019-12-11 13:41:34 UTC+0000
. 0xfffffa8001c98530:services.exe                     484    424     13    219 2019-12-11 13:41:35 UTC+0000
.. 0xfffffa8002170630:wmpnetwk.exe                   1856    484     16    451 2019-12-11 14:16:08 UTC+0000
.. 0xfffffa8001f91b30:TCPSVCS.EXE                    1416    484      4     97 2019-12-11 13:41:55 UTC+0000
.. 0xfffffa8001da96c0:svchost.exe                     876    484     32    941 2019-12-11 13:41:43 UTC+0000
.. 0xfffffa8001d327c0:VBoxService.ex                  652    484     13    137 2019-12-11 13:41:40 UTC+0000
.. 0xfffffa8000eac770:svchost.exe                    2660    484      6    100 2019-12-11 14:35:14 UTC+0000
.. 0xfffffa80022199e0:svchost.exe                    2368    484      9    365 2019-12-11 14:32:51 UTC+0000
.. 0xfffffa8001e50b30:svchost.exe                    1044    484     14    366 2019-12-11 13:41:48 UTC+0000
.. 0xfffffa8001d8c420:svchost.exe                     816    484     23    569 2019-12-11 13:41:42 UTC+0000
... 0xfffffa80021da060:audiodg.exe                   2064    816      6    131 2019-12-11 14:32:37 UTC+0000
.. 0xfffffa8001c38580:svchost.exe                     948    484     13    322 2019-12-11 14:16:07 UTC+0000
.. 0xfffffa8001eba230:spoolsv.exe                    1208    484     13    282 2019-12-11 13:41:51 UTC+0000
.. 0xfffffa8001d376f0:SearchIndexer.                  480    484     14    701 2019-12-11 14:16:09 UTC+0000
... 0xfffffa8000fff630:SearchProtocol                2524    480      7    226 2019-12-11 14:37:21 UTC+0000
... 0xfffffa8001020b30:SearchProtocol                2868    480      8    279 2019-12-11 14:37:23 UTC+0000
... 0xfffffa8000ecea60:SearchFilterHo                1720    480      5     90 2019-12-11 14:37:21 UTC+0000
.. 0xfffffa8000f3aab0:taskhost.exe                   2908    484      9    158 2019-12-11 14:37:13 UTC+0000
.. 0xfffffa8001cf4b30:svchost.exe                     588    484     11    358 2019-12-11 13:41:39 UTC+0000
.. 0xfffffa8001d49b30:svchost.exe                     720    484      8    279 2019-12-11 13:41:41 UTC+0000
.. 0xfffffa8001da5b30:svchost.exe                     852    484     28    542 2019-12-11 13:41:43 UTC+0000
... 0xfffffa8000f4db30:dwm.exe                       3004    852      5     72 2019-12-11 14:37:14 UTC+0000
... 0xfffffa8001dfa910:dwm.exe                       1988    852      5     72 2019-12-11 14:32:25 UTC+0000
.. 0xfffffa8001e1bb30:svchost.exe                     472    484     19    476 2019-12-11 13:41:47 UTC+0000
.. 0xfffffa8000d3c400:sppsvc.exe                     1508    484      4    141 2019-12-11 14:16:06 UTC+0000
.. 0xfffffa8001f58890:svchost.exe                    1372    484     22    295 2019-12-11 13:41:54 UTC+0000
.. 0xfffffa8001eda060:svchost.exe                    1248    484     19    313 2019-12-11 13:41:52 UTC+0000
.. 0xfffffa8001eb47f0:taskhost.exe                    296    484      8    151 2019-12-11 14:32:24 UTC+0000
. 0xfffffa8001ca0580:lsass.exe                        492    424      9    764 2019-12-11 13:41:35 UTC+0000
. 0xfffffa8001ca4b30:lsm.exe                          500    424     11    185 2019-12-11 13:41:35 UTC+0000
 0xfffffa800154f740:csrss.exe                         320    312      9    457 2019-12-11 13:41:32 UTC+0000
 0xfffffa8000ca0040:System                              4      0     80    570 2019-12-11 13:41:25 UTC+0000
. 0xfffffa800148f040:smss.exe                         248      4      3     37 2019-12-11 13:41:25 UTC+0000
.. 0xfffffa8001c45060:psxss.exe                       376    248     18    786 2019-12-11 13:41:33 UTC+0000
 0xfffffa8001c5f060:winlogon.exe                      416    360      4    118 2019-12-11 13:41:34 UTC+0000
 0xfffffa8000ca81e0:csrss.exe                         368    360      7    199 2019-12-11 13:41:33 UTC+0000
. 0xfffffa8002227140:conhost.exe                     2692    368      2     50 2019-12-11 14:34:54 UTC+0000
. 0xfffffa800104a780:conhost.exe                     2260    368      2     50 2019-12-11 14:37:54 UTC+0000
 0xfffffa8002046960:explorer.exe                      604   2016     33    927 2019-12-11 14:32:25 UTC+0000
. 0xfffffa80021c75d0:VBoxTray.exe                    1844    604     11    140 2019-12-11 14:32:35 UTC+0000
. 0xfffffa8002222780:cmd.exe                         1984    604      1     21 2019-12-11 14:34:54 UTC+0000
. 0xfffffa80022bab30:mspaint.exe                     2424    604      6    128 2019-12-11 14:35:14 UTC+0000
. 0xfffffa8001048060:DumpIt.exe                       796    604      2     45 2019-12-11 14:37:54 UTC+0000
 0xfffffa8001e68060:csrss.exe                        2760   2680      7    172 2019-12-11 14:37:05 UTC+0000
 0xfffffa8000ecbb30:winlogon.exe                     2808   2680      4    119 2019-12-11 14:37:05 UTC+0000
```

<p></p>
From this output we can see some processes running that should stick out mainly: WinRAR.exe, mspaint.exe and cmd.exe we will look at the last process for this stage of the challenge.
<p></p>
The next option we will run is <kbd>cmdline</kbd>:
<p></p>

```
sudo volatility -f MemoryDump_Lab1.raw --profile=Win7SP1x64 cmdline
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f MemoryDump_Lab1.raw --profile=Win7SP1x64 cmdline                                                                                                                         
Volatility Foundation Volatility Framework 2.6                                                                                                                                                
************************************************************************                                                                                                                      
System pid:      4                                                                                                                                                                            
************************************************************************                                                                                                                      
smss.exe pid:    248                                                                                                                                                                          
Command line : \SystemRoot\System32\smss.exe                                                                                                                                                  
************************************************************************                                                                                                                      
csrss.exe pid:    320                                                                                                                                                                         
Command line : %SystemRoot%\system32\csrss.exe ObjectDirectory=\Windows SharedSection=1024,20480,768 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=winsrv:UserServerDllInitia
lization,3 ServerDll=winsrv:ConServerDllInitialization,2 ServerDll=sxssrv,4 ProfileControl=Off MaxRequestThreads=16                                                                           
************************************************************************                                                                                                                      
csrss.exe pid:    368                                                                                                                                                                         
Command line : %SystemRoot%\system32\csrss.exe ObjectDirectory=\Windows SharedSection=1024,20480,768 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=winsrv:UserServerDllInitia
lization,3 ServerDll=winsrv:ConServerDllInitialization,2 ServerDll=sxssrv,4 ProfileControl=Off MaxRequestThreads=16
************************************************************************
psxss.exe pid:    376
Command line : %SystemRoot%\system32\psxss.exe
************************************************************************
winlogon.exe pid:    416
Command line : winlogon.exe
************************************************************************
wininit.exe pid:    424
Command line : wininit.exe
************************************************************************
services.exe pid:    484
Command line : C:\Windows\system32\services.exe 
************************************************************************
lsass.exe pid:    492
Command line : C:\Windows\system32\lsass.exe
************************************************************************
lsm.exe pid:    500
Command line : C:\Windows\system32\lsm.exe
************************************************************************
svchost.exe pid:    588
Command line : C:\Windows\system32\svchost.exe -k DcomLaunch
************************************************************************
VBoxService.ex pid:    652
Command line : C:\Windows\System32\VBoxService.exe
************************************************************************
svchost.exe pid:    720
Command line : C:\Windows\system32\svchost.exe -k RPCSS
************************************************************************
svchost.exe pid:    816
Command line : C:\Windows\System32\svchost.exe -k LocalServiceNetworkRestricted
************************************************************************
svchost.exe pid:    852
Command line : C:\Windows\System32\svchost.exe -k LocalSystemNetworkRestricted
************************************************************************
svchost.exe pid:    876
Command line : C:\Windows\system32\svchost.exe -k netsvcs
************************************************************************
svchost.exe pid:    472
Command line : C:\Windows\system32\svchost.exe -k LocalService
************************************************************************
svchost.exe pid:   1044
Command line : C:\Windows\system32\svchost.exe -k NetworkService
************************************************************************                                                                                                         [60/4517]
spoolsv.exe pid:   1208
Command line : C:\Windows\System32\spoolsv.exe
************************************************************************
svchost.exe pid:   1248
Command line : C:\Windows\system32\svchost.exe -k LocalServiceNoNetwork
************************************************************************
svchost.exe pid:   1372
Command line : C:\Windows\system32\svchost.exe -k LocalServiceAndNoImpersonation
************************************************************************
TCPSVCS.EXE pid:   1416
Command line : C:\Windows\System32\tcpsvcs.exe
************************************************************************
sppsvc.exe pid:   1508
Command line : C:\Windows\system32\sppsvc.exe
************************************************************************
svchost.exe pid:    948
Command line : C:\Windows\System32\svchost.exe -k secsvcs
************************************************************************
wmpnetwk.exe pid:   1856
Command line : "C:\Program Files\Windows Media Player\wmpnetwk.exe"
************************************************************************
SearchIndexer. pid:    480
Command line : C:\Windows\system32\SearchIndexer.exe /Embedding
************************************************************************
taskhost.exe pid:    296
Command line : "taskhost.exe"
************************************************************************
dwm.exe pid:   1988
Command line : "C:\Windows\system32\Dwm.exe"
************************************************************************
explorer.exe pid:    604
Command line : C:\Windows\Explorer.EXE
************************************************************************
VBoxTray.exe pid:   1844
Command line : "C:\Windows\System32\VBoxTray.exe" 
************************************************************************
audiodg.exe pid:   2064
Command line : C:\Windows\system32\AUDIODG.EXE 0x20c
************************************************************************
svchost.exe pid:   2368
Command line : C:\Windows\System32\svchost.exe -k LocalServicePeerNet
************************************************************************
cmd.exe pid:   1984
Command line : "C:\Windows\system32\cmd.exe" 
************************************************************************
conhost.exe pid:   2692
Command line : \??\C:\Windows\system32\conhost.exe
************************************************************************
mspaint.exe pid:   2424
Command line : "C:\Windows\system32\mspaint.exe" 
************************************************************************
svchost.exe pid:   2660
Command line : C:\Windows\system32\svchost.exe -k imgsvc
************************************************************************
csrss.exe pid:   2760
Command line : %SystemRoot%\system32\csrss.exe ObjectDirectory=\Windows SharedSection=1024,20480,768 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=winsrv:UserServerDllInitia
lization,3 ServerDll=winsrv:ConServerDllInitialization,2 ServerDll=sxssrv,4 ProfileControl=Off MaxRequestThreads=16
************************************************************************
winlogon.exe pid:   2808
Command line : winlogon.exe
************************************************************************
taskhost.exe pid:   2908
Command line : "taskhost.exe"
************************************************************************
dwm.exe pid:   3004
Command line : "C:\Windows\system32\Dwm.exe"
************************************************************************
explorer.exe pid:   2504
Command line : C:\Windows\Explorer.EXE
************************************************************************
VBoxTray.exe pid:   2304
Command line : "C:\Windows\System32\VBoxTray.exe" 
************************************************************************
SearchProtocol pid:   2524
Command line : "C:\Windows\system32\SearchProtocolHost.exe" Global\UsGthrFltPipeMssGthrPipe_S-1-5-21-3073570648-3149397540-2269648332-10032_ Global\UsGthrCtrlFltPipeMssGthrPipe_S-1-5-21-3073
570648-3149397540-2269648332-10032 1 -2147483646 "Software\Microsoft\Windows Search" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT; MS Search 4.0 Robot)" "C:\ProgramData\Microsoft\Search\Da
ta\Temp\usgthrsvc" "DownLevelDaemon"  "1"
************************************************************************
SearchFilterHo pid:   1720
Command line : "C:\Windows\system32\SearchFilterHost.exe" 0 508 512 520 65536 516 
************************************************************************
WinRAR.exe pid:   1512
Command line : "C:\Program Files\WinRAR\WinRAR.exe" "C:\Users\Alissa Simpson\Documents\Important.rar"
************************************************************************
SearchProtocol pid:   2868
Command line : "C:\Windows\system32\SearchProtocolHost.exe" Global\UsGthrFltPipeMssGthrPipe3_ Global\UsGthrCtrlFltPipeMssGthrPipe3 1 -2147483646 "Software\Microsoft\Windows Search" "Mozilla/
4.0 (compatible; MSIE 6.0; Windows NT; MS Search 4.0 Robot)" "C:\ProgramData\Microsoft\Search\Data\Temp\usgthrsvc" "DownLevelDaemon" 
************************************************************************
DumpIt.exe pid:    796
Command line : "C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe" 
************************************************************************
conhost.exe pid:   2260
Command line : \??\C:\Windows\system32\conhost.exe
```

<p></p>
This showed us that the commands were run and showed us some important information for WinRAR.exe but we will come abck to that.
<br>
We will now run <kbd>consoles</kbd> to see the input/output within the command-line:
<p></p>

```
sudo volatility -f MemoryDump_Lab1.raw --profile=Win7SP1x64 consoles
```

<p></p>
Which outputs:
<p></p>

```
❯ sudo volatility -f MemoryDump_Lab1.raw --profile=Win7SP1x64 consoles
Volatility Foundation Volatility Framework 2.6
**************************************************
ConsoleProcess: conhost.exe Pid: 2692
Console: 0xff756200 CommandHistorySize: 50
HistoryBufferCount: 1 HistoryBufferMax: 4
OriginalTitle: %SystemRoot%\system32\cmd.exe
Title: C:\Windows\system32\cmd.exe - St4G3$1
AttachedProcess: cmd.exe Pid: 1984 Handle: 0x60 
----
CommandHistory: 0x1fe9c0 Application: cmd.exe Flags: Allocated, Reset
CommandCount: 1 LastAdded: 0 LastDisplayed: 0
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
Cmd #0 at 0x1de3c0: St4G3$1
----
Screen 0x1e0f70 X:80 Y:300
Dump:
Microsoft Windows [Version 6.1.7601]                                             
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.                 
                                                                                 
C:\Users\SmartNet>St4G3$1                                                        
ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=                                         
Press any key to continue . . .                                                  
**************************************************
ConsoleProcess: conhost.exe Pid: 2260
Console: 0xff756200 CommandHistorySize: 50
HistoryBufferCount: 1 HistoryBufferMax: 4
OriginalTitle: C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe
Title: C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe
AttachedProcess: DumpIt.exe Pid: 796 Handle: 0x60
----
CommandHistory: 0x38ea90 Application: DumpIt.exe Flags: Allocated
CommandCount: 0 LastAdded: -1 LastDisplayed: -1 
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
----
Screen 0x371050 X:80 Y:300
Dump:
  DumpIt - v1.3.2.20110401 - One click memory memory dumper                     
  Copyright (c) 2007 - 2011, Matthieu Suiche <http://www.msuiche.net>           
  Copyright (c) 2010 - 2011, MoonSols <http://www.moonsols.com>                 
                                                                                 
                                                                                 
    Address space size:        1073676288 bytes (   1023 Mb)                    
    Free space size:          24185389056 bytes (  23064 Mb)                    
                                                                                 
    * Destination = \??\C:\Users\SmartNet\Downloads\DumpIt\SMARTNET-PC-20191211-
143755.raw                                                                       
                                                                                 
    --> Are you sure you want to continue? [y/n] y                              
    + Processing...                                                              
```

<p></p>
This command however did print some useful information for us, we can see that cmd.exe ran a binary called "St4G3$1" and we can see the output of that binary.
<p></p>

```
ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=
```

<p></p>
Over time you will get used to seeing certain types of hashes, encryption and computer language. For example this output is base64 which can be identified by the "=" at the end of the string. 
<br>
(In programming, Base64 is a group of binary-to-text encoding schemes that represent binary data (more specifically, a sequence of 8-bit bytes) in an ASCII string format by translating the data into a radix-64 representation. The term Base64 originates from a specific MIME content transfer encoding. Each non-final Base64 digit represents exactly 6 bits of data. Three 8-bit bytes (i.e., a total of 24 bits) can therefore be represented by four 6-bit Base64 digits.
<p></p>
Common to all binary-to-text encoding schemes, Base64 is designed to carry data stored in binary formats across channels that only reliably support text content. Base64 is particularly prevalent on the World Wide Web where its uses include the ability to embed image files or other binary assets inside textual assets such as HTML and CSS files.) 
<p></p>
Now that we know that the output is base64 how do we convert it to something we can read? Linux comes built in with a base64 encoder and decoder it takes a standard input and gives a standard output. To pass the encoded string as a standard input we will use the <kbd>echo</kbd> command:
<p></p>

```
echo ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0= | base64 -d
```

<p></p>
In this command we are echoing the string and piping it as a standard input into <kbd>base64</kbd> we use the <kbd>-d</kbd> flag to decode from base64 to human readable language.
<br>
The output is our first flag.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>

```
❯ echo ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0= | base64 -d
flag{th1s_1s_th3_1st_st4g3!!}% 
```

<p></p>
FLAG{th1s_1s_th3_1st_st4g3!!}
<p></p>
</details>
</details>
<p></p>
<details>
    <summary>Walkthrough Flag 2</summary>
</details>
<p></p>
<details>
    <summary>Walkthrough Flag 3</summary>
</details>
</details>
</details>
<p></p>
<hr>

<p></p>
<details>
    <summary>Lab 2 - A New World</summary>

</details>
<p></p>
<hr>

<p></p>
<details>
    <summary>Lab 3 - The Evil's Den</summary>

</details>
<p></p>
<hr>

<p></p>
<details>
    <summary>Lab 4 - Obsession</summary>

</details>
<p></p>
<hr>

<p></p>
<details>
    <summary>Lab 5 - Black Tuesday</summary>

</details>
<p></p>
<hr>

<p></p>
<details>
    <summary>Lab 6 - The Reckoning</summary>

</details>
</details>
<p></p>
<hr>