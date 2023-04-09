---
title: "Why am I here?"
author: kyle
date: 2020-08-15 21:41:16 -0700
categories: [Blogging, Analysis]
tags: [analysis, cybersecurity]
---

## Why this post

Go to any SOC or blue team or similar org and look at multiple analysts. Over time you will probably identify some analysts "do better" than others. There are a variety of reasons why this may be, but we don't usually have good solid advice for what to do about it. Or even if you remove the inter-personal comparison aspect of it, if you were the only SOC analyst in an organization, what's the answer to the question, *"How do I get better at this?"*

> For those analysts that don't appear to be performing as well as they can be, how can they improve?

If someone is just beginning in their career, advice like *"just work more tickets"* and *"git gud nooblol"* aren't helpful.

Strap in your cliche seatbelt and get ready to bear with me here.  I think that we don't ask the question, "why am I here?" enough of ourselves in regards to our jobs, and what that means for the work we do. This is meant as a follow-on to the previous [article on analysis](https://criminal.group/infosec/analysis/2020/08/13/the-art-of-analysis.html) and most specifically targeting people in traditional InfoSec positions, but hopefully could be interesting in other ways to other people.

## Answering the question

Here's the trap I see many InfoSec professionals sink into...it's like the scene from the Office Space when the Bobs are asking Glen, *"What would you say you do here?"*  Then the security guy says, *"I take the alert from ${tool A} and I close it in ${tool B}!"*

"Why am I here....like, really?"

>
>    * I monitor for alerts from ${x} system...
>    * I look for threats in our network...
>    * I attack our own network to dunk on the blue team...
>    * etc. etc. etc.
>

If your answers are anything like the above, it may be worth doing some deeper thinking about that question. For most of us that work in the security field, whether directly for a company or contracting for customers, really that answer boils down to "protecting the company".

>  Thanks Captain Obvious for the astounding observation...

Hear me out, because I think there are aspects of this most people don't think about. I think many people view their work functions without the full context of where they fit in the big picture.

## The Big Picture

Companies do stuff. They generally hope to do stuff to make money. Ideally, if they can do stuff efficiently and well, they will be better at making money. If a bunch of unanticipated roadblocks get thrown in the way of how the company hopes to operate, that can impede their ability to make money. That is where we come in. Generally, we attempt to identify these areas where things can/have gone wrong, and either fix them where we can, or inform the company of the situation where we don't have the authority/permission/buy-in to fix it. With the company informed, they can make the decision about balancing the risk versus reward of addressing or not addressing the issue we are raising.

>    Why am I still reading this? Where is this going?

As mentioned above, most people don't consider the work they do in the proper context of the big picture. Here is an example of what the big picture may look like:

  * Company uses systems (networks, workstations, servers, etc.) to perform the functions they hope will make them money
  * Those systems generate logs about what they're doing, and they get sent ${somewhere}
  * Some threat hunter-like person somewhere develops a detection that says, "Hey, in these kinds of logs, if you see the pattern ${x}, that is bad"
  * Inevitably that pattern matches, and it generates an alert
  * There is an analyst that looks at that alert and has to decide, "Is this really bad? and if so, how bad?"
  * Depending on the answer to that question, they decide how to proceed from there

Mixed in with all of this, we may also have a pentester or red teamer that is also attacking the company's resources with the goal of understanding things like:

  * If an attacker used tool/technique ${a}, would it actually work?
  * If an attacker used tool/technique ${a}, will it get caught?
  * When gaps are identified, work with the blue team/defensive team to fill that gap so no other true adversaries can take advantage of it

## An example as concrete as silly putty

Let's say you're a SOC analyst. It is not abnormal for many SOC analysts to view their job as clicking through tickets. However, if we were to slightly switch the context, we could reframe that as:

>  my role is to evaluate data points to determine risks to our company and react appropriately

What would something like that really look like? Let's say you get an alert (maybe it's an anti-virus alert, maybe it's some behavioral detection, whatever...), your goal is to analyze how accurate that is. When you see that, below are some questions you can start asking:

  * How accurate is the detection that lead to this alert?
  * Do you know the logic behind this alert?
  * How much do you trust that alert?
  * What other data can you use to determine how legitimate that alert is? Do you have access to that data?
  * What assumptions are you making about this alert? (this is a huge one with many sub-topics and will probably be the subject of another post)
  * What possible explanations are there for this alert firing? How can you test those explanations?

By being able to answer questions like these you will be able to better assess the actual risk and optimal outcomes of that alert. For example, you may be able to identify this alert is a false positive and prevent some poor user from having their machine isolated/re-imaged/etc. You may investigate this alert and identify a chain of legitimately malicious activity. Or, you may dig into the alert and identify the fact that there is another set of data that you'd need to fully determine how legitimate it is; which would be a fantastic use case to raise with the business/${other teams} about getting this data going forward.

### ...and now for a diatribe...

>  Ha! Suggesting we can make other teams give us data we want. That's funny, we (security people) are pretty powerless in the kinds of data we can get our hands on. We don't have buy-in from the business and are mostly an afterthought when it comes to the business.

Trust me, been there, know that feeling. Here's a secret.

Lean in a little closer to the monitor and I'll tell you....

...


...closer...

...ready?

As an InfoSec professional, maybe half of the problems you'll have to solve are technical. The rest are social, relationship-based, and communication-based.

Yup, I took what this post was supposed to be about, got distracted on a diatribe, and took you all into left field with me.  But even after ~pretending to~ proofread this, I feel like this is important enough of a point to diverge from what I really wanted to tell you. Many times, the reason that the business doesn't give you buy-in on logging that one thing, or getting telemetry on that data is because they haven't been given a good enough reason to.

*"B/c ScUriTy!"* will very rarely work, yet that seems to be the tone of a number of justifications that I've seen/heard in the past.  Remember, the business exists to make money, and more than likely they aren't making money by selling your work in security (i'm handwaving infosec consulting firms atm, even though their own internal security teams count as sunk cost (assuming they have their own internal security team...)).

If you frame the conversation in the light of helping the business continuing to make money, that can drastically increase the chances of success. An example of this could be something like:

>    "We get ${x} number of anti-virus alerts in our Windows server environment a week. However, because we don't get Sysmon-like logs (as an example) from these servers, we don't have the ability to quickly identify when these alerts are valid. This could result in an inability to detect or investigate a large-scale compromise in our server environment that could affect our users' ability to use our product."

This approach still pleads the case for what you need to most effectively do your job, but it presents it in a fashion that justifies the monetary/man-power expense to the business to get it done.

## Why are you here?

You are not here to push tickets. You aren't here to mindlessly click buttons. That kind of work (generally) isn't fulfilling on a fundamental human level and will inevitably be automated away. **You are here to analyze data and make decisions that keep the company safe so it can do what it's been created to do** (yes, the phrase "analyze data" can have a plethora of meanings which can be explored at a later date).

Now that we've explored what analysis actually is, and how that roughly fits into our world of InfoSec, next we can start to explore what it means to get better at it.

## Outtros are overrated

I will say this now and always, but I am happy for anyone to hit me up with feedback. Negative, positive, ad hominems, whatever you wanna send my direction.  

Take care of yourself fam.
