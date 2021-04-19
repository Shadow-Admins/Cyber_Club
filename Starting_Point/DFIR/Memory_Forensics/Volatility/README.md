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

<H2>Where do I Start?</H2>
<details>
    <summary></summary>
Volatility has a ton of options (flags) that can be used and can be overwhelming at the start.
But once you work out a general road-map you can work through an analysis methodically. Once you have your initial foothold you can start searching for the information you need.
<br>
If at any stage you dont know what command to run you can use the <kbd>-h</kbd> flag to print a list of commands you can use.
<br>
We will use a series of challenges (A Bob's Life... and Memory is RAM) from the CSC presented as a write up IOT practice the use of volatility.
</details>
<hr>

<H2>Getting your Foothold</H2>
<details>
    <summary></summary>
Assuming you already have a memory file (we will use 'bob.vmem' for this exercise as obtaining a memory file is another lesson) we need to identify what type of system (or profile) the memory is from.

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
This one command have us alot of info however we are looking for one specific piece of information, that is the suggested profiles.

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
<p></p>
<hr>
<p></p>
<H3>Memory is RAM</H2>
<p></p>
This series of challenges is from the 2020 Cyber Skills Challenge, These challenges will use the file dump.vmem
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
<br>
    - Onion routing is implemented by encryption in the application layer of a communication protocol stack, nested like the layers of an onion. Tor encrypts the data, including the next node destination IP address, multiple times and sends it through a virtual circuit comprising successive, random-selection Tor relays. Each relay decrypts a layer of encryption to reveal the next relay in the circuit to pass the remaining encrypted data on to it. The final relay decrypts the innermost layer of encryption and sends the original data to its destination without revealing or knowing the source IP address. Because the routing of the communication was partly concealed at every hop in the Tor circuit, this method eliminates any single point at which the communicating peers can be determined through network surveillance that relies upon knowing its source and destination.
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
<p></p>
<hr>
<p></p>
<H3>MEMLABS</H3>
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
The first challenge we are given is:
<p></p>
My friend John is an "environmental" activist and a humanitarian. He hated the ideology of Thanos from the Avengers: Infinity War. He sucks at programming. He used too many variables while writing any program. One day, John gave me a memory dump and asked me to find out what he was doing while he took the dump. Can you figure it out for me?
<p></p>
Challenge File: https://drive.google.com/file/d/1MjMGRiPzweCOdikO3DTaVfbdBK5kyynT/view
<p></p>



</details>
<p></p>
<hr>

<p></p>
<details>
    <summary>Lab 1 - Beginner's Luck</summary>

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
<p></p>
<hr>