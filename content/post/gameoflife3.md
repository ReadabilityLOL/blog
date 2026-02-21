+++
title = "Let's make our UI for the Game of Life look better!"
date = 2026-02-20T09:38:20-05:00
draft = false
tags = ["development","tutorial"]
+++

## Intro

We've basically finished the Conway's Game of Life project. All we need to do now is make it look better, with some nicer css.

## Some quick changes

I'm hosting on cloudflare, so I changed the html file to index.html

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

I decreased the number of rows by 25% in the Python code.

```python
gameArray = createGrid(
    math.floor((window.screen.availHeight/20)*0.75),
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

## Adding a header

Let's add a header to index.html.

```html
<center><h1>Welcome to Conway's Game of Life!</h1></center>
```

Let's style it.

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans:ital,wght@0,100..900;1,100..900&display=swap" rel="stylesheet">
```

```css
* {
  font-family: "Noto Sans", sans-serif;
  font-optical-sizing: auto;
  font-weight: <weight>;
  font-style: normal;
  font-variation-settings:
    "wdth" 100;
}
```

## More styling

I made the background color blue and added border radii.

```css
h1{
  padding:10px;
  min-width: 0px;
  width: 40%;
  border: solid 5px black;
}

h1, button{
  background-color: #79b5ff;
  border-radius: 5px;

}

button{
    margin-top: 10px;
    font-size: 2ch;
    border: solid 3px black;
}
```

Let's make the margins on the side larger.

.row{
  display:flex;
  flex: 1 1 0;
  flex-direction: row;
  min-height: 0;
  width:90%;
  margin-left:5%;
}


## Done?

I think I'm pretty done with this project now, unless I think of something later. Thanks for following this tutorial.
