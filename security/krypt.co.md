# [Krypton](https://krypt.co)

See https://krypt.co and sources on https://github.com/kryptco.

https://krypt.co/start/ => https://krypt.co/ext/ for U2F Browser Extension, if interested.

More on https://krypt.co/docs.


## Setup SSH

Install [the Android App](https://play.google.com/store/apps/details?id=co.krypt.kryptonite),
and in its _Settings (Krypton Core)_ enable **[X] Developer Mode**
(and review other Settings; perhaps _Disable Google Analytics_).
Now on workstation/desktop host:

    curl https://krypt.co/kr | sh

    kr pair

and scan the displayed QR code in the _PAIR_ tab on the App.
The printed SSH public key is `~/.ssh/id_krypton.pub` (also `kr me`),
and can be put e.g. on https://github.com/settings/keys or on a server
(also using `kr add <user>@<server>`) as per https://krypt.co/docs/start/upload-your-ssh-publickey.html.


## Setup PGP/GPG

    kr codesign

Note that `krgpg` (see below) *IGNORES* the _signingkey_.


## Conflict with existing Security Keys

This problem seen [with ed25519-sk keys](ed25519-sk.md):

    $ ssh USER@THESERVERNAME
    sign_and_send_pubkey: signing failed for ECDSA-SK "/var/home/vorburger/.ssh/id_ecdsa_sk" from agent: agent refused operation
    no such identity: /var/home/vorburger/.ssh/id_rsa: No such file or directory
    no such identity: /var/home/vorburger/.ssh/id_ecdsa: No such file or directory
    no such identity: /var/home/vorburger/.ssh/id_dsa: No such file or directory
    core@toby: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).

is not fixed merely by adding this to `~/.ssh/config`:

    Host THESERVERNAME
        IdentityFile ~/.ssh/id_ecdsa_sk

but changing the `Host *` to be more specific, e.g. `Host github.com` does fix it.


## Background

`~/.ssh/id_krypton.pub` is the public key (`kr me`).

As per https://krypt.co/docs/start/installation.html, note the changes made to `~/.ssh/config`:

    # Added by Krypton
    Host *
        IdentityAgent ~/.kr/krd-agent.sock
        ProxyCommand /usr/bin/krssh %h %p
        IdentityFile ~/.ssh/id_krypton
        IdentityFile ~/.ssh/id_ed25519
        IdentityFile ~/.ssh/id_rsa
        IdentityFile ~/.ssh/id_ecdsa
        IdentityFile ~/.ssh/id_dsa

and to `~/.gitconfig` after `kr codesign` as per https://krypt.co/docs/start/code-signing.html:

    [gpg]
        program = /usr/bin/krgpg

    [commit]
        gpgSign = true

    [tag]
        forceSignAnnotated = true


## ToDo

1. How to use this to GPG crypt... sub-key is missing crypt, can it be added?

1. Back up, see https://krypt.co/docs/start/backup.html and https://krypt.co/docs/start/transfer_authority.html

1. [Android App on F-Droid](https://github.com/kryptco/krypton-android/issues/51)
