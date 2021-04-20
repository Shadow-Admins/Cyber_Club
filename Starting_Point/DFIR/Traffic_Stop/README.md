<H1>Traffic Stop</H1>
<hr>

<p></p>
Traffic Stop was a series of challenges from CSC 2020. The challenges revolve around file carving and data recovery. I would classify the challenges as medium to hard and requiring some research. Below are walkthroughs for the three challenges.
<hr>
<p></p>
<H2>Challenges</H2>
<details>
    <summary></summary>
<p></p>
I recommend you attempt these challenges on your own prior to looking through the walkthrough. Answers are at the end of the walkthroughs.
<p></p>
All three challenges us the usb.zip file located below.
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1MMLbIp_GT-RTojR38AFXm91lnehNf3Rf/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
<details>
    <summary>Extracting the File</summary>
<p></p>
All you need to do is run the following command on the file:

```
unzip usb.zip
```

<p></p>
Which outputs:
<p></p>

```
❯ unzip usb.zip
Archive:  usb.zip
  inflating: usb.raw                 
```

<p></p>
You now have a file to work with usb.raw
<p></p>
</details>
<p></p>
<details>
    <summary>Challenge 1</summary>
<p></p>
The first challenge we are given is:
<p></p>
We have an acquired a USB from a suspect believed to be associated with
human trafficking. Can you find any evidence to support this claim?
<p></p>
<details>
    <summary>Hint</summary>
If you find an account number use `echo <account> | xxd -r -p`
</details>
<p></p>
<details>
    <summary>Hint</summary>
I spent all this time configuring IRC but everyone is using slack these days.
</details>
<p></p>

</details>