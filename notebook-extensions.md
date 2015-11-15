# Notebook extensions

Notebook extensions, or plugins, allow the end user to highly control the behavior and the appeareance of the Notebook application.

Extensions capability can highly vary from being able to load notebook files from [Google Drive](https://github.com/jupyter/jupyter-drive), or PostGresSql server, presenting the Notebook in the form of Slideshow, or by just adding a convenient button or keyboard shortcut for an action the user is often doing.

The way we write Jupyter/IPython is to provide the minimal sensible default, with easy access to configuration for extensions to modify behavior.


Extensions can be composed of many pieces, but you will mostly find a Javascript part that lives on the frontend side (ie, the Browser, written in Javascript), and a part that lives on the server side (written in Python). We will, for a first time, mostly focus on the Javascript side.

A large number of repos exist here and there on the internet and we haven't taken the time to write a Jupyter Store (yet) to make extensions easily installable. Well I suppose this can be done as an extension, and your research on the web will probably show that it can be done, but we will still focus on the old manual way of installing extensions to learn how things work because that's why you are here right ?

Here are links to some active repos with extensions:

 - https://github.com/ipython-contrib/IPython-notebook-extensions – check out the right branch depending on your version of IPython. If you are using IPython 3.x you most likely want extensions from the 3.x branch.
 - https://bitbucket.org/ipre/calico/ look in the notebook/nbextension folder.


I've made a minimal extension for you in extensions. Link or move it over into:

```bash
$ ls ~/.ipython/nbextensions/
hello-scipy.js
```

Now let's open a notebook and configure it to load the extension automatically.
In a new notebook, or the one I provided with a reminder of the instructions, open the developer console
and enter the following:


```js
IPython.notebook.config.update({
  "load_extensions": {"hello-scipy":true}
})
```

Now Reload your page, and observe the Javascript console, it should tell you what to do next !

## Explanation

Do not be preoccupied with what `IPython.notebook.config.update` is. We will see that later.

The `"load_extensions"` part takes a dict with the name of extensions and whether they are loaded
or not. It is one of the config values which is now stored on server side.

There is a way to activate extensions from outside the notebook but we won't use that for now.

### The extension

```js
define(function(){

    function _on_load(){
          console.info('Hello SciPy 2015')
    }

    return {load_ipython_extension: _on_load };
})
```

The define call: `define(function(){` suggests we have no dependencies.

For readability we define a function that will be called on notebook load at the right time.
We keep the python convention that `_xxx` indicates a private function.
```js
  function _on_load(){
        console.info('Hello SciPy 2015')
  }
```

We only export a function called `load_ipython_extension` to the outside world:
`return {load_ipython_extension: _on_load };`. Anything outside of this dict will
be inaccessible for the rest of the code. You can see that as Python's `__all__`.

Note that you will find legacy extensions on the internet that **do not define**
`load_ipython_extension` and rely on IPython's Events, and `Custom.js`.
While this does work for the time being, these extensions will break in the future
and are subject to race conditions.

While our Javascript API is still highly in motion, and not guaranteed stable,
we will try our best to make updating extensions that use `load_ipython_extension` easier
than the ones using Events and `custom.js` !


## New keyboard shortcut !

Now let's modify our extension in order to be able to actually modify the
User interface. We will try to create a shortcut that kills the kernel without confirmation,
clears all the cell output, and finally re-runs all cells.

First, we want to get access to the IPython instance. To do so we want
to import the right module so that the `IPython` variable can be used safely.

Change the first line to the following


```js
define(['base/js/namespace'],function(IPython){
```

I remind you that this is basically equivalent to :

```python
import base.js.namespace as IPython
```


Now in your `_on_load` you can access `IPython.<things>`. If you fail
to use the above way of declaring import, IPython might still be accessible on your
machine with your current workload. Though it might break in some cases.
Using `define([...])` insures in the dependency graph that the right file is loaded
and that the local name will be `IPython` (hint, in next release the global name might be `Jupyter`).

Now let's make a detour and [Keyboard Shortcut](./keyboardshortcut.md).

A few things you might need :

```javascript
var internal_name = IPython.keyboard_manager.actions.register(data, name , `scipy-2015`)
IPython.keyboard_manager.command_shortcuts.remove_shortcut(string)
IPython.keyboard_manager.command_shortcuts.add_shortcut(string, internal_name)
```

The notebook instance has a `clear_all_output` method, and a `kernel` attribute.
The `kernel` instance has a `restart` method that uses `on_success` and `on_error` callbacks.


...

have you figured it out ?

My solution:

```js
function (env) {
    var on_success = undefined;
    var on_error = undefined;

    env.notebook.clear_all_output();
    env.notebook.kernel.restart(function(){
          setTimeout(function(){ // wait 1 sec,
              // todo listen on Kernel ready event.
              console.log('executing all cells')
              env.notebook.execute_all_cells()
          }, 1000)
        },
        on_error // Todo also
    );
}
```

```js
// register our new action
var action_name = IPython.keyboard_manager.actions.register(
      clear_all_cell_restart,
      'clear-all-cells-restart',
      'scipy-2015')

// unbind 00
IPython.keyboard_manager.command_shortcuts.remove_shortcut('0,0')

// bind 000
IPython.keyboard_manager.command_shortcuts.add_shortcut('0,0,0', action_name)
```


### Why use an action?

How are things up until now ? You might feel like the code is a bit too verbose,
and that some parts are unnecessary right ? Now we will start to see why we use such
verbose methods.

You might have seen that some attributes of actions seem to be unused.

```
help: 'Clear all cell and restart kernel without confirmations',
icon : 'fa-recycle',
help_index : '',
```

Now that your extension works go take a look in the help menu, keyboard shortcut submenu.
If all is fine, you should see your new shortcut in there, with the help text.
The help index is use to order/group the common shortcuts together. The only last
unused piece is the icon.

With all these attribute, you can easily bind an _action_, to either a keyboard shortcut,
a button in a toolbar, or in a menu item (api is not there yet for that though).
We often saw people wanting the same action in two places, and duplicating code,
which is a bit painful. By defining actions separately it is easy to use these
in many places keeping it DRY. This also allows you to distribute actions libraries
without actually binding them and lets the users do their own key/icon bindings.

Let see that in next section with toolbars.

### Toolbars.

This will be pretty simple since you already did all the work :-)

You just need to know that the following exists, and takes a list of action names:

`IPython.toolbar.add_buttons_group`

Now, go edit your custom extension ! You can also try to install the `markcell.js` extension,
`require()` it in your extension and try to use some of the methods defined in it.
This shows you how to spread your extension potentially across many files.

Here is my solution:

```
IPython.toolbar.add_buttons_group(['scipy-2015.clear-all-cells-restart','ipython.restart-kernel'])
```

each call to this API will generate a new group of buttons with the default icons,
and if you hover the button the help text will remind you of the action.



### Interact with user

You can ask a value with the `base/js/dialog` module that has some convenience functions.

This module has a `modal` function that you can use like this:

```
dialog.modal({
    body: text_or_dom_node , // jQuery is you friend
    title: string,
    buttons: {
      'Ok':{
            class: 'btn-primary',
            click: on_ok_callback
            },
      'Cancel':{
              //... (or nothing to just dismiss )
            }
    },
    notebook:env.notebook,
    keyboard_manager: env.notebook.keyboard_manager,
})
```


### Server side handler.

Ok, enough javascript (for now). Let's get back into a sane language.
Notebook extensions on the client-side have been there for quite a while
and we recently added the ability to have a server side extension.

Server side extensions are, as any IPython extension, simply Python modules that
define a specific method. In our case `load_jupyter_server_extension`
(Yes we are ready for the future).

Here is the minimal extension you can have:

```python
def load_jupyter_server_extension(nbapp):
    pass
```

I've already provided that for you in the `extensions` dir.
You can try to run the following.  If you look at the console while starting the notebook
you will be able to see a new login message.

```
python3 -m IPython  notebook --notebook-dir=~ --NotebookApp.server_extensions="['extensions.server_ext']"
```


Now let's add a handler capable of preating requests:
