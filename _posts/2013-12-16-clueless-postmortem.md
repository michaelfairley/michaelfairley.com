---
title: Clueless Postmortem - Ludum Dare 28
layout: post
---

I participated in [Ludum Dare](http://www.ludumdare.com/compo/) this past weekend, and [Clueless](http://clueless.m12y.com/) is what I have to show for it. (I'm simultaneously proud and embarrassed of what lives behind that link.)

A brief postmortem:

### Things that went well:

#### Tooling
I ended up super happy with all of my tooling choices: libgdx, Box2D, and Tiled among others. Everything more or less work as expected and was easy enough to learn (though I spent too much time learning on the spot), and ending up with an HTML/JS deliverable was nice. I also had a blast working with Java in IntelliJ all weekend.

#### Friends
Getting together with coworkers to hack near each other made the weekend much more enjoyable. The mutual energy kept us all focused on our games, but ping-pong and board game breaks provided much needed relaxation after hours of heads-down coding. Having people to bounce ideas off of also proved helpful when I got in a rut. And most importantly, I'll always chose a shared experience over one I have alone.

#### Shipping
I almost called it quits mid-Sunday, but I'm glad I followed through and ended up with something shippable. I feel like I'm in a really good place for future LDs.

### Things that went less than well:

#### Nailing down the idea too late
I didn't have a clear idea of what my game was going to be until ~5pm on Saturday, leaving me with not a ton of time to build and polish it.

#### Graphics
Artistic skills are not something I've spent much time developing. And it shows.

#### Movement
The guest's movement is my least favorite thing about Clueless. It looks awkward, and it leads to games where there are no killings for at least a minute. The worst part is that I knew how to make it better, but I ran out of time. Maybe I'll fix this after voting is over and release v1.1.

#### Box2D overkill
My final product ended up not really needing the sophisticated physics engine I spent time learning and building against. Some of the intermediate versions of the project leaned on it pretty heavily, but by the end I could've gotten away with simple AABB collision detection.
