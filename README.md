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

## OpenWRT Hardwaredata

https://openwrt.org/toh/hwdata/asus/asus_rt-ax52

## OpenWRT Firmware

https://downloads.openwrt.org/releases/24.10.2/targets/mediatek/filogic/openwrt-24.10.2-mediatek-filogic-asus_rt-ax52-initramfs-kernel.bin

### Firmware Image and SHA Hashes

https://downloads.openwrt.org/releases/24.10.2/targets/mediatek/filogic/

`asus_rt-ax52-initramfs-kernel.bin`

SHAT256sum `acd3234d7d53fbb0757957fc44d9650ff607cd50e924abd3bba5210c87fc7219`

#### Verifyh Firmware SHA In PowerShell

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

Note that the downloaded file is a zip, you need to unzip first and uploaded the firmware file from there.

See also the online ASUS FAQ: https://www.asus.com/support/faq/1008000/


#### Asus Firmware Restoration Tool

Plan B, in case the above fails. The `Firmware Restoration` tool from ASUS. It is a Windows tool.
See the ASUS user manual, section 4.2.


