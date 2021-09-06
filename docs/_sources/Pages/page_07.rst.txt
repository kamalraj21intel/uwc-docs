========================
7.0  Site Configurations
========================

This section provides configurations required to configure site, wellhead, device and points for UWC containers.

-------------------------------------
7.1	System Level Global Configuration
-------------------------------------

This file contains configurations to be used for operations across UWC containers for (Modbus-TCP, Modbus-RTU, MQTT-Bridge.)

Global_Config.yml file location - /opt/intel/eii/uwc_data/common_config

Based on realtime requirement, operations are classified into following sub-operations:

1. Polling realtime

2. Polling non-realtime

3. On-demand read realtime

4. On-demand read non-realtime

5. On-demand write realtime

6. On-demand write non-realtime

7. SparkPlug communication for Sparkplug-Bridge

8. Default scale factor


7.1.1 	Settings for Polling and On-Demand operation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Following is a sample for Polling operation. Similar is applicable for On-demand operations:

Global:
    Operations:
        - Polling:
            default_realtime: false

            realtime:
              operation_priority: 4

              retries: 1

              qos: 1
            non-realtime:
              operation_priority: 1

              retries: 0

              qos: 0

Following is a description of each field.

.. figure:: Doc_Images/image8_1.png
    :scale: 60 %
    :align: center

*Sub fields for “realtime” and “non-realtime” group are as follow*

.. figure:: Doc_Images/image8_2.png
    :scale: 60 %
    :align: center

If incorrect value is specified for any of above fields, a default value (listed below) will be used:

    default_realtime: false

    operation_priority: 1

    retries: 0

    qos: 0

If configuration parameter or section is missing for any of the sub-operation (related to Polling and On-Demand), then default values mentioned above will be used.

.. _link:

7.1.2	Settings for Sparkplug-Bridge – SparkPlug communication operation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Following is a sample:

Global:
    Operations:
        - SparkPlug_Operation:

            group_id: "UWC nodes"
            
			edge_node_id: "RBOX510"

Above are also the default values for mentioned parameters. If configuration parameter or section is missing for SparkPlug communication, then default values mentioned above will be used.

The parameters here are used to form SparkPlug formatted topic name. 

These values should properly be configured to ensure correct representation of data under SCADA Master. 

Following is a description of each field.

.. figure:: Doc_Images/table6.png
    :scale: 70 %
    :align: center


7.1.3	Settings for default scale factor
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Following is a sample:

Global:
    Operations:
    .

    .

    .
default_scale_factor: 1.0

.. figure:: Doc_Images/table7.png
    :scale: 70 %
    :align: center

------------------------------------------
7.2 	How to Configure Site and Wellhead
------------------------------------------

There is one file which lists down reference to device-groups (i.e., wellheads) controlled by one UWC gateway. Ideally, in one UWC gateway there is one TCP and one RTU container. Please note one RTU container can manage communication with multiple RTU networks.

---
*file:*

    *version: "1.0.0"*

    *author: "Intel"*

    *date: "Sun Sep 1 13:34:13 PDT 2019"*

    *description: "Common device group file for TCP and RTU devices"*

*devicegrouplist:*

   *- "Device_group1.yml"*

   *- "Device_group2.yml"*

Above example shows “Device_group1” and “Device_group2” as a reference to group of devices. “Device_group1.yml” is a separate file listing down all TCP and RTU devices falling under one device-group (e.g. wellhead PL0)

Each device-group file will have information about devices in that group. 

---
*file:*

  *version: "1.0.0"*

  *author: "Intel"*

  *date: "Sun Sep 1 13:34:13 PDT 2019"*

  *description: "Device group 1"*

*id: "PL0"*

*description: "Device group 1"*

*devicelist:*

*- deviceinfo: "flowmeter_device.yml"*

  *id: "flowmeter"*

  *protocol:*

    *protocol: "PROTOCOL_TCP"*

    *ipaddress: "192.168.0.222"*

    *port: 502*

    *unitid: 1*

  *tcp_master_info: "tcp_master_info.yml"*
  
*- deviceinfo: "iou_device.yml"*

  *id: "iou1"*

  *protocol:*

    *protocol: "PROTOCOL_RTU"*

    *slaveid: '10'*

  *rtu_master_network_info: "rtu_network1.yml"*

Following sections provide details about TCP and RTU device configuration in device-group file.


7.2.1 	Configuring TCP device in device-group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

E.g. 
*devicelist:*
*- deviceinfo: "flowmeter_device.yml"*

  *id: "flowmeter"*

  *protocol:*

    *protocol: "PROTOCOL_TCP"*

    *ipaddress: "192.168.0.222"*

    *port: 502*

    *unitid: 1*

  *tcp_master_info: "tcp_master_info.yml"*

Following parameters are needed for each TCP device:

-	ipaddress – for TCP communication IP address for client device required 
-	port – can be configured as per client device configuration
-	unitid – id can used to distinguish multiple clients on same ipaddress
-	tcp_master_info - tcp_master_info.yml – In this file interframe delay and response timeout can be configured for TCP network


Following is a sample file for tcp_master_info.yml 

*file:*

    *version: "1.0.0"*

    *author: "Intel"*

    *date: "Sun Sep 1 13:34:13 PDT 2019"*

    *description: "TCP master config parameter file"*

.. note::

   inter-frame delay and response timeout values are in Millisecond

interframe_delay: 1

response_timeout: 80

.. note::
   
   This reference shall be unique across TCP devices and needs to be given for each TCP device.


7.2.2 	Configuring RTU device in device-group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

E.g. 

*devicelist:*

*- deviceinfo: "iou_device.yml"*

  *id: "iou1"*

  *protocol:*

    *protocol: "PROTOCOL_RTU"*

    *slaveid: '10'*

  *rtu_master_network_info: "rtu_network1.yml"*

Following parameters are needed for each RTU device:

-	slaveid – This is end device id in case of RTU communication
-	rtu_master_network_info: "rtu_network1.yml" – This file is used to configure RTU configuration for a specific RTU network.
Following is a sample file for rtu_network1.yml

---
*file:*

    *version: "1.0.0"*

    *author: "Intel"*

    *date: "Sun Sep 1 13:34:13 PDT 2019"*

    *description: "RTU Network information for network 1"*

*baudrate: 9600*

*parity: "N"*

*com_port_name: "/dev/ttyS0"*

.. note::

   inter-frame delay and response timeout values are in Millisecond

interframe_delay: 1

response_timeout: 80

.. note::

   This file needs to be specified for each RTU device. If multiple RTU networks are present (RS485/RS232) then those many files should be created. For each RTU device, an appropriate RTU network reference shall be provided.

--------------------------------
7.3 	How to Configure Devices
--------------------------------

Device contains information of a device. Below is a sample file –

*file:*

  *version: "1.0.0"*

  *author: "Intel"*

  *date: "Sun Sep 1 13:34:13 PDT 2019"*

  *description: "Information for Demo IOUnit"*

*device_info:*

  *name: "IO Unit"*

  *description: "Power Scout Meter"*

  *manufacturer: "Dent Instruments"*

  *model: "PS3037"*

*pointlist: "iou_datapoints.yml"*

--------------------------------------
7.4 	How to Configure Device points
--------------------------------------

Device Point contains end point information. Below is a sample file.

Below parameters can be changed in this file –

-	addr - can be of range 0 to 65534
-	pollinterval – value in milliseconds 
-	type – Function Code 
-	width – Number of bytes to be read
-	realtime – To be used for real time, as of date it is false.
-	Datatype – Represents data type of the data point.
-	dataPersist-Represents if data to be persisted into DB.
-	Scalefactor – Represents scale factor to be used for the data point.

*file:*

  *version: "1.0.0"*

  *author: "Intel"*

  *date: "Sun Sep 1 13:34:13 PDT 2019"*

  *description: "Data for Demo IOUnit data points"*

*datapoints:*

*- id: "Arrival"*

  *attributes:*

    *type: "DISCRETE_INPUT"*

    *addr: 2048*

    *width: 1*

    *datatype: “boolean”*
    
    *dataPersist: true*

    *scalefactor: 1*

  *polling:*

    *pollinterval: 250*

    *realtime: true*

    

*- id: "AValve"*

  *attributes:*

    *type: "HOLDING_REGISTER"*

    *addr: 640*

    *width: 2*

    *datatype: “INT”*

    *dataPersist: true*

    *scalefactor: 1*

  *polling:*

    *pollinterval: 1000*

    *realtime: true*

    

*- id: "DValve"*

  *attributes:*

    *type: "COIL"*

    *addr: 2048*

    *width: 1*

    *datatype: “boolean”*

    *scalefactor: 1*

  *polling:*

    *pollinterval: 1000*

    *realtime: true*


*- id: "TubingPressure"*

  *attributes:*

    *type: "INPUT_REGISTER"*

    *addr: 1030*

    *width: 2*

    *datatype: “float”*

    *scalefactor: -1.0*

  *polling:*

    *pollinterval: 250*

    *realtime: true*


*- id: "CasingPressure"*

  *attributes:*

    *type: "INPUT_REGISTER"*

    *addr: 1024*

    *width: 4*

    *datatype: “double”*

    *scalefactor: 1.0*
    
  *polling:*

    *pollinterval: 250*

    *realtime: true*


*- id: "KeepAlive"*

  *attributes:*

    *type: "COIL"*

    *addr: 3073*

    *width: 1*

  *polling:*

    *pollinterval: 2000*

    *realtime: true*

.. note::
   
   For coil type width should be 1. 

**YML file Configuration table**

.. figure:: Doc_Images/table8_1_updated.png
    :scale: 80 %
    :align: center

.. figure:: Doc_Images/table8_2_updated.png
    :scale: 80 %
    :align: center

.. figure:: Doc_Images/table8_3_updated.png
    :scale: 80 %
    :align: center

.. figure:: Doc_Images/table8_4_updated.png
    :scale: 80 %
    :align: center

.. figure:: Doc_Images/table8_5_updated.png
    :scale: 80 %
    :align: center

.. figure:: Doc_Images/table8_6_updated.png
    :scale: 80 %
    :align: center

.. figure:: Doc_Images/table8_7_updated.png
    :scale: 80 %
    :align: center

-----------------------------------------------------------------------
7.5 	How to add/edit/delete new wellhead/device/point configurations
-----------------------------------------------------------------------

1.	User can add/update/edit/delete Oil well configurations files (YML files) from /opt/intel/eii/uwc_data directory
2.	Open a terminal and go to <working_dir>/IEdgeInsights directory.
3.	Run below command to apply new Oil well site configurations 

    Navigate to *<working_dir>/IEdgeInsights/uwc/build_scripts*

    *sudo ./05_applyConfigChanges.sh*

.. note::
   
   This script will restart all UWC docker containers.

-----------------------------
7.6 	KPI App Configuration
-----------------------------

Following is a sample configuration file for KPI App.


---
*file:*

  *version: "1.0.0"*

  *author: "Intel"*

  *date: "Sun Sep 1 13:34:13 PDT 2020"*

  *description: "KPI App Config File"*

*isMQTTModeApp: false*

*timeToRun_Minutes: 10*

*isRTModeForPolledPoints: true*

*isRTModeForWriteOp: true*

**# This section lists down number of control loops.**

**# For each control loop, following information is presented:**

**# 1. Point being polled**

**# 2. Point and value to be used for writing**

**# 3. Delay to be used before sending a write operation.**

*controlLoopDataPointMapping:*

*- polled_point: "/flowmeter/PL0/P1"*

*delay_msec: 5*

*write_operation:*

    *datapoint: "/iou/PL0/D1"*

    *dataval: "0x01"*

*- polled_point: "/flowmeter/PL0/P2"*

*delay_msec: 15*

*write_operation:*

    *datapoint: "/flowmeter/PL0/D2"*

    *dataval: "0x1234"*

Following is a description of each field.

.. figure:: Doc_Images/table9_1_update.png
    :scale: 70 %
    :align: center

.. figure:: Doc_Images/table9_2_update.png
    :scale: 70 %
    :align: center

Please note following: This configuration file should be created manually with following considerations:

    A)	The points in *“polled_point”* and *“datapoint”* fields in this file should be configured as per actual configuration in wellhead, device and datapoints config files.

    e.g., If a point to be polled is not present in datapoints config file, then data for that control loop will not be collected.

    B)	If the points being polled are configured as *“realtime”* in datapoints config file, then *“isRTModeForPolledPoints”* should be set to *“true”*. It should be set to *“false”* otherwise.

    C)	ZMQ-based KPI App can monitor either RT or Non-RT points at a time.

    D)	KPI App container can run either in ZMQ mode or in MQTT mode at a time.  


