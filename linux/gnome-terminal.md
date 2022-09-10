# How to launch a CLI tool in a new terminal window

Here is one way to run a CLI process in a new window, make closing
that window kill that process, yet wait for the user when said process
exits, e.g. not to loose some start-up error message:

    gnome-terminal -- bash -c 'ls / ; read -p "Press Enter to close..."'

I used this for `qemu-system-x86_64` [in this script](https://github.com/vorburger/LearningLinux/blob/f4fdd79723d198b445665d613a4ed7a5ecc6cca1/run), but the approach is general, of course.  BTW, note the `export` - that's needed for those environment variables to work as arguments to the process in the gnome-terminal.

    export KERNEL
    gnome-terminal -- bash -c '\
      qemu-system-x86_64 \
        (...)
        -kernel "$KERNEL" ; \
      read -p "Hi, $IMAGE QEMU has exited - press any key to close this window..."'

PS: I'm using this instead of QEMU own GUI Window because the VM I'm launching does not initialize a console and I needed to see it's Kernel's output to the serial bus.
