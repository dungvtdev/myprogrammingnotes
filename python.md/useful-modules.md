# Useful modules python

> ```python
> def learnedOneThing(you):
>     you.success += 1
> 
> while(you.alive):
>   learnedOneThing(you)
> ```

__copy__
- copy(o), deepcopy(o)

__pyperclip__
- copy(str), paste()

__sys__
- exit(): exit current program.
- sys.argv: command-line argument values
    + sys.argv[0]: program name. ex 'pw.py'

__os__

__time__
- time(), sleep()

__cProfile__

__subprocess__
- create subprocess, poll() or wait().

__datetime__
- object:
    + datetime
    + timedelta
- datetime.datetime.now()
- datetime.datetime(y,m,d,h,m,s,ms,us)
- datetime.datetime.fromtimestamp(s)
- datetimes are comparable.
- str(datetime): toString.
- datatime.timedelta(days=,hours=,minutes=,seconds=)
- datetimeobject.strftime(): format time to string
- datetime.datetime.strptime(), string -> datetime.

__threading__
```python
threadObj = threading.Thread(target=print, args=['Cats', 'Dogs', 'Frogs'],kwargs={'sep': ' & '})
threadObj.start()

# threadObj.join()
```

__requests__
- requests.get(url) return response object
    + response: .text, .status_code, .raise_for_status(), .iter_content(len)

__beautifulsoup__
- select html tag by css selector.
- link https://automatetheboringstuff.com/chapter11/

__type__
- IntType, StringType
    + ex type(id) is IntType

__logging__ 
- levels: tuong ung .debug(),.info()...
    + DEBUG
    + INFO
    + WARNING
    + ERROR
    + CRITICAL
- level=L, toan bo level > L se duoc log
    + ex: level= INFO, toan bo tru DEBUG deu duoc log ra
- logging.disable(L): disable toan bo level <= L
