To import Ciphey, you'll need these 2 things:

```python
from ciphey import decrypt
from ciphey.iface import Config
```

Decrypt does what it says on the tin, Config is the config for Ciphey. I.E. It's where you place your text among other things.

To call Ciphey's decrypt function:

```python
    res = decrypt(
        Config().library_default().complete_config(),
        "SGVsbG8gbXkgbmFtZSBpcyBiZWUgYW5kIEkgbGlrZSBkb2cgYW5kIGFwcGxlIGFuZCB0cmVl",
    )
```

Although note, this may run forever. We do not currently support timeouts so please set a timeout yourself.