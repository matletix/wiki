+++
title = "Iterators, Generators and Coroutines"
date = "2022-06-19T14:52:15+01:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["python", "iterators", "generators", "coroutines"]
keywords = ["", ""]
description = "Quick overview of these concepts in Python"
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
toc = true
+++


## Iterable
An *iterable* is an object that can produce an *iterator* by calling the `iter()` function on it. 
Eg: `list`, `tuple`, `dict`, `set`, `string`, etc.
It must define the `__iter__()` method:

> `__iter__()`: method called on the initialization of an Iterator. (eg. by `iter()`)
{.is-info}

```python
abcd = ["a", "b", "c", "d"]
type(abcd)  # <class 'list'>
it = iter(abcd)
type(it)  # <class 'list_iterator'>
```

## Iterator
An iterator is a value producer that *yields* successive values from its associated iterable object. It retains its state internally, so when you call `next()`,  it knows which values have been obtained already and what value to return next.
It must define the `__next__()` method:

> `__next__()` : method called to produce the next element of an Iterator. (eg. by `next()`)
{.is-info}

The `for ... in ...` loop makes use of iterators. The two codes below are equivalent :
```python
abcd = ["a", "b", "c", "d"]
for letter in abcd:
  print(letter)
# a, b, c, d
```
```python
abcd = ["a", "b", "c", "d"]
it = iter(abcd)  # Create the Iterator
while True:
  try:
    letter = next(it)  # Get the next element
  except StopIteration:
    break  # Stop looping at the end of the iterator
  print(letter)
  # a, b, c, d
```

## Generator
A generator can refers to several concepts: the *generator functions*, the *generator expressions* and the *generator iterators*.

- **generator iterator** : it’s an object built on top of Iterators. It can be created using a generator function or a generator expression, that greatly reduce the boilerplate necessary to create a pure iterator (by defining a class with `__iter__()` and `__next__()`).

- **generator function** : a function that uses the yield keyword. When called, it returns a new generator iterator. Example:
```python
def generator_function(max=10):
  """Return a generator that iterates from 1 to max included."""
  n = 1
  while n <= max:
    yield n
    n += 1
    
g = generator_function()
type(g)  # <class 'generator'>
list(g)  # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

- **generator expression** : it’s the same as a list-comprehension except that the brackets are replaced by parentheses and the returned object is a generator iterator instead of a list. Example:
```python
max = 10
g = (i for i in range(1, max+1))
type(g)  # <class 'generator'>
list(g)  # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

### Special methods
Generators come with their own methods that extend the way they can be interacted with: `send()`, `throw()` and `close()`.


- `send()` : the *send* method allows you to inject a value back into the generator, effectively making it a *coroutine*. The injected value is returned by the yield statement that stopped the generator when it restarts its execution. It can be retrieved by assigning a value to the yield statement:

```python
value = yield
```

Here is an example of a coroutine jumping from 0 up to *max* by a dynamically injected step:

```python
def jump_to(max):
  """
  Yield a value from 0 to max, incrementing by <jump> everytime.
  
  If a value is sent back to the generator, then set <jump> to this value.
  Otherwise set <jump> to 1.
  """
  i = 0
  while i <= max:
    jump = yield i
    i += 1 if jump is None else jump

g = generator_function(10)
next(g)    # 0
next(g)    # 1
g.send(5)  # 6
g.send(4)  # 10
next(g)    # StopIteration
```

- `throw()` : the *throw* method allows you to throw errors within the generator. The following example creates a generator from a list of numbers and raises an error if the generator yields a non-integer value:

```python
numbers = (i for i in [1, 5, 2, 0.33, 8, 3])  # create generator
for n in numbers:
  if n % 1:
    numbers.throw(ValueError)  # throw an error if n is not an interger
  print(n)
```

- `close()` : the *close* method allows you to stop an iterator. It makes it throw the StopIteration exception. It’s particularly useful with inifinite generators. Let’s look at an example : the code below creates an infinite generator counting from 0. It then uses a function that defines a max number to count to, and when reaching this max number, closes the infinite generator.

```python
def counting():
  n = 0
  while True:
    yield n
    n += 1

def count_to(max):
  g = counting()
  for i in g:
    print(i)
    if i >= max:
      g.close()

count_to(10)  # 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
```

### Subgenerators
Let consider this example:

```python
g1 = (i for i in range(1, 6))   # 1, 2, 3, 4, 5
g2 = (i for i in range(6, 11))  # 6, 7, 8, 9, 10

def main_generator():
  for i in g1:
    yield i
  for i in g2:
    yield i

for i in main_generator():
  print(i)  # 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
```

It uses two generators (*g1* and *g2*) inside a generator function (*main_generator*) to create a third generator combining the values yielded by g1 and g2. These two generators are subgenerators of the delegating generator (main_generator). Python 3.3 introduces a new syntax for simplifying the use of subgenerators : 

```python
yield from EXPRESSION
```

where `EXPRESSION` is an expression evaluating to an iterable. It can replace:

```python
for i in EXPRESSION:
  yield i
```

This syntax allows to factor out a portion of a generator inside another generator. It has also the benefit of handling the corner cases and calls to `.send()`, `.throw()` and `.close()`: if these methods are called on the delegating generator, they are propagated to the subgenerators. More details about the `yield from syntax` in the [PEP 380](https://www.python.org/dev/peps/pep-0380/).
The previous code can be rewritten as : 

```python
g1 = range(1, 6)   # 1, 2, 3, 4, 5
g2 = range(6, 11)  # 6, 7, 8, 9, 10

def main_generator():
  yield from g1
  yield from g2

for i in main_generator():
  print(i)  # 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
```


---
*Sources*:
- https://www.python.org/dev/peps/pep-0380/
- https://dbader.org/blog/python-iterators
- https://www.integralist.co.uk/posts/python-generators/
- https://realpython.com/python-for-loop/
