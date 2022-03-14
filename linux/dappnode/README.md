# DAppNode

https://dappnode.io

## ToDo

1. Geth sync finished on http://dappnode.local/#/dashboard? Disable http://dappnode.local/#/system/repository.
1. Install http://dappnode.local/#/installer/prysm.dnp.dappnode.eth
1. IPFS conflict if using Brave's own local built-in IPFS Node?
   Switch brave://settings/ipfs from _Brave local IPFS node_ to Disabled, for now.
   _TODO Make Brave use DAppNode as IPFS Node?_
1. Ethereum RPC API ?
1. Learn more about accessing `.eth` domains, through VPN? But what example domain?
1. http://dappnode.local/#/installer More packages to install?
1. http://dappnode.local/#/community => https://sourcecred.dappnode.io/#/explorer PAN?
1. git server (local at first, then on IPFS); e.g. on https://github.com/linuxserver?
1. [Backups](https://docs.dappnode.io/user-guide/ui/recommended-set-ups/backup-functionality), for git server and other, on IPFS
1. [DAppNode DApp list ](https://docs.google.com/forms/d/e/1FAIpQLSf-SI3NfcvD0tXLvn6aoBpHpwiujjhg8z8kuCDCyFka-f5cRQ/viewform)


## Manage

Power Off on http://dappnode.local/#/system/power.


## Set up

As per official documentation, and then:

1. choose Initial Configuration Full Node Geth (OpenEthereum never sync'd)
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
1. http://dappnode.local/#/vpn/wireguard set-up:
   [Android set-up works](https://docs.dappnode.io/user-guide/ui/access/vpn/#android-1)
   `sudo dnf install wireguard-tools`, then [follow DAppNode doc](https://docs.dappnode.io/user-guide/ui/access/vpn/#linux-1),
   or could try out [`nm-connection-editor` etc.](https://www.xmodulo.com/wireguard-vpn-network-manager-gui.html).
   But this tunnels all traffic through DAppNode, right? That seems like it may be a bad idea.
1. Instead of VPN, simply adding e.g. 5001 e.g. on http://dappnode.local/#/packages/ipfs.dnp.dappnode.eth/network
   allows to use e.g. http://dappnode:5001/ipfs/ instead of http://ipfs.dappnode:5001/webui; simple enough!

## Bugs

* OpenEthereum seems to be broken, it never synced (even after I opened ports; unless it was the wrong ones)
* http://dappnode.local/#/system/add-ipfs-peer "Error getting your peer multiaddress: Failed to fetch"
  Error on Console shows it's because it's using http://ipfs.dappnode:5001/api/v0/id.
  Is it possible to switch the UI to use e.g. http://dappnode:5001/api/v0/id ?
