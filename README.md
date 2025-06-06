# Install-Vmware-Tools-on-GhostBSD-FreeBSD-
Guide for Installing Vmware Tools on GhostBSD (FreeBSD)

During the installation of GhostBSD on the Vmware Workstation, the Vmware Tools installation guide was fragmented, so I organized it separately. If you follow this guide, mouse integration (seamless mouse) mode and window sizing autosize work normally. I've tested it on GhostBSD at the moment, but I think you can activate it in FreeBSD in a similar way with the same kernel.

First, let's update the package in the OS. Run the `pkg update` and `pkg upgrade` commands.

Next, install the package related to vmware tools. `sudo pkg install open-vm-tools xf86-input-vmouse xf86-video-vmware`

After that, we need to modify a total of two files: `/etc/rc.conf` and `/usr/local/etc/X11/xorg.conf.d/vmware.conf`

Let's open `/etc/rc.conf` to add the phrase below.
```
hald_enable="YES"
dbus_enable="YES" #Some are already entered
moused_enable="YES"
vmware_guest_vmblock_enable="YES"
vmware_guest_vmhgfs_enable="YES"
vmware_guest_vmmemctl_enable="YES"
vmware_guest_vmxnet_enable="YES"
vmware_guestd_enable="YES"
```
Once you're done entering, open `/usr/local/etc/X11/xorg.conf.d/vmware.conf (if you don't have one, let's create one)` and add the phrase below.
```
Section "ServerFlags"
    Option "AutoAddDevices" "false"
EndSection

Section "InputDevice"
    Identifier "Mouse0"
    Driver "vmmouse"
    Option "Device" "/dev/sysmouse"
EndSection

Section "Device"
    Identifier "Card0"
    Driver     "vmware"
EndSection
```

Once the addition is complete, the reboot will allow the vmware tools to function normally.
