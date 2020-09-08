**Work in progress, not fully implemented yet**.

The tagging system is a way of calling all, subsets of, or specific ciphers of Ciphey.

Sometimes you'll want to run ciphers which are:
* Offline only
* Online only
* Numbers based only
* Popular (no esoteric encodings)
* Crackers only
* Only those made by the core contributors
* Community only (made by everyone apart from the core)

And so on.

We will use set theory to allow you to customise Ciphey even more. For example, to get all the popular encoders which are built by the core team:

```
ciphey --tag "Core and Encoder and Popular"
```

We plan to use the hamming distance if the tag isn't found to approximate the tag you wanted. So if you misspell "encoder" by one letter, no worries!

This is still a work in progress and you cannot use this system yet.

# List of tags
* Offline 
* Core approved (as in, crackers / decoders that the core team have created)
* Community
* Most popular
* Meme (the hint might be "funny number base encoding" or something)
* Crackers
* Encoders
* Hashes
* Transposition
* Modern ciphers
* Esoteric Programming Languages
* Modern Day Crypto
* Families I.E. Atbash and Vig, All bases, XORcrypt and XOR. (this will be 1 tag per family, so all Bases will get tagged `base` whereas Atbash and Vig will get `vig`)

# List of Tags in the ciphers
Here is every encoding/encryption/hash system Ciphey has with their tags.

```
binary = "binary", "popular", "core", "decoder", "numbersBased"
```