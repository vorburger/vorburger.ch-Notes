# GPG hangs on hostname change

I currently use [GnuPG](https://gnupg.org) with [Yubikeys](https://www.yubico.com), mostly (but not only) for [`pass`](https://www.passwordstore.org).

One fine day I'll switch to [`age`](https://age-encryption.org) (from [FiloSottile](https://github.com/FiloSottile/age); with [SK](https://github.com/str4d/age-plugin-yubikey) and [TPM](https://github.com/Foxboron/age-plugin-tpm)) with [`passage`](https://github.com/FiloSottile/passage), and other [awesome stuff](https://github.com/FiloSottile/awesome-age); but today it's `gpg`.

Today that `gpg (GnuPG) 2.4.3` suddenly stopped working, just stuck and hanging in there... I've debugged it with [`strace`](https://strace.io):

```sh
$ strace gpg --debug-all --card-status
(...)
openat(AT_FDCWD, "/etc/localtime", O_RDONLY|O_CLOEXEC) = 4
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=1909, ...}, AT_EMPTY_PATH) = 0
newfstatat(4, "", {st_mode=S_IFREG|0644, st_size=1909, ...}, AT_EMPTY_PATH) = 0
read(4, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\5\0\0\0\5\0\0\0\0"..., 4096) = 1909
lseek(4, -1217, SEEK_CUR)               = 692
read(4, "TZif2\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\0\6\0\0\0\6\0\0\0\0"..., 4096) = 1217
close(4)                                = 0
newfstatat(AT_FDCWD, "/run/user/1000/gnupg/S.keyboxd", {st_mode=S_IFSOCK|0700, st_size=0, ...}, 0) = 0
socket(AF_UNIX, SOCK_STREAM, 0)         = 4
newfstatat(AT_FDCWD, "/run/user/1000/gnupg/S.keyboxd", {st_mode=S_IFSOCK|0700, st_size=0, ...}, 0) = 0
connect(4, {sa_family=AF_UNIX, sun_path="/run/user/1000/gnupg/S.keyboxd"}, 32) = 0
recvmsg(4,
```

Turns out this is https://dev.gnupg.org/T6838, which I found after some research, via https://bugzilla.redhat.com/show_bug.cgi?id=2249218.

That bug mentions something related to lock files and hostname changes, and I had indeed changed it.
What was quite confusing is that I had made that change a few days ago already; but the problem only surfaced today, after a reboot.
Interestingly, changing the hostname back to what it used to be didn't fix it (for me).

I didn't try removing `use-keyboxd` from `~/.gnupg/common.conf` (which some posts in some Forums recommend), because it wasn't fully clear to me what that changes, breaks or not. I wasn't super motivated to dig further into it, as I like stuff to "work with out of the box defaults", when possible.

The "solution" what worked for me was just to `rm -rf ~/.gnupg/` and start over; that's easy with [my dotfiles](https://github.com/vorburger/vorburger-dotfiles-bin-etc), for me. Of course, [YMMV](https://en.wiktionary.org/wiki/your_mileage_may_vary); if you're reading this, you presumably are an adult who knows what you are doing; don't blame me if you inadvertently loose your private keys by removing that directory!
