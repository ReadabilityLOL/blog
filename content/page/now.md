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

Last updated: 2026-02-14


## The website
I'm going to try to update this site around once every two weeks. If I don't, just send me an angry email or something, and I'll be sure to disregard it completely.

## What I'm reading
- Discworld by Terry Pratchett
- Stranger in a Strange Land by Robert A. Heinlein

## What I'm working on right now
- Running (trying to anyways) a Star Wars 5e campaign with a friend
- Attempting to wake up early
- Submitting a research paper
- Memorizing Philippians for a competition

## What I'm playing
- Chess
- [Hive](https://gen42.com/product/hive)
### Chess
<div class = "section" id="elo"></div>

## Code
Here's my Github contribution chart so far:

{{ partial "ghcontrib.html" }}

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


