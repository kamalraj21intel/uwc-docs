=================
1.0  Introduction
=================


The Intel® UWC is a reference design for a secured management platform that provides third party application developers an easy access to data services including data collection from field devices, control data pathways, and connections to centralized data systems (i.e., SCADA) for Upstream Oil and Gas facilities, including gas well sites.

The Intel UWC platform will provide a secure, management platform for oil and gas upstream process monitoring and control to support oil and gas installations with various artificial lift methods such as plunger lift, gas lift, gas-assisted plunger lift, rod-beam and electronic submersible pump (ESP).
Intel’s primary objective in this market is to move the Upstream Oil and Gas vendors, service providers, and end-users to adopt Intel-based hardware hosting a rich range of open-architecture software-defined platforms. Solution is targeted to address multiple pain areas that the O&G industry is facing in day-to-day operations. These pain areas are further restricting the O&G industry to benefit from technology advancements resulting from cloud-based services and applications for business intelligence (BI), analytics, dashboards, and so on. There is a need to provide a uniform mechanism to connect, monitor and control various devices in an O&G well site adhering to real-time nature of the industry. 
While the Intel UWC software solution described in this User Guide contains a data model specific to a Gas Wellpad, the software is flexible and can be configured for use with other soft-RT process control sites and operating assets.

------------
1.1  Purpose
------------

This document provides information on how to install the UWC software framework and configure the device models, data model and data flows for data collection (from field devices) and reporting (such as to SCADA) at remote process control sites such as an oil or natural gas well (wellpad).  The document will help the developer to evaluate the performance of the data collection processes using an applet called the ‘KPI Application’.  The document will also help the developer to set up the security and manageability services.

----------
1.2  Scope
----------

This document aims to provide steps to build, deploy and check if the gateway is up and running all containers. This document also provides steps to set UWC containers for communication with Modbus devices. 

------------------------
1.3  System Requirements
------------------------

    • Intel processor family
    • 4GB system memory
    • 25GB hard disk space
    • Ubuntu 18.04 server operating system with RT Patch 
      (instructions for patching the Ubuntu OS are provided)
    • Docker >=19.03.8
    • docker-compose version >=1.24.0
