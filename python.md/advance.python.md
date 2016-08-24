# Advance python

> ```python
> def learnedOneThing(you):
>     you.success += 1
> 
> while(you.alive):
>   learnedOneThing(you)
> ```

## Function

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
