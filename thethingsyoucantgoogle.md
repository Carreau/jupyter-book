# The things you can't Google

There are inherently some things that are hard to Google. 
Let's takes for example `html5 website` (I want https://html5.org/). Google have difficulties understanding. You might encounter a few of these during web development.

![cannotknow](cannotknot.jpg)


# jQuery (aka `$`)

In Javascript `$` is a valid identifier. By convention, q widely use Javascript library known a [`jQuery`](jquery.org) inject itself in global namespace as `$`. Unless when it's not. In which case `$` might be browser native interface that looks and behave almost like jQuery, but only in console.

If you could wrote jQuery as a python package, it is would be the following. 

A callable module, that behave sometime as a class constructor, and have static methods that are getters and setters (with same name, depending on the number of parameter). It also execute code at startup, add some importhook mechanisme and probably rewrite the Ast. 
[Q](https://pypi.python.org/pypi/q) is probably the kind of things that get closes to that. It is aweful, but we have not done better, and you can't live without. 

Anyway, you will see different construct: `$(...)`, and `$.something`. The first is a convenient wrapper around DOM (or collection of DOM) elements. It allow you to get some short and readable syntax like `$('.cell:odd').remove()` instead of:

```
var elts = document.getElementsByClassName('cell');
var to_remove = [];
for(var i in elts){
    if (i%2 === 0){
        to_remove.push(es[i]);
    }
}

...

```

Try think of `$(...)` as objects with superpowers.


`$.` are just generally utilities function. 


## This or that ?


## Underscore (aka _), 


## require



