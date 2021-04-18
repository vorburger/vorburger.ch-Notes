# Exploring Fedora CoreOS

## First Steps

Following https://docs.fedoraproject.org/en-US/fedora-coreos/getting-started/ :

    podman run --pull=always --rm -v $HOME/.local/share/libvirt/images/:/data -w /data \
        quay.io/coreos/coreos-installer:release download -s stable -p qemu -f qcow2.xz --decompress

    qemu-img create -f qcow2 -b \
        ~/.local/share/libvirt/images/fedora-coreos-33.20210328.3.0-qemu.x86_64.qcow2 \
        ~/.local/share/libvirt/images/my-first-fcos-vm.qcow2
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


## Reset PV and start over

    rm ~/.local/share/libvirt/images/my-first-fcos-vm.qcow2

    qemu-img create -f qcow2 -b \
         ~/.local/share/libvirt/images/fedora-coreos-33.20210328.3.0-qemu.x86_64.qcow2 \
         ~/.local/share/libvirt/images/my-first-fcos-vm.qcow2
