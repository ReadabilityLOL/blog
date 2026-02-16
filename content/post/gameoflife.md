+++
title = "Let's make Conway's Game of Life in Python!"
date = 2026-02-14T22:17:24-05:00
draft = false
tags = ["development","tutorial"]
+++

## What is Conway's game of life?

Conway's Game of Life is a simulation that runs on an infinite two dimensional grid. Each square in the grid can be in one of two states, dead or alive. On each turn, the cells interact with their neighbors, following a few rules.

1. Every living cell with less than two live neighbors dies because of underpopulation

2. Every living cell with more than three neighbors dies because of overpopulation

3. Every dead cell with exactly three neigbors becomes alive again, because of reproduction.

These three simple rules actually lead to surprisingly complicated results, depending on the starting position of the game. For example, Conway's game is actually Turing Complete, meaning that any problem that can be solved with a computer or other programming language can be theoretically solved using Conway's game. This also means that Conway's game of life can be made *inside itself*, like how you could theoretically program Python in Python.

## Create basic grid

Let's make a new python file called `gameoflife.py`.

To start, we'll create a variable to contain the grid that the game is played on. Since the grid is two-dimensional, it'll be a nested list, with the grid variable containing other lists inside of it. To start it off, we'll just make a basic 1x1 grid.

```python
#create the gameArray list
gameArray = [[0]]
```

Throughout this project, we'll use the number 0 to represent a dead cell, and 1 to represent a living cell. I suppose you could change it, but this seems pretty logical, so I don't know why you would.

## Create some helper functions

To start off the logic behind the project, let's create some functions that do various repetitive tasks. Let's start with simple functions to check if a cell is living or not.

```python
def isAlive(x,y):
    return gameArray[y][x] == 1
```

Testing it out on our array gives us False, and if you update the code to have the starting array have a one in it, it returns True, as expected.

Now onto another important task: Updating the array. I've made a variable called `l` to set which list is being updated, which will be important later, and I've made it default to gameArray just because I was being lazy.

```python
def setDead(x,y,l=gameArray):
    l[y][x] = 0

def setAlive(x,y,l=gameArray):
    l[y][x] = 1
```

Let's test these out

```python
print(isAlive(0,0)) #False
setAlive(0,0)
print(isAlive(0,0)) #True
setDead(0,0)
print(isAlive(0,0)) #False
```

## Setting up the grid

All these functions are great and all, but it's kind of boring having a single cell. Let's make a function to create a grid of a certain length and width, all filled with zeroes.

```python
def createGrid(length, width):
    return [([0] * width) for x in range(length)]
```

While we're at it, why not make a function to make the printed list look nicer?

```python
def prettyprint():
    for y in gameArray:
        for x in y:
            print(x,end="")
        print()
```


Let's test it out!

```python
prettyprint()
gameArray = createGrid(5,7)
print() #just to get a newline
prettyprint()
```
```bash
0

0000000
0000000
0000000
0000000
0000000
```

Now, as a final part of this section, let's make a function to randomly populate the grid with ones. No need to go crazy here, let's just make it so every square has a 1/3 chance to be alive. We can always change the function up later.

```python
import random

# ...all the other stuff


def populate():
    for x in gameArray:
        for y in range(0,len(x)): #Get the index of each item rather than the value
            if random.randint(1,3) == 1:
                x[y] = 1 
```

Now to test it again:

```python
gameArray = createGrid(5,7)
populate()
prettyprint()
```
```bash
0001010
0010010
0111000
0010001
0110101
```

## Creating the logic behind the game

Now, let's move on to creating functions that check how many living and dead neighbors each square has. 

```python
def numNeighbors(x,y):
    number = 0
    if isAlive(x-1,y-1):
        number+=1
    if isAlive(x-1,y):
        number+=1
    if isAlive(x-1,y+1):
        number+=1
    if isAlive(x,y-1):
        number+=1
    if isAlive(x,y+1):
        number+=1
    if isAlive(x+1,y-1):
        number+=1
    if isAlive(x+1,y):
        number+=1
    if isAlive(x+1,y+1):
        number+=1

    return number
```

This function is not very pretty, but we can change that later.

Let's test it out:

```python
gameArray = createGrid(5,7)
populate()
prettyprint()
print(numNeighbors(1,1))
```
```bash
1000010
0011000
0010000
0100010
0010010
3
```

This works quite well, except for a problem you may have noticed.

If we change our test to:

```python
print(numNeighbors(5,7))
```
we get

```bash
1000000
1011010
1100010
0110000
0101001
Traceback (most recent call last):
  File "/home/kram/projects/gameoflife/gameoflife.py", line 56, in <module>
    print(numNeighbors(4,6))
          ~~~~~~~~~~~~^^^^^
  File "/home/kram/projects/gameoflife/gameoflife.py", line 38, in numNeighbors
    if isAlive(y-1,x+1)== 1:
       ~~~~~~~^^^^^^^^^
  File "/home/kram/projects/gameoflife/gameoflife.py", line 8, in isAlive
    return gameArray[y][x] == 1
           ~~~~~~~~~^^^
IndexError: list index out of range
```

This is because our code checks the areas around each square, even if the area is outside of the list index. For now, we can just change our `isAlive` function to return false if either coordinate is outside of the array's bounds.

```python
def isAlive(x,y):
    if y >= len(gameArray) or x >= len(gameArray[y]):
        return False
    return gameArray[y][x] == 1
```
Now, if we run it again, everything should work!

```python
1000000
0110111
1001100
0001011
1001000
2
```

Now, in a similar vein, we have another problem. 

```python
print(numNeighbors(0,0))
```
```bash
1001111
0111000
0010110
0100010
1100010
4
```

At first, there doesn't seem to be a problem, since no error was thrown, until you realize that (0,0) only has one neighbor! This is because in Python, when we give a list a negative index, it begins indexing backwards, with -1 meaning the last element of a list and so on.

To fix this, we just need to update isAlive to also make sure neither value is negative.

```python
def isAlive(x,y):
    if y >= len(gameArray) or x >= len(gameArray[y]) or x < 0 or y < 0:
        return False
    return gameArray[y][x] == 1
```
```bash
1110100
0101101
0110100
0100011
1100000
2
```
Yay!


## Updating the grid

Now that we've created a lot of useful functions, it's almost time to start updating the grid. What I plan to do is create a copy of the grid, update it, and then replace the original grid with the copy.

First off, because lists are weird sometimes, let's import the `copy` module to make sure the copy of our list doesn't interfere with the main one.

```python
import copy
```

Now, to start our function. We'll call it `step`, and it will go cell by cell and see which ones become alive and which ones die.

```python
def step():
    gameArrayCopy = copy.deepcopy(gameArray)
    for y in range(len(gameArray)):
        for x in range(len(gameArray[y])):
            if isAlive(x,y):
                if not (1 < numNeighbors(x,y) < 4):
                    setDead(x,y,gameArrayCopy)
            else:
                if numNeighbors(x,y) == 3:
                    setAlive(x,y,gameArrayCopy)
    return gameArrayCopy
```

Now, to test it out:

```python
gameArray = createGrid(5,7)
populate()
prettyprint()
print()
gameArray = step()
prettyprint()
```
```bash
0000010
0000011
0010000
1101010
1011000

0000011
0000011
0110111
1001100
1011100
```

It works! Side note: I spent an hour debugging this because I switched x and y for the numNeighbors function.

## Make it look a bit nicer

Since we've finished the important bits, let's make this project look a bit nicer. First off, let's change `prettyprint` to use the unicode white and black vertical rectangles instead.

```python
def prettyprint():
    for y in gameArray:
        for x in y:
            if x == 1:
                print("▮", end="")
            else:
                print("▯", end="")
        print()
```

Testing it again:

```python
gameArray = createGrid(5,7)
populate()
prettyprint()
print()
gameArray = step()
prettyprint()
```
```bash
▯▯▯▯▮▯▮
▯▯▯▯▯▮▯
▮▮▯▯▮▯▯
▯▯▮▯▯▯▯
▯▯▯▯▮▯▮

▯▯▯▯▯▮▯
▯▯▯▯▮▮▯
▯▮▯▯▯▯▯
▯▮▯▮▯▮▯
▯▯▯▯▯▯▯
```

Looks nicer, doesn't it?

Now, let's make a small user interface, nothing fancy, just something asking users to press enter to move forward a step.

```python
gameArray = createGrid(20,20)
populate()

print("\nWelcome to Conway's game of life! \n\n")
while True:
    print()
    prettyprint()
    value = input("\nPress enter to advance a step or type q to quit: ").strip()
    if value == "":
        gameArray = step()
    elif value == "q" or "Q":
        break
    else:
        print(f"Unknown input {value}. Accepted values are: <blank>, 'q', or 'Q'")

```

Trying it out:

```bash

Welcome to Conway's game of life!

▯▯▯▯▯▮▯▯▯▮▮▯▯▯▯▯▮▮▯▮
▯▯▮▯▯▯▯▯▯▮▯▮▮▮▮▯▯▮▯▯
▯▮▯▮▯▯▯▯▯▮▮▯▯▮▯▮▯▯▯▯
▮▯▮▯▮▯▮▯▯▮▯▯▮▯▯▯▯▮▯▯
▯▯▯▮▯▯▯▯▯▯▮▯▮▯▯▯▮▯▯▮
▯▯▮▯▯▮▯▮▯▯▯▯▮▯▮▯▯▮▮▯
▯▯▮▯▯▮▯▯▯▯▮▯▯▯▯▯▮▮▮▯
▯▮▮▮▯▯▯▯▮▯▮▮▯▯▮▯▯▯▮▮
▯▯▯▮▯▯▯▯▯▯▮▯▯▮▮▮▯▯▮▯
▮▯▮▯▮▯▯▯▯▯▯▮▯▯▯▯▯▯▯▮
▯▮▯▯▯▯▯▮▯▯▯▮▮▮▯▯▯▯▯▯
▯▯▯▯▯▮▯▯▮▯▮▯▮▮▯▯▯▮▯▮
▮▯▮▯▯▯▯▮▯▮▯▮▯▮▮▯▮▯▮▯
▯▯▯▯▯▯▮▯▮▮▮▯▮▯▯▯▯▮▮▮
▯▯▯▮▯▮▮▯▯▯▮▯▯▮▯▯▯▯▯▯
▯▯▮▯▯▯▯▯▮▮▯▮▯▮▯▮▯▮▮▮
▯▯▮▮▮▯▮▯▯▯▯▮▯▮▯▯▯▯▯▯
▯▮▯▯▯▮▯▮▯▯▯▯▮▮▯▯▯▯▯▯
▯▯▮▯▮▯▯▮▯▯▮▯▯▯▮▯▯▯▮▯
▯▯▯▯▯▮▯▯▯▮▯▯▯▯▮▮▯▯▮▯

Press enter to advance a step or type q to quit: 

▯▯▯▯▯▯▯▯▯▮▮▮▮▮▯▯▮▮▮▯
▯▯▮▯▯▯▯▯▮▯▯▮▮▮▮▮▯▮▮▯
▯▮▯▮▯▯▯▯▮▮▯▯▯▯▯▯▮▯▯▯
▯▮▮▯▮▯▯▯▯▮▯▯▮▮▯▯▮▯▯▯
▯▮▮▮▮▮▮▯▯▯▯▯▮▯▯▯▮▯▯▯
▯▯▮▮▮▯▮▯▯▯▯▯▯▮▯▮▯▯▯▮
▯▯▯▯▮▯▮▯▯▮▮▯▯▮▯▮▮▯▯▯
▯▮▯▮▮▯▯▯▯▯▮▮▯▮▮▯▮▯▯▮
▯▯▯▯▮▯▯▯▯▮▮▯▮▮▮▮▯▯▮▯
▯▮▮▮▯▯▯▯▯▯▮▮▯▯▯▯▯▯▯▯
▯▮▯▯▯▯▯▯▯▯▮▯▯▮▯▯▯▯▮▯
▯▮▯▯▯▯▮▮▮▮▮▯▯▯▯▯▯▮▮▯
▯▯▯▯▯▯▮▮▯▯▯▯▯▯▮▯▮▯▯▯
▯▯▯▯▯▮▮▯▮▯▯▯▮▯▮▯▯▮▮▮
▯▯▯▯▯▮▮▯▯▯▯▯▯▮▮▯▮▯▯▯
▯▯▮▯▯▯▮▮▯▮▯▮▯▮▯▯▯▯▮▯
▯▮▮▮▮▮▮▮▮▯▮▮▯▮▯▯▯▯▮▯
▯▮▯▯▯▮▯▮▯▯▯▮▮▮▮▯▯▯▯▯
▯▯▯▯▮▮▯▯▮▯▯▯▯▯▮▮▯▯▯▯
▯▯▯▯▯▯▯▯▯▯▯▯▯▯▮▮▯▯▯▯

Press enter to advance a step or type q to quit: quit
```

Last of all, let's let the user input the dimensions of the grid.

```python
print("\nWelcome to Conway's game of life!")

validinput = False
while not validinput:
    try:
        y = int(input("\nEnter the length of the grid: ").strip())
        x = int(input("Enter the width of the grid: ").strip())
        validinput = True
    except KeyboardInterrupt:
        quit()
    except:
        print("\ninvalid input, try again")

gameArray = createGrid(y,x)

populate()

while True:
    print()
    prettyprint()
    value = input("\nPress enter to advance a step or type q to quit: ").strip()
    if value == "":
        gameArray = step()
    elif value in ("q", "Q"):
        break
    else:
        print(f"Unknown input: '{value}'. Accepted values are: <blank>, 'q', or 'Q'")

```
```lisp

Welcome to Conway's game of life!

Enter the length of the grid: 7
Enter the width of the grid: eight

invalid input, try again

Enter the length of the grid: 7
Enter the width of the grid: 8

▮▮▯▯▯▯▯▯
▮▯▯▯▯▯▯▯
▮▯▯▮▮▮▮▯
▯▯▮▯▮▮▯▯
▯▯▯▮▯▮▮▯
▯▯▯▯▯▯▮▯
▮▯▯▯▯▯▯▮

Press enter to advance a step or type q to quit: 

▮▮▯▯▯▯▯▯
▮▯▯▯▮▮▯▯
▯▮▯▮▯▯▮▯
▯▯▮▯▯▯▯▯
▯▯▯▮▯▯▮▯
▯▯▯▯▯▮▮▮
▯▯▯▯▯▯▯▯

Press enter to advance a step or type q to quit: advance
Unknown input: 'advance'. Accepted values are: <blank>, 'q', or 'Q'

▮▮▯▯▯▯▯▯
▮▯▯▯▮▮▯▯
▯▮▯▮▯▯▮▯
▯▯▮▯▯▯▯▯
▯▯▯▮▯▯▮▯
▯▯▯▯▯▮▮▮
▯▯▯▯▯▯▯▯

Press enter to advance a step or type q to quit: q
```
 ## All done?

 That seems to be it for now. Thank's for reading this tutorial. My code for this project is on my [Github](https://github.com/ReadabilityLOL/Python-Implementation-of-Conway-s-Game-of-Life) Feel free to contact me if I messed something up.
