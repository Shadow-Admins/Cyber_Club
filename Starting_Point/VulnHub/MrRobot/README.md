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
Now that we have done initial enumeration we can start looking at the website being hosted.
<p></p>







</details>