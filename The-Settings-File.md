The settings file contains settings for Ciphey. Specifically, some of these you may want: 
* REGEX list. Have a list of Regex’s for the Regex checker? Use the settings file. 
* Default language. Hate how Ciphey always loads in English? Use the settings file to change the default language to whatever you want. 
* Is the language checker not working how you want it to work? Fine-tune the details in the settings file.

# Default Settings File
Save this as settings.yml in the appdirs location, which can be found by running ciphey -where or –where.
```console
➜ python3 ciphey -where
settings.yml should be placed in /home/bee/.config/ciphey
```
From this example, we can see that using the argument we need to place the settings file at /home/bee.config/ciphey/settings.yml

The settings file follows a specific format. **Copy and paste this below!**

```yaml
---
params:
  regex:
    regex:
    - FLAG{*.}
    - CTF{*.}
    - HTB{*.}
    - THM{*.}
  brandon:
    top1000: cipheydists::list::english1000
    wordlist: cipheydists::list::english
    stopwords: cipheydists::list::englishStopWords
    threshold: 0.45
```