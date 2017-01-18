Consider the Maintainer
=======================

* Nadia Eghbal (@nayafia)

* Consider the lobster... once upon a time, lobster was a low-class food, and it became a high-class food. Lobster is extremely fresh - because we boil it alive. Is it alright to boil a sentient creature alive just for our gustatory pleasure? (Is the previous question irksomely PC or sentimental?)

* When it comes to open source - is it alright if a project struggles or dies because the maintainer can't keep up any more? Is it alright to compromise or even deliberately ignore, the happiness of maintainers so we can enjoy FOSS?

* "The whole animal cruelty and eating issue is not just complex, it's also uncomfortable... avoid thinking about the whole unpleasant thing" - it's similar in open source.

The problem
-----------

* More people are consuming open source over the last 20 years.

* 1998 - Netscape open sourced its browser, and had 180K downloads in 2 weeks. 2017 - lodash has 18M downloads in the same timeframe. 100x!

* 2001 - SourceForge had 208K users. 2016 - GitHub has 14M users. 70x!

* Two-thirds of top GitHub projects have 1-2 people maintaining them. Massive growth in users, but much, much smaller growth in maintainers.

* Why so few maintainers?

* 1. Style of production has changed - more smallish projects. Not as many big projects with multiple maintainers. This matches the change in creation style in other areas.

* 2. Being a maintainer isn't glamarous. Spend a lot of time triaging issues and responding to pull requests and so on. Appealing to some people but not most.

* 1% of people create the content that 99% consume - except bloggers don't have to respond and merge every single comment on the internet. Maintainers do.

* Rapid evolution poses the risk of overwhelming the system - thus introducing errors more quickly than the system can fix them (Steven Weber)

* Consumption scales if there's no additional cost for the producer - that's true for videos on the internet, but more users of open source projects involves additional effort for the maintainer.

* For many small projects, the increase in users and contributors isn't problematic - but for many larger projects...

* Many users today are new to software development and don't understand norms and techniques. Organisations like GitHub need to be better at educating users on how to do things like resolving merge conflicts...

* Maintainers are lacking support and mentorship.

* People see themselves as users rather than potential contributors - lots of shouting about "wasting time" and maintainers being "rude"!

* Making controversial changes leads to backlash from users and lots of rudeness

* We don't talk about the need for maintainership as a discipline when we talk about bringing new people into open source.

Why aren't we talking about this?
---------------------------------

* The experience of being a software maintainer is minimised or totally ignored. We tell them that this is how it's always been in open source - but it's now at a different scale. Why is it so hard for maintainers to talk about their experience?

* History: Free Software Movement - it's about user freedom, not popularity. Fundamental values were about protecting the needs of the user - and was unconcerned about the process of how software is produced. Rights of developers only matter as far as legal issues are concerned. "Some people create software sometimes" - if the maintainer can't keep up, the project dies and everyone moves on. Debian Social Contract (1997) - everything oriented at users - "we will be guided by the needs of our users"

* The Open Source movement - distinguished itself on benefits to users. Guided by need to get people using Open Source software. "The implication of the label is that we intend to convince the corporate world to adopt our methods" - focus on spreading the word, but not helping the creators. Software quality.

* Both Free Software and Open Source are oriented around the user, not producer.

* The maintainer's needs only exist in so far as they relate to the user's needs.

* It's not really about Free vs Open any more, but about needs of producers vs users.

The four producer freedoms
--------------------------

* The Four Freedoms - run the program, study the program, redistribute copies, distribute modified copies

* The freedom to decide who participates in your community

* The freedom to say no to contributions or requests

* The freedom to define priorities and policies of the project

* The freedom to step down or move on from a project, temporarily or permanently

* Users don't just use - they have to help themselves.

Other things maintainers need help with
---------------------------------------

* Community best practices - how many maintainers is enough? How do I get in new contributors? Establishing baselines for community health.

* Project analytics - metrics for project usage. Maintainers should have the right to obtain metrics about users. We should encourage them to do so.

* Tools and bots

* Conveying support status - currently, an odd assumption that maintainers are obliged to respond to every request into perpetuity. Maintainers should be able to say whether or not the project is currently accepting contributions, or to announce other limitations on project support. Limiting contributions on a project doesn't mean you can't fork.

* Finding funding - not everyone wants to get paid to work on open source. But those who do should have avenues to get that. Currently, sources of funding tend to be rather ad hoc - donation drives, corporate sponsorship. Open source is too important to rely on this. Some foundations have started grants but we still have a long way to go.

* Existential questions - what does it mean to be a maintainer? When can I go take a vacation? These will become more important as 1-2 maintainer projects become more important.

* We should have people becoming experts in this in the same way we have experts in open source licensing

People have been talking about this
-----------------------------------

* Steven Weber - "What is clearly missing from esr's statement, and is ultimately as important, is how those ["all bugs are shallow with..."] eyeballs are organised."

* Open source != Linux/Apache/etc. Linux has been successful and made open source look successful. We talked about this in the 1990s and early 2000s. Then social media, GitHub, content creation explosion happened. And then we stopped talking about how all these new projects are sustainable.

* People like to point to Linux as a success story of open source - not all the little projects. The struggles and challenges of those projects are being swept under the rug.

* We have to confront the awkward fact that open source today isn't like 20 years ago - and we have to make everyone know this so we can start figuring out solutions.

* This stuff is confusing! We're struggling to adapt economic systems to reflect change.

* But it's worth doing - we shouldn't ignore it or regress.

Questions
---------

* A lot of this explosion is enabled by GitHub. This puts GitHub in an interesting position to set expectations. What do you see as the responsibility/potential of an organisation like GitHub to do some of the things you've talked about here and use their power wisely? A: It's why I joined GitHub 6 months ago after I did all this research and concluded GitHub was the place for making these kinds of changes. GitHub should be supporting open source better through the product itself. The hardest thing about open source right now is the actual collaboration. For example, GitHub Issues could have more robust features for maintainers. Building community around maintainers and power users on GitHub - cf the big open letter. Give maintainers the tools they need to be successful to coordinate massive amounts of human effort.

* You said GitHub and SourceForge contain a mix of large healthy projects and small but crucial maintainers without many contributors/maintainers. Ive always been skeptical of those statistics - dominated by small projects that never actually got of the ground. How do you distinguish those categories and make more useful metrics about small projects that are actually important vs ones that really don't matter? A: Hearing more about the concept of dependencies - how many projects depend on this project? This is a better measure of importance rather than pure "popularity"/downloads/stars. Organisations trying to measure dependencies across every package manager. Hoping to have better metrics for stuff like this by the end of this year.

* It seems from a historical perspective that maintainers have lost authority and prestige. Attribute any of that to the rise of agile? It may be my bias but it seems like more of a culture of "I have an idea, next, next, next" with maintainers as obstacles rather than people with the larger global viewpoint on keeping the project functional. A: I share that bias. I didn't go into detail here partly because we don't have numbers about this, but coding doesn't mean what it did 20 years ago, many more people identifying as developers than they used to. People are just given tools and told they can do what they want with it - but under the hood, it's not just a hammer, there's a human being maintaining your things. "Open source has no respect for users" - do you know what's going on under the hood? I think there's a culture that will rebalance.

* Tension between a volunteer project crossing the line into getting funding - e.g. one person being paid while others are not. A: I think it's really hard. When I jumped into this it was all from a funding perspective. It's not as easy as just getting money. A lot is this governance question - if one person is being paid, do they have more responsibility? Django's model is one of my favourites - one paid fellow who does coordination work, others are doing types of work they want to do. Identify different roles for people being paid. I don't think it's been "worked out" yet, on a per-project basis you need to lay it out really transparently.

* A lot of examples in your talk are centred around stuff that's the same in the proprietary world - e.g. people complain/whinge about proprietary software companies. Have proprietary companies actually done this any better? Has anyone solved issues making things proprietary? A: Difference between proprietary and free software - in proprietary software people have to deal with users complaining, but they're being paid to do it. As a volunteer it's very different. Rehumanising the work that goes into producing open source. What is a user, what is a contributor? Are all users really potential contributors? To me everyone is a potential contributor. Proprietary software has only solved it by giving people salaries.

* I find it fascinating you bring in perception of open source by those outside the community. What can non-Linux projects do to improve that perception to help maintainers? A: Rust has done a really great job of humanising production work. Disconnect I see coming from VC/startup - if you talk about open source, "I love open source!" which means they love *using* it, not contributing. People from open source should come into talk to tech companies and non-open source meetups/conferences. Build relationships and connections between the open source ecosystem and users who don't understand what's going on.

* I have seen on Reddit a thread collecting Patreon links for OSS projects. Thoughts on that kind of funding model moving the decision on who should be funded down to the users? A: Recurring crowdfunding works when you have a very specific deliverable. Some major projects like Redux, vue.js funded through Patreon. Crowdfunding requires huge following, which is hard for people who don't want to promote themselves all the time. Aren't many examples of people raising tonnes of money. I think it works better for one-offs than recurring as people don't want to spend lots of money indefinitely. It's also unclear - when I've raised all these funds, what do I spend it on? My salary, organisation costs, hiring outside skills. If someone can find success that way it's great - I just don't think it'll work for every small project.
