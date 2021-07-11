<H1>VulnHub</H1>
<p></p>
<div align="center">
<img src="https://github.com/Shadow-Admins/Cyber_Club/blob/ea3cf3e76b754f9e3979be6323b2c4e83380db82/Starting_Point/VulnHub/images/vulnhub.png"><br>
</div>
<p></p>
<H2>What is it?</H2>
<p></p>
<a href="https://www.vulnhub.com/" rel="nofollow">VulnHub</a> is a free platform that hosts vulnerable Virtual Machines (VM) images. These images can be downloaded and hosted locally on your own machine IOT practice hacking.
<p></p>
Below is a guide on how to host a VM locally on your machine.
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