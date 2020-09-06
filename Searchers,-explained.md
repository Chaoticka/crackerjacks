Searchers are the heart of Ciphey. They are search algorithms which find the path from the ciphertext to the plaintext.

They are the artificial intelligence of Ciphey (under the Russel & Norvig definition).

# Perfection
Our first search algorithm. Perfection randomly chose what node to visit next. In this algorithm, nodes are decryption methods and the paths are the text.

This was a proof of concept algorithm we developed so we could test earlier and release on time. 

It's called Perfection after 1984.

# Augmented (AU) Search
Our current searching algorithm. Edges are ciphertext and nodes are decryption methods.

We choose the node with the highest priority more first. Priority is set by the creator of the cracker/decoder.

We determine priority based on how likely something is to appear, and how easy it is to check.

For example, Base64 appears all the time and it's very cost-efficient to check as much as we want so it's a high priority. Whereas Vigenère doesn't appear too often and it's expensive to check so it's a lower priority.

This is a hand-created heuristic that our next search algorithm will be able to fix.

# Imperfection (A* search)
Imperfection is our next goal in terms of searching. It is an A* algorithm where the heuristic matches the close priority in AU search.

The formula for our A* search is:

```
expected time / + k * heuristic, for some experimentally determined value of k
Where k = Shannen Entropy
Where heuristic = success_runtime + (1-success_likelihood)/success_likelihood  * failure_runtime
```

The 2 big players in this algorithm are runtime and probability of success (as well as the time of that success). We will try cheap (fast) decryptions first which are likely, but if we have an expensive decryption and we are sure that's the right answer we won't skip over it.

The Shannen Entropy reduces the more decryptions we do (at least on average in our testing). We can use entropy to work out which path is leading us to the right decryption method even if we do not know what the text is encrypted with.

