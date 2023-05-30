---
share: true
layout: post
categories: python
---

- Install [pyenv](https://github.com/pyenv/pyenv)  via Homebrew and set up in the shell
```bash
andy@macbook pykafka % brew install pyenv
# Then do shell setup: https://github.com/pyenv/pyenv#set-up-your-shell-environment-for-pyenv
andy@macbook pykafka % pyenv install 3.11.3 
# Wait for a bit while it all compiles... ☕️
Installed Python-3.11.3 to /Users/andy/.pyenv/versions/3.11.3
andy@macbook pykafka % python3 --version 

Python 3.10.7
andy@macbook pykafka % 
```

To select a Pyenv-installed Python as the version to use, run one of the following commands:

- [`pyenv shell <version>`](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-shell) -- select just for current shell session
- [`pyenv local <version>`](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-local) -- automatically select whenever you are in the current directory (or its subdirectories)
- [`pyenv global <version>`](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-shell) -- select globally for your user account

E.g. to select the above-mentioned newly-installed Python 3.10.4 as your preferred version to use:

```shell
pyenv local 3.11.3
andy@macbook pykafka % python3 --version

Python 3.11.3

andy@macbook pykafka % pip install --upgrade pip
```

Now whenever you invoke `python`, `pip` etc., an executable from the Pyenv-provided 3.10.4 installation will be run instead of the system Python.

## Install the packages you want
``` shell
andy@macbook pykafka % pip3 install aiokafka

Collecting aiokafka
...
Successfully installed aiokafka-0.8.0 async-timeout-4.0.2 kafka-python-2.0.2 packaging-23.1
```

#python #kakfa 