# I can't install Ciphey on Windows

Ciphey only supports 64-bit Python, but if you download Python from the website it defaults to 32-bit Python. Please make sure your Python is 64-bit.

Otherwise, use Docker.
[See here for our Docker instructions](https://github.com/Ciphey/Ciphey/wiki/Installation).

# I can't install Ciphey on Mac OS
This is a known issue and the workaround is to use Docker.
[See here for our Docker instructions](https://github.com/Ciphey/Ciphey/wiki/Installation).

If you might have information that is unique and can help us fix this bug, please [see this issue and add to it.](https://github.com/Ciphey/Ciphey/issues/362)

# I can't install Ciphey on versions of Python under 3.7
Ciphey does not support any version of Python below 3.7. Please use Docker or upgrade your Python version.
[See here for our Docker instructions](https://github.com/Ciphey/Ciphey/wiki/Installation).

# The installation instructions on the README are confusing
We have a much larger article for installation instructions.
[See it here](https://github.com/Ciphey/Ciphey/wiki/Installation).

# Ciphey runs forever / can't decrypt this input

Is your plaintext one of these:
* JSON
* English
* A CTF flag

If not, Ciphey cannot decrypt your text unless you provide a crib.

Unlike CyberChef magic which only does encodings, Ciphey aims to do everything including hashes and encryptions.

Because CyberChef Magic does encodings, it has a 1-to-1 map of input > output. Ciphey has a one-to-many map because we do hashes and encryptions.

We have seen both of these bugs in production:
* Base64 is perfectly Caesar cipher (and decodes to English text via Caesar cipher)
* Vigenère Cipher is perfectly a SHA hash and decodes to plaintext.

To prevent the wrong plaintext from being outputted, Ciphey has to use `checkers` to check to see if the plaintext is really the plaintext the user wants.

By default, we support CTF flags (in the format THM{*}, HTB{*}, CTF{*}, FLAG{*}), English & JSON. 

If your plaintext is not one of these, we cannot decrypt it.

If you know what your plaintext _might_ contain you should use a `crib`. This is a small bit of knowledge on what the plaintext is.

To do this, run Ciphey with this:

```console
ciphey -t = "text" -C regex -p regex.regex={
```

To use the `regex checker`. The `{` indicates you want the regex to succeed when it sees `{`. Write your custom regex here, or plaintext if you know it such as:

```console
ciphey -t = "text" -C regex -p regex.regex=ciphey
```

# Why do you have a lot of encodings? You should work on real world ciphers more!
It is very easy to contribute to Ciphey. So easy in fact that we regularly get pull requests with weird and esoteric encodings.

Thankfully, we are implementing a tag system. So when the user runs Ciphey by default it will run with the popular encodings, not the weird esoteric ones. This means our lovely contributors can add as many weird & esoteric decodings as they want, without effecting the performance of Ciphey **and** providing more value for people who want to try everything.

Also, Ciphey's checkers are very specific in what they take. For example, the string "hello_my_name_is_emily" is English to you and me, but to the checker it is one giant word that isn't English.

Our leetspeak decoder would convert those `_`s into spaces. The same for NATO, URL encoding and more. 

**TL;DR**
To wrap up, we have a lot of encodings because:
1. A significant amount of our pull requests are encodings (and people don't feel bad about adding them since it doesn't negatively effect Ciphey).
2. What is plaintext to you and I won't be plaintext to a computer unless we write the code to decode it into plaintext.
3. Decodings are significantly easier to work on than crackers (most of our crackers are written in C++, whereas Decoders are pure Python).


-----
##### Why can't Ciphey bruteforce this ciphertext which is encrypted with a AES-128 bit key?
This would take more energy than there is in the solar system.
Also, yes, this was a genuine question someone asked.



