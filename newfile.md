# test 


##  install nbextension


Installing notebook extensions for the notebook. 

you can use either the programatic API with the same name

`ipython install-nbextension path/url`

this shoud install the file for the extension in `/usr/share/jupyterhub` so that they could be share between all notebook users.



to activate extension you need to either load the extension manually in your `custom.js`, but the simplest way is to modify the frontend configuration file(s) to tell the notebook which extension to load.


The frontend config file is a per user file situated in the profile use to run the notebook. By default this shoudl be `~/.ipython/profile_default`, in the profile directory, modify, or create, `nbextension/notebook.json` to contain the following: 

```
{
"load_extensions": [
    'path/to/main/file/of/the/extension1',
    'path/to/main/file/of/the/extension2',
    'path/to/main/file/of/the/extension3'
    ]
}
```