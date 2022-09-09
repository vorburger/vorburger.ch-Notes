# Linux Kernel Random Number Entropy

The Linux Kernel uses _"entropy"_ to generate random numbers:

    $ cat /proc/sys/kernel/random/entropy_avail
    256

    $ cat /proc/sys/kernel/random/poolsize
    256

    $ cat /proc/sys/kernel/random/write_wakeup_threshold
    256

This 256 seems to [have recently changed](https://unix.stackexchange.com/questions/704737/kernel-5-10-119-caused-the-values-of-proc-sys-kernel-random-entropy-avail-and-p).

Older blog posts state [that 256 available entropy is too low](https://major.io/2007/07/01/check-available-entropy-in-linux/).

I doubt that on modern 2022 Kernels, such as a 5.17 from Fedora 34 or a 5.18 in Fedora 36, this is still accurate.  It now actually remains at 256 forever, even with keyboard and mouse and disk events; even a restart does not budge it.


## References

* https://wiki.archlinux.org/title/Rng-tools
* https://blog.pcfe.net/hugo/posts/2011-11-04-tpm-to-feed-random-number-generator/
* https://github.com/jirka-h/haveged, note the _IMPORTANT UPDATE_
* https://wiki.alpinelinux.org/wiki/Entropy_and_randomness
* https://wiki.qemu.org/Features/VirtIORNG
* https://fedoraproject.org/wiki/Features/Virtio_RNG
* https://wiki.archlinux.org/title/Random_number_generation
