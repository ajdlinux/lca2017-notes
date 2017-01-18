
The kernel project
------------------

* The kernel is big - 60K+ files, 4k+ directories, 63-day release cycle, 12,000 commits per release
* Huge and fast moving
* >90% of code is written by paid developers
* Nobody is paid to write kernel documentation - a few paid devs work on it on the side, but no-one actually has a job working in kernel documentation. This is common across free software
* Kernel has a well-defined maintainer model. The maintainer model matches the kernel file hierarchy pretty well. This helps keep maintainers stepping on each other pretty well.
* Documentation doesn't fit this file-hierarchy model. Everyone touches Documentation/ and lots of documentation falls outside.
* Being the docs maintainer is challenging.

Challenges
----------

* Kernel developers are conservative people who don't like change, new tools, big dependencies
* As such, it's a hard sell to introduce new tools/methods


What do we have now
-------------------

* Documentation directory. Excluding device tree, 2,264 files, 228 directories, 23MB of material
* Some utilities in scripts/, arguably samples/
* Lots of plain text, some are nice and current and comprehensive... some are... not.
* DocBook. 34 template files rendered into PDF, HTML etc
* Kerneldoc comments scattered throughout the source - doxygen-esque. Around 55,000 throughout the kernel
* The kernel has lots of documentation - just not quite what we want it to be
* Information architecture... there is none. There is no overall vision. Stuff just gets added.
* grep is just not an information architecture tool!
* The root of Documentation/ has tonnes of files, memory-barriers.txt is really useful, zorro.txt less so
* No cross-document linkage. Few references at all
* Keeping documentation in the source code does have some advantages
* DocBook allows somewhat integrated documents... to a point
* Kerneldoc - It's a documentation system written by kernel developers! It... kinda works sort of. scripts/docproc... slow, brittle, hard to set up, ugly output, no formatting in kernel doc comments, no integration with the rest of Documentation/

How can we make things better
-----------------------------

* Basic requirements: clean up the mess, better formatted output, more rational toolchain, preserve or enhance plain text access
* Making it harder to write documentation than it already is will be a very hard sell...
* Initial work - adding markdown processing to kerneldoc comments (Daniel Vetter etc) - this was later switched to asciidoc. More documentation in the source, less ugly DocBook. But... adding new dependencies (inc Ruby) to the existing brittle toolchain. Lots of integration problems. Really slow. Still no linkage between documents.
* What Jon wanted - get rid of DocBook, simple markup processor for everything... but, working solutions (the asciidoc patches) trump plans
* Then... Sphinx! reStructuredText! Designed for code documentation from the very beginning! Designed for large documents with multiple files! Widely used! Well supported! Multiple outputs!
* Proof of concept posted... if there's a job that needs doing, post a really bad solution and wait for someone else to make it good! Jani Nikula refined it. Consensus formed quickly.
* Kerneldoc comments still work as they always did - no need to change all 55,000 comments. But the comments can now contain RST!
* Cross document references
* Function/struct indexes
* Nicer output
* Simpler and much faster build process
* To make it work - place an RST document in Documentation/, add a ref in index.rst, done!

Current status
--------------

* Merged in 4.8 - 20% of 4.8 was docs updates!
* Docs HOWTO, DRM and media subsystems are already using RST pretty well!
* 4.9 - driver API book converted from DocBook + text files. developer tools book, fixed PDF output
* 4.10 - process book (CodingStyle, SubmittingPatches, ManagementStyle...), core-api book, admin-guide book

Future work
-----------

* Convert the rest of the DocBook, eliminate DocBook
* Get rid of kerneldoc - 20 years of perl!

Questions
---------

* Coverage - Rob Landley had scripts to figure out e.g. which syscalls have docs and which ones don't. How do you automate which kerneldoc comments are never used? A: Would be good to have...

* Matthew Wilcox - largest problems we have: devs don't know who their audience is. Sysadmins, API users, interesting miscellanea, people hacking on it... mechanisms for splitting that out? A: By hand... deleting old docs is rather hard.

* The Sphinx docs look underdone - started but dropped. Looks like it's improving. Wondering about people who are looking at stuff like the cmdline options list. A: we're leaving pointers

* What should other maintainers be doing? A: Imposing requirements is rather hard. But we can apply pressure. Most maintainers don't insist of documentation, but we could be hopeful...

* Joel Stanley: device tree does force documentation. DTB - are they going to be sphinxified? A: No. Long term plan to move out of the kernel.

* Flaming/bikeshedding? A: Relatively little trouble. Biggest problem is deleting stuff.


