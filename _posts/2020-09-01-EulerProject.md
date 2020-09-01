---
layout: post
title: The Euler Project
image: https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Coding.jpg?raw=true
---
The Euler project is a website that contains computational problems that are intended to be solved by computer programs. In this post I'll be examining a few problems I found interesting and detail my solutions. 

# Fibonacci Sequence
The Fibonacci sequence is a sequence of numbers where each number is defined as being the sum of the previous two numbers, starting from 0 and 1. 

The sequence begins: 0, 1, 1, 2, 3, 5..... and continues infintely. 
This sequence is often found in both nature and mathematics, so much research has been done on their properties and potential scientific applications. The problem I am personally going to examine is the following; how far in the Fibonacci sequence do you need to go before finding a number that exceeds 1000 digits? 

I began by creating a Python generator that produces the values of the Fibonacci sequence. This allows me to iterate over the generator and continually produce consecutive values. 

```Python3
#Create a python generator to yield the values of the Fibonacci sequence.
def fibonacci():
    n_1, n_2 = 0,1
    while True:
        yield (n_1 + n_2)
        n_1, n_2 = n_2, n_1 + n_2 #Update values of n-1 and n-2.

```
Since I now had a way to create the values in the sequence, all I needed to do was calculate the values and check how many digits each value had. Once the program hits 1000 digits it stops and produces the answer. It ultimately takes 4782 iterations of the Fibonacci sequence to produce a number with at least 1000 digits.

```Python3
#Create fibonacci generator and start counting the indices. 
f = fibonacci()
index = 1

#Iterate over the generator to produce values until you get to a value with a length of 1000. 
for x in f:
    index += 1
    length = (int(math.log10(x))+1) #Check the length of the number. 
    if (length == 1000):
        print ('The first index of the Fibonacci sequence to break 1000 digits is {}. It has a length of {} digits.'.format(index, length))
        break
```
# Lychrel Numbers 

# Dice Game
