# How to launch a CLI tool in a new terminal window

Here is one way to run a CLI process in a new window, make closing
that window kill that process, yet wait for the user when said process
exits, e.g. not to loose some start-up error message:

```bash
gnome-terminal -- bash -c 'ls / ; read -p "Press Enter to close..."'
```

I used this as follows for `qemu-system-x86_64` [in this script](https://github.com/vorburger/LearningLinux/blob/f4fdd79723d198b445665d613a4ed7a5ecc6cca1/run):

```bash
export KERNEL
gnome-terminal -- bash -c '\
  qemu-system-x86_64 \
    (...)
    -kernel "$KERNEL" ; \
  read -p "QEMU has exited - press any key to close this window..."'
```

Note the `export` - that's needed for those environment variables to work as arguments to the process in the gnome-terminal.

PS: I'm using this to see a VM's output to the serial bus, for which I cannot use QEMU own GUI Window because a particulat VM I'm launching does not initialize a console. Of course you could also keep it in your main working console, but because the VM is "long running" in the background, it's "in the way" if it's in the foreground.
