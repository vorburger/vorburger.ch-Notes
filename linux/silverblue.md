# Fedora Silverblue

## OSTree

After running e.g. `rpm-ostree install kitty`
as e.g. in [my `ostree-install-gui.sh`](https://github.com/vorburger/vorburger-dotfiles-bin-etc/blob/master/ostree-install-gui.sh),
`rpm-ostree status` shows what's currently running, and what it should boot into on the next `systemctl reboot`:

    $ rpm-ostree status
    State: idle
    Warning: failed to finalize previous deployment
             error: Bootloader write config: grub2-mkconfig: Child process exited with code 1
             check `journalctl -b -1 -u ostree-finalize-staged.service`
    Deployments:
    ● fedora:fedora/36/x86_64/silverblue
                      Version: 36.1.5 (2022-05-04T18:42:06Z)

But beware that that _Warning_ on top means that `install` won't work!  The following workaround did the trick for me:

    find /boot/efi -exec touch '{}' ';'
    sudo ostree admin finalize-staged
    systemctl reboot

_This has to be re-run after EVERY `rpm-ostree install`, BEFORE the next reboot!_ This is a known bug:

* https://github.com/coreos/rpm-ostree/issues/3925
* https://bugzilla.redhat.com/show_bug.cgi?id=2096192
* https://bugzilla.redhat.com/show_bug.cgi?id=2118172
* https://ask.fedoraproject.org/t/fedora-silverblue-36-will-not-succesfully-deploy-after-layering-packages/25352/13
* https://discussion.fedoraproject.org/t/rpm-ostree-updates-broken-cant-rollback-grub2-mkconfig-erroring-out/33982



## Flatpack

As per https://github.com/vorburger/vorburger-dotfiles-bin-etc#on-fedora-silverblue,
Apps from Flatpack will have certain limitations, such as that:

* [YK SK won't work in Brave](https://github.com/flathub/com.brave.Browser/issues/126).
