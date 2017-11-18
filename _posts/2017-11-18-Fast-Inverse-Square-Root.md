---
layout: post
title: Fast Inverse Square Root
categories: Programing
excerpt_separator:  <!--more-->
---

The Fast Inverse Square Root hack is an amazing piece of computer science hackery

This article asusmes you have numpy installed. If you don't, it's super easy to do

The main function can be defined as the following

``` 
def numpy_isqrt(number):
    threehalfs = 1.5
    x2 = number * 0.5
    y = np.float32(number)
    
    i = y.view(np.int32)
    i = np.int32(0x5f3759df) - np.int32(i >> 1)
    y = i.view(np.float32)
    
    y = y * (threehalfs - (x2 * y * y))
    return float(y)

number = float(input('Value:' ))
print (numpy_isqrt(number))
```
