# DAppNode

https://dappnode.io

## ToDo

1. http://dappnode.local/#/dashboard Openethereum Blocks synced: 0 / 14380482 :-(
1. https://www.brightid.org Verification, for https://discord.com/channels/@me/952670892280922142 ?
1. http://dappnode.local/#/vpn/wireguard set-up
1. http://ipfs.dappnode:5001/webui ?
1. http://openethereum.dappnode:8545 Ethereum RPC API ?
1. http://dappnode.local/#/system/add-ipfs-peer "Error getting your peer multiaddress: Failed to fetch"
1. How-to access `.eth` domains? Needs VPN?
1. http://dappnode.local/#/installer Packages to install?
1. http://dappnode.local/#/community => https://sourcecred.dappnode.io/#/explorer PAN?


## Manage

Power Off on http://dappnode.local/#/system/power.


## Set up

As per official documentation, and then:

1. choose Initial Configuration Full Node OpenEthereum
1. disable WiFi on http://dappnode.local/#/wifi/wifi
1. run http://dappnode.local/#/system/security updates
1. run http://dappnode.local/#/system/update
1. `ssh-copy-id -i ~/.ssh/id_ecdsa_sk.pub vorburger@dappnode.local`
1. `ssh vorburger@dappnode.local`
1. `su -`, and then `nano /etc/ssh/sshd_config` and change the following:
   `PasswordAuthentication no`, and `ChallengeResponseAuthentication no` and `UsePAM no` and `X11Forwarding no`
1. `systemctl restart sshd`
1. check http://dappnode.local/#/support/auto-diagnose
1. set up Telegram notifications http://dappnode.local/#/system/notifications following https://core.telegram.org
1. Note http://dappnode.local/#/support/ports and open ports 4001-4002 for IPFS
   and 49154-49156 for OpenEthereum, on both TCP/UPD, on the firewall.
   (The _API Scan_ button says _"Unknown"_ for the UDP ports, that's fine.)
1. restart http://dappnode.local/#/packages/openethereum.dnp.dappnode.eth/info
1. check http://dappnode.local/#/packages/openethereum.dnp.dappnode.eth/logs
1. check http://dappnode.local/#/packages/ipfs.dnp.dappnode.eth/logs
