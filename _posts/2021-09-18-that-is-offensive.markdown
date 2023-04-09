---
title: "That's so offensive!"
author: kyle
date: 2021-09-18 21:41:16 -0700
categories: [Blogging, Offense]
tags: [offense, cybersecurity]
---

## Why this post

There have been more and more "discussions" recently that have crossed my radar in the Twitter-sphere talking about OST (offensive security tools), and the morality surrounding such things. The basic crux of the argument is that if someone were to create an offensive/hacking tool and make it freely available on a platform like Github, that they are inherently making the world a less secure place due to the ease with which attackers will be able to leverage such tooling for malicious means. This is a very complex topic, and the "discussion" on Twitter does not lend itself towards making meaningful progress on the topic. Twitter provides an easy mechanism to try to distill exceptionally complex topics into witty platitudes, which inherently means that most of the points that are made completely gloss over necessary details. Then, as soon as one side makes such a statement, the opposing side can point at all of the details that were glossed over, make their own platitudes, and the cycle continues.

This post is meant to dictate my actual thoughts on the topic. I will warn you up front that I don't have a warm-fuzzy answer for the best path forward for this. However, I do believe the topic is sufficiently complex that if anyone does offer a simple answer, there is a good chance they are full of shit.

## First we're gonna talk about death

You heard me, we're going there. If we're going to address a topic that boils down to moral judgement, let's take a look at a concrete example anyone can understand. Let's start by making a statement that most people will probably agree with:

>  It is better to not be directly responsible for the death of another person.

Morbid, but sure. That sounds like a reasonable statement to make, and I imagine most people would agree with the sentiment. Let's take a look at the extreme manifestation of a situation that contradicts that statement. Someone is a psychopathic serial killer, and they kill other people in cold blood with no justification and with no motivation other than to just do it; legitimate malevolence. I imagine that most people would look at that situation and say, *"That is wrong."*, and rightfully so.

What would happen if we took a look at the extreme use case of cold blooded murder and attempted to use that as the foundation for a policy framework on how to handle any situation where someone dies? You quickly enter exceptionally dangerous territory. What about if someone died in a situation of self-defense? What if there were 2 people attempting to drive as safe as they could, but inclement weather causes a fatal crash? We don't live in a world with binary outcomes, binary motivations, and binary circumstances. If you attempt to handle exceptionally complex circumstances based on the extreme edge case, the only people that will take you seriously are those that will blindly gloss over all of the nitty-gritty details of reality.

## Good intentions gone awry

Moving on to a slightly more palatable subject, I think it's worth exploring the gap between ideal outcomes and the downfalls of human nature. Let's take a super quick look at the world of vulnerabilities in technology. If we were to distil what the best outcome would be about vulnerabilities, we could probably make a statement like:

> The world would be a safer and more secure place if any time a vulnerability in a product was discovered, the vendor was contacted with the details and they immediately fixed the vulnerability.

Yeah, sure. If that happened in reality, we *would* end up in a world that would become increasingly more secure over time, which would benefit everyone. I imagine that this is the fundamental mindset of people that actively espouse the "responsible disclosure" model. I am willing to venture that many people that discover vulnerabilities began with such good intentions as well.

However, it doesn't take much effort to find a string of vulnerability researchers with broken souls. Many that have become jaded from things like companies ignoring their critical findings, companies suing them from their research, companies silently fixing the vulns without offering any credit to the research, etc. etc. If you go through those experiences enough times, it is only natural to not feel morally obligated to freely share the fruits of your labor with a company that may feel inclined to persecute you for your efforts.

> "But didn't we just agree above that the world would be a better place if everyone did responsible disclosure?!"

Yeah, but responsible disclosure only works when everyone involved acts with the best of intentions. Maybe that periodically happens, but it is not always the case. To suggest that each vuln researcher must abide by such standards without equally stringent expectations of each and every company that they interact with is just naive. We live in a messy world, and Santa isn't real. Sorry to break it to you.

## OST - the lay of the land

Let's take a step back into reality and explore what that means to offensive tooling that gets released on the wild internet. Fundamentally, I think that a sentiment that most people can agree with could be:

> It is not optimal to take people with malicious intentions, and give them easy access to tools that enable them to act upon their malicious intentions; especially in a manner that could be difficult to detect or defend against.

Believe me, I have zero interest in giving an angsty-teenager or malicious person easy access to _./break-the-internet.py_. So, what are we to do about this? People post tools and code to github that could potentially enable such things, and that is a use case we need to protect against, right? Well, this also is a massively gray area, where concentrating solely on the extreme edge cases does no one real justice. What are the various shades of gray in this equation? They include items such as:

  *  *Learning offsec* - For anyone that wants to get into pentesting or red teaming, providing open access to such offensive tooling gives them the ability to learn how certain technologies work and how certain technologies can be taken advantage of. If you restrict access to such tooling, you cut off a viable learning mechanism for a pipeline of people that could grow into this field. How do we solve for this issue?  How do we propose new talent learns and hones these skills? Do we expect some kid in India that barely has access to a computer to start paying premiums for authorized offensive training for a potential career path?

  * *Open detection options* - By creating open source/free offensive tooling it also provides benefits to defenders. There are a couple of ways that this manifests itself. First, it enables them to play with tooling that some attackers on the Internet use. It enables them to understand the limitations of the tooling, what artifacts it creates and how to potentially detect it, and how it could manifest itself in their networks. If you remove open offensive tooling from the equation, what is the mechanism for defenders to test the defenses of their network? Shall blue teams start to budget for approved offensive tooling for these things?

  * *TTP funneling* - There is something to be said about understanding what kinds of tooling a defender should even prepare for. If your threat model includes the 5-Eyes Intelligence Community or the Mossad, then good luck. There are ways to defend your network from such threats, but may the deities be on your side, because you have an up-hill battle. But that's not what we're talking about here. This *entire* discussion is around putting tools into the hands of people that don't have the skills to build it themselves and all they have to do it run it maliciously. Understanding this as our threat model, it means we can more easily understand what tooling we need to defend against. _"What tooling could I easily find on Github to attack my network?"_ The types of tools that you find are probably going to be very similar to the tools attackers find, which can better inform defenders on what tools and techniques to prioritize creating detections for. _For example, the moment [subTee](https://twitter.com/subtee) tweets something like, "I just had a really good Hawaiian burger", you can bet your ass I'm immediately starting to wonder how I can detect pineapples in network traffic._

  * *An OST by any other name* - There is something to be said about who gets to define what is truly offensive in this case. For example, is *nmap* an offensive tool? It is used maliciously in some cases, absolutely. But when a sys admin uses it to check if a port is open or reachable, is that offensive as well? What about *netcat*? Because it can be used as an offensive tool, does that mean that it is an "offensive tool"? What about *powercat*? The list goes on. Now, there are some things that I think are easily definable in this category, such as a proof-of-concept for a RCE. There are few legitimate ways that this could be used (even though there is still an argument that a sysadmin could use such a proof-of-concept to confirm whether his/her infrastructure is vulnerable to attack). But there is a *very broad category* of tools that fall into muddy waters and are not easily defined. Who gets to define what is "offensive" or not?

## OST - the discussion

So what are we to do? Many of the conversations that I've seen on Twitter are basically guaranteed to fail. In my naive observations, the conversations have gone kinda like this _(yes, I am oversimplifying a bit, but below is my rough interpretation)_:

  > A: "It is bad that people are open sourcing offensive tooling, because it makes it easier for attackers to up their game with little to no effort."

  > B: "Aiding attackers is obviously not optimal, but there are a number of other details about this subject that need to be considered...(at which point they talk about any of the above bullet points).  What do you propose is actually done about this?"

  > A: "We need more regulations on people releasing OST's!"

  > B: "Okay, that sounds like it could turn unreasonable/dangerous very quickly. How do you propose this actually gets accomplished without neutering all of the benefits that have already been listed?"

  > A: "No one is able to provide a meaningful argument that we SHOULDN'T regulate OST to keep it out of the hands of attackers!"

  > B: "Whaaaa?"

So here we are. There are people that appear to be in the camp of _"we should regulate OSTs to make the world a safer place"_. Yet, to the best of my understanding, no one has actually provided concrete recommendations of what, _exactly_, should be done. I imagine that most of us would agree with the intention of the argument, but see an equally dangerous slippery slope of where "regulations" could lead. A few examples of potential slippery slopes that could be introduced would be things like:

  * What does "regulation" even mean? Do people not get to post certain kinds of code to Github anymore? Are there technical restrictions imposed to prevent people from posting certain things in the first place, or social/legal ramifications for doing it?

  * Who are the gatekeepers of these regulations? Who gets to decide what is right or wrong? What is offensive or sys admin? Who gets to decide what the ramifications are for breaking the rules?

  * Whoever these gatekeepers are for regulations around OST, what standards are _they_ held at? How is the community ensured that everyone's best interest is actually accounted for? Looking at vulnerability researchers and "responsible disclosure" you can get a glimpse of what can happen when you try to dictate the actions of the security community based off of moral decisions that only apply to them. You drive talent underground. Is that what we want?

  * Let's pretend there is an agreed upon group of gatekeepers and they even create a set of regulations that is as reasonable as possible. What do you think would happen from there? Do you think people will stop people from developing OST's in general? The answer is likely not. They will just change how they socialize and share the code that they write, and assuredly this will manifest itself in more disparate and closed venues. As you end up with more tooling that is shared in this way, will that really provide a net positive for defenders? Also, to clarify, if your fundamental position boils down to, _"people just shouldn't create hacking tools in the first place!"_, then I don't know what to say to you. People will do that. It's the itch to figure out how certain things work. It's the curiosity to poke what no one else pokes. Saying people just shouldn't do such things, you're effectively saying, _"why can't everyone be like I expect them to be"_, at which point you are already in a losing argument, and trying to create suggestions based off of such viewpoints will never stand reliably.

## And here we are

Yup, I don't know exactly what to do about this. Is there a way to make the world a safer place without sacrificing all of the benefits of our current system? Probably, but assuredly it's not gonna come from an arrogant person that pounds their fist on the table and says, *"we need arbitrary regulations to solve this, and if you disagree with me you are part of the problem!"*

One thing that I think could be a reasonable step forward could be saying something like, _"Hey, if you are creating and openly distributing an offensive tool, please publish it with a corresponding way to detect it (or, if you don't have a detection, please be open to posting a detection for your tool created by someone else)"_. I feel like that could be a reasonable expectation we could socialize within the community. Some OST developers may even be kind enough to build certain detections right into their tooling.

But who knows, maybe that isn't the next best step.  I don't have the answers myself. The one thing that I'm sure of is that we, as an Infosec community, will not make progress on this subject if we can't agree on our actual goals or have conversations with the appropriate appreciation for the complexity at hand. If you're the kind of person who wants to continue handwaving nuance while just spouting empty moral platitudes, well, you deserve the inevitable hand-wavy snark that is coming your way.
