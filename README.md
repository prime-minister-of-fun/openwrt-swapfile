# openwrt-swapfile
The steps and files needed to enable swap on an openwrt device.
Assumption is the device has a connection to the internet.
This is a riff on this: https://forum.archive.openwrt.org/viewtopic.php?id=12419
My version has status and creates a 10MB swapfile.
Tune the service to your liking with etc-config-swap.
1. git clone my repo.
2. Ssh into the device.
3. opk add kmod-loop losetup swap-utils openssh-sftp-server
4. sftp etc-config-swap root@your-openwrt-address:/etc/config/swap
5. sftp etc-init.d-swap root@your-openwrt-address:/etc/init.d/swap
6. ssh root@your-openwrt-address
7. chmod +x /etc/init.d/swap
8. /etc/init.d/swap enable
9. /etc/init.d/swap start
10. /etc/init.d/swap status  #This should return success.

The luci UI Status->Overview should show the swap available.

##Problems
As of 2025-04-12, I had a problem with opkg not returning everything in the base repo.
I set up a new repo. System->Software->Configure opkg
The UI has a "custom feeds" text box where I added my device's repo again under a different name.  
DO NOT blindly copy/paste.  You need your architecture's repo, probably not mine.
src/gz openwrt_base_again https://downloads.openwrt.org/releases/24.10.0/packages/aarch64_cortex-a53/base/
And then "Update lists"
