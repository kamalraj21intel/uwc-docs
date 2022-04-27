================
1.0 Introduction
================

Universal Wellpad Controller is a reference design for a secured management platform that provides third party application developers an easy access to data services including data collection from field devices, control data pathways, and connections to centralized data systems (i.e., SCADA) for Upstream Oil and Gas facilities, including gas well sites.

Universal Wellpad Controller platform will provide a secure, management platform for oil and gas upstream process monitoring and control to support oil and gas installations with various artificial lift methods such as plunger lift, gas lift, gas-assisted plunger lift, rod-beam and electronic submersible pump (ESP).
Intel’s primary objective in this market is to move the upstream oil and gas vendors, service providers, and end-users to adopt Intel-based hardware hosting a rich range of open-architecture software-defined platforms. Solution is targeted to address multiple pain areas that the O&G industry is facing in day-to-day operations. These pain areas are further restricting the O&G industry to benefit from technology advancements resulting from cloud-based services and applications for business intelligence (BI), analytics, dashboards, and so on. There is a need to provide a uniform mechanism to connect, monitor and control various devices in an O&G well site adhering to real-time nature of the industry. 
While the Universal Wellpad Controller software solution described in this User Guide contains a data model specific to a Gas Wellpad, the software is flexible and can be configured for use with other soft-RT process control sites and operating assets.


-----------
1.1 Purpose
-----------

This document provides information on how to install the Universal Wellpad Controller software framework and configure the device models, data model and data flows for data collection from field devices and reporting such as to Supervisory Control and Data Acquisition (SCADA) at remote process control sites such as an oil or natural gas well (wellpad). The document will help the developer to evaluate the performance of the data collection processes using an applet called the ‘KPI Application’.  The document will also help the developer to set up the security and manageability services.

---------
1.2 Scope
---------

This document aims to provide steps to build, deploy, and check if the gateway is up and running all containers. This document also provides steps to set Universal Wellpad Controller containers for communication with Modbus devices. 

------------------------
1.3  System Requirements
------------------------
The requirements for Universal Wellpad Controller reference middleware are: 

  * Intel® processor family
  * 4 GB system memory
  * 25 GB hard disk space
  * Ubuntu 20.04 server operating system with RT Patch (Instructions for patching the Ubuntu OS are provided)
  * Docker version 20.10.6 or above
  * Docker Compose version 1.29.0 or above

When enabling the Data Persistence recipe (with InfluxDB* and Telegraf*), if data access queries are intensive or desired database size is large, it is recommended to use higher performance and capacity components for CPU, memory, and storage: 

  * Four or more physical or virtual cores 
  * Intel® Core™ processor family 
  * 8 GB memory 
  * 64 GB hard disk space 
  * Ubuntu 20.04 server operating system with RT Patch (Instructions for patching the Ubuntu OS are provided)
  * Docker version 20.10.6 or above
  * Docker Compose version 1.29.0 or above
