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


```
define(function(){
    return {
    load_ipython_extension: function(){
            console.log("I have been loaded ! -- my nb extension");
        }
    };
})
```





