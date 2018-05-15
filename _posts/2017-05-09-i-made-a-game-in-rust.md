---
title: I Made a Game in Rust
layout: post
---

Last week, I released [A Snake's Tale](https://m12y.com/a-snakes-tale/) on [iOS](https://itunes.apple.com/us/app/a-snakes-tale/id1211845149?mt=8&at=1001lnX5), [Android](https://play.google.com/store/apps/details?id=com.m12y.asnakestale), [Windows, Mac, and Linux](https://store.steampowered.com/app/654810/A_Snakes_Tale/).

A Snake's Tale was programmed entirely in [Rust](https://www.rust-lang.org/), and to the best of my knowledge, it is the first commercial game made in Rust to be released.
It's not the largest or most technically demanding of games, but I'm proud to have an existence proof for the viability of gamedev in Rust.

Here's a brain dump of my thoughts and feelings about how that went and where I think Rust currently stands for game development.

TL;DR: I'm glad I picked Rust for A Snake's Tale, and I will be continuing to make my games in Rust for the foreseeable future[^1]; however, I can't strongly recommend Rust for gamedev in the near-term, especially if you're hoping for a batteries included experience.


### Some background

I've written a number of small, hobby games in the past, but A Snake's Tale is my first commercial release.
It consists of **~7700 lines of Rust** (and ~500 lines of GLSL), written entirely by me over the course of 10 months (during the last 2 of which it received my full time attention). It is also not my first take on writing a game in Rust, but the previous attempts have never seen the light of day.

The libraries it uses are
- [sdl2](https://github.com/AngryLawyer/rust-sdl2) (and consequently the SDL2 C library) for windowing, input, audio
- [cgmath](https://github.com/brendanzab/cgmath) for vector and matrix math
- [rusttype](https://github.com/dylanede/rusttype) for font rendering
- [gl_generator](https://github.com/brendanzab/gl-rs/tree/master/gl_generator) for loading OpenGL functions
- [png](https://github.com/PistonDevelopers/image-png) for, uh, loading pngs

(That list might seem short or might be missing your favorite crate; more about that later.)

&nbsp;

Okay, enough boring facts. On to the promised thoughts and feelings!

### Overall, **Rust is a _fantastic_ language**

Beyond the "big ideas" contained in Rust (e.g. the ownership model), **the "small ideas" that most other systems-y language don't contain** (`Option`, `Result`, `Iterator`, discriminated unions, pattern matching, etc. etc. etc.) **make minute-to-minute programming much nicer**.

**Holy cow are the compiler error messages generally fantastic.**
`rustc` seems like a friend who's trying to help you write better code, rather than a gatekeeper working to slow you down.

**Rather than being locked into one paradigm, I found myself being able to cleanly use different programming styles as warranted in different situations.**[^2]
Certain sections of my code worked best in a functional style and made heavy use of pure functions and `Iterator`; other sections were mostly cleanly expressed with imperative code, and a `for` loop and mutable collections did the trick. 
Sometimes "objects" (i.e. structs with no `pub` fields, with everything hidden behind a constructor, encapsulated methods, and `drop`) are the best way to handle certain pieces of data; other times, dumb data bag structs with all of it fields marked `pub` and almost no `impl` worked best.
None of these styles felt "second class", and I felt trusted as a programmer to make the right decisions about how to structure my code.
Ultimately, the game logic heavy parts of my code look about 90% as nice as they would in e.g. Ruby, and the performance critical, more optimized sections of my code look about 90% as nice as they would in C.

**Rust is not easy to learn.**
I've found that Rust requires me to think differently than when programming in other languages, and it took months to grow into that new mindset.
Learning to tango with the borrow checker is not a one-weekend kind of activity; in fact, I had two or three false-start attempts at learning Rust where I'd spend a few hours with it, not get anything done, and put it down until a few months later.


**Stable upgrades have been a breeze, and the release train has been very nice.**
8 new versions of Rust were released during the development of A Snake's Tale, and each upgrade took no more than a few minutes.
The release train has also consistently delivered much-welcomed improvements.

### Building and running Rust code is incredibly easy

**Cargo is a solid package manager and build tool.**
- Adding a new library dependency: a one line addition to `Cargo.toml`
- Forking a dependency: hit "fork" in GitHub; tweak forked repo; add `git` attribute to dependency in Cargo.toml
- Building someone else's Rust project: 99% chance all you need to do is run `cargo build`

**Rust code being platform independent by default is 😍😍😍.**
There are no source level differences between my Mac, Windows, and Linux builds[^3], and the differences between mobile and desktop are relatively small and focused mostly around the UI delta between touchscreen and mouse+keyboard and OpenGL vs OpenGL ES.

I'm a huge fan of vendoring dependencies in applications, and **[`cargo-vendor`](https://github.com/alexcrichton/cargo-vendor) worked well for that task**.
It's not quite up to par with `bundler`'s vendoring (upgrading dependencies require a slightly awkward dance of commenting `cargo-vendor`'s config out, running `cargo update`, deleting the outdated vendored files, rerunning `cargo vendor`, and then restoring `cargo-vendor`'s config), but it gets the job done.

**[`build.rs`](http://doc.crates.io/build-script.html) handles custom build tasks pretty well.**
It took me a little while to figure out exactly what should go in `build.rs` vs in my `Makefile`, but I eventually landed on `build.rs` containing the bare minimum to make `cargo run` work, and no more.

**Compile times with Rust are Not Great.**
This is easily my single biggest gripe about Rust right now.
The build for A Snake's Tale takes 15+ seconds, which makes iterating rather tedious.
The current incremental compiler work also doesn't seem to make the build for A Snake's Tale's codebase any faster.
(The code hotloading system I've built for my next game seems to have the side benefit of reducing compile times down to sub-second, so I'm optimistic that a lot of my pain here can be avoided going forward.)

**The [`include_*` macros](https://doc.rust-lang.org/std/macro.include_bytes.html) are great for "packaging".**
Being able to compile small assets directly into the binary and eschewing run time file loading is fantastic for a small game.

**`cfg` is much nicer than `#ifdef` for managing conditionally compiled code.**
(It turns out that first class support for something is better than a processor layer glued onto the top.)
I do have a few gripes about `cfg`, ~~the biggest of which is that there's no way to derive custom configurations (I would love to be able to replace all my `#[cfg(any(target_os = "ios", target_os = "android", target_os = "emscripten"))]`s with something like `#[cfg(opengles)]`.)~~ [It turns out this is possible!](https://www.reddit.com/r/rust/comments/6a67py/i_made_a_game_in_rust/dhc0c23/).
It was also frustrating to occasionally break the build on platforms other than the one I was primarily developing on but not have a way to determine that without doing a full build on every target platform. (It seems like the forthcoming [portability lint](https://github.com/rust-lang/rfcs/pull/1868) will solve some or all of this pain.)

**Calling (and being called by) C code is straightforward**.
Which is very nice, considering the existing body of mature C libraries worth integrating with and the unavoidable platform specific APIs that need to be used.

However, **passing pointers across FFI boundaries is currently a little scary.**
Structs that are not marked `repr(C)` will currently work with FFI, but are very much not guaranteed to over the long term as their layout is subject to change (see [Austin Hick's recent work on layout optimization](http://camlorn.net/posts/April%202017/rust-struct-field-reordering.html)).
`improper_ctypes` is an attempt at catching this issue, but in practice, most of the FFI APIs I use require a cast to `void*` which sidesteps the warning.
It would be nice if there was a way to indicate in FFI signatures that a given pointer will (e.g. writing to an OpenGL buffer) or won't (e.g. passing a callback parameter) be read by C code, and `improper_ctypes` would apply to all of the "wills", regardless of nearby casting.

**It was difficult to stay off of nightly Rust.**
Plenty of libraries and compiler features require nightly, and I had to pass some of these up lest I get stuck on nightly.
I really didn't want to ship the game on nightly, as I want to be able to come back in months or years and still have it easily compile.

### Game development in Rust is still early in its growing stages

**I found that very few libraries I investigated were usable in a production quality, multi-platform game.**
This is not incredibly surprising in a relatively young language, especially in a domain where most people involved are currently working as hobbyists.
Most libraries I looked into got rejected for at least one of these reasons:
- Didn't support a platform I was targeting (typically iOS or Android)
- Had an API that I would be interfacing with extensively yet didn't love
- Had an API that seemed to be changing rapidly
- Had a serious bug that I encountered within minutes of attempting to use the library
- Seemed under-tested relative to the complexity/importance of the library

This is not to say any of those libraries are "bad", just that I was not willing to bet on them for the success of my project.
(I also suffer from [NIH](https://en.wikipedia.org/wiki/Not_invented_here) syndrome and am all too happy to write my own special-purpose code, which definitely contributes to how picky I am in adding dependencies.)

**Of the libraries I directly depend on, none have reached version 1.0**.
(Though, a few of them have been around long enough and with enough stability that they probably could be relabeled as 1.0 right now).
1.0 is a somewhat arbitrary thing, but this definitely demonstrates the current state of the Rust game library ecosystem.

**There are more game engines written in Rust than there are games[^4]**, and none of them are mature enough to actually produce games with.
Engine tech can be much more fun and straightforward to program than a proper game, so it's understandable that folks working in their free time are drawn to it.
However, I'm a strong believer in the [Write Games, Not Engines](https://geometrian.com/programming/tutorials/write-games-not-engines/) philosophy, and I think that "writing an engine" is a very dangerous task to get sucked into if you're interested in making games.

**When someone asks you what you made the game with and you say "Rust", you get just the blankest stare.**

It's fantastic that `rustup` and `cargo` made compiling for a whole slew of targets (10 in total) incredibly easy, but **I was definitely in uncharted territory with regards to actually packaging up a Rust application to be distributed** on all of them.
With the bulk of existing Rust applications being targeted at servers or the command-line, tasks like "Add an icon to my .exe" don't have ready-made solutions.
iOS and Android were particularly tough, as almost all existing literature on getting Rust onto iOS or Android assume you're building a library (rather than an application).
My current solutions to packaging are very, very duct-tape-y, but I hope to clean them up and make them publicly available at some point.
([I wrote up a few small details about my current solutions for a friend](https://gist.github.com/michaelfairley/d86137085307d2e5c16ee0df2fb84cd9), and if you're actively attempting to package a game up for distribution and are running into difficulties, feel free to [email me](mailto:michael@m12y.com) and I'd be happy to share whatever other tips I can.)

### A handful of somewhat gamedev specific gripes

Fortunately, there seems to be progress on solutions to many of these.

**Floats implementing `PartialOrd` but not `Ord` is maddening.**
(Or rather, the `*_by_key` functions operating on `Ord` rather than `PartialOrd` is what's frustrating.)
Yes, this is _technically_ correct, but it's also hugely inconvenient to not be able to `min_by_key` on a collection of `f32`s that are definitely not NaN.
I've seen a handful of workarounds for this, but none of them have been my favorite.
`f32`s are incredibly, incredibly common in games, and it's frustrating that tasks like "find the point in set S that's closest to point P" are not as straightforward as they could be.

**Some more advanced optimizations (mostly related to wanting more direct control of memory) that I would have like to have done are extremely hard or impossible to do on stable Rust[^5].**
Some combination of placement new, custom dynamically sized types, and me knowing Rust better would have made these possible.

**I've spent more than a few minutes working around the current lack of [non-lexical lifetimes](https://github.com/rust-lang/rust-roadmap/issues/16).**

**More fine grained control over compiler optimizations would be super helpful.**
I'd love to be able to have my hottest methods get a bit of optimization even during debug builds.
I'd also like to be able to have my dependencies be compiled with optimizations during debug builds of the application.

**First class code hot-loading support would be a huge boon for game developers.**
The majority of game code is not particularly amenable to automated testing, and lots of iteration is done by playing the game itself to observe changes.
I've got something hacked up with dylib reloading, but it requires plenty of per-project boilerplate and some additional shenanigans to disable it in production builds.

### Wrap up

To reiterate, Rust is completely usable for making games _right now_, but it's certainly not without its tradeoffs compared to more established languages and tooling.
I think gamedev in Rust has a bright future, and I am personally betting on it for the time being.

If you're interested in making games in Rust, come hang out on [/r/rust_gamedev](https://www.reddit.com/r/rust_gamedev/) and in #rust-gamedev on irc.mozilla.org.

You can also [follow me on Twitter](https://twitter.com/michaelfairley) if you're interested in keeping up with my ramblings about game development.

Oh, and please consider buying a copy of [A Snake's Tale](https://m12y.com/a-snakes-tale/).

***

##### Footnotes

[^1]: I will be giving [Jai](https://github.com/BSVino/JaiPrimer/blob/master/JaiPrimer.md) a fair shake once it's generally available.
[^2]: This flexibility can definitely be a downside in a team-based setting, as every team member needs to understand every possible permutation of code style. For me working alone, the option to code in different styles is a clear win.
[^3]: Okay, I lied, there's one difference: Mac and Linux request a Core OpenGL context, whereas I use a Compatibility context on Windows in attempt to avoid [this issue](https://www.gaslampgames.com/2016/07/20/technical-director-vining-versus-the-haxx-of-planet-playsteevee/).
[^4]: And let's not even think about how many ECS frameworks there are.
[^5]: Well, doing the optimizations themselves would've been possible, but presenting a clear, safe API on top of them would not have been.
