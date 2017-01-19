Rewriting MMU for fun and profit
================================

* Aneesh Kumar K. V., IBM Linux Technology Centre

Overview of Power
-----------------

* Introduced in ~1990 with RS/6000

* Performance Optimization with Enhanced RISC

* PowerPC (1993) with Motorola/Apple

* POWER3 (64-bit), POWER4 (64-bit > 1 GHz, dual core, hypervisor), POWER5 (SMT, hypervisor), POWER6 (high frequency), POWER7 (balanced multicore, accelerators),POWER8 (CAPI, OpenPOWER)

Power memory management
-----------------------

* Desktop/server (Book 3S)

* Embedded (Book 3E) - not talking about this

* Hashed page table structure

* Power uses the terminology "Effective Address" -> "Virtual Address" -> "Real Address", the term "virtual address" means something different in Power, EA is closer to the usual meaning.

* EA is converted into a hash value using a hash page table structure, hash value used to look up real address

* One HPT for all process + kernel - this is remarkably different from other architectures which have multiple page tables

* Segment table used to translate 64-bit EA space to 78-bit VA space

* Embedded - software loaded TLB, handled by the operating system when you get a TLB miss, PTE format varies between implementation


POWER8 MMU
----------

* Uses hash page table

* Page sizes: 4K, 64K, 16M and 16G

* 256 MB and 1 TB segments

* Multiple page sizes per segment - e.g. 4k/64k. There are some implications from this.

* Linux uses an abstracted MMU model in most of the kernel, with a multi-level page table. The architecture must map the Linux model to the real hardware.

* 3 level Linux page table supplied to the generic Linux code, used by core mm routines

* We map this to a hardware hash page table structure that is used by the CPU

* This leads to complex PTE update rules as we have to keep both in sync

* EA -> SLB lookup -> Virtual Address -> TLB lookup -> Page table lookup -> Real Address

* Segment table is one per process. Translates Effective Segment ID to Virtual Segment ID.

* 78-bit VA is effectively a Virtual Page Number + Byte Offset (it's a VSID + page number + offset)

* Hash table address stored in SDR1 register. With each hash value there are 8 buckets. Take Virtual Page Number, run through a hash function, get a hash value which is in one of the 8 buckets. Find the slot (you have to search a bit to find it). The slot contains Real Page Number.

* Take the Real Page Number and Byte Offset and you get real address.

* Hash slot tracking - the hash value doesn't tell you the exact slot, just the group. The hypercalls in PAPR for invalidating things expect an exact slot, not just a hash or group. To use these hypercalls you have to track the exact slot number. We track the hash slot information for each faulted page in the Linux PTE entry.

* Hash slot tracking made trickier by the fact that our default page size is 64K, one slot for 64K, but some generations of our hardware only support 4K pages in hardware. How do you do a 64K Linux page with 4K hardware pages? We essentially map 16 4K pages per Linux page. This requires 16 slot entries in the HPT. This means we have to track 16 slots in the Linux PTE. Due to limitations in the Linux PTE entry size we track the slot valid details in the PTE entry and we track the actual slot number in second half of the level 3 page table.

* The Linux page table format on Power is a completely software defined entry that needs hacks to support all these different things

POWER9 MMU
----------

* Supports two translation modes - hash and radix

* Radix page table: 4k, 64k, 2M and 1G page size, guest managed TLB, Linux and hardware using same page table structure, big endian format

* Guests have translation caches and ability to invalidate translations

* Hardware structure matches very closely with Linux

* Radix: EA -> Lookup in process-scoped page table -> Guest RA -> Lookup in partition(VM guest)-scoped page table -> real address

* Compare to Hash: EA -> SLB Lookup -> VA -> TLB lookup -> Page table lookup -> Real address

* PTCR - Partition Table Control Register. Partition table has 128 bit entry for every guest with a pointer to base of the process- and partition-scoped tables. Index into partition table is partition ID, index into process table is the hardware (not Linux) process ID.

* Split Effective Page Number into 4 chunks - offset in PGD, PUD, PMD, PTE - get real page number.

Challenges
----------

* We want a single kernel to boot on POWER8 Little Endian as well as POWER9, supporting both hash (on both P8 and P9) and radix (on P9). This is particularly hard as mm code has lots of hot paths where you can't just introduce conditionals.

* Rework the Linux PTE format to be the same on both P8 and P9. No free bits in the old PTE format - don't track the individual slot valid info, freeing 11 bits.

* Switch POWER8 to use a 4 level table like P9 does. Fix up all the hugepage allocation routines etc (note that P8/P9 hugepage sizes are different)

* Endianness - P9 must be big endian (hardware defined), P8 uses regular endianness (because it's only used for other Linux code). Switch P8 to big-endian. Fix up the hot path to avoid the overhead of converting from big endian to native - if comparing against a constant, rather than converting the page table data to anative endian, convert the constant to big endian at compile time!

* There is some conditional code that is unavoidable - use static keys and hot patching to optimise (CPU feature table)

* Supporting the 4K Linux page size - we can't do Transparent HugePage on P8 with 4K page size, because it falls at the wrong level (not PMD). We use a "hugepage directory" to support 16MB hugepages. 4K on P9 is way simpler and we can do THP with 2M hugepages at the PMD level.

* The kernel ultimately supports 3 translation modes - SDR1 based hash (P8), PTCR based hash (P9) and radix (P9) all of which need to be determined at runtime

Status
------

* Most code upstream in 4.8

* Hash KVM (running guest in hash mode) upstream in 4.9

* Radix KVM (running guest in radix mode) will be upstream in 4.11

Questions
---------

* Why did hugepages shrink from 16M to 2M? A: Efficient 4K and hugepage support - 2M makes it easy to do hygepage at PMD level. While we're updating the page table, we have to hold various types of lock. Switching to 2M makes this locking more efficient (*technical details*) This gives us a challenge on the 64K page size (*technical details*)

* Traditionally page size was 64K, is that still the case? A: Linux page size (page size with which we build the kernel) vs "base page size" (Power-specific concept) that is used by the hash function and is determined by hardware. Base page size on hash remains 64K (with exceptions), Linux page size remains at 64K. We don't have a concept of base page size for radix, but Linux page size remains at 64K for radix.

* Power advantage of using 64K page size to make I/O faster than Intel 4K size, is that still preserved? A: Yes

* Implementing the new page table format, you converted the Linux page table from 3 to 4 level, did that result in performance penalties on P8? A: We have done some measurements and haven't found observable impact. We don't really have a choice. We don't want to switch between a 3 and 4 level table at run time.

* Did you need to make other changes for processors older than POWER8? A: The part of the code we changed for the software defined Linux page table structure works from POWER4 onwards. We haven't touched the embedded ("no hash") side at all - we don't want to impact their optimisations.

