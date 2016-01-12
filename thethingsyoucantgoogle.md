# The things you can't Google

There are inherently some things that are hard to Google.
Let's take for example `html5 website` (I want https://html5.org/). Google has difficulties understanding. You might encounter a few of these during web development.

![cannotknow](cannotknot.jpg)


# jQuery (aka `$`)

In Javascript `$` is a valid identifier. By convention, a widely used Javascript library known as [`jQuery`](jquery.org) injects itself in global namespace as `$`. Unless when it's not, in which case `$` might be browser native interface that looks and behave almost like jQuery, but only in console.

If you could write jQuery as a python package, it is would be the following.

A callable module that behaves sometimes as a class constructor, and has static methods that are getters and setters (with the same name, depending on the number of parameters). It also executes code at startup, add some importhook mechanism and probably rewrite the Ast.
[Q](https://pypi.python.org/pypi/q) is probably the kind of things that get closest to that. It is awful, but we have not done better, and you can't live without it.

Anyway, you will see different constructs: `$(...)`, and `$.something`. The first is a convenient wrapper around DOM (or collection of DOM) elements. It allow you to get some short and readable syntax like `$('.cell:odd').remove()` instead of:

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


`$.` are just generally utility functions.


## This or that ?

In Javascript you will often find the following construct :

```
var that = this;
```

This, or should I say that, is often confusing for the newcomer, especially if they come from a Python background. The reason is simple: the parallel is easy between `self` and `this`. Though, as Javascript is prototype based and does not really have the notion of objects like python, `this` does often not refer to what the experienced Pythonista thinks is the current object.

In Javascript, there is no real difference between objects and functions. When the programmer mimics the class inheritance and believes that they are actually creating methods on a class for which `this` will refer to the current object, he is mistaken. The keyword `this` always refers to the current context the object is in, which by default is the **current function**.

Let's take for example the following piece of code, that could be thought of as the `execute` method of a cell:

```javascript`

Cell.prototype.execute = function(){
  this.kernel.execute(this.code)  // this refers to a Cell object.
}
```

Let one want to delay the execution, one is tempted to write:


```javascript`

Cell.prototype.execute = function(delay){
  var do_ex = function(){
    this.kernel.execute(this.code)  // this refers to `do_ex` object.
  }

  setTimeout(do_ex, delay);
}
```

As the comment points out, `this` does refer to the current function. The way around that is to use a closure around `that`, hence the `var that = this`.
A second similar construct one could find, is the use of `$.proxy`. That will set the context (value of `this` of a callback). It is about the same as using a closure except for the fact that `$.proxy` can be used on functions you did not construct, or for which you cannot create a closure around the context you like.

## Underscore (aka `_`),

One of the beauties of javascript is its ability to use ungoogleable names that have ambiguous meaning. This is one of the reasons you find `.js` or `js` suffixes in javascript to lift the ambiguity. Though it is not always the case. In particular you will find a few modules that have the good habit of being bound (or being themselves ) to `_`. Thus you might see things like `_.map`, `_.proxy`,`_.filter`, ...

Most of the time the library bound to `_` is called "Underscore", but also rarely named "Underscore.js". It provides a few utility functions.

## use strict

## unlimited argument, default to undefined

## require

## IIFE

You might find the following Here and there. These are Inmediately Invoked Function Expression. You might find them:

 - At module/file level
 - In loops.

```
X = (function(A){
  // do stuff
  // loosely
  // var X = something(A)
  //
  return X
})(A)
```

(looks like lisp right ?)

These are basically to work around some scoping problem in JS. Imagine that in python:

```
>>> for i in range(5):
>>>    print(i)
5
5
5
5
5
```

You could fix that by:
```
myfun = lambda x:print(x)
for i in range(5):
    myfun(i)
```
That can be rewritten as

```
for i in range(5):
    (lambda x:print(x))(i)
```

Same in js

```
  //...
  X = do_stuff(A)
```

wrap in a function

```
function(A){
  ...
  return do_stuff(A)
}
```
Make it an expression

```
(
function(A){...}
)
```

Call with A as parameter, and assign to X

```
X = (function...)(A)
```
