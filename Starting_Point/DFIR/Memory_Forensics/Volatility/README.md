<H1>Volatility</H1>
<hr>

<H2>What is volatility?</H2>
<p>
Volatility is an extremely powerful program that allows us to analyse runtime state of a system using the data found in volatile storage (RAM). 
<br>
Volatility is one of the best open source software programs for analyzing RAM in 32 bit/64 bit systems. It supports analysis for Linux, Windows, Mac, and Android systems. It is based on Python and can be run on Windows, Linux, and Mac systems. It can analyze raw dumps, crash dumps, VMware dumps (.vmem), virtual box dumps, and many others.
</p>
<hr>

<H2>Where do I Start?</H2>
Volatility has a ton of options (flags) that can be used and can be overwhelming at the start.
But once you work out a general road-map you can work through an analysis methodically. Once you have your initial foothold you can start searching for the information you need.
If at any stage you dont know what command to run you can use the <kbd>-h</kbd> flag to print a list of commands you can use.
We will use a series of challenges from the CSC to work through IOT practice the use of volatility.
<hr>

<H2>Getting your Foothold</H2>
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

<H2>First Challenge</H2>
