---
title: Rejected Python Enhancement Proposals (PEPs)
excerpt: "Taking a look of some rejected PEPs and the reason behind each one"
read_time: true
share: true
toc: true
related: true
comments: true
tags: [Python, Programming]
---
<figure>
	<a href="https://files.realpython.com/media/PEP-8-Tutorial-Python-Code-Formatting-Guide_Watermarked.9103cf7be328.jpg"><img src="https://files.realpython.com/media/PEP-8-Tutorial-Python-Code-Formatting-Guide_Watermarked.9103cf7be328.jpg"></a>
</figure>

Ever wandered why there isn't a `switch` statement on Python? Let's take a look of some interesting PEPs that were rejected.

**Note**: See the full list [here](https://www.python.org/dev/peps/#abandoned-withdrawn-and-rejected-peps).

## Switch statement
> A switch statement is a type of selection control mechanism used to allow the value of a variable or expression to change the control flow of program execution via search and map.

The Benevolent dictator for life (BDFL), Guido van Rossum, wrote [PEP 3103](https://www.python.org/dev/peps/pep-3103/) with some variations of the syntax proposed in [PEP 275](https://www.python.org/dev/peps/pep-0275/):
```python
def whatis(x):
    switch x:
        case 'one':
            print('1')  # break is not needed, no fall-through like in other langs
        case 'two':
            print('2')
        else:
            print("D'oh!")
```

The main reasons against just using `if-else` were:
* Avoid the repetition the variable and test operator (usually '==' or 'in).
* Making the search more efficient using a dictionary O(1) instead of testing every case O(n).

It was rejected after a poll.

> "A quick poll during my keynote presentation at PyCon 2007 shows this proposal has no popular support. I therefore reject it." -- Guido van Rossum

## Overloadable Boolean Operators
[PEP 335](https://www.python.org/dev/peps/pep-0335/) tried to introduce custom `and`, `not` and `or` operators.

```python
from numpy import array, ndarray

class BArray(ndarray):

    def __and2__(self, other):
        return (self & other)
    def __or2__(self, other):
        return (self & other)
    def __not__(self):
        return (self == 0)

def barray(*args, **kwds):
    return array(*args, **kwds).view(type = BArray)

a0 = barray([0, 1, 2, 4])
a1 = barray([1, 2, 3, 4])
a2 = barray([5, 6, 3, 4])
a3 = barray([5, 1, 2, 4])

not a0  # self == 0
# barray([True False False False])

a0 == a1 and a2 == a3  # self & other
# barray([False False False True])

a0 == a1 or a2 == a3  # self & other
# barray([False False False True])
```
It was rejected because it added bytecode that wasn't going to be used in
most cases.

## Exception-catching expressions
In [PEP 463](https://www.python.org/dev/peps/pep-0463/), Chris Angelico proposed
a way to assign default values using exception-based conditions.
```python
# As an expression:
process(dic[key] except KeyError: None)
```
The ways of doing this at this time are:
```python
# Custom ad-hoc method
dic.get(key, None)

# LBYL (look before you leap):
if key in dic:
    process(dic[key])
else:
    process(None)
# As an expression:
process(dic[key] if key in dic else None)

# EAFP (Easier to ask for forgiveness than permission):
try:
    process(dic[key])
except KeyError:
    process(None)
```

It looks quite clean and readable, though as GvR said:
> [...] the thing I can't get behind are the motivation and rationale. I don't think that e.g. dict.get() would be unnecessary once we have except expressions, and I disagree with the position that EAFP is better than LBYL [...]
