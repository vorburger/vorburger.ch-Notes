# Visual Studio Code Tunnel systemd service authorization token refresh

If VSC shows you:

> An unexpected error occurred that requires a reload of this page. The workbench failed to connect to the server (Error: The VS Code gateway is not currently running.)

and a web page reload doesn't help, and if `code tunnel service log` (or `systemctl --user status code-tunnel.service`, which is ~same, under Linux) shows:

```sh
● code-tunnel.service - Visual Studio Code Tunnel
     Loaded: loaded (/home/vorburger/.config/systemd/user/code-tunnel.service; enabled; preset: disabled)
    Drop-In: /usr/lib/systemd/user/service.d
             └─10-timeout-abort.conf
     Active: active (running) since Tue 2024-02-20 20:35:36 CET; 7min ago
   Main PID: 439302 (code-cli)
      Tasks: 13 (limit: 76604)
     Memory: 3.0M
        CPU: 63ms
     CGroup: /user.slice/user-1000.slice/user@1000.service/app.slice/code-tunnel.service
             └─439302 /home/vorburger/.local/bin/code-cli --verbose --cli-data-dir /home/vorburger/.vscode/cli tunnel service internal-run
Feb 20 20:41:47 HOSTNAME code-cli[439302]: [2024-02-20 20:41:47] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
Feb 20 20:41:57 HOSTNAME code-cli[439302]: [2024-02-20 20:41:57] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
Feb 20 20:42:07 HOSTNAME code-cli[439302]: [2024-02-20 20:42:07] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
Feb 20 20:42:18 HOSTNAME code-cli[439302]: [2024-02-20 20:42:18] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
Feb 20 20:42:28 HOSTNAME code-cli[439302]: [2024-02-20 20:42:28] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
Feb 20 20:42:38 HOSTNAME code-cli[439302]: [2024-02-20 20:42:38] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
Feb 20 20:42:48 HOSTNAME code-cli[439302]: [2024-02-20 20:42:48] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
Feb 20 20:42:58 HOSTNAME code-cli[439302]: [2024-02-20 20:42:58] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
Feb 20 20:43:08 HOSTNAME code-cli[439302]: [2024-02-20 20:43:08] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
Feb 20 20:43:19 HOSTNAME code-cli[439302]: [2024-02-20 20:43:19] trace refresh poll failed, retrying: Error getting authorization: authorization_pending The authorization request is still pending.
```

then this means that the authentication token which the tunnel uses does not work (anymore, expired, or some GitHub security limitation; with VSC not renewing it), note this:

```sh
$ code tunnel user show
```

this should show your GitHub user name - if it's empty, then that's this problem. The following *MAY* help:

```sh
$ code tunnel user logout
$ code tunnel user login
$ code tunnel user show
```

this shold now show your user name. If it still does not, then perhaps you are hitting [the limit of 10 tokens that GitHub issues
per user/application/scope combination, and a rate limit of ten tokens created per hour](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/authorizing-oauth-apps#creating-multiple-tokens-for-oauth-apps).
Unfortunately those token are not listed on [your OAuth](https://github.com/settings/developers) (that's only "Apps"),
[your (classic) Tokens](https://github.com/settings/tokens) or [your fine-grained tokens](https://github.com/settings/tokens?type=beta) pages.

PS: It's a shame this GitHub problem doesn't cause an audit event on your https://github.com/settings/security-log - that would be much clearer to debug.

## Clean Up Tunnels

It's possible that you somehow have "old tunnel servers" running. If you suspect that, you may want to try this to "clean up":

```sh
$ code tunnel service stop
$ code tunnel kill
$ code tunnel prune
$ code tunnel unregister
```

After this, you should normally no longer see that machine's tunnel in the _Remotes > Tunnels_ view of VSC's _Remote Explorer._

## Service re-installation

Re-installing the service to force a new token does the same, but won't help either if the above doesn't work:

```sh
$ systemctl --user stop code-tunnel.service
$ code tunnel service uninstall
$ systemctl --user daemon-reload
$ code tunnel service install
[2024-02-20 20:51:36] info Using Github for authentication, run `code tunnel user login --provider <provider>` option to change this.
To grant access to the server, please log into https://github.com/login/device and use code ...
$ systemctl --user daemon-reload
```

## Background

* https://github.com/microsoft/vscode-remote-release/issues/9320
* https://github.com/microsoft/vscode-remote-release/issues/9058
* https://github.com/microsoft/vscode-remote-release/issues/8289
* https://github.com/microsoft/vscode/issues/194658
