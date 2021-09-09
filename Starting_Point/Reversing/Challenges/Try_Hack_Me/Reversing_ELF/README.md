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
<H2>Getting Set Up</H2>
<details>
    <summary></summary>
<p></p>
The first thing we will go through is ensuring we have the tools we need to carry out the challenges.
<p></p>
To start with we will install Ghidra (new releases of Kali linux come with Ghidra pre installed)
<p></p>
<H3>Ghidra</H3> 
<p></p>
<a href="https://ghidra-sre.org/" rel="nofollow">https://ghidra-sre.org/</a>
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
<H3>OPTIONAL: adding an alias so you can run Ghidra anywhere</H3>
<details>
    <summary></summary>
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
Now that we are editing the file we need to scroll until we find the 'alias' section. You can see mine bellow.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/Reversing/Challenges/Try_Hack_Me/Reversing_ELF/images/zshrc_alias.png"><br>
</div>
<p></p>
Now that you have found where your aliases are stored you need to add a line at the bottom, you can see my alias I have created but will be dependant on the version of ghidra you have.
<p></p>

```
alias ghidra='sudo /opt/<YOUR_GHIDRA_FOLDER>/ghidraRun'
```

<p></p>
Once you have entered this line you can exit and save .zshrc (if youre using nano the command is: <kbd>Ctrl+x</kbd> then <kbd>y</kbd> to save finally <kbd>Enter</kbd> to save as the current name '.zshrc')
<p></p>
Now that you have updated your '.zshrc' or '.bashrc' file you now need to tell your terminal to use this updated file as the 'source' we do this through the following command or by exiting your terminal and starting a new terminal.       ghidraRun 
<p></p>

```
source .zshrc
```

<p></p>
You have now sucessfully added a persistant alias to your shell config file. This alias will stay regardless of shutdown/restart unlike using command line to set a temporary alias.
<p></p>
Regardless of where you are located in your system now you can enter <kbd>ghidra</kbd> and it will run!
</details>
<p></p>
<hr>
<p></p>
<H3>Radare2 (r2)</H3>
<p></p>
The next program we will install is Radare2 commonly known as 'r2'.
<p></p>
<a href="https://github.com/radareorg/radare2" rel="nofollow">https://github.com/radareorg/radare2</a>
<p></p>
r2 is a rewrite from scratch of radare. It provies a set of libraries, tools and plugins to ease reverse engineering tasks.
<p></p>
The radare project started as a simple command-line hexadecimal editor focused on forensics, over time more features were added to support a scriptable command-line low level tool to edit from local hard drives, kernel memory, programs, remote gdb servers and be able to analyze, emulate, debug, modify and disassemble any binary.
<p></p>
Navigating to the above link will take you to the r2 GitHub page. Scrolling down you can see the install instructions.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/Reversing/Challenges/Try_Hack_Me/Reversing_ELF/images/radre2_github.png"><br>
</div>
<p></p>
This is an easy program to install. First we <kbd>cd /opt</kbd>, then we enter the listed commands:
<p></p>

```
git clone https://github.com/radareorg/radare2
radare2/sys/install.sh
```

<p></p>
This will clone the git repository of r2 into your '/opt' directory then run the install script.
<p></p>
You have now installed r2, you can run it from anywhere by entering 'r2' into your command line.
<p></p>
<hr>
<p></p>
<H3>GDB</H3>
<p></p>
<a href="https://www.gnu.org/software/gdb/" rel="nofollow">https://www.gnu.org/software/gdb/</a>
<p></p>
The final program we will check to see if its intalled (if you're using linux it is likely already installed) and if it isnt we will go through the process of installing it.
<p></p>
To check if GDB is installed enter the following command:
<p></p>

```
# gdb
```

<p></p>
Which should output:
<p></p>

```
# gdb                                                                                                                                         ⇣5.97 KiB/s ⇡0.61 KiB/s 192.168.191.128   ─╯
GNU gdb (Debian 10.1-1.7) 10.1.90.20210103-git
Copyright (C) 2021 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word".
GEF for linux ready, type `gef' to start, `gef config' to configure
96 commands loaded for GDB 10.1.90.20210103-git using Python engine 3.9
[+] Configuration from '/home/parrot/.gef.rc' restored
gef➤  
```

<p></p>
Use <kbd>q</kbd> to exit GDB if it runs.
(note: you can see that my gdb command input says 'gef' thats because I have the 'gef' plugin installed) If you see an output like the one above, GDB is already installed and you dont need to do anything. If you get a return such as:
<p></p>

```
# gdb
zsh: command not found: gdb
```

<p></p>
You will need to install gdb, luckily this is very easy to do and can be done through the command line using the following commands:
<p></p>

```
$ sudo apt-get install libc6-dbg gdb valgrind 
```

<p></p>
Once that completes attempt to run GDB again and you should be ready to go!
</details>
<p></p>
<hr>
<p></p>
<H2>Challenges</H2>
<details>
    <summary></summary>
<p></p>
<details>
    <summary>Crackme1</summary>
<p></p>
The first challenge we are given is:
<p></p>
Let's start with a basic warmup, can you run the binary?
<p></p>
What is the flag?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
This challenge is simple, and is an introduction to 'file permissions' to start with if you are using the zip file I provided you need to unzip it, we do this in the command line using the following command:
<p></p>

```
unzip Reverse_ELF.zip
```

<p></p>
Once you have done this we use 'long list' to view the file permissions.
<p></p>

```
ls -l
  rw-r--r--   1   parrot   parrot      7 KiB   Thu Sep  2 09:29:02 2021    crackme1 
  rw-r--r--   1   parrot   parrot      5 KiB   Thu Sep  2 09:29:12 2021    crackme2 
  rw-r--r--   1   parrot   parrot      9 KiB   Thu Sep  2 09:29:18 2021    crackme3 
  rw-r--r--   1   parrot   parrot      8 KiB   Wed Aug 25 13:53:44 2021    crackme4 
  rw-r--r--   1   parrot   parrot      8 KiB   Thu Sep  2 09:29:26 2021    crackme5 
  rw-r--r--   1   parrot   parrot      8 KiB   Thu Sep  2 09:29:34 2021    crackme6 
  rw-r--r--   1   parrot   parrot      6 KiB   Thu Sep  2 09:29:40 2021    crackme7 
  rw-r--r--   1   parrot   parrot      5 KiB   Thu Sep  2 09:11:54 2021    crackme8 
  rw-r--r--   1   parrot   parrot     24 KiB   Thu Sep  2 09:53:41 2021    Reverse_Elf.zip 
  rwxrwxrwx   1   parrot   parrot      4 KiB   Sat Sep  4 13:14:45 2021    tasks.txt 
```

<p></p>
The important thing to note is the first column which exaplins the file permissions of each file.
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/main/Starting_Point/Reversing/Challenges/Try_Hack_Me/Reversing_ELF/images/file-permission-syntax-explained.jpg"><br>
</div>
<p></p>
Here we can see that the files have read & write for the 'user' then read for 'group' and 'others. What we need to do is give execute permissions to to the crackme1 binary (we can go a step further and give execute permissions to all files at once) To do this we need to use the following command:
<p></p>

```
chmod +x crackme*
```

<p></p>
What this command is doing is adding the <kbd>x</kbd> flag, the <kbd>crackme*</kbd> is using the <kbd>*</kbd> wildcard to say apply this to everyfile starting with 'crackme' regardless of what comes after that ie. 1,2,3 etc. if you dont do it this way you can allow each binary to execute as you get to it ie <kbd>chmod +x crackme3</kbd>.
<p></p>
If we 'long list' again we can see the changes have occured.
<p></p>

```
ls -l
  rwxr-xr-x   1   parrot   parrot      7 KiB   Thu Sep  2 09:29:02 2021    crackme1 
  rwxr-xr-x   1   parrot   parrot      5 KiB   Thu Sep  2 09:29:12 2021    crackme2 
  rwxr-xr-x   1   parrot   parrot      9 KiB   Thu Sep  2 09:29:18 2021    crackme3 
  rwxr-xr-x   1   parrot   parrot      8 KiB   Wed Aug 25 13:53:44 2021    crackme4 
  rwxr-xr-x   1   parrot   parrot      8 KiB   Thu Sep  2 09:29:26 2021    crackme5 
  rwxr-xr-x   1   parrot   parrot      8 KiB   Thu Sep  2 09:29:34 2021    crackme6 
  rwxr-xr-x   1   parrot   parrot      6 KiB   Thu Sep  2 09:29:40 2021    crackme7 
  rwxr-xr-x   1   parrot   parrot      5 KiB   Thu Sep  2 09:11:54 2021    crackme8 
  rw-r--r--   1   parrot   parrot     24 KiB   Thu Sep  2 09:53:41 2021    Reverse_Elf.zip 
  rwxrwxrwx   1   parrot   parrot      4 KiB   Sat Sep  4 13:14:45 2021    tasks.txt 
```

<p></p>
Now that we have made the binary executable we can run it using the following command.
<p></p>

```
./crackme1
flag{not_that_kind_of_elf}
```

<p></p>
Which prints the flag for us, giving us the answer.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
flag{not_that_kind_of_elf}
</details>
</details>
</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>Crackme2</summary>
<p></p>
The second challenge we are given is:
<p></p>
Find the super-secret password! and use it to obtain the flag
<p></p>
What is the super secret password?
<p></p>
What is the flag?
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
For this challenge we need to obtain 2 peices of information. This is still very early in the series and focuses on very basic things, to do this challenge we need to run strings on the program to see what strings of text are visable. But to start we will attempt to run the binary.
<p></p>

```
./crackme2
Usage: ./crackme2 password
```

<p></p>
We can see that the binary needs a password argument passed to it when its run, we can try this by passing 'test' to the binary.
<p></p>

```
./crackme2 test
Access denied.
```

<p></p>
Now we know how the binary functions we can start interogating it. To start we will run <kbd>strings</kbd> on the binary. You can see the output below.
<p></p>

```
strings crackme2

/lib/ld-linux.so.2
libc.so.6
_IO_stdin_used
puts
printf
memset
strcmp
__libc_start_main
/usr/local/lib:$ORIGIN
__gmon_start__
GLIBC_2.0
PTRh 
j3jA
[^_]
UWVS
t$,U
[^_]
Usage: %s password
super_secret_password
Access denied.
Access granted.
;*2$"(
GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.9) 5.4.0 20160609
crtstuff.c
__JCR_LIST__
deregister_tm_clones
__do_global_dtors_aux
completed.7209
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
conditional1.c
giveFlag
__FRAME_END__
__JCR_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
strcmp@@GLIBC_2.0
_ITM_deregisterTMCloneTable
__x86.get_pc_thunk.bx
printf@@GLIBC_2.0
_edata
__data_start
puts@@GLIBC_2.0
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_start_main@@GLIBC_2.0
__libc_csu_init
memset@@GLIBC_2.0
_fp_hw
__bss_start
main
_Jv_RegisterClasses
__TMC_END__
_ITM_registerTMCloneTable
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rel.dyn
.rel.plt
.init
.plt.got
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.jcr
.dynamic
.got.plt
.data
.bss
.comment
```

<p></p>
Looking through the output you should note these lines:
<p></p>

```
UWVS
t$,U
[^_]
Usage: %s password
super_secret_password
Access denied.
Access granted.
```

<p></p>
Here we can see what looks like a password and we can now pass that to the binary in the command line.
<p></p>

```
./crackme2 super_secret_password
Access granted.
flag{if_i_submit_this_flag_then_i_will_get_points}
```

<p></p>
We can see that the password was accepted and that the flag was returned.
<p></p>
<details>
    <summary>Answer</summary>
<p></p>
What is the password?
super_secret_password

What is the flag?
flag{if_i_submit_this_flag_then_i_will_get_points}
</details>




</details>



up


















</details>

























<details>
    <summary>GDB/Ghidra</summary>
<p></p>


</details>
<p></p>
<hr>
<p></p>
<details>
    <summary>r2</summary>
<p></p>


</details>




</details>





</details>