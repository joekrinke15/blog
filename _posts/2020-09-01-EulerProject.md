---
layout: post
title: The Euler Project
image: https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Coding.jpg?raw=true
---
The Euler project is a website that contains computational problems that are intended to be solved by computer programs. In this post I'll be examining a few problems I found interesting and detail my solutions. 

# Fibonacci Sequence
The Fibonacci sequence is a sequence of numbers where each number is defined as being the sum of the previous two numbers, starting from 0 and 1. 

The sequence begins: 0, 1, 1, 2, 3, 5..... and continues infintely. 
This sequence is often found in both nature and mathematics, so much research has been done on its properties and potential scientific applications. The problem I am personally going to examine is the following; how far in the Fibonacci sequence do you need to go before finding a number that exceeds 1000 digits? 

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
If we take 47, reverse and add, 47 + 74 = 121, which is palindromic.

Not all numbers produce palindromes so quickly. For example,

349 + 943 = 1292

1292 + 2921 = 4213

4213 + 3124 = 7337

That is, 349 took three iterations to arrive at a palindrome.

Mathemeticians theorize that some numbers never produce a palindrome through this repetition of reversal and addition. Numbers with this property are called Lychrel numbers.  The vast majority of numbers that eventually become a palindrome take less than 50 iterations of reversal and addition to do so. For this problem, I will be determining how many numbers under 10000 are Lychrel numbers. This calculation will assume that a number is a Lychrel number if it doesn't become a palindrome within 50 iterations.

I began by creating functions to sum a number and its reverse.
```Python3
#Define function to reverse a number.
def reverse(number):
    reversed = int(str(number)[::-1]) 
    return(reversed)

#Define function to sum a number and its reverse. 
def sum_reverse(number): 
    return(number + reverse(number))
```
Next, I had to create a function to check if a given number is a palindrome. This works by reversing the number and checking for equality.
```Python3
#Define a function to check if a number is a palindrome. 
def is_palindrome(number):
    if (reverse(number) == number):
        return True
    else:
        return False
```
The last function repeats the reversal and summation process 50 times and checks if the output of each iteration is a palindrome. 
```Python3
#Define a function to check if a number is a lychrel value.
def is_lychrel(number):
    new_num = number
    bool_lychrel = True
    for i in range (50):
        new_num = sum_reverse(new_num)
        if is_palindrome(new_num): #Change flag to false if any numbers produced are palindromes. 
            bool_lychrel = False
    return(bool_lychrel)
```
Finally, I loop over the numbers from 1-10000 and append their Lychrel status to a list. I can then sum the list to determine how many numbers are Lychrel- this produces a final answer of 249. 
```Python3
#Iterate over the first 10000 numbers to see how many lychrel numbers there are.
lychrel_list = []
for i in range(0,10000):
    lychrel_list.append(is_lychrel(i))
print("The number of Lychrel numbers from 1 to 10000 is {}.".format(sum(lychrel_list)))
```

# Dice Game
