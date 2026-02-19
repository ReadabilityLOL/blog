+++
title = "Let's make a web ui for Conway's game of life!"
date = 2026-02-19T14:00:00-05:00
draft = false
tags = ["development","tutorial"]
+++

## Intro

This is a sequel to my [previous post]("https://glitched.tech/gameoflife"). I've decided that I may as well try to make the game of life look good. If you're following along, I would suggest going back to the first post.

Goals:
1. Create a web-based UI for the Game of Life
2. Make the board fit screen width and length
3. Allow users to make custom setups, rather than just randomly generated ones

## Pyscript

Since we already wrote all of this in Python, it would be a shame to get rid of it. We'll be using [pyscript](https://pyscript.net) to use python in our website.

Here's our starter html, with some code to run our previous python file and put it in a web terminal thing.

```html
<!doctype html>
<html>
    <head lang="en">
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width,initial-scale=1.0">
        <link rel="stylesheet" href="https://pyscript.net/releases/2026.2.1/core.css" />
        <script type="module" src="https://pyscript.net/releases/2026.2.1/core.js"></script>
    </head>
    <body>
        <script type="py" config="./config.json" src="./gameoflife.py" terminal worker></script>
    </body>
</html>
```

Before we run it though, we need to slightly modify our original code and put `await` in front of all of our inputs.

```python
y = int(await input("\nEnter the length of the grid: "))
x = int(await input("Enter the width of the grid: "))
...
value = (await input("\nPress enter to advance a step or type q to quit: ")).strip()
```

To run, install npm's `serve` or a different webserver and just serve the folder you're doing this in.

## Setting up the grid

Javascript is scarier than python, and we're already doing a lot in python, so why don't we continue with that?

First off, let's comment out the bit that runs all of our functions in the python file. Do the same with the html file, and remove that script tag in the `body`.

Now, let's make a grid in pyscript. Since we already have a python file with most of the logic, and since we're probably not trying to make this code maintainable anyways, let's do everything in `gameoflife.py`

```html
<script type="py" src="./gameoflife.py"></script>
```
First, let's make a container in `gameoflife.html`

```html
<div id="gamegrid"></div>
```

Now, in `gameoflife.py`, we'll set up the grid.

First, we'll import some stuff from pyscript

```python
from pyscript import web
```
Next, we'll write some code to update the grid.

```python
grid = web.page["#gamegrid"]
for y in gameArray:
    row = web.div(class="row")
    for x in y:
        row.append(web.div("cell", class="cell"))
    grid.append(row)
```

If you were to run this, you'd notice it looks terrible and nothing like Conway's Game of Life. The first thing we should do to resolve this is turn everything into a flexbox.

Create styles.css and put this in there:

```css
.row{
    display: flex;
    flex-direction: row;
}
```

Next, of course, include it in your html file.

```html
<link rel="stylesheet" href="styles.css">
```

Now, if you run it, you'll see the word "cell" over and over, in a grid-shaped pattern. 

Let's edit styles.css again to let the cells expand in size to fit the page. While we're at it, let's also add borders to each cell so we can differentiate them.

```css
.cell{
    border: 1px solid black;
    aspect-ratio: 1 / 1;
    flex: 1 0 0;
}
```

You might notice that the cells flow off the page, but we can fix that later.

Now that we've done that, we can get rid of the word "cell" from `gameoflife.py`.

Let's change the line of code updating the grid to add a class depending on whether the cell is living or dead:

```python
grid = web.page["#gamegrid"]
for y in gameArray:
    row = web.div(classes=["row"])
    for x in y:
        if x == 1:
            row.append(web.div(classes=["cell","alive"]))
        else:
            row.append(web.div(classes=["cell","dead"]))
    grid.append(row)
```

Then, let's update `styles.css` to change the background color based on class.

```css
.alive{
    background-color:black;
}

.dead{
    background-color:white;
}
```

Let's stick the grid updater code in a function, so we don't have to keep calling it. I've also added a line to clear the grid each time.

```python
def updatePage():
    grid = web.page["#gamegrid"]
    for y in gameArray:
        row = web.div(classes=["row"])
        for x in y:
            if x == 1:
                row.append(web.div(classes=["cell","alive"]))
            else:
                row.append(web.div(classes=["cell","dead"]))
        grid.append(row)
```

Now, just to test this out fully, let's add a button at the bottom of the html that just advances a turn in the Game of Life.

```html
<button id="advance">Advance</button>
```

To handle it in Python, we also need to import `when` from Pyscript

```python
from pyscript import web, when
```

Then, let's add a script to handle it.

```python
@when("click","#advance")
def advance():
    global gameArray
    gameArray = step()
    updatePage()
```

You may have noticed that I used the `global` keyword here, something that is generally considered bad practice. It is also bad practice here, I was just feeling lazy. Feel free to find a different solution.

## Sizing based on window size

Instead of creating a grid with a size we decide, why don't we grab the available width and height and then divide it by the number of pixels per box?

First, import `window` from pyscript as well. Also, import `math`.

```python
from pyscript import web, when, window
import math
```

Then, let's change our setup code.

```python
gameArray = createGrid(
    math.floor(window.screen.availHeight/20),
    math.floor(window.screen.availWidth/20)
)

populate()

updatePage()

@when("click","#advance")
def advance():
    global gameArray
    gameArray = step()
    updatePage()
```

You'll probably notice that the game of life overflows down the screen. I've decided to ignore this and call it a feature. You'll also most likely notice that the game is inefficient and slows down a lot if there are many cells. Maybe I'll fix this later.

## Allowing custom setup

To allow users to start the game with a custom setup, we'll need to do a couple things. First of all, we'll of course need to add a function to check if a cell has been clicked. Second of all, we'll need to assign each on-screen cell a coordinate that we can relate to our Python grid of cells.

Let's start by adding the ability to click cells.

```python
@when("click",".cell")
def cellclick(event):
    target = event.target
    if "alive" in target.classList:
        target.classList.add("dead")
        target.classList.remove("alive")
    elif "dead" in target.classList:
        target.classList.add("alive")
        target.classList.remove("dead")
```

Now, we need to link the clicks on screen to the array in Python. I personally wasn't sure how to create a proper coordinate system for the on-screen grid, so I instead gave each box an id relating to their coordinates.

```python
def updatePage():
    grid = web.page["#gamegrid"]
    grid.innerHTML = ""
    for y in range(len(gameArray)):
        row = web.div(classes=["row"])
        for x in range(len(gameArray[y])):
            if gameArray[y][x] == 1:
                row.append(web.div(classes=["cell","alive"],id=f"{y}_{x}"))
            else:
                row.append(web.div(classes=["cell","dead"],id=f"{y}_{x}"))
        grid.append(row)
```

Then, we also update the `cellclick` function to update gameArray.

```python
@when("click",".cell")
def cellclick(event):
    target = event.target
    y,x = (target.id).split("_")
    x = int(x)
    y = int(y)

    if "alive" in target.classList:
        target.classList.add("dead")
        target.classList.remove("alive")
        gameArray[y][x] = 0
    elif "dead" in target.classList:
        target.classList.add("alive")
        target.classList.remove("dead")
        gameArray[y][x] = 1
```

## Adding a reset button

First, add a button titled "reset"

```html
<button id="reset">reset</button>
```
And the python code, of course

```python
@when("click","#reset")
def reset():
    global gameArray
    for y in range(len(gameArray)):
        for x in range(len(gameArray[y])):
            gameArray[y][x] = 0
    updatePage()

```

## Adding a randomize button

Once again, add a button titled "randomize"

```html
<button id="randomize">randomize</button>
```
And the Python

```python
@when("click","#randomize")
def randomize():
    reset()
    populate()
    updatePage()
```

## A small bug

You may have noticed that after clicking any buttons, it's impossible to modify the grid, which seems like a pretty big issue. I'm pretty sure this is because we are a cutally deleting and regenerating the grid every time we do anything, which deletes the helpful functions.

Now, a quick rant here. It turns out that you can't have the id of an element start with a number. You might be thinking, "But Josh, haven't you had the id of all the cells start with a number?". Yes. This is true, but it turns out you can't search for an id starting with a number, but it's all right to just have one. That's why I had to modify the code in `updatePage` and `cellclick`. It didn't inconvenience me too much, but it could have been really annoying if I hadn't caught this until later.

Anyway, what I did was create a new function, `createPage`, that creates all the elements, and then update `updatePage` to just modify the cells. This should also speed the whole thing up a bit.

Here's `createPage`:

```python
def createPage():
    grid = web.page["#gamegrid"]
    grid.innerHTML = ""
    for y in range(len(gameArray)):
        row = web.div(classes=["row"])
        for x in range(len(gameArray[y])):
            if gameArray[y][x] == 1:
                row.append(web.div(classes=["cell","alive"],id=f"a{y}_{x}"))
            else:
                row.append(web.div(classes=["cell","dead"],id=f"a{y}_{x}"))
        grid.append(row)

```

Here's `updatePage()`:

```python
def updatePage():
    grid = web.page["#gamegrid"]
    for y in range(len(gameArray)):
        row = web.div(classes=["row"])
        for x in range(len(gameArray[y])):
            target = web.page[f"a{y}_{x}"]
            if gameArray[y][x] == 1:
                #row.append(web.div(classes=["cell","alive"],id=f"{y}_{x}"))
                if "dead" in target.classList:
                    target.classList.add("alive")
                    target.classList.remove("dead")
            else:
                #row.append(web.div(classes=["cell","dead"],id=f"{y}_{x}"))
                if "alive" in target.classList:
                    target.classList.add("dead")
                    target.classList.remove("alive")
        grid.append(row)
```

And here's `cellclick`:

```python
@when("click",".cell")
def cellclick(event):
    print("clicked")
    target = event.target
    y,x = ((target.id)[1:]).split("_")
    x = int(x)
    y = int(y)

    if "alive" in target.classList:
        target.classList.add("dead")
        target.classList.remove("alive")
        gameArray[y][x] = 0
    elif "dead" in target.classList:
        target.classList.add("alive")
        target.classList.remove("dead")
        gameArray[y][x] = 1
```

## Done?

We're almost done with everything now, all we need to do is make everything look better. The code for it is on [my Github](https://github.com/ReadabilityLOL/Python-Implementation-of-Conway-s-Game-of-Life). Feel free to contact me if you think I've made a mistake.
