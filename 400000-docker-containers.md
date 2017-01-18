400,000 Ephemeral Containers
============================

I'm not taking notes about the actual talk here, I work at IBM and thus already know everything in this talk. Go watch the recording.

Questions
---------

* You mentioned that you don't do any dependency graph work? Have you thought about adding that? A: It's funny you should ask... the next step for this is CI, running this all the time, and it takes too long to run over the whole thing. Reverse dependency graph, flatten it into a list. We can then work out which other packages are affected by a change to a particular package. Can poll the npm database for changes and then just work out packages affected by that change.

* Since you're running this on one machine, have you run into issues when you're running >1000 containers? A: The containers aren't all running simultaneously. `parallel` enforces running 30 containers simultaneously. Running 10,000 simultaenous containers would break in fun and exciting ways.

* What does the RAM usage end up being for 30 concurrent containers, inc temporary files? A: That's an excellent question. In ruby, it's not too bad, in node.js it has a very inefficient way of storing files and can blow through 128 GB of RAM for RAM disks...

* Any reason why you didn't think about having a local repository rather than caching? A: Yes - I looked into it, but it would mean allocating virtual disks for my VM (and I'm lazy) and it's more complex.
