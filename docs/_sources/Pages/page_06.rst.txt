====================================
6.0  Container Configuration setting
====================================

This section provides details about configuring UWC containers. 

---------------
6.1  Containers
---------------

Following containers are developed under UWC:
    •	Modbus TCP Master
    •	Modbus RTU Master
    •	MQTT-Bridge
    •	MQTT
    •	Sparkplug-Bridge
    •	KPI Application

For configuring these containers, docker-compose.yml file is used. The docker-compose.yml is auto generated based on inputs provided while executing script 02_provision_UWC.sh..

For more details, please refer README provided in EII documentation.

Path for README – https://github.com/open-edge-insights/eii-core/blob/master/README.md

UWC containers (i.e., Modbus Clients and MQTT-Bridge) use ZeroMQ to communicate with each other.

At present, recommendation is to have only one Modbus-TCP and one Modbus-RTU container. Hence, changes to configuration present in docker-compose.yml file shall not be needed.


6.1.1  Common Configuration for Modbus Client Containers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Following are configurations, applicable for Modbus Client (TCP and RTU both) containers:

.. figure:: Doc_Images/table3.png
    :scale: 80 %
    :align: center

Example for Modbus-TCP-Master container from docker-compose.yml file:
    AppName: "TCP"

    ETCD_HOST: ${ETCD_HOST}

    ETCD_CLIENT_PORT: ${ETCD_CLIENT_PORT}

    ETCD_PREFIX: ${ETCD_PREFIX}
    
    DEV_MODE: ${DEV_MODE}

    no_proxy: ${eii_no_proxy}

    Log4cppPropsFile: "/opt/intel/config/log4cpp.properties"

    MY_APP_ID: 1

    CUTOFF_INTERVAL_PERCENTAGE: 90

    CertType: "zmq"

    PROFILING_MODE: ${PROFILING_MODE}

    NETWORK_TYPE: TCP

    DEVICES_GROUP_LIST_FILE_NAME: "Devices_group_list.yml"


6.1.2  Configuration foodbus network
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A separate network configuration YML file is maintained for each network. E.g., If there are 2 RTU and 1 TCP networks, then there will be 3 network configuration files. This file contains following configuration for both TCP and RTU:

.. note:: inter-frame delay and response timeout values are in Millisecond.

interframe_delay: 1

response_timeout: 80

For Modbus RTU master, following additional configurations are needed apart from above mentioned parameters:
    baudrate: 9600

    parity: "N"

    com_port_name: "/dev/ttyS0"

.. figure:: Doc_Images/table4.png
    :scale: 80 %
    :align: center

--------------------------------
6.2		Modbus TCP Communication
--------------------------------

When used in TCP mode, the publisher of a stream will bind to a TCP socket and the subscribers connect to it to receive data. In Request-Response pattern, the responder acts as the server and binds the socket and the requester connects to it.

--------------------------------
6.3	    Modbus RTU Communication
--------------------------------

Modbus RTU is an open serial protocol derived from the Master/Slave architecture. UWC Modbus-rtu-container is used as master and slave can be configured.

.. figure:: Doc_Images/flow1.png
    :scale: 60 %
    :align: center

    parameters (i.e., baud rate, parity, stop bits) from docker-compose.yml file.

-------------------
6.4 	MQTT Bridge
-------------------

This container is used to send messages from ZeroMQ to MQTT and vice-versa. 

Modbus containers communicate over the internal Edge Insights for Industrial data bus (ZMQ). The MQTT-Bridge module enables communication with Modbus containers using MQTT. The MQTT- Bridge module reads data on ZMQ received from Modbus containers and publishes that data on MQTT. Similarly, the MQTT- Bridge module reads data from MQTT and publishes it on ZMQ.

------------
6.5 	MQTT
------------

The MQTT container is a mosquitto broker required for MQTT to publish/subscribe data. MQTT broker use port “11883”. 

MQTT clients should use above mentioned port for communication.


6.5.1	Accessing secured MQTT container from an external MQTT client
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Pre-requisites:**
    All UWC containers must be deployed-on gateway with DEV_MODE=false (i.e., secured mode).

**Steps to follow:**

    1.	Open a terminal and execute following command to create local directory to keep certificates of MQTT broker,
        *mkdir ~/mqtt_certs && cd ~/mqtt_certs*

        Copy ca/ and /mymqttcerts directories in local directory i.e., created in  script *02_provision_UWC.sh* from 
        *working_dir/IEdgeInsights/build/provision/Certificates/ directory*.

        Command to copy ca/ and /mymqttcerts/ dir in local dir (i.e., mqtt_certs)

        *sudo cp -r /<working_dir>/IEdgeInsights/build/provision/Certificates/ca ~/mqtt_certs/*

    2.	Assign read permission to local certs using following command,
        sudo chown -R $USER:$USER  && sudo chmod +r ca/* mymqttcerts/*

        **Please Note:** Read permissions are only required for ca/ and /mymqttcerts directories present inside mqtt_certs directory copied in step2.

        Provide right access to certificates directory using below command – sudo chmod +x Certificates in 

        *<working_dir>/IEdgeInsights/build/provision/Certificates*

    3.	Open MQTT client e.g., MQTT.fx

    4.	Open the connection setting and click on SSL/TLS tab.

        >> then click on Self Signed certificate option >> select CA file from *mqtt_certs/ca*  directory (file name : ca_certificate.pem) , Client Certificate file from *mqtt_certs/mymqttcerts* directory (File name : mymqttcerts_client_certificate.pem) and Client key File from  *mqtt_certs/mymqttcerts*  directory (File name : mymqttcerts_client_key.pem) copied in step 2


    5.	Click on PEM Formatted check box and then save the setting and then connect. Refer below screenshot for more details 

.. figure:: Doc_Images/image4.png
    :scale: 70 %
    :align: center

    Figure. 4.5.1: Screen capture for mqtt.fx client connection


6.5.2	Accessing secured MQTT container from a client inside a container
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ 
1.	Mention following secrets for a new container in docker-compose.yml file.
    •	ca_broker – CA certificate
    •	client_cert – Client certificate 
    •	client_key – Client Key

Following sample snippet for docker-compose.yml file

.. figure:: Doc_Images/image5.png
    :scale: 90 %
    :align: center

2.	Use certificates mentioned in step 1 inside application to connect with secured MQTT broker which is running as a part of UWC.

Following is the sample code snippet in C++ to use certificates in a program,

.. figure:: Doc_Images/image6.png
    :scale: 60 %
    :align: center

3.	Deploy containers using usual deployment process.

------------------------
6.6 	Sparkplug-Bridge
------------------------

This container implements Eclipse Foundation’s SparkPlug standard to expose data to compliant SCADA Master over MQTT. 


6.6.1 	Pre-requisite for running Sparkplug-Bridge 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

a)	SCADA Master (e.g., Ignition System) shall be installed and configured.
b)	MQTT broker shall be installed and configured in SCADA Master. At present secured connectivity for MQTT is not supported. 
c)	Following parameters should be configured for Sparkplug-Bridge in docker-compose.yml file:

.. figure:: Doc_Images/table5.png
    :scale: 70 %
    :align: center

---------------
6.7 	KPI App
---------------

This is a sample application which implements control loops and logs data in a log file named “AnalysisKPIApp.log”. Normally 3 log files are created on rolling basis i.e., once tie set file size limit is exceeded, a new file is created and likewise max 3 files are created. After this, the log files are overwritten. 

The log file size can be updated, if required.

File: log4cpp.properties

Path in release package: kpi-tactic/KPIApp/Config

Path after deployment inside container: /opt/intel/config/log4cpp.properties 

Log files created are - AnalysisKPIApp.log, AnalysisKpiApp.log1, and AnalysisKpiApp.log2. These files are created in .txt format. Latest data will be available in AnalysisKPIApp.log followed by AnalysisKpiApp.log1, and AnalysisKpiApp.log2
   
Default log file size is around 34mb.

.. figure:: Doc_Images/image7.png
    :scale: 100 %
    :align: center

To change the file size, “log4cpp.properties” needs to be changed. Please change the limit highlighted above. The max file size mentioned here is in bytes. Please identify number of bytes as per file size needed and set the value here. 

Please run script 03 Build_Run_UWC.sh   after changing “log4cpp.properties”.
