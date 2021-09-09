<H1>Reversing ELF</H1>
<p></p>
Reversing ELF is an introduction to reverse engineering ELF binaries hosted on <a href="https://tryhackme.com/room/reverselfiles" rel="nofollow">Try Hack Me</a>
<p></p>
All Challenge Files: <a href="https://drive.google.com/file/d/1Ia8cUnWHgCGtAKBznp-Q6xTKOCmxB5t8/view?usp=sharing" rel="nofollow">Challenge Files</a>
<p></p>
For the walkthroughs found bellow I will first run through the challenges using GDB/Ghidra followed by how to use R2 (Radare2) to do the challenge. As this is an introduction its important for you to find a tool that you like as you continue to learn RE. Note there are many other tools that you can use for RE these are just three that I will demonstrate.
<p></p>
<hr>
<p></p>
<H2>Getting Set Up<H2>
<details>
    <summary></summary>
<p></p>
The first thing we will go through is ensuring we have the tools we need to carry out the challenges.
<p></p>
To start with we will install Ghidra (new releases of Kali linux come with Ghidra pre installed)
<p></p>
<H3>Ghidra</H3> 
<p></p>
https://ghidra-sre.org/
<p></p>
Ghidra is a software reverse engineering (SRE) framework created and maintained by the National Security Agency Research Directorate. This framework includes a suite of full-featured, high-end software analysis tools that enable users to analyze compiled code on a variety of platforms including Windows, macOS, and Linux. Capabilities include disassembly, assembly, decompilation, graphing, and scripting, along with hundreds of other features. Ghidra supports a wide variety of processor instruction sets and executable formats and can be run in both user-interactive and automated modes. Users may also develop their own Ghidra extension components and/or scripts using Java or Python.
<p></p>
Navigating to the above site takes us to Ghidra's home page, from here we need to click on the "Download from GitHub" link.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/Reversing/Challenges/Try_Hack_Me/Reversing_ELF/images/ghidra_webpage.png"><br>
</div>
<p></p>
Clicking on this link takes us to the Ghidra GitHub "Releases" page, at the time of this write up you can see that the latest Ghidra version is '10.0.2'. From here you want to download the .zip file in my case 'ghidra_10.0.2_PUBLIC_20210804.zip'. (Whenever I install or download new tools I always use the '/opt' directory, this is actually what this directory is for.)
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/Reversing/Challenges/Try_Hack_Me/Reversing_ELF/images/ghidra_releases.png"><br>
</div>
<p></p>
Now that you have downloaded the zip folder you can either use the GUI to unzip and extract the contents or use the command line:
<p></p>

```
unzip -d ghidra_10.0.2_PUBLIC_20210804.zip
```

<p></p>
Once you have extracted the folder you can 'cd' into the directory and you can see that there is a shell script called 'ghidraRun'. using the following command you can run Ghidra.
<p></p>

```
./ghidraRun
```

<p></p>
<details>
    <summary>Optional: adding an alias so you can run Ghidra anywhere</summary>
<p></p>
To add an alias so you can run ghidra from anywhere you will do the following.
<p></p>
Navigate to your home directory indicated by the <kbd>~</kbd> symbol (this can be easy done simply by entering <kbd>cd</kbd> with no directory listed).
Once you are in your home directory you need to list all files which can be done by entering <kbd>ls -a</kbd>. Depending on your flavour of linux you will see a fair few files (you can see my directory listing below).
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/Reversing/Challenges/Try_Hack_Me/Reversing_ELF/images/home_directory.png"><br>
</div>
<p></p>
The file you are looking for is either '.zshrc' or '.bashrc' (your shell configuration files) depending on what you are currently using (hopefully you are using .zsh by now, if you would like to know the differences between the two shells check out this <a href="https://linuxhint.com/differences_between_bash_zsh/" rel="nofollow">site</a>. You can tell that my terminal probably looks significantly different to yours, thats because I am using Z shell with oh-my-zsh, p10k and colourls).
<p></p>
Once you have found the 'rc' file you need use a terminal editor (nano, vim) to edit it. I will use <kbd>nano</kbd> using the following command:
<p></p>

```
nano .zshrc
```

<p></p>
Now that we are editing the file we need to scroll until we find the 'alias' section. You can see mine bellow:






</details>



</details>

<H2>Challenges</H2>
<details>
    <summary></summary>
<p></p>
<details>
    <summary>Crackme1</summary>
<p></p>
The first challenge we are given is:









</details>





</details>