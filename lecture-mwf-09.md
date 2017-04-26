# Arrays


All right, let's talk about arrays.

You will no doubt be shocked--*shocked*--to learn that linked lists
are not the only kind of lists that Python has.

In addition to Linked Lists, Python also has a notion of "arrays".
An array also represents a sequence of values, but it has a number
of seriously different characteristics from the lists that we've
discussed thus far.

The most obvious difference between the two is in the set of operations
that Python provides to work on the two kinds of lists. What operations
do we have on linked lists? (Q)

Well, actually, you're right. There really aren't any operations that
are specific to linked lists; we're just using classes, so we have
the operations that are related to objects: creation, and field
reference (first and rest). Really, that's about it. Each of
these operations is essentially a constant-time operation.

Python provides a different set of constant-time operations for
arrays. Well, I'm going to use the term "array"; in Python, they're
often just called "Lists." So you'll just have to translate :).

Arrays provide a number of primitive constant-time operations:

* get length
* fetch element (indexing)
* mutate element

Arrays also provide two not-linear-time operations:

* creation
* comparison

Now, you may notice that I explicitly mention mutation here. In
principle, you can mutate fields of Pairs, so lists are mutable
too, but you don't really need to do that, much, and as you'll
see, it doesn't affect asymptotic running time, much. For arrays,
it's deeply un-idiomatic to operate on arrays without mutation.
Arrays algorithms almost always use mutation.

How do we use these operations? I think you've all seen these,
so this should just be review. Suppose we have an array named arr
 
* get length : len(arr)
* fetch element : arr[idx]
* mutate element : arr[idx] = 1234
* creation: [3, 4, "pony", True]
* equality [3, 14, "abc"] == [3, 14, "abc"]
 

Okay, let's extend our model to include arrays. It's not going
to be much of a stretch; it's going to look a lot like object creation.

So, for instance, we saw pair creation before:

```python
class Pair:
    def __init__(self, first, rest):
    #...

do_something(Pair(3,"mt"))
```

goes to

```python
class Pair:
    def __init__(self, first, rest):
    #...

MV_1 = Pair(3,"mt")
do_something(MV_1)
```

Here's the same thing, with an array. Actually, let's throw a few
array operations in there, too. Oh, let's put a Dog in there, too,
to show how nested expressions get evaluated

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

my_array = [13,4,Dog("Pomeranian",23),6]
increment_second_element(my_array)
```

goes to

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
my_array = [13,4,MV_1,6]
increment_second_element(my_array)
```
and then through

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
MV_2 = [13,4,MV_1,6]
my_array = MV_2
increment_second_element(my_array)
```

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
MV_2 = [13,4,MV_1,6]
my_array = MV_2
increment_second_element(MV_2)
```

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
MV_2 = [13,4,MV_1,6]
my_array = MV_2
arr_1 = MV_2
{arr_1[1] = arr_1[1] + 1}
```

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
MV_2 = [13,4,MV_1,6]
my_array = MV_2
arr_1 = MV_2
{MV_2[1] = arr_1[1] + 1}
```

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
MV_2 = [13,4,MV_1,6]
my_array = MV_2
arr_1 = MV_2
{MV_2[1] = MV_2[1] + 1}
```

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
MV_2 = [13,4,MV_1,6]
my_array = MV_2
arr_1 = MV_2
{MV_2[1] = 4 + 1}
```

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
MV_2 = [13,4,MV_1,6]
my_array = MV_2
arr_1 = MV_2
{MV_2[1] = 5}
```

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
MV_2 = [13,5,MV_1,6]
my_array = MV_2
arr_1 = MV_2
{}
```

```python
# a Dog is Dog(str, float), representing a dog's breed and its weight in kg
class Dog:
    def __init__(self, breed, weight):
    # ...
    
def increment_second_element(arr):
    arr[1] = arr[1] + 1

MV_1 = Dog("Pomeranian",23)
MV_2 = [13,5,MV_1,6]
my_array = MV_2
arr_1 = MV_2
None
```

As on Monday (erm... except that I just went back and modified Monday's lecture,
sorry) we need to distinguish between mutable values `MV_1`, `MV_2`, etc., and
ordinary bindings like my_array and arr_1. They're different because bindings
get reduced, and mutable values don't. If I try to pass my_array to a function,
it gets reduced to MV_2. (But not any further.)

# consider equality for arrays: again linear

# list-ref : linear for linked lists, constant for arrays

NB: this problem appears on the lab, might not want to write the code for
them in class.

# writing functions on arrays

Okay, it's time to write functions on arrays.

Let's take a simple example that we've seen before: multiply the elements
of a list.

Let's follow the design recipe, and ask for help when we get stuck.

on linked lists (let's put the data def'n. in there just in case we forgot it).

Also, what the heck, let's change "mt" to None. It's the idiomatic choice, and
we've made the point, I think.

```python
# a FloatList is one of
# - None
# - Pair(float, FloatList)

# FloatList -> float
# multiply the elements of a list
def list_product(l):
  if (l == None):
    return 1
  else:
    return l.first * list_product(l.rest)

self.assertEqual(list_product(None), 1)
self.assertEqual(list_product(Pair(3, Pair(9, None))), 27)
```

Okay, let's do this for arrays. We can write the data definition
like this:

```python
# A FloatArray is [float, ...]
```

Steps two and three are pretty straightforward:

```python
# FloatArray -> float
# multiply the elements of an array
def array_product(a):
  pass

self.assertEqual(array_product([]))
```

etc. etc.

--

Copyright (C) 2017, John Clements (clements@racket-lang.org)
