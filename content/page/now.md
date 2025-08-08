+++
title = "What I'm doing right now"
author = "Joshua Anand"
description = "What I'm doing now"
+++

Last updated: 2025-08-08

## Who are you, anyway?

I’m Josh, a high schooler who decided last year to start a blog. I’m interested in technology, old games, and books. When I’m not wasting my time mindlessly doomscrolling on Reddit, I’m usually reading a book or trying to teach myself something, like how to make a blog or how to design a wallet in Freecad. I like making things, and I intend this blog to be both a log of things I make and a way to get more practice writing. You never know, it might even help me on a j*b applicat**n one day.

## The website
I'm going to try to update this site around once every two weeks. If I don't, just send me an angry email or something, and I'll be sure to disregard it completely.

## What I'm reading/listening to
I just finished listening to Penguin Audio's adaptation of [Reaper Man](https://bookwyrm.social/book/21745/s/reaper-man) by Terry Pratchett. I really love Penguin's adaptations of discworld and this book was no different. I'm also currently reading [The Tipping Point](https://bookwyrm.social/book/154/s/the-tipping-point) by Malcolm Gladwell for a summer assignment and [Rise and Kill First](https://bookwyrm.social/book/269459/s/rise-and-kill-first-the-secret-history-of-israels-targeted-assassinations) by Ronen Bergman because I saw a Reddit comment talking about it and figured it sounded interesting.

## What I'm watching
A friend recently coerced me into watching the Star Wars original and prequel trilogies with him, which means I can now cross those off my list of things to do.
I'm also planning to start watching my Cosmos DVDs soon.

## What I'm working on right now
I'm currently working on a few projects right now. First off, I'm working on a [notes app for Windows XP](/makinganotesapp1). My first log for that should be out soon. I'm also working on creating a website for a friend, which may have been a bad decision on his part, given my history with website design. 

I'm also currently taking Health over the summer at my school, since I figured it would take up unnecessary time during the school year. I don't really regret the decision, even though it takes up 2 hours each day, because I wasn't doing anything with those hours anyway.

I started working on my DnD campaign for school with a friend. We decided on which system we'll use, which is proabably a good first step. We've decided to use [sw5e](https://sw5e.com) as our system, because it seemed like fun, and we both have some star wars knowledge from the movies and from playing KOTOR.

I'm trying to exercise a bit more, since that's always healthy, and I'll try to update that here later.

Finally, I'm working on the Genelab for High Schooler's capstone project with a group, which is taking up a good bit of my time.

## What I'm learning
Over the summer, I've been learning a few things using online resources, and I'm going to try to ramp them up to prepare for the upcoming school year. I started working on the [Haskell course at mooc.fi](https://haskell.mooc.fi), and I've been doing a few sections every other day for the past month or so. 

I'm also preparing for my high school Calculus and Physics classses with the help of [Khan Academy](https://khanacademy.org), though I haven't really been doing these very much. I'm going to start trying to work on these for an hour each day. 

I've also been practicing German a bit, using [Clozemaster](https://clozemaster.com), and [Anki](https://ankiweb.net), though I should probably work on these a bit more intensely as school approaches. I'm also preparing for the SAT, because I'm taking it for the first time in November. I've already started doing Collegeboard's [SAT Question of the Day](https://qotd.collegeboard.org), but I feel that I can do a bit more, and I'll probably start doing Khan Academy's SAT prep course.

Finally, I'm also learning a bit of [Game Theory](https://en.wikipedia.org/wiki/Game_theory), since that's a pretty cool subject. I'm currently using William Spaniel's excellent [Game Theory 101](https://www.youtube.com/playlist?list=PLKI1h_nAkaQoDzI4xDIXzx6U2ergFmedo) course, and taking notes in [Obsidian](https://obsidian.md).

## Chess
<div id="elo"></div>

I've started playing chess a bit more lately, mostly as a guest on [Chess.com](https://chess.com) and on [Lichess](https://lichess.org). I'll used lichess's api to attach my current elo in this section of the page, since that seemed like a fun way to provide motivation to improve my chess abilities.

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


