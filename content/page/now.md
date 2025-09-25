+++
title = "What I'm doing right now"
author = "Joshua Anand"
description = "What I'm doing now"
+++

Last updated: 2025-09-25

## Who are you, anyway?

I’m Josh, a high schooler who decided last year to start a blog. I’m interested in technology, old games, and books. When I’m not wasting my time mindlessly doomscrolling on Reddit, I’m usually reading a book or trying to teach myself something, like how to make a blog or how to design a wallet in Freecad. I like making things, and I intend this blog to be both a log of things I make and a way to get more practice writing. You never know, it might even help me on a j*b applicat**n one day.

## The website
I'm going to try to update this site around once every two weeks. If I don't, just send me an angry email or something, and I'll be sure to disregard it completely.

## What I'm reading/listening to
I recently finished reading Mort, by Terry Pratchett, which was the only book of Discworld I hadn't read yet. I'm currently rereading the books in random order now each night before bed, since I like doing some light reading each night. I'm also reading Don Quixote by Miguel de Cervantes in free periods at school and theoretically reading SPQR by Mary Beard in my free time, though the amount of tasks I have to do always expands to fill that free time. I'm also reading Harry Potter and the Methods of rationality, as mentioned in [a previous post](/alittlebitofprocrastination).

## What I'm watching
I'm not really currently watching anything except Bills games and random Youtube videos.

## What I'm working on right now
I'm currently working on a few projects right now. First off, I'm working on a [notes app for Windows XP](/makinganotesapp1). My first log for that should be out soon. I'm also working on creating a website for a friend, which may have been a bad decision on his part, given my history with website design. 

I started working on my DnD campaign for school with a friend. We decided on which system we'll use, which is proabably a good first step. We've decided to use [sw5e](https://sw5e.com) as our system, because it seemed like fun, and we both have some star wars knowledge from the movies and from playing KOTOR.

I've come up with a routine for exercising, and it seems to be going well so far. Its basically just a full body workout 3x a week. I should probably start doing some cardio though. 

I finished working on the Genelab for High Schooler's capstone project with a group, and we actually managed to make it to the next round. Now we just have to write a research proposal

## What I'm learning

As with many of my plans, a lot of the things I wanted to do over the summer failed to materialize. A few of my habits did remain though, which was probably survival of the fittest or something. *Note to self: Fix my time management problems*

I've been doing Anki reviews every day for the last 36 days, which probably means I've turned it into a sustainable habit. I review around 100 cards a day, which range from Bible verses to the countries of the world. I've also, as previously mentioned, begun exercising consistently, which is nice. I try to wake up at 5:20 and get it done. 

I've been working on SAT prep, and that's going fine, apart from my apparent lack of aptitude with basic math. I keep reversing the sign on terms in equations and messing up basic arithmetic. I've got to work on that.

I've got to start working on learning Haskell again, and I'm thinking of making a few small projects just to keep my programming skills sharp (or less dull).

I've been trying to learn some note taking skills, but we'll see how that goes. I hear obsidian is good, so I'll probably consolidate my notes there.


## Chess
<div id="elo"></div>

I've started playing chess a bit more lately, mostly as a guest on [Chess.com](https://chess.com) and on [Lichess](https://lichess.org). I used lichess's api to attach my current elo in this section of the page, since that seemed like a fun way to provide motivation to improve my chess abilities.

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


