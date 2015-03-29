# keyboard shortcut. 


Fair warning: All these API are unstable and can chage at any time. 


Through javascript you can access the keyboard manager.

The keyboardmanager maps (some) shortcuts, in command and edit mode to `actions`.

A shortcut is a string the represent a sequence of Multiple key pressed at the same time. Comma represent each step of the sequence and dashes,represent key that have to be pressed together. 

For example `a,b,c,d` represent the succession of pressing the letter A, B, C and D without modifier.
`Shift-a,b,c,d` will have only `A`key pressed with shift modifier, and `Shift-a,Shift-b,Shift-c,Shift-d` represent holding shift and pressing `a,b,c,d` in order. 

Bind some existing action in command mode, in javascript console (where `>` and `<` are in and ot prompt): 


```javascript
> IPython.keyboard_manager.command_shortcuts.add_shortcut('Shift-k','ipython.move-selected-cell-up')
< undefined

>IPython.keyboard_manager.command_shortcuts.add_shortcut('Shift-j','ipython.move-selected-cell-down')
< undefined
```

To see the list of availlable actions: 

```
> $.map(
     IPython.keyboard_manager.command_shortcuts.actions._actions,
     function(k,v){return v}
     )

< ["ipython.run-select-next",
"ipython.execute-in-place",
"ipython.execute-and-insert-after",
"ipython.go-to-command-mode",
...
"ipython.move-cursor-down-or-next-cell",
"ipython.scroll-down", "ipython.scroll-up",
"ipython.save-notebook"]
```



