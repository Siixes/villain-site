---
title: "Why We Phish?"
author: kyle
date: 2021-07-22 22:41:16 -0700
categories: [Blogging, Phishing]
tags: [analysis, cybersecurity, approach]
---

## A Brief History of Time
One benefit of being around the InfoSec industry for a while is that you get a longer snapshot of time to observe trends, mindsets, and methodologies. You get to watch, over the long-haul, what tends to work and what doesn't. For example, looking back at security conferences 10-15 years ago, most conversations and presentations felt like a blue team pity party, where the tone boiled down to, "woe is the security industry, attackers will always win, defenders will always lose." Over a period of time, there developed pockets of blue teams/defenders that started to alter their mindset of the situation.

> "Maybe we can't drastically alter the initial barrier to entry into our network for attackers, but maybe we could become exceptionally good at identifying them and kicking them out of our network before before they can do anything meaningful."

By altering how they view the situation, some teams were able to build tooling and processes to do just that. Teams that get good at this can drastically raise the cost for attackers to operate effectively, which I would count as a win.

What does this have to do with phishing?  

If you've been in any security meeting/conference/talk in recent years, you'll find another manifestation of the above woeful chant. As soon as the speaker asks the question, _"What is the weakest link in any network?"_ you can almost feel the entire audience think in unison:

> "The user is the weakest link."

And we tacitly accept this. We just accept that users will click on every single link that shows up in every single email. Every attacker is confident users will click their links, and every defender is confident there's gonna be a mess to clean up afterwards.

I don't believe this needs to remain the case. I believe that we, as an industry, could alter how we approach the problem to make meaningful progress in this area.

## The Old Way
The way that many companies attempt to solve this problem traditionally contains some mix of the below items:

>    * As employees are onboarded, part of their onboarding may include a phishing awareness training

>    * Employees are required to periodically (like annually?) complete their phishing awareness training

>    * InfoSec teams or security consultants are brought in to phish the workforce, identifying who would still fall for phishing attempts from real attackers

>    * Those employees might be disciplined, put on a watch list, put back in a queue for more phishing campaigns, or required to take more rounds of phishing awareness training

We do such things, people continue to click on links, then we sit back and say, _"see, these people never learn, they keep clicking on links in phishing emails..."_

## Why do we do these things?
There are completely valid reasons for us to be doing things like these, but I'm not sure if most of us easily understand or could articulate what they actually are.  If you ask yourself "why" enough times, you can get to the heart of it. Such as,

> "Why do we do phishing awareness training?"

Because phishing is a dominant method for attackers to get access to our network, and we don't want our employees to fall for such things.

> "Why do we perform periodic phishing campaigns?"

Because we want to identify and validate who didn't understand the phishing awareness training and still leaves the company vulnerable to such attacks.

> "What should we do with those people?"

We want to enable and empower them to not be as vulnerable to such attacks.

> "How should we accomplish that?"

Now THAT is a fantastic question. If, after completing any arbitrary number of phishing awareness trainings people continue to fall for the same phishing techniques, why do we conclude that more of the same training will yield any better results?  If a child has a hard time understanding basic addition, we _expect_ their teacher to find a different way of teaching them in a way they can understand.

How is it that we desire behavioral changes from our coworkers (the act of better recognizing phishing emails and NOT clicking on the links), yet when our "industry standard" training fails to elicit the behavioral changes we desire, we just throw our hands up in the air and write off the users themselves as opposed to exploring better ways of reaching them?

## You know what they say about assumptions...
In my proposal there are some caveats and assumptions that I am going to make. The ones that I can think of are:

>
>    * This will not address every use case and edge case that we care about, and I am not attempting to do so. This is primarily geared at the very basic use case of, "we want to ensure our co-workers are as resilient as possible in identifying and not falling for link-based spear phishing".
>    * You have employees that generally want to do the right thing. This may sound odd, but if you have malicious (or maliciously indifferent) coworkers that don't care about downloading all malware under the sun, then no difference in approach may matter.
>    * You have an InfoSec team that is willing to work with employees in investigating potential phishing. Remembering that our goal is creating resilient co-workers who don't fall for phishing. There will absolutely be times where they will know enough to be suspicious of an email, but not know enough to determine if it is truely malicious. In those cases, there is a good chance they may reach out to Infosec for a sanity check. If the Infosec team responds in ways that negatively reinforce that interaction (being gruff, dismissive, condescending, etc), that will deter from the exact behaviors we are trying to develop.
>    * These phishing campaigns are approached in legitimate good faith with the intention of educating and changing the behaviors of our co-workers, and not with a "gotcha!" mentality. No body _likes_ to be tricked, *especially* in bad faith. If this is performed in a manner that comes off as condescending, then general trust will be lost with the people whose behavior you are seeking to change. That is never a good place to be.
>

## Dude, get on with the proposal...
Given the fact that we are seeking behavioral changes, what if we target our phishing campaigns to provide feedback to our coworkers that is *immediate* and *relevant*.

What if we created our phishing campaigns in such a way that, when a user clicks on a link they are taken to a page that immediately notifies them that they got caught by a phishing campaign. But not only that, the page contains easily digestable information for *how* they were caught by that exact phishing campaign. This would include things like *screen shots taken directly from the phishing email they just clicked on and probably still have open in a tab* that highlight things like:

>
>  "Here's a screenshot of someone's name you probably know, but coming from an email address that TOTALLY doesn't match."
>
>  "Here's a screenshot of a link that looks fine, but when you mouse over it, you can see it's gonna take you somewhere totally different."
>
>  etc.
>

Here are a couple reasons why I feel like this approach is so critically important:

>
>    * Timely - This feedback needs to be timely. For example, if you are potty training a puppy, if it pees on the carpet, you can't wait until the next day and _then_ punish it. The association between the behavior you are trying to change and your feedback is totally lost. How in the world can we phish coworkers, put them through follow-on training at an arbitrary time in the future, and expect them to ever understand what they should have done differently at the time that really mattered? Also, even *if* they remember the phishing email in question, almost assuredly the context of why they clicked will be lost or muddled (such as recognizing things like, "I clicked because I felt a sense of urgency because it looked like it came from my CEO, but I forgot to make sure the email address matched...")
>    * Relevant - The feedback needs to be relevant. People can alter best if they can understand, concretely in that moment, what was wrong. Even if you immediately inform them they got phished and link them back to your general phishing awareness page, if it's not pointed out to them directly, how are they expected to decipher exactly which tactics tricked them? For example, I am trying to learn how to work on cars. If I mess something up on my car, it would _not be helpful_ to hand me a 300 page manual for everything there is to know about my car. Sure, the answer is assuredly in there, but I don't know enough to know exactly where I went wrong and where I should look to correct myself. I need someone to take me by the hand and say, _"Dude, you were trying to change the oil. The bolt you took out drained the coolant, not the oil. You should put that back and take out that other one..."_  This is why it is critically important for us to provide the feedback to our coworkers *in the form of the phishing email that they still have up right in front of their faces* to highlight what they _could_ have caught and what _should_ have jumped out as being suspicious to them.
>

## I see your logical fallacy, and raise you another logical fallacy
I am wrong about many things, and perhaps this is another example of where I am wrong. Upon some very brief socialization of this thought process, below are some of the counter arguments (both that I have actually heard, as well as those that I internally imagine that could be presented), along with my counter-counter arguments:

>  "Well, if you provide this immediate feedback, it may happen to one person, then they tell their coworkers about it, and those people won't click your email so it will skew your metrics."

I mean, yes that is true. But my take is "coworkers socializing recognizing phishing campaigns should be a positive behavior we reinforce, not a negative one because it _potentially_ throws off the numbers we get to present to management". Remember, the *entire goal of this* is to elicit positive behavioral changes.

>  "If phishing is one small link in an overall red-team chain of attack/engagement, then of course, you wouldn't leverage this approach."

That is absolutely a true statement. If the intention is 'phishing as part of a larger operation', don't do this. But if we are phishing as an exercise to create a more resilient workforce, then I think we should do this.

>  "This will require more effort on the teams creating the phishing campaigns, because you have to tailor each landing page to the campaign"

That is true, but it is important to keep in mind what problem we are solving for. The entire point of this is to solve for trying to change end-user behaviors via direct educational feedback. If "phishing engineer time" is what we are trying to optimize for, then I would argue that we are focusing on solving the wrong problems.

>  "This kind of education could result in the workforce looking more suspiciously at their email in general, resulting in more frequently pinging Infosec for sanity checks on potentially non-malicious things."

Again, in my mind, that would be a successful outcome of an effort like this where the relationships between Infosec and coworkers could be groomed over time to make the situation better. However, as called out in my assumptions, some InfoSec teams may not want that level/kind of interaction/engagement.

> "You don't know if this will work!"

That is absolutely a true statement. But the problem is that I don't know if this would work because I am not aware of anyone that is actually trying this approach. I would love it if someone actually tried this and was able to measure if it elicited the kinds of changes that we, as an industry, are desiring.

## "...C is for Conclusion..."
I believe that, as an industry, we have been going through the motions with all of these phishing awareness trainings and phishing campaigns. We are fantastic at "doing what's always been done" and measuring how poorly our coworkers are at recognizing phishing emails. We are just as fantastic at complaining and blaming them as opposed to taking the onus on ourselves to figure out a better way to address the real issues at hand.

I think there are real tangible ways that we can make meaningful progress on this front, but the required changes will have to start with ourselves.

If you have any feedback on this, please feel free to shoot me an email, shoot me a tweet, or even shoot me with a rubber band. I'm usually always happy to chat and reconsider my thoughts on these things where warranted!

(p.s. I realize that I didn't mock up an example of what I am thinking about the kind of educational landing page for a phish that I described. I am horrible when it comes to UI design and wouldn't trust myself to create an example of what a 'gold standard' would look like, but if it would be helpful as a starting example, feel free to ping me and I'll be happy to give it a go.)
