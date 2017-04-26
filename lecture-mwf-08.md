# Running Time descriptions for actual problems. List ADTs

* Constant Running Time
* Linear Running Time

Okay, we've spent some time talking building an evaluation model.

Now, let's try it out! Specifically, we want to look at some
concrete programs, and try to figure out what the running times
will look like. Hopefully we'll be able to draw some nice graphs,
as well.

Okay, first things first. How about a nice simple equality method?

Ha! Ha! Ha!

"Nice simple equality."

Here's the equality method for our Pair class:

def __eq__(self, other):
    return (type(other) == Pair
            and self.first == other.first
            and self.rest == other.rest
            )

Okay, let's try it out.

Here's a program:

```python
class Pair:
    # etc. etc.

Pair(3, "mt") == "mt"
```

```python
class Pair:
    # etc. etc.

p_1 = Pair(3, "mt")
p_1 == "mt"
```

(note that since strings are immutable, we don't need to give them new
top-level bindings)

```python
class Pair:
    # etc. etc.

p_1 = Pair(3, "mt")
self_1 = p_1
other_1 = "mt"
{return (type(other_1) == Pair
         and self_1.first == other_1.first
         and self_1.rest == other_1.rest
         )}
```

```python
class Pair:
    # etc. etc.

p_1 = Pair(3, "mt")
self_1 = p_1
other_1 = "mt"
{return ("mt" == Pair
         and self_1.first == other_1.first
         and self_1.rest == other_1.rest
         )}
```

```python
class Pair:
    # etc. etc.

p_1 = Pair(3, "mt")
self_1 = p_1
other_1 = "mt"
{return (False
         and self_1.first == other_1.first
         and self_1.rest == other_1.rest
         )}
```

```python
class Pair:
    # etc. etc.

p_1 = Pair(3, "mt")
self_1 = p_1
other_1 = "mt"
{return False}
```

```python
class Pair:
    # etc. etc.

p_1 = Pair(3, "mt")
self_1 = p_1
other_1 = "mt"
False
```

So, that took 6 steps. How long is a step? Well,
strangely:

1) steps can take different times, but
2) it doesn't matter.

Why? Because each of our different kinds of steps
takes a constant amount of time. The sum of six
constants is another constant. So no matter what
kind of steps they are, the result is a constant.

So: what if we tried lists of different size?
Would the result be different?

Surprisingly, the answer is "yes". But... for a
funny reason. Specifically, the evaluation of
the value that we're testing for equality takes
one step for each Pair. It's not fair to
count that, though... that's not the equality
test, that's prior to the equality test.

The equality test itself takes a constant number
of steps, whenever we're comparing to "mt".

Let's graph that. How does the time change
as a function of the length of the list? (flat line)

What's the height of the line? It's hard to be
precise, and frankly, we probably don't care.

Okay, let's try again; this time, let's compare
a Pair to another Pair.

Can we characterize the running time of this
operation?

Well, it depends on what's *in* the list. If the
two pairs differ in their first elements, then
it's going to run in four or five steps.

If they're the same, though, what happens?

Let's take a look at one example.

```python
class Pair:
    # etc. etc.

Pair(3, "mt") == Pair(3, Pair(4, "mt"))
```

* discussion of worst case (the same all the way down) vs average case.

in general: equality is linear in the size of the lists

--

Copyright (C) 2017, John Clements (clements@racket-lang.org)
