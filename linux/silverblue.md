# Fedora Silverblue

See also [remaining ToDo](https://github.com/vorburger/Notes/blob/master/ToDo/LOW-PRIORITY/silverblue.md) (private).


## OSTree

    rpm-ostree install kitty
    rpm-ostree status
    systemctl reboot


## Flatpack

As per https://github.com/vorburger/vorburger-dotfiles-bin-etc#on-fedora-silverblue,
Apps from Flatpack will have certain limitations, such as that:

* [YK SK won't work in Brave](https://github.com/flathub/com.brave.Browser/issues/126).


## Troubleshooting

### The blocked updates of 2022-08

As per https://fedoramagazine.org/manual-action-required-to-update-fedora-silverblue-kinoite-and-iot-version-36/,
after running e.g. `rpm-ostree install kitty`
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

This had to be re-run after EVERY `rpm-ostree install`, BEFORE the next reboot, until this known bug was fixed:

* https://github.com/fedora-silverblue/issue-tracker/issues/322
* https://github.com/coreos/rpm-ostree/issues/3925
* https://bugzilla.redhat.com/show_bug.cgi?id=2096192
* https://bugzilla.redhat.com/show_bug.cgi?id=2118172
* https://ask.fedoraproject.org/t/fedora-silverblue-36-will-not-succesfully-deploy-after-layering-packages/25352/13
* https://discussion.fedoraproject.org/t/rpm-ostree-updates-broken-cant-rollback-grub2-mkconfig-erroring-out/33982
* https://fedoramagazine.org/manual-action-required-to-update-fedora-silverblue-kinoite-and-iot-version-36/
