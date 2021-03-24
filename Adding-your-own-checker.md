After reading [this page](https://github.com/Ciphey/Ciphey/wiki/Checkers,-explained) perhaps you want to create your own checkers? This is for you!

1. Copy and paste the general template:

```py
from typing import Dict, Optional

from loguru import logger

from ciphey.iface import Checker, Config, ParamSpec, T, registry

from collections import Counter


@registry.register
class ChiSquared(Checker[str]):

    """
    Checks things
    """

    def check(self, text: T) -> Optional[str]:
        return None


    def getExpectedRuntime(self, text: T) -> float:
        # TODO: actually bench this
        # Uses benchmark from Discord
        return 2e-7 * len(text)

    def __init__(self, config: Config):
        super().__init__(config)

    @staticmethod
    def getParams() -> Optional[Dict[str, ParamSpec]]:
        pass
```

Change the name to your checker. See this for a working example of a checker:

```py
from typing import Dict, Optional

from loguru import logger

from ciphey.iface import Checker, Config, ParamSpec, T, registry

from collections import Counter


@registry.register
class ChiSquared(Checker[str]):

    """
    Uses Chi Squared to determine plaintext
    """

    def check(self, text: T) -> Optional[str]:
        print("chi runs")
        logger.trace("Trying Chi Squared checker")
        chi_sq = 0.0
        counts = Counter(text)
        for i in list(counts.keys()):
            # If letter is not in dict, don't do it.
            try:
                chi_sq = chi_sq + ((counts[i] - self.wordlist[i])**2 / self.wordlist[i])
            except:
                continue
        logger.debug(f"Chi squared is {chi_sq}")
        logger.debug(chi_sq / len(text))
        if chi_sq / len(text) > 50.0:
            print(chi_sq / len(text))
            return True
        return None


    def getExpectedRuntime(self, text: T) -> float:
        # TODO: actually bench this
        # Uses benchmark from Discord
        return 2e-7 * len(text)

    def __init__(self, config: Config):
        super().__init__(config)
        self.wordlist = config.get_resource(self._params()["wordlist"])

    @staticmethod
    def getParams() -> Optional[Dict[str, ParamSpec]]:
        return {
            "wordlist": ParamSpec(
                desc="A wordlist of all the words",
                req=False,
                default="cipheydists::list::english",
            )
        }
```

**Important** Your checker needs to return True for it's worked, and None for it's failed.

Next add your checker to:

```
_-init__.py
```

Like so:

```
from . import any, brandon, ezcheck, format, human, quorum, regex, chisquared
```

And finally, go to `ezcheck.py` and add your checker there, basing it on the speed it takes.

Like so:

```py
        # First the flag regexes, as they are the fastest
        flags_config = config
        flags_config.update_param("regexlist", "resource", "cipheydists::list::flags")
        # We do not cache, as this uses a different, on-time config
        self.checkers.append(RegexList(flags_config))

        # Next, the json checker
        self.checkers.append(config(JsonChecker))

        self.checkers.append(config(ChiSquared))

        # Finally, the Brandon checker, as it is the slowest
        self.checkers.append(config(Brandon))
```

Finally, write some tests, run our tests with [Nox](https://nox.thea.codes/en/stable/) and make sure everything is a-okay!