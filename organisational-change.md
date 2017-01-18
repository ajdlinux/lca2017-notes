Organizational Change: Challenges in Shipping Open Source Firmware
==================================================================

* Stewart Smith, OPAL Architect, IBM

* OPAL (OpenPOWER Abstraction Layer) is open source firmware for IBM Power systems

* Last year, I talked about the technical details of our firmware and hardware

* This year, I'm talking about the trickier people-related problems and how we got to where we are, and where we're going to

Change is inevitable
--------------------

* Netflix used to rent out DVDs! So did Blockbuster...

* We've had significant change in IBM Power Systems and our Linux and open source teams over the last few years.

Companies are big
-----------------

* IBM is big. It's twice the size of Hobart.

* I've worked in many scales of company, I think the same core ideas apply differently to each level

* You won't ever be able to get everyone in an organisation to row in the same direction, but the closer you get the better.

* If we express our needs, we have a better chance of having them met

Why talk about organisational change?
-------------------------------------

* Because you don't get to create new organisations and teams. And even if you are, you won't get it perfect.

* The world will change. We have to adapt to it. People change.

OpenPOWER
---------

* First OpenPOWER machine shipped Oct 2015. Many systems shipped since.

* Before OpenPOWER machines, we developed KVM on Power, which needed some changes.

* Traditional IBM machines: proprietary firmware, hypervisor and OS. Maybe a Linux guest under proprietary hypervisor.

* To get KVM running, we needed bare metal Linux. Hence OPAL.

* The Power S822L with KVM was the first machine that the Linux Technology Centre was deeply involved in

* At the same time, OpenPOWER initiative started to expand Power into new markets with more customers. More industry standard machines. License the whole stack.

* It was very clear that OpenPOWER was a strategic direction for the company.

* Strategy != tactics. Strategy is long term vision. Tactics are how you get there. OpenPOWER is a strategic direction with many individual tactics.

* If you have a strategic direction in a big company and an individual thing you want to do, link your thing to the strategic direction to convince management to let you do it.

Shipping OpenPOWER
------------------

* New machines, new firmware, new hypervisor, etc - what's the most important thing to do for the company? Ship it! First machine wasn't full OpenPOWER, but was a stepping stone to the broader vision

* The journey from there on to current OpenPOWER machines was fairly short

* Step 2 - release the source code! Everyone knows `git push`, that's how you release source code and solve all your problems, right?

* Hostboot 400 KLOC, OCC 70 KLOC, skiboot 70 KLOC, plus buildroot, linux, etc

* While we were pushing OpenPOWER, we still have an enterprise market that makes us a lot of money. We don't want to break things for existing enterprise customers.

* Opening it all up didn't happen at the same time. It was a process of change working with individual teams to get them to release their source code and maintain it. Doing everything in sync isn't necessarily needed - you can do it gradually.

* Proprietary BMCs - started with AMI, now other manufacturers, and now OpenBMC.

* We now had a nascent community and people could ship machines.

* June 2014, first POWER8 machine. October, Palmetto. December, code all on GitHub, March 2015 op-build 1.0, April Tyan commercial machines, October, IBM-branded OpenPOWER machines.

* Most of us in Australia were very familiar with open source communities, but other teams and external partners had different attitudes.

* This part of the journey happened! But the initial stage of change isn't the key to at all - you have to make it last.

Solidifying change
------------------

* How do we make open source community mindset intrinsic to developing OpenPOWER firmware? How do we manage this alongside business relationships? How do we have a more open development community where we do things in public?

* Eight steps for leading change: establish urgency, create guiding coalition, develop vision and strategy, communicate change vision, empower employees for broad-based action, generate short term wins, consolidate gains and produce more change, anchoring new approaches in culture.

* In IBM, you're not going to get everyone to agree on something - you need to create a smaller coalition to develop and enact change

* Short term wins are how you train animals...

* Encourage a cycle of change rather than saying "we're done, stop"

* Create a culture

* There are people who know how brains work - why not ask them how to achieve stuff?

* You can have very dysfunctional teams and you may need to figure out how the team works before you can proceed with larger change. Dysfunctions: absence of trust, fear of conflict, lack of commitment, avoidance of accountability, inattention to results. Not much of a problem in my experience at IBM.

Shipping is only the start
--------------------------

* Ship it again!

* Have other people ship it!

* Establish a healthy development community inside and outside the company

* ???

* Profit!

Specific examples
-----------------

* Bug trackers - do you even have one? Internal vs external? In IBM, we have several bug trackers - LTC Bugzilla (internal), PFW ClearQuest (internal), GitHub (external) and GitHub (private projects for work with external partners). Q: How do you maintain linkage? A: Tears and booze! Each system vendor has their own issue tracker. At any one time there was at least 4 bug trackers I needed to care about to ship a product. Can use bridges to help, but often need a human who understands things.

* Side quest - Bugzilla performance - to solve issues with our Bugzilla infrastructure, I used the same 8 step process to drive change. Need it working to ship our product, involve the right people, link to strategy, generate short term wins (do a simple MySQL change, move to cloud, etc).

* <3 your bug tracker. Bug trackers supply metrics. Use metrics to manipulate your management chain into doing things. When you look competent and know what's going on you build trust and win allies. Being able to show numbers saying you're getting better and things are improving is good.

* Side quest - bug bridges - when you're coming in to an existing project or community, their rules apply and you have to use their infrastructure and tooling. We didn't try to change the world, we fixed some of the issues and have a human solve the rest.

* There's no substitute for knowing what fires are currently burning.

* "The bug list is far too small... we're not doing enough QA!"

* Communicating up through management is a skill that's very different from dealing with devs. The assumed knowledge is very different. Bridging that gap and knowing how to communicate is helpful, especially when building coalitions for change.

Putting together a team for change
----------------------------------

* Needed: position power (including management to make it hard for other managers to block change), expertise (relevant to task at hand), credibility (people with good reputations who are taken seriously - not just someone who gets angry on the internet), leadership (people to drive it).

* Leadership != management. Very different skill sets. In Free Software, we know this very well: technical leadership != employee manager. Important to have good relationship with your management, important to have solid technical leadership.

* Leaders come up with vision. Managers come up with budget. Don't underestimate importance of either.

* Non-ideal people: you don't necessarily need to involve them in change efforts - you can work around them. But having civility is important.

Upstream
--------

* Upstream vs product: as an organisation that was incredibly product focused, hard to think about what could be done to improve deliverability of next product. Need to break down silos and tunnel vision. Encourage people to own the whole stack - product and upstream. We're making progress here. Upstreaming stuff to external projects (e.g. kernel) makes life way easier. If it's all upstream, creating a new product is easy.

* Vision: upstream first, upstream always works, good OSS citizens, release at any time, short lived forks for releases, IBM isn't special.

CI and testing
--------------

* An ongoing challenge

* How do we automate testing for multiple types of products, how do we package our test suites as a product so others can use it, etc...

* It's been a rocky road but we're making some progress.

Communication
-------------

* Focus on communicating things that will be wins for people - "you'll have less work to do if you do it this way"

* Sit down with people and talk through your different views of the world - this is very valuable. Something you see as completely insane may be sane in a different context. Establish common ground for collaboration. Sometimes works best face-to-face.

* Never underestimate thanking someone. Generate kudos and evidence that people can use for why they should get a promotion. Reward people. This will help people and win allies. Positive reinforcement!

Consolidating gains
-------------------

* Worst thing you can do after doing some amount of change is to say well done and finish! You may end up regressing.

* A critical time after initial wins - what's next? Snowball and drive more change. Keep your coalition or build a new coalition to achieve more.

Anchoring in culture
--------------------

* Often invisible. Culture is what you don't write down.

* Respect the old way of doing things - there's a reason why it exists. It may not be a good reason - you'll need to prove why the new way of doing things is better. (e.g. more bugfixes, shorter release time)

* Unfortunately, sometimes the way to change this is staff turnover...

Summary
-------

* Software problems are easy

* Hardware problems are easy

* Human problems are hard. Be pragmatic. You can't fix everything. Real change takes time, but it's worth it.

Questions
---------

* You started with the OpenPOWER group established. Comment on how that started up and how you got high level management buy-in that it was a good concept? A: I wasn't a party to it, but pressures from several places including up high. Several people had roughly the same idea. Overall vision was already there.

* RFC compliant email. Discuss. A: ....................

* Given the project involves other organisations, is any of this relevant to how you want to help them drive organisational change, or is this focused on IBM? A: Yes. One partner was reluctant to share source, and we worked closely with them to drive change through our and their org charts. They are now a model of sending stuff upstream to us. Beneficial to them - in fact crucial to them in getting new products out.
