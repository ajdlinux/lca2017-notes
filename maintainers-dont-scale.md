Maintainers Don't Scale
=======================

* Daniel Vetter (@danvet)

* I've been graphics maintainer for the kernel

* This maintainer thing doesn't really scale

* This talk is not about burnout - the "cult of busy" - know progression and early signs, this is very important to know as a maintainer. When I started as graphics maintainer there were 2 people working on it full time, now there's around 20. Jacob Kaplan-Moss: "what part of 'for life' don't you understand?"

The traditional kernel process
------------------------------

* `git send-email` + `git apply-mbox`

* ~80% of patches are applied by maintainers

* 20% are directly applied

* maintainers send pull requests to Linus or to another maintainer at an intermediate level until it goes up the hierarchy

New model
---------

* being implemented by graphics tree

* all regular contributors have commit rights! 

* maintainer group for coordination - interfacing with the wider world, like sending out pull requests

* take a look at Rust!

Things we do right
------------------

* git - git won. git was created by kernel developers for kernel developers. (Though it might not have the best UI...) It scales well and has made collaboration a lot easier.

* Commit messages - the further away you get from the kernel and into userspace, the worse the commit messages get... the Linux community, being very traditional and using mailing lists where people sent emails to explain their patches has carried into git. Useful history is great.

* Bisecting - kernel does very well at this

Things we do less well
----------------------

* How do people become maintainers? Mostly by accident. You write some code that seems useful. Other people find it useful. Congrats, you're a maintainer.

* What if other things come up in life that get in the way of maintaining? We don't handle this very well. Rather than adding extra maintainers, you get a second *level* of maintainers. You start sending pull requests to another maintainer.

* Lots of boutique trees. Very few trees which are run by group maintainership.

* The bus factor is an issue - but a far bigger issue is people going on holidays! When this happens, you can't get new stuff in.

* The hierarchy where we have single maintainers by default doesn't work very well.

Checks and balances (lack thereof)
----------------------------------

* Does the hierarchy take into account the needs of the 80% who aren't maintainers so their life doesn't suck? Are there checks and balances?

* 80% of patches are developed by a non-maintainer - the maintainer's job is to ensure the patch looks reasonable.

* The 20% that *are* written by maintainers - do maintainers subject their *own* patches to peer review? The maintainer one level up is supposed to check if those patches look reasonable, but is it real peer review?

* Peer-reviewed maintainer patches - Intel GFX: 97%, graphics core: 95%, GFX overall: 75%, everything else: 25%.

* In other contexts, e.g. managers/employees, teachers/students - you obtain anonymous feedback. Not in the kernel.

* Code of Conflict - is there a separate body that can enforce some standards? The TAB does not have enough enforcement power.

Starting a revolution
---------------------

* They're not going to be pretty. They're drawn out, long struggles.

* When we gave people the keys to the kingdom (commit rights), things changed from a hierarchy towards a mesh. Maintainers aren't as special as people think they are. People need to work together.

* Trust people to do the right thing - this results in much better collaboration.

* The maintainer isn't special when it comes to reviewing. Maintainer review often results in rewriting entire patches - which can push away new contributors. If the maintainer is seen as on an equal level, that's different.

* Downsides of having fixed roles - harder to mentor new people, not much freedom to move around and do different things.

* People become maintainers by accident - and if you become successful, there's not exactly a blueprint or any resources to help you. People learn by hopefully not burning out or burning too many bridges... Nadia's keynote discussed the need for advice for maintainers.

Some ideas for a maintainer's manifesto
---------------------------------------

* It's about the people. The software bit-rots, the maintainer works with people. It's still technical but very much about the people. Recognise people - write blog posts about what they're doing, etc. Trust them - more than they trust themselves, otherwise they never get a chance to grow. It's like trusting the kids with the car keys for the first time. Every once in a while someone will screw it up, but...

* Recognise your power. A common theme of maintainers complaining that they can't force people to do things and they're in a weak position. This is wrong. Every few months or so I have a project manager screaming at me because I exercise my power. Give all regular contributors commit rights so they feel in control. Don't play power games - sometimes reviews are just a "I can block your patch because I can" game with a technical excuse.

* Accept your limits. The maintainer role changes - maybe it's not a role you want as it scales up. Allow other people to step in. If you don't like doing something as a maintainer - CI, mentoring, whatever - don't walk around all the times saying that's useless or unimportant, that's creating a negative space.

* Be a steward, not a lord. Benevolent dictator for life - we talk a lot about "for life" as if it's a sentence... Looking at the maintainer role as about stewardship, building a community for everyone, working towards getting more users and achieving world domination!

Questions
---------

* (Rafael Wysocki) I agree with almost everything you said - except anonymous feedback. Is that based on your experience, or research? A: I don't know exactly how widespread it is, but sometimes I interact with others who discuss with me in private the troubles they have with their maintainers. From these stories I would say they have a hard time giving their criticisms in a useful way. This idea of anonymous feedback wasn't my idea. Providing maintainers with feedback would probably help in getting concerns addressed. I think it helps if you have some structure so that feedback comes from people who are actually involved in your subsystem.

* I'm an ARM platform comaintainer and I've been working on changing my little hierarchy from "no go away" to "yes". In the negativity of the LKML world, how do you inroduce code of conducts into even a small domain to start wokring on attitude? a: on the topic of codes of conduct, I see them as a product of expectationsm the first step is to change expectations. When a subsystem community has agreement that they want to be respectful to each other, then I guess you can introduce a code of conduct, but who's going to enforce it? If you can't enforce it then peer pressure if your code of conduct already and there's not much point having a code. Making feedback more constructive - we need more automation and CI so that the question of "is this correct" can be answered by computers and review can concentrate more on design and long term issues, and information sharing. Shifting goals of review away form nitty-gritty towards sharing information would help a lot in making it more useful and constructive

* (Stewart Smith) When you have a single maintainer who steps down or neglects the project obviously, that's obvious - what about when a co-maintainer steps away partially? A: Can't tell you how to handle co-maintainers stepping away. For committers overall you just have to deal with that. In Intel GFX we're in the process of figuring out how we add/remove committers. For co-maintainers - train up new people, phase out over time. If you have a subsystem that's dying, then...
