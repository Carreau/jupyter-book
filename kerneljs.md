# Kernel js 


Some of Jupyter kernel might need to load custom javascript on the page. If you decide to do so, 
then the unloading on this javascript is not guarantied, and if your user want to switch back to another kernel they will most likely need to refresh the page. 

We might even enfore the reloading of the page for kernel shipping with custom javascript. 



Though, if you like to ship Javascrit with your kernel(s) and have it loaded when user switch to your kernel, or open a notebook workign with your kernel, then you need to put a `kernel.js` file in the kernelspec folder of your kernel.

It is recommended for this file to define an anonymous module that exposes (at least) the `onload` method which take no arguments.

Here is a minimal version of such a file:


```javascript
define(function(){
    
    var onload = function(){
        console.log("I am being loaded")    
    }
    
    return {onload:onload}
})
```


You will notice the presence of `define()` instead of `require()` use in previous `custom.js`. This example do not have any dependencies, but you can express them as in any other extensions.

The `onload` method will be called by Jupyter/IPython at the right time

