# **IOT2050-Firmware-Update to V1.5.x**

These instructions will show the required steps update your firmware to version V1.5.x.

![iot2050-firmware-update](./docs/graphics/1-iot2050-firmware-update.png)

- [**IOT2050-Firmware-Update to V1.5.x**](#iot2050-firmware-update-to-v15x)
  - [**Requirements**](#requirements)
  - [**Operating**](#operating)
    - [**Check Preconditions**](#check-preconditions)
    - [**Transfer Files (using Example Image \< V1.4.x) and Update Firmware Update Tool**](#transfer-files-using-example-image--v14x-and-update-firmware-update-tool)
    - [**Transfer Files (using Example Image \>= V1.4.x)**](#transfer-files-using-example-image--v14x)
    - [**Optional: Clean eMMc on IOT2050 Advanced**](#optional-clean-emmc-on-iot2050-advanced)
    - [**Update the firmware**](#update-the-firmware)
  - [**Appendix**](#appendix)
    - [**Service and support**](#service-and-support)
    - [**Links \& Literature**](#links--literature)
  - [**Contribution and Contribution License Agreement**](#contribution-and-contribution-license-agreement)
  - [**Licence and Legal Information**](#licence-and-legal-information)

___

To be able to use the Example Image V1.5.x, it is required to update the firmware of the following devices:

|Model|MLFB|
|-|-|
|IOT2050 Basic|6ES7 647-0BA00-0YA2|
|IOT2050 Advanced|6ES7 647-0BA00-1YA2|
|IOT2050 M.2|6ES7647-0BB00-1YA2|
|IOT2050 SM|6ES7 647-0BA00-1AA2|

To update to the compatible firmare version V1.5.x the Example Images from V1.1.1 can be used. Example Image V1.5.x is recommended

After the update to firmware version V1.5.x it is not possible to boot the following operating systems:

- Example Image V1.0.2
- Industrial OS V2.x

Example Image from V1.1.1 can be used with the new firmware version.

## **Requirements**

This chapter contains the hardware and software required for the firmware update.

|**Hardware**||
|-|-|
|**SIMATIC IOT2050 & power supply**|To run the update a SIMATIC IOT2050 with power supply is required. This power supply must provide between 12 and 24V DC|
|**µSD card / USB flash drive / eMMc**|To boot from Example Image to perform the update, either a µSD card or USB drive is required. For the IOT2050 Advanced the internal eMMc can be used as well.|
|**Engineering Station**|To get remote access to the SIMATIC IOT2050 and to transfer files an Engineering station is required. In this example a PC with Windows 11 is used.|
|**Ethernet cable**|For an Ethernet Connection between the Engineering Station and the SIMATIC IOT2050 to establish a SSH connection an Ethernet cable is required.|

|**Software**||
|-|-|
|**Example Image**|V1.5x (recommended) - The firmware update cannot be performed with Example Image V1.0.2 or Industrial OS V2.x.|
|**Firmware Version**|at least V1.1.1 - The Example Image V1.5.x is not compatible with firmware version V1.0. Please use an additional SD card to perform the update if you want to keep your current image.|
|**ssh client**|To get remote access to the SIMATIC IOT2050 software is required. We always use [MobaXterm](https://mobaxterm.mobatek.net/) as it also allows to copy files from the Engineering Station to the IOT2050 per Drag & Drop. (Instead of MobaXterm you also can use Windows or Linux built-in ssh client - the files can also be transfered to the device using a USB flash drive)|
|**Firmware Update tool and firmware file**|The required `firmware-update-tool` is already part of the Example image V1.5.x. The `Firmware Update File` can be downloaded at the [IOT2050 Download Page](https://support.industry.siemens.com/cs/document/109741799/downloads-for-simatic-iot20x0)|

![Firmware Download](./docs/graphics/1-Download-Page-Firmware-File.png)

> We always recommend using the latest version of the Firmware Version V1.5.x.

## **Operating**

This chapter describes the steps necessary to update the firmware of the SIMATIC IOT2050 from any IOT2050 with at least firmware V1.1.1 to V1.5.x.

> The IOT2050 is set up with minimum FW version V1.1.1. Otherwise, the Example Image V1.5.x is not running, and the firmware update cannot be executed.

### **Check Preconditions**

The Example Image V1.4.x & V1.5.x already includes the ``iot2050-firmware-update-tool 0.2``.
When using an older Example Image then V1.4.x you need to additionally transfer the [iot2050-firmware-update-tool 0.2](/source/iot2050-firmware-update_0.2_arm64.deb).

>If you ran ``apt upgrade`` before, the firmware update won’t work, because the *BUILD_ID* is removed from the ``os-release-file``. In this case use a fresh Example Image.

|No.|Action|
|-|-|
|1.|Make sure the IOT2050 is running with at least firmware version V1.1.1 using the command `fw_printenv fw_version`|
||![fw-version-check](/docs/graphics/1-Firmware-Version-Check.png)|
|2.|Make sure your IOT2050 is running with Example Image V1.5.x (recommended) using the command `cat /etc/os-release`|
||![os-version-check](/docs/graphics/1-OS-Version-Check.png)|

### **Transfer Files (using Example Image < V1.4.x) and Update Firmware Update Tool**

If using Example Image V1.1.1, V1.2.1 & V1.3.1 you need to download the [iot2050-firmware-update-tool 0.2](/source/iot2050-firmware-update_0.2_arm64.deb) and transfer it to the device additionally.

> The following steps are only necessary for Example Images below V1.4.x. For Example Image >= V1.4.x the correct firmware-update-tool is also included. Skip this step.

|No.|Action|
|-|-|
|1.|Establish a connection with MobaXterm to copy the downloaded files to the IOT2050. Copy ``iot2050-firmware-update-tool 0.2`` and ``IOT2050-FW-Update-PKG-V01.05.xx-x-gxxxcxxx.tar.xz`` from the FW Update Files V1.5.x to the  `/mnt-directory` of the device.|
||![fw-version-check](/docs/graphics/1-Copy-Files.png)|
|2.|Navigate to ``/mnt-directory`` and install the transferred firmware update tool using ``dpkg -i iot2050-firmware-update_0.2_arm64.deb``. |
||![os-version-check](/docs/graphics/1-Firmware-Update-Tool-Update.png)|
||The device is now ready to perform the firmware update.|

### **Transfer Files (using Example Image >= V1.4.x)**

Example Image V1.4 and higher is delivered with ``iot2050-firmware-update-tool 0.2`` so it's only necessary to transfer the FW-Update-File.

> For older images it's additionaly necessary to transfer and update the ``iot2050-firmware-update-tool 0.2`` (see [step before](#transfer-files-using-example-image--v14x-and-update-firmware-update-tool))

|No.|Action|
|-|-|
|1.|Establish a connection with MobaXterm to copy the downloaded ``IOT2050-FW-Update-PKG-V01.05.xx-x-gxxxcxxx.tar.xz`` from the FW Update Files V1.5.x to the IOT2050.|
||![fw-version-check](/docs/graphics/1-Copy-Files-2.png)|
|2.|Navigate to ``/mnt-directory``. The device is now ready to perform the firmware update.|

### **Optional: Clean eMMc on IOT2050 Advanced**

Since the Firmware V1.5.x is not able to boot the Example Image V1.0.2 and Industrial OS V2.x, it may be required to clear the eMMc before updating the firmware when either Example Image V1.0.2 or Industrial OS V2.x is installed on it.

The update asks whether the current boot order should be kept or set back to defaults.

If your current boot order has mmc0 or usbx as first boot device and you keep the current settings, erasing the eMMc is not mandatory.

**If mmc1 is the first boot device or you reset the current settings, clearing the eMMc beforehand is mandatory to guarantee the device will run after the update and not stuck due to an incompatible OS.**

|No.|Action|
|-|-|
|1.|Clear the eMMc with ``mkfs.ext4 /dev/mmcblk1``|
||![fw-version-check](/docs/graphics/1-Clear-emmc.png)|

### **Update the firmware**

Now the firmware can be updated.

|No.|Action|
|-|-|
|1.|Update the firmware with the command ``iot2050-firmware-update IOT2050-FW-Update-PKG-V01.05.xx-x-gxxxcxxx.tar.xz`` and confirm that the device becomes unbootable with ``(Y)``. This won’t be the case if everything is done as described! The update process will begin.|
||![fw-version-check](/docs/graphics/1-Firmware-Update-1.png)|
|3.|Check installed firmware version after reboot `fw_printenv fw_version`|
||![fw-version-check](/docs/graphics/1-Check-FW-version.png)|

## **Appendix**

### **Service and support**

**SiePortal**
The integrated platform for product selection, purchasing and support - and connection of Industry Mall and Online support. The SiePortal home page replaces the previous home pages of the Industry Mall and the Online Support Portal (SIOS) and combines them.

- Products & Services: In Products & Services, you can find all our offerings as previously available in Mall Catalog.
- Support: In Support, you can find all information helpful for resolving technical issues with our products.
- mySieportal: mySiePortal collects all your personal data and processes, from your account to current orders, service requests and more. You can only see the full range of functions here after you have logged in.

You can access SiePortal via this address: [sieportal.siemens.com](https://sieportal.siemens.com/en-ww/home).

**Technical Support**
The Technical Support of Siemens Industry provides you fast and competent support regarding all technical queries with numerous tailor-made offers – ranging from basic support to individual support contracts.
Please send queries to Technical Support via Web form: [support.industry.siemens.com/cs/my/src](https://support.industry.siemens.com/cs/my/src?lc=en-WW)

**SITRAIN – Digital Industry Academy**
We support you with our globally available training courses for industry with practical experience, innovative learning methods and a concept that’s tailored to the customer’s specific needs.
For more information on our offered trainings and courses, as well as their locations and dates, refer to our web page: [siemens.com/sitrain](https://www.siemens.com/sitrain)

**Industry Online Support app**
You will receive optimum support wherever you are with the "Industry Online Support" app. The app is available for iOS and Android:

|![Available-App-Store](/docs/graphics/1-Available-App-Store.png)|![Available-Google-Store](/docs/graphics/1-Available-Google-Store.png)|
|-|-|
|![QR-Apple](/docs/graphics/1-QR-Apple.png)|![QR-Google](/docs/graphics/1-QR-Google.png)|

### **Links & Literature**

|No.|Topic|
|-|-|
|1|[Siemens Industry Online Support](https://support.industry.siemens.com/)|
|2|[IOT2050 Forum](https://support.industry.siemens.com/)|
|3|[IOT2050 Download Page](https://support.industry.siemens.com/cs/ww/en/view/109780231)|
|4|[IOT2050 Operating Instructions](https://support.industry.siemens.com/cs/ww/en/view/109779016)|
|5|[How To Set Up IOT2050 with Example Image](https://support.industry.siemens.com/tf/ww/en/posts/238945/)|

## **Contribution and Contribution License Agreement**

Thank you for your interest in contributing. Anybody is free to report bugs, unclear documentation, and other problems regarding this repository in the Issues section.
Additionally everybody is free to propose any changes to this repository using Pull Requests.

If you haven't previously signed the [Siemens Contributor License Agreement](https://cla-assistant.io/industrial-edge/) (CLA), the system will automatically prompt you to do so when you submit your Pull Request. This can be conveniently done through the CLA Assistant's online platform. Once the CLA is signed, your Pull Request will automatically be cleared and made ready for merging if all other test stages succeed.

## **Licence and Legal Information**

Please read the [Legal information](/LICENSE).
