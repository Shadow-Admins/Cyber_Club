<H1>Mr Robot</H1>
<p></p>
<H2>What is it?</H2>
<p></p>
Mr Robot is a vulnerable VM that can be downloaded from <a href="https://www.vulnhub.com/entry/mr-robot-1,151/" rel="nofollow">VulnHub</a> so that you can host it locally or you can complete it on <a href="https://tryhackme.com/room/mrrobot" rel="nofollow">TryHackMe</a>. Mr Robot is a free room on TryHackMe so anyone can do it without needing to subscribe (however I do strongly recommend subscribing to TryHackMe). The difference being that when you host the VM locally it will run faster and your brute forcing will occur quicker because you aren't tunneling through a VPN; however, you don't have the bonus of submitting the flags as you find them.
<p></p>
Mr Robot is a themed box on the TV show also called Mr Robot, it contains 3 flags (web flag, user flag and root flag) and has different ways of completing the box to achieve the same outcome. Doing this box you will cover the following areas:
<p></p>
<H3>Enumeration</H3>
<br>
- Network Scanning (Nmap)
<br>
<H4>Web Recon</H4>
<br>
- Recon (Nikto,gobuster,dirb,uniscan)
<br>
- Web directory traversal/file access
<br>
<H3>Breaking into a badly configured login page</H3>
<br>
- Setting up a dictionary file for username and password enumeration/cracking
<br>
- WordPress password cracking (wpscan,hydra)
<br>
<H3>Generating Backdoors</H3>
<br>
- Generate PHP Backdoor (Msfvenom)
<br>
- Upload and execute a backdoor
<br>
<H3>Catching the reverse conection on your host</H3>
<br>
- Reverse connection (Metasploit)
<br>
<H3>Identifying and cracking a hash</H3>
<br>
- Getting a hash and decrypting it
<br>
<H3>Getting a TTY shell</H3>
<br>
- Import python one-liner for proper TTY shell
<br>
<H3>Privilege Escalation</H3>
<br>
- One liner, WinPEAS,
<br>
- binary, dirtysock


<br>
- Memory Forensics
<br>
- Forensics
<br>
- Network
<br>
- Stenography
<p></p>
All challenges have the following flag format:
<p></p>
CTF{ANSWER}
<p></p>
Bellow are write-ups for all challenges.
<p></p>
<details>
    <summary>Challenges</summary>
<p></p>
<details>
    <summary>Reverse Engineering</summary>
<p></p>
<details>
    <summary>
















</details>