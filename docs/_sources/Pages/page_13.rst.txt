==========================================
13.0 	UWC Gateway to Cloud Communication
==========================================

Below steps describe UWC Gateway and cloud communication architecture and steps to connect UWC Gateway to AWS Sitewise.

13.1 	Architecture
--------------------

Modbus devices connects to the on-prem UWC gateway. The gateway is having Sparkplug MQTT client which securely publishes Sparkplug format data to AWS IoT Core. AWS IoT Core service provisions cloud connectivity to IoT edge devices. AWS IoT Core possesses an MQTT broker as one of its components. With this connectivity, UWC gateway published data is available in the AWS cloud. Please find more information about AWS IoT Core at https://aws.amazon.com/iot-core.

.. figure:: Doc_Images/image_update_12.png
    :scale: 90 %
    :align: center

Sparkplug Sitewise Bridge (SSB) is a service which rapidly connects operational technology (OT) data from Industrial Operations (on-prem data) to AWS IoT Sitewise with minimal configuration and zero coding. Please find more information on SSB at https://aws.amazon.com/marketplace/pp/Cirrus-Link-Sparkplug-SiteWise-Bridge/B08L8KNCNN.

SSB software runs in an EC2 instance which is running in AWS cloud. SSB software comprises MQTT client which subscribes to the AWS IoT Core broker to receive UWC gateway data. When SSB receives UWC gateway data it creates and update resources (Assests, Models) in AWS Sitewise. In AWS Sitewise, user can monitor UWC gateway data. Please find more information about Sitewise at https://aws.amazon.com/iot-sitewise/. 

13.2 	Installation and Configuration
--------------------------------------

13.2.1 	SSB installation and cloud infrastructure provisioning
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We need to provision the AWS infrastructure and install SSB in the EC2 instance. Please use below link to carry out SSB installation and cloud infrastructure provisioning procedure - 

    https://docs.chariot.io/display/CLD80/IBAS%3A+Installation

Please note that there are two different delivery methods for SSB installation. Please select ‘CloudFormation Template’ as delivery method.

Once the process is completed, the result will be 'CREATE_COMPLETE.'

.. figure:: Doc_Images/image12_1_update.png
    :scale: 90 %
    :align: center

13.2.2 	AWS IoT core broker and SSB configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
A ‘thing' needs to be created in AWS IoT core which represent the IoT edge device i.e., UWC gateway. SSB needs to be configured so that it can access IoT core to fetch the UWC gateway data. Please use the link to carry out the complete AWS IoT Core broker and SSB configuration procedure - 

https://docs.chariot.io/display/CLD80/SSB%3A+Quickstart.

Alternate link to get an insight on the creation of a 'thing' in AWS IoT core - https://docs.aws.amazon.com/iot/latest/developerguide/iot-moisture-create-thing.html


13.2.3 	UWC gateway configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: sh

    SSL certificates which were created in STEP 2 during the creation of a ‘thing’ in AWS IoT core must be inputted while running the ‘01_pre-requisites.sh’ script.

    *$sudo ./02_provision_UWC.sh --deployMode=dev --recipe=3 --isTLS=yes --caFile="/<path>/root-ca.crt" --crtFile="/<path> /client.crt" --keyFile="/<path> client.key" --brokerAddr="azeyj7bji4ghe-ats.iot.us-west-2.amazonaws.com" --brokerPort=8883 --qos=1*

Deploy Mode ‘dev’ or ‘Prod’.

Select Recipe as 3 to have Sparkplug Container deployed.

Make sure the ‘isTLS’ argument is set to 'yes'

Configure the ‘caFile’ argument with the path of the CA certificate obtained from AWS IoT core.

Configure the ‘crtFile’ argument with the path of the client certificate obtained from AWS IoT core.

Configure the ‘keyFile' argument with the path of the client private key obtained from AWS IoT core

‘brokerPort’ should be set to ‘8883.’

‘brokerAddr' should be set to the custom endpoint of the AWS IoT core. Use the following couple of steps to fetch the custom endpoint.

Go to the IoT core console. Select the ‘Settings’ tab in the left pane.

.. figure:: Doc_Images/image12_2_update.png
    :scale: 70 %
    :align: center

Custom endpoint which represents the IoT core broker address. This address needs to be configured in the ‘brokerAddr' argument as shown in below image.

.. figure:: Doc_Images/image12_3_update.png
    :scale: 70 %
    :align: center

13.3 	Monitor Data on Cloud
-----------------------------

The data can be monitored on the AWS Sitewise service. 

Scroll to the AWS Sitewise service in the AWS management console as shown in below image

.. figure:: Doc_Images/image12_4_update.png
    :scale: 60 %
    :align: center

Go to the ‘Models’ tab. The attribute ‘Protocol’ of a model can be seen.

.. figure:: Doc_Images/image12_5_update.png
    :scale: 60 %
    :align: center

The ‘measurement' parameter representing a data point can be seen in the model.

.. figure:: Doc_Images/image12_6_update.png
    :scale: 60 %
    :align: center

Navigate to the ‘Assets’ tab. The attribute ‘Protocol’ can be seen with its defined value.

.. figure:: Doc_Images/image12_7_update.png
    :scale: 60 %
    :align: center

The ‘measurement' parameter representing a data point can be seen in the asset with its defined value.

.. figure:: Doc_Images/image12_8_update.png
    :scale: 60 %
    :align: center

.. note::

   One should delete old Assets & Models from AWS IoT to ensure the updated Assets and Models get reflected. Duplicate Assets and Models will not be refreshed.
