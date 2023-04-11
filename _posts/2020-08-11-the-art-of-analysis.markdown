---
title: What is analysis
author: kyle
date: 2020-08-11 18:32:00 -0500
categories: [Blogging, Analysis]
tags: [analysis, cybersecurity]
---

## Why this post

There are a plethora of specialities within InfoSec, many of which have a number of learning resources dedicated to them. However, one aspect that I believe is fundamental to many of them, yet rarely receives any amount of attention, is analysis. There are entire career paths within InfoSec that, by their very name, imply that analysis is the fundamental activity (SOC analyst, threat intel analyst, etc). However, there is almost no shared understanding of what analysis actually is. If someone is an 'analyst', but cannot define what 'analysis' actually is, how can they expect to get better and develop their skills in this undefined activity? Analysis is an actual skill that can be learned, practiced, and honed, however, that requires us to define what that looks like.

## Caveats

I am not even close to the first person (nor the last) to ponder and write on the essence of analysis. There have been numerous before me to explore and document their findings on this process. One of the most notable that comes to mind is [The Psychology of Intelligence Analysis](https://www.cia.gov/library/center-for-the-study-of-intelligence/csi-publications/books-and-monographs/psychology-of-intelligence-analysis/PsychofIntelNew.pdf). While that explores analysis through the lens of government intelligence analysts that seek to leverage their analytic skills to inform policy makers, it covers many cognitive biases that impacts how we do analysis in other fields.

This post is not a comprehensive view of analysis and how it can apply to our functions in the InfoSec world, but my goal is to provide an initial introduction to the subject. How can people beginning in the field better identify a path to grow their skills? How can someone established in the field better evaluate the accuracy of their own work? These are a couple of the topics this introduction seeks to touch on.

## Definition of Analysis

In order to have a meaningful discussion of how to get better at analysis, we need to define what the act of analysis actually is. I currently define it as:

> "The observation of a data point, and making an inference from that observation."

 While this may sound like a simple definition, I believe there are multiple aspects of it to unpack in how that applies to the act of 'analysis' in the InfoSec world. In my mind, a few of those aspects are:

>
>    * Our level of expertise in the data we are observing
>    * Our ability to synthesize inferences from multiple data points together
>    * Our ability to understand logical chains that lead to our data point
>    * Our imagination to account for unintended results (or "be prepared to be wrong")
>

## Data Expertise

Our ability to 'analyze' data can be directly correlated to our level of understanding of that data. For example, let's say you walked up to my desk and handed me a magazine written in Hindi and asked me to tell you what it says. Depending on my level of understanding of Hindi, I may have any of the following results for you:

>
>    1. If I don't know Hindi at all, I may say, "Sorry, I got nothing for you".
>    2. If I have the most basic understanding of the language, I may know enough to look up each word in the dictionary and give you a very rough translation in broken English.
>    3. If I have a well versed understanding of the language, I may be able to give you a translation in coherent English.
>    4. If I have an expert level understanding of the language, I may be able to not only give you a proper English translation, but also provide insights such as, even though they said "A", that is actually a Hindi saying that actually means "B".
>

Given this example, it becomes more obvious how our ability to infer data grows as our expertise/understanding of the data in question grows. It's not abnormal for people entering a career in Infosec to be told something like, "Go learn networking/operating systems". Even starting out as an "analyst", this is imperative advice because it enables people to better understand what they are trying to analyze.

## Synthesizing Multiple Data Points

Our ability to build a complete picture of reality rarely hinges on a single data point. However, when analyzing technical data, sometimes we fall into the trap of drawing conclusions off a single data point (a single network connection, a single process execution, etc.) without looking at what else was going on at the time of the event that can change the context.  Imagine the below scenario:

>    * There is a man knocking at your front door, and he is holding a knife in his hand.

How would you interpret this situation? Different people will evaluate this scenario differently; some may feel innately threatened, while others may not. However, what if we add the following data points:

>
>    * There is a man knocking at your front door, and he is holding a knife in his hand.
>    * This man is your neighbor that you have known for years and have a positive relationship with.
>    * You let him borrow a kitchen knife last week, which he said he would return. That is the knife he is holding.
>

Adding these data points can completely change the context of the scenario and how we interpret what is going on. This is where it is important to get a composite view of multiple data points to provide clearer context for your initial data point of interest.

## Understanding Logical Chains of Data Points

This one is slightly tied to the previous two. Sometimes you can observe a data point that allows you to infer other data points, even though you may not be able to observe them. This is recognizing that your data point of interest didn't happen in a vaccuum and requires certain circumstances to get to the point that it got to (even though you may not have access to other data points to back that up). Below are a couple examples for this:

>
>You observe a computer on your network making a network connection to 'downloadingmalware.com' (or whatever example malicious domain you want to use here)
>

There are multiple other data points you can infer from this:

>
>    1. Computers don't make network connections because they just feel like it. There must be a process
>    on that computer that initiated that network connection.
>    2. That process was not started in a vaccuum, it was started by something...potentially this was started
>    automatically by other software, or perhaps it was started manually by a human.
>    3. There is a fair chance that there is a binary file (like a MZ executable on a Windows system) on
>    the computer associated with the process that made this network connection.
>    4. The computer had to be able to resolve the domain 'downloadingmalware.com'. Are you logging DNS, and
>    did it make a DNS lookup for that domain? If there is evidence that a DNS lookup did **not** happen, that
>    could imply the domain could have been added to the computers hosts file. Is there evidence of that?
>    If so, there may be _another_ process that made that change.
>    5. (assuredly more here)
>

Looking at that, you can take a single data point and identify related data points that would had to happen to get to the data point that you initially observed. You can then start to build a trail of other data points to begin investigating to create a more complete picture of what happened.

Below is another example of this from a slightly different perspective. Caveat: I MADE UP THE BELOW DATA FOR THE SAKE OF ILLUSTRATION, IT IS NOT REAL. Let's say all you have to evaluate is a single line of netflow data:

>
>Date flow start          Proto   Src IP Addr:Port      Dst IP Addr:Port        Packets    Bytes
>
>2010-09-01 17:29:00.459     TCP     159.93.12.79:22   ->  103.148.117.12:22126        1    61488  
>

There are multiple ways people could interpret that data. Some reactions could be, _"that is just a single packet and not a whole session, so it doesn't tell us much"_, or _"That is SSH because of port 22, so it is encrypted so we don't have any idea what's going on"_
. But you could also infer some of the following data:

>
>    1. That is TCP port 22, which implies it is SSH, but it is going in the server -> client direction. Given this, we can immediately imply that SSH (or a process listening on port 22) is actually running on the server.
>    2. Given what we discussed above, the server SSH process will not randomly reach out to the client, so we are probably seeing a reflection of the server responding to traffic initiated by the client.
>    3. Looking at the size of the packet, we can imply that the client has successfully SSH'd to the server.
>    * Thinking about how SSH works, a client will attempt to connect either via pki or intending to use a username and password.
>    * After the 3-way TCP handshake, the server will traditionally display it's banner and negotiate the authentication, requesting a username and password.
>    * If the client fails 3 password attempts, the connection will be killed and they'll have to start over.
>    * Thinking about that, in the circumstance where the client does *not* know how to correctly authenticate to the server, the amount of data transferred from the server back to the client is very small (the only real variability in it is the size of the banner).
>    * Given that the size of the packet is 61,488 bytes, that can reasonably be assumed to be larger than any banner that a SSH server would return. This implies that the client has completed the authentication to the server, and ran a command that elicited the server to return that many bytes of data.
>    4. The server IP address belongs to AS2875, which is the Joint Institute for Nuclear Research in Russia, and the client IP address belongs to AS207529 (Adnan Rafique) which is in Pakistan.
>

Given an understanding of the protocol at play in the above data point, you can go from a perception of, "this is just a packet", to "an entity in Pakistan appears to have successful SSH access to a server in Russia's Joint Institute for Nuclear Research", which is a far more comprehensive view of the situation, all based off of the implications of what would be required for a single data point to exist.

## Imagination to Account for the Unexplained

Sometimes technology doesn't work the way we think it should.  For example, one time I was looking at building detections based on [Windows logon types](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4624) and RDP.  There are different logon types in Windows logs that I _expected_ to account for different situations. For example, there are interactive logons (which normally means hands on keyboard, "type 2" logons), some network logons ("type 3" logons), and some remoteinteractive (which is supposed to represent RDP, a "type 10" logon).  So I assumed you could use the logon type of a Windows log to start building detections for certain kinds of activity. To my surprise, when I was doing testing, I realized that the way RDP was implemented at the time, a single RDP session created a _type 2_, _type 3_, *and* _type 10_ logon event. This took me by surprise and I had to completely alter the way that I intended to start building detections and alerts.

This is where our internal biases and the very expertise that I espouse we develop in ourselves in the first point, can get in our own way. The very expertise we develop within ourselves may give us a false sense of confidence in our initial interpretation of data. I have had a number of conversations with analysts where they observed a data point which did not fully conform to their expectation of how a protocol/process _should_ work. Sometimes they got so far deep in the rabbit hole trying to defend their expectations of how things should work, that they lose sight of what the data points are actually telling them.

One great way that I have found to check a conclusion that I come to is to ask myself the question, _"What data, if presented, would render my hypothesis incorrect?"_ For example, let's look at the SSH netflow example above; what if we determine that the SSH service isn't running on *159.93.12.79:22*, but it is actually HTTP? That would negate every other follow-on conclusion we made based off of that data point and we would have to develop new conclusions. Then, what if we said *159.93.12.79* wasn't acting as a server at all, but *103.148.117.12* was hosting a service on TCP port 22126? That would again change our entire interpretation of the data point and force us to come to new conclusions. But if we don't actively look for data points that can disprove our theories, it can be far more difficult to identify instances where our analysis is incorrect.

While this is just a single example of identifying biases in our thinking and analysis, there are many more that may arise that are covered in other writings, such as *the Psychology of Intelligence Analysis* noted in the beginning. **Some of the most successful analysts that I have observed have been able to take data points that did not conform to their preconceived notions, identify where it diverged from their expectations, research the other data points that contextualized why the divergence happened, and then reincorporated this new information into their experience/expectations going forward.**

## TL;DR at the End

It is odd that analysis has been largely undiscussed and undefined from an industry perspective, despite being such an integral skill in many positions. If we don't actually understand what it is, how can we get better at it? Analysis could be defined as "the observation of a data point, and making an inference from that observation." One's strength and ability to do that can be deepened by some of the following skills:

>
>    * Our level of expertise in the data we are observing - If you are analyzing network data, learn networking as well as you can. If you are analyzing processes, learn operating system internals the best you can. etc.
>    * Our ability to synthesize inferences from multiple data points together - How often do you look for alternate data points that are related to what you are looking at to build a more complete picture of what is going on?
>    * Our ability to understand logical chains that lead to our data point - Data points almost never happen in complete isolation. What other factors must exist (whether you have the data points to support them or not) for your initial data point to exist?
>    * Our imagination to account for unintended results - Sometimes technology (and the world) does not work as we think it should. When you come across data that breaks your expectations, how able are you to incorporate other data points to compensate for that and adjust your expectations? Are you flexible enough to adjust your expectations of reality at all?
>    * Sometimes we are wrong and our expertise can trick us into consistently assuming we are right. What are you doing to identify when this happens?
>

Hopefully this is useful for anyone. Feel free to hit me up with any feedback (positive or negative), questions, or comments. Hopefully more to come soon. Peace out fam!
