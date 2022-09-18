# DAppNode

https://dappnode.io

## ToDo

1. `ipfs.dappnode` with HTTPS? See https://github.com/dappnode/DAppNode/issues/406, and below.
1. Fix http://alice.eth etc. as it's still NOK when now that IPFS works, see https://github.com/dappnode/DAppNode/issues/492
1. Install http://dappnode.local/#/installer/prysm.dnp.dappnode.eth
1. http://dappnode.local/#/installer More packages to install?
1. http://dappnode.local/#/community => https://sourcecred.dappnode.io/#/explorer PAN?
1. [DAppNode DApp list ](https://docs.google.com/forms/d/e/1FAIpQLSf-SI3NfcvD0tXLvn6aoBpHpwiujjhg8z8kuCDCyFka-f5cRQ/viewform)
1. Configure `/etc/wireguard/wg0.conf` to "route" / "lookup" (?) ONLY `.eth` and `.dappnode` domain names through that VPN? Test by shutdown DAppNode.
1. (Re-)install and configure http://dappnode.local/#/packages/rotki.dnp.dappnode.eth/info
1. git server (local at first, then on IPFS); e.g. on https://github.com/linuxserver?
1. [Backups](https://docs.dappnode.io/user-guide/ui/recommended-set-ups/backup-functionality), for git server and other, on IPFS


## Use

* IPFS
  * WebUI http://ipfs.dappnode:5001/webui/ (see below for Troubleshooting if this doesn't work)
  * Peering: Add e.g. `/ip4/192.168.1.99/tcp/4001/p2p/12D3Koo...P8TN` on brave://settings/ipfs/peers for peering and use Brave local IPFS node. 
    Strangely, this then does NOT show up in the list on http://127.0.0.1:45005/webui/#/peers.
  * Gateway http://ipfs.dappnode:8080 how-to? Expose port on http://dappnode.local/#/packages/ipfs.dnp.dappnode.eth/network?
  * API http://ipfs.dappnode:5001?
    TODO: How to publish a file on one local computer, pin it on the DAppNode, and then access it from another local computer?
* Ethereum RPC API
  * TODO: How to use http://dappnode.local/#/packages/geth.dnp.dappnode.eth/info's Querying API http://geth.dappnode:8545 and Engine API http//geth.dappnode:8551?
* `*.eth` [domain names](https://ens.domains):
  * [alice.eth](http://alice.eth) or [freedomain.eth](http://freedomain.eth) (or e.g. [radek.freedomain.eth](http://radek.freedomain.eth) from [this tutorial](http://radek.freedomain.eth) are some examples
  * These normally do not resolve over traditional root DNS servers
  * Brave has built-in ENS resolution by using Infura, instead of decentralized DAppNode
  * When connected to DAppNode's WireGuard VPN, that will resolve both `.eth` and `.dappnode` domain names
  * [nonexistantx.eth](http://nonexistantx.eth) should show an error message from DAppNode's `/usr/src/app/webpack:/@dappnode/dappmanager/src/ethForward/resolveDomain.ts`: _Decentralized website not found Make sure the ENS domain exists"
  * `ipfs resolve -r /ipfs/: invalid path "/ipfs/": not enough path components` means _TODO, IDK, fix IPFS #first?_ See https://github.com/dappnode/DAppNode/issues/492


### IPFS in Brave (not directly DAppNode related)

* brave://settings/ipfs
* brave://ipfs-internals/
* If using Brave Local IPFS Node:
  * http://127.0.0.1:45005/webui
  * brave://settings/ipfs/peers


## Manage & Maintenance

* http://dappnode.local
* `sudo wg-quick up wg0` is required ;) to use `*.dappnode` hostnames
* http://dms.dappnode/d/dappnode-exporter-host/host?orgId=1&refresh=10s
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
1. restart ~~http://dappnode.local/#/packages/openethereum.dnp.dappnode.eth/info (?) and~~ IPFS (both maybe not required?)
1. ~~check http://dappnode.local/#/packages/openethereum.dnp.dappnode.eth/logs~~
1. check http://dappnode.local/#/packages/ipfs.dnp.dappnode.eth/logs
1. ~~Instead of VPN, simply adding e.g. 5001 e.g. on http://dappnode.local/#/packages/ipfs.dnp.dappnode.eth/network
   allows to use e.g. http://dappnode.local:5001/ipfs/ instead of http://ipfs.dappnode:5001/webui; simple enough!~~
1. As per https://github.com/dappnode/DAppNode/issues/406,
   forward port 8080 <=> 8080 by TCP on http://dappnode.local/#/packages/ipfs.dnp.dappnode.eth/network.
   Now on brave://settings/ipfs switch from _Brave local IPFS node_ to _Gateway_, and change the
   _IPFS public gateway address_ from the default https://dweb.link to http://ipfs.dappnode:8080/ (port 8080, not 5001).
   With that, just like https://bafkqae2xmvwgg33nmuqhi3zajfiemuzahiwss.ipfs.dweb.link showing _Welcome to IPFS :-)_
   our http://dappnode:8080/ipfs/bafkqae2xmvwgg33nmuqhi3zajfiemuzahiwss shows the same, and
   http://ipfs.dappnode:8080/ipfs/bafkqae2xmvwgg33nmuqhi3zajfiemuzahiwss => http://bafkqae2xmvwgg33nmuqhi3zajfiemuzahiwss.ipfs.ipfs.dappnode:8080 same;
   and going e.g. to https://ipfs.io in Brave takes me to http://ipfs.tech.ipns.ipfs.dappnode:8080! 🚀
1. Wait for Geth sync finished on http://dappnode.local/#/dashboard
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
1. Install http://dappnode.local/#/installer/dappnode-exporter.dnp.dappnode.eth for https://github.com/prometheus/node_exporter 
1. Install http://dappnode.local/#/installer/dms.dnp.dappnode.eth


## Support

* [Discord](https://discord.com/channels/747647430450741309/747647430920503338)
* [Issues](https://github.com/dappnode/DAppNode/issues)


## Troubleshooting

1. Logs on http://dappnode.local/#/support/activity
1. _DAppNode Package is not compatible: dappGet could not resolve request core.dnp.dappnode.eth@0.2.57, error on aggregate stage: IPFS hash not available /ipfs/QmUr87duodYqkWX8wZNuZCdWA35D3MSTZrxUzF9E4mFNY1: request to http://ipfs.dappnode:5001/api/v0/ls?timeout=30000ms&arg=%2Fipfs%2FQmUr87duodYqkWX8wZNuZCdWA35D3MSTZrxUzF9E4mFNY1 failed, reason: connect ECONNREFUSED 172.33.1.5:5001 Error fetching core.dnp.dappnode.eth@0.2.57_ :: Shutdown & Reboot after Core packge upgrade failure due to the following seems to have helped at least somewhat.
1. _Repository Source Full node: using remote not available: Error: connect ECONNREFUSED_ :: This is caused e.g. if Geth is down, but is selected instead of a Remote or Light Client on http://dappnode.local/#/system/repository. Fix Geth, or switch to another one, to fix this.
1. Geth error something or the other on the Dashboard, e.g. after a restart, sometimes goes away if you wait for a while to re-start and finish sync-ing.
1. If e.g. http://ipfs.dappnode:5001/webui works in Firefox but fails in Brave saying _ipfs resolve -r /ipns/ipfs.dappnode/ipfs/bafybeibozpulxtpv5nhfa2ue3dcjx23ndh3gwr5vwllk7ptoyfwnfjjr4q/: could not resolve name: "ipfs.dappnode" is missing a DNSLink record (https://docs.ipfs.io/concepts/dnslink/)_, then use Disabled _Method to resolve IPFS resources_ on brave://settings/ipfs.

## Bugs

* OpenEthereum seems to be broken, it never synced (even after I opened ports; unless it was the wrong ones)
* http://dappnode.local/#/system/add-ipfs-peer "Error getting your peer multiaddress: Failed to fetch"
  Error on Console shows it's because it's using http://ipfs.dappnode:5001/api/v0/id.
  So it's just a UI bug if you don't use a VPN.
  Is it possible to switch the UI to use e.g. http://dappnode:5001/api/v0/id ?
