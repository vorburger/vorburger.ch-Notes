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
    systemctl --type=service --state=active

    podman run --rm -it hello-world


## Automatic Updates

See https://docs.fedoraproject.org/en-US/fedora-coreos/update-streams/,
and https://docs.fedoraproject.org/en-US/fedora-coreos/tutorial-updates/ :

    systemctl status zincati

    rpm-ostree status

Note https://docs.fedoraproject.org/en-US/fedora-coreos/bootloader-updates/
and https://github.com/coreos/bootupd/.


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
        --ignition-url http://192.168.1.100:8000/first.ign


## ToDo

1. reduce services
1. bare metal
1. custom toolbox
1. podman from within toolbox
1. IPFS
1. Marketplace
1. webshell
1. cadvisor
1. Kube, e.g. https://github.com/poseidon/typhoon
