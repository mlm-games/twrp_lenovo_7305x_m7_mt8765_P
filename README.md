# Maintainer's notes

## Android twrp device tree for Lenovo TB-7305X (mt8765_P)

Works after unlocking bootloader and flashing recovery by fastboot.

```
fastboot flash recovery recovery.img
```

You can use the recovery name instead of <recovery.img>

## Features

Works:

- [X] ADB
- [ ] Decryption
- [X] Display
- [X] Touch
- [X] Flashing
- [X] MTP
- [X] Sideload
- [X] USB OTG
- [X] Vibrator


#### Thanks to 
- The recovery from 4pda for reference

## Notes
> This device does not have a vbmeta partition so you may have to either replace hexcode of string "verify" in boot.img to 0s or flash the disable_dm_verity_force_decrypt.zip afterwards to disable avb

> The kernel (i think but it could also be the modem parts or nvdata as the logcat show errors based on them) does not allow booting into some GSIs (tested, didn't go past boot animation -> lineage-os A14 to A9, arrowos A11, caos-A11, /e/-a10) but it does allow adb logcatting for AOSP A9 (look at the start)...

> If you have tried any GSI and it boots, please open an issue

---

# Generic info below

## Unofficial TWRP Recovery for Lenovo Tab M7 (TB-7305X)

![TWRP Logo](https://github.com/TeamWin/Team-Win-Recovery-Project.github.io/raw/master/images/logo.png)

This is an **unofficial** build of TWRP (Team Win Recovery Project) for the **Lenovo Tab M7 (TB-7305X)**.

## Device Information

| Device Info     | Specification                                   |
|-----------------|-------------------------------------------------|
| **Device Name** | Lenovo Tab M7                                   |
| **Model Number**| TB-7305X                                        |
| **Codename**    | TB-7305X                                        |
| **SoC**         | MediaTek MT8321 (32-bit) or 8765B (64 bit)      |
| **CPU**         | Quad-core 1.3 GHz Cortex-A7                     |
| **GPU**         | Mali-400 MP2                                    |
| **RAM**         | 1 GB                                            |
| **Storage**     | 8 GB / 16 GB internal storage                   |
| **MicroSD**     | Up to 128 GB microSD card support               |
| **Battery**     | Non-removable Li-Po 3450 mAh                    |
| **Display**     | 7.0 inches, 600 x 1024 pixels, IPS LCD          |
| **Android OS**  | Android 9.0 Pie (Go edition)                    |
| **Release Date**| September 2019                                  |
| **Maintainer**  | *[Me]*                                          |

---

## Features

- Full touch support
- Backup and restore capabilities
- Flashing of zip and image files
- ADB access in recovery mode
- MTP support for file transfers
- **Note:** Data decryption is not be supported currently due to device limitations

---

## What's Working

- ✅ Booting into recovery
- ✅ Touchscreen functionality
- ✅ Mounting system and vendor partitions
- ✅ ADB and MTP access
- ✅ Flashing of zip and image files
- ✅ Backup and restore functions

---

## Known Issues

- ❌ **Data Partition Decryption:** Unable to decrypt /data partition encrypted with default Android encryption
- ❌ **SELinux:** May need to be set to permissive mode for certain operations
- ❌ **Others:** Please report any issues encountered

---

## Installation Instructions

> **Warning:** Installing custom recovery and modifying your device can void your warranty and may result in device malfunctions if not done properly. Proceed at your own risk.

### Prerequisites

- **Unlocked Bootloader:** [Instructions here](#unlocking-the-bootloader)
- **ADB and Fastboot Tools:** Installed on your PC ([Download](https://developer.android.com/studio/releases/platform-tools))
- **USB Cable:** Original or compatible USB cable for data transfer
- **TWRP Image File:** Download the recovery image from the [Downloads](#downloads) section

### Steps

1. **Enable Developer Options and USB Debugging on Your Device**

   - Go to **Settings > About Tablet**
   - Tap on **Build Number** seven times until it says "You are now a developer!"
   - Go back to **Settings > System > Developer Options**
   - Enable **USB Debugging**

2. **Unlocking the Bootloader**

   - **Note:** This process will erase all data on your device.
   - Connect your tablet to your PC via USB.
   - Open a command prompt or terminal window on your PC.
   - Enter the following commands:

     ```bash
     adb reboot bootloader
     fastboot oem unlock
     (or)
     fastboot flashing unlock
     (or)
     fastboot flashing unlock_critical
     ```
   - Follow any on-screen prompts to confirm bootloader unlocking.

3. **Flash TWRP Recovery**

   - Place the downloaded `twrp.img` file in your ADB/Fastboot directory.
   - From the bootloader mode, flash TWRP:

     ```bash
     fastboot flash recovery twrp.img
     ```
     *(Replace `twrp.img` with the actual filename if different.)*

   - After flashing, boot directly into TWRP to prevent the stock recovery from replacing it:

     ```bash
     fastboot reboot recovery
     ```

4. **Optional Steps in TWRP (Depends per device)**

   - **Prevent Stock Recovery Restoration:**
     - In TWRP, go to **Mount** and ensure **System** is mounted as **Read/Write**.
   - **Disable DM-Verity and Force Encryption:**
     - Flash the **Disable DM-Verity and Force Encryption** zip file (if applicable).
   - **Backup Your Device:**
     - It is highly recommended to create a full backup before making further changes.

---

## Build Instructions

To build TWRP for the Lenovo Tab M7 (TB-7305X), follow these steps.

### Prerequisites

- **Operating System:** Linux (Ubuntu recommended)
- **Required Tools:** Git, Repo tool, Java Development Kit (OpenJDK 8), ADB and Fastboot
- **Disk Space:** At least 100 GB free
- **RAM:** Minimum 8 GB recommended
- **Internet Connection:** Stable and fast for downloading source code

### Steps

1. **Install Required Packages**

   ```bash
   sudo apt-get update
   sudo apt-get install git-core repo gnupg flex bison gperf build-essential zip curl zlib1g-dev \
   gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev \
   lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip python
   ```

2. **Initialize the Repo**

   ```bash
   mkdir twrp-workspace
   cd twrp-workspace
   repo init -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_omni.git -b twrp-9.0
   ```

3. **Sync the Source Code**

   ```bash
   repo sync
   ```

4. **Clone the Device Tree**

   ```bash
   git clone https://github.com/yourusername/android_device_lenovo_TB-7305X.git device/lenovo/TB-7305X
   ```

5. **Set Up the Build Environment**

   ```bash
   source build/envsetup.sh
   ```

6. **Select the Device to Build**

   ```bash
   lunch omni_TB-7305X-eng
   ```

7. **Start the Build**

   ```bash
   mka recoveryimage
   ```

8. **Retrieve the Built Recovery Image**

   - After the build completes successfully, find the recovery image at:

     ```
     out/target/product/TB-7305X/recovery.img
     ```

---

## Credits

- **Team Win Recovery Project** for their exceptional recovery project
- **Community Contributors** who provided support and resources

---

**Disclaimer:** Installing custom recovery and modifying your device involves risks. **Proceed at your own risk.** Neither the developers nor anyone else is responsible for any damage that may occur to your device by using this software.
