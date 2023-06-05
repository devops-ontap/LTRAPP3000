vManage
=====

Manages the configuration of the entire SD-WAN deployment
Single Pane of Glass GUI
One vManage can manage x routers 
maintains statistics database which is replicated between the cluster nodes for fault tolerance
Cluster of vManage min is 3 up to 6 
A Standby Cluster can be configured for DR

Cisco vManage provides the application interface GUI, collects operational data, presents statistical information, hosts troubleshooting tools, and manages configurations for all devices, templates, and policies. These are orchestrated through a set of services known collectively as the Network Management System (NMS).

vManage runs four main NMS services:

●      Application Server: This service is responsible for the web GUI interface, the APIs, and other tasks.

●      Statistics Database: This is a database of statistical data, audit logs, alarms, events, storing DPI info, etc.

●      Configuration Database: This is a database of all configuration information, policies, templates, certificates, and more.

●      Messaging Server: This is the service that passes all messages between the vManage devices in the cluster.

●      Coordination Server: This service coordinates the interation between vManage services.

●      Data Collection Agent: This collects the data and statistics about the operation of the SD-WAN deployment.

●      Cloud Agent: It manages cloud services.

●      Container Manager: It manages containers that run under vManage, such as SD-AVC

Plannning
======
Calculate edge routers numbers and the high availability needs
DPI volume
Control Connections to vManage are over vpn0