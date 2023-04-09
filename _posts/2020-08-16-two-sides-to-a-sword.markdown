---
title: "Two sides to a blade"
author: kyle
date: 2020-08-16 00:49:16 -0700
categories: [Blogging, Detections]
tags: [analysis, cybersecurity, detections]
---

<img src="/assets/img/sharpen.jfif" alt="Obligatory blade sharpening image">

If you look at the make-up of a blade, it consists of a piece of metal where two opposing sides of it are carefully filed to specific angles to create an edge at a specific degree. If the two sides of the metal are grinded/filed at specific degrees, they meet in the middle and can create an exceptionally sharp edge.

To me, this is a fair representation of our skills as InfoSec professionals from the offensive and defensive perspectives.

>    I believe that to be truly effective as an InfoSec professional, you need to hone both your offensive AND defensive skills.

I mean, like, duh. This conversation has been played out ad nauseum. *"Think like an attacker"* is the catch-phrase for all of that offensive training that permeates the InfoSec world. The concept is that, as a defender, if you learn how to "hack things" you'll learn how to defend those things. There is also the fantastic article about how [Defenders think in lists, attackers think in graphs](https://github.com/JohnLaTwC/Shared/blob/master/Defenders%20think%20in%20lists.%20Attackers%20think%20in%20graphs.%20As%20long%20as%20this%20is%20true%2C%20attackers%20win.md). The goal of this is to take the attacker mindset and implant that upon the defenders to better equip them with what to expect and how to operate.

One thing that I think is an interesting omission in this conversation is what attackers can learn from defenders. For example, as an attacker, if you are intimately familiar with the biases of defenders, you can better prepare your tactics to take advantage of that. For example, looking at the netflow example in this [analysis post](https://criminal.group/infosec/analysis/2020/08/13/the-art-of-analysis.html), the natural inclination of many analysts would be to view that as SSH until proven otherwise. If you just have some version of netflow it can be hard to determine what protocol is *actually* being used on a port, so it's easy to go with that assumption.

An analyst worth their salt may then want to check if SSH is actually open. In the example we'll say the machine with SSH open is in their network, so they may have the access to determine if it a running service on that machine. Lo and behold, we'll say SSH isn't running. That's odd, could that imply that port 22126 is open on the other machine for this connection?  Let's say you use nmap/masscan/shodan to scan that port on that IP, and it's closed. Then what? As a defender, it can be difficult to determine where to go from there.

Without getting into the details at the moment, you could customize your C2 and malware to accommodate for these defender biases and thought processes to make your implants **exceptionally** harder to find.

As an industry we have very much normalized defenders learning the attackers' tricks of the trade; but if attackers truly understood the biases and presuppositions of defenders, they could find very interesting ways to up their game as well.

**But that's the entire point, you have to sharpen both sides of a blade for it to be truly effective.**
