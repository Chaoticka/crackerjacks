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