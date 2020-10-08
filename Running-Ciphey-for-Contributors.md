So you've added some code to Ciphey and now want to run it. How do you run Ciphey?

# Poetry
The core team uses [Poetry](https://python-poetry.org/) to run Ciphey. 

You can install Poetry with [this page here](https://python-poetry.org/docs/).

To run Ciphey with Poetry, in the root directory of Ciphey's repo run:

```
poetry run ciphey -t "hello"
```

**Note** if you are having dependency issues, run `poetry install`.

We use Poetry because it creates a virtual environment and installs the packages every time you run it. Essentially it's fake-Docker for Python. But it means that if it runs for you via Poetry, it has to run for everyone else too. 

It also fixes dependency issues. Because it installs the required dependencies on run-time it will always have the right ones. No more conflicts!

# Running without Poetry

In the root folder create a file named `main.py` (name does not matter). The file contains this:

```python
from ciphey.ciphey import main

main()
```

Now to run Ciphey, call:

```console
python3 main.py
```