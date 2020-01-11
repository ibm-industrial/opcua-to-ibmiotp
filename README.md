# Transfer OPC-UA Data to the IBM Internet of Things platform
While modern information technology/IT systems use protocols like http, websockets, MQTT  (for IoT) and architectural styles (e.g. REST),   for operations technology/OT systems in manufacturing and plants a variety of other protocols are being used. In order to use this data a protocol conversion is needed. Fortunately OPC UA becomes a standard protocol and OPC UA servers can be used to collect that later can be consumed in OT (e.g. predictive maintenance) or combined OT/IT (worker assistants) e.g. uses cases . 
  
![Data flow](OpcuaToWiotp.jpeg)

These are the steps to consume (simulated) OPC UA data on the IoT platform.

## OPC UA Server 
* create a [Ubuntu VM](https://cloud.ibm.com/classic/devices) with an Ubuntu 16.04 on the IBM Cloud (register to the IBM Cloud, if not done already) with a public IP address
* note down the root password
* setup [security groups](https://cloud.ibm.com/classic/security/securitygroups) for the ports that are needed for that device: allow_opc_ua/inbound&outbound/53530, allow_ssh/inbound/22, allow_vnc/inbound/5900-5999 and assign them to the VM

VNC is used to get a graphical interface/desktop to the Ubuntu VM running the OPC-UA server
* ssh into the VM 
~~~~
# ssh root@<public-address>
~~~~
* install the [VNC Server](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-16-04) in the Ubuntu VM
* run the VNC server in the background so it doesn't stop when you are closing the terminal session

~~~~
# vncserver &
~~~~

* install a VPN Viewer on your local machine, e.g. VNC Viewer
* login to the Ubuntu desktop via VNC

We are using the Prosys OPC-UA Simulation server to create some OPC-UA simulation data
* download and install the [Prosys OPC-UA Simulation Server](https://www.prosysopc.com/products/opc-ua-simulation-server/), you might need to install a browser (e.g. Firefox) first
* run the Prosys OPC UA Simulation Server from the Ubuntu desktop and note down the ocp.tcp address, e.g. opc.tcp://your-opc-ua-address:53530/OPCUA/SimulationServer
* Optional (for testing purposes): install an OPC UA client on your local machine and connect to the OPC UA server using the ocp.tcp address

## Node-RED application
Node-REDs used to receive any incoming OPC-UA messages from the OPC simulation server and send them to the IoT platform
* install Node-RED locally, as a Docker container or as part of the [Node-RED starter kit] (https://cloud.ibm.com/catalog/starters/node-red-starter) on the IBM Cloud
* install the *node-red-contrib-opcua*, *node-red-dashboard* and *node-red-contrib-scx-ibmiotapp* nodes via the Hamburger icon > Manage palette
* import the [Node-RED flow](./node-red-flow) 
* configure the OPC-UA client node *OPC-UA*: Endpoint = opc.tcp://your-opc-ua-address:53530/OPCUA/SimulationServer

To be completed:
* configure the IBM IoT node

## IoT Platform
* create an [Internet of Things Platform service(]https://cloud.ibm.com/catalog/services/internet-of-things-platform) and note down your Internet of Things Organization ID, e.g. lt9l36
* create a device, which represents the interface to the Node-RED application; note down Device Type (e.g. OPCUA), Device ID (e.g. opc_ua_1) and the Authentication Token
