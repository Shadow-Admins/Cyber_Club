<H1>Reversing ELF</H1>
<p></p>
Reversing ELF is an introduction to reverse engineering ELF binaries hosted on <a href="https://tryhackme.com/room/reverselfiles" rel="nofollow">Try Hack Me</a>
<p></p>
All Challenge Files: <a href="https://drive.google.com/file/d/1Ia8cUnWHgCGtAKBznp-Q6xTKOCmxB5t8/view?usp=sharing" rel="nofollow">Challenge Files</a>
<p></p>
For the walkthroughs found bellow I will first run through the challenges using GDB/Ghidra followed by how to use R2 (Radare2) to do the challenge. As this is an introduction its important for you to find a tool that you like as you continue to learn RE. Note there are many other tools that you can use for RE these are just three that I will demonstrate.
<p></p>
<details>
    <summary>Getting Set Up</summary>
<p></p>
The first thing we will go through is ensuring we have the tools we need to carry out the challenges.
<p></p>
The first tool we will install is Ghidra (new releases of Kali linux come with Ghidra pre installed)
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