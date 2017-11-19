---
layout: post
title: Fast Inverse Square Root
categories: Programing
excerpt_separator:  <!--more-->
---

The Fast Inverse Square Root (FISR) hack is a fascinating piece of computer science history that centers on a quicker implementation of the operation x^(1/n) (or 1 divided by the root of x). 

This is a common computation needed in graphics programing for finding the distance between two vectors.  FISR manages to accurately approximate this operation by simple subtraction and multiplication, using the 'magical' number 0x5f3759df. How this is done, and why it's so cool, will be explored below.

If you want to follow along and implement the code yourself, this article will be working on the basis of using python with numpy installed. If you don't have access to these tools they are easy to install and only a simple Google search away.

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
