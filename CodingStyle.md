## Style Guide for Python Code in brad2 ##

In Python whitespace is part of the syntax. This seems to restrict the
formating of Python code but in fact there is still an awful amount of
formating styles. The trouble is they don't mix well. And depending on
your choice of editor tabs might be converted on the fly to spaces without
you knowing it.

This is a proposal for the conventions of the brad2 code base. I have used
and developed these rules over the last years and the only thing I can say
is that they work for me. We need to reach an agreement for spaces vs. tabs
(see "coding law" below) but everything else is more or less open for
reevaluation in a particular case.

As [PEP9](http://aspn.activestate.com/ASPN/docs/ActivePython/2.5/peps/pep-0008.html) says:

```
But most importantly: know when to be inconsistent -- sometimes the style
guide just doesn't apply.  When in doubt, use your best judgment.  Look
at other examples and decide what looks best.
```


## The Guide ##

### Coding Law ###

Unbreakable and unbendable rules that have to be followed -- or amended.

  1. Use 4 spaces - not tabs - for indentation.
> > This follows the Python style guides but is apparently not very
> > popular among Blender scripters. They prefer tabs. We have to
> > chose one way.


### Guidlines ###

  1. See the Zen of Python below.
  1. Use docstrings. A lot.
  1. Keep lines below 80 characters.
  1. Use `camelCaseNames` for long method and funcion names.
  1. Use lower case only for local variables?
  1. Use leading `_underscores` for non-public methods and variables.
  1. Use `UPPERCASE` for 'constant-like' variables.
  1. Qualify external methods and functions with the modules name:

```
   import bradscene
   ...
   scn = bradscene.scene()
```


> is better than

```
   from bradscene import scene
   ...
   scn = scene()
```
  1. Use `veryLongNamesForMethods` when it helps to structure the code.
  1. See PEP 8 for more ideas.


## The Zen of Python ##

(by Tim Peters)

```
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```


