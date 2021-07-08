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
<br>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/56b950dbe5882e192d4736c6eb9cf2feddcdccc4/Starting_Point/VulnHub/MrRobot/images/open.png"><br>
</div>
<p></p>
This will then direct you to the import screen, give the VM a name and store it in a folder on your system somewhere.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/b5ce8bd80aeae75809411b402e4912ebb4af4e93/Starting_Point/VulnHub/MrRobot/images/import.png"><br>
</div>
<p></p>
This will then import the machine and you will be able to see it on the left hand panel once completed, the last thing you need to do is confirm the network settings. Below you can see that I have highlighted the network setting for the VM, it should be bridged.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/9c8d067e1e74ed9477deaf36fcd537aab721cf73/Starting_Point/VulnHub/MrRobot/images/bridged.png"><br>
</div>
<p></p>
If it isn't set as bridged you can click on the network which will bring you into network settings as displayed below.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/9c8d067e1e74ed9477deaf36fcd537aab721cf73/Starting_Point/VulnHub/MrRobot/images/network.png"><br>
</div>
<p></p>
You can now start your Mr Robot VM and your penetration VM and start hacking.

</details>

<details>
    <summary>Reverse Engineering</summary>
<p></p>
<details>
    <summary>
















</details>