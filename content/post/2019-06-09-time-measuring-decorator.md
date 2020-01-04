---
title: Time measuring decorator
author: ''
date: '2019-06-09'
slug: time-measuring-decorator
categories: []
tags: []
---

This is an example how to write your own time measuring decorator.

```
import datetime
from numpy.random import rand

class time_decorator:

    def __init__(self, f):
        self.f = f
        print(f"Time decorator for {self.f.__name__} created")

    def __call__(self, *args, **kwargs):
        start = datetime.datetime.now()
        result = self.f(*args, **kwargs)
        duration = datetime.datetime.now() - start
        print(f"Duration of {self.f.__name__} function call was {duration}.")
        return result

@time_decorator
def sum_of_random_numbers(n):
  random_numbers = rand(n)
  return sum(random_numbers)
```

    Time decorator for sum_of_random_numbers created



```
sum_of_random_numbers(10_000_000)
```

    Duration of sum_of_random_numbers function call was 0:00:01.092953.

    5001076.5500206305
    

    
**Original Gist**: https://gist.github.com/simecek/8a3f579037dc31d9e91c5043f91e4509

**Try it in Colab**: https://colab.research.google.com/gist/simecek/8a3f579037dc31d9e91c5043f91e4509/time_measuring_decorator.ipynb