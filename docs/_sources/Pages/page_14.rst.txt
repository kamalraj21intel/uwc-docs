===============================
14.0 RT Patch (Optional)
===============================

-----------------------------
14.1 	Steps to Choose and Apply RT Kernel Patch 
-----------------------------

The latest version of UWC has been tested with Ubuntu 20.04.2 LTS. Check the kernel version corresponding to the Ubuntu OS version being used and map it with the correct RT kernel patch.

Use the links below to map the kernel version with theRT kernel patch: 

  * https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/ 
  * https://mirrors.edge.kernel.org/pub/linux/kernel/

--------------------------------------------
14.2 	Steps To Apply RT Kernel Patch
--------------------------------------------

Default Ubuntu 20.04.2 LTS kernel version is 5.4.0-80.

1.	Make a working directory on the system: 

.. code-block:: sh

  $ mkdir ~/kernel && cd ~/kernel

2.	Download kernel in ~kernel directory created in step 1

•	Download kernel manually 

**Link for download** - https://www.kernel.org/pub/linux/kernel/

This will download kernel manually or use following command to download it from command line inside current directory.

.. code-block:: sh
  $ wget https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-5.4.129.tar.gz

**Recommendation:** Please get Linux kernel version 5.4.129

•	Download preempt RT patch

**Link for download -** https://www.kernel.org/pub/linux/kernel/projects/rt/ this will download patch manually or use following command to download it from command line inside current directory.

.. code-block:: sh

  $ wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/5.4/older/patch-5.4.129-rt61.patch.gz 

**Recommendation:** Please get PREEMPT_RT version 5.4.129-rt61

3.	Unzip the kernel using following command

.. code-block:: sh

  $ tar -xzvf linux-5.4.129.tar.gz

4.	Patch the kernel

.. code-block:: sh

  $ cd linux-5.4.129
  $ gzip -cd ../patch-5.4.129-rt61.patch.gz | patch -p1 –verbose 

5.	Launch the graphical UI for setting configurations

The next command launches a graphical menu in the terminal to generate the .config file.

.. code-block:: sh

  $ make menuconfig

**Graphical UI is shown below:**

.. figure:: Doc_Images/image9.png
    :scale: 60 %
    :align: center

    Figure 11.1: Main launching screen

6.	Select the preemption model as Basic RT using tab key on keyboard

    1)	Select and enter on “General setup” option.
    2)	Select and Enter on Preemption Model (Voluntary Kernel Preemption (Desktop))
    3)	Select and Enter on Preemption Model (Fully Preemptible Kernel (RT))
    4)	After successful selection click on save button and then come back to main page using Esc button on keyboard. 

Refer the following screen capture for more details

.. figure:: Doc_Images/image12.png
    :scale: 60 %
    :align: center

    Figure 11.2: Preemption Model (Fully Preemptible Kernel (RT))

.. figure:: Doc_Images/image11.png
    :scale: 60 %
    :align: center

    Figure 11.3 Fully Preemption Kernel (RT)

Save and exit

7. To save the current setting click on *<save>* button and then exit the UI using *<exit>* button.

8.	Compile the kernel (Execute the following commands)

.. code-block:: sh

  $ make –j20
  $ sudo make INSTALL_MOD_STRIP=1 modules_install -j20
  $ sudo make install -j20


9.	Verify that initrd.img-'5.4.129-rt61, vmlinuz-'5.4.129-rt61, and config-'5.4.129-rt61 are generated in /boot directory and update the grub.

.. code-block:: sh

  $ cd /boot
  $ ls
  $ sudo update-grub

10. Verify that there is a menu entry containing the text "menuentry 'Ubuntu, with Linux '5.4.129-rt61" in /boot/grub/grub.cfg file 

11. To change default kernel in grub, edit the GRUB_DEFAULT value in /etc/default/grub to your desired kernel. 

.. note::
   
   0 is the 1st menuentry

12.	Reboot and verify using command

.. code-block:: sh

  $ sudo reboot

13. Once the system reboots, open the terminal and use uname -a to check the kernel version

Command will show below output for successfully applied RT patch – 
*Linux ubuntu 5.4.129-rt61 #1 SMP PREEMPT RT Tue Mar 24 17:15:47 IST 2020 x86_64 x86_64 x86_64 GNU/Linux*

.. note::
    If RT patch installation fails during make commands, comment the  “CONFIG_SYSTEM_TRUSTED_KEYS" and "CONFIG_MODULE_SIG_KEY” lines from/boot/config<version> and try the installation again (if enabled, the server will lose the secure boot).  


