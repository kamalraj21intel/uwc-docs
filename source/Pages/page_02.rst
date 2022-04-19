==================
2.0 Release Notes
==================

------------------------------------
2.1 Feature list- supported in UWC-v2.0 
------------------------------------

* All UWC micro-services/features ported to new UWC design following "new EMB (EII Msg bus) topic format".
* The backward compatibility with older MQTT topic format is maintained.
* Sparkplug-Bridge which historically was compatible with only MQTT-broker (bus) has now been ported to work with EMB (EII Msg Bus).
* Multi stage build support enabled for all UWC services. Hence, enabling reducing the build time &  image sizes of all UWC services drastically.
* UWC docker images pushed to remote docker hub (hub.docker.com). Hence, giving an option for developers to either pull pre-built images directly from docker hub or locally build the container images.
* New Recipe files are added to cover more usecases. 
* Repo manifest based cloning enabled instead of git clone method. 
* UWC codebase rebased on top of latest validated tag of EII - v3.0. Follow the clone commands as below:

.. code-block:: sh

    •	repo init -u https://github.com/open-edge-insights/eii-manifests.git -b refs/tags/v2.0
    •	repo init -m uwc.xml
    •	repo sync

---------------------------------------
2.2 Feature list- supported in UWC-v1.5.1 (Hotfix Release)
---------------------------------------

* UWCv1.5 rebased & migrated to EII v2.5.2.

---------------------------------------
2.3 Feature list- supported in UWC-v1.6.1 (Hotfix Release)
---------------------------------------

* UWC migrated to EII v2.6.1
* Fixed issue of high number of recursive queries hiting DNS server
* Fixed issue related with ESH installer not displaying some prints in Sparkplug use cases 
*	Minor user guide & ReadMe updates   

---------------------------------------
2.4 Feature list- supported in UWC-v1.6
---------------------------------------

* Enabled Data Persistence feature in UWC
   *  Integrated InfluxDBConnector & Telegraf services of EII's TICK stack timeseries services with UWC.
   *  Data persistence enabled for all 6 operations - ROD (Read on demand), WOD (Write on demand) & Polling in the combinations of both Realtime & non-realtime operations.
   *  Data Retention policy enabled giving flexibility to users to customize the data retention period in InfluxDB.
*	UWC migrated to EII 2.6
*  Network mode host removed from UWC micro-services and services use docker network.
*	Sample database publisher application use case provided which serves as a reference
*	Enabled UWC on Ubuntu 20.04-LTS Operating system

.. note:: Build time will increase due to addition of two more components needed for data persistence depending on system configurations.

----------------------------------------
2.5 Feature list- supported in UWC-v1.5
----------------------------------------
*	Eclipse foundation Sparkplug standard Template feature support
  	*  User Defined Template (UDT) definition and instance 
  	*  Publish –Subscribe interface for third party App for publishing UTD and instances  
*	Seamless edge to cloud connectivity with AWS IoT site wise 
  	*  UDT publishing
  	*  Realtime Tags update 
    *  Realtime connection/disconnect update 
*	Data Conversion and transformation 
    *  Data ingested by Modbus services is converted to data type defined in the configuration
    *  Data ingested by Modbus services is transformed based on the scale factor defined in the configurations   
*	UWC migrated to EII 2.5 
*	UWC open source with MIT license on GitHub 

------------------------------------
2.6 Feature list supported in UWC-v1.0 
------------------------------------
*	Harden Modbus TCP protocol stack and application supporting soft real-time control 
*	Harden Modbus RTU protocol stack and application supporting soft real-time control 
*	User defined System model configuration in YAML format 
*	MQTT Publish-Subscribe interface for process control APP development 
*	Internal EII Data bus with IPC mode  
*	Eclipse Foundation Sparkplug specification compliant SCADA RTU 
*	Sample KPI testing for control loop benchmarking 
*	Device Management with OTA (Over-The-Air) firmware, OS and Docker container update 

------------------------------------
2.7 Changes to Existing Features 
------------------------------------
*	In UWC-v1.6, Duplicate "cout" prints removed & replaced with UWC logger prints
*	In UWC-v1.6, Updated readme for RT patch installation steps for ubuntu 20.04
*	In UWC-v1.6, Removed the PDF version of user guide from https://github.com/open-edge-insights/uwc with Sphinx documentation at  https://github.com/open-edge-insights/uwc-docs
* In UWC-v1.6, KPI bugs on random KPI-app crashing, non-linearity of bad records versus control loop count addressed

------------------------------------
2.8 Unsupported or Discontinued Features 
------------------------------------
*	None 


