---
layout: post
title:  "Python Generator"
date:   2019-02-09 01:05:19
categories: [python]
comments: true
---

In this post, I would like to cover what python generator is.
Before diving in more deeply, defining what "generator" is necessary, because the term "generator" is used to mean both "a function that generates an iterator" and "resulting iterator".
Here, I would like to use the term `generator` to mean a function which returns a generator object and `generator iterator` to mean an object created by a generator. (This is the official way of distinguishing these two.)
According to the distinction, the following code snippet shows an example of `generator`.

{% highlight python %}
def generator_example():
    for i in range(10):
        yield i
{% endhighlight %}

And, "gen_iterator" in the code snippet below should be `generator iterator`.
{% highlight python %}
gen_iterator = generator_example()
{% endhighlight %}


### "What is generator?"

With the clear distinction above, generator is a function that generates an iterator.
In the previous post, I explained that there is a protocol in iterator:

1) `__next__()` should be implemented.

2) At the end of a series, `StopIteration` should be raised.

So, creating a custom iterator could be a cumbersome process.
In this regard, `Generator` simplifies the creation of iterators. (much more convenient way of writing an iterator!)

{% highlight python %}
def generator_example():
    print("Example of an generator")
    for i in range(3):
        yield i
{% endhighlight %}

Each time the `yield` statement is executed the function generates a new value.

```
>>> gen_example = generator_example()
>>> gen_example
<generator object generator_example at 0x10efecf00>
>>> next(gen_example)
Example of an generator
0
>>> next(gen_example)
1
>>> next(gen_example)
2
>>> next(gen_example)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

What is notable here is that the function is not executed until the `next` method gets called. 
When `next` method is called for the first time, the function starts executing until it reaches `yield` statement.

The following example shows the interplay between `yield` and call to `next` method on generator iterator.

{% highlight python %}
def generator_example():
    for i in range(3):
        print("before yield")
        yield i
        print("after yield")
{% endhighlight %}

```
>>> gen_example = generator_example()
>>> next(gen_example)
before yield
0
>>> next(gen_example)
after yield
before yield
1
>>> next(gen_example)
after yield
before yield
2
>>> next(gen_example)
after yield
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```

### Generator Expressions

We can create a `generator iterator` with the following expression:
```
>>> gen_example = (i for i in range(5))
>>> gen_example
<generator object <genexpr> at 0x10ddbff00>
```

The notable differences from list comprehensions are as follows:
- It does not construct a list
- Only useful purpose of it is iteration
- Once consumed, it can't be reused

### Generators vs. Iterators

A `generator iterator` is different from `iterator` which is not created by `generator`.

To be specific,
- A generator is a one-time operation. You can iterate over the generated data once, but if you want to do it again, you have to call the generator function again.
- This is different than a list (which you can iterate over as many times as you want)

Here is the example:
```
>>> generator_iterator = (i for i in range(5))
>>> generator_iterator
<generator object <genexpr> at 0x10ddbfeb0>
>>> sum(generator_iterator)
10
>>> sum(generator_iterator)
0
```

#### References:
- https://docs.python.org/3/glossary.html
- https://anandology.com/python-practice-book/iterators.html
- http://www.dabeaz.com/generators/Generators.pdf