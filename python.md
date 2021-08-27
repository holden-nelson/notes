# Python

## Pyenv and Virtualenv

```shell
## list available versions for install
$ pyenv install --list
# ... there are a lot
  
# works with grep
$ pyenv install --list | grep " 3\.[678]"
# versions 3.6.0 through 3.8-dev

## install 
$ pyenv install -v 3.7.2

## update
$ pyenv update

## list installed versions and environments
$ pyenv versions
* system (set by /Users/holden/.pyenv/version)
  3.7.2
  3.7.2/envs/harmondown
  3.7.2/envs/myproject
  3.7.2/envs/wplogin
  harmondown
  myproject
  wplogin

## list all commands
$ pyenv commands
activate
commands
completions
deactivate
...
virtualenvs
whence
which

# get more info about a command
$ pyenv shims --help
Usage: pyenv shims [--short]

List existing pyenv shims

## set python versions
# global
$ pyenv global 3.7.2

# local
$ pyenv local 3.7.2

# shell
$ pyenv shell 3.7.2

# create a virtualenv
$ pyenv virtualenv 3.7.2 myproject

## activate environment
$ pyenv local myproject
```

## Regex

`^` matches the beginning of the line

`$` matches the end of the line

`.` matches any character

`\s` matches a whitespace character

`\S` matches a non-whitespace character

`*` matches zero or more of the preceding character

`*?` is not greedy

`+` matches one or more of the preceding character

`+?` is not greedy

`[aeiou``]` matches any character in the specified set

`[a-z0-9]` matches any character in the specified range

`[^A-Z124]` matches any character that is a part of the negation of the specified set

`()` are ignored for the purposes of matching, but can be used by functions such as `findall()`

`\b` matches the empty string at the start or end of a word

`\B` matches the empty string, not at the start or end of a word

`\d` is equivalent to `[0-9]`

`\D` is equivalent to `[^0-9]`

