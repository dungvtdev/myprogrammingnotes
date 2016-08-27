# Core python

> ```python
> def learnedOneThing(you):
>     you.success += 1
> ```

## Simplest

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
- (?:abc) non-capturing group
- (?=abc) positive lookahead
- (?!abc) negative lookahead

Quantifiers & Alternation
- a* a+ a?    0 or more, 1 or more, 0 or 1
- a{5} a{2,}  exactly five, two or more
- a{1,3}  between one & three
- a+? a{2,}?  match as few as possible
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