---
layout: post
title: Fast Inverse Square Root
categories: Programing
excerpt_separator:  <!--more-->
---

The Fast Inverse Square Root (FISR) hack is a fascinating piece of computer science history that centers on a quicker implementation of the operation x^(1/n) (or 1 divided by the root of x). 

This is a common computation needed in graphics programing for normalizing vectors.  FISR manages to accurately approximate this operation by simple subtraction and multiplication, using the 'magical' hexidecimal number 0x5f3759df. How this is done, and why it's so cool, will be explored bit by bit below.

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

## Lines 1 - 3
Simple enough, we set up the first two variables for the Newton approximation in line 9 and assign the given number to a float 'y'

## Lines 5 - 7

### Floats and Ints
We then assign that same float to a variable 'i', but in the int format. This is a pretty radical move, as floats and ints are in no way analogous. The float itself is made up of three distinct parts: The first single bit being the sign, the next 8 bits the exponent of the number and the remaining 23 bits the mantissa. The mantissa's overall value is bounded between 0 and slightly less than 1. 

In action, a float is able such as 119 or -18 by having some kind of mantissa that is then modified by leveraging the exponent and sign parts of the float. The int on the other hand, just sees 32, 0 - 9 digits and takes them at face value to be representing a number in the nth base-10 column they occupy.

### Bitwise shift
The conversion from float to int allows us to perform the next steps of the hack, the bitwise shift and subtraction on line 6. 

At the end of line 6 you can the bitwise shift of 1 taking place (>> 1) but it's not clear what exactly this does. Essentially a bitwise shift is a left or right push of some number's binary representation, so if we shifted 44 once to the right it would look like this

44 = 101100

Right shift

22 = 010110

### 0x5f3759df
With our given number shifted, we then subtract it from 0x5f3759df. We do this because, mathematically, what we are looking for can be reduced to log2 (1 + x), where x i between 0 and 1. As shown here, http://h14s.p5r.org/2012/09/0x5f3759df.html, this can be approximated by simply computing x + c, where c is some constant. By extended deduction, we can find that c = 0.0450465 is an excellent approximation. When entered into a formula too esoteric for this article (but present in the link above) this value eventually gives us the magical value of 0x5f3759df, a reliable constant for approximating 1 divided by root x.

Finally, we convert the number back to a float and reassign y to equal it.

## Lines 9 - 13
As a hold over from the original code, we preform the optional step of running one Newton approximation on our modified float and finally output it. 

In the last two lines, we set up a custom input mechanism and finally tell the program to print out the result when its done.

That's all folks!
