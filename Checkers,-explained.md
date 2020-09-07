We use multiple different checkers to check for when something becomes plaintext.

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
# Regex
The Regex checker runs a Regex against the ctext. By default we check for CTF flags. These include:
1. HTB{*.}
2. THM{*.}
3. FLAG{*.}
4. CTF{*.}

Case insensitive.

The user can specify different Regex in the program arguments or using the config file.
# Entropy (coming soon)
We plan to add an entropy checker to see if the data is ordered or not. We have the code already, but we need to implement a feature that asks the user if the output is correct else we may end up with many false positives.
# G-test of Goodness-of-fit
We use [this](https://en.wikipedia.org/wiki/G-test) instead of Chi Squared (as we found it to work better in our testing).