# Example of custom extension

I'll respond to the following question in [issue 252 of jupyter notebook repo](https://github.com/jupyter/notebook/issues/252#issuecomment-127368408). 


So first we want to create or extension folder, I'll do it directly in `~/.jupyter/nbextensions/runselected/main.js` on Linux, 
`~/Library/Jupyter/nbextensions/runselected/main.js` on OS X, God knows where on Windows. 

```bash
~ $ mkdir ~/.jupyter/nbextensions
~ $ mkdir ~/.jupyter/nbextensions/runselected/
~ $ cd  ~/.jupyter/nbextensions/runselected/
~/.j/n/runselected $ vim main.js
```

Activate the extension in the JS console of your browser
```
IPython.notebook.config.update({
  "load_extensions": {"runselected/main":true}
})
```

And set past this in `main.js` created above.

```javascript
define(function(){

    var execute_selection= function (cell, stop_on_error) {
        // adapted from Cell.prototype.execute, (copy and past + except take cell a on first input) 
        // then s/this/cell/
        // s/get_text/code_mirror.getSelection/
        if (!cell.kernel || !cell.kernel.is_connected()) {
            console.log("Can't execute, kernel is not connected.");
            return;
        }

        cell.output_area.clear_output(false, true);

        if (stop_on_error === undefined) {
            stop_on_error = true;
        }

        var old_msg_id = cell.last_msg_id;

        if (old_msg_id) {
            cell.kernel.clear_callbacks_for_msg(old_msg_id);
            if (old_msg_id) {
                // figure out how to uncomment this one.
                //delete CodeCell.msg_cells[old_msg_id];
            }
        }
        if (cell.code_mirror.getSelection().trim().length === 0) {
            // nothing to do
            cell.set_input_prompt(null);
            return;
        }
        cell.set_input_prompt('*');
        cell.element.addClass("running");
        var callbacks = cell.get_callbacks();
        
        cell.last_msg_id = cell.kernel.execute(cell.code_mirror.getSelection(), callbacks, {silent: false, store_history: true,
            stop_on_error : stop_on_error});
        // figure out how to uncomment this one.
        //CodeCell.msg_cells[cell.last_msg_id] = cell;
        cell.render();
        cell.events.trigger('execute.CodeCell', {cell: cell});
    };

    var run_cell_selection  = {
        help: 'run the current seleciton of the curent focused cell',
        icon : 'fa-recycle',
        help_index : '',
        handler : function (env) {
            console.info("executing selection")
            var cell = env.notebook.get_selected_cell(); 
            execute_selection(cell)
        }
  }

  return {
    // this will be called at extension loading time
    //---
    load_ipython_extension: function(){
        var action_name = IPython.keyboard_manager.actions.register(run_cell_selection, 'run-current-cell-selection', 'issue-252')

        // bind to keyboard shortcut.
        IPython.keyboard_manager.edit_shortcuts.add_shortcut('Ctrl-Alt-Cmd-Enter', action_name) 

    }, 
    //---
  };
})
```

