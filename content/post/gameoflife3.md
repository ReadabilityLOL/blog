+++
title = "Let's make our UI for the Game of Life look better!"
date = 2026-02-20T09:38:20-05:00
draft = true
tags = ["development","tutorial"]
+++

## Intro

We've basically finished the Conway's Game of Life project. All we need to do now is make it look better, with some nicer css.

## Some quick changes

You may have noticed that the grid currently goes off the screen a bit. To change that, I've made some quick CSS and Python changes.

Here's the updated CSS:

```css
html, body{
  height:100%
  margin:0;
}

.row{
  display:flex;
  flex: 1;
  flex-direction: row;
  min-height: 0;
}

.cell{
    border: 1px solid #d3d3d3;
    flex: 1 1 0;
    height:100%;
    aspect-ratio: 1 / 1;
    min-width: 0;
    min-height: 0;
}

.gamegrid{
  display:flex
  flex-direction: column;
  height:100vh;
}

.alive{
    background-color:black;
}

.dead{
    background-color:white;
}



```

I decreased the number of rows by 10% in the Python.

```python
gameArray = createGrid(
    math.floor((window.screen.availHeight/20)*0.9),
    math.floor(window.screen.availWidth/20)
)
```

## Styling the buttons

First off, let's add a margin between our buttons and the grid. Also, put all the buttons inside a `<center>` element. Let's also make them a bit bigger.

```css
button{
    margin-top: 5px;
    width:3%;
}
```
