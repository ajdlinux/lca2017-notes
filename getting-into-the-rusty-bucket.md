Getting into the Rusty Bucket: Lessons from Integrating Rust with Existing C
============================================================================

* William Brown (@Firstyear)

Directory Server
----------------

* Directory Server - an ancient but critical codebase.

* It has a plugin architecture to add functionality! Plugins are called during the request-response flow at various points to filter things, log things, modify things...

* A DS plugin is a .so object that gets dlopen()ed. Passed a pointer to a very fat struct (parameter block = "pblock") in every operation which describes the request. The plugin registers callbacks for hooks.

* 20 years of history, very stable, but ~500,000 lines of C that can't be easily reasoned about. You keep chasing your tail when you're trying to track down bugs. This isn't sustainable.

Rust
----

* Why is Rust good? Type safe, memory safe, compiler-checked. No null pointer dereferences, mandatory error handling, no memory leaks.

* The type system provides certain guarantees that will help your program not blow up.

* Mozilla is using this in Firefox - their current uses for Rust have already fixed bugs in their existing C++ implementations.

* But we don't want to rewrite the whole product - how can we combine the safety of Rust with an existing C codebase?

* There is a tool called corrode to convert C to Rust - but we can't just pipe 500KLOC of code into it. We can't exactly trust the output.

* It takes a lot of time and energy - this is rather unknown territory

* Think about support - you need to have your team and company on board, you can't just start using it in many projects - but don't let that discourage you

Linking Rust <-> C
------------------

* Cargo.toml - add `links = ["slapd"]` and you link to slapd!

* Add `crate-type = ["dylib"]` in there, and you get a .so

Types
-----

* The hard part is making the types match. How do you get a pblock struct in there? How do you register your Rust function callbacks?

* Define constants from C header files - this is tedious and boring, but doable.

* Define structs... this is *very* painful - doable, but tricky. It's easier to take it a pointer as a `c_void`, don't access its members in the Rust code, and pass it back to C to access or modify the members.

Symbols
-------

* Rust mangles symbol names - that change every single compilation. This isn't going to work.

* The `#[no_mangle]` directive on a function cleans everything up and disables mangling! But be careful not to `#[no_mangle]` two things with the same name.

Function pointers/callbacks
---------------------------

* We need to cast some stuff around

* Pull in the `libc` crate, and you can cast any function into a `c_void` pointer - this just works!

* To register the callback, pull in a function from the C library using an `extern` block and make a function signature. Call the function from within an `unsafe` block. Be aware that within an unsafe block you don't get safety guarantees and you could segfault.

Debugging
---------

* You can use `gdb` with Rust, there are caveats. You can't break on Rust symbol names - you have to break on line number. `gdb` doesn't know how to interpret Rust symbol names. There is a module for `lldb` that may support this.

* Once you do break, `gdb` works pretty well.

* You are going to get segfaults - `gdb` will help you!

Improving the design
--------------------

* Need to minimise calls to C, and reduce the `unsafe`.

* Less usage of `c_void` etc - we want clean Rust types.

* Also want traits to enforce plugin behaviour.

* We wrote a middle layer in Rust to be the plugin callback target for DS, that wraps everything in nice Rust types then calls the real Rust plugin that doesn't need to call any unsafe code directly.

* Small piece of unsafe code to set function pointers that can be extensively tested.

* The code that handles a callback takes a `c_void`, creates a pblock struct in Rust, calls the real callback, handles a Result type from the real callback, and returns it.

* You need to write "functional C" that takes context pointers - you can't really be using struct members.

* Use traits so you can gradually rewrite more in Rust underneath

* One problem - you still can't initialise/register a plugin through the middle layer. Oh no, more unsafe code in the plugin! But we can define a macro that wraps the plugin, checks that it has the right traits, and calls the register function. Use Rust macros to hide ugly details and ensure type checking.

Build systems
-------------

* Autotools... because it's the de facto standard, and it can do some things that Cargo just can't do. Cargo can't even install a shared library.

* In `configure.ac`, check if cargo and rustc are there, and tell Cargo to build partial objects and emit them. Use autotools' libtool to finish the linking and generate the archive. This ensures it's versioned correctly.

What next?
----------

* We need to write more bindings for libraries (e.g. NSPR, NSS)

* Rewrite existing critical paths in Rust, like password hashing. Especially really security sensitive stuff.

* Rust ASN.1 parser for BER - this is needed for X.509

* New plugins in Rust

* Need more companies on board with Rust and distros distributing it

Questions
---------

* From the testing side of things, did you look at using tools to test stuff without needing a full LDAP server? A: You need a full LDAP server for many things. Really difficult to test - that's not a Rust problem though, that's a DS problem.

* Any work on Corrode to generate C header shims? A: I believe there are some projects working on that, not sure about Corrode

* Testing(???), embed some native calls into Rust? A: I think there are some creative ways to get around things, you could have a Rust test that calls into C that calls into Rust. They haven't done much when it comes to integrating unit tests for this

* Since you have control of C side of DS and its process of linking to external lbrary, anything you can do on C side to make it easier? A: Yes, you need some of the C things there so Rust can use them. I've written a similar binding for SNMP, the API there is not very friendly for external bindings. There's a lot of things you can do to make it easier for external bindings

* Can you access C struct members directly if you can prove it's the same as the Rust struct? A: You can define a struct in Rust and label it as a C type, and you can just straight out use it as long as you assert that the Rust struct is the same as the C struct - but for DS we couldn't do that since the struct is huge. For smaller projects, then go ahead.

* There is a tool called bindgen that can take header files and generate struct definitions.

* Overheads of shim layer? A: Didn't notice. DS is stupidly fast. Rust compiles to native code. The reason for the overhead is architectural.
