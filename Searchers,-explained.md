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

This is a hand-created heuristic that our next search algorithm will be able to automate.

# Imperfection (A* search)
Imperfection is our next goal in terms of searching. It is an A* algorithm where the heuristic matches the close priority in AU search.

Nodes are ciphertext and paths are decryption methods (reverse of what we are currently doing).

The formula for our A* search is:

```
expected time / probability + k * heuristic, for some experimentally determined value of k
Where k = Shannon Entropy
Where heuristic = success_runtime + (1-success_likelihood)/success_likelihood  * failure_runtime
```

The 2 big players in this algorithm are runtime and probability of success (as well as the time of that success). We will try cheap (fast) decryptions first which are likely, but if we have an expensive decryption and we are sure that's the right answer we won't skip over it.

The Shannon Entropy reduces the more decryptions we do (at least on average in our testing). We can use entropy to work out which path is leading us to the right decryption method even if we do not know what the text is encrypted with.

Shannon entropy let's us see if we are going in the right direction.

![](https://cdn.substack.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F0e1c211c-f3ef-4c0f-aa71-fcc5749583c3_1437x1195.png)
This is encrypted with Base64 -> Rot13 -> Vigenère (key: “key”).

The Shannon Entropy is 5.23.

Now if we “decode Base64” and get the entropy again:
![](https://cdn.substack.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fa9ceeddb-dece-4113-96c9-5a8c5776ce34_1441x1189.png)
It’s now 3.88. Even though we have no idea how many more decryptions there are, or if our Base64 decoding successfully resulted in the correct decoding we know we are going in the right direction because the Entropy is decreasing. 
