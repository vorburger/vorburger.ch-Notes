# The Need for Speed (Part #1)

## The Past

For the past 20 years, Swisscom was my ISP at home, and I had a (now) _Swisscom blue Internet L subscription for Residential Customers_.

The bandwidth that gave me for the last couple of years was 1 Gigabit per second (Gbps). One day when I logged in to my ISP to pay a bill, there was a message:

**"Technology upgrade: With a change to the new fiber optic standard, you can achieve a speed of up to 10 Gbit/s without a monthly fee and extra cost."**

All right! Nobody can refuse a free upgrade? 😄

## The Present

I initiated that upgrade process, and they sent me some other [kind of Fibre Optic cable](https://en.wikipedia.org/wiki/Fiber-optic_cable). Cool, except... well, there were 2 problems to really use the advertised 10 Gbps:

* #1: My Internet router now gets 10 Gbps from the ISP, but actually only has a 2.5 Gbps Ethernet plug?!
* #2: I didn't actually have any computer that could use 10 instead of 1 Gbps networking!

The first problem is fixed with a moment of "friendly whining" on the ISP's tech support hotline;
they eventually replaced my [Internet-Box 3](https://www.swisscom.ch/de/privatkunden/produkte/internetrouter/details.html/internet-box-3-ip-11039000)
(which has 1×2.5 Gbit/s und 4×1 Gbit/s Ethernet ports) with their new [Internet-Box 4](https://www.swisscom.ch/de/privatkunden/produkte/internetrouter/details.html/internet-box-4-fibre-11050518) (which has 1×10 Gbit/s und 4×1 Gbit/s Ethernet ports).

The second problem lead to a "fun" (long) night of researching what [network interface controller / card](https://en.wikipedia.org/wiki/Network_interface_controller) (NIC) to upgrade to for my home server. Many of the options were surprisingly expensive, and clearly targeting more of "[data center](https://en.wikipedia.org/wiki/Data_center)" (DC) market! I eventually settled on using an **[ASUS XG-C100](https://www.asus.com/networking-iot-servers/wired-networking/all-series/xg-c100c/)**. It didn't immediately just work out of the box (on my distro), but after [a big of digging and learning](https://www.anandtech.com/show/10908/aquantia-launches-new-2g-5g-multi-gigabit-network-controllers-for-pcs) that its chip is based on the (now Marvell, acquired) [Aquantia AQtion AQC107 controller](https://www.marvell.com/products/ethernet-adapters-and-controllers/fastlinq-edge-ethernet-controllers.html) which requires the [`atlantic`](https://www.kernel.org/doc/html/latest/networking/device_drivers/ethernet/aquantia/atlantic.html) Linux Kernel Module ([GitHub Aquantia/AQtion](https://github.com/Aquantia/AQtion)), an `sudo modprobe atlantic` of course did the trick, and the Kernel log showed `atlantic: Detect ATL2FW 1030012`. Fun fact:  Apparently Kernel versions [before 4.16.6](https://askubuntu.com/a/1106382/89557) and 5.15 [before 5.15.4](https://github.com/pop-os/beta/issues/275) had issues with it (#[199177](https://bugzilla.kernel.org/show_bug.cgi?id=199177)).

## The Future

[Init 7](https://www.init7.net) was mentioned by a number of my friends as an alternative ISP to Swisscom in the past. They offer 25 Gbps with their Fiber7-X2 subscription - wow!

I am currently looking into how to go from 10 Gbps to 25 Gbps, as this would mean a few more changes in the setup at home - the family still has to be able to watch 🎥 Netflix! 😸 Current thinking is leaning towards sticking a **[MikroTik CCR2004-1G-2XS-PCIe](https://mikrotik.com/product/ccr2004_1g_2xs_pcie)** "Smart NIC" (whoa!) running [Microtik RouterOS](https://help.mikrotik.com/docs/) on-board into the home server and use that as my new ISP inbound "router". Then going out to an Ethernet Switch, with a new WiFi Access Point (AP) for wireless, perhaps a [hAP ac³](https://mikrotik.com/product/hap_ac3). I will post another blog about all that in the future.

PS: Yeah, I know what you're going to ask... 🕵️ Strange and surprising as it may sound, and in addition to all this just being a lot of fun, I actually do have an idea how to put this bandwidth to use... stay tuned!
