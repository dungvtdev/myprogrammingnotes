# Core python

> ```python
> def learnedOneThing(you):
>     you.success += 1
> ```

## Simplest

- PYTHONPATH: path de python interpreter load cac module tu cac path duoc liet ke
	+ sys.path

- __//__: interger division/floored quotient  22//8 = 2
- __**__: Exponent
- __'str' * n__: 'str...str' n times, n: int.
- __'str'+'str2'__
- __'str' + other_type__: error
- __and, or, not, &, | for boolean operations__
- True, False, not true, false.

__Types__
- cast: int(), float(), str(), bool()
- None is __NoneType__

__Tricks__
- range(begin, end, step) __not include end__.
- list-like[begin:\end:step] __not include end__.
    - bat dau tu begin, do theo step den end.
- multi assignment: size,color = ['fat','orange']
                    a, b = b, a
- list * n : ['a'] * 3 = ['a','a','a']
- raw strings: __r__'That is Carol \'s cat.' -> That is Carol \'s cat.
- multi-line string: '''[multi-line string]'''
- __list[n]=value, n is must in range(0,len(list))__
- __dict[key]=value, k may not be in dict.keys()__

__Input/output__
- python2
    + __print 'str',__  : khong bi xuong dong
    + a=input('Name'):
        * Neu dat trong "" (ex "Dung"), return string
        * Neu khong, return 1 ten bien
    + a= raw_input(''): luon return string.

__is vs ==__
- is so sanh 2 object cung tro toi 1 vi tri
- == so sanh 2 gia tri bang nhau
- ex: 
```python
a =[1,2,3]
b = a
a is b  # True
a == b  # True

b = a[:]
a is b  # False
a == b  # True
```

__ListMethods__
- index(), append(), insert(), remove(value), 
- sort(): increase, sort(reverse=True): decrease, value must comparable.
- sort(key=str.lower): lowercase before sorting.
- list(('a','b')): tuple -> list

__StringMethods__
- str.join(interable): 
    + interable : x for x in list-like
    + join x-items, separate with str.
- str.join(list-like):
    + ex: ','.join(['a','b']) -> a,b
- str.split(sep)
- upper(), lower(), isupper(), islower()
- isalpha(), isalnum(), isdecimal(),istitle(), isspace():space | tab | new-line,
- startswith(str), endswith(str)
- rjust(len,sep-char), ljust(len,sep-char), center(len,sep-char): extends str with sep-char until have total len char.
- strip(), rstrip(), lstrip(): strip spaces.

__Tuple__
- t = ('a') not a tuple, t is string
- t= (1)...
- a = tuple([1,2,3]): list -> tuple

__Set__
```python
s = set([iterable])
s.union(t)
s.intersection(t)
s.difference(t)
...
```

__Dictionary__
- unordered -> can't sort, slice...
- __key__ types: int, float, string, tuple.
- key in dict: check if key in dict.keys()
- dict == dict2
- .keys(), .values(), .items(): [(key,value)]: return list-like objects (dict\_keys, dict\_values, dict\_items), but unmutable
- .get(key, fallback)
- .setdefault(key,value): set value if key doesn't exist.

__type(o)__: return type of a object.

__sys.exit()__: must __import sys__, exit program.

__global__: use global [variable] keywork define a global variable in localscope if u want to assign it a value in local scope.

## Regex
- import re
- Match object: m
    + group theo ngoac: \d\d(\d\d)(\w) -> 2 part
    + m.group(n): n=0: all, n=1,2..: part 1,2..
    + m.groups() return tuple of groups.
- re.compile('.*', re.DOTALL): . as spaces.
    + re.I : ingorecase
    + re.VERBOSE: cho phep regex nhieu dong, co comment, bo qua spaces va comments
        * ex: '''\d #comment'''

__CheatSheet__

Character classes
- .   any character except newline
- \w \d \s    word, digit, whitespace
- \W \D \S    not word, digit, whitespace
- [abc]   any of a, b, or c
- [\^abc]  not a, b, or c
- [a-g]   character between a & g

Anchors
- ^abc$   start / end of the string
- \b  word boundary

Escaped characters
- \\. \\* \\    escaped special characters
- \t \n \r    tab, linefeed, carriage return
- \u00A9  unicode escaped ©
- Groups & Lookaround
- (abc)   capture group
- \1  backreference to group #1, __use for sub method__
    + ex: (.+) \1 match "4242"
- (?:abc) non-capturing group
- (?=abc) positive lookahead
- (?!abc) negative lookahead

Quantifiers & Alternation
- a* a+ a?    0 or more, 1 or more, 0 or 1
- a{5} a{2,}  exactly five, two or more
- a{1,3}  between one & three
- a+? a{2,}?, a*?, a??  match as few as possible (non-greedy)
- ab|cd   match ab or cd

## Exception

- KeyboardInterrupt: input() waiting -> interrupt come from user.

## Assert

- assert bool_expression, comment_when_false
    + raise AssertionError and crash the application, can catch with try-except
- disabling assertings:
    + use option -O when run python. ex python -O a.py
                                                                                                                                                                                                 
## Color text
```python
    print("\033[1;32;40m Bright Green \033[0m \n")
```
- \033[0m: ve mac dinh
- \033['text_style';'text_color';'bg_color'm
- text_color: Black,Red,Green,Yello,Blue,Purple,Cyan,White = [30..37]
- bg_color: [40..47]
- text_style: NoEffect,Bold,Underline,Negative1,Negative2 = [0,1,2,3,5]

## Import module
```python
from X import *
from X import a,b,c

import X

X = __import__('X')
```

### Import module HOWTO
__When Python imports a module, it first checks the module registry (sys.modules) to see if the module is already imported__. If that’s the case, Python uses the existing module object as is.

Otherwise, Python does something like this:

- Create a new, empty module object (this is essentially a dictionary)
- Insert that module object in the sys.modules dictionary
- Load the module code object (if necessary, compile the module first)
- Execute the module code object in the new module’s namespace. All variables assigned by the code will be available via the module object.

This means that it’s fairly cheap to import an already imported module; Python just has to look the module name up in a dictionary.

### Circular dependency
__Case 1__

If you run a module as a script (i.e. give its name to the interpreter, rather than importing it), it’s loaded under the module name __main__.
If you then import the same module from your program, it’s reloaded and reexecuted under its real name. If you’re not careful, you may end up doing things twice.

__Case 2__

In Python, things like def, class, and import are statements too.
Modules are executed during import, and new functions and classes won’t appear in the module’s namespace until the def (or class) statement has been executed.
Ex:
```python
# module X

import Y

def spam():
    print "function in module x"


# module Y
from X import spam # doesn't work: spam isn't defined yet!
```
We run module X first. WHen Python reaches the import Y, it loads the code for Y, and starts executing it instead.
At this time, X, Y are already installed in sys.modules, but X is not executed complete yet, so the statement Y import spam get error.
To fix that, let import Y move to the end of module X.

```python
# module X

def spam():
    print "function in module x"

import Y
```


