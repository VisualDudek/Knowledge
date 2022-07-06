# VIM

* autocompletion: `Ctrl-x Ctrl-n OR Ctrl-p`

### moving around

`{ / }` jump do the next empty newline

### show compiled modules

`vim --version`

`:echo has('clipboard')`

### using tab pages

```
:tabe[dit] {file}

# In normal mode
gt            go to next tab
gT            go to previous tab
```

{% embed url="https://vim.fandom.com/wiki/Using_tab_pages" %}

### panes

{% embed url="https://thoughtbot.com/blog/vim-splits-move-faster-and-more-naturally" %}

{% embed url="https://vim.fandom.com/wiki/Resize_splits_more_quickly" %}

### YAML setup

```
# vim config
autocmd FileType yaml setlocal ts=2 sts=2 sw=2 expandtab

# Vim has a shortcut for every keyword. 
#Examples: tabstop becomes ts, expandtab becomes et, listchars becomes lcs
```

Indentation guides by using [indentLine plugin](https://github.com/Yggdroot/indentLine).

{% embed url="https://www.arthurkoziel.com/setting-up-vim-for-yaml/" %}

### display tabs as characters

```
:set list
:set listchars=tab:>-
```

### startuptime

`vim <filename> --startuptime debug.log` Write startup timing messages to debug.log

### readonly mode

`vim -R filename`

### Learn to use help

`:h[elp]`

```
# Get specific help:
Option :help 'textwidth'
```

{% embed url="https://vim.fandom.com/wiki/Learn_to_use_help" %}
