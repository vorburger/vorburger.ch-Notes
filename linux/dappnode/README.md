# DAppNode

https://dappnode.io

## ToDo

1. Use Geth, how-to with http://geth.dappnode:8545 ? VPN?
1. Install http://dappnode.local/#/installer/dms.dnp.dappnode.eth
1. http://dappnode.local/#/system/security Host Updates
1. Geth sync finished on http://dappnode.local/#/dashboard?
1. Install http://dappnode.local/#/installer/prysm.dnp.dappnode.eth
1. Ethereum RPC API ?
1. Learn more about accessing `.eth` domains, through VPN? But what example domain?
1. http://dappnode.local/#/installer More packages to install?
1. http://dappnode.local/#/community => https://sourcecred.dappnode.io/#/explorer PAN?
1. [DAppNode DApp list ](https://docs.google.com/forms/d/e/1FAIpQLSf-SI3NfcvD0tXLvn6aoBpHpwiujjhg8z8kuCDCyFka-f5cRQ/viewform)
1. https://github.com/dappnode/DAppNode/issues/406: IPFS conflicts if using Brave's own local built-in IPFS Node.
   Instead, add 8080 on http://dappnode.local/#/packages/ipfs.dnp.dappnode.eth/network.
   Now switch brave://settings/ipfs from _Brave local IPFS node_ to _Gateway_, and change the
   _IPFS public gateway address_ from the default https://dweb.link to http://dappnode:8080.
   Even though `curl http://dappnode:8080/ipfs/bafybeifx7yeb55armcsxwwitkymga5xf53dxiarykms3ygqic223w5sk3m#x-ipfs-companion-no-
   redirect` now works just like e.g. `curl https://gateway.ipfs.io/ipfs/bafybeifx7yeb55armcsxwwitkymga5xf53dxiarykms3ygqic223w5sk3m#x-ipfs-companion-no-
   redirect`, Brave fails due to `Only a valid IPFS gateway with Origin isolation enabled can be used in Brave`.
   _TODO Make Brave use DAppNode as IPFS Node!_ See  https://bafybeicospfr7cqekqutqxlcjp767vqmptxoriehv2gpfqvu4tq5c5okry.ipfs.dweb.link/concepts/ipfs-gateway/#limitations-and-potential-workarounds ... perhaps using a VPN fixes this?
1. (Re-)install and configure http://dappnode.local/#/packages/rotki.dnp.dappnode.eth/info
1. git server (local at first, then on IPFS); e.g. on https://github.com/linuxserver?
1. [Backups](https://docs.dappnode.io/user-guide/ui/recommended-set-ups/backup-functionality), for git server and other, on IPFS

## Manage & Maintenance

* http://dappnode.local/#/system/security Host Updates
* VPN: `sudo wg-quick up wg0` & `sudo wg-quick down wg0`
* Power Off on http://dappnode.local/#/system/power.


## Set up

As per official documentation, and then:

1. choose Initial Configuration Full Node Geth
1. disable WiFi on http://dappnode.local/#/wifi/wifi
1. run http://dappnode.local/#/system/security updates
1. run http://dappnode.local/#/system/update
1. `ssh-copy-id -i ~/.ssh/id_ecdsa_sk.pub vorburger@dappnode.local`
1. `ssh vorburger@dappnode.local`
1. `su -`, and then `nano /etc/ssh/sshd_config` and change the following:
   `PasswordAuthentication no`, and `ChallengeResponseAuthentication no` and `UsePAM no` and `X11Forwarding no`
1. `systemctl restart sshd`
1. check http://dappnode.local/#/support/auto-diagnose
1. Optional: set up Telegram notifications http://dappnode.local/#/system/notifications following https://core.telegram.org
1. Note http://dappnode.local/#/support/ports and open ports 4001-4002 for IPFS
   and 49154-49160 for Geth (or OpenEthereum), on both TCP/UPD, on the firewall.
   (The _API Scan_ button says _"Unknown"_ for the UDP ports, that's fine.)
1. restart http://dappnode.local/#/packages/openethereum.dnp.dappnode.eth/info (?) and IPFS (both maybe not required?)
1. check http://dappnode.local/#/packages/openethereum.dnp.dappnode.eth/logs
1. check http://dappnode.local/#/packages/ipfs.dnp.dappnode.eth/logs
1. Instead of VPN, simply adding e.g. 5001 e.g. on http://dappnode.local/#/packages/ipfs.dnp.dappnode.eth/network
   allows to use e.g. http://dappnode:5001/ipfs/ instead of http://ipfs.dappnode:5001/webui; simple enough!
1. http://dappnode.local/#/vpn/wireguard set-up:
   [Android set-up works](https://docs.dappnode.io/user-guide/ui/access/vpn/#android-1).
   But this tunnels all traffic through DAppNode, right? That seems like it may be a bad idea.
1. For VPN setup on Linux, follow [follow DAppNode doc](https://docs.dappnode.io/user-guide/ui/access/vpn/#linux-1),
   on http://dappnode.local/#/vpn/wireguard/ NOTE _Show local credentials_ but
   beware of https://github.com/dappnode/DAppNode/issues/405. TL;DR to:
   `sudo dnf install wireguard-tools`,
   `sudo nano /etc/wireguard/wg0.conf`,
   `sudo wg-quick up wg0`,
   `sudo wg show`.
   Or could try out [`nm-connection-editor` etc.](https://www.xmodulo.com/wireguard-vpn-network-manager-gui.html).
   (GNOME Settings Network integration for Wireguard is WIP.)

## Support

* [Discord](https://discord.com/channels/747647430450741309/747647430920503338)
* [Issues](https://github.com/dappnode/DAppNode/issues)

## Troubleshooting

1. Logs on http://dappnode.local/#/support/activity
1. DAppNode Package is not compatible: dappGet could not resolve request core.dnp.dappnode.eth@0.2.57, error on aggregate stage: IPFS hash not available /ipfs/QmUr87duodYqkWX8wZNuZCdWA35D3MSTZrxUzF9E4mFNY1: request to http://ipfs.dappnode:5001/api/v0/ls?timeout=30000ms&arg=%2Fipfs%2FQmUr87duodYqkWX8wZNuZCdWA35D3MSTZrxUzF9E4mFNY1 failed, reason: connect ECONNREFUSED 172.33.1.5:5001 Error fetching core.dnp.dappnode.eth@0.2.57 :: Shutdown & Reboot after Core packge upgrade failure due to the following seems to have helped at least somewhat.
1. Repository Source Full node: using remote not available: Error: connect ECONNREFUSED :: This is caused e.g. if Geth is down, but is selected instead of a Remote or Light Client on http://dappnode.local/#/system/repository. Fix Geth, or switch to another one, to fix this.
1. Geth error something or the other on the Dashboard, e.g. after a restart, sometimes goes away if you wait for a while to re-start and finish sync-ing.

## Bugs

* OpenEthereum seems to be broken, it never synced (even after I opened ports; unless it was the wrong ones)
* http://dappnode.local/#/system/add-ipfs-peer "Error getting your peer multiaddress: Failed to fetch"
  Error on Console shows it's because it's using http://ipfs.dappnode:5001/api/v0/id.
  So it's just a UI bug if you don't use a VPN.
  Is it possible to switch the UI to use e.g. http://dappnode:5001/api/v0/id ?
