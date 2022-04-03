# VSCode

## LINKS

{% embed url="https://www.barbarianmeetscoding.com/blog/boost-your-coding-fu-with-vscode-and-vim" %}

## EXTENSIONS

```
Python
Vim
```

### showHover

settings.json file

```javascript
    {
        "key": "ctrl+k i",
        "command": "editor.action.showHover",
        "when": "editorTextFocus"
    }
```

### reportMissingModuleSource

F1, Developer: Reload Window

### debug no just in my code

edit `launch.json` file, key: `justMyCode`

```javascript
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Current File",
            "type": "python",
            "request": "launch",
            "program": "${file}",
            "console": "integratedTerminal",
            "justMyCode": false
        }
    ]
}
```

### go to the symbol in editor

Ctrl + Shift + O

e.g. when you want find `Logger` class in `logging` module

### fix internal terminal font for zsh

Settings -> Editor:Font Family add 'PowerlineSymbols'
