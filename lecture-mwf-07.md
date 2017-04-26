# Organizing Data, Models of running time

## Lecture topics:
* running time,
* comparing graphs,
* a model of running time for Python

The very first thing that programmers need to do is to figure out how
to represent data, and that's what we've been talking about for the first
two weeks. Soon after this, though, we start worrying about representing
data efficiently, and, usually more difficult, processing it efficiently.

So: what does "efficient" mean to a programmer?

# Efficiency

Running programs requires resources. We could just say "money" and
leave it at that, but when we look a little closer, it makes sense
to talk about both "space" and "time", though both of these mean
something somewhat different in the context of programming.

First, let's talk about time.

## Measuring Time is Impossible

Let's take a simple example. I want to figure out whether two people
in this room have the same birthday. Or, more generally, *which* pairs
of people share a birthday.

Me and my pal Barbara both write a program to figure out the result.
Mirabile dictu, they're both correct.

Could one be better than the other? (Q) Yes, one could use less
time than the other.

Okay, let's try to figure out which one uses less time.  I run my program
in my town, and Barbara runs it in her town, and it turns out that
my program runs in 3,400 seconds, and hers runs in 1,200. Does that mean that
her program is better than mine? (Q)

No, here are some reasons that might account for the difference:

- hardware speed
- measuring error
- problem size!

Oh, this problem size thing is a big issue. What if there are 58,000
people in my town, and only 2,000 in hers?

If we're going to try to compare our programs, then, we should probably
try to do an "apples to apples" comparison. Run them on the same problem
size and same machine.

Okay, so we run on my laptop, with a sample of 200 people. My program
takes 28 seconds, and hers takes 30.

Excellent! So my program is faster! (Q) No? Not faster? Why not?

Because we're just trying one particular setup. What variables are
"interesting"?

Well, computer speed is usually pretty linear: it's somewhat likely that
if a computer runs my program twice as fast as another, it will probably
run your program twice as fast. The obvious and major hole in that
comes with multi-threaded or parallel programs, and frankly, we're just
not really ready to talk about that right now.

However, the "problem size" is a variable that is *definitely*
"interesting," in the sense that (unlike computer speed) it's
by no means the case that running the program on a problem
of twice the size will take twice the time.

In this case (the birthday problem), our "size" can easily be
described by a single variable. Can you think of problems that
can't be described by just a single variable? (Q)

Actually, you can go in a lot of directions here. A variant
of the birthday problem where we have groups A and B and try to
find A/B pairs that share a birthday is an obvious one. To jump
straight into the deep end, you might also want to talk about
the time it takes to run a program written in Java. The
program length is one parameter, but by *NO MEANS* the only
parameter. In fact, you'd have a hard time parameterizing this
in any meaningful way at all.

Anyhow, we're going to start out looking at problems where the
"size" can be described by a single number, like our birthday
problem.

What would be a good way to see the time taken, and how it depends
on the problem size, 'n'? (Q) Yes! A graph!

Here's a graph (wiggly line, up and down). Is this a plausible
graph? Probably not. We expect our graphs to be monotonically
increasing.

Okay, how about this one? (straight line that crosses the X axis
to the right of the origin)

No, that doesn't work either. The time taken can't be less than zero.

Also, the problem size can't be less than zero, for most of the
problems we'll be looking at.

Okay, so here's a plausible graph. (Straight line.) Here's another
one (crossing line.) Which is better? Hmm... it looks like it
depends. Down here, this one is better. Up here, this one is better.
So for this problem, we'd want to make our choice depending on
the size of the problem that we have.

In fact, for any two straight lines, either they're parallel or
there's a crossover point.

Okay, enough of straight lines. It turns out that a lot of these
graphs look like this: (draw x^3 + 2). whoa... that looks bad.
let's scale out and see how it looks. Wow, that's really bad!
Let's compare it to a straight line: (draw 0.5x + 50). Well, down
here this one's better, but pretty soon this one is better, and
then it's *way way* better.

What about one that's curved the other way? (draw log_2(n)+100).
Starts out bad, but eventually looks better.

What's the takeaway? Well, it seems like if one line is curved upward
and the other one isn't, the curved upward one is eventually going
to be worse. Way worse.

# From the Bottom Up

Okay, let's stop talking about computer programs as black boxes, with
arbitrary functions describing their running times. Let's be concrete.

Here's a program:

```python
def f(x):
    return 7 * (24231 + (332 * 243))
```

How long does it take to run this program? (Q) Yeah, actually, it
doesn't depend on the input at all. So... how long does it take?
Let's dig in for just a
minute and try to list the "things" entailed by running the program.

In order to talk about running times of programs, you need a new
model; we've got to throw out a bunch of our "equality" rules.

For instance. In math class, I can write

```python
7 = 1 + 1 + 1 + 1 + 1 + 1 + 1
```

This equality means that wherever I see this over here (the seven),
I can replace it by this over here (1 + 1 + ...) without changing
the meaning of the term.

However, if we're interested in the *time* taken to evaluate an
expression, suddenly these are different. How many "operations"
are required to evaluate the right-hand expression? Probably
six. How about the one on the left? Probably ... zero.

So now we need a new model. Specifically, we need a model that takes
a program, and takes a sequence of "steps", until we get to an
answer (or maybe we just run forever).

So, let's think about the expression we wrote before:

```python
7 * (24231 + (332 * 243))
```

What are the "steps" involved in evaluating this?

I would probably write them like this:

```python
7 * (24231 + (332 * 243))
```
```python
7 * (24231 + 80676)
```
```python
7 * 104907
```
```python
734349
```

What we're sketching here is an operational semantics
for Python. Specifically, this is what we'd call a
"small-step" semantics, because it doesn't solve programs
in one big step, but rather rewrites them one piece at
a time. This is important for us, because it allows us
to *count* those steps.

We could try to write rules down for how to choose what step
to do next, but that would be way too intense for 202.

Here's one very important thing about these "steps": each
one *always takes the same amount of time.* They don't
have to take the same amount of time as each other, but
each one must always take the same amount of time on
a given machine. Is this true for arithmetic operations?

The answer is... "sort of." If we confine ourselves to fixed-size
numbers, then the answer is yes. Let's assume, for the next couple
of weeks at least, that we're only interested in integers less
than 2^63-1.

What steps would you take to evaluate this one? :

```python
if (3 == 4):
  return 9+2;
else:
  return 8+4;
```

?

You'd probably do something like this:

```python
if (3 == 4):
  return 9+2;
else:
  return 8+4;
```

```python
if false:
  return 9+2;
else:
  return 8+4;
```

```python
return 8+4;
```

```python
return 12;
```

```python
12
```

Once again, each of these steps takes a constant amount of
time.

Finally, what about function calls? Here's a simple example.

```python
def f(x):
    return 16 + x * x

34 + f(2)
```

The steps here are probably something like this:

```python
def f(x):
    return 16 + x * x

34 + f(2)
```

```python
def f(x):
    return 16 + x * x

x_1 = 2
34 + {
  return 16 + x_1 * x_1
}
```

```python
def f(x):
    return 16 + x * x

x_1 = 2
34 + {
  return 16 + 2 * x_1
}
```

```python
def f(x):
    return 16 + x * x

x_1 = 2
34 + {
  return 16 + 2 * 2
}
```

```python
def f(x):
    return 16 + x * x

x_1 = 2
34 + {
  return 16 + 4
}
```


```python
def f(x):
    return 16 + x * x

x_1 = 2
34 + {
  return 20
}
```

```python
def f(x):
    return 16 + x * x

x_1 = 2
34 + 20
```

```python
def f(x):
    return 16 + x * x

x_1 = 2
54
```

Notice anything interesting here? What are these curly brackets? It
turns out that in order to model languages with `return`, you have to be
a bit tricky; these brackets delimit the boundaries of a function
call. You need to keep them there so that when you encounter a `return`,
you can discard context out to the nearest set of curly braces.

Also, what happens to the values that we pass to a function? Last week
we mentioned "substitution" in passing, but it looks like we didn't do
it, here; instead, we created a declaration for 'x'.

Why did we do that? (Q)

(Maybe they get it... probably not.)

It turns out that if you want to allow mutation of local bindings in our language,
substitution isn't going to work. instead, we have to take each of
these bindings, and give it a fresh name, and hoist it up to the top
level. Then, each reference to the given variable becomes a reference
to this new top-level variable.


Okay, one more hurdle. What about mutable values? Do we want mutable
values? Well, in the next couple of weeks, we're going to be talking a
lot about mutable values.  I think it would be a bit irresponsible to
go without mentioning them entirely.

Here's a simple example that creates a list of length 2

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

f(Pair(3, Pair (4, "mt")))
```

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

MV_1 = Pair(4, "mt")
f(Pair(3, MV_1))
```

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

MV_1 = Pair(4, "mt")
MV_2 = Pair(3, MV_1)
f(MV_2)
```

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

MV_1 = Pair(4, "mt")
MV_2 = Pair(3, MV_1)
pr_1 = MV_2
{pr_1.first = 9
 return 2 + pr_1.first}
```

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

MV_1 = Pair(4, "mt")
MV_2 = Pair(3, p_1)
pr_1 = MV_2
{MV_2.first = 9
 return 2 + pr_1.first}
```

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

MV_1 = Pair(4, "mt")
MV_2 = Pair(9, p_1)
pr_1 = MV_2
{return 2 + pr_1.first}
```

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

MV_1 = Pair(4, "mt")
MV_2 = Pair(9, MV_1)
pr_1 = MV_2
{return 2 + MV_2.first}
```

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

MV_1 = Pair(4, "mt")
MV_2 = Pair(9, MV_1)
pr_1 = MV_2
{return 2 + 9}
```

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

MV_1 = Pair(4, "mt")
MV_2 = Pair(9, MV_1)
pr_1 = MV_2
{return 11}
```

```python
class Pair:
    def __init__(self, first, rest):
    # etc. etc.

def f(pr):
  pr.first = 9
  return 2 + pr.first

MV_1 = Pair(4, "mt")
MV_2 = Pair(9, MV_1)
pr_1 = MV_2
11
```

There's something important and different about our `MV` bindings.
They're values. So, if I see a binding like `pr_1`, I need to find
it's value, so I reduce it to `MV_2`. However, we *don't* reduce
MV_2 any further; it's a value. If we were to reduce MV_2 to
`Pair(9, MV_1)`, we would lose the ability to handle mutation
correctly.

Does that look like a headache? It *is* a headache.

I need to make this clear. All of the worst parts of this model
are consequences of mutation. The hoisting instead of substitution
for function calls, the creation of a new binding for every
class that's created: if you don't have mutation in your language,
all of that stuff just evaporates.

Also, the special "return" rules are unnecessary, too; that's
not a consequence of mutation, just poor language design.


Okay, I think that's enough for one day.


--

Copyright (C) 2017, John Clements (clements@racket-lang.org)
