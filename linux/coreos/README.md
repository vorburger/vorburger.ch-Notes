# Exploring Fedora CoreOS

See https://docs.fedoraproject.org/en-US/fedora-coreos!


## First Steps

Following https://docs.fedoraproject.org/en-US/fedora-coreos/getting-started/ :

    podman run --pull=always --rm -v $HOME/.local/share/libvirt/images/:/data -w /data \
        quay.io/coreos/coreos-installer:release download -s stable -p qemu -f qcow2.xz --decompress

    qemu-img create -f qcow2 -b \
        ~/.local/share/libvirt/images/fedora-coreos-33.20210328.3.0-qemu.x86_64.qcow2 \
        ~/.local/share/libvirt/images/my-first-fcos-vm.qcow2 20G
    ls -lh ~/.local/share/libvirt/images/

    podman run --interactive --rm quay.io/coreos/butane:release \
        --pretty --strict <first.bu >first.ign
    bat first.ign

    qemu-kvm -m 2048 -cpu host -nographic \
        -drive if=virtio,file=$HOME/.local/share/libvirt/images/my-first-fcos-vm.qcow2 \
        -fw_cfg name=opt/com.coreos/config,file=first.ign \
        -nic user,model=virtio,hostfwd=tcp::2222-:22

    ssh-keygen -R "[localhost]:2222"
    ssh -o StrictHostKeyChecking=accept-new -p 2222 core@localhost

    pstree
    systemctl status
    systemctl --type=service --state=active
    hostnamectl

    podman run --rm -it hello-world


## Automatic Updates

See https://docs.fedoraproject.org/en-US/fedora-coreos/update-streams/,
and https://docs.fedoraproject.org/en-US/fedora-coreos/tutorial-updates/ :

    systemctl status zincati

    rpm-ostree status

Note https://docs.fedoraproject.org/en-US/fedora-coreos/bootloader-updates/
and https://github.com/coreos/bootupd/.


## Switching to a Different Update Stream

As per https://docs.fedoraproject.org/en-US/fedora-coreos/update-streams/ :

    sudo systemctl stop zincati.service
    sudo rpm-ostree rebase --bypass-driver fedora/x86_64/coreos/testing
    sudo systemctl reboot
    rpm-ostree status

To undo:

    sudo rpm-ostree rebase --bypass-driver fedora/x86_64/coreos/stable
    rpm-ostree rollback -r


## Administration

See https://docs.fedoraproject.org/en-US/fedora-coreos/debugging-with-toolbox/ :

    toolbox enter


## Persistent Disks, Storage Volumes

See https://docs.fedoraproject.org/en-US/fedora-coreos/storage/ :

    lsblk
    df -h
    mount
    free -h

Note that there's No Swap, by default (good), but there's a 50 GB `/var`, defined in our [`first.bu`](first.bu).


## cgroups v2

As per https://lists.fedoraproject.org/archives/list/coreos-status@lists.fedoraproject.org/thread/6NGBXYMJ4YU3V667XN627WOGCJA47POT/,
with some background e.g. on https://thenewstack.io/linux-cgroups-v2-brings-rootless-containers-superior-memory-management/,
with the our [`first.bu`](first.bu), `ls /sys/fs/cgroup` shows e.g. `system.slice` with the limits of each systemd service.


## Reset VM and start over

    rm ~/.local/share/libvirt/images/my-first-fcos-vm.qcow2

    qemu-img create -f qcow2 -b \
         ~/.local/share/libvirt/images/fedora-coreos-33.20210328.3.0-qemu.x86_64.qcow2 \
         ~/.local/share/libvirt/images/my-first-fcos-vm.qcow2


## Bare Metal

See https://docs.fedoraproject.org/en-US/fedora-coreos/bare-metal/ :

    podman run --privileged --pull=always --rm -v $HOME/Downloads:/data -w /data \
        quay.io/coreos/coreos-installer:release download -s stable -p metal -f iso
    ls -hl ~/Downloads/fedora-coreos*.iso

Use e.g. _GNOME Disks_ to write ISO to USB stick, boot it e.g. on an Intel NUC, and find the primary block device:

    lsblk

On another machine, replace `/dev/vda` in `first.bu` with the primary block device (e.g. `/dev/nvme0n1`),
and, as above, rebuild the `ign` (Ignition) from the `bu` (Butane), and serve it over HTTP:

    podman run --interactive --rm quay.io/coreos/butane:release \
        --pretty --strict <first.bu >first.ign

    python3 -m http.server

so that we can now launch the installation on the new machine, pointing it to our Ignition file:

    sudo coreos-installer install /dev/nvme0n1 \
        --insecure-igntion --ignition-url http://192.168.1.124:8000/first.ign \
        --stream stable

Reboot (twice?) and you can:

    ssh core@MACHINE

Note that UEFI Booting from USB can be PITA, especially if the machine already has a previous Fedora; you have to:

1. NUC: Press F10 to enter boot menu (F2 enters Setup, F7 BIOS, F12 PXE)
1. Note the x3 (!) first entries labeled "Fedora" (following by IPv4 and IPv6 PXE)
1. Guess that the 1st Fedora is an old install, 2nd is GRUB, and 3rd is USB UEFI Boot
1. Secure Boot can (should) be enabled

`ls /sys/firmware/efi/` existence proves that CoreOS ISO booted as UEFI.


## Containers

### Podman run in systemd

`podman run -d --name hello-app -p 9090:8080 gcr.io/google-samples/hello-app:1.0` runs https://github.com/GoogleCloudPlatform/kubernetes-engine-samples/tree/master/hello-app on http://localhost:9090. But this is manual, and won't survive a restart.

`podman generate systemd --new -n hello-app > ~/.config/systemd/user/hello-app.service` produces a proposed systemd service unit file.

`podman rm -f hello-app` stops the initial manually started container.

`systemctl --user start hello-app` starts it as a service. systemd automatically restarts this container. Test this e.g. by doing `podman stop hello-app` - it comes back!  (You may however want to tweak the `TimeoutStopSec=7`, and perhaps change the generated `Restart=on-failure` to `Restart=always` so that even an "orderly" (`exit 0`) container stop causes restarts.)

`systemctl --user status hello-app` shows podman logs, but NOT the container's logs.  Add `--log-driver=journald` after `podman run` in `~/.config/systemd/user/hello-app.service`, _and remember to `systemctl --user daemon-reload`_ before a `systemctl --user restart hello-app` - and `status` will now show the container's logs.  (An older alternative was to instead remove `-d` **AND** `Type=forking`; but that approach seems to consume more memory, according to systemctl status. Makes sense, right?)

`journalctl --user -u hello-app` shows a full longer log than `systemctl status`.

`podman run ... --read-only`, with or without `--read-only-tmpfs`, are other options possibly of interest.

`loginctl enable-linger core && systemctl --user enable --now hello-app` enables the unit to auto-start.
(`enable-linger` is not required if Butane 0644 creates `/var/lib/systemd/linger/core`, see [Documention about enabling systemd lingering](https://docs.fedoraproject.org/en-US/fedora-coreos/tutorial-user-systemd-unit-on-boot/).)

`sudo systemctl reboot` tests the container's automatic start-up.


### Podman play kube in systemd

    podman run -d --name hello-app -p 9090:8080 gcr.io/google-samples/hello-app:1.0

    podman generate kube hello-app > ~/.config/k8s/hello-app.yaml
    
    podman rm -f hello-app
    
    podman play kube ~/.config/k8s/hello-app.yaml

    podman pod rm -f hello-app_pod

`~/.config/systemd/user/hello-app.service` for this:

    [Unit]
    Wants=network.target
    After=network-online.target
    RequiresMountsFor=/var/home/core/.local/share/containers/storage /run/user/1000/containers

    [Service]
    Environment=PODMAN_SYSTEMD_UNIT=%n
    Restart=always
    TimeoutStopSec=7
    ExecStartPre=/usr/bin/podman pod rm -f -i hello-app_pod
    ExecStart=/usr/bin/podman play kube --log-driver journald %E/k8s/hello-app.yaml
    ExecStop=/usr/bin/podman pod stop --ignore hello-app_pod -t 10
    Type=forking

    [Install]
    WantedBy=multi-user.target default.target

`systemctl --user enable --now hello-app` enables this on boot.

https://github.com/containers/podman/issues/10189 discusses making it possible to auto-generate this.

Nota bene that `podman play kube --log-driver journald` is broken in Podman 3.1.0
due to https://github.com/containers/podman/issues/10015, fixed by https://github.com/containers/podman/pull/10044;
https://github.com/containers/podman/issues/10117 seems to imply that Podman 3.1.1+ will include the fix, and indeed
switching the update stream from the "stable" 33.20210412.3.0 to the 33.20210426.2.0 from "testing" gives us Podman 3.1.2.


## Personal User

Let's set-up another user than `core`, which does not have root, that we use for development, and under which we can later run containers,
as per https://github.com/endocode/coreos-docs/blob/master/os/adding-users.md#add-user-manually:

    sudo useradd -p "*" -U -m vorburger
    sudo mkdir -p ~vorburger/.ssh/authorized_keys.d
    sudo cp ~/.ssh/authorized_keys.d/ignition ~vorburger/.ssh/authorized_keys.d/
    sudo chmod 700 ~vorburger/.ssh
    sudo chmod 700 ~vorburger/.ssh/authorized_keys.d
    sudo chown -R vorburger.vorburger ~vorburger/.ssh/

Remember to [enable systemd lingering](https://docs.fedoraproject.org/en-US/fedora-coreos/tutorial-user-systemd-unit-on-boot/):

    loginctl enable-linger vorburger

To start over:

    sudo userdel -rZ vorburger

 _TODO `/etc/subuid` and `/etc/subgid` ?_

_TODO Just add to [`first.bu`](first.bu) instead?_


## ToDo

1. [podman from within toolbox](https://github.com/containers/toolbox/issues/145)
1. Matrix? https://fedoramagazine.org/deploy-your-own-matrix-server-on-fedora-coreos/
1. IPFS
1. Marketplace
1. webshell
1. Kube, e.g. https://github.com/poseidon/typhoon
1. cadvisor
1. CoreOS disable non-needing systemd service processes? (Minor optimization)
1. CoreOS security.. as long as root account exists, and files can be ostree added and reboot, still risk?
1. How to make a custom CoreOS based distribution, self built, with much less packages? No podman, no docker..
