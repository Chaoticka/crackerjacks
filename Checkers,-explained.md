Ciphey currently uses **7 checkers** to determine plaintext.

# Language Checking
Our language checker is called `brandon`, since we couldn't think of a better name.

The basic principle of it is this:

```
for word in ciphertext:
    if word in list:
        threshold += 1
```

When building Brandon checker, we experimented with around ~10 different English checkers and found this solution was the fastest and most accurate. There was another that was more accurate, but it took far too long to run. You can see our tests [here](https://github.com/Ciphey/CipheyDocs/blob/master/docs/lc2.rst) and the corresponding GitHub issue is #90.

It returns True when threshold is met, otherwise it returns False.

The ciphertext is cleansed of anything that isn't pure alphabetical text beforehand. This is because `brandon`'s only job is to detect English, nothing else.

There are 2 major players in this check.
1. The threshold
2. The list

The threshold is determined on a list-by-list basis. We grab English text and generate Ciphertext and place it into a dictionary. Where `{text: True}` indicates that the text is English, but `{text: False}` indicates it is false.

We then built a contingency table of the results. The idea was we would be able to tell which algorithms successfully guessed English the most while also guessing crypto the least. An example of the contingency table is:

![https://www.researchgate.net/profile/Jeffrey_Foster/publication/280535795/figure/fig5/AS:267911000490014@1440886368724/Contingency-table-True-Positive-False-Positive-False-Negative-and-True-Negatives-are.png](https://www.researchgate.net/profile/Jeffrey_Foster/publication/280535795/figure/fig5/AS:267911000490014@1440886368724/Contingency-table-True-Positive-False-Positive-False-Negative-and-True-Negatives-are.png)

For each list we ran the algorithm in increments of 1% until we found the optimal threshold for that list.

The lists were found by guessing good lists (trigrams, stop words, top 1k words, all English words, words that end in "ing" and far more). We then ran the threshold tests against each of these to find the optimal thresholds, before running all the tests with different lists to find the ones that performed the best.

In short, we likely performed ~200 million tests on an incredibly large corpus of English. This process took ~1 month.

We came to the conclusion that the optimal checker used 3 lists and had different thresholds depending on the size of the text.
The three lists we chose were:
1. Stop words
2. Top 1000 words
3. The entire English dictionary

We use a 2-prong approach where if something passes Stop words or top 1k words, it will be passed to the dictionary checker to make sure.

Stop words or top 1k words uses a small threshold than dictionary checker.

You can see our thresholds [here](https://github.com/Ciphey/CipheyDists/blob/master/cipheydists/brandon/english.json).

**We are welcome to feedback about this approach, as we are not experts in this area.**

# JSON
The JSON checker works by running `json.loads(ctext)` on the ciphertext. If it succeeds, it is JSON and returns True.

The input:
```
192834131381
```

is valid JSON. This is not plaintext, so we have edited the JSON checker to comply. See [#389](https://github.com/Ciphey/Ciphey/issues/389)

# Regex
The Regex checker runs a Regex against the ctext. By default we check for CTF flags. These include:
1. HTB{*.}
2. THM{*.}
3. FLAG{*.}
4. CTF{*.}

Case insensitive.

The user can specify different Regex in the program arguments or using the config file.

# G-test of Goodness-of-fit
We use [this](https://en.wikipedia.org/wiki/G-test) instead of Chi Squared (as we found it to work better in our testing).

This is currently not its own checker (will be added soon) but is used in some of our crackers to determine whether or not is it worth continuing the computation of decrypting with a given key.

This is also planned to be a generalised instead of specific checker.

# What

[What](https://github.com/bee-san/pyWhat) is a more advanced Regex checker. To put it simply, it is the regex checker but with the regular expressions already populated. We have around 100 regular expressions in What.

The idea is to solve generality of checkers via specificity.

We have many regular expressions which are hyper-specific -- meaning they are unlikely to be false positives.

By having hundreds of them, we can create a more "generalised" checker which is generalised by how many of the specific, smaller checkers (regular expressions) it uses.

TL;DR is that this is a general checker using the shotgun approach, but each shotgun pellet is a sniper rifle shot.

[Click here to see all the Regex What supports](https://github.com/bee-san/pyWhat/blob/main/pywhat/Data/regex.json)

# QuadGrams

Quadgrams are 4 letters together which are seen commonly. An example is `exam`. 

This checker goes through every 4 letters in the text and if the 4 letters match one of the quadgrams, it increments a counter.

If a certain percentage of all quadgrams are matched in the text, then we return true.

This checker is more general than a hyper-specific dictionary checker.

# The Human Checker
The best checker is a human.

We can only try to guess what the plaintext is, but humans should always be able to spot plaintext.

The Human checker uses laxer automated checking to ask you if the text is plaintext or not.

# ???? Polymorphic Checkers
These are a really dark art, and have a *very* limited use case. We currently have no use of these checkers, but are planning on adding them for our more advanced cryptosystems support.

They allow you to create a `Checker`-like that accepts an object of any type. In fact, all `Checker`s are silently converted into this more general class when you use the `@registry.register` or `@registry.register_multi` decorators.

If you wish to implement one, see the [relevant docs](https://github.com/Ciphey/Ciphey/wiki/Extending-Ciphey)

# Archived / Not Implemented Yet Checkers

# Entropy (coming soon)
We plan to add an entropy checker to see if the data is ordered or not. Since the addition of the Human Checker, we plan to add this soon. We have the code -- just need to test it.

This checker has lower accuracy but higher generality. All the checkers you have seen until now solve very specific problems very well. JSON, Language (English), or Regex.

The entropy checker is designed to find things that are ordered (and thus may be plaintext) but aren't picked up by the other checkers.
