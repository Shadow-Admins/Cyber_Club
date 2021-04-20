<H1>Flag Collection</H1>
<hr>

<p></p>
Flag Collection was a series of challenges from CSC 2020. It revolves around file carving and stenography to a point. I would classify the first challenge as very easy and the second requiring some research. Below are walkthroughs for both challenges.
<hr>
<p></p>
<H2>Challenges</H2>
<details>
    <summary></summary>
<p></p>
I recommend you attempt these challenges on your own prior to looking through the walkthrough. Answers are at the end of the walkthroughs.
<p></p>
<details>
    <summary>Challenge 1</summary>
<p></p>
The first challenge we are given is:
<p></p>
Have you seen my flag collection? I could have sworn it was around here
somewhere.
<p></p>
<details>
    <summary>Hint</summary>
<p></p>
File carving is pretty cool.
</details>
<p></p>
<details>
    <summary>Hint</summary>
<p></p>
Foremost is my favourite.
</details>
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1MRxLoCJQTiKhUys-G_vBCAjkOgz07HKQ/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
<details>
    <summary>Extracting the File</summary>
<p></p>
The file comes compressed as a .7z IOT extract it we first need to ensure we have p7zip installed on our system by using the following command:
<p></p>

```
sudo apt install p7zip
```

<p></p>
Once p7zip is installed we run the following command to extract the challenge file:
<p></p>

```
❯ p7zip -d collection.7z

7-Zip (a) [64] 16.02 : Copyright (c) 1999-2016 Igor Pavlov : 2016-05-21
p7zip Version 16.02 (locale=en_AU.UTF-8,Utf16=on,HugeFiles=on,64 bits,8 CPUs Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz (806EA),ASM,AES-NI)

Scanning the drive for archives:
1 file, 3658662 bytes (3573 KiB)

Extracting archive: collection.7z
--
Path = collection.7z
Type = 7z
Physical Size = 3658662
Headers Size = 138
Method = LZMA2:26
Solid = -
Blocks = 1

Everything is Ok     

Size:       133169152
Compressed: 3658662
```

<p></p>
</details>
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>
Now we have a file to work with I always start with determining what type of file it is using the <kbd>file</kbd> command.
<p></p>
> file tests each argument in an attempt to classify it.  There are three sets of tests, performed in this order: filesystem tests, magic tests, and language tests.  The first test that succeeds causes the file type to be printed.
<p></p>

```
file collection.img
```

<p></p>
Whaich returns:
<p></p>

```
❯ file collection.img
collection.img: Linux rev 1.0 ext4 filesystem data, UUID=e1b1a8d2-f026-443c-94f0-6f799837d5ba (needs journal recovery) (extents) (large files) (huge files)
```




</details>
</details>
</details>
