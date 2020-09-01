---
layout: post
title: The Euler Project
image: https://github.com/joekrinke15/JoeKrinke15.github.io/blob/master/img/Coding.jpg?raw=true
---
The Euler project is a website that contains problems that are intended to be solved by computer programs. In this post I'll be examining a few problems I found interesting and detail my solutions. 

# Fibonacci Sequence
The Fibonacci sequence is a sequence of numbers where each number is defined as being the sum of the previous two numbers, starting from 0 and 1. 
The sequence begins: 0, 1, 1, 2, 3, 5..... and continues infinitely. 
This sequence is often found in both nature and mathematics, so much research has been done on its properties and potential scientific applications. 
<p align="center">
  <img src="https://mistrzwitold.com/wp-content/uploads/2018/10/HOW-NATURE-CREATED-THE-FIBONACCI-SEQUENCE-%E2%80%93-MATH-IN-NATURE.jpg">
</p>

The problem I am personally going to examine is the following; how far in the Fibonacci sequence do you need to go before finding a number that exceeds 1000 digits? 

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

Mathematicians theorize that some numbers never produce a palindrome through this repetition of reversal and addition. Numbers with this property are called Lychrel numbers.  The vast majority of numbers that eventually become a palindrome take less than 50 iterations of reversal and addition to do so. For this problem, I determined how many numbers under 10000 are Lychrel numbers. This calculation assumed that a number is a Lychrel number if it doesn't become a palindrome within 50 iterations.

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
The last problem I chose to solve was a bit trickier than the previous ones. The problem is as follows: 

Peter has nine four-sided dice, each with faces numbered 1, 2, 3, 4. 
Colin has six six-sided dice, each with faces numbered 1, 2, 3, 4, 5, 6.

Peter and Colin roll their dice and compare sums: the highest sum wins. The result is a draw if the sums are equal. 
What is the probability that Peter beats Colin? Give your answer accurate to 7 decimal places.
<p align="center">
  <img width="600" height="450" src="https://miro.medium.com/max/1200/1*QkbOwDJaAqSj29wpIBAkaw.jpeg">
</p>


My initial approach to solve this problem was a Monte Carlo simulation, that is, I would simulate the game being played over and over again and estimate Peter's win rate. The more simulations I ran the more accurate (in theory) the estimate would be. I began this approach by creating a function that simulates each player's throws; the function "rolls the dice" for each player and compares the sum of the dice to produce the winner. 
```Python3
#A function to simulate throwing the dice once.
def play_game():
    #Calculate the scores for each player
    pete_score = np.sum(np.random.randint(low = 1, high = 5, size = 9))
    colin_score = np.sum(np.random.randint(low = 1, high = 7, size = 6))
    #Compare scores to see who wins
    if pete_score > colin_score:
        winner = 1.0
    else:
        winner = 0.0
    return(winner)

```
Now that I could model one throw I could throw the dice many times to try to estimate Peter's true win percentage. I decided to run 1000000 simulations of the game and see what the result was. This gave an estimate of Peter's win rate being 0.5739220. I was happy with this answer and submitted it to the Euler Project for verification. Unfortunately, however, the answer I produced was only accurate to the first 3 decimal places. I needed to find a more exact way to estimate the win rate. 

```Python3
#Run 1000000 simulations of the game. 
winners = []
for i in range(1000000):
    winner = play_game()
    winners.append(winner)
    
#Print the result. It's accurate to the first ~3 decimal places.
print("Pete's win probability is about {}.".format(np.mean(winners)))
```
