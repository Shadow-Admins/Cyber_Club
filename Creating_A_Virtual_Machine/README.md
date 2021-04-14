<H1>Creating A Virtual Machine</H1>
<br>
<H2><b><u>What is a VM and why use one?</u></b></H2>

<p>
<H3>What is a VM?</H3>
A virtual machine, commonly shortened to just VM, is no different than any other physical computer like a laptop, smart phone, or server. It has a CPU, memory, disks to store your files, and can connect to the internet if needed. While the parts that make up your computer (called hardware) are physical and tangible, VMs are often thought of as virtual computers or software-defined computers within physical servers, existing only as code.
</p>

<p>
<H3>Why use a VM?</H3>
- A VM allows you to run multiple OS (Operating System) on the one computer without needing to dedicate one computer to a specific OS (also known as 'on the metal').
<br>
- It is likely when starting out, and even later on you will break the system (its going to happen)... this will happen because dependancies will break or something will just go wrong. If you have your OS running in a VM you can revert it back to before it broke saving a lot of time.
<br>
- A VM can also be isolated from the rest of your system, so if you are working on a malicious piece of code it won't destroy the rest of your system.
</p>
<hr>

<H2>Virtulization Software</H2>
<p>
  <H3>What is Virtulization Software?</H3>
  Virtulization Software is the software that allows you to run an OS within a container on your normal OS (think of it as a box in a box). It allows the virtulized OS to utilise the hardware (think CPU, GPU, NIC) of your computer while isolating it from the system. Depending on whether you use Windows or Mac will depend on the software you use.
</p>
<p>
  <details>
    <summary>Windows</summary>
			<H4>Virtual Box</H4>
				Oracle VM VirtualBox is a cross-platform virtualization application. What does that mean? For one thing, it installs on your existing Intel or AMD-based computers, whether they are running Windows, Mac OS X, Linux, or Oracle Solaris operating systems (OSes). Secondly, it extends the capabilities of your existing computer so that it can run multiple OSes, inside multiple virtual machines, at the same time. As an example, you can run Windows and Linux on your Mac, run Windows Server 2016 on your Linux server, run Linux on your Windows PC, and so on, all alongside your existing applications. You can install and run as many virtual machines as you like. The only practical limits are disk space and memory. 
		<br>
		https://www.virtualbox.org/wiki/VirtualBox
			<H4>VMware Workstation Player</H4>
					VMware Workstation Player allows you to run a second, isolated operating system on a single PC.
		<br>
		https://www.vmware.com/au/products/workstation-player.html
  </details>

  <details>
    <summary>Mac</summary>
			<H4>Virtual Box</H4>
				Oracle VM VirtualBox is a cross-platform virtualization application. What does that mean? For one thing, it installs on your existing Intel or AMD-based computers, whether they are running Windows, Mac OS X, Linux, or Oracle Solaris operating systems (OSes). Secondly, it extends the capabilities of your existing computer so that it can run multiple OSes, inside multiple virtual machines, at the same time. As an example, you can run Windows and Linux on your Mac, run Windows Server 2016 on your Linux server, run Linux on your Windows PC, and so on, all alongside your existing applications. You can install and run as many virtual machines as you like. The only practical limits are disk space and memory. 
		<br>
		https://www.virtualbox.org/wiki/VirtualBox
			<H4>VMware Fusion</H4>
				VMware Fusion Pro and VMware Fusion Player Desktop Hypervisors give Mac users the power to run Windows on Mac along with hundreds of other operating systems, containers or Kubernetes clusters, side by side with Mac applications, without rebooting.
		<br>
		https://www.vmware.com/au/products/fusion.html
  </details>
</p>
<hr>

<H2><b><u>Distros... Which one?</u></b></H2>
<p>
The first thing you need to do is pick a distribution (distro) that you like and that suites you, they are nearly all the same bar some nuances. You will naturally gravitate towards one or use different ones for different tasks. I would start with Kali or Parrot as they are well known and functional. The other distros on the list are either more task specific or CLI (Command Line Input) only (note this list isnt exhaustive and there are hundreds of distros out there, these are just the well known ones). Try them all and see which you prefer.
</p>
<hr>

<details>
  <summary>Click the <kbd>arrow</kbd> for a list of distros and descriptions.</summary>

<H3>Kali</H3>
         
Kali Linux is an open-source, Debian-based Linux distribution geared towards various information security tasks, such as Penetration Testing, Security Research, Computer Forensics and Reverse Engineering.
<br>
https://www.kali.org/
<br>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Creating_A_Virtual_Machine/images/kali.png" width="600"><br>
</div>
<hr>

<H3>Parrot</H3>
Parrot OS, the flagship product of Parrot Security is a GNU/Linux distribution based on Debian and designed with Security and Privacy in mind. It includes a full portable laboratory for all kinds of cyber security operations, from pentesting to digital forensics and reverse engineering, but it also includes everything needed to develop your own software or keep your data secure.
<br>
https://www.parrotsec.org/
<br>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Creating_A_Virtual_Machine/images/Parrot.jpg" width="600"><br>
</div>
<hr>

<H3>Tsurugi</H3>
Tsurugi Linux is a DFIR (Digital Forensics & Incident Response) Linux distro. It comes out of the box with many DFIR tools with the enviroment for them to work in harmony without breaking. It allows forensics on all system file types which you often cant do without difficulty on other distros.
<br>
https://tsurugi-linux.org/index.php
<br>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Creating_A_Virtual_Machine/images/Tsurugi.png" width="600"><br>
</div>
<hr>

<H3>Black Arch</H3>
BlackArch Linux is an Arch Linux-based penetration testing distribution for penetration testers and security researchers. The repository contains 2670 tools. You can install tools individually or in groups.
<br>
https://blackarch.org/
<br>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Creating_A_Virtual_Machine/images/black_arch.png" width="600"><br>
</div>
<hr>

<H3>SIFT</H3>
The SIFT Workstation is a group of free open-source incident response and forensic tools designed to perform detailed digital forensic examinations in a variety of settings. It can match any current incident response and forensic tool suite. SIFT demonstrates that advanced incident response capabilities and deep dive digital forensic techniques to intrusions can be accomplished using cutting-edge open-source tools that are freely available and frequently updated.
<br>
https://digital-forensics.sans.org/community/downloads
<br>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Creating_A_Virtual_Machine/images/sift.png" width="600"><br>
</div>
<hr>

<H3>Make Your Own</H3>
You can start with a barebones distro such as debian, ubuntu or arch and install the tools you require on them as you need them. The above distros are basically done for you with tools already installed.
<hr>

</details>
