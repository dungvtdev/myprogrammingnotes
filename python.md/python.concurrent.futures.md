# Abu Ashraf Masnun
Tales of a Software Craftsman

__PYTHON: A QUICK INTRODUCTION TO THE CONCURRENT.FUTURES MODULE__

The concurrent.futures module is part of the standard library which provides a high level API for launching async tasks. We will discuss and go through code samples for the common usages of this module.

## Executors

This module features the Executor class which is an abstract class and it can not be used directly. However it has two very useful concrete subclasses – ThreadPoolExecutor and ProcessPoolExecutor. As their names suggest, one uses multi threading and the other one uses multi-processing. In both case, we get a pool of threads or processes and we can submit tasks to this pool. The pool would assign tasks to the available resources (threads or pools) and schedule them to run.

__ThreadPoolExecutor__

Let’s first see some codes:

```python
from concurrent.futures import ThreadPoolExecutor
from time import sleep

def return_after_5_secs(message):
    sleep(5)
    return message

pool = ThreadPoolExecutor(3)

future = pool.submit(return_after_5_secs, ("hello"))
print(future.done())
sleep(5)
print(future.done())
print(future.result())
```

I hope the code is pretty self explanatory. We first construct a ThreadPoolExecutor with the number of threads we want in the pool. By default the number is 5 but we chose to use 3 just because we can ;-). Then we submitted a task to the thread pool executor which waits 5 seconds before returning the message it gets as it’s first argument. When we submit() a task, we get back a Future. As we can see in the docs, the Future object has a method – done() which tells us if the future has resolved, that is a value has been set for that particular future object. When a task finishes (returns a value or is interrupted by an exception), the thread pool executor sets the value to the future object.

In our example, the task doesn’t complete until 5 seconds, so the first call to done() will return False. We take a really short nap for 5 secs and then it’s done. We can get the result of the future by calling the result() method on it.

A good understanding of the Future object and knowing it’s methods would be really beneficial for understanding and doing async programming in Python. So I highly recommend taking the time to read through the docs.

__ProcessPoolExecutor__

The process pool executor has a very similar API. So let’s modify our previous example and use ProcessPool instead:

```python
from concurrent.futures import ProcessPoolExecutor
from time import sleep

def return_after_5_secs(message):
    sleep(5)
    return message

pool = ProcessPoolExecutor(3)

future = pool.submit(return_after_5_secs, ("hello"))
print(future.done())
sleep(5)
print(future.done())
print("Result: " + future.result())
```

It works perfectly! But of course, we would want to use the ProcessPoolExecutor for CPU intensive tasks. The ThreadPoolExecutor is better suited for network operations or I/O.

While the API is similar, we must remember that the ProcessPoolExecutor uses the multiprocessing module and is not affected by the Global Interpreter Lock. However, we can not use any objects that is not picklable. So we need to carefully choose what we use/return inside the callable passed to process pool executor.

__Executor.map()__

Both executors have a common method – map(). Like the built in function, the map method allows multiple calls to a provided function, passing each of the items in an iterable to that function. Except, in this case, the functions are called concurrently. For multiprocessing, this iterable is broken into chunks and each of these chunks is passed to the function in separate processes. We can control the chunk size by passing a third parameter, chunk_size. By default the chunk size is 1.

Here’s the ThreadPoolExample from the official docs:

```python
import concurrent.futures
import urllib.request

URLS = ['http://www.foxnews.com/',
        'http://www.cnn.com/',
        'http://europe.wsj.com/',
        'http://www.bbc.co.uk/',
        'http://some-made-up-domain.com/']

# Retrieve a single page and report the url and contents
def load_url(url, timeout):
    with urllib.request.urlopen(url, timeout=timeout) as conn:
        return conn.read()

# We can use a with statement to ensure threads are cleaned up promptly
with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
    # Start the load operations and mark each future with its URL
    future_to_url = {executor.submit(load_url, url, 60): url for url in URLS}
    for future in concurrent.futures.as_completed(future_to_url):
        url = future_to_url[future]
        try:
            data = future.result()
        except Exception as exc:
            print('%r generated an exception: %s' % (url, exc))
        else:
            print('%r page is %d bytes' % (url, len(data)))
```

And the ProcessPoolExecutor example:

```python
import concurrent.futures
import math

PRIMES = [
    112272535095293,
    112582705942171,
    112272535095293,
    115280095190773,
    115797848077099,
    1099726899285419]

def is_prime(n):
    if n % 2 == 0:
        return False

    sqrt_n = int(math.floor(math.sqrt(n)))
    for i in range(3, sqrt_n + 1, 2):
        if n % i == 0:
            return False
    return True

def main():
    with concurrent.futures.ProcessPoolExecutor() as executor:
        for number, prime in zip(PRIMES, executor.map(is_prime, PRIMES)):
            print('%d is prime: %s' % (number, prime))

if __name__ == '__main__':
    main()
```

__as_completed() & wait()__

The concurrent.futures module has two functions for dealing with the futures returned by the executors. One is as_completed() and the other one is wait().

The as_completed() function takes an iterable of Future objects and starts yielding values as soon as the futures start resolving. The main difference between the aforementioned map method with as_completed is that map returns the results in the order in which we pass the iterables. That is the first result from the map method is the result for the first item. On the other hand, the first result from the as_completed function is from whichever future completed first.

Let’s see an example:

```python
from concurrent.futures import ThreadPoolExecutor, wait, as_completed
from time import sleep
from random import randint

def return_after_5_secs(num):
    sleep(randint(1, 5))
    return "Return of {}".format(num)

pool = ThreadPoolExecutor(5)
futures = []
for x in range(5):
    futures.append(pool.submit(return_after_5_secs, x))

for x in as_completed(futures):
    print(x.result())
```

The wait() function would return a named tuple which contains two set – one set contains the futures which completed (either got result or exception) and the other set containing the ones which didn’t complete.

We can see an example here:

```python
from concurrent.futures import ThreadPoolExecutor, wait, as_completed
from time import sleep
from random import randint

def return_after_5_secs(num):
    sleep(randint(1, 5))
    return "Return of {}".format(num)

pool = ThreadPoolExecutor(5)
futures = []
for x in range(5):
    futures.append(pool.submit(return_after_5_secs, x))

print(wait(futures))
```

We can control the behavior of the wait function by defining when it should return. We can pass one of these values to the return_when param of the function: FIRST_COMPLETED, FIRST_EXCEPTION and ALL_COMPLETED. By default, it’s set to ALL_COMPLETED, so the wait function returns only when all futures complete. But using that parameter, we can choose to return when the first future completes or first exception encounters.