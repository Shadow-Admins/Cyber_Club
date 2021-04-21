<H1>Speed Me Up Scottie</H1>
<p></p>
Speed Me Up Scottie was a series of python scripting challenges from the CSC 2020.
<p></p>
<H2>Challenges</H2>
<details>
    <summary></summary>
<details>
    <summary>Speed Me Up Scottie 1</summary>
<p></p>
This program prints the flag, but takes a reeeeeeeeeally long time to do so. Can you rewrite it faster?
<p></p>
<details>
    <summary>Hint</summary>
<p></p>
My calculations say it will take over 100 years on an average PC. My friends version finished in milliseconds!
</details>
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1oKn3B1gbc7xDdAL-TDqORSh9VagiePQl/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
<details>
    <summary>Code</summary>
<p></p>

```python
def my_func(x):
    # Note: Don't fall for the off by one
    if x == 0 or x == 1:
        return 1
    return my_func(x - 1) + my_func(x - 2)


print(f"flag{{{my_func(100)}}}")
```

<p></p>
</details>
</details>
<hr>
<p></p>
<details>
    <summary>Speed Me Up Scottie 2</summary>
<p></p>
This program also prints the flag, but takes a reeeeeeeeeally long time to do so. Can you rewrite it faster?
<p></p>
Challenge File: <a href="https://drive.google.com/file/d/1tHrVEFq4okemG5r28f_brH8anJMzJLRd/view?usp=sharing" rel="nofollow">Google Drive</a>
<p></p>
<details>
    <summary>Code</summary>

```python
"""
Example:
> my_func2('ABAAABBCCACBBBBCBCBCBBBBACBCBCCBBBBCBBBBCAACCABCAACABACACCAB', 'CCCBAABAACCCAABABBCCABBCBCABAABAABCABACACACABABBBCACBACBABCC')
CCCBBCCCBABBCCBBCBBBBCAACCACAACABACACC
"""


def my_func2_inner(x, y, a, b):
    if a == 0 or b == 0:
        return 0, ""
    if x[a - 1] == y[b - 1]:
        m, n = my_func2_inner(x, y, a - 1, b - 1)
        return m + 1, n + x[a - 1]
    return max(my_func2_inner(x, y, a, b - 1), my_func2_inner(x, y, a - 1, b))


def my_func2(x, y):
    return my_func2_inner(x, y, len(x), len(y))[1]


print(
    f"flag{{{my_func2('CCABBCBACABBABACACBBBCBCABABBAABACBCAAACBBCCCBBABBBACCBBBBAB', 'ACCCCBCBCBBCBABBCACCCBBAABCCCCAAABBAABACAAABCBABBCCBCAACCCAA')}}}"
)
```

<p></p>
</details>
</details>