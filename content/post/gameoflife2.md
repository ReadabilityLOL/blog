+++
title = "Let's make a web ui for Conway's game of life!"
date = 2026-02-16T14:35:44-05:00
draft = true
tags = ["development","tutorial"]
+++

## Intro

This is a sequel to my [previous post]("https://glitched.tech/gameoflife"). I've decided that I may as well try to make the game of life look good. If you're following along, I would suggest going back to the first post.

Goals:
1. Create a web-based UI for the Game of Life
2. Allow board resizing
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
<div class="game"></div>
```

Now, in `gameoflife.py`

