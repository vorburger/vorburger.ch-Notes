---
title: "SSH failure due to gcr-ssh-agent"
date: 2026-05-25
tags: ["linux", "security"]
image: "/images/ssh-tpm.png"
---

# SSH failure due to gcr-ssh-agent

On a Fedora Linux 44 Workstation which worked great so far
with [SSH with TPM](../security/ssh-tpm.md), I suddenly hit:

    ssh git@github.com
    git@github.com: Permission denied (publickey).

turns out that:

    echo $SSH_AUTH_SOCK
    /run/user/1000/gcr/ssh

We don't want that, so to permanently disable it, do:

    systemctl --user stop gcr-ssh-agent.socket gcr-ssh-agent.service
    systemctl --user mask gcr-ssh-agent.socket gcr-ssh-agent.service

You'll then need to restart your system (not just logout and re-login from the session).
