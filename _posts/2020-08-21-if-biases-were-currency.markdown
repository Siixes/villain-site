---
title: "If Biases Were Currency, I'd be Rich"
author: kyle
date: 2020-08-21 00:49:16 -0700
categories: [Blogging, Analysis]
tags: [analysis, cybersecurity, detections]
---

### I am one biased dude

Let's be honest, I am biased. You are biased. Repeat after me kiddos, _points to the blackboard_ "he, she, we are biased".

There are a plethora of things that contribute to this, including the culmination of our lived experience and perceptions of reality at any given point in time. These color the lens through which we view the world. The product of all of these factors results in the biases we have, the unconscious filters through which we interpret the world and inputs around us.

Why am I talking about this? Because I am very interested in analysis, and as pointed out [here](https://criminal.group/infosec/analysis/2020/08/13/the-art-of-analysis.html), analysis is the observation of a data point and making an inference off of that observation. If you are not careful, the biases that you bring to the table can jade how you interpret data, which can result in you coming to incorrect conclusions and making incorrect decisions because of it.

For example, in [The Psychology of Intelligence Analysis](https://www.cia.gov/library/center-for-the-study-of-intelligence/csi-publications/books-and-monographs/psychology-of-intelligence-analysis/PsychofIntelNew.pdf) they discuss a fascinating study. The gist is this, someone is shown only a small portion of a picture, and they need to guess what the picture is of. Slowly, the picture is completely revealed. As the picture becomes slowly more revealed, the subject has to keep guessing what it is of. The result was pretty fascinating:

> The earlier someone had to form an opinion of what the picture was, the longer it took and the more concrete data was required for them to change their mind. The longer someone waited before they had to form an opinion on it, the easier it was for them to change their minds based on the increasing amount of data presented to them.

Why does this matter to us? Well, if we don't recognize the biases that we have towards the data we work with and how to counter them, it can make our jobs/lives **a lot** harder.

### Story time of how I almost failed

There I was, about to sip some coffee, starting out the day. *"Hrm, what do we have here...an alert for user brute forcing?"* Yup, there it was, an alert that there was a workstation on our network that had generated roughly ~16k failed authentication attempts in the past few hours.

*"Shit, ok, gotta figure out what's going on...something trying to brute-force a user's password?  Something trying to brute force a bunch of users' passwords?!"*

The clock is spinning...

*"Okay, take a step back, something must be running on this machine that is trying to do all of these lookups. Let me see if I can find the process in ${EDR tool}....shit, the workstation isn't there!"*

The clock starts spinning a little faster...

*"The workstation is on a VPN IP. Okay, I have the username of the person that's VPN'd in (we'll call her Sally). Okay, workstation name looks like a personal computer they used to VPN in...damn, that's probably infected to hell. What did they download recently? We don't have ${EDR tool} on their personal machine, so no way for me to figure that out. Should I kill their VPN? Lock their account? Send them a message trying to figure out what they installed lately?"*

The clock is working in overtime...

*"Okay, these are failed authentications. Let me go check the Active Directory Domain Controller logs...if there are thousands of failed authentication events for this user's account maybe someone is trying to brute-force their password.  They're VPN'd in, so they already had to successfully connect with creds, why would malware be trying to brute-force their creds if they just successfully logged into our network? I dunno, I've seen malware do dumber shit..."*

...or maybe...

*"Maybe if I see this machine failing authentication for a ton of different accounts! I can see what accounts it's trying to break into! Then, maybe I could take the list of accounts and figure out more about this malware! If it looks like legit internal user accounts, this could be a well informed attacker! If it looks like generic usernames, maybe I could take those and try to figure out what malware does that. So even though I have no access to this person's computer, maybe I can get a clue about what's going on..."*

...my body is ready, give me them DC logs...

And I see ~16k failed logon events in all their glory. Looking through them, every single one of them was a **Windows EventID 4625** (failed logon attempt) with a **Status Code of 0xC0000064** (username does not exist). The username of every single one of them was for Sally (like, *SALLY-WORKSTATION/sally*).

...wait, u fokken wut m8? What is going on?

After consulting with far wiser people in my IT department, it turns out that this is a known thing. If someone VPN's into our domain on a personal machine, there are some domain resources Windows will try to fetch. However, the client's Windows OS won't prompt the user to enter domain creds to access those resources, it'll just try to use the user's currently logged in creds in the background.  Well, obviously we don't create AD objects for people's personal computers/user accounts, so in the background of the user just being VPN'd in, they were racking up failed authentication events on our domain controller like they won the jackpot in Vegas!

### Miley Cyrus remix - I (could've) CAME IN LIKE A WRECKING BALL!!!

There were multiple times throughout this process that I could have (in semi-good-conscious) ruined Sally's day. She pops up as a culprit for account brute forcing, and there were a number of times that I was about to kill vpn/lock out account/etc until I figured out what malware was running on her machine. Not having all of the information made it exceptionally easy for me to jump that conclusion. It doesn't help that the alert that I was responding to, account brute forcing, colored how I was interpreting the situation. (Side note: this is why it is so important to do everything you can to understand the logic behind the alerts you investigate, so you are familiar with where they might fail, as described in my blog [here](https://criminal.group/infosec/2020/08/20/your-detections-arent-working.html) )

I have seen/heard of a number of times when the SOC takes nuclear measures before they have all of the data. I have also seen analysts that look at data and initially come to a conclusion about what is going on, and *no matter what new information is presented to them that contradicts their conclusion*, they never give it up.

Do not let this happen to you. Learn to recognize the biases you start investigations with, because if you can't even recognize them, how will you ever know when they take you down the wrong path?

Again, hit me up with any feedback/comments/questions/etc.  Go forth and do good shit fam!
