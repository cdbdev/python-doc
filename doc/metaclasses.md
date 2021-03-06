# 1. Classes as objects
Before understanding metaclasses, you need to master classes in Python. And Python has a very peculiar idea of what classes are, borrowed from the Smalltalk language.

In most languages, classes are just pieces of code that describe how to produce an object. That's kinda true in Python too:
```python
>>> class ObjectCreator(object):
...       pass
...

>>> my_object = ObjectCreator()
>>> print(my_object)
<__main__.ObjectCreator object at 0x8974f2c>
```

But classes are more than that in Python. Classes are objects too.

Yes, objects.

As soon as you use the keyword `class`, Python executes it and creates an OBJECT. The instruction:
```python
>>> class ObjectCreator(object):
...       pass
...
```
creates in memory an object with the name "ObjectCreator".

**This object (the class) is itself capable of creating objects (the instances), and this is why it's a class.**

But still, it's an object, and therefore:

- you can assign it to a variable
- you can copy it
- you can add attributes to it
- you can pass it as a function parameter

e.g.: 
```python
>>> print(ObjectCreator) # you can print a class because it's an object
<class '__main__.ObjectCreator'>
>>> def echo(o):
...       print(o)
...
>>> echo(ObjectCreator) # you can pass a class as a parameter
<class '__main__.ObjectCreator'>
>>> print(hasattr(ObjectCreator, 'new_attribute'))
False
>>> ObjectCreator.new_attribute = 'foo' # you can add attributes to a class
>>> print(hasattr(ObjectCreator, 'new_attribute'))
True
>>> print(ObjectCreator.new_attribute)
foo
>>> ObjectCreatorMirror = ObjectCreator # you can assign a class to a variable
>>> print(ObjectCreatorMirror.new_attribute)
foo
>>> print(ObjectCreatorMirror())
<__main__.ObjectCreator object at 0x8997b4c>
```

# 2. Creating classes dynamically
Since classes are objects, you can create them on the fly, like any object.

First, you can create a class in a function using `class`:

```python
>>> def choose_class(name):
...     if name == 'foo':
...         class Foo(object):
...             pass
...         return Foo # return the class, not an instance
...     else:
...         class Bar(object):
...             pass
...         return Bar
...
>>> MyClass = choose_class('foo')
>>> print(MyClass) # the function returns a class, not an instance
<class '__main__.Foo'>
>>> print(MyClass()) # you can create an object from this class
<__main__.Foo object at 0x89c6d4c>
```

Classes as objects
Before understanding metaclasses, you need to master classes in Python. And Python has a very peculiar idea of what classes are, borrowed from the Smalltalk language.

In most languages, classes are just pieces of code that describe how to produce an object. That's kinda true in Python too:

>>> class ObjectCreator(object):
      pass

>>> my_object = ObjectCreator()
>>> print(my_object)
<__main__.ObjectCreator object at 0x8974f2c>
But classes are more than that in Python. Classes are objects too.

Yes, objects.

As soon as you use the keyword class, Python executes it and creates an OBJECT. The instruction

>>> class ObjectCreator(object):
        pass

creates in memory an object with the name "ObjectCreator".

This object (the class) is itself capable of creating objects (the instances), and this is why it's a class.

But still, it's an object, and therefore:

you can assign it to a variable
you can copy it
you can add attributes to it
you can pass it as a function parameter
e.g.:

>>> print(ObjectCreator) # you can print a class because it's an object
<class '__main__.ObjectCreator'>
>>> def echo(o):
...       print(o)
...
>>> echo(ObjectCreator) # you can pass a class as a parameter
<class '__main__.ObjectCreator'>
>>> print(hasattr(ObjectCreator, 'new_attribute'))
False
>>> ObjectCreator.new_attribute = 'foo' # you can add attributes to a class
>>> print(hasattr(ObjectCreator, 'new_attribute'))
True
>>> print(ObjectCreator.new_attribute)
foo
>>> ObjectCreatorMirror = ObjectCreator # you can assign a class to a variable
>>> print(ObjectCreatorMirror.new_attribute)
foo
>>> print(ObjectCreatorMirror())
<__main__.ObjectCreator object at 0x8997b4c>
Creating classes dynamically
Since classes are objects, you can create them on the fly, like any object.

First, you can create a class in a function using class:

>>> def choose_class(name):
...     if name == 'foo':
...         class Foo(object):
...             pass
...         return Foo # return the class, not an instance
...     else:
...         class Bar(object):
...             pass
...         return Bar
...
>>> MyClass = choose_class('foo')
>>> print(MyClass) # the function returns a class, not an instance
<class '__main__.Foo'>
>>> print(MyClass()) # you can create an object from this class
<__main__.Foo object at 0x89c6d4c>
But it's not so dynamic, since you still have to write the whole class yourself.

Since classes are objects, they must be generated by something.

When you use the `clas`s keyword, Python creates this object automatically. But as with most things in Python, it gives you a way to do it manually.

Remember the function `type`? The good old function that lets you know what type an object is:
```python
>>> print(type(1))
<type 'int'>
>>> print(type("1"))
<type 'str'>
>>> print(type(ObjectCreator))
<type 'type'>
>>> print(type(ObjectCreator()))
<class '__main__.ObjectCreator'>
```

see: [Info](https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python)
