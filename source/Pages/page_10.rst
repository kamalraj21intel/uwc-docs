=====================
10.0 	Debugging steps
=====================

**Checking logs** 

1. Syntax - *sudo docker logs <container_name>*

For example, to check the modbus-tcp-container logs, run the *"sudo docker logs modbus-tcp-container"* command.

2. Run the command to check logs inside the container *"sudo docker exec -it <container_name> bash"* and then, go to the “logs” directory using *“cd logs”*.

3. Use the *"cat <log_file_name>"* command to see log file inside the container.

4. To copy logs from container to host machine refer to the following command:
  Syntax - *docker cp <container_name>:<file to copy from container> <file to be copied i.e., host directory>*

5. To check the IP address of machine, run the *"ifconfig"* command.

6. For Modbus RTU, to check attached COM port for serial communication, run the *"dmesg | grep tty"* command.

Redirect docker logs to file including errors

*docker logs modbus-tcp-container > docker.log 2>&1*

**Accessing container logs through docker volumes:**

Go to docker volume directory using 

*cd /opt/intel/eii/container_logs/<container_name>*

Where <container name> is directory name for each container (i.e. “modbus-tcp-master”, “modbus-rtu-master”, “mqtt-bridge”, and so on).

Example to access container logs for modbus-tcp-master container,

*cd /opt/intel/eii/container_logs/modbus-tcp-master*

*cat Modbus_App.log*

.. note::
  
   These logs are persisted across container/gateway restart.

**Steps to apply new configuration (i.e., YML files)**

Once YML files/docker-compose.yml are changed/Modified in */opt/intel/eii/uwc_data* directory then, execute following command to apply new configurations,

*sudo ./05_applyConfigChanges.sh*

**How to bring up/down the Universal Wellpad Controller containers**

*docker-compose down* - bring down all containers

*docker-compose up* - bring up all containers


