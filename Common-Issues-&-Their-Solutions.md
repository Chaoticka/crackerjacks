# I can't install Ciphey on Windows
This is a known issue and the workaround is to use Docker.
[See here for our Docker instructions](https://github.com/Ciphey/Ciphey/wiki/Installation).

# I can't install Ciphey on Mac OS
This is a known issue and the workaround is to use Docker.
[See here for our Docker instructions](https://github.com/Ciphey/Ciphey/wiki/Installation).

# I can't install Ciphey on versions of Python under 3.7
Ciphey does not support any version of Python below 3.7. Please use Docker or upgrade your Python version.
[See here for our Docker instructions](https://github.com/Ciphey/Ciphey/wiki/Installation).

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

By default we support CTF flags (in the format THM{*}, HTB{*}, CTF{*}, FLAG{*}), English & JSON. 

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

# Why can't Ciphey brochette this ciphertext which is encrypted with AES-128 bit key?
This would take more energy than there is in the solar system.
Also, yes, this was a genuine question someone asked.