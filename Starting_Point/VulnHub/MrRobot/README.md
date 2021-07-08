<H1>Mr Robot</H1>
<p></p>
<H2>What is it?</H2>
<p></p>
Mr Robot is a vulnerable VM that can be downloaded from <a href="https://www.vulnhub.com/entry/mr-robot-1,151/" rel="nofollow">VulnHub</a> so that you can host it locally or you can complete it on <a href="https://tryhackme.com/room/mrrobot" rel="nofollow">TryHackMe</a>. Mr Robot is a free room on TryHackMe so anyone can do it without needing to subscribe (however I do strongly recommend subscribing to TryHackMe). The difference being that when you host the VM locally it will run faster and your brute forcing will occur quicker because you aren't tunneling through a VPN; however, you don't have the bonus of submitting the flags as you find them.
<p></p>
Mr Robot is a themed box on the TV show also called Mr Robot, it contains 3 flags (web flag, user flag and root flag) and has different ways of completing the box to achieve the same outcome. Doing this box you will cover the following areas:
<p></p>
<H5>Enumeration</H5>
- Network Scanning (Nmap)
<H5>Web Recon</H5>
- Recon (Nikto,gobuster,dirb,uniscan)
<br>
- Web directory traversal/file access
<H5>Breaking into a badly configured login page</H5>
- Setting up a dictionary file for username and password enumeration/cracking
<br>
- WordPress password cracking (wpscan,hydra)
<H5>Generating Backdoors</H5>
- Generate PHP Backdoor (Msfvenom)
<br>
- Upload and execute a backdoor
<H5>Catching the reverse conection on your host</H5>
- Reverse connection (Metasploit)
<H5>Identifying and cracking a hash</H5>
- Getting a hash and decrypting it
<H5>Getting a TTY shell</H5>
- Import python one-liner for proper TTY shell
<H5>Privilege Escalation</H5>
- One liner, WinPEAS,
<br>
- binary, dirtysock
<p></p>
All flags have the following flag format:
<p></p>
key-1-of-3.txt
<br>
And when opened look like a random string of characters eg. ABCDENOASFINEAUINFAOPDSNFRUENPANFD
<p></p>
As stated before one flag is located on the website itself, one is located in the user folder and the final is located in the root folder.
<p></p>
<H2>Locally Hosting the VM</H2>
<details>
    <summary></summary>
<p></p>
The first thing you need to do is download the mrRobot.ova file from <a href="https://www.vulnhub.com/entry/mr-robot-1,151/" rel="nofollow">VulnHub</a> (<a href="https://download.vulnhub.com/mrrobot/mrRobot.ova" rel="nofollow">Download link</a>).
<br>
Now that you have the .ova file you need open it in either <a href="https://www.virtualbox.org/" rel="nofollow">Virtual Box</a> or <a href="https://www.vmware.com/au/products/workstation-player.html" rel="nofollow">VMWare</a>.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/2429ae9e3f58140ed5905513114b710f0153067e/Starting_Point/VulnHub/MrRobot/images/open.png"><br>
</div>
<p></p>
This will then direct you to the import screen, give the VM a name and store it in a folder on your system somewhere.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/2429ae9e3f58140ed5905513114b710f0153067e/Starting_Point/VulnHub/MrRobot/images/import.png"><br>
</div>
<p></p>
This will then import the machine and you will be able to see it on the left hand panel once completed, the last thing you need to do is confirm the network settings. Below you can see that I have highlighted the network setting for the VM, it should be Host-only.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/2429ae9e3f58140ed5905513114b710f0153067e/Starting_Point/VulnHub/MrRobot/images/bridged.png"><br>
</div>
<p></p>
If it isn't set as Host-only you can click on the network which will bring you into network settings as displayed below.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/2429ae9e3f58140ed5905513114b710f0153067e/Starting_Point/VulnHub/MrRobot/images/network.png"><br>
</div>
<p></p>
You can now start your Mr Robot VM and let it run, you don't need to do anything further with it.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/2429ae9e3f58140ed5905513114b710f0153067e/Starting_Point/VulnHub/MrRobot/images/logon.png"><br>
</div>
<p></p>
We now need to make some changes to our penetration VM, looking at the settings we can see that there is only one network adapter that is set to NAT.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/17c7433328d45f62ef541af78f404d6556d848be/Starting_Point/VulnHub/MrRobot/images/penbox.png"><br>
</div>
<p></p>
We can add another adapter though the settings to do this right click on the VM name in the left panel.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/17c7433328d45f62ef541af78f404d6556d848be/Starting_Point/VulnHub/MrRobot/images/settings.png"><br>
</div>
<p></p>
Next we need to add another adapter, to do this we click on add at the bottom of the settings screen.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/17c7433328d45f62ef541af78f404d6556d848be/Starting_Point/VulnHub/MrRobot/images/add.png"><br>
</div>
<p></p>
We then click on Network Adapter then click finish.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/17c7433328d45f62ef541af78f404d6556d848be/Starting_Point/VulnHub/MrRobot/images/networkadapter.png"><br>
</div>
<p></p>
We can see that Network Adapter 2 has been created, we need to click on that adapter and select Host-only followed by ok.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/17c7433328d45f62ef541af78f404d6556d848be/Starting_Point/VulnHub/MrRobot/images/adapter2.png"><br>
</div>
<p></p>
Now that we have done this process we can start our penetration VM and begin the challenge. The reason that I do it this way is so that my penetration VM keeps internet connection and has a direct link to the target VM (mrRobot). If we put both VM's on Host-only our penetration VM would lose internet connectivity. 
<br>
We can confirm that the adapter is working by logging into our penetration VM and running the following command:
<p></p>

```
sudo ifconfig
```

<p></p>
Which returns something like this:
<p></p>

```
❯ sudo ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.191.129  netmask 255.255.255.0  broadcast 192.168.191.255
        inet6 fe80::11ec:b5d:f22:834f  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:df:18:d9  txqueuelen 1000  (Ethernet)
        RX packets 24843  bytes 33147054 (31.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 10896  bytes 920671 (899.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.125.134  netmask 255.255.255.0  broadcast 192.168.125.255
        inet6 fe80::744b:c7cf:2382:75d3  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:df:18:e3  txqueuelen 1000  (Ethernet)
        RX packets 5  bytes 875 (875.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 34  bytes 2534 (2.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 25070  bytes 14986245 (14.2 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 25070  bytes 14986245 (14.2 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

<p></p>
We can see that eth1 has been added as interface and it has an ip next to inet.
<p></p>
We can now begin hacking our target machine.
<p></p>
The process is basically identical for Virtual Box however I prefer using VMWare.

</details>


<hr>
<p></p>
<H2>Walkthrough</H2>
<p></p>
<details>
    <summary></summary>
<p></p>
Our first step in this box is to enumerate the network IOT locate the VM. We do this using <kbd>nmap</kbd> however there are some additional flags and steps we use. The command we will run first is:
<p></p>

```
nmap -e eth1 -T5 192.168.125.0/24
```

<p></p>
Looking at this command we used the <kbd>-e</kbd> flag to let nmap know which interface we want to use for the scan, the <kbd>-T5</kbd> flag tells nmap to do the fastest scan possible and the network we scanned against was the network we identified when we ran <kbd>ifconfig</kbd>.
<br>
This command returns:
<p></p>

```
❯ nmap -e eth1 -T5 192.168.125.0/24
Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-08 13:29 AEST
Nmap scan report for 192.168.125.132
Host is up (0.016s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE
22/tcp  closed ssh
80/tcp  open   http
443/tcp open   https

Nmap scan report for 192.168.125.134
Host is up (0.035s latency).
Not shown: 999 closed ports
PORT    STATE SERVICE
111/tcp open  rpcbind
```

<p></p>
Looking through the results we can identify our own ip address <kbd>192.168.125.134</kbd> and one other ip <kbd>192.168.125.132</kbd> we can therefore determine that the second ip is our target VM. Time for further enumeration with nmap. This time the command will look like this:
<p></p>

```
sudo nmap -e eth1 -A -vvv --script vuln -oN nmap.txt 192.168.125.132
```

<p></p>
For this command, we use the <kbd>-e</kbd> flag to direct nmap to our desired network interface, ,<kbd>-A</kbd> to run all scripts, <kbd>-vvv</kbd> to give very verbose output, <kbd>--script vuln</kbd> to run the vulnerability script against the machine, this is extremely helpful and does a lot of enumeration for us which we can see in the output, <kbd>-oN nmap.txt</kbd> outputs the scan to a text file se we can refer to it later and finally the ip address we wish to scan.
This command outputs the following:
<p></p>


```
# Nmap 7.91 scan initiated Thu Jul  8 13:41:13 2021 as: nmap -e eth1 -A -vvv --script vuln -oN nmap.txt 192.168.125.132
Nmap scan report for 192.168.125.132
Host is up, received arp-response (0.0019s latency).
Scanned at 2021-07-08 13:41:24 AEST for 94s
Not shown: 997 filtered ports
Reason: 997 no-responses
PORT    STATE  SERVICE  REASON         VERSION
22/tcp  closed ssh      reset ttl 64
80/tcp  open   http     syn-ack ttl 64 Apache httpd
| http-csrf: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=192.168.125.132
|   Found the following possible CSRF vulnerabilities: 
|     
|     Path: http://192.168.125.132:80/js/BASE_URL+%22/live/%22);this.firstBoot?(this.firstBoot=!1,this.track.omni("Email
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/js/BASE_URL+%22/live/%22);this.firstBoot?(this.firstBoot=!1,this.track.omni("Email
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/js/rs;if(s.useForcedLinkTracking||s.bcf){if(!s."+"forcedLinkTrackingTimeout)s.forcedLinkTrackingTimeout=250;setTimeout('if(window.s_c_il)window.s_c_il['+s._in+'].bcr()',s.forcedLinkTrackingTimeout);}else
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/js/rs;if(s.useForcedLinkTracking||s.bcf){if(!s."+"forcedLinkTrackingTimeout)s.forcedLinkTrackingTimeout=250;setTimeout('if(window.s_c_il)window.s_c_il['+s._in+'].bcr()',s.forcedLinkTrackingTimeout);}else
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/js/u;c.appendChild(o);'+(n?'o.c=0;o.i=setTimeout(f2,100)':'')+'}}catch(e){o=0}return
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/js/u;c.appendChild(o);'+(n?'o.c=0;o.i=setTimeout(f2,100)':'')+'}}catch(e){o=0}return
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/js/vendor/null,this.tags.length=0%7d,t.get=function()%7bif(0==this.tags.length)return
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/js/vendor/null,this.tags.length=0%7d,t.get=function()%7bif(0==this.tags.length)return
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/js/BASE_URL+%22/live/
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/js/BASE_URL+%22/live/
|     Form id: 
|     Form action: http://192.168.125.132/
|     
|     Path: http://192.168.125.132:80/wp-login.php
|     Form id: loginform
|_    Form action: http://192.168.125.132/wp-login.php
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum: 
|   /admin/: Possible admin folder
|   /admin/index.html: Possible admin folder
|   /wp-login.php: Possible admin folder
|   /robots.txt: Robots file
|   /feed/: Wordpress version: 4.3.1
|   /wp-includes/images/rss.png: Wordpress version 2.2 found.
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found.
|   /wp-includes/images/blank.gif: Wordpress version 2.6 found.
|   /wp-includes/js/comment-reply.js: Wordpress version 2.7 found.
|   /wp-login.php: Wordpress login page.
|   /wp-admin/upgrade.php: Wordpress login page.
|   /readme.html: Interesting, a readme.
|   /0/: Potentially interesting folder
|_  /image/: Potentially interesting folder
|_http-jsonp-detection: Couldn't find any JSONP endpoints.
|_http-litespeed-sourcecode-download: Request with null byte did not work. This web server might not be vulnerable
|_http-server-header: Apache
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
443/tcp open   ssl/http syn-ack ttl 64 Apache httpd
| http-csrf: 
| Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=192.168.125.132
|   Found the following possible CSRF vulnerabilities: 
|     
|     Path: https://192.168.125.132:443/js/BASE_URL
|     Form id: 
|     Form action: https://192.168.125.132:443/
|     
|     Path: https://192.168.125.132:443/js/BASE_URL
|     Form id: 
|     Form action: https://192.168.125.132:443/
|     
|     Path: https://192.168.125.132:443/js/vendor/null,this.tags.length=0%7d,t.get=function()%7bif(0==this.tags.length)return
|     Form id: 
|     Form action: https://192.168.125.132:443/
|     
|     Path: https://192.168.125.132:443/js/vendor/null,this.tags.length=0%7d,t.get=function()%7bif(0==this.tags.length)return
|     Form id: 
|     Form action: https://192.168.125.132:443/
|     
|     Path: https://192.168.125.132:443/js/u;c.appendChild(o);'+(n?'o.c=0;o.i=setTimeout(f2,100)':'')+'}}catch(e){o=0}return
|     Form id: 
|     Form action: https://192.168.125.132:443/
|     
|     Path: https://192.168.125.132:443/js/u;c.appendChild(o);'+(n?'o.c=0;o.i=setTimeout(f2,100)':'')+'}}catch(e){o=0}return
|     Form id: 
|     Form action: https://192.168.125.132:443/
|     
|     Path: https://192.168.125.132:443/js/rs;if(s.useForcedLinkTracking||s.bcf){if(!s."
|     Form id: 
|     Form action: https://192.168.125.132:443/
|     
|     Path: https://192.168.125.132:443/js/rs;if(s.useForcedLinkTracking||s.bcf){if(!s."
|     Form id: 
|     Form action: https://192.168.125.132:443/
|     
|     Path: https://192.168.125.132:443/wp-login.php
|     Form id: loginform
|_    Form action: https://192.168.125.132:443/wp-login.php
|_http-dombased-xss: Couldn't find any DOM based XSS.
| http-enum: 
|   /admin/: Possible admin folder
|   /admin/index.html: Possible admin folder
|   /wp-login.php: Possible admin folder
|   /robots.txt: Robots file
|   /feed/: Wordpress version: 4.3.1
|   /wp-includes/images/rss.png: Wordpress version 2.2 found.
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found.
|   /wp-includes/images/blank.gif: Wordpress version 2.6 found.
|   /wp-includes/js/comment-reply.js: Wordpress version 2.7 found.
|   /wp-login.php: Wordpress login page.
|   /wp-admin/upgrade.php: Wordpress login page.
|   /readme.html: Interesting, a readme.
|   /0/: Potentially interesting folder
|_  /image/: Potentially interesting folder
|_http-jsonp-detection: Couldn't find any JSONP endpoints.
|_http-litespeed-sourcecode-download: Request with null byte did not work. This web server might not be vulnerable
|_http-server-header: Apache
|_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
|_sslv2-drown: 
MAC Address: 00:0C:29:43:37:0E (VMware)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.10 - 4.11
TCP/IP fingerprint:
OS:SCAN(V=7.91%E=4%D=7/8%OT=80%CT=22%CU=%PV=Y%DS=1%DC=D%G=N%M=000C29%TM=60E
OS:67442%P=x86_64-pc-linux-gnu)SEQ(SP=105%GCD=1%ISR=108%TI=Z%CI=I%II=I%TS=8
OS:)OPS(O1=M5B4ST11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O5=M5B
OS:4ST11NW7%O6=M5B4ST11)WIN(W1=7120%W2=7120%W3=7120%W4=7120%W5=7120%W6=7120
OS:)ECN(R=Y%DF=Y%TG=40%W=7210%O=M5B4NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%TG=40%S=O%A=
OS:S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%TG=40%W=0%S=A%A=Z%F=R%O=%RD=0%
OS:Q=)T5(R=Y%DF=Y%TG=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%TG=40%W=0%
OS:S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=N)U1(R=N)IE(R=Y%DFI=N%TG=40%CD=S)

Uptime guess: 0.072 days (since Thu Jul  8 11:59:55 2021)
Network Distance: 1 hop
TCP Sequence Prediction: Difficulty=261 (Good luck!)
IP ID Sequence Generation: All zeros

TRACEROUTE
HOP RTT     ADDRESS
1   1.89 ms 192.168.125.132

Read data files from: /usr/bin/../share/nmap
OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Thu Jul  8 13:42:58 2021 -- 1 IP address (1 host up) scanned in 105.32 seconds
```

<p></p>
Looking through the results we can see a lot of interesting information and a lot of enumeration that has been done for us just by using the vuln script.
<br>
We can see there are 3 ports, 22 (ssh), 80 (http), 443 (https).
<br>
The http enumeration is also extremely helpful as it has carried out the job of gobuster or dirb.
<p></p>

```
| http-enum: 
|   /admin/: Possible admin folder
|   /admin/index.html: Possible admin folder
|   /wp-login.php: Possible admin folder
|   /robots.txt: Robots file
|   /feed/: Wordpress version: 4.3.1
|   /wp-includes/images/rss.png: Wordpress version 2.2 found.
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found.
|   /wp-includes/images/blank.gif: Wordpress version 2.6 found.
|   /wp-includes/js/comment-reply.js: Wordpress version 2.7 found.
|   /wp-login.php: Wordpress login page.
|   /wp-admin/upgrade.php: Wordpress login page.
|   /readme.html: Interesting, a readme.
|   /0/: Potentially interesting folder
|_  /image/: Potentially interesting folder
```

<p></p>
From this output we can see our website enumeration is done and we can see that there are a couple of files and a wordpress hosted.
<p></p>
Since we ran the script to do enumeration for us we don't need to carry out any further enumeration ie. gobuster, dirb, uniscan however I will include them for your information below.
<p></p>



<details>
    <summary>Enumeration</summary>
<p></p>
We will start with uniscan:
<p></p>
Uniscan is a default scanner that comes with ParrotOS and kali, it has a very simple GUI interface:
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/acd6c27914258430c5418614a4129b71f690d48d/Starting_Point/VulnHub/MrRobot/images/uniscan.png"><br>
</div>
<p></p>
You can see that i have imputed the target ip address and selected the following options:
<br>
- Check Directory
- Check Files
- Check /robots.txt
- Dynamic tests
- Static tests
- Web Fingerprint
- Server Fingerprint




</details>







</details>