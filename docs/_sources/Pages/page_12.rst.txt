===============================
12.0 	Steps to flash LFM BIOS
===============================

LFM is Low Frequency Mode. This mode is enabled on gateway for efficient power usage.

Following steps and BIOS are applicable for below model of Device:

RBOX510 ATEX & C1D2 ANTI-EXPLOSIVE CERTIFIED ROBOST DIN-RAIL FANLESS SYS.W/ATOM E3827(1.75GHz)

.. list-table:: 
   :widths: 25 25 25 25
   :header-rows: 1

   * - 
     - LFM (500)
     - LFM_1750
     - Original
   * - Axiomtek ICO-300
     - 87842XV.102
     - None
     - 87842V.105
   * - Axiomtek RBOX510
     - XN.001
     - 0502_1500M.ROM 
     - A.103  

**Steps for converting to LFM**

Use files from **0502_1500m.zip** folder

1.	Create a FAT32 file-system on a USB stick, then dump each BIOS file structure to the root as needed.
2.	Prepare the LFM BIOS first on USB, then boot to the USB’s EFI device (boot priorities).
3.	After setting the Boot priorities save & exit from boot menu. You will be switched to Command window. Press any key to enter to command line, type flash.nsh to update the BIOS.
4.	Restart the HW and you will now be locked at 1750MHz
5.	Check frequency of device using command *$ cat /proc/cpuinfo | grep “MHz”*

**Steps for reverting to normal Frequency**

Use files from **A.103** folder

1.	Create a FAT32 file-system on a USB stick, then dump each BIOS file structure to the root as needed.
2.	Prepare the LFM BIOS first on USB, then boot to the USB’s EFI device (boot priorities).
3.	After setting the Boot priorities save & exit from boot menu. You will be switched to Command window. Press any key to enter to command line, type flash.nsh to update the BIOS.
4.	Restart the HW and you will now be locked at 1750MHz
5.	Check frequency of device using command *$ cat /proc/cpuinfo | grep “MHz”*

