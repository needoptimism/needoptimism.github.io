---
layout: post
title:  "Python Iterable & Iterator"
date:   2019-02-07 00:04:19
categories: [python]
comments: true
---

While reading documents in Python, it's common to see the terms like `iterable`, `iterator`, and `generator`.
Unfortunately, however, I have not tried to understand these confusing concepts clearly so far, so let me try to make these concepts clear in this post. (But, in order to avoid putting too many contents in this post, I will cover `generator` in the next post.)

### "What is iterable?"

- It is an object that is able to return its members one at a time.
- Then, how we could make it available to return members of a certain object one at a time?
- Iterable objects should support at least either `__iter__()` or `__getitem__()` method.
- `list`, `str`, `tuple`, `dict`, and objects of any classes you define with `__iter__()` or `__getitem__()` methods are all iterables.
- Iterables can be used in a `for` loop.
- When Iterables passed as an argument to the built-in function `iter()`, it returns an iterator object.


Let's make a simple `iterable` that has `__getitem()__` function.
{% highlight python %}
class IterableObj:
    def __init__(self):
        self.data = 0
    
    def __getitem__(self, index):
        if index == 4:
            raise IndexError
        return self.data

iterable_obj = IterableObj()

for i in iterable_obj:
    print(i)

print(type(iter(iterable_obj)))
{% endhighlight %}

The output is as follows:
```
0
0
0
0
<class 'iterator'>
```

You can now see that this mock iterable can be used in `for` loop, and turned into `iterator` type when passed to built-in `iter()` function. (NOTE: What `for` loop does behind the scenes, in fact, is to call `iter()`)


### Then, "What is iterator?"

As we have seen so far, `iterator` is generated through passing `iterable` to `iter()` function.
Then, what is the essence of `iterator`? How we can iterate over different iterators?
There is a specific protocol for iterator, which is as follows:

- Iterator object has the method `__next__()` which accesses members in the given object one at a time.
- And, when there are no more members, `__next__()` raises `StopIteration` exception which causes for loop to terminate.

Just like `iterable`, `iterator` can easily be implemented like the following:.

{% highlight python %}
class IteratorObj:
    def __init__(self):
        self.data = [1,2,3]
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index == len(self.data):
            raise StopIteration
        self.index += 1
        return self.data[self.index-1]


iterator_obj = IteratorObj()
for i in iterator_obj:
    print(i)
{% endhighlight %}

The output is as follows:
```
0
0
0
```

As a side note, many built-in functions accept iterators as arguments:

{% highlight python %}
# continued from codes above
print(sum(iterator_obj))
print(list(iterator_obj))
{% endhighlight %}

The code snippets above results in:
```
6
[1, 2, 3]
```


#### References:
- https://docs.python.org/3/glossary.html
- https://anandology.com/python-practice-book/iterators.html
- http://www.dabeaz.com/generators/Generators.pdf