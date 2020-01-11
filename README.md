# Move OPC UA Data to the Watson IoT platform
While modern information technology/IT systems use protocols like http, websockets, MQTT  (for IoT) and architectural styles (e.g. REST),   for operations technology/OT systems in manufacturing and plants a variety of other protocols are being used. In order to use this data a protocol conversion is needed. Fortunately OPC UA becomes a standard protocol and OPC UA servers can be used to collect that later can be consumed in OT (e.g. predictive maintenance) or combined OT/IT (worker assistants) e.g. uses cases . 

![Data flow](OpcuaToWiotp.jpeg)

These are the steps to consume (simulated) OPC UA data on the Watson IoT platform.

## OPC UA Server 
* Create device with an Ubuntu 16.04 virtual machine on the IBM Cloud (register to the IBM Cloud, if not done already) with a public IP address
* setup security groups for the ports that are needed for that decice opc ua, ssh

VNC is used to get a graphical interface/desktop to the Ubuntu VM running the OPC-UA server
* Install the [VNC Server](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-16-04) in the Ubuntu VM
* run the VNC server (vncserver)
* Install a VPN Viewer on your local machine
* login to the Ubuntu desktop via VNC

We are using the Prosys OPC-UA Simulation server to create some OPC-UA simulation data
* Download and install the Prosys OPC UA Simulation Server, you might need to install a browser (e.g. Firefox first)
* Run the Prosys OPC UA Simulation Server and note down the ocp.tcp address
* Optional (for test purposes): create an OPC UA Client on your local machine and connect to the OPC UA server usinf the ocp.tcp address

## Watson IoT Platform
* Create Watson IoT Platform service
* Create a device, note down Organization ID, Device Type (e.g. OPCUA), and Device ID (e.g. opc_ua_1) and the Authentication Token

## Node-RED application
Node-REDs used to receive any incoming OPC-UA messages from the OPC simulation server and send them to the Watson IoT platform
* Install Node-RED on the IBM Cloud
* Import the Node-RED flow Move
* Configure the OPC UA node
* Configure the Watson IoT node
