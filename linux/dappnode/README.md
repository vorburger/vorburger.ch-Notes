# DAppNode

https://dappnode.io

## ToDo

1. Telegram Notification http://dappnode.local/#/system/notifications
1. Do any ports need to be opened on the firewall? http://dappnode.local/#/support/ports
   Note http://dappnode.local/#/system/add-ipfs-peer "Error getting your peer multiaddress: Failed to fetch"
1. How-to access `.eth` domains? Needs VPN?
1. Packages to install?
1. Ethereum RPCs?


## Manage

Power Off on http://dappnode.local/#/system/power.


## Set up

As per official documentation, and then:

1. choose Initial Configuration Full Node OpenEthereum
1. disable WiFi on http://dappnode.local/#/wifi/wifi
1. run http://dappnode.local/#/system/security updates
1. `ssh-copy-id -i ~/.ssh/id_ecdsa_sk.pub vorburger@dappnode.local`
1. `ssh vorburger@dappnode.local`
1. `su -`, and then `nano /etc/ssh/sshd_config` and change the following:
   `PasswordAuthentication no`, and `ChallengeResponseAuthentication no` and `UsePAM no` and `X11Forwarding no`
1. `systemctl restart sshd`
1. check http://dappnode.local/#/support/auto-diagnose
