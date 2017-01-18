* Computing is all about caching
* 1 cache miss every 10 instructions will halve your CPU speed...
* Obviously even worse the further down the storage hierarchy you go


Buffer cache
------------

* Caches disk blocks
* Has always existed in Unix (even in 1975)
* Index by (disk, block number)

Page cache
----------

* read() -> VFS -> page cache -> filesystem -> buffer cache -> device driver
* Introduced in Linux in 1.3.50 (Dec 1995)
* Sits between VFS and file system - if it's in the page cache, we don't even need to go to the fs level
* Index by (file, page number)
* Now page and buffer cache are unified (1999). Buffer cache interface still exists, but data held in page cache
* A whole bunch of page cache operations (find, create, lock, remove, replace, read, write, tag, etc etc etc)

Locking
-------

* Handles locking internally
* When changing what things are cache it holds per-file spinlock
* For lookups, use RCU
* Locked bit to prevent other users seeing a page while it's being read from media (security!)

LRU
---

* Active vs inactive list - active = used multiple times, inactive = only used once. Pages are promoted from inactive to active. Bottom of active list demoted to inactive list
* Better predictions when inactive list is longer
* When a page ages out from the cache, it's replaced with a "shadow entry" - which basically says "this should be on the active list but we don't have enough RAM" (due to memory pressure). When we fault on it again, we load the page straight into the active list to give it a better chance of hanging around (I think I have this right???)

Huge Pages
----------

* THP - originally used for anonymous memory (regular malloc()ed memory that's private to you, and doesn't get involved with the page cache at all)
* Large, terabyte-sized files - using huge TLB entries would be great
* A first attempt to support hugepages in the page cache is available - adds 512 entries to the page cache... but that's a bit silly
* We've enhanced the radix tree to store "multiorder" entries - pending patches so that we can add a single order-9 entry to the page cache

Do we still need a page cache
-----------------------------

* Dave Chinner - "we're rapidly moving away from the world where a page cache is needed"
* Linus - "removing page cache bad for both performance and stability"
* DAX - didn't use the page cache... and that's wrong.
* Don't want to have filesystem code in the critical path - it's a locking disaster

Future enhancements
-------------------

* Improved hugepage support
* Filesystems blocksize > pagesize
* PFNs instead of/as well as pages
* Huge swap entries
* Swap to pmem?

Questions
---------

* Swapping to pmem - for suitably large machines, do you end up with a hierarchy of swap where you eventually end up on a SSD or spinning rust? A: Yes, that may make sense. But do people want to use computers that way? What kind of machines are being designed? Somebody will try it - we won't know if it's a good idea until we try it.
* How does this fit with the use of pmem as long term (file system) storage? A: At the moment, DAX bypasses page cache entirely. Hopefully we'll be able to intermix page cache pages from a slower device and persistent memory entries in the page cache
* Swap system. A: Hadn't gotten much love because if you've started swapping you've already lost. But that's not really true, especially now with SSDs. People making lots of effort to improve swap.
* Huge pages that are too huge to put to swap in a reasonable amount of time (e.g. 16GB pages) - A: Linus has come out in favour of basically never doing pages that big. It never makes sense to do a single I/O >4GB. Page cache is limited to PMD level. No work to get to PGD/PUD level. Not looking at the 16GB problem yet, at that size it makes sense to fragment. But this could change over time.
* When you have pages of vastly different size, how do you decide what to (?)omit? A: I don't have to answer it! I'm not a mm hacker... no sense to weight a 2MB page same as 4kb page, not sure what it makes sense to do. Once we have infrastructure in place, the people who are good at this kind of work will be in a position to do that kind of work and find out what heurisitcs work best
* We still haev the assumption that we treat anonymous and file pages differently, why? A: Because they are different. Cost of swapping out anonymous pages > cost of just discarding file backed page. Used for different purposes, it makes sense to treat them as completely different. Doesn't lead to best utilisation of hardware but heuristics work out to make it better to treat them completely separately

Lockless page cache protocol
----------------------------

Reader - find page in radix tree, conditional increment refcount if not zero, if successful check if page still in page cache (by checking if it's still assigned to page cache instance). Now during this time pages may have been assigned somewhere else.

Writer - atomically check-and-zero refcount - check against an expected value they already know, if it's not that, then there's a reader in there. Remove page from page cache - set mapping pointer to something else (zero). Free page.

This has been checked exhaustively for all orderings of events!
