This is a page for things you may want to do, but are too confused by our documentation for.

This page will show you specific applications of Ciphey.


# Common flags you may want to use
## How do I enable nested encryption?
[We held a vote and found that nested encryptions by default wasn't something people wanted.](https://github.com/Ciphey/Ciphey/issues/327)
```
ciphey -p ausearch.enable_nested=True
```
## How do I use Regex?
Use this as a **crib**. If you know some plaintext but not all of it, this will help Ciphey a lot.
```
ciphey -C regex -p regex.regex={
```

## How do I disable the human checker?
Set the verbosity to less than 0.

```
ciphey -q
```

-q sets the verbosity to -1.

