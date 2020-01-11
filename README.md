# How to move OPC UA Data to the Watson IoT platform
While modern information technology/IT systems use protocols like http, websockets, MQTT  (for IoT) and architectural styles (e.g. REST),   for operations technology/OT systems in manufacturing and plants a variety of other protocols are being used. In order to use this data a protocol conversion is needed. Fortunately OPC UA becomes a standard protocol and OPC UA servers can be used to collect that later can be consumed in OT (e.g. predictive maintenance) or combined OT/IT (worker assistants) e.g. uses cases . 

![Data flow]('./OPC-UA to WIoTP.jpeg')

These are the steps to consume (simulated) OPC UA data on the Watson IoT platform.

## OPC UA Server 
* Create Ubuntu Device (virtual machine) on the IBM Cloud (register to the IBM Cloud, if not done already)
* setup security groups for the ports that are needed for that decice opc ua, ssh

VNC is used to get graphical interface to the OPC-UA server
* Install the VNC Server in the Ubuntu VM
* Install VPN Viewer on your local machine
* login on the Ubuntu GUI via VNC

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
* configure the OPC UA node
* configure the Watson IoT node
