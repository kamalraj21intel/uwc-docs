=====================================
5.0  Container Configuration Settings
=====================================

------------------------------
5.1 	Deactivate Auto Update
------------------------------

Once all the containers are deployed successfully, disable system’s auto update feature as specified in the below sections. Auto update feature is enabled by default in Ubuntu.

These steps are optional. It is needed to switch off auto updates of packages (Package-Lists, periodic etc) when connected to internet.

5.1.1 	Deactivate Unattended Upgrades
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
To deactivate unattended upgrades, we need to edit

 */etc/apt/apt.conf.d/20auto-upgrades* file and do the following changes.

        1.	Disable update package list by changing setting

            **from** *APT::Periodic::Update-Package-Lists* **"1"**

            **to** *APT::Periodic::Update-Package-Lists* **"0"**  


        2.	Disable unattended upgrade by changing setting
        
            **from** *APT::Periodic::Unattended-Upgrade* **"1"**

            **to** *APT::Periodic::Unattended-Upgrade* **"0"**

5.1.2 	Deactivate Periodic Unattended Upgrades
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To deactivate periodic unattended upgrades edit the */etc/apt/apt.conf.d/10periodic* file and do the following changes:

    1.	Disable update package list by changing setting 

        **from** *APT::Periodic::Update-Package-Lists* **"1"** 

        **to** *APT::Periodic::Update-Package-Lists* **"0"**

    2.	Disable download upgradable packages by changing setting

        **from** *APT::Periodic::Download-Upgradeable-Packages* **"1"**

        **to** *APT::Periodic::Download-Upgradeable-Packages* **"0"**
    
    3.	Disable auto clean interval by changing setting 

        **from** *APT::Periodic::AutocleanInterval* **"1"**

        **to** *APT::Periodic::AutocleanInterval* **"0"**

5.1.3 	Deactivate Scheduled Upgrades
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To deactivate scheduled download run the following commands:

.. code-block:: sh

    	sudo systemctl stop apt-daily.timer
    	sudo systemctl disable apt-daily.timer
    	sudo systemctl disable apt-daily.service
    	sudo systemctl daemon-reload


