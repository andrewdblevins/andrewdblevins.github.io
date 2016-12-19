---
layout: post
title: Monoids
---

A monoid is a combination of an object (a,b,c) and an operation ($+$) that meets the following conditions

* the operation on two of the objects produces a new object. e.g. $int +  int = int$
* the operation must be associative: $(a + b) + c = a + (b + c)$
* a null object $e$ must exist, such that $e + a = a + e = a$

$(0,int,+)$ is a monoid:

* $(a + b) + c = a + (b + c)$ 
* $0+a=a+0=a$

$(1,int,\times)$ is a monoid: 
* $(a \times b)\times c = a\times(b\times c)$ 
* $1\times a=a \times 1=a$


It turns out that if you have these properties, it becomes trivial to run these operations on massive datasets very quickly while using very little memory. Also splitting up these operations and running them in parallel becomes trivial. A couple caveats before we begin. We are doing this in python. 

But for something that cares so much about the types of objects, a duck typed language like python is not a very natural choice. These concepts fit much better in strongly typed languages like Scala or Haskell.

However, python is plenty to gain intuition for the topic. We will just need to do an extra step of typifying python objects into monoid compatable objects, and we will have to be careful that we don't feed out monoids any objects of the wrong type.

```python
class Monoid:
    def __init__(self, null, typify, op):
        self.null = null
        self.typeify = typify
        self.op   = op

    def __call__(self, *args):
        result = self.null
        for arg in args:
            arg = self.typeify(arg)
            result = self.op(result, arg)
        return result
```

We have seen some simple examples of monoids, but it turns out that they can get much more complicated. 
* Matrix Multiplication 
* Cartesian Products 
* Statistical Moments
* Bloomfilters
* HyperLogLogs 
* Stochastic Gradient Descent

```python
def cartesian_prod(a_list,b_list):
    c = []
    for a in a_list:
        for b in b_list:
            c.append(a+b)
    return c

cart_prod_monoid  = Monoid([''],  lambda x: x, cartesian_prod)

base = ['A','T','C','G']
print cart_prod_monoid(base,base,base,base)
```

```python
phone_keys = {'2': "abc",
              '3': "def",
              '4': "ghi",
              '5': "jkl",
              '6': "mno",
              '7': "pqrs",
              '8': "tuv",
              '9': "wxyz",
              '0':' ',
              '1':' '}
#wget https://github.com/dolph/dictionary/blob/master/enable1.txt?raw=true
english_words = set([word.strip() for word in open('enable1.txt').readlines()])

def phone_words(n):
    possible_letters = [phone_keys[digit] for digit in str(n)]
    return [word for word in cart_prod_monoid(*possible_letters) if word in english_words]
    
    
#english_words    
phone_words(228)
```

[Chan et al](http://i.stanford.edu/pub/cstr/reports/cs/tr/79/773/CS-TR-79-773.pdf) provides a formulation of the statistical moments that can be calculated like monoids