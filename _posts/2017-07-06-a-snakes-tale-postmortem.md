---
title: A Snake's Tale Postmortem
layout: post
---

Wooooo A Snake's Tale is now out on [Steam](http://store.steampowered.com/app/654810/A_Snakes_Tale/)!
And you can still get it on [iOS](https://itunes.apple.com/us/app/a-snakes-tale/id1211845149?mt=8&at=1001lnX5) or [Android](https://play.google.com/store/apps/details?id=com.m12y.asnakestale).

In celebration of the Steam launch, I wrote some things about how I feel about the game.

## What went well

**I finished a thing.**
I have a dozen or so quasi-serious game projects that I've started over the past few years but never found the stamina to finish.
It feels really, really nice to have now seen something through from beginning to end.

**I'm _very_ happy with some of the art in the game, especially the snakes**.
I'm also happy with how they came about: rather than 3d modeling or drawing (2 skills that I haven't developed as much as I'd like) them, I wrote a bunch of code to procedurally generate them.
I ended up with a unique look and was able to do it all myself.

**I'm incredibly pleased with how accessible A Snake's Tale is**.
It requires basically no games literacy, twitch movements, or large time commitments.
I've seen 5 years old pick it up and figure it out in just a few seconds, yet it's also not without depth and challenge.

**I'm glad I picked Rust for A Snake's Tale**, and [I've written plenty about that elsewhere](/blog/i-made-a-game-in-rust/).

**I'm quite happy with the [trailer I made](https://www.youtube.com/watch?v=23pQ).**
I had no clue what I was doing, and iMovie was super frustrating to use, but I think the end result is pretty decent.

**I learned a ton about how to make games**.
I learned even more about how _not_ to make games.  
I'm hard at on game #2, which you can expect to be bigger and better now that I have some clue what I'm doing.

**Sales have exceeded my expectations**, mostly due to me having incredibly low expectations.

**Yay for guest level designers.**
David Peter and Bri Fairley made a few of the levels in the game, and their levels ended up "feeling different" from the levels I made, and I really like the variety.

### David's levels:
#### Gauntlet
![Gauntlet](/images/snakes/gauntlet.png)
#### Chronicler
![Chronicler](/images/snakes/chronicler.png)

### Bri's levels:
#### Linchpin
![Linchpin](/images/snakes/linchpin.png)
#### Temptation
![Temptation](/images/snakes/temptation.png)
#### Brevil
![Brevil](/images/snakes/brevil.png)
#### Danger Noodle
![Danger Noodle](/images/snakes/danger-noodle.png)
#### Water Landing
![Water Landing](/images/snakes/water-landing.png)
#### Nice
![Nice](/images/snakes/nice.png)
#### Elbow Room
![Elbow Room](/images/snakes/elbow-room.png)

## What didn't go well

**Some of the art in the game is not the best.**
Especially the level select map.  
I didn't really have a great vision for what it could look like, and I probably thought of it as much less important than the puzzle solving part of the game.
I also designed it with the intention that you'd be relatively zoomed in while looking at it, but I added "zoom all the way out" after a while, and that's both the easiest way to use the map and the worst looking 😐.

**The audio in the game is also pretty lackluster.**
I mute mobile games 99% of the time, I didn't think of audio as super important to the experience of A Snake's Tale, so it didn't get a ton of time dedicated to it.
On top of that, I underestimated how hard it is to do audio well.

**I waited too long to start getting feedback**.
I didn't really have a fully playable build of the game until pretty late in its development, but as soon as I did, watching others play it provided me with tons of information on how to improve the game.

**Oh my gosh developing for Android is the worst.**  
Fragmentation (especially across graphics capabilities and performance) is a huge problem that caused me a ton of pain.  
Finding the right common denominator to target (which ended up being below iOS in most cases) and testing across a representative set of devices was not fun.  
The Android NDK is consistently painful to work with.  
Android also has a bunch of weird quirks (Bug report: "This weird button that does nothing and that you certainly didn't ask for is showing up for me!"; Solution: Change a number in an XML file), and it works rather differently than most other modern computing platforms.  
On top of that, I don't have much personal experience using Android, which certainly hurt my ability to develop for it effectively.  

**A Snake's Tale didn't make it through [Steam Greenlight](http://steamcommunity.com/sharedfiles/filedetails/?id=893373312) in time for a simultaneous launch with the mobile versions**.
I goofed up a few things with the Greenlight campaign which probably slowed down its success.
For better and for worse, Greenlight has been retired, and the things I've learned from this aren't even applicable anymore.

**Naming levels was very difficult**.
I'm pretty happy with at least half of the names, but the other half aren't quite as good.

**The ending to the game is rather anticlimactic**.
Yeah... I had no clue what to do, but maybe even just popping open the credits automatically would've been better than ~nothing.

## Finally, A few of my favorite levels
With a small bit of commentary

#### Tête-à-Tête
![Tête-à-Tête](/images/snakes/tete-a-tete.png)
In the level editor for A Snake's Tale, I have a button that tells me whether or not a level is solvable.
I managed to convince myself that that button had a bug, because it kept telling me this level can be solved, but I just _could not_ figure out how.
It turns out I overlooked one of the most interesting things about 2-headed snakes!

#### Psych!
![Psych!](/images/snakes/psych.png)
This was another level where I managed to surprise myself (with the gate closing as I tried to move onto it).

#### Between a Rock
![Between a Rock](/images/snakes/between-a-rock.png)
This level was only _okay_ for a while, but I added one more rock near the end of development that has made it into one of the hardest levels.  
(I found that whenever I could increase difficult by _shrinking_ the state space, something interesting was going on.)

#### Ouroboros
![Ouroboros](/images/snakes/ouroboros.png)
This level is the best joke I could tell with the "language" of the game's rules.  
It's also satisfying to watching the snake's long descent into the hole.

#### Half of Eight
![Half of Eight](/images/snakes/half-of-eight.png)
I really like how this level looks (simple, with lots of non-perfect symmetry), _and_ its solution has an interesting idea in it.

#### Heavy
![Heavy](/images/snakes/heavy.png)
When I realized that an egg could start on a tile with a button, I knew I had to make a level about that possibility.
