===================================
9.0 	Sparkplug-Bridge Operations
===================================

Sparkplug-Bridge implements Eclipse Foundation’s SparkPlug standard. 

**Refer:** https://www.eclipse.org/tahu/spec/Sparkplug%20Topic%20Namespace%20and%20State%20ManagementV2.2-with%20appendix%20B%20format%20-%20Eclipse.pdf

This section explains the features in detail. UWC gateway acts as a “node” as per SparkPlug standard. Please note that Sparkplug-Bridge is an under-development feature and hence not all message types are supported from SparkPlug.

This section also explains how information from real device and virtual device is mapped to SparkPlug-formatted data.

------------------------------------------
9.1 	App (virtual device) communication
------------------------------------------

Apps running on UWC platform can be represented as a SparkPlug device to SCADA Master. SCADA Master can monitor, control these apps using SparkPlug mechanism. Sparkplug-Bridge defines following to enable this communication between apps and SCADA Master:


TemplateDef message: This allows providing a definition for a Sparkplug Template i.e., UDT

BIRTH message: This corresponds to a SparkPlug DBIRTH message.

DEATH message: This corresponds to a SparkPlug DDEATH message.

DATA message: This corresponds to a SparkPlug DDATA message.

CMD message: This corresponds to a SparkPlug DCMD message.

Apps and Sparkplug-Bridge communicate over internal MQTT using above defined messages.


9.1.1 	App Message Topic Format
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

MQTT Topic: MESSAGETYPE/APPID/SUBCLASS

Where,

•	MESSAGETYPE: Any of “BIRTH”, “DEATH”, “DATA”, “CMD”
•	APPID: Any string e.g., “UWCP”
•	SUBCLASS: Any string like wellhead-id e.g., “PL0”. This is not needed in case of DEATH message.

Sparkplug-Bridge uses following format to represent name of virtual device in SparkPlug Topic namespace:

[value of “APPID” from app message topic] + “-“ + [value of “SUBCLASS” from app message topic]

9.1.2 	App Message - BIRTH
~~~~~~~~~~~~~~~~~~~~~~~~~~~

MQTT Topic: BIRTH/APPID/SUBCLASS

Message format:

It is a JSON format message which contains a list of metrics having following fields:

.. figure:: Doc_Images/table12.png
    :scale: 70 %
    :align: center


**Example:** 

{
  "metrics": 

  [

    {

      "name": "Properties/Version",

      "dataType": "String",

      "value": "2.0.0.1",

      "timestamp": 1486144502122

    },

    {

      "name": "Properties/RTU_Time",

      "dataType": "String",
      
      "value": "1234",

      "timestamp": 1486144502122

    },

    {

      "name": "UDT/Prop1",

      "dataType": "UDT",

      "value": 

      {

        "udt_ref":

        {

          "name": "custom_udt",

          "version": "1.0"

        },

        "metrics": 

        [

          {

            "name": "M1",

            "dataType": "String",

            "value": "2.0.0.1",

            "timestamp": 1486144502122

          },

          {

            "name": "RTU_Time",

            "dataType": "Int32",

            "value": 1234,

            "timestamp": 1486144502122

          }

        ],

        "parameters":

        [

          {

            "name": "P1",

            "dataType": "String",

            "value": "P1Val"

          },

          {

            "name": "P2",

            "dataType": "Int32",

            "value": 100

          }

        ]

      }

    }

  ]

}

**Data Flow**:

This message is published by App over MQTT broker and subscribed by Sparkplug-Bridge. This message provides information about all metrics related to a SUBCLASS which App wants to expose to a SCADA Master. 

Sparkplug-Bridge publishes a DBIRTH message to SCADA Master if metrics contain a new metric or if datatype of any of metrics is changed.

.. note::

   •	If the App publishes multiple BIRTH messages for a SUBCLASS, then Sparkplug-Bridge remembers all metrics reported in all BIRTH messages. Sparkplug-Bridge reports all these metrics to SCADA Master in DBIRTH message. This data with Sparkplug-Bridge is cleared on restart of gateway or Sparkplug-Bridge container.

   •	A DBIRTH message can result in refreshing of data in Sparkplug-Bridge and in SCADA Master. Hence, it is recommended for an App to provide information about all metrics in one BIRTH message. App should avoid using multiple BIRTH messages for same SUBCLASS.

   •	If App wants to publish a metric of type “UDT”, the definition of “UDT” should be provided prior to publishing the BIRTH message. UDT definition can be provided using “TemplateDef” message, explained in subsequent section.


Following information is required as a part of “value” key when UDT type is used:

.. figure:: Doc_Images/table13.png
    :scale: 70 %
    :align: center


9.1.3 	App Message - DATA
~~~~~~~~~~~~~~~~~~~~~~~~~~

MQTT Topic: DATA/APPID/SUBCLASS

Message format:

It is a JSON format message which contains a list of metrics having following fields:

.. figure:: Doc_Images/table14.png
    :scale: 70 %
    :align: center

**Example:**

{

  "metrics": 

  [

    {

      "name": "Properties/Version",

      "dataType": "String",

      "value": "5.0.0.1",

      "timestamp": 1486144502122

    },

    {

      "name": "UDT/Prop1",

      "dataType": "UDT",

      "value": 
      
      {

        "metrics": 
        
        [

          {

            "name": "M1",

            "dataType": "String",

            "value": "a.b",

            "timestamp": 1486144502122

          }

        ]

      }

    }

  ]

}


Data Flow:

This message is published by App over MQTT broker and subscribed by Sparkplug-Bridge. This message provides information about all changed metrics related to a SUBCLASS. 

Sparkplug-Bridge publishes a DDATA message to SCADA Master if value of any of “known metrics” is changed compared to last known value from a BIRTH or DATA message.

.. note::
   
   A “known metric” is one which was reported in BIRTH message. The name and datatype for a “known metric” in DATA message and BIRTH message shall match.


9.1.4	App Message - CMD
~~~~~~~~~~~~~~~~~~~~~~~~~

MQTT Topic: CMD/APPID/SUBCLASS

Message format:

It is a JSON format message which contains a list of metrics having following fields:

.. figure:: Doc_Images/table15.png  
    :scale: 70 %
    :align: center

**Example:**

{

  "metrics":

  [

    {

      "name": "Properties/Version",

      "dataType": "String",

      "value": "7.0.0.1",

      "timestamp": 1486144502122

    },

    {

      "name": "UDT/Prop1",

      "dataType": "UDT",
      
      "metrics":

      [

        {

          "dataType": "Int32",

          "value": 4,

          "name": "RTU_Time",
          
          "timestamp": 1614512107195

        }

      ],

      "timestamp": 1614512107195

    }

  ]

}

Data Flow:

This message is published by Sparkplug-Bridge over MQTT broker and subscribed by App. This message provides information about control command i.e., DCMD received from SCADA Master. 

Sparkplug-Bridge publishes a CMD message to the App if DCMD message is received for a known metric.

.. note::
   
   A “known metric” is one which was reported in BIRTH message. The name and datatype for a “known metric” in DCMD message and BIRTH message shall match.


9.1.5 	App Message - DEATH
~~~~~~~~~~~~~~~~~~~~~~~~~~~

MQTT Topic: DEATH/APPID

Message format:

It is a JSON format message which contains the following fields:

.. figure:: Doc_Images/table16.png
    :scale: 70 %
    :align: center

**Example:** 

{

  "timestamp": 1486144502122

}

Data Flow:

When App’s connection with MQTT broker breaks then this message is published. 

Sparkplug-Bridge publishes a DDEATH message to SCADA Master for all known SUBCLASS associated with the App. 


9.1.6 	App Message - TemplateDef
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

MQTT Topic: TemplateDef

Message format:

It is a JSON format message which contains a list of metrics having following fields:

.. figure:: Doc_Images/table17_1.png
    :scale: 70 %
    :align: center

.. figure:: Doc_Images/table17_2.png
    :scale: 70%
    :align: center

**Example:**

{

  "udt_name": "custom_udt",

  "version": "1.0",
  
  "metrics": 
  [

    {

      "name": "M1",

      "dataType": "String",

      "value": ""

    },
    
    {

      "name": "RTU_Time",

      "dataType": "Int32",

      "value": 0

    }

  ],

  "parameters": 
  [

    {

      "name": "P1",

      "dataType": "String",

      "value": ""

    },

    {

      "name": "P2",

      "dataType": "Int32",

      "value": 0

    }

  ]

}

Data Flow:

App should use this message to provide definition of a Sparkplug Template i.e., UDT. UDT definitions are published as a part of NBIRTH message. Hence, after receiving a UDT definition, Sparkplug-Bridge publishes NDEATH and then NBIRTH to SCADA-Master. 


9.1.7 	START_BIRTH_PROCESS
~~~~~~~~~~~~~~~~~~~~~~~~~~~

MQTT Topic: START_BIRTH_PROCESS

Message format:

It is an empty JSON format message:

.. list-table:: 
   :widths: 25 25 25
   
   * - **Field Name**
     - **Datatype**
     - **Description**

**Example:** 

{
  
}

Data Flow:

This message is published by Sparkplug-Bridge over MQTT broker and subscribed by App. This message tells the App to publish following:

•	Definition of Sparkplug Templates i.e., UDT which are used by App in BIRTH message
•	BIRTH messages for all SUBCLASS the App is having. The App shall publish BIRTH messages on receiving START_BIRTH_PROCESS message.

START_BIRTH_PROCESS message will be sent on restart of Sparkplug-Bridge container or whenever Sparkplug-Bridge container needs to refresh the data that it maintains for virtual devices.

------------------------------------------
9.2 	Modbus (real) device communication
------------------------------------------

Modbus devices present in network are reported to SCADA Master using SparkPlug mechanism.

Apps and Sparkplug-Bridge communicate over internal MQTT using above defined messages.

9.2.1 	Support for DBIRTH
~~~~~~~~~~~~~~~~~~~~~~~~~~

Data from device YML configuration files is used to form a DBIRTH message for real devices at the start of Sparkplug-Bridge container. One datapoint YML file corresponds to one SparkPlug template definition. One real Modbus device contains one metric of type SparkPlug template. The SparkPlug template in turn contains all other metrics which correspond to datapoints mentioned in datapoints-YML file.

9.2.2	Support for DDATA
~~~~~~~~~~~~~~~~~~~~~~~~~

Data from polling operation published by MQTT-Bridge over internal MQTT is used to determine a change in value of any of metrics associated with a real device. If a change is detected, a DDATA message is published by Sparkplug-Bridge.

9.2.3 	Support for DCMD
~~~~~~~~~~~~~~~~~~~~~~~~

When a DCMD message is received from a SCADA Master for a real device for a “known metric”, then an on-demand write operation is initiated by SCADA and sent to MQTT-Bridge over internal MQTT.

.. note::
   
   •	A “known metric” is one which is present in device YML configuration file. The name and datatype for a “known metric” in DCMD message and YML file shall match.
   •	A DCMD message can result in multiple on-demand write operations.

9.2.4 	Support for DDEATH
~~~~~~~~~~~~~~~~~~~~~~~~~~

Data from polling operation published by MQTT-Bridge over internal MQTT is used to determine whether a device is reachable or not, based on error_code. If device unreachable error-code is found, a DDEATH message is published by Sparkplug-Bridge. When correct values are found, a DBIRTH message is published.

--------------------------
9.3 	SparkPlug Messages
--------------------------

Refer SparkPlug standard for more information.

9.3.1 	NBIRTH Message
~~~~~~~~~~~~~~~~~~~~~~

NBIRTH is Node-Birth.

On start-up, Sparkplug-Bridge module publishes this message over MQTT broker. The message is published in SparkPlug encoded format.

For Modbus real device, one datapoint YML file corresponds to one SparkPlug template. These template definitions are sent in NBIRTH message. DBIRTH message for Modbus device specifies a particular SparkPlug template.

Following are sample contents in simplified JSON format:

**Topic:** *spBv1.0/UWC nodes/NBIRTH/RBOX510-00*

**Message:** 

{
  "timestamp": 1608243262157,

  "metrics":

   [

    {
      "name": "Name",

      "timestamp": 1608243262157,

      "dataType": "String",

      "value": "SPARKPLUG-BRIDGE"

    },

    {

      "name": "bdSeq",

      "timestamp": 1608243262157,

      "dataType": "UInt64",

      "value": 0

    },

    {

      "name": "Node Control/Rebirth",

      "timestamp": 1608243262157,

      "dataType": "Boolean",

      "value": false

    },

    {

      "name": "iou_datapoints",

      "timestamp": 1608243262157,

      "dataType": "Template",
      
      "value": 

      {
        "version": "1.0.0",

        "reference": "",
        
        "isDefinition": true,

        "metrics":

        [

          {

            "name": "D1",

            "timestamp": 1608243262157,

            "dataType": "String",

            "properties": 

            {

              "Pollinterval":

              {

                "type": "UInt32",

                "value": 0

              },

              "Realtime":

              {

                "type": "Boolean",

                "value": false

              }

            },

            "value": ""

          },

          {

            "name": "D2"
            ,
            "timestamp": 1608243262157,

            "dataType": "String",

            "properties": 

            {

              "Pollinterval":

              {

                "type": "UInt32",

                "value": 0

              },

              "Realtime":
               
              {

                "type": "Boolean",

                "value": false

              }

            },

            "value": ""

          }

        ],

        "parameters": 

        [

          {

            "name": "Protocol",

            "type": "String",

            "value": ""

          }

        ]

      }

    }

  ],

  "seq": 0,

  "uuid": "SPARKPLUG-BRIDGE"

}     

9.3.2 	NDEATH Message
~~~~~~~~~~~~~~~~~~~~~~

NDEATH is Node-Death.

Whenever Sparkplug-Bridge module’s connection with MQTT broker breaks, the MQTT broker publishes this message. The message is published in text format.

Following are sample contents in simplified JSON format:

**Topic**: spBv1.0/UWC nodes/NDEATH/RBOX510-00

Message: 

{

  "timestamp": 1592306298537,

  "metrics":

   [

    {

      "name": "bdSeq",

      "alias": 10,

      "timestamp": 1592306298537,

      "dataType": "UInt64",

      "value": 0

    }

  ],

  "seq": 0

}



9.3.3 	DBIRTH Message
~~~~~~~~~~~~~~~~~~~~~~

DBIRTH is Device-Birth.

On start-up, Sparkplug-Bridge module publishes this message over MQTT broker. The message is published in SparkPlug encoded format.

Following are sample contents in simplified JSON format for a Modbus device:


{

  "timestamp": 1608242600219,

  "metrics":

  [

    {

      "name": "iou",

      "timestamp": 1608242600219,

      "dataType": "Template",

      "value": 

      {
        "version": "1.0.0",

        "reference": "iou_datapoints",

        "isDefinition": false,

        "metrics": 

        [

          {

            "name": "D1",

            "timestamp": 1608242599889,

            "dataType": "Int16",

            "properties":

            {

             "Scale": 

             {

              "type": "Double",

              "value": 1

             },

              "Pollinterval": 

              {

                "type": "UInt32",

                "value": 1000

              },

              "Realtime": 

              {

                "type": "Boolean",

                "value": false

              }

            },

            "value": 0

          },

          {

            "name": "D2",

            "timestamp": 1608242599889,

            "dataType": "Int32",

            "properties": 

            {

             "Scale": 

             {

              "type": "Double",

              "value": 1

             },

              "Pollinterval": 
              
              {

                "type": "UInt32",

                "value": 1000

              },

              "Realtime": 

              {

                "type": "Boolean",

                "value": false

              }

            },

            "value": 0

          }

        ],

        "parameters": 

        [

          {

            "name": "Protocol",

            "type": "String"
            ,
            "value": "Modbus TCP"

          }

        ]

      }

    }

  ],

  "seq": 1

}

9.3.4 	DDEATH Message
~~~~~~~~~~~~~~~~~~~~~~

DDEATH is Device-Death.

Sparkplug-Bridge module publishes this message over MQTT broker whenever it detects that device is not reachable. The message is published in SparkPlug encoded format.

Following are sample contents in simplified JSON format:

{

"timestamp":1599467927490,

"metrics":[],

"seq":7

}

9.3.5 	DDATA Message
~~~~~~~~~~~~~~~~~~~~~

DDATA is Device-Data.

Sparkplug-Bridge module publishes this message over MQTT broker whenever it detects a change in value of any of metrics of devices. The message is published in SparkPlug encoded format.

Following are sample contents in simplified JSON format for a Modbus device:

{

  "timestamp": 1608242631070,

  "metrics": 

  [

    {

      "name": "iou",

      "timestamp": 1608242631070,

      "dataType": "Template",

      "value":

      {
        "version": "1.0.0",

        "reference": "iou_datapoints",

        "isDefinition": false,

        "metrics":

        [

          {

            "name": "D1",

            "timestamp": 1571887474111145,

            "dataType": "String",

            "value": "0x00"

          }

        ]

      }

    }

  ],

  "seq": 2

}

Following is sample contents in simplified JSON format for a Modbus device with scalefactor applied:

{

  "timestamp": 1621951388659
  ,
  "metrics": 

  [

    {

      "name": "flowmeter",

      "timestamp": 1621951388659,

      "dataType": "Template",

      "value": 

      {
        "version": "1.0.0",

        "reference": "flowmeter_datapoints",

        "isDefinition": false,

        "metrics": 
        
        [

          {

            "name": "D1",

            "timestamp": 1621951388658,

            "dataType": "Int32",

            "value": 2910

          }
          
        ]

      }

    }
    
  ],

  "seq": 2

}


9.3.6 	NCMD Message
~~~~~~~~~~~~~~~~~~~~

NCMD is Node-Command.

SCADA Master can tell edge node to reinitiate the node birth process. The node starts publishing NBIRTH, DBIRTH messages after receiving NCMD.

Following are sample contents in simplified JSON format:

**Topic:** *spBv1.0/UWC nodes/NCMD/RBOX510-00*

**Message:** 

{

  "timestamp": 1615619351980,

  "metrics": 

  [

    {

      "name": "Node Control/Rebirth",

      "timestamp": 1615619351980,

      "dataType": "Boolean",

      "value": true

    }

  ],

  "seq": -1
  
}



