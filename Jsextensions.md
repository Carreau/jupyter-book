# JS extension. 


javascript extensiosn are pure javascript extensions that are loaded  on the browser side. 

They are used to change the behaviour, functionality and UI of the Jupyter notebook. 

Most of the time, you  should install *nbextensions* in the directory of the same name, either system wide. You can also install *nbextensions* on your user profile. Most nbextensions provide the documentqtion to indicate how they should be installed.

## Activating nbextensions

This part describe how to decide which nbextensions are loaded. If you are an end-user, and not interested in developping your own nbextension then you can skip this section.


`Nbextension` can be loaded by provding only their name, but to meet this contidion the shoud follow the following criteria.

  - Install in one of the local, or global `nbextension` folder
  - Their entry point should be named `<extensionname>.js`
  - The extension should define an AMD module definition that exposes the method `load_ipython_extension` which takes not aguments.


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


To decide which extension will be loaded, Jupyter notebook will look at its configuration, and in particular to the value of the `load_extensions` key.

Yo see the current value of the config in a noteobook, open an IPython  notebook, andtype the following in a console: 

```javascript
IPython.notebook.config
```

The value shoudl be something like `ConfigSection {section_name: "notebook", base_url: "/", data: Object, …}`.
 
This give you access to the `config`object which handles loading, and updating the config. By default  the config shoudl be empty. Acees the `data`attribute to check it: 

```
var data = IPython.notebook.config.data
data
```

Responds with `Object {}` in my case.

Let's update the config 
```
data['load_extensions'] = {"my_ext":"main"}
```

And update the config with the new value :

```
IPython.notebook.config.update(data)
```

The config is technically store as a json object in  `~/.ipython/profile_default/nbconfig/notebook.json`

For example in  our case : 

```
// notebook.json
{
  "load_extensions": {"my_ext":"main"}
}
```

You should not update this file manually. It can be used by extension authors to modify configuration programatically. Extensions authors can also provide convenience methods to activate extensions. 








