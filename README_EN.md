# Firmware Installation Guide for Cudy WR1300 v4

🌐 [Русская версия (README.md)](README.md)

This guide is for the **Cudy WR1300 v4** router and describes the procedure verified by the video's author. Do not use this firmware image on any other hardware revision.

> ⚠️ **Warning:** Flashing a router always carries a risk of bricking it. Look up the available recovery methods beforehand, and do not disconnect the power while flashing.

## 1. Preparation

1. Check the sticker on the bottom of the router. It must show revision **WR1300 V4.0**.
2. Connect your computer to the router with an Ethernet cable.
3. Download and unzip the intermediate image from [Cudy's official Google Drive](https://drive.google.com/drive/folders/1BKVarlwlNxf7uJUtRhuMGUqeCa5KpMnj?usp=sharing). It is needed to remove the signature check that blocks third-party firmware. Choose the file for v4 (`WR1300 V4.0.zip`). More details: [Cudy OpenWrt Software Download](https://www.cudy.com/blogs/faq/openwrt-software-download).
4. Download the custom build `*-squashfs-sysupgrade.bin` from the **[Releases](../../releases)** section of this repository.

## 2. Flash the Intermediate Cudy Firmware

1. Open the web interface of the stock Cudy firmware (`192.168.10.1` by default).
2. Open the firmware update page (Advanced Settings → System → Firmware), select the intermediate image you downloaded, and confirm the update.
3. Wait until the process finishes and the router reboots. Do not disconnect the power.

*After flashing the intermediate image, the OpenWrt web interface is available at `http://192.168.1.1`. There is no password yet: just press Enter on the login page.*

## 3. Flash the Custom OpenWrt Build

1. In the web interface of the intermediate OpenWrt, go to **System → Backup / Flash Firmware**.
2. In the **Flash new firmware image** section, click **Flash image...**.
3. Select the `*-squashfs-sysupgrade.bin` file you downloaded from this repository's Releases.
4. **Important:** uncheck **Keep settings** (this makes it a clean install).
5. Confirm and wait for the router to reboot.

After it boots, log in to the new web interface. On the first start of the custom build, you will need to set the system up from scratch.

---

## 🛠️ Building from Source

* Want to build this firmware yourself? The build instructions and the patch are in **[BUILD_EN.md](BUILD_EN.md)**.

---

## 📺 Video Guide

* The full process of building and flashing is shown in this [**YouTube video**](https://youtu.be/OzstybJYyGg) (in Russian).