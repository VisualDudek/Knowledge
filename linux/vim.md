# VIM

* autocompletion: `Ctrl-x Ctrl-n OR Ctrl-p`

### show compiled modules

`vim --version`

`:echo has('clipboard')`

### using tab pages

```text
:tabe[dit] {file}

# In normal mode
gt            go to next tab
gT            go to previous tab
```

{% embed url="https://vim.fandom.com/wiki/Using\_tab\_pages" %}

### panes

{% embed url="https://thoughtbot.com/blog/vim-splits-move-faster-and-more-naturally" %}

{% embed url="https://vim.fandom.com/wiki/Resize\_splits\_more\_quickly" %}



### YAML setup

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







