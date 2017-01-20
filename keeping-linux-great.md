Keeping Linux great
===================

* Robert M Lefkowitz (@r0ml)

* Pia: "we don't have to carry the systems of the past into the future - challenge assumptions!" Dan: "Free licences aren't enough!" Nadia: "We have to think about uncomfortable issues!"

* What is the future of free and open source software? It has no future!

* We had this aspiration that we wanted to use technology to make the world a better place and liberalise access. There was a time when FLOSS was great for advancing that goal. We've had a good run - but it's coming to an end.

* All these ideas and techniques are based on how technology works today. How would free software have worked in the day of punch cards? The technology needs to be at a certain point to make a strategy work, and then new technology comes along necessitating new strategy

FLOSS is yesterday's gravy
--------------------------

* Tim O'Reilly, etc etc have been writing about this

* Open source is dead! The cloud has killed it! It's legacy! We won the battle for Linux, but we're losing the battle for freedom!

* Google Trends - the term "open source" peaked around in 2005-6 and has declined massively since. Same for "free software". Same for just the term "software" (even when software is eating the world)! Same for "Linux".

* cf Google Trends for Git vs SVN vs CVS. CVS in decline, SVN kills it. Git chugs along and after 2008 gets serious traction - 2010, it's in first. This matches up with our theory of the world.

* The term "app" is now more searched than "software"

* William of Ockham: the ontologists (categorisation and classification - everything in categories!) vs the nominalists (categories are a social construct! There are individual objects, with categories being constructs with no real existence that could be changed). Why would you create far too many categories that don't really exist?

* If everyone thinks that "software" and "apps" are different categories, then so they are

* ...Is Linux an app? If not, then it's some other thing

Lithification
-------------

* Unix philosophy - small pieces loosely joined. Composition with pipes. You didn't need to embed text searching into every application when you can pipe to grep.

* Sediment -> rocks: grains loosely packed -> compaction -> sedimentary rock.

* This is what's happening to software today vs when open source made sense.

* Zawinski's law: all programs expand until they can send mail! Those that cannot get replaced by those which can!

* Mobile operating systems - there are APIs between apps so that you can compose your web browser with Messenger, Facebook, mobile printing... you don't really need the source when you have an API

* I was working on this presentation on the way over and I wanted to add a reference to a book (photo of the cover), and while I have an app for my Mac, I wanted to switch to the tablet. No app for that - let's implement it with open source libraries! ...you don't need an external library for that, there's an API in the operating system for iOS and Android.

What is a Linux?
----------------

* Is Ubuntu Linux? Yes.

* Is Android Linux? Ish.

* Is Windows Linux? Linux is just a kernel, and there's a "Linux kernel" in Windows 10... so yes!

* Google Trends - Linux has declined, iOS and Android have grown massively. iOS and Android are better than Linux!

What should you do?
-------------------

* The "commercial Linuxes" are succeeding, the free Linux is failing.

* Vendoring dependencies - lithification - why not just copy all your libraries into your application? It feels dirty!

* Butler Lampson - Software components: only the giants survive - if you're going to use a component in your software as a dependency, it's not worth it unless the component is >5 million LOC. Anything less than that, you should just copy it into your source.

* In our new world software is bigger on one hand - more functionality at the lower level (facial recognition in your OS), and our apps are getting smaller (they can rely on new OS calls and APIs for other apps)

* Deployment environments are changing, e.g. AWS Lambda

* cf left-pad fiasco - adding dependencies for 11 lines of code is just unnecessarily creating fictitious entities!

* With practice you can do lithification and effectively reduce the number of dependencies

App freedom
-----------

* "We are conitinually faced with a series of great opportunities brilliantly disguised as insoluble problems"

* Bret Victor: advanced programming environments conflate the runtime with the devtime.

* Open source is specifically about preventing you from conflating runtime with devtime. The Free Software stuff is all about devtime, not runtime.

* The Four Freedoms don't account for this

* 1980s - Object Oriented Programming (see Brad Cox, OOP An Evolutionary Approach). You don't *need* to see the source code of the software you're modifying, because you can inherit and override. You can take their binary object and override its behaviours. No need to get the source and worry about copyright...

* The FSF, in the 1990s, sold its source code on CD-ROMs. They sold it for more if you wanted it compiled! They thought that charging $5000 for a compiled version of GNU was reasonable... but you could compile it yourself, on your own computer, and getting the source was the hard part.

* These days - most of your computing, Facebook, Google, happens in data centres. Even if you had the code, you can't modify it *elsewhere*.

* Computing is basically a non-rivalrous good

* We need part 2 of rms' printer story. The Razor VCS - you couldn't commit the software unless you had a defect number for the commit message. If you needed to, you could file your own ticket, and use that number. Now, let's say you want to do this on GitHub. You don't have the source code, and you don't have their server farm. Or... you could convince GitHub to let you push modifications to how GitHub works for you. Even if you didn't have their source and needed to reverse engineer their system, the important part here is having access to the runtime environment! The GPL doesn't help you here! The GPL doesn't care about what happens after you deploy the software in this context.

* Versioning a website - https://github.com/r0ml/mod_git. Set a cookie to specify which version of the website you'd like to ask for. "But you need to have access to the source code!" That's because of where we're at in development. Before we look into doing this for binary blobs, let's do it for source.

* "Liberal Software" Wouldn't it be great if there were a community of people who understood how computers worked and were committed to liberalising technology into the future with technical solutions? If such an organisation existed, what would we call it? Not "free software", people would think it's about free of cost... "Liberal software" sounds good!

End User Development
--------------------

* "Computer programming for everyone" - at the beginning of open source, everyone would learn how to code, the source code would be a great resource, etc - but now it's making a resurgence for another reason: jobs.

* The most successful programming environment of all time: Lotus 1-2-3, followed by Microsoft Excel. More people have solved more problems with spreadsheets than anything else.

* But open source people don't tell people to convert stuff to spreadsheets!

* 35% of needs could be addressed by a high-level development tool offering basic data collection/storage/retrieval.

* 40% satisfied by customisation of 5 generic web apps

* Only 25% of requests needed more advanced applications

* You need to make your software customisable by the end user - and most of the time, that customisation doesn't involve writing code.

* What if you could just ask Siri for an app to do whatever you want? Where's the source code? There's no source code! Just a giant neural net or something! As we conflate data vs program - free software people start seeing it as an invasion of privacy!

* Notions of copyright - to base this whole free software thing on copyright depends on copyright being a good idea.

* Once upon a time you couldn't copyright software - it was "mathematical". There were two notions of computing - mathematical and literary. The government mostly thought that it was mathematical.

* In the future - ubiquitous facilities for people to deploy stuff that they want. Some of us are going to have to build that.

* #liberalsoftware
