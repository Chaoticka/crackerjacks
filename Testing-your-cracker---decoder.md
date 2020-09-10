# Testing Your Code

Now we need to add tests to your code.

Find this file in your fork:

https://github.com/Ciphey/Ciphey/blob/master/tests/test_main.py

It's at `/tests/test_main.py`

Now, go to the bottom of the file and add this:

```python
def test_<CIPHER_NAME_HERE>():
    res = decrypt(
        Config().library_default().complete_config(),
        "<ENCRYPTED TEXT HERE>",
    )
    assert res == answer_str
```

We use one string to test in this file, because we can guarantee the plaintext of this string will always pass the checkers. This is the string we use:

https://github.com/Ciphey/Ciphey/blob/master/tests/test_main.py#L5

```python
answer_str = "Hello my name is bee and I like dog and apple and tree"
```

Encrypt this string with your chosen cipher. So for binary, we'd turn this into binary code. I'd use CyberChef for this.

https://gchq.github.io/CyberChef/

Encrypting our string with binary (use your own encryption method here) gives us:

```
01001000 01100101 01101100 01101100 01101111 00100000 01101101 01111001 00100000 01101110 01100001 01101101 01100101 00100000 01101001 01110011 00100000 01100010 01100101 01100101 00100000 01100001 01101110 01100100 00100000 01001001 00100000 01101100 01101001 01101011 01100101 00100000 01100100 01101111 01100111 00100000 01100001 01101110 01100100 00100000 01100001 01110000 01110000 01101100 01100101 00100000 01100001 01101110 01100100 00100000 01110100 01110010 01100101 01100101
```

Now we change the name of our test and add our encrypted string.

```python
def test_binary():
    res = decrypt(
        Config().library_default().complete_config(),
        "01001000 01100101 01101100 01101100 01101111 00100000 01101101 01111001 00100000 01101110 01100001 01101101 01100101 00100000 01101001 01110011 00100000 01100010 01100101 01100101 00100000 01100001 01101110 01100100 00100000 01001001 00100000 01101100 01101001 01101011 01100101 00100000 01100100 01101111 01100111 00100000 01100001 01101110 01100100 00100000 01100001 01110000 01110000 01101100 01100101 00100000 01100001 01101110 01100100 00100000 01110100 01110010 01100101 01100101",
    )
    assert res == answer_str
```

We can run this test with `pytest` locally, to see if it works. 

## Debugging your code
If this test fails, run Ciphey with the test like so:

```console
python3 ciphey -t "01001000 01100101 01101100 01101100 01101111 00100000 01101101 01111001 00100000 01101110 01100001 01101101 01100101 00100000 01101001 01110011 00100000 01100010 01100101 01100101 00100000 01100001 01101110 01100100 00100000 01001001 00100000 01101100 01101001 01101011 01100101 00100000 01100100 01101111 01100111 00100000 01100001 01101110 01100100 00100000 01100001 01110000 01110000 01101100 01100101 00100000 01100001 01101110 01100100 00100000 01110100 01110010 01100101 01100101" -vvv
```

And include the `-vvv` on the end. 

This will provide you with a lot of information on what Ciphey is doing.

if this does not work, and you are absolutely sure your code works submit it as a draft PR and let us know. Or contact us in the Discord.