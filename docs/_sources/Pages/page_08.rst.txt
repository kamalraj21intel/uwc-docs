=========================
8.0	UWC Modbus Operations
=========================
This section provides configurations required to read and write data from sensors and actuators connected over Modbus TCP or RTU on UWC gateway.
An application can perform following operations using UWC containers:

    •	Data Polling
    •	On-Demand Write
    •	On-Demand Read

Following section explains how to use MQTT topics to perform above operations. Further these operations can be performed in realtime (RT) and non-realtime (Non-RT) mode.
Multiple modules are involved in processing the operation. To capture the time taken by each module (i.e., a step), epoch timestamps in microseconds are added at various levels. These timestamps are present in JSON message. 

**The table of terms here is useful for interpreting the JSON payloads of the messages.**

.. figure:: Doc_Images/table10_1_updated.png
    :scale: 70 %
    :align: center

.. figure:: Doc_Images/table10_2_update.png
    :scale: 70 %
    :align: center

.. figure:: Doc_Images/table10_3_update.png
    :scale: 70 %
    :align: center

.. figure:: Doc_Images/table10_4_update.png
    :scale: 70 %
    :align: center

--------------------
8.1 	Data Polling
--------------------

In the datapoint YML configuration file, a polling frequency is configured. As per polling frequency, data is fetched from the end point and published on MQTT by the UWC container. This section describes how to read the data for polled points using MQTT.

The data actions which are “Polling” actions are initiated by the Protocol container (in this case the Modbus protocol application (i.e., the driver) within the Modbus container. 

To receive polled data: Application should use a topic in following format to receive (i.e., subscribe) polling data from MQTT:

**MQTT topic to receive (i.e., subscribe) write response:**
     **/device/wellhead/point/update**

Please refer to the table in section 6 for details of fields.


**Example:**

**Polling Topic**: /flowmeter/PL0/D3/update

**Polling Message**: Success Response 

{

  "datatype": "int16",

  "respPostedByStack": "1626778386833063",

  "dataPersist": true,

  "respRcvdByStack": "1626778386832788",

  "status": "Good",

  "tsMsgRcvdForProcessing": "1626778386838199",

  "wellhead": "PL0",

  "scaledValue": 4,

  "driver_seq": "1153204606361944465",

  "value": "0x0004",

  "reqRcvdInStack": "1626778386809284",

  "data_topic": "/flowmeter/PL0/D3/update",

  "metric": "D3",

  "usec": "1626778386833431",

  "reqSentByStack": "1626778386819158",

  "tsPollingTime": "1626778386809058",

  "version": "2.0",

  "realtime": "0",

  "timestamp": "2021-07-20 10:53:06",

  "tsMsgReadyForPublish": "1626778386838268"

}


**Polling Message**: Error Response

{

  "datatype": "int16",

  "respPostedByStack": "0",

  "lastGoodUsec": "1626847050409405",

  "realtime": "0",

  "dataPersist": true,

  "respRcvdByStack": "0",

  "status": "Bad",

  "tsMsgRcvdForProcessing": "1626847098399696",

  "wellhead": "PL0",

  "scaledValue": 0,

  "driver_seq": "1155737881221051931",

  "value": "0x00",

  "reqRcvdInStack": "0",

  "data_topic": "/flowmeter/PL0/D3/update",

  "metric": "D3",

  "usec": "1626847098395437",

  "reqSentByStack": "0",

  "tsPollingTime": "1626847098394978",

  "version": "2.0",

  "error_code": "2003",

  "timestamp": "2021-07-21 05:58:18",

  "tsMsgReadyForPublish": "1626847098399751"

}

-----------------------
8.2 	On-Demand Write
-----------------------

This section describes how to write data to some specific Modbus point using MQTT.

To send request: Application should use a topic in the following format to send (i.e., publish) write request on MQTT:

**MQTT topic to send (i.e., publish) write request: /device/wellhead/point/write**



To receive response: Application should use a topic in the following format to receive (i.e., subscribe) response of write request from MQTT:

**MQTT topic to receive (i.e., subscribe) write response: /device/wellhead/point/writeResponse**

Please refer to the table in section 6 for details of fields.

**Example:**

**Request Topic**: /flowmeter/PL0/Flow/write 

**Request Message:**

{"wellhead":"PL0","command":"Flow","value":"0x00","timestamp":"2019-09-20 12:34:56","usec":"1571887474111145","version":"2.0","app_seq":"1234"}

A message without *“realtime”* field is treated as a non-realtime message. To execute a message in realtime way, a field called *“realtime”* should be added as shown below:

{"wellhead":"PL0","command":"Flow","value":"0x00","timestamp":"2019-09-20 12:34:56","usec":"1571887474111145","version":"2.0","app_seq":"1234",”realtime”:”1”}

A message with *“value”* is treated as On-Demand Write from vendor App.

{"wellhead" : "PL0","command" : "INT16_MF10","timestamp" : "2019-09-20 12:34:56",
"usec" : "1571887474111145","version" : "2.0","realtime" : "0","app_seq" : "1234",
"scaledValue" : 12}

A message with *“scaledValue”* is treated as On-Demand Write from Ignition system.

The *“value”* / *"scaledValue"* field represents value to be written to the end device as a part of on-demand write operation.  

**Response Topic:** /flowmeter/PL0/Flow/writeResponse 

**Response Message:** Success Response 

{

  "app_seq": "1234",

  "respPostedByStack": "1626846891692261",

  "dataPersist": true,

  "respRcvdByStack": "1626846891692219",

  "status": "Good",

  "tsMsgRcvdForProcessing": "1626846891693976",

  "wellhead": "PL0",

  "tsMsgRcvdFromMQTT": "1626846891669463",

  "tsMsgPublishOnEII": "1626846891669925",

  "reqRcvdInStack": "1626846891672050",

  "data_topic": "/flowmeter/PL0/Flow/writeResponse",

  "reqRcvdByApp": "1626846891671963",

  "metric": "Flow",

  "usec": "1626846891692549",

  "reqSentByStack": "1626846891673238",

  "timestamp": "2021-07-21 05:54:51",

  "version": "2.0",

  "realtime": "0",

  "tsMsgReadyForPublish": "1626846891694023"
}

**Response Message**: Error Response 

{

  "app_seq": "1234",

  "respPostedByStack": "0",

  "dataPersist": true,

  "respRcvdByStack": "0",

  "status": "Bad",

  "tsMsgRcvdForProcessing": "1626778808002974",

  "wellhead": "PL0",

  "tsMsgRcvdFromMQTT": "1626778808000285",

  "tsMsgPublishOnEII": "1626778808000437",

  "reqRcvdInStack": "0",

  "data_topic": "/flowmeter/PL0/Flow/writeResponse",

  "reqRcvdByApp": "1626778808001309",

  "metric": "Flow",

  "usec": "1626778808001814",

  "reqSentByStack": "0",

  "error_code": "2003",

  "version": "2.0",

  "realtime": "0",

  "timestamp": "2021-07-20 11:00:08",

  "tsMsgReadyForPublish": "1626778808003021"

}

**Response Message:** Error Response for Invalid request JSON

{

  "app_seq": "1234",

  "respPostedByStack": "0",

  "dataPersist": true,

  "respRcvdByStack": "0",

  "status": "Bad",

  "tsMsgRcvdForProcessing": "1626778808002974",

  "wellhead": "PL0",

  "tsMsgRcvdFromMQTT": "1626778808000285",

  "tsMsgPublishOnEII": "1626778808000437",

  "reqRcvdInStack": "0",

  "data_topic": "/flowmeter/PL0/Flow/writeResponse",

  "reqRcvdByApp": "1626778808001309",

  "metric": "Flow",

  "usec": "1626778808001814",

  "reqSentByStack": "0",

  "error_code": "100",

  "version": "2.0",

  "realtime": "0",

  "timestamp": "2021-07-20 11:00:08",

  "tsMsgReadyForPublish": "1626778808003021"

}

----------------------
8.3 	On-Demand Read
----------------------

This section describes how to read data from some specific Modbus points using MQTT.

To send request: Application should use a topic in the following format to send (i.e., publish) read request on MQTT:

**MQTT topic to send (i.e. publish) read request: /device/wellhead/point/read**

To receive response: Application should use a topic in the following format to receive (i.e., subscribe) response of read request from MQTT:

**MQTT topic to receive (i.e., subscribe) write response:**
     **/device/wellhead/point/readResponse**
     
Please refer to the table in section 6 for details of fields.

**Example:**

**Request Topic: /flowmeter/PL0/Flow/read**

**Request Message:** 

    {"wellhead":"PL0","command":"Flow","timestamp":"2019-09-20 12:34:56","usec":"1571887474111145","version":"2.0","app_seq":"1234"}

A message without “realtime” field is treated as a non-realtime message. To execute a message in realtime way, a field called “realtime” should eb added as shown below:

    {"wellhead":"PL0","command":"Flow","timestamp":"2019-09-20 12:34:56","usec":"1571887474111145","version":"2.0","app_seq":"1234",”realtime”:”1”}

**Response Topic:** /flowmeter/PL0/Flow/readResponse 


**Response Message:** Success Response

{

  "app_seq": "1234",

  "respPostedByStack": "1626778599282378",

  "dataPersist": true,

  "respRcvdByStack": "1626778599282315",

  "status": "Good",

  "tsMsgRcvdForProcessing": "1626778599284171",

  "wellhead": "PL0",

  "scaledValue": 4,

  "value": "0x0004",

  "tsMsgRcvdFromMQTT": "1626778599275557",

  "tsMsgPublishOnEII": "1626778599277242",

  "reqRcvdInStack": "1626778599279507",

  "data_topic": "/flowmeter/PL0/Flow/readResponse",

  "reqRcvdByApp": "1626778599279422",

  "metric": "Flow",

  "usec": "1626778599282619",

  "reqSentByStack": "1626778599280674",

  "timestamp": "2021-07-20 10:56:39",

  "version": "2.0",

  "realtime": "0",

  "tsMsgReadyForPublish": "1626778599284240"

}


**Response Message:** Error Response

{

  "app_seq": "1234",

  "respPostedByStack": "0",

  "dataPersist": false,

  "respRcvdByStack": "0",

  "status": "Bad",

  "tsMsgRcvdForProcessing": "1626846987971314",

  "wellhead": "PL0",

  "tsMsgRcvdFromMQTT": "1626846987968830",

  "tsMsgPublishOnEII": "1626846987968983",

  "reqRcvdInStack": "0",

  "data_topic": "/flowmeter/PL0/Flow/readResponse",

  "reqRcvdByApp": "1626846987969861",

  "metric": "Flow",

  "usec": "1626846987970320",

  "reqSentByStack": "0",

  "error_code": "2003",

  "version": "2.0",

  "realtime": "0",

  "timestamp": "2021-07-21 05:56:27",

  "tsMsgReadyForPublish": "1626846987971358"

}

**Response Message**: Error Response for Invalid Input JSON 

{

  "app_seq": "1234",

  "respPostedByStack": "0",

  "dataPersist": false,

  "respRcvdByStack": "0",

  "status": "Bad",

  "tsMsgRcvdForProcessing": "1626846987971314",

  "wellhead": "PL0",

  "tsMsgRcvdFromMQTT": "1626846987968830",

  "tsMsgPublishOnEII": "1626846987968983",

  "reqRcvdInStack": "0",

  "data_topic": "/flowmeter/PL0/Flow/readResponse",

  "reqRcvdByApp": "1626846987969861",

  "metric": "Flow",

  "usec": "1626846987970320",

  "reqSentByStack": "0",

  "error_code": "100",

  "version": "2.0",

  "realtime": "0",

  "timestamp": "2021-07-21 05:56:27",

  "tsMsgReadyForPublish": "1626846987971358"

}

-----------------------
8.4 	KPI Application
-----------------------

Following data (if available) is logged in a log-file by KPI Application for control loops.

.. figure:: Doc_Images/table11_1_update.png
    :scale: 80 %
    :align: center

.. figure:: Doc_Images/table11_2_update.png
    :scale: 80 %
    :align: center



