---
layout: post
title: Python Fundamentals (in progress!)
---

**Note: Based on Python Fundamentals on Pluralsight**

# Table of Contents
  * [Chapter 1 - Python Fundamentals](#chapter-1)

## Chapter 1 - Python Fundamentals <a id="chapter-1"></a>

### Python Scope (Context) Hierarchy
* Python Name Scope looked ups occur first in the narrowest relevant context and then higher in hierarchy if not found

* Built-in (defined in Python `builtins` module)
* Global (defined at top-level of module)
    * i.e. Module-Scope Names bindings introduced by `import`, `def`, `class` definitions, and special variables like `__name__`
* Enclosing (any/all enclosing functions)
* Local (defined in current function) - Narrowest
    * i.e. Local-Scope Names (within `def` function scope). References destroyed when function completes.

* Rebind a global name in the Module-Scope from within a Local-Scope (function)
{% highlight python %}

# Binds Object referred by arg to new name count inside 
# innermost namespace context (scope of current function). 
# The arg variable shadows and prevents access to global of same name,
# unless we instruct Python to resolve to count variable in Module Namespace
count = 0
def my_func(arg):
    global count
    count = arg

my_func("a")
{% endhighlight %}

### Python Object-Oriented
* Everything is an Object in Python

{% highlight python %}
import words
type(words)         # <class 'module'>
dir(words)          # introspects module object and lists the module's attributes
type(words.my_func) # <class 'function'>
dir(words.my_func)  # introspects function object and lists the functions attributes
words.my_func.__name__
{% endhighlight %}


### Python Type System
* Python is strongly-type as all objects have definite type
* Python is dynamically-typed (object types only resolved at runtime due to implicit type conversion) with no type checking by compiler prior to running
* Python variables are just untyped named bindings to underlying Objects.
Python variables may be rebound or reassigned even to Objects of different types.
* Python bindings (i.e. named binding to underlying Object) are stored
according to the Python Name Scope (context) and also looked up
* Python uses duck-typing where object suitability for context determined at runtime 
* Pythong is partly an interpreted language compiled invisibly in form 
of byte code before execution so not noticeable delay to user
* Scalar types
    * `int`
        * unlimited precision
        * specify in binary (prefix `0b__`), octal (prefix `0o__`) or hex (prefix `0x__`)
        * integer constructor (i.e. `int(-3.5) # -3.5`)
            * convert from base3 (i.e. `int("10000", 3) # 81`)
    * `float` (i.e. `3.5e8`, `1.25`)
        * float constructor convert from int (`float(7) # 7.0`)
        * float constructor convert from string 
        (`float("1.6") # 1.6`, `float("nan") # nan`, `float("-inf") # -inf`)
        * promotes to float (i.e. `1.4 + 2 # 3.4`)
    * `NoneType` (i.e. `a is None # True`
    * `bool`
        * 0, 0.0, [], and "" is False, all others are True
        * bool constructor (i.e. `bool(0) # True`)
        
* Relational operators (i.e. `==, !=, <, >, >=, <=`)
* Augmented assignment operators (i.e. `-=`)
* Conditional statements (branch execution based on expression value)

{% highlight python %}
if a > 1:
    print("")
elif a < 1:
    print("")
else:
    print("")
{% endhighlight %}

* While Loops

{% highlight python %}
c = 4
while c != 0:
    if c % 2:
        break   # break out of innermost loop to first statement after loop
    print(c)
    c -= 1
print("broken out")

while True
    response = input()
    if int(response) % 3 == 0:
        break
{% endhighlight %}

* For Loops

{% highlight python %}
names = ["joe","luke"]
for name in names:
    print(name)

names = {"a1": "joe","a2": luke"}
for name in names:
    print(name, names[name])
{% endhighlight %}

* Collection types
    * `str` (Strings) (immutable sequence of Unicode codepoints, like characters)
        * Delimit Literal Strings with single quote i.e. `'hi'`, or use double quote inside `'"hi"'`
        * Concatenate Strings Automatically i.e. `"blah" "blah"`
        * Strings with Newlines
            * Multiline Strings OR Escape Sequences (using Universal Newlines `\n`)
            https://docs.python.org/3/reference/lexical_analysis.html#strings
            
{% highlight python %}
a = """Blah
blah blah"""

b = '''Blah
blah blah'''

c = 'Blah\nblah blah'
print(a)
print(b)
print(c)

"blah\"\'blah"
{% endhighlight %}

* Cont'd
    * Cont'd
        * String are Unicode as source encoding in Pythnon 3 is UTF-8 so international characters may be used
        * String Operation Methods `help(str)`
        * Strings are immutable so are not modified in place and instead return new string
        
{% highlight python %}
>>> c = 'abc'
>>> c.capitalise()
'Oslo'
>>> c
'oslo'

>>> a = 'aabsbabbabaabbbbii'
>>> a.count('a', 5, -1)
4
{% endhighlight %}        
* Cont'd
    * Cont'd
        * Characters are single element Strings i.e. `a = 'abc'; type(a[0]) # <type 'str'>`
        * Strings are Sequence Types (support operations to Query Sequences) i.e. access certain chars with zero-based index `a[0]`
        * Raw Strings `r'__'` (does not support escape sequences, so WYSIWYG and overcomes ugly doubling up of backslashes in a string)
    
{% highlight python %}
a = r'C:\blah\blah'
{% endhighlight %}
     
* Cont'd
    * `bytes` (Bytes) i.e. prefix `b'data'`
        * Byte Streams are how Files, Network Resources, and HTTP responses are transferred
        * Unicode is the format we prefer to use
        * Convert from Bytes to Strings by knowing the [Encoding](https://docs.python.org/3/library/codecs.html#standard-encodings) 
        of Byte Sequence used to represent a Strings Unicode codepoints as Bytes

{% highlight python %}
d = b'some bytes' 
b.split() # return list of byte objects
# => [b'some', b'bytes']
{% endhighlight %}

* Cont'd
    * Cont'd
        * Example: Encode Unicode string into Bytes object and decode to reverse process

{% highlight python %}
uni = "Àbc"
data = uni.encode('utf-8')
french = data.decode('utf-8')
french == uni
{% endhighlight %}


* Cont'd

    * `list` (List / Array)
        * mutable Sequences of Objects
        * Literal Lists i.e. `[1,2,3]`, `a[0]`, `a[1] = 3`
        * Create empty list `a = []`
        * Manipulate list i.e. `a.append(4)`
        * List Constructor i.e. `list("blah") # ['b','l','a','h']`
        * Comma after last element allowed for maintainability
        
{% highlight python %}
c = ['a',
     'b',]
c # => ['a','b']
{% endhighlight %}       

* Cont'd

    * `dict` (Dictionary / Associative Array)
        * mutable Mapping of Keys to Values

{% highlight python %}
d = {}          # Empty Dictionary
d = {'a1': 'b', 'a2': 'c'}
d['a2']         # Get K/V
d['a1'] = 'k'   # Update K/V
d['a3'] = 'g'   # Create K/V
{% endhighlight %} 

* Cont'd
    * `with` (With)
        * String to Bytes use `encode`
        * Bytes to String use `decode`

{% highlight python %}
with urlopen('http://www.blah.com/a.txt') as story
    story_words = []
    for line in story:
        # convert from byte to string
        line_words = line.decode('utf-8').split()
        for word in line_words:
            story_words.append(word)
story_words
{% endhighlight %} 

### Modularity
* Structure and organisation
* Combinable, Self-contained, Reusable pieces
* Module code files are collections of grouped and related reusable functions
* Avoid introducing circular dependency when importing modules into other modules
* Modules functions may be imported into REPL and also run as a script when 
special Python runtime system idiom delimited by double underscore is used
such as `__name__` (evaluates to `__main__` if run directly as program outside REPL or actual module name
depending on how enclosing module being used).
* Example: `__name__` variable detects whether Module run as script or 
imported into another module or REPL
{% highlight python %}
# words.py
def my_func():
    print("")
print(__name__)

>>> import words    # module code only executed once on import
words

$ python3 words.py
__main__

{% endhighlight %} 

* Example: Check the value of `__name__` and only run the function as a script 
at the command line if the value is `__main__`, else otherwise if detect Module
being imported into REPL then only import it but do not execute the function
{% highlight python %}
# words.py
def my_func():
    print("")
if __name__ == '__main__':
    my_func()

>>> import words    # module code only executed once on import
words

$ python3 words.py
__main__

{% endhighlight %} 

### Functions

* Situations when None returned from function

{% highlight python %}
def my_func():
    print("")   # returns None

def my_func(a):
    if a == "a":
        return  # returns None
    print("")

>>> None == my_func("a") # True
>>> None is my_func("a") # True
{% endhighlight %} 

* Applying "Default Arguments" convert them to "Optional" Arguments
    * "Optional" Arguments are always listed last
    * "Default Arguments" are evaluated only once when `def` is evaluated
    * "Default Arguments" should always be only **immutable** Objects like Integers or Strings, NOT Dicts like `[]`

{% highlight python %}
def function(a, b="abc"):
    ...
{% endhighlight %} 

### Objects

* Variables in Python are just named References to Objects
    * Creates an **immutable** int Object with value 100 (1)
    * Create Object Reference with name x (2)
    * Arranges (2) to refer to (1)
    * Re-assigning a new value to (2) does not change (1). Python
    creates a new **immutable** int Object with value 50 (3). Python
    redirects (2) to instead refer to (3)
    * Python Garbage Collector (GC) collects unreachable (1)
    * Assigning x to y creates another Object Reference from y to (3). 
    Both x and y point to same int Object
    * Re-assigning x again creates an **immutable** int Object with value
    200 (4) that x points to instead, but y still refers to (3). No work
    for GC as all objects reachable from live references.

{% highlight python %}
x = 100
x = 50
y = x
x = 200
id(x)
id(y)
{% endhighlight %} 

* List with References to Objects
    * Create **mutable** List object (1)
    * Bind (1) to reference named `r` (2)
    * Assign `r` to new reference named `s` (3)
    * Modify (3) also changes (2) as they both refer to underlying Object (1)
    
{% highlight python %}
r = [1, 2]
s = r
s[0] = 3
s is r # True (since underlying Object ID is the same)
{% endhighlight %}

* `id()` built-in function returns constant unique id for Object's lifetime (not the Reference to the Object)
* Assignment Operator only binds to names, only Passes Object by Reference, but never Copies Object by Value

* Value Comparison
    * Defining Types allows control of how class determines Value Equality
* Identity Comparison - controlled by language so cannot change behaviour

* Passing an Object Reference (variable) as Argument to a Function
    * Assigns the actual Object Referenced by the Argument to the local argument accepted by the function
    * Passes Object by Reference occurs whereby value of reference is copied into function
    argument, but not the actual value of the referred object as no objects are copied.
    
* Modify Copy of an Object
    * Function's responsibility to perform the copying

### Python Execution Model
* Describes when code evaluated and executed
* Python Module (**convenient import** with API) is any .py file
* Python Script (**convenient execution** from command line)
* Python Module/Script Hybrid (when postscript used to facilitate execution only running when `__name__ == __main__`
* Python Program (composed of numerous Modules)
* Consider calling a `def main():` function to run various functions subject to Single Responsibility Principle
* Pass argument to Python Program using sys.argv[1] `python3 prog.py "abc"`

{% highlight python %}
import sys

def main(arg):
    print(arg)

if __name__ == '__main__':
    main(sys.argv[1])
{% endhighlight %} 

### Python Syntax
* Whitespace delimits code blocks to avoid unnecessary parenthesis
* Convention is 4 space indentation
* Convention is two blank lines between top-level functions

### Python Implementations
* CPython (standard)
* Jython (targets JVM)
* IronPython (targets .Net)
* pypy (targets RPython and written in this subset)

### Python Standard Library (aka "batteries included")
https://docs.python.org/3/library/

### Python Community Code Goals
* Simplicity, readability, explicitness

### Python Language Development
* Python Enhancement Proposals (PEPs)
* [PEP 8 Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/)

### Python Principles
* `import this` 

### REPL
* `_` underscore refers to last printed value
* `fg` reactivates suspended Python REPL as foreground process

### Importing Libraries
* Import `from x import (a, b, c)`
* Alias `from x import y as z`

### Math
* Integer division operator `//`
* Python can compute large integers limited only by computer memory
* Show length of large number `len(str(n))`

### Shebang
* Allows program loader to know what interpreter (i.e. Python 2 or 3) to use to run program
* Change access mode of file to allow use of Shebang
* Script may then be run directly instead of being prefixed with `python` or `python3`
`#!/usr/bin/env python`
`#!/usr/bin/env python3`
`chmod +x words.py`
`./words.py`

### Generate API Documentation
* Docstrings explain how to consume modules, not how it works
* Code comments explain why an approach is chosen
* Docstrings are literal strings occurring as first statement in named block (function or module)
* Access Docstrings via Help in REPL
* Sphinx for HTML documentation from Docstrings

words.py
{% highlight python %}
# Module Docstring (before any statements or comments)
"""Purpose:
    __
    
Usage:
    __
"""
import sys

def main(arg):
    # Function Docstring
    """Purpose: 
        __
    Args:
        __
    Returns: 
        __
    """
    print(arg)

if __name__ == '__main__':
    main(sys.argv[1])
{% endhighlight %}

{% highlight bash %}
$ python
>>> import words
>>> help(words)
{% endhighlight %}

### Python Libraries
* Moment for dates Python https://github.com/zachwill/moment
* Argparse for Python Command Line Arguments parsing https://docs.python.org/3/library/argparse.html
* Docopt for 3rd party Command Line interface https://github.com/docopt/docopt

### TODO
* Kivy - Multitouch Python apps https://kivy.org/#home


