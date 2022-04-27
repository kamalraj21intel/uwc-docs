===================================
15.0 Appendix for UWC terminologies
===================================

**UWC contains these folders/files:**

.. list-table:: 
   :widths: 25 25 25 25
   :header-rows: 1

   * - #
     - Folder/File
     - Destination
     - Comment
   * - 1
     - Modbus-master
     - IEdgeInsights/uwc
     - Modbus application folder used to install TCP/RTU container
   * - 2
     - MQTT
     - IEdgeInsights/uwc
     - It is used to install the Mosquito (MQTT) container
   * - 3
     - mqtt-bridge
     - IEdgeInsights/uwc  
     - Mqtt-bridge application folder used to install mqtt-bridge container
   * - 4
     - sparkplug-bridge
     - IEdgeInsights/uwc
     - Sparkplug-Bridge application folder used to install Sparkplug-Bridge container. 
   * - 5
     - kpi-tactic
     - IEdgeInsights/uwc
     - kpi-tactic application folder used to install kpi-app container.
   * - 6
     - Vendor_Apps
     - IEdgeInsights/uwc
     - Vendor_Apps application folder used to install Sample publisher and subscriber.     
   * - 7
     - Others    
     - /opt/intel/eii/uwc_data/
     - All yml files containing device, datapoints, and configurations. It also contains Global_config.yml
   * - 8
     - uwc_common
     - IEdgeInsights/uwc
     - Common libraries installation docker file and source code. 
   * - 9
     - build_scripts
     - IEdgeInsights/uwc
     - All installation scripts are kept here
   * - 10
     - uwc_recipes
     - IEdgeInsights/uwc
     - This directory contains recipe files
