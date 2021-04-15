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
We will use a series of challenges (A Bob's Life...) from the CSC presented as a write up IOT practice the use of volatility.
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
<hr>
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

<hr>

<details>
    <summary>A Bob's Life 2</summary>
<p></p>
The second challenge we are given is:
<P></P>
So much for being air gapped. Maybe someone did use his machine after
<br>
all? Bob keeps notes in his "Blog", Can you make sure no one has found it?
<hr>
So what information can we pull from the hint?
<p></p>
<details>
    <summary>Spoilers</summary>
<p></p>
- We are looking for a file called "Blog"
<br>
- We need to make sure it hasn't been changed.
<p></p>
<p>So with that information we can start our analysis.</p>
</details>

<hr>

<p></p>
Now that we know what we are looking for what are the possible options we can use?
<details>
    <summary>Spoilers</summary>
<p></p>
Options | Description
-------------------------------------
asd | asd



</details>


</details>
