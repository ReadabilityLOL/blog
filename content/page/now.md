+++
title = "What I'm doing right now"
author = "Joshua Anand"
description = "What I'm doing now"
nocomments = true
+++

<style>
date{
    display: none;
}
</style>

Last updated: 2025-12-02

## Who are you, anyway?

I’m Josh, a high schooler who decided last year to start a blog. I’m interested in technology, old games, and books. When I’m not wasting my time mindlessly doomscrolling on Reddit, I’m usually reading a book or trying to teach myself something, like how to make a blog or how to design a wallet in Freecad. I like making things, and I intend this blog to be both a log of things I make and a way to get more practice writing. You never know, it might even help me on a j*b applicat**n one day.

## The website
I'm going to try to update this site around once every two weeks. If I don't, just send me an angry email or something, and I'll be sure to disregard it completely.

## What I'm reading/listening to
I'm currently rereading Discworld in order of books available at my library, and have picked up *The Science of Discworld*. I'm also reading *Starship Troopers* by Robert A. Heinlein, *This Inevitable Ruin* by Matt Dinniman, and *The Good Virus* by Tom Ireland.

## What I'm watching
I've been trying to watch through Agatha Christie's *Hercule Poirot Mysteries*, though I've gotten a bit sidetracked due to homework.

## What I'm working on right now
I'm currently working on a few projects right now. First off, I'm working on a [notes app for Windows XP](/makinganotesapp1).  

My friend and I have been DMing for a while now, and it seems like we're not the best at the whole planning business. Hopefully we'll improve.

I'm still exercising, but since I exercise in the mornings, I'm reliant on waking up early, which is not my strong suit.

My group for gl4hs presented at ASGSR, and now we're planning to submit a research paper.

Finally, I'm memorizing the book of Philippians for a bible quizzing competition.


## Chess
<div class = "section" id="elo"></div>

I've started playing chess a bit more lately, mostly as a guest on [Chess.com](https://chess.com) and on [Lichess](https://lichess.org). I used lichess's api to attach my current elo in this section of the page, since that seemed like a fun way to provide motivation to improve my chess abilities.

## Hive

I recently got into playing [Hive](https://gen42.com/product/hive), which is a cool board game played with hexagonal tiles and no actual board. I 3d printed some pieces for it and have been playing with my friends recently.


## Code
Here's my Github contribution chart so far:
<br>
<br>

<div class = "section" style = "padding-left:15%; padding-right:15%;">
<center><img style = "scale: 1.3" class = "contrib-chart" src="http://ghchart.rshah.org/readabilityLOL" alt="My github contributions chart"></center>
</div>


<!-- js goes below -->

<script>
let elo_container = document.getElementById("elo")
let wca_container = document.getElementById("wca")

//curl --location 'https://raw.githubusercontent.com/robiningelbrecht/wca-rest-api/master/api/persons.json'
async function getData() {
  const url = 'https://lichess.org/api/user/JimKram';
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const result = await response.json();
    return result
  } catch (error) {
    console.error(error.message);
  }
}

getData().then((promisedata) => {
  perfs = promisedata["perfs"]
  console.log(promisedata)
  elo_container.innerHTML = `
  Games played: ${perfs["bullet"]["games"]+perfs["blitz"]["games"]+perfs["rapid"]["games"]}
  <p>
  Ratings: Bullet: ${perfs["bullet"]["rating"]}, Blitz: ${perfs["blitz"]["rating"]}, Rapid: ${perfs["rapid"]["rating"]}
  `

  /*
  `
<div style="border-bottom: 3px solid #ffffff; margin-bottom: 1em;">Lichess</div>
<table>
  <tr>
    <th></th>
    <th>Bullet</th>
    <th>Blitz</th>
    <th>Rapid</th>
  </tr>
  <tr>
    <th>Games</th>
    <td>${perfs["bullet"]["games"]}</td>
    <td>${perfs["blitz"]["games"]}</td>
    <td>${perfs["rapid"]["games"]}</td>
  </tr>
  <tr>
    <th>Rating</th>
    <td>${perfs["bullet"]["rating"]}</td>
    <td>${perfs["blitz"]["rating"]}</td>
    <td>${perfs["rapid"]["rating"]}</td>
  </tr>
</table>
  `
*/
})

</script>


