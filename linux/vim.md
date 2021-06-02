# VIM

* autocompletion: `Ctrl-x Ctrl-n OR Ctrl-p`

### using tab pages

{% embed url="https://vim.fandom.com/wiki/Using\_tab\_pages" %}

### YEAML setup

```text
# vim config
autocmd FileType yaml setlocal ts=2 sts=2 sw=2 expandtab

# Vim has a shortcut for every keyword. 
#Examples: tabstop becomes ts, expandtab becomes et, listchars becomes lcs
```

Indentation guides by using [indentLine plugin](https://github.com/Yggdroot/indentLine).

{% embed url="https://www.arthurkoziel.com/setting-up-vim-for-yaml/" %}

### display tabs as characters

```text
:set list
:set listchars=tab:>-
```

### startuptime

`vim <filename> --startuptime debug.log` Write startup timing messages to debug.log

### readonly mode

`vim -R filename`

### Learn to use help

`:h[elp]`

```text
# Get specific help:
Option :help 'textwidth'
```

{% embed url="https://vim.fandom.com/wiki/Learn\_to\_use\_help" %}







