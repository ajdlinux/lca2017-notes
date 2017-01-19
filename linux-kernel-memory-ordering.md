Linux Kernel Memory Ordering: Help Arrives At Last
==================================================

* Paul McKenney, IBM Linux Technology Centre

* This is a paulmck memory ordering talk. Watch the recording. All the interesting technical details are too hard to write down.

Who cares about memory models?
------------------------------

* Memory models are a formalism, we're practical people in the kernel, what's the point?

* Litmus test (litmus/manual/extra/sb+o-o+o-o.litmus) - you can express multiple threads' memory access patterns and an "exists" clause to determine if a particular thing could happen.

* A tool that can tell you how things work in the Linux kernel would be very useful. Tools like this for individual architectures (x86, Power, ARM, etc) exist. Useful design aid for verifying bugs and fixes (IBM has used it for Power). People porting Linux to new hardware with new memory models.

* This memory model stuff could help with building other tools like thread sanitiser stuff from Google

Tooling
-------

* Right now, we have memory-barriers.txt... no-one submits patches to it...

* A Linux kernel memory model tool - useful for a small amount of core critical code, not for the whole kernel.

* memory-barriers.txt isn't a formal model, it's a how-to guide. Many corner cases unexplored - you ask people and they fix it when they see it. No mathematical completeness.

* Expanding the memory-barriers.txt model to be more complete - there are guiding principles (see slides)

History
-------

* For a long time, very academic.

* 2005 - C/C++ model in the standard draft

* 2009 - x86/Power/ARM

* 2014 - a clear need for Linux memory model... but... lots of work, legacy code, unmarked accesses, moving target. A model that gives you an intersection of all the guarantees on different architectures.

* Early 2015 - it's happening! Jade Alglave at UCL

A realistic strategy
--------------------

* Ignore legacy code, unmarked accesses. The maintainer of the legacy code has to fix it!

* Solution is now feasible - don't need to model every possible compiler optimisation...

* Use the `cat` language and the `herd` tool for litmus tests.

* paulmck recruited to clarify memory-barriers.txt and to add RCU to the model. A few existing RCU formalisations... but it turns out they were incomplete.

* An interview process of writing litmus tests, and asking what the result to be - use the answer to adjust the model (example: C-RW-R+RW-G+RW-R.litmus)

* RCU synchronize_rcu() guarantee can be broken on weak CPUs in problematic ways. herd tool can find this.

* Writeup of RCU behaviour - general rule: If there are at least as many grace periods as read-side critical sections in a given cycle, then that cycle is forbidden. "Principled", but... "difficult to represent as a formal memory model"

* Jade produced the first feasible model of the Linux memory model and also forced Paul to improve his RCU understanding...

RCU modelling
-------------

* Expressing RCU in litmus tests. Need use of stuff like "prophecy variables"

* Automation to generate litmus tests using scripts and expected outcome. Currently at 2,722 auto-generated tests + 348 manually written tests. Another tool to generate 1,879 more tools. Validation is critically important!

* Does the model actually match the hardware? In models and in real hardware implementation? Who's going to run the tests?

* Luc produced a model with RCU support

Real world testing
------------------

* Script to convert litmus test to a Linux kernel module, add types etc, and then run it on x86, ARM, Power

* Add support for "C-like" litmus test code

* Infrastructure to publish test results on the web

* Alan Stern (USB maintainer) then rewrote the model...

* Handles multiple and nested critical sections, and report errors on unbalanced locking

* Handles arbitrary critical section/grace period combinations

* Model is ~200 lines of code

* *complex technical details*

Questions
---------

* Validation tool that returned sometimes response - is that iterating over a bunch of CPU models, or just all the states from the memory model? A: It's the latter - the memory models have been very carefully constructed so that someothing is only forbidden if all CPUs forbid it. If any CPU says it can happen then it can happen. Not just the hardware, but also compiler and arch code. The model has to handle everything.

* Right towards the end you said there was a particular relation you can look at when writing code with lighter-weight barriers. A: *complex technical details*
