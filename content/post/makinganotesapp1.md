+++
title = 'Making a Notes App for Windows XP: part 0'
date = 2025-08-03T17:02:33Z
draft = true
tags = ['windows xp','code']
+++

<audio id="page-sound" autoplay=True>
    <source src="/audio-effects/xp.mp3">
</audio>

<script>
  var audio = document.getElementById("page-sound");
  audio.volume = 0.1;
</script>


> **_NOTE:_**  This post is more of a project log than a tutorial on how to make a notes app for windows xp. I will try to document my progress as I go along, but its important to note that I may skip steps if I feel they are trivial, and while my code will be put on Github, I might not always state the exact logic behind specific sections of it.

## Intro
The other day, as detailed in [a previous post](/windowsxp), I revieved a windows xp machine. I had some fun playing with it, and I played a bit of DOOM and Halo. Recently, though, I wondered to myself if I could develop my own app within the limitations of Windows XP. After all, how hard could it possibly be? [A lot of people](https://archive.org/details/cd-roms) have made apps for Windows XP, after all. I actually made quite a bit of progress on the app before I decided to start documenting it, so this post is meant to display all the stuff I already got done.

Here's a first look at the app as it is right now. Note that my notes app can't precisely take notes right now, given that it can't read files, parse anything other than plain and italic text, or even edit text. I just made a minimal parser and text display.
