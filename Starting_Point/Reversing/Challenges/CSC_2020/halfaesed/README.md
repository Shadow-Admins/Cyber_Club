<H1>halfaesed</H1>
<p></p>
halfaesed was a 400pt reversing challenge from the CSC 2020.
<p></p>
<H2>Challenge</H2>
<details>
    <summary></summary>
<p></p>
Last week, the network forensic team decrypted some suspicious encrypted communications to an
external C2 server, by analyzing the full packet capture data from one of your network monitoring
sensors. Of course, the security operations team then blocked the C2 domain, so that any other
compromised hosts would not be able to communicate with it.
<p></p>
The endpoint forensic team, of which you are a member, examined a forensic image of the hard disk
from the compromised workstation. Unfortunately, you were unable to find the malware that sent
those messages from the compromised host, and you were unable to determine the initial compromise
vector. It would appear the attackers thoroughly cleaned up after themselves. "Or maybe you just
need to try harder! HAHAHA!" said the network forensic team at drinks on Friday after work.
Network team thinks they're so good.
<p></p>
The compromised workstation was replaced with another, freshly built from a gold image. You also
made sure that IT installed an endpoint security agent that records file write events, process
creation events, and network connection events. If it gets compromised again in the same way, maybe
the agent event logs will help you find how it was compromised. That will show those stuck-up
network forensic analysts.
<p></p>
It is not even lunchtime on the following Monday, when it happens again. The security operations
team notifies you and the network forensics team of an alert on some suspicious traffic, coming
from the replacement workstation given to the original user. The C2 domain is different to last
week's C2 domain, but there are some structural similarities in the traffic that suggest it could
be the same threat actors. The race is on!
<p></p>
You acquire the agent event buffer from the replacement host, and soon locate a network connection
event with IP addresses and ports that match the alert. The network connection agent event logs
state that this connection was initiated by a process named "halfaesed". You check the process
agent event logs, and find the corresponding process ID. This log entry tells you that the full
path of the executable file for that process was "/tmp/.data/tluvgw/halfaesed". You quickly try
to acquire "/tmp/.data/tluvgw/halfaesed" from the endpoint, but... it's gone. Another look at the
process agent event logs shows why. Just 5 minutes later the command "shred -fzu /tmp/.data/*"
was executed by the attackers. So that is (probably) why you couldn't find the malware last week.
<p></p>
However this week, the endpoint agent has recorded the MD5 hash of the file, the first 64 bytes,
and which process wrote "/tmp/.data/tluvgw/halfaesed" to disk in the first place. It was "firefox"
the standard browser on the gold image. And the agent also recorded the URL that the user visited
immediately before the malware was written, which was the URL for the enterprise email application
web interface. File write agent event logs show the file
<p></p>
"SecondInvoicePaymentDeclined_RTY-7356-18373.pdf" was written to the disk and opened, just before
"halfaesed" was written to disk. Security operations searches the user's email for an attachment
with the same name, and bingo! it is there. You detonate the PDF file in a malware analysis
sandbox, and it writes the file "/tmp/.data/pamfgc/halfaesed" to disk. Your sandbox
instrumentation captures the file, BEFORE it is deleted or shredded. The hash matches the hash
seen in the file write agent event logs, so you know it is the same file. Your team lets out a
cheer. Huzzah! We have the malware AND the compromise vector!
<p></p>
Meanwhile, the network forensic team in the next bay is very quiet, as they gather around one
monitor with puzzled looks on their faces. They have found an encrypted data stream that is very
similar to the one they cracked last week. However, they are unable to find the key from the
packet capture file. "Maybe you just need to try harder! HAHAHA!", you are about to taunt, but
then you remember watching the monsters learning how to cooperate on Sesame Street this morning
(you watch it with your 3-year-old daughter before you go to work every day). "Maybe we should
work together. You have the pcap, and we have the malware. If we share and cooperate, maybe we
can figure it out."
<p></p>
Analyze the malware file "halfaesed", and use it to decrypt the pcap. You will need information
from both the executable and the pcap in order to decrypt the message and recover the flag.
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1z6bRl-Rd8YOx2U1G9MzuX7edBIfQQNak/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1P6txU5DXBuRr28eIxIw6cEqu9F5dYIbn/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>

</details>
</details>