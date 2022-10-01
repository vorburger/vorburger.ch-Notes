# Fedora Upgrades

Following [Fedora Docs' _DNF System Upgrade_](https://docs.fedoraproject.org/en-US/quick-docs/dnf-system-upgrade/) works reasonably well.

Here are how I resolve a few issues I run into every time I upgrade.


## _"The password you use to log in to your computer no longer matches that of your login keyring."_

With _Automatic Login,_ the error message above appears e.g. when
[using GNOME Tweaks to auto-start Brave](https://github.com/vorburger/vorburger-dotfiles-bin-etc#on-fedora-workstation).

Work around it by using the _Passwords and Keys_ (Seahorse) app to delete the _Brave Safe Storage_.
This will break Brave's Saved Passwords and Sync; to fix that, go to `brave://sync-internals` to _Disable Sync (Clear Data),_
close Brave, and set up Sync again.


## GPG with YubiKey

<!-- https://github.com/vorburger/p#troubleshooting -->

    $ gpg --card-status
    gpg: selecting card failed: No such device
    gpg: OpenPGP card not available: No such device

`sudo pkill gpg-agent && sudo systemctl restart pcscd` fixes this - but only temporarily, for the current boot.

`sudo dnf remove opensc` permanently fixes this, so that I don't have to keep doing it at each boot; [Fedora Bug #1893131](https://bugzilla.redhat.com/show_bug.cgi?id=1893131) has the full background.

_TODO: Alternative, untested: Add [70-u2f.rules](https://github.com/Yubico/libfido2/blob/main/udev/70-u2f.rules) into `/etc/udev/rules.d/`?
(And futzing around with `~/.gnupg/scdaemon.conf`? Add to [my dotfiles](https://github.com/vorburger/vorburger-dotfiles-bin-etc), if required.)
Also note that [this support.yubico.com](https://support.yubico.com/hc/en-us/articles/360013714479-Troubleshooting-Issues-with-GPG) article seems
to suggest adding `reader-port Yubico Yubi` to `~/.gnupg/scdaemon.conf`._
