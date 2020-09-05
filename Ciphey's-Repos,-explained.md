* [Ciphey](https://github.com/Ciphey/Ciphey) - The core repo for the program. This is where we spend most of our time.
* [CipheyCore](https://github.com/Ciphey/CipheyCore) - The C++ Core of Ciphey. Anything computationally expensive will go here, in C++.
* [CipheyDists](https://github.com/Ciphey/CipheyDists) - This is where we store our distributions for frequency analysis and more. Things like the English alphabet, Morse code dictionary, Charsets, thresholds for checkers. If you have a dictionary / list / charset / alphabet you should store it here. This allows us to be more modular. For example, by using CipheyDists for English we can easily switch out to French or German using an argument.
* [CipheyDocs](https://github.com/Ciphey/CipheyDocs) - The old documentation for Ciphey. We use the Wiki now, but it still contains some useful things for archive purposes.
* [EnCiphey](https://github.com/Ciphey/enCiphey) - The opposite of Ciphey, encrypts things. We use this to generate data sets that we can test our program against.
* [Docker](https://github.com/Ciphey/docker) - The REMnux Docker image of Ciphey, forked. This is so we can update their image.
* [homebrew-Ciphey](https://github.com/Ciphey/homebrew-ciphey) - Our ~~failing~~ attempt at making a Homebrew install.
* [CipheyOnline](https://github.com/Ciphey/CipheyOnline) - The website for Ciphey
* [CipheyNeuralNetwork](https://github.com/Ciphey/CipheyNeuralNetwork) - The code for the neural network(s) for Ciphey. We no longer use them, so this will lay dormant.