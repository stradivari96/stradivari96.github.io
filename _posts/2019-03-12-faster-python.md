---
title: Making Python a bit faster
excerpt: "Some basic tips to bump your program's performance"
read_time: true
share: true
toc: true
related: true
comments: true
tags: [Python, Programming]
---
<figure>
	<a href="https://6lli539m39y3hpkelqsm3c2fg-wpengine.netdna-ssl.com/wp-content/uploads/2017/07/AI-HPC-Converge.jpg"><img src="https://6lli539m39y3hpkelqsm3c2fg-wpengine.netdna-ssl.com/wp-content/uploads/2017/07/AI-HPC-Converge.jpg"></a>
</figure>

Let's face it, Python is **slow** if you compare it with compiled languages like C or Fortran. But they are some tricks you can use to shorten the gap, let's see some of them:

**Note**: Many examples come from [Python High Performance by Packt](https://www.packtpub.com/application-development/python-high-performance-second-edition)

## Cache
> In computing, a cache is a hardware or software component that stores data so that future requests for that data can be served faster.

### lru_cache
Lets try to implement the Fibonacci sequence:
```python
def fibonacci(n):
    if n == 0: return 0
    if n == 1: return 1
    return fibonacci(n-1) + fibonacci(n-2)

fibonacci(35) # Took 5 seconds on my high-end CPU
```
We have too many recursive calls.

But what if we make the function remember the most recent results?
```python
from functools import lru_cache

@lru_cache(maxsize=3) # number of elements to maintain
def fibonacci(n):
    ...

fibonacci(100) # Took 0.0009 seconds
```

### On disk caching
You can also store the results on-disk using the `joblib` library, the cached results will also persist over **subsequent runs**.

Install the package using `pip install joblib` and follow this example:
```python
from joblib import Memory
memory = Memory(cachedir='/tmp/python_cache') # directory to store the results

@memory.cache
def fibonacci(n):
    ...
```

## Comprehension and generators
Comprehensions are slightly optimized in Python and with generators you will also save memory as they are **lazy**.
> Lazy evaluation, or call-by-need is an evaluation strategy which delays the evaluation of an expression until its value is needed.

```python
import timeit

loop = """
res = []
for i in range(100000):
    res.append(i * i)
sum(res)
"""

comprehension = "sum([i * i for i in range(100000)])"
generator = "sum(i * i for i in range(100000))"

print('loop', timeit.timeit(loop, number=1000))
print('comprehension', timeit.timeit(comprehension, number=1000))
print('generator', timeit.timeit(generator, number=1000))

# loop 11.002702700000555
# comprehension 8.966031799999655
# generator 5.443436499999734
```

## Algorithms and Data Structures
Last but not least, do not forget about using the best data structure for each problem.

* ***[lists](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)***
    * **Fast:** Accessing an element by index, appending or removing the last element, iterating over its elements.
    * **Slow:** Inserting or deleting the first element.
* ***[collections.deque](https://docs.python.org/3/library/collections.html#collections.deque)***
    * **Fast:** Inserting or deleting the first or the last element.
    * **Slow:** Accessing an element in the middle of the deque.
* ***[sets](https://docs.python.org/3/tutorial/datastructures.html#sets)***
    * **Fast:** Determining if an object is present in the set, adding and removing elements.
    * **Slow:** Iterating over all its elements.

* ***[dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries)***
    * **Fast:** Inserting, deleting and accessing elements.
    * **Slow:** Searching for a value.

**Extra:** searching on **sorted lists** using [bisect](https://docs.python.org/3.5/library/bisect.html)
```python
from bisect import bisect_left

# you can use _ to make big numbers more readable
mylist = list(range(30_000_000))

for i in range(10):
    mylist.index(29_999_999)
# took 2.42 seconds

for i in range(10):
    bisect_left(mylist, 29_999_999)
# took 0.0006 seconds
```
