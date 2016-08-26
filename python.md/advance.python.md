# Advance python

> ```python
> def learnedOneThing(you):
>     you.success += 1
> 
> while(you.alive):
>   learnedOneThing(you)
> ```

## Function
```python
def fct(x,y,z,*args,a=3,b=5,**kwargs):
    pass
    # *args variable positional arguments (-> tuple), default values,
    # **kwargs variable named arguments (->dict)
    
# ex  fct(1,2,3,4,5,6,7,x=30,y=70)
    x=1, y=2, z=3
    args = tuple (4,5,6,7)
    a=3,b=5
    kwargs=dict (x=30, y=70).
```
All parameters (arguments) in the Python language are passed by reference.


## Inheritance

```python
class Base(object):
    pass

class Child(Base):
    pass
```

__super__
- python3: super()
- python2: super(type,instance)

```python
class Base(object):    # neu khong inherit tu object, dung super(Child,self) se bao loi TypeError: super() argument 1 must be type, not classobj
    def __init__(self):
        pass

class Child(Base):     # neu Base khong inherit tu object, thay the bang Chile(bBase,object) se khong bao loi
    def __init__(self):
        super(Child,self).__init__()
```
- class inherit tu object la new-style class, la 1 type
- class khong inherit tu object, goi la classic class, la 1 classobj

Xem http://stackoverflow.com/questions/1713038/super-fails-with-error-typeerror-argument-1-must-be-type-not-classobj

## Class style
- python2: 
    + old-stype, not inherit from object
    + new-stype, inherit from object
- python3: all is new-style

## \_\_new\_\_ and \_\_init\_\_

http://spyhce.com/blog/understanding-new-and-init

- \_\_new\_\_:
    + not executed in old-style class
    + dung de tao object, tham sao la class type, return 1 object/value (ex int,string...)
- \_\_init\_\_:
    + dung de init 1 object sau khi da tao, truyen vao object da tao trong \_\_new\_\_
    + neu \_\_new\_\_ khong return 1 object/value, \_\_init\_\_ se khong duoc goi, constructor A() return None.
```python
class A(object):  
    def __new__(cls):   # truyen vao cls la class type (ex class '__main__'.A, type='type')
        print "A.__new__ called"
        return super(A, cls).__new__(cls) # build-in function tao instace tu class-type

    def __init__(self):
        print "A.__init__ called"

A()
# A.__new__ called
# A.__init__ called

class Sample(object):  
    def __str__(self):
        return "SAMPLE"

class A(object):
    def __new__(cls):
        return Sample()

class A(object):
    def __new__(cls):
        return super(A, cls).__new__(Sample)

print(A())

# log SAMPLE
```

## From java to python OOP

Nếu biến trong class có prefix "__" (2 undercore) thì khi interpreter dịch code, biến nó thành tên khác. Ví dụ
XClass.\_\_name -> XClass.\_XClass\_\_name

Có tác dụng tránh conflict tên biến khi đa kế thừa.


```python
class A(object):
    name='A'                # static property, class-level
    __private_val=10        # pseudo private class attribute,
                            # Names inside a class statement that start with two underscores __
                            #(and don't end with two underscores) are automatically expanded to
                            # include the name of the enclosing class  
    
    @classmethod
    def method(cls):        # class method
        print('method ' + cls.name)

    @staticmethod
    def smethod():          # static method
        print('smethod')

    def __init__(self): 
        self.age=10         # object property, instance-level
        pass

    def create(self):
        self.age=10         # object property, instance-level

a=A()
a.home='hp'                 # assign new object property, instance-level
print(a.home)            # log: hp
# print(a.__private_val)   # crash
print(a._A__private_val) # this is __private_val, log: 10

a.__hehe=20              # not hide because this is not class-level
print(a.__hehe)          # log 20

print(A.name)            # log A
print(A().name)          # log A
A.name='AA'             
A.method()               # log method AA - name is change to AA
A.smethod()              # log smethod
print(A().name)          # log AA - because name is class-level property
print(a.name)            # log AA 

a.create()
print(a.age)             # log 10

```

## Method decorators

```python
@decorator        # <=> func=decorator(func)
def func():
    pass

# Ex
def tags(tag_name):
    def tags_decorator(func):
        def func_wrapper(name):
            return "<{0}>{1}</{0}>".format(tag_name, func(name))
        return func_wrapper
    return tags_decorator
@tags("p")
def get_text(name):
    return "Hello "+name
print get_text("John")
# Outputs <p>Hello John</p>


#  Problem
print get_text.__name__
#  Outputs func_wrapper

# Fix, use functools module
from functools import wraps
def tags(tag_name):
    def tags_decorator(func):
        @wraps(func)
        def func_wrapper(name):
            return "<{0}>{1}</{0}>".format(tag_name, func(name))
        return func_wrapper
    return tags_decorator
@tags("p")
def get_text(name):
    """returns some text"""
    return "Hello "+name

print get_text.__name__ 
# get_text
print get_text.__doc__ 
# returns some text
print get_text.__module__ 
# __main__

```