# Asus RT-AX52 WiFi Router

Installation notes for OpenWRT on ASUS RT-AX52.

## Asus Manuals

https://www.asus.com/networking-iot-servers/wifi-routers/asus-wifi-routers/rt-ax52/helpdesk_manual?model2Name=RT-AX52

## Asus RT-AX52 Original Firmware

Download the original firmware from ASUS in case you need to restore the device. Keep this with the manuals and these notes.

https://www.asus.com/networking-iot-servers/wifi-routers/asus-wifi-routers/rt-ax52/helpdesk_bios?model2Name=RT-AX52


    ASUS RT-AX52 Firmware version 3.0.0.4.388_33911
    Version 3.0.0.4.388_33911
    25.23 MB
    2025/04/25
    SHA-256 ：769FC8A3CE28F14CAE08DE1DA6BAE711BE3926BE9C5E464881824029934DE550 

The firmware is in the downloaded file `FW_RT_AX52_300438833911.zip`

## Asus Firmware Restoration Tool

For reference, in case messing with the firmware fails, there is a `Firmware Restoration` tool from ASUS. It is a Windows tool.
See the ASUS user manual, section 4.2.


## OpenWRT Hardwaredata

https://openwrt.org/toh/hwdata/asus/asus_rt-ax52

## OpenWRT Firmware

https://downloads.openwrt.org/releases/24.10.2/targets/mediatek/filogic/openwrt-24.10.2-mediatek-filogic-asus_rt-ax52-initramfs-kernel.bin

### Firmware Image and SHA Hashes

https://downloads.openwrt.org/releases/24.10.2/targets/mediatek/filogic/

`asus_rt-ax52-initramfs-kernel.bin`

SHAT256sum `acd3234d7d53fbb0757957fc44d9650ff607cd50e924abd3bba5210c87fc7219`

#### Verify Firmware SHA Checksum in PowerShell

See guide here: https://openwrt.org/docs/guide-quick-start/verify_firmware_checksum


    cd Downloads
    certutil -hashfile openwrt-24.10.2-mediatek-filogic-asus_rt-ax52-initramfs-kernel.bin sha256

### Install OpenWRT Firmware on Router Running ASUS Firmware

So according to the OpenWRT docs in the hardware page the installation method is `GUI OEM`. See https://openwrt.org/toh/hwdata/asus/asus_rt-ax52

So we use the built-in tooling, the web admin GUI.

From the Asus user manual, section 3.1.3:

To upgrade the firmware:

  1. From the navigation panel, go to `Advanced Settings` > `Administration` > `Firmware Upgrade`.
  2. In the `New Firmware File` field, click `Browse` to locate the downloaded file.
  3. Click `Upload`.

Note that the downloaded file with the ASUS firmware is a zip, you need to unzip first and uploaded the firmware file from there.

See also the online ASUS FAQ: https://www.asus.com/support/faq/1008000/

To install the OpenWRT firmware we generally use the stable versions downloaded above. Try that.

### When 24.10.2 Stable Fails to Install

So, perhaps you get this error when installing the Stable 24.10.2 build above in the 

    Firmware upgrade unsuccessful. This may result from incorrect image or error transmission. Please check the version of firmware and try again.

If you feel courageous try with the snapshot build and then sysupgrade to the 24.10.2 Stable sysupgrade build.

See this thread: https://forum.openwrt.org/t/help-installing-openwrt-on-rt-ax52/236402/3

The files are listed on the https://openwrt.org/toh/hwdata/asus/asus_rt-ax52 page:

Firmware SNAPSHOT install file:

https://downloads.openwrt.org/snapshots/targets/mediatek/filogic/openwrt-mediatek-filogic-asus_rt-ax52-initramfs.trx

Then once OpenWrt is installed, upgrade to the stable version. The image is here:

https://downloads.openwrt.org/releases/24.10.2/targets/mediatek/filogic/openwrt-24.10.2-mediatek-filogic-asus_rt-ax52-squashfs-sysupgrade.bin

Snapshot builds do not have the LuCI web interface installed, that stable builds provide at http://192.168.1.1, so you have to use ssh to
do this, see this guide at: https://openwrt.org/docs/guide-quick-start/sshadministration


    ssh root@192.168.1.1


There is initially no root password. Set this.

Here is how to install the sysupgrade:  

https://openwrt.org/docs/guide-user/installation/sysupgrade.cli


Now the snapshot is not ready for any of this (no SCP, no SFTP), 
so we have to find another way. 

Do this in WSL (from Linux):

    $ scp openwrt-24.10.2-mediatek-filogic-asus_rt-ax52-squashfs-sysupgrade.bin root@192.168.1.1:/tmp
    root@192.168.1.1's password:
    ash: /usr/libexec/sftp-server: not found
    scp: Connection closed

However, we can copy the image using this command (use the WSL Linux, this does not work in SSH on Windows):

    ssh root@192.168.1.1 "cat > /tmp/openwrt-24.10.2-mediatek-filogic-asus_rt-ax52-squashfs-sysupgrade.bin" < openwrt-24.10.2-mediatek-filogic-asus_rt-ax52-squashfs-sysupgrade.bin

Note that the /tmp directory is in RAM. There is no network enabled on the device yet.


### Upgrade OpenWRT Firmware on Router

This is called "sysupgrade". `ssh` into the the router. Note the
welcome message, we are running `OpenWrt SNAPSHOT, r30497-d8e738e5c4`. Now install the stable sysupgrade we just copied 
over to the `/tmp` directory:

    sysupgrade -v /tmp/openwrt-24.10.2-mediatek-filogic-asus_rt-ax52-squashfs-sysupgrade.bin

This will log out your SSH session. Let's hope it comes back up.

...

Wait a bit...

This will change the SSH key of the host, so while you wait you can delete the key from your `~/.ssh/known_hosts` file.

Now log in again.

    ssh root@192.168.1.1

And we see the stable build, `OpenWrt 24.10.2, r28739-d9340319c6`.

We also see that there is no password defined. Everything is gone.

Set the password again using the `passwd` command.

## Set up Networking on the Router

Now, the Stable firmware image has the LuCI we interface, so we can now open http://192.168.1.1

You can now follow the setup guide.




