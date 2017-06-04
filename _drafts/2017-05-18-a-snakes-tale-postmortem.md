---
title: A Snake's Tale Postmortem
layout: post
---

## What went well

**I finished a thing.**
I have a dozen or so quasi-serious game projects that I've started over the past few years but never found the stamina to finish.

**I'm _very_ happy with some of the art in the game, especially the snakes**.
I'm also happy with how they came about: rather than 3d modeling or drawing (2 skills that I haven't developed as much as I'd like) them, I wrote a bunch of code to procedurally generate them.
I ended up with a unique look, was able to do it all myself, 




**I'm glad I picked Rust for A Snake's Tale**, but [I've written plenty about that](/blog/i-made-a-game-in-rust/).

**I'm quite happy with the [trailer I made](https://www.youtube.com/watch?v=23pQmEuueNw).**
I had no clue what I was doing, and iMovie was super frustrating to use, but the end result is pretty decent.

**Yay for guest level designers.**
David Peter and Bri Fairley made a few of the levels in the game, and their levels ended up "feeling different" from the levels I made, and I really like the variety.

David's levels:

Bri's levels:

## What didn't go well

**Some of the art in the game is not the best.**
Especially the level select map.
I didn't really have a great vision for what it could look like, and I probably thought of it as much less important than the puzzle solving part of the game (probably because it is).
I also designed it with the intention that you'd be relatively zoomed in while looking at it, but I added "zoom all the way out" after a while, and that's both the easiest way to use the map and the worst looking 😐.

**The audio in the game is lackluster.**
I mute mobile games 99% of the time, I didn't think of audio as super important to the experience of the game, and 
On top of that 

**Oh my gosh developing for Android is the worst.**
Fragmentation (especially across graphics capabilities and performance) is a huge problem that caused me a ton of pain.
Finding the right common denominator to target (which ended up being below iOS in most cases) and testing across a representative set of devices was not fun.
The Android NDK is consistently painful to work with.
Android also has a bunch of weird quirks (Bug report: "This weird button that does nothing and that you certainly didn't ask for is showing up for me!"; Solution: Change a number in an XML file) and works rather differently than most other modern computing platforms.
On top of that, I don't have much personal experience using Android, which certainly hurt my ability to develop for it effectively.

**A Snake's Tale didn't make it through [Steam Greenlight](http://steamcommunity.com/sharedfiles/filedetails/?id=893373312)**
Oh well.

Good:
- Sales
- Learnin'
- Press

Bad:
- Audio

Favorite levels
- Tete
