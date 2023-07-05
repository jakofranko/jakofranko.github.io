---
title: "Understanding Uxn: Preamble - Why would you do that???"
date: 2023-07-05 10:30am
---

_**Note**: If you'd like to jump into the meat of understanding Uxn, feel free to skip ahead to the next article. If you too have wondered why people would ever move to lower level programming languages, read on!_

Wow, it's been a while since I've written anything here! Let's spend a paragraph catching up and then jump into how and why Uxn came into existence.

I've still been coding and learning; I've been trying to learn other front-end frameworks like [Svelt](https://svelte.dev/), and [Mithril JS](https://mithril.js.org/), as well as make a couple attempts at learning the JS game engine [Phaser JS](/tags/phaser). I've been writing some small apps here and there, and have been diving deep on [Nest JS](https://nestjs.com/) professionally.

Most recently though, I decided to take another look at [Uxn](https://wiki.xxiivv.com/site/uxn.html). I've been a huge fan of Devine's work for years now. Uxn is essentially the current home for Devine's software tools. He is notorious for abandoning old projects in order to move forward to the best option for him at the time, and will often port applications from one language to another. I've followed his trek from Ruby to JavaScript, from the Electron platform to pure browser apps, and then from JavaScript to...machine code.

This move was really strange to me. [Hundred Rabbits have written extensively about their challenges living on a ship](https://100r.co/site/off_the_grid.html), but porting elegant software into (what I thought at the time was) ugly machine code that can only run in emulators seemed extreme. Not to mention it made reading through the code very difficult, and this was a task I had previously enjoyed. Devine's code is always a bit cryptic, but figuring it out was much like solving a puzzle in Myst.

So I quit trying to understand it and moved on. But they kept releasing very cool applications on this platform, and when they came out with [Yufo](https://git.sr.ht/~rabbits/yufo), I had to take another look. I didn't realize something like that was possible in Uxn!

So I took another look...and fell down the rabbit hole. After doing several tutorials, reading through the documentation, reading various articles that Devine wrote on Uxn, and staring at source code for many hours, I feel like I finally get it. I'm hoping to write an implementation of Uxn actually, and will document that process here as a resource for others who might want to write implementations of Uxn, but don't know where to start.

But first, I wanted to synthesize some of my learnings. The documentation is cryptic, but helpful once you know how Uxn works and what it's doing. The big picture is also hard to understand - what is Uxn, and more importantly, why go through all the hassle? This requires understanding Hundred Rabbit's motivations, which requires understanding their limitations as a studio due to their lifestyle, and it requires reading a lot of source code and articles.

Perhaps I can take a stab at summarizing some of these things.

## Why go from Ruby, to JavaScript, to Uxn?

First, [read this article by Devine on permacomputing](https://wiki.xxiivv.com/site/permacomputing.html). This concept is the heart of these language migrations.

The problem Hundred Rabbits is trying to solve is this: how do you build things that will last?

Whatever language you use, you will be limited by access to the binaries that run those languages, the ecosystem of those languages (package managers mostly), and hardware's ability to implement those languages.

If you're using Ruby, you'll need to use some other piece of software that can display graphics and text; Ruby is just a scripting language. This could be solved by writing web applications, and using the browser as the interface.

However, now you are bound by the version of Ruby you're using, which can become out of date and lose support if you don't keep up with version updates. You're bound by whatever packages you use to run your web applications. These too can go out of date, especially if package maintainers are not active. And you're bound by the browser, which has its own updates and dependencies. Browsers have also become quite resource intensive as the internet becomes more and more ubiquitous.

For you and me, these are problems we don't usually think about. We have access to electricity and the internet, and can keep our various projects up to date if we put in the effort. However, I'm sure we can all think of old projects we have made that don't work any more, whether it's because it was written in an old language we no longer have installed, or simply used an API that no longer exists.

For others, maintaining popular packages can become a tremendous burden, and [there are horror stories of people who snapped under the pressure](https://javascript.plainenglish.io/open-source-a-horror-story-c14caba386a8).

So the progression starts to make sense:

1. Cut out the piece that runs web applications, and write web applications in the browser native language, JavaScript. Using Electron, you can have one codebase and port to desktop, mobile, and browser with a single build step.
2. Cut out the resource intensive, external-package dependent desktop and mobile versions, and write plain JavaScript for the browser only.
3. Create a virtual machine that can't run any of these things, requires an emulator, and painstakingly convert applications to a proprietary machine language...?

Ok, step 3 still doesn't make sense. So what happened?

## Why Uxn?

Read the [software section on Hundred Rabbit's site](https://100r.co/site/off_the_grid.html#software).

Then, read [Devine's devlog on Uxn](https://wiki.xxiivv.com/site/uxn_devlog.html).

These will give you the best picture, but here's the big idea: if the thing that runs your programs is simple, small, backwards compatible, and able to be implemented on nearly any piece of hardware, then your programs become stable and long-lasting.

Uxn, then, is the solution to the resource intensive browser, or hardware-specific SDKs like iOS, Mac OS, Windows, Android, and others.

The big "Aha!" moment for me was this realization: _If I can write a program for Uxn, then **anything** that can run Uxn can run my program!_

Uxn has been implemented for [all major operating systems, for GBA, for Raspberry Pi, for Arduino, and many others](https://wiki.xxiivv.com/site/uxn_devlog.html). Since Uxn can be run on so many things, investing the time and energy it takes to learn Uxntal, the programming language for Uxn, pays many, many dividends.

This was the missing link for me in understanding why one could abandon modern programming tools for a fairly archaic and limited ecosystem.

## Next Up: **What** is Uxn?

This has gone on for quite a while now, so I'll break this into multiple parts. In my next article, I'll be writing some high-level explanations on what Uxn is, what it's not, what Varvara is, and something that I feel is lost in all the other documentation I've read: the importance of the Uxn emulator.

Till next time.
