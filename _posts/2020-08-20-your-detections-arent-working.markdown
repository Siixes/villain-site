---
title: "Your detections aren't working"
author: kyle
date: 2020-08-20 00:49:16 -0700
categories: [Blogging, Detections]
tags: [analysis, cybersecurity, detections]
---

Your detections aren't working...at least, that should be your going assumption.

Let's take a look at a potential scenario some companies may find themselves in...

>
>    * Data is coming in ${somewhere}...like a SIEM or something like that. Let's say you have EDR data (Sysmon, CarbonBlack, etc.), AV logs, server logs, network logs, etc.
>    * Alerts are being generated off of that data
>    * Those alerts make their way to the eye balls of analysts to evaluate how legitimate they are
>

Cool, sounds great. If something bad happens, it should generate an alert, that'll get in front of an analyst. If it's truly bad, they should be able to figure that out and respond appropriately. Remember, our goal as defenders isn't to create detections to catch every single thing an attacker does; it is to catch *something* an attacker does that leads us down the trail of breadcrumbs to find *everything* they did and kick them out as soon as possible. (a good article on this was written by @thegrugq [here](https://www.recordedfuture.com/cyber-operations-time/) )

That's a wonderful fairy tale that probably isn't as close to reality as many of us hope.  Why is that? First, let's discuss where detections come from.

### Rough Types of Detections

Where do our detections come from? You see, when a mommy detection and a daddy detection love each other very much...

Oh, sorry, wrong blog...

There are probably a couple ways you get detections in your environment. There are at least a couple more posts I want to write on detections, so I'll try to contain myself here. But let's look at how detections will probably manifest in your environment:

>
>    * Vendor created - An example of this may be using some anti-virus software. If someone downloads this_totally_isnt_a_bitcoin_miner.exe, AV may catch that and create an alert saying it caught it.  This could also be an EDR tool running on your endpoints that comes pre-configured to create an alert if a process does something it is told to look out for.   What gets detected here is purely based on the logic of the tool that is evaluating the data. You may or may not be able to figure out how valid/trustworthy this detection is.
>    * Threat intel - I have some strong opinions about threat intel, but the world doesn't revolve around me, so it's still part of the *cYb0r-3c0noMy*. Companies pay for feeds of threat intel that they then try to match against the logs they have coming into their SIEM. If something in your logs matches something from a threat intel feed, that may generate an alert. With this as well, there is an exceptionally high chance they will have no idea what logic/data was used to generate that alert.
>    * Custom alerts - These are the ones you/your team create yourself. These are the ones where you're like, "Hey, if you see Bob from HR accessing our sensitive source code repo, alert on that". This is also the custom detections you may write against your EDR data, a very common example would be looking for whenever `cmd.exe` or `powershell.exe` are spawned from `msword.exe` (as this would be a very strong indicator someone got phished). Depending on how well you play with others, you may develop some friends that will share the custom detections they wrote with you.
>

Cool, now we're sitting pretty with these detections rolling in. They show up in the system, and an analyst responds to them. Life is good...or is it?

### Story time with Cthulhu

Quite a while ago a handful of friends from different companies were chatting about detections they use. We each shared some methodologies and detections with each other (rising tide raises all boats and all that).

*"Hell yeah!"* was my general take away from that, *"We'll be able to detect so many moar things!"* (I was so naive in my youth...those were the days...)

 I slept even easier knowing that if shit were to pop off, there was a great chance we'd catch it! Fast forward some time, and I was going to test out some attacker tool/technique in our environment. *"This will create an alert, but I want to see what an attacker could do with this on our network anyway"*, I thought. Well, I ran the tool and **...drumroll...** no detection fired.

 Going back and looking through the (rather complex) logic of the detection that *should* have fired, it turns out the logic got fucked up, and there was almost no circumstance in which it would **ever** create an alert.

 Yeah, my world-view was shattered. It raised the question...all that time that some detections weren't firing, was that because there was no malicious activity happening? or because the detections weren't working the way we thought they were?

### Knee-jerk reactions aren't known for their subtlety

So, what do you do now? Luckily, the methodology to fix something like this is already a solved problem in the software world (*I'm looking at YOU, [unit testing](https://en.wikipedia.org/wiki/Unit_testing)*). The concept of unit testing is pretty simple; there are tests created to ensure that bits of code work as anticipated. Sadly, I haven't seen or heard this applied much in the context of detection and response, even though I think we absolutely should.

What would this look like? Well, here is how I've seen it implemented before:

>
>    * You have one (or more) test machines/VMs in your network
>    * It is imperative these machines have the exact same policies/tools applied to them that every other machine does
>    * Those machines feed their logs to a system that uses the exact same detections as what you have running in prod
>    * You run an attack that you think you will detect
>    * You observe if a detection actually fires
>    * You reset the machine and start over
>

To do this in a way that is continuous and scales it will require you to have a corpus of "attacks" you can run in a controlled environment in your network. There are products out there that offer suites of threat emulation that could be built into a pipeline like this. But even if you can't get your hands on something like that, you could still run most of these manually.

The beauty of doing these things continuously and building a pipeline for this is you could get immediate feedback and notice changes over time. For example, did IT make a change to group policy last week that took an attack that didn't used to work in your environment, and now it will work?  Having a system like this will be able to give you that feedback.

### I have my goals but the Universe is playing goal-line defense...

What are your real goals from doing something like this? There are a couple of them:

>
>    1. Identify detections that aren't detecting activity you think they should be. For example, let's say you have a detection looking for psexec; if you run psexec and that detection doesn't fire, it is time to go back to the drawing board on that one.
>    2. Provide a feedback loop to improve detections that don't work very well/are too niche. Let's say you have a detection for Mimikatz, and that detection is looking for any process run from **C:\Windows\Syystem32\Mimikatz.exe**. *"Awesome, now I can sleep well knowing that we'll detect Mimikatz in our network..."*, you may be thinking to yourself.  One question you should **always** be asking yourself is, **"Under what circumstances would an attacker do this thing, but my detection not catch it?"** For this example, what if the attacker runs Mimikatz from a temp or downloads folder? What if "Mimikatz.exe" is renamed to "Mimidogz.exe"? Or "update.exe"? Attackers can be crafty, and just renaming an attack tool happens relatively frequently. If you slightly alter one of these attacks on your test box, would your detection still work?
>    3. You may find yourself in a situation where you are running certain attacks on your test box and realize that you don't even have the right logs to be able to identify the activity/build detections off of. This is a fantastic and concrete data point that you can raise with business stakeholders to try to get buy-in for that kind of visibility!
>    4. It's not always feasible to build detections for all permutations of how an attacker may manifest themselves in your environment. But over time you will create better detections and you will have to get far more creative to pull off attacks in a way to evade them. That will have raised the bar high enough that attackers will have to expend more and more effort to do the same attacks, which is part of the point of all of this.
>

### Outtro

Trust me, the idea of a big CI/CD system for unit testing your detections may sound daunting and unrealistic for most people. But consider it something like that a north star; most people should be able to coordinate doing some manual testing of attacks in their network to ensure you're able to see and detect some of the low hanging fruit you may expect to see from attackers.

Don't become complacent in the concept of *"I don't have to worry, we have a detection for that..."* if you haven't actively seen that detection fire on what you expect. Too many people (myself included), have made that mistake.

There's at least a couple more blogs in the pipeline about detection methodologies, but I'm happy to respond to any questions/comments/concerns. Go make good shit happen today!
