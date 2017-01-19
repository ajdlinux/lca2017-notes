/sys/class/gpio/Parking Brake: Hacking my car
=============================================

* Joel Stanley (@shenki)

The Mazda CX-5
--------------

* Mazda CX-5 - contains MZD Connect entertainment unit

* Connects to home WiFi...

* nmap the device - it's running Linux! Port 22 open... but required a SSH cipher downgrade.

* Search up the details on the internet - find that every Mazda vehicle uses a 3-character password.

Embedded Linux
--------------

* Embedded Linux system attached to Canbus, bluetooth and wifi. 2 USB ports. NXP i.MX6, kernel 3.0.35 (from 2011). (You should all use upstream kernels for embedded! But this board only gets upstream support in 3.2) 1 GB RAM, 512 MB flash

* Userspace - Opera web browser on top of Wayland, UI probably HTML + JS. Various interesting applications, including a DVD player - which isn't actually used? Uses OpenCar SDK

* Embedded Linux systems often built with tools like Yocto or Buildroot

* They were shipping OpenSSL 0.9.8k - post Heartbleed, but not by much. Plenty of security fixes since. It's not serving pages to the internet or anything, but still not great.

* GPL compliance? System is built by a 3rd party, "JCI". No written offer to acquiresource, per GPL - there's a URL listed deep in the interface that gives licence information, but completely omits the kernel, BusyBox etc.

* Terminal app!

* Some months later - I tried to ssh into it again and it failed. The car had gone back in for servicing... and they upgraded the firmware. (Aside: what are your rights as a consumer?)

* But not only did it have WiFi, it also has USB ports. A bit more research revealed that if I plugged a USB Ethernet dongle in there it would load the driver...

* A PDF on the internet suggested there's a diagnostic tool that can be placed on a USB key and run some software to get diagnostic data on to the drive. It conveniently has tools to allow you to run anything you want as root! Load it up with some scripts... It helpfully popped up "service complete" when it was done!

* ssh had now been set to key based authentication on port 3600. iptables denying most incoming connections. Used script to open iptables. But ssh authorised keys file was on a read only squashfs filesystem which made things harder.

User freedom
------------

* There was a ticket number next to all the changes in the config files with the firmware update. Someone at Mazda had asked for this to be done through a change control process.

* They had been responsible enough to patch some issues - but could they have done it better? Over the air rather than through a dealer service?

* The downside is that they can disable features.

* User modifiable systems that respect user freedoms but also resist malicious intruders is a challenge.

* Can you add your own keys to a root of trust device?

GPIO
----

* GPIO is "general purpose I/O". The kernel + GPIO framework exposes files in "/sys" for this.

* Normally, these files are just a mysterious numeric name with no information - but this car had useful labels

* Including... /sys/class/gpio/Parking Brake! An electric control parking brake!

* This is probably related to car safety regulations - specific functions have to be disabled when the parking brake is on or off.

* Fortunately, it's read only...

Future work
-----------

* Make it easier to modify without touching root fs

* Log GPS/engine data

* Port OpenStreetMap

* Investigate security of CAN bus bridge

Questions
---------

* I need to apologise... I made a source code request for my Mazda, which is probably why you no longer have a shell. Did you do a source code request? A: I haven't pursued it, there is another Australian blogger who has tried and failed.

* Did you get a full NAND dump? A: Not a full dump. There are multiple flash chips for primary and failsafe boot.

* Does your car speak HTTP? There's a video around of a car that speaks HTTP to the manufacturer that you can query. A: Not as far as I know

* Have you tried any other hacks against stuff you can interact with? A: Everything inside is writable, it's just how much of a risk I want to take.

* If you put a DVD in and you have a shell, it will actually work - it's enabled in Japan, but disabled in countries that restrict it like Australia. It will actually just play anything you plug in. The hack I want to do is enabling the reversing camera while driving. A: People want to get Android mirroring so they can use Google Maps.

* You mentioned a USB Ethernet adaptor - have you thought of the possibilities of a USB 4G dongle? A: That would be fun!

* All the multimedia stuff is based on GStreamer - I recall the security vulnerability in the NES audio format, have you tried attacking GStreamer at all? A: This was my thoughts but I had root straight away...

* I also bought a car, which has aftermarket firmwares available - do you have any thoughts about responsibility of users for stuff like watching DVDs while driving? A: Would be nice to have a few more features. It's a tough one.

* You have the CAN interface - does that lead to the ECU for performance things? A: Someone has done investigation here, it apparently has a "hardware firewall"...

* Would it be possible to buy one of these boards for spare parts? A: Probably expensive. You could get one from a smashed up vehicle from a repairer.

* Opera web UI - there's a flag to allow you to modify code. Have you managed to get the web UI running in non-car environment? A: Basic JS/HTML fine, but most of what it does is launch processes in the background.

* Following on from 4G dongle, could you find your car if you lose it with dongle and GPS? (Is there potential for manufacturer to find the car if they wanted to?) A: That'd be cool.

* Potential work to look at engine data - what's exposed? A: I haven't explored, probably similar to OBD. Mazdas have auto-idle at traffic lights... Would be cool to do more logging with GPS + engine data.

* When you got the update - did you notice any feature improvements or bugfixes? A: Nothing I noticed, I don't drive it very often

* Did you try a USB keyboard? A: No, but it would be a good idea

* Aside from fixes and improvements, there are regressions to look for. Lack of control - we can't report bugs against this.

* What's your contingency if you brick it? A: Being very careful!
