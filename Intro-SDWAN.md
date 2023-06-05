SD-WAN
Software Defined Wide Area Networking
=========
It is a Fabric


Co-Selling Opportunities
============
Service Providers 

189 Billion in Committed Spend with Microsoft.
90% of Fortune 500 with Microsoft

Some of the Largest Strategic initiatives for our F 500 companies are committed working to transition their estates to Cloud and Cloud Hybrid Architectures.





Provider Benefits
========
Allows Enterprise Organizations and Service Providers - interconnect to application and workloads in private or clouds using flexible transports.
Allows MSPs to manage multiple SDWANs or Tenants using a single set of service resources. MSPs do not need to reserve dedicated controllers for each Tenant.
MSP gets control, visualization and access into each Tenant with Provider Privileges
CapEx Savings - MSPs beneifit from CAPX by reducing services, rack space
Opex Savings - MSPS benefit from a common Dashboard
MSPs can scale from 3 node to 6 node cluster as needed.
They can deploy controllers in their own environment or in Cisco Cloud
Security and Segmentation for each tenant database and configurations
Telemetry Data and Quality metrics for each Tenant
Optimization of Resources

Enterprise WAN connections allow us to connect Cloud, DataCenter, Branches, Offices
Share Resources - ie)

Historically MPLS or frame relay (dedicated circuits) have been used

SD-WAN simplies the networking architecture and allows for extension of the landbased  networks into the cloud 

No need to backhaul traffic from ie)Branch to the Datacenter
Historically,
Saturation of WAN connections would occur with backhauling all the datacenter.
Branch to Datacenter over an MPLS Circuit - we would tunnel all our traffic from the branch to the datacenter (backhauled for the security services ie) traffic destined to cloud or internet as well

SD-WAN addresses this by  being able to interact with Cloud Applications. Example Office 365, AWS, Azure, Dropbox, etc.
Intelligent optimizes traffic flow
What happens to security and inspections? End to end traffic encryption via IPSEC and traffic inspection.
Anti-malware and Botnet protection

Traffic Transport Independence
Underlay is the physicl network infra responsible for the delivery of packets. 
SD-WAN is an overlay network, a virtual network built on top and decoupled from the underlay.
VPNs and VOIP are also overlays.

LTE, Serial, Wireless, Sattelite, MPLS - it doesn't matter. SDWAN can chose the best data path.


Simplies the benefits of the Network Administrator.


Cisco's Recommended SD-WAN is Viptela SD-WAN 

4  Planes in the SD-WAN

Data Plane -WAN Edge - Physical and or virtual. Cisco vEdge routers.
Control Plane - vSmart - Brain of SD-WAN. As we create Policies in vManage, vSmart is responsible for enforcing the policies. Policies are shared amount networks and devices. Uses known rules agains OMP
Management Plane - vManage, Config, Monitoring, Provisioning
Orchestration Plane -vBond - How the network is constructed and allows interconnected components work together - ZTP (Vbond can remotely provision the router anywhwere)

The Data Plane WAN Edge Routers connect with each other over IPSEC tunnels so traffic does not need to be backhauled to a datacenter as we did historically.
Image you have devices at different sites and all have different underlay network types like Satellite, LTE, MPLS, etc.

The vBond, vSmart and vManage establish secure control connections using DTLS to the various Edge Routers
This is used for provisoining and configuration
Routers can be hardware or software.



2. Key Design Elements
3. Planning and Migration
1. Architecture


Dataplane - Edge Devices
=======
Underlay - Routers, SD-WAN appliances, Different Transports - 4G, 5G, MPLS, LTE, Satellite. Can be both virtual and hardware.
Make up the dataplane

Cloud Onramp is a feature of the dataplane - it creates constant HTTP pings and DNS requests to determine the best and most performant path to the provider.
Security - can be local or cloud based. Can be a firewall and single box solution. You can also re-direct traffic to cloud
Application QoE - Quality of Experience = Packet Loss + Latency and is measured over multiple paths to determine which path has the least packet loss and latency. 

Control Plane -vSmart - manages routers and uses OMP (Overlay Management Protocol)
=======
exists on vSmart Controllers
They can push the routing and security configuration down and they scale horizontally - they scale out.
As you expand your business you just sping up additional controllers to handle the load.
X-number of tunnels and appliances have limits so you need to

vBond authorizes and authenticates the devices. The first time a device comes up it must talk to the vBond Controllers.
How does it do this?

You can have an on-prem controller (less desireable ie)vmware, microsoft, kvm) - this is not recommended but possible if necessary. You need to worry about redundancy etc.
Cloud is recommended - 99% of customers go with a Cloud Delivered Model.

How Does ZTP work?
================
Edge Creates a DTLS Tunnel with VBond, vBond looks at the digital cert and the serial number and makes sure it matches. 
Private Key needs to match on the digital certificate. If it authenticates and authorizes, vBond tells vManage and vSmart.
Establishes a DTLS(UDP)/TLS(443) with the edge devices.
vEdge begins to communicate with the sSmart
vSmart establishes an OMP session with the Edge Device, OMP can carry security + routing information and thus is more robust
Vedges start to talk to each other via IPSEC tunnel
BFD between vEdges for a quick failover. As soon as a BFD goes doen it immeidately switches the path over.


Management Plane - vManage
=========
Role Based Access
Monitoring and Troubleshooting
Single Pane of Glass
Supports Third Party Automation.
VAnalytics using Machine Learning and can analyze the entire environment for Bandwidth and can keep your Service Provider Honest. Tells you how apps perform.

Design
=======
the Edge Device VPNS are just VRFS, so they are separate encrypted segmentation via IPSEC.
EAch VPN can have its own topology, ie) hub and spoke, full mesh, poit to point.
You can chose a topology that meets your business needs.


Traffic Engineering
===========
1. Per Session Load Sharing = Active Active Load Sharing over a session - the life of that session will always take the same path.
Next Session a different path will be taken (Default)
2. Per Session Weighted (Device Configurable) - if you have 10 MB MPLS circuit and a 1 MB Internet you can use per sssion weighted.
3. Application Pinning(Policy Enforced) ie) you can PIN your realtime traffic to MPLS and if it fails we can use Internet as a BAckup
4. Application Aware Routing (SLA Compliant and Policy Enforced) - unver vManage we configure policy for our voice and we define a threshold : latency jitter, loss. If the policy is violated we switch 

DPI (Deep Packet Inspection)

Site Redundancy/Availability
====
Layer 2 - we can use VRRP
Layer 3 - OSFP, BGP 
Head/End - Redundant appliances 
Controller Perspective - Multiple Controllers to failover if any vSmart Controllers were to fail.


Security - These are CPU intensive so requries load testing and accurate planning
=====
Enterprise Firewall
Intrustion Protection
URL-Filtering
Adv Malware 
Secure Internget GAteway
SSL Proxy - Detects Threats in Encrypted Traffic


Migration - How does it work?
Consideration-
How many controllers what type of deployment 
Strong Regulations - Self Managed or On Prem?
Scale of Controllers, how many sites, how many tunnels, how many controllers plus redundancy
All Controllers are highly available and redundant
Firewall Ports - to enable the controllers to talk to each other - Firewall Ports
Data Center and Cloud Full Mesh, Hub and Spoke, QOS, SLAs, etc. 
Branch Sites, existing underlay or new circuits?
Look at different applications and flows?
Policies? Backhaul or local internet breakout?
Access to SAS and IAS.
Licensing, smart and virtual accounts etc.


Considerations
============
1. Cloud First
2. Datacenter
3. Branches or Sites 
Transition Strategy
1. Run in parallel
2. Run land based to cloud
3. Run X over

Firewall Ports
https://www.cisco.com/c/en/us/td/docs/routers/sdwan/configuration/sdwan-xe-gs-book/cisco-sd-wan-overlay-network-bringup.html#c_Firewall_Ports_for_Viptela_Deployments_8690.xml


Programmatic Interfaces
==================
REST
NETCONF


Multi-Tenancy
============





Tenancy
======
Onboard Tenant Devices using ZTP
Optimal Cloud Connectivity for Tenants 
UC Voice Services for Tenants

Layman's Explanation
=============
Instead of taking a line card and sticking it into two different chassis located in two different locations
We are sending data into one line card and go to the other line card.
Its a cloud distributed switch



Orchestrate and Manage multiple SDWANS
=============================
Each Tenant will have their own GUI view
Provier Dashboard, displayes all Tenants


Providing Cisco SD-WAN Managed Services
========
https://www.cisco.com/c/dam/en_us/partners/downloads/partner/WWChannels/download/sd-wan-nanaged-services-aag.pdf

Managed SD-WAN for Service Providers
============
https://www.cisco.com/c/dam/en/us/solutions/collateral/service-provider/service-offers-service-provider/managed-sd-wan-aag.pdf

https://www.ciscolive.com/c/dam/r/ciscolive/emea/docs/2020/pdf/BRKOPS-2316.pdf

https://www.cisco.com/c/en/us/solutions/enterprise-networks/sd-wan/what-is-multitenancy.html

