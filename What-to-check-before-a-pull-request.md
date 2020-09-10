# What your function returns
If your function has a chance of failing, in that failing case it should return `None`.

If your function works on every single text (I.E. the function you are using (from an external library maybe) never fails to return) you should see to see if the input string is different from the resulting string. 

If they are, return the resulting string else return None. This is because your function would return True for every single input which would result in a mess in the Ciphey logs.

So for example (taken from a PR):

```python
    def decode(self, ctext: T) -> Optional[U]:
        ctext = ctext.replace("+", " ")  
        res =  unquote(ctext) 
        return res if res != ctext else None
```

`unquote` will never fail, so it will always return _something_. We have had to implement our own check to see if unquote did anything, this way we can return None.

# Dictionaries or Lists
If your code requires a dictionary or list of letters or alphabets or the likes, it should go into CipheyDists.

[https://github.com/Ciphey/CipheyDists](https://github.com/Ciphey/CipheyDists)

CipheyDists contains all the dictionaries / alphabets required by decoders.

If you have a list in your code that's rather quite long, place it in the [lists directory](https://github.com/Ciphey/CipheyDists/tree/master/cipheydists/list) as a JSON file with an appropriate name.

If you have a dictionary, place it in [translate](https://github.com/Ciphey/CipheyDists/tree/master/cipheydists/translate) with an appropriate name.

Now you can call your dictionary / list.

Your first step is to change the `__init__` of yourr code. Let's look at Galactic Decoder as an example.

```python
    def __init__(self, config: Config):
        super().__init__(config)
        self.GALACTIC_DICT = config.get_resource(self._params()["dict"], Translation)
```

You don't have to change much here. Just add in this variable and name it something appropriate.

The next part requires some code. Change the `gatParams` method to look like this:

```python
    @staticmethod
    def getParams() -> Optional[Dict[str, ParamSpec]]:
        return {
            "dict": ParamSpec(
                desc="The galactic alphabet dictionary to use",
                req=False,
                default="cipheydists::translate::galactic",
            )
        }
```

Fill out the description (what it is you're getting from CipheyDists), whether or not it is required for your program to run, and the location of it.

In this case, the Galactic dictionary is in CipheyDists, it is in the translate subdirectory and it is called "galactic". If we look at the [code](https://github.com/Ciphey/CipheyDists/blob/master/cipheydists/translate/galactic.json) we can see this is true.

This is our [resource loader](https://github.com/Ciphey/Ciphey/wiki/Extending-Ciphey#resourceloader). You don't need to know much about it, other than it allows you to select resources without memorising weird file paths.

**Make sure to bump up the version of CipheyDists in your fork when we push a release to CipheyDists**