+++
title = "What I'm doing right now"
author = "Joshua Anand"
description = "What I'm doing now"
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
I recently finished reading Mort, by Terry Pratchett, which was the only book of Discworld I hadn't read yet. I'm currently rereading the books in random order now each night before bed, since I like doing some light reading each night. I'm also reading Don Quixote by Miguel de Cervantes in free periods at school and theoretically reading SPQR by Mary Beard in my free time, though the amount of tasks I have to do always expands to fill that free time, as per [Parkinson's Law](https://wikipedia.org/wiki/Parkinson's_law). I'm also reading Harry Potter and the Methods of rationality, as mentioned in [a previous post](/alittlebitofprocrastination).

## What I'm watching
I'm not really currently watching anything except Bills games and random Youtube videos.

## What I'm working on right now
I'm currently working on a few projects right now. First off, I'm working on a [notes app for Windows XP](/makinganotesapp1).  

My friend and I have DMed a couple sessions now, and it seems to be going well with Star Wars 5e. The main issue I think is that we haven't read the rulebook yet.

I'm still exercising, but since I exercise in the mornings, I'm reliant on waking up early, which is not my strong suit.

My group for gl4hs managed to make it past the research writing phase! Now we're going to present at ASGSR on December 6th.

I published my notes online, but it didn't help my quality of notes at all.

Finally, I'm memorizing the book of Ephesians for a bible quizzing competition.

## What I'm learning

I've neglected my anki cards for a few days now, and they've piled up to around 300. I should do them soon.

I took the SAT for the first time last month, and I managed to get a 1510, praise God. I'm going to take it again, though. 1600 or bust!


## Chess
<div id="elo"></div>

I've started playing chess a bit more lately, mostly as a guest on [Chess.com](https://chess.com) and on [Lichess](https://lichess.org). I used lichess's api to attach my current elo in this section of the page, since that seemed like a fun way to provide motivation to improve my chess abilities.

## Hive

I recently got into playing [Hive](https://gen42.com/product/hive), which is a cool board game played with hexagonal tiles and no actual board. I 3d printed some pieces for it and have been playing with my friends recently.


## Code
Here's my Github contribution chart so far:
<br>
<br>

<center><img style = "scale: 1.3" class = "contrib-chart" src="http://ghchart.rshah.org/readabilityLOL" alt="My github contributions chart"></center>


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


