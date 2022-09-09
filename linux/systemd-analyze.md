# Debugging Linux Start-Up Speed Performance Issues

Today while [LearningLinux](https://github.com/vorburger/LearningLinux) I noticed that
my ArchLinux VMs started as fast as always, but it seemed to take longer and longer for `sshd` to be ready.
This clarified what was happening:

    $ systemd-analyze
    Startup finished in 1.270s (kernel) + 42.586s (userspace) = 43.857s
    graphical.target reached after 42.585s in userspace.

    $ systemd-analyze blame
    41.769s pacman-init.service
    29.292s reflector-init.service
     1.198s systemd-networkd-wait-online.service
      601ms dev-vda2.device
      393ms ldconfig.service
      360ms sshdgenkeys.service
      234ms systemd-networkd.service
      168ms systemd-tmpfiles-setup.service
      155ms systemd-timesyncd.service
      145ms systemd-resolved.service
      102ms systemd-udev-trigger.service
       94ms systemd-logind.service
       93ms systemd-udevd.service
       82ms systemd-machine-id-commit.service
       79ms systemd-journal-catalog-update.service
       69ms user@1000.service
       54ms systemd-tmpfiles-setup-dev.service
       43ms systemd-journald.service
       42ms systemd-journal-flush.service
       37ms systemd-tmpfiles-clean.service
       37ms sys-kernel-tracing.mount
       36ms kmod-static-nodes.service
       36ms dev-mqueue.mount
       36ms modprobe@configfs.service
       36ms sys-kernel-debug.mount
       36ms modprobe@drm.service
       35ms dbus.service
       35ms modprobe@fuse.service
       35ms dev-hugepages.mount


    $ systemd-analyze critical-chain
    The time when unit became active or started is printed after the "@" character.
    The time the unit took to start is printed after the "+" character.

    graphical.target @42.585s
    └─multi-user.target @42.585s
      └─sshd.service @42.585s
        └─pacman-init.service @814ms +41.769s
          └─basic.target @811ms
            └─sockets.target @811ms
              └─dbus.socket @810ms
                └─sysinit.target @805ms
                  └─systemd-update-done.service @798ms +5ms
                    └─ldconfig.service @404ms +393ms
                      └─local-fs.target @402ms
                        └─run-user-1000.mount @20.308s
                          └─local-fs-pre.target @402ms
                            └─systemd-tmpfiles-setup-dev.service @346ms +54ms
                              └─systemd-sysusers.service @314ms +30ms
                                └─systemd-firstboot.service @293ms +20ms
                                  └─systemd-remount-fs.service @258ms +29ms
                                    └─systemd-journald.socket @242ms
                                      └─-.mount @237ms
                                        └─-.slice @237ms


Huh, so `critical-chain` shows that `sshd` was blocked by `pacman-init`... we can see that from the `Before=sshd.service` here:

    $ cat /etc/systemd/system/pacman-init.service
    [Unit]
    Description=Initializes Pacman keyring
    Before=sshd.service cloud-final.service
    ConditionFirstBoot=yes

    [Service]
    Type=oneshot
    RemainAfterExit=yes
    ExecStart=/usr/bin/pacman-key --init
    ExecStart=/usr/bin/pacman-key --populate

    [Install]
    WantedBy=multi-user.target

Why is `pacman-init` THAT (42s!) slow?

    $ sudo systemctl status pacman-init
    ● pacman-init.service - Initializes Pacman keyring
         Loaded: loaded (/etc/systemd/system/pacman-init.service; enabled; preset: disabled)
         Active: active (exited) since Fri 2022-09-09 21:17:26 UTC; 54min ago
        Process: 294 ExecStart=/usr/bin/pacman-key --init (code=exited, status=0/SUCCESS)
        Process: 348 ExecStart=/usr/bin/pacman-key --populate (code=exited, status=0/SUCCESS)
       Main PID: 348 (code=exited, status=0/SUCCESS)
          Tasks: 1 (limit: 2322)
         Memory: 7.9M
            CPU: 40.283s
         CGroup: /system.slice/pacman-init.service
                 └─332 gpg-agent --homedir /etc/pacman.d/gnupg --use-standard-socket --daemon

    Sep 09 21:16:51 archlinux pacman-key[521]: gpg: setting ownertrust to 4
    Sep 09 21:16:51 archlinux pacman-key[348]: ==> Disabling revoked keys in keyring...
    Sep 09 21:17:25 archlinux pacman-key[348]:   -> Disabled 52 keys.
    Sep 09 21:17:25 archlinux pacman-key[348]: ==> Updating trust database...
    Sep 09 21:17:25 archlinux pacman-key[694]: gpg: marginals needed: 3  completes needed: 1  trust model: pgp
    Sep 09 21:17:25 archlinux pacman-key[694]: gpg: depth: 0  valid:   1  signed:   6  trust: 0-, 0q, 0n, 0m, 0f, 1u
    Sep 09 21:17:26 archlinux pacman-key[694]: gpg: depth: 1  valid:   6  signed:  93  trust: 0-, 0q, 0n, 6m, 0f, 0u
    Sep 09 21:17:26 archlinux pacman-key[694]: gpg: depth: 2  valid:  73  signed:  28  trust: 73-, 0q, 0n, 0m, 0f, 0u
    Sep 09 21:17:26 archlinux pacman-key[694]: gpg: next trustdb check due at 2022-11-16
    Sep 09 21:17:26 archlinux systemd[1]: Finished Initializes Pacman keyring.

Note how it apparently took it 31s to _disable 52 revoked keys in keyring_". That seems like a long time?

Note how the earlier `systemd-analyze blame` also mentioned `reflector-init.service` taking 29s... it also blocks `sshd`:

    $ cat /etc/systemd/system/reflector-init.service
    [Unit]
    Description=Initializes mirrors for the VM
    After=network-online.target
    Wants=network-online.target
    Before=sshd.service cloud-final.service
    ConditionFirstBoot=yes

    [Service]
    Type=oneshot
    RemainAfterExit=yes
    ExecStart=reflector --latest 20 --protocol https --sort rate --save /etc/pacman.d/mirrorlist

    [Install]
    WantedBy=multi-user.target

Why is `reflector` THAT (31s!) slow? We can actually easily reproduce it manually without systemd:

    [arch@archlinux ~]$ time reflector --latest 20 --protocol https --sort rate
    [2022-09-09 23:21:29] WARNING: failed to rate http(s) download (https://archmirror.it/repos/community/os/x86_64/community.db): HTTP Error 403: Forbidden
    [2022-09-09 23:21:29] WARNING: failed to rate http(s) download (https://mirror.cyberbits.asia/archlinux/community/os/x86_64/community.db): <urlopen error [Errno 101] Network is unreachable>
    ################################################################################
    ################# Arch Linux mirrorlist generated by Reflector #################
    ################################################################################

    # With:       reflector --latest 20 --protocol https --sort rate
    # When:       2022-09-09 23:21:32 UTC
    # From:       https://archlinux.org/mirrors/status/json/
    # Retrieved:  2022-09-09 23:21:02 UTC
    # Last Check: 2022-09-09 23:17:25 UTC

    Server = https://mirror.osbeck.com/archlinux/$repo/os/$arch
    Server = https://mirror.pseudoform.org/$repo/os/$arch
    Server = https://mirror.selfnet.de/archlinux/$repo/os/$arch
    Server = https://ftp.halifax.rwth-aachen.de/archlinux/$repo/os/$arch
    Server = https://archlinux.mirror.luzea.de/$repo/os/$arch
    Server = https://mirror.cyberbits.eu/archlinux/$repo/os/$arch
    Server = https://europe.mirror.pkgbuild.com/$repo/os/$arch
    Server = https://geo.mirror.pkgbuild.com/$repo/os/$arch
    Server = https://archlinux.mailtunnel.eu/$repo/os/$arch
    Server = https://archlinux.thaller.ws/$repo/os/$arch
    Server = https://mirror.theo546.fr/archlinux/$repo/os/$arch
    Server = https://arch.mirror.constant.com/$repo/os/$arch
    Server = https://america.mirror.pkgbuild.com/$repo/os/$arch
    Server = https://asia.mirror.pkgbuild.com/$repo/os/$arch
    Server = https://sydney.mirror.pkgbuild.com/$repo/os/$arch
    Server = https://seoul.mirror.pkgbuild.com/$repo/os/$arch
    Server = https://phinau.de/arch/$repo/os/$arch
    Server = https://archlinux-br.com.br/archlinux/$repo/os/$arch
    Server = https://archmirror.it/repos/$repo/os/$arch
    Server = https://mirror.cyberbits.asia/archlinux/$repo/os/$arch


    real    0m30.372s
    user    0m0.886s
    sys     0m0.418s

I'm not sure how expected this is. I'm pretty sure it was much faster for me just yesterday - as in just a few seconds, not 40s. Perhaps this is a temporary situation due to problems with some ArchLinux mirrors. IMHO it would be great if that wouldn't slow down start-up of new Arch VMs.

I have [created this ArchLinux bug for further discussion about this](https://gitlab.archlinux.org/archlinux/arch-boxes/-/issues/153).


## References

* https://man.archlinux.org/man/systemd-analyze.1.en
* https://www.freedesktop.org/software/systemd/man/systemd-analyze.html
* https://wiki.archlinux.org/title/Improving_performance/Boot_process
* https://opensource.com/article/20/9/systemd-startup-configuration
