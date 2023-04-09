---
title: "Atomic vs Composite Detections - Round 1 - FIGHT!"
author: kyle
date: 2020-08-24 00:49:16 -0700
categories: [Blogging, Detections]
tags: [analysis, cybersecurity, detections]
---

### Non-Original Intro

There are a couple different ways that people can work with detections. This is a follow-on article to my other [post on detections](https://criminal.group/infosec/2020/08/20/your-detections-arent-working.html). There are different ways that you can build and use detections, but it's important to be cognizant of what they are and when to use them.

> Detections are like tools in your work shop, there are different kinds, and some are more appropriate for certain jobs than others.

In this article, we'll be exploring a couple types of detections - atomic and composite. Full disclosure, these are just the terms I've become accustomed to amongst my circle of friends when we talk about these things. If there are *"industry accepted vernacular that represent these concepts in far superior way to best reflect the intellectual elegance of what is being discussed"*, then I don't know it. :)

"Atomic" and "composite" is what you're stuck with now from me.

### Lay the groundwork

One of the primary goals of a SOC/blue team/etc. is to catch attackers as soon as possible. The idea is that the sooner you catch them, the sooner you are able to figure out exactly what they've done, and respond appropriately and kick them out. The concept of this is sounds pretty simple on the surface:

> Figure out what things an attacker is likely to do, then detect that stuff.

I know, this sounds so dreadfully simple, but bear with me, because this concept really matters in how you approach detections.

### Atomic Detections

The concept of an atomic detection is pretty simple, you create a detection that relies on a single event that matches whatever criteria you are looking for. Below are some examples of what that may look like:

>
>  * A computer makes a network connection to a known-bad domain that hosts malware
>  * Anti-Virus identifies a known malicious file on a host
>  * Something like msword process spawns cmd.exe
>

There are a plethora of other examples, but these highlight the concept of an atomic detection. It basically boils down to an event that, if it were to happen in isolation, its mere presence alone warrants showing up in front of a SOC analyst's eyes.

### Composite Detections

The idea of a composite detection is that it will only fire if it sees a culmination of pre-defined events. As noted in the [article I wrote on analysis](https://criminal.group/infosec/analysis/2020/08/13/the-art-of-analysis.html), one form of analysis is not only making an inference off of observing a single data point, but creating a more complex inference based off of observing multiple data points. An example of this showed up in the example I used [talking about biases](https://criminal.group/infosec/2020/08/21/if-biases-were-currency.html) on account brute forcing. Seeing a single failed logon attempt by itself may not warrant an investigation, but seeing 1,000 (or another arbitrary number) of failed logon attempts within a small time frame would warrant a detection to fire for an analyst to investigate.

### Exemplar Example of Exampleness

Let's look at an example that has worked before. But before looking at the example detection, let's briefly look at the mindset behind it.

>  We want to detect attackers as soon as possible. If our detections don't catch them getting a foothold in our network, what do we think they would do *next*, and can we detect *that*?

Cool, so we have a bunch of detections looking for attackers getting into our network (IDS/AV/phishing/etc), but if they somehow get past all of that, it may be reasonable to assume they will perform some sort of reconnaissance (obligatory shout-out to [Mitre ATT&CK Framework](https://attack.mitre.org/) ). What would that recon look like? Well, maybe some of the commands could be:

>
> * whoami
> * id
> * ipconfig / ifconfig
> * netstat
> * ping
> * net user
> * tasklist
> * systeminfo
> * cmdkey /list
> etc.
>

Cool, here's the problem. If you just create alerts for the execution of those commands, you and your analysts will wish for the sweet consequence of committing seppuku multiple times a day. It will be healthy for no one. However, if you were to take the culmination of all of these events, the chance that your EDR product would see a single user running 3+ of these commands in a short timeframe is *(probably)* exceptionally low. This could be a fantastic method of catching those squirrelly actors that seem to find an imaginative way to get into your network. *(Quick anecdote, a long time ago in a former life I was looking at some data where a threat actor was able to hack supply chain to get access to a target's network. Honestly, good luck catching initial access via supply chain, but once the actor got on the network, they ran a handful of recon commands to see where the landed, and it was exactly an analytic like this that initially caught them.)*


### Caveats

Yes, I know. Building a combination of atomic and composite alerts may be great, but that will not catch absolutely everything. This is a cat and mouse game, the back and forth between attackers and defenders will never stop. Great attackers [who understand defenders](https://criminal.group/infosec/2020/08/16/two-sides-to-a-sword.html) will always be exceptionally dangerous and hard to catch. Our goal is to make the bar as high as possible to increase the cost to attackers for looking in our direction.

### Outtie like a belly button

Here we explored a couple methodologies for creating detections to catch attackers. Basically they fall into two categories:

>
>  * Atomic - "If this certain singular event happens, notify someone"
>  * Composite - "If a combination of <x,y,z> events happens in a timeframe, notify someone"
>

While they are both aimed at identifying attackers as soon as possible, they approach it in slightly different ways. There is value in both of these ways, and neither should be used to the exclusion of the other. The question you should be consistently asking yourself is, *"If I were attacking me, what would I be doing?"*, and the answer to that question can help guide what you should be looking for.  Sometimes there is a single thing you'll expect an attacker to do, and sometimes there is a combination of (otherwise benign) things.

The ideology of not catching every single hacker on keyboard should never stop you from making progress where you can. Go forth and raise the bar one notch higher every time you can, because it's the culmination of all of these little steps in defense that will make your organization an absolute pain in the ass for an attacker to get in and stay in.

<3 you all. Please feel free to shoot me any feedback/comments/questions/concerns/etc.
