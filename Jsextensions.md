# JS extension. 

Contrary to general IPython extensions, Jupyter notebook extensions are written in javascript (or css).
They are used to change the behaviour, functionality and UI of the Jupyter notebook. 

Most of the time, you  should install *nbextensions* in the directory of the same name, either system wide or in your user profile. Most nbextensions provide the documentation to indicate how they should be installed.

## Activating nbextensions

This part describe how to decide which nbextensions are loaded. If you are an end-user, and not interested in developing your own nbextension, you can skip this section.

`Nbextension` can easily be loaded using their name, if

  - they are installed in one of the local, or global `nbextension` folder
  - their entry point is named `<extensionname>.js`
  - the extension defines an AMD module definition that exposes the method `load_ipython_extension`

Here is a minimal example of AMD extension.

```javascript
define(function(){
  return {
    // this will be called at extension loading time
    //---
    load_ipython_extension: function(){
        console.log("I have been loaded ! -- my nb extension");
    }
    //---
  };
})
```
Without further doing, the extension can be dynamically loaded inside of a Jupiter notebook:

```javascript
%%javascript
IPython.load_extensions('custom_shortcuts');
```

To load the extension automatically on start up, the IPython configuration has to be modified. 
To see the current value of the config in a noteobook, open a Jupiter notebook, and type the following in the javascript console of your browser: 

```javascript
IPython.notebook.config
```

The value should be something like `ConfigSection {section_name: "notebook", base_url: "/", data: Object, …}`.
If no extensions are installed, the `data` attribute (``IPython.notebook.config.data``) should be empty.

By updating the config, we can register a new extension to be loaded on start up.

First, the new config value is created:
```
var new_data = {"my_ext":true}
```
The value assigned to `"my_ext"` should be non-`null`. Setting the value to `null` would remove the key from the configuration object. However, to keep everthing clear and clean, `true` should be used.

To actually update the config file, the update function of the config object has to called:

```
IPython.notebook.config.update(new_data)
```

Or as a one-liner:

```
IPython.notebook.config.update({"my_ext":true})
```


The config is technically store as a json object in  `~/.ipython/profile_default/nbconfig/notebook.json`

For example in  our case : 

```
// notebook.json
{
  {"my_ext":true}
}
```

You should not update this file manually as it can be used by extension authors to modify configuration programatically. Extensions authors can also provide more convienint methods to activate extensions. 








