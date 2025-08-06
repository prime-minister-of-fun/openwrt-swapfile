# openwrt-swapfile
The steps and files needed to enable swap on an openwrt device.
Assumption is the device has a connection to the internet.

This is a riff on this: https://forum.archive.openwrt.org/viewtopic.php?id=12419

My version has service status and creates a 10MB swapfile.
Tune the service to your liking with etc-config-swap.
1. git clone my repo.
2. Adjust etc-config-swap to your preferences.
3. Check the Problems section before continuing.
4. Ssh into the device.
5. On the command line: opk add kmod-loop losetup swap-utils openssh-sftp-server. if you are resource-starved, you might get away with copy/paste.
6. sftp etc-config-swap root@your-openwrt-address:/etc/config/swap
7. sftp etc-init.d-swap root@your-openwrt-address:/etc/init.d/swap
8. ssh root@your-openwrt-address
9. chmod +x /etc/init.d/swap
10. /etc/init.d/swap enable
11. /etc/init.d/swap start
12. /etc/init.d/swap status  #This should return success.

The luci UI Status->Overview should show the swap available.

**Problems**

DO NOT blindly copy/paste.  You need your architecture's repo, probably not mine.  And, maybe repo URLs change.

As of 2025-04-12, I had a problem with opkg not returning everything in the base repo on a very low-resource device. I set up a new repo. System->Software->Configure opkg.  I set up a new repo by using the UI.  
The UI has a "custom feeds" text box where I added my device's repo again under a different name.  The "custom feed" looks something like the next line.  _Your architecture's URL is probably different.

src/gz openwrt_base_again https://downloads.openwrt.org/releases/24.10.0/packages/aarch64_cortex-a53/base/

And then "Update lists."

After that, you should be able to run opk in the steps above.
