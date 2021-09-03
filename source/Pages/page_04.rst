=======================
4.0  Installation Guide
=======================

---------------------------------------------------
4.1  How to install UWC software with EII installer
---------------------------------------------------

This section provides steps to install and Deploy UWC containers using the EII installer

**Pre-requisite**: Internet connection (with proper proxy settings) is required for Installation.

**Steps:**

1.	Install Ubuntu 20.04 server version on gateway and Apply RT Patch (refer section 14).
2.	Git clone in following order will place the directories in proper relative directory  structure

.. code-block:: sh

    •	git clone https://github.com/open-edge-insights/eii-core.git IEdgeInsights --branch=v2.6
    •	git clone https://github.com/open-edge-insights/eii-messagebus.git IEdgeInsights/common/libs/EIIMessageBus --branch=v2.6
    •	git clone https://github.com/open-edge-insights/eii-c-utils.git IEdgeInsights/common/util/c --branch=v2.6
    •  	git clone https://github.com/open-edge-insights/eii-zmq-broker.git IEdgeInsights/ZmqBroker --branch=v2.6
    •	git clone https://github.com/open-edge-insights/ts-telegraf.git IEdgeInsights/Telegraf --branch=v2.6
    •	git clone https://github.com/open-edge-insights/eii-influxdb-connector.git IEdgeInsights/InfluxDBConnector --branch=v2.6
    •	git clone https://github.com/open-edge-insights/eii-etcd-ui.git IEdgeInsights/EtcdUI --branch=v2.6
    •	git clone https://github.com/open-edge-insights/eii-tools.git IEdgeInsights/tools --branch=v2.6
    •	git clone https://github.com/open-edge-insights/uwc.git  IEdgeInsights/uwc  --branch=v1.6
    •   git clone https://github.com/open-edge-insights/uwc-docs.git IEdgeInsights/uwc-docs --branch=v1.6
    

3.	Navigate to $ <working_dir>/IEdgeInsights/build


4.	Execute Command 

.. code-block:: sh

    a.	$./pre_requisites.sh --proxy=<proxy address with port number> for proxy enabled network.

    b.	$ sudo ./pre_requisites.sh – for non-proxy network


.. note::
       
    If the error "Docker CE installation step is failed" is seen while running pre-requisite.sh script on a fresh system then kindly re-run the pre_requisite.sh script again. This is a known bug in docker community for Docker CE.

5.	Navigate to <working_dir>/IEdgeInsights/uwc/build_scripts.   
6.	Execute Command $ sudo ./01_uwc_pre_requisites.sh
7.	Execute Command $ sudo ./02_provision_UWC.sh

It prompts these options – 

•	Please choose one of the below options based on Dev or Prod mode.
    1) Dev
    2) Prod 
•	Please choose one of the below options based on the use case (combination of UWC services) needed.
    1) Basic UWC micro-services without KPI-tactic Application & Sparkplug-Bridge - (Modbus-master TCP & RTU, mqtt-bridge, internal mqtt broker, ETCD server, ETCD UI & other base EII & UWC services)
    2) Basic UWC micro-services as in option 1 along with KPI-tactic Application (Without Sparkplug-Bridge)
    3) Basic UWC micro-services & KPI-tactic Application along with Sparkplug-Bridge
    4) Basic UWC micro-services with Sparkplug-Bridge and no KPI-tactic Application
    5) Basic UWC micro-services with Time series micro-services (Telegraf & InfluxDBConnector)
    6) Running Basic UWC micro-services with time series services (Telegraf & InfluxDBCOnnector) along with KPI-tactic app
    7) Running Basic UWC micro-services with time series services (Telegraf & InfluxDBCOnnector) along with Sparkplug service
    8) Running the sample DB publisher with Telegraf, InfluxDBCOnnector, ZmqBroker & Etcd container

Following is a sample output for Sparkplug-Bridge related configuration:

**• Enter the following parameters required for Sparkplug-Bridge container..**

Is TLS required for sparkplug-bridge (yes/no): 
    yes
Enter the CA certificate full path including file name:
    /home/ubuntu/new/ca/root-ca.crt
Enter the client certificate full path including file name: 
    /home/ubuntu/new/client/client.crt
Enter the client key certificate full path including file name: 
    /home/ubuntu/new/client/client.key
Enter the external broker address/hostname:
    192.168.1.11
Enter the external broker port number: 
    22883
Enter the QOS for scada (between 0 to 2): 
    1

**•	Enter the following parameters required for sparkplug-bridge container**

Is TLS required for sparkplug-bridge (yes/no): 
    no
Enter the external broker address/hostname:
    192.168.1.11
Enter the external broker port number: 
    22883
Enter the QOS for scada (between 0 to 2): 
    1

8.	Execute Command $ sudo ./03_Build_Run_UWC.sh

Above is a process for interactive mode. A non-interactive mode is also supported. 
Following are the details: 

9. To support non-interactive mode, following options are added in 2nd script(02_provision_UWC).

.. figure:: Doc_Images/table2.png
    :scale: 80 %
    :align: center


If required parameters are missing, then those will be requested from user in an interactive mode.

10.	Following are sample commands for non-interactive mode execution.

.. code-block:: sh

        All UWC basic modules (no KPI, no Sparkplug-Bridge)
        sudo ./02_provision_UWC.sh --deployMode=dev --recipe=1

        All UWC modules (with KPI and with Sparkplug-Bridge).
        sudo ./02_provision_UWC.sh --deployMode=dev --recipe=3 --isTLS=yes --caFile="scada_ext_certs/ca/root-ca.crt" --crtFile="scada_ext_certs/client/client.crt" --keyFile="scada_ext_certs/client/client.key" --brokerAddr="192.168.1.11" --brokerPort=22883 --qos=1


Build scripts descriptions– 

    1.	01_uwc_pre_requisites.sh - This script creates docker volume directory /opt/intel/eii/uwc_data, creates “/opt/intel/eii/container_logs/” for storing log and git clone modconn into respective directory of modbus master container.  
    
    2.	02_provision_UWC.sh - It runs the builder to generate consolidated docker-compose.yml. This script performs provisioning as per docker-compose.yml file. Along with this, it generates certs for mqtt. 
        It allows user to choose combination of UWC services, allows to choose deployment mode either dev or prod mode.

    3.	03_Build_Run_UWC.sh - This script will build and deploys all UWC containers.

    4.	04_uninstall_UWC.sh – Used for cleanup and uninstalling docker, docker-compose and installed libraries. This script will bring down all containers and removes all running containers.

    5.	05_applyConfigChanges.sh - This script will stop and start all running containers with updated changes.

    6.	06_UnitTestRun.sh - This script will generate unit test report and code coverage report.

