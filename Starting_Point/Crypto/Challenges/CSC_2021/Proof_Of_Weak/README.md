<H1>Proof Of Weak</H1>
<p></p>
Proof Of Weak was a 150pt crypto challenge from the CSC 2021.
<p></p>
<H2>Challenge</H2>
<details>
    <summary></summary>
<p></p>
I just upgraded my gaming rig with a new GPU but for some reason the hash rate is much slower.
The problem is I have some important files encrypted that I need to access.
<br>
The encryption key is derived from the password "CryptoD00D_willrockyou.txt" but it is taking too long.
<p></p>
I'm using a custom iterated hashing scheme that has been optimised for performance and security.
<p></p>

```
from hashlib import *
hash = sha512(password.encode('utf-8')).hexdigest()[0:12]
i = 1
while i <= 2**98789:
    hash = sha512(hash).encode('utf-8')).hexdigest()[0:12]
    i+= 1
key = sha256((password + hash).encode('utf-8')).hexdigest()
```

<p></p>

I'm offering 1 flagcoin to anyone who can help me recover my files.
<p></p>
Generate the key and decrypt the included file to prove you have solved the challenge.
<p></p>
File is encrypted with AES-256-ECB.
<p></p>
flag format: flag{I_AM_A_STRING}
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1r5CiLW_RlrW0UMxQRuweBQq9Xg-YZrus/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
<details>
    <summary>Walkthrough</summary>
<p></p>

</details>
</details>