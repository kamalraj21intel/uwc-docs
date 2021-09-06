========================
2.0 Release Notes
========================

---------------------
2.1 Feature list- supported in UWC-v1.6
---------------------

1.	Enabled Data Persistence feature in UWC
  a. Integrated InfluxDBConnector & Telegraf services of EII's TICK stack timeseries services with UWC.
  b. Data persistence enabled for all 6 operations - ROD (Read on demand), WOD (Write on demand) & Polling in the combinations of both Realtime & non-realtime operations.
  c. Data Retention policy enabled giving flexibility to users to customize the data retention period in InfluxDB.
2.	UWC migrated to EII 2.6
3.   Network mode host removed from UWC micro-services and services use docker network.
4.	Sample database publisher application use case provided which serves as a reference
5.	Enabled UWC on Ubuntu 20.04-LTS Operating system

.. note:: Build time will increase due to addition of two more components needed for data persistence depending on system configurations.

------------------------------------
2.2 Feature list- supported in UWC-v1.5
------------------------------------
1.	Eclipse foundation Sparkplug standard Template feature support  
  a.	User Defined Template (UDT) definition and instance  
  b.	Publish –Subscribe interface for third party App for publishing UTD and instances  
2.	Seamless edge to cloud connectivity with AWS IoT site wise 
  a.	UDT publishing  
  b.	Realtime Tags update 
  c.	Realtime connection/disconnect update 
3.	Data Conversion and transformation 
  a.	Data ingested by Modbus services is converted to data type defined in the configuration 
  b.	Data ingested by Modbus services is transformed based on the scale factor defined in the configurations 
4.	UWC migrated to EII 2.5 
5.	UWC open source with MIT license on GitHub 

------------------------------------
2.3 Feature list supported in UWC-v1.0 
------------------------------------
1.	Harden Modbus TCP protocol stack and application supporting soft real-time control 
2.	Harden Modbus RTU protocol stack and application supporting soft real-time control 
3.	User defined System model configuration in YAML format 
4.	MQTT Publish-Subscribe interface for process control APP development 
5.	Internal EII Data bus with IPC mode  
6.	Eclipse Foundation Sparkplug specification compliant SCADA RTU 
7.	Sample KPI testing for control loop benchmarking 
8.	Device Management with OTA (Over-The-Air) firmware, OS and Docker container update 

------------------------------------
2.4 Changes to Existing Features 
------------------------------------
1.	In UWC-v1.6, Duplicate "cout" prints removed & replaced with UWC logger prints
2.	In UWC-v1.6, Updated readme for RT patch installation steps for ubuntu 20.04
3.	In UWC-v1.6, Removed the PDF version of user guide from https://github.com/open-edge-insights/uwc with Sphinx documentation at  https://github.com/open-edge-insights/uwc-docs
4. In UWC-v1.6, KPI bugs on random KPI-app crashing, non-linearity of bad records versus control loop count addressed

------------------------------------
2.5 Unsupported or Discontinued Features 
------------------------------------
*	None 


