
# Chapter 2. Open Source and Software Defined Networking Landscape


> Objectives
- Familiarize yourself with the concept of open source networking.
- Discuss the grouping of software defined networking technologies based on the Linux Foundation Networking Fund framework.
- Explain the functions and use cases of each group of software defined networking technologies.
- Understand how these open source networking technologies communicate with each other.


![image](https://user-images.githubusercontent.com/414141/81550753-6793a080-93a0-11ea-9242-7c346266544d.png)
The networking landscape in the software defined networking (SDN) and open source networking projects and products in different groups or layer.


The hierarchical layers in the landscape:
- Disaggregated Hardware
- IO Abstraction and Datapath
- Network Operating Systems(NOS)
- Network Control
- Network Virtualization
- Cloud and Virtual Management
- Orchestration, Management, Policy
- Network Data Analytics
- Application Layer


**Disaggregated Hardware** basically means to be enable to to load or install your own software or operating system in a specific router or a switch that allows you. The main requirement of Disaggregated and Open Hardware is to have its software separated from the hardware. The most simple form of a disaggregated networking hardware is a standard PC or a server with multiple network adapter cards.

Disaggregated Network Device inside an Open Hardware
![image](https://user-images.githubusercontent.com/414141/81551100-fbfe0300-93a0-11ea-80e0-6d9ef38b1fc3.png)


> Differences between a network device that runs Linux and a purpose-built network device

Function | An x86 server running Linux | Dedicated Ethernet switch or router
-- | -- | -- 
Packet Processing | Packets are sent to CPU and OS to make forwarding, routing or firewalling decisions | Packets are processed in packet processor ASIC (Application-Specific Integrated Circuit) silicon, not the CPU 
Throughput | Limited to server hardware, such as CPU, I/O bus, kernel. Normally, limited to Gbps | Depends on the ASIC model. Varies from Gbps to tens of Tbps
Boot process | System boots as a normal PC, loads the Linux kernel, OS and networking software (for example, iptables, etc.) | A tiny OS runs on CPU and starts driving the ASIC
Port density | Limited to the number of ports on the server, or additional ports via a PCIe (Peripheral Component Interconnect Express) card | Ethernet switches can support 48 or 52 ports in 1U form factor


**IO Abstraction and Datapath** includes projects and products provided by hardware vendors in order to program the hardware with SDKs and APIs for programming the silicon ASIC (Application-Specific Integrated Circuit) which allows to communicate with the hardware. Furthermore, IO Abstraction and Datapath management is not limited to hardware, but there are also products and projects that abstract and manage the kernel.

Hardware Abstraction in an Open Network Device
![image](https://user-images.githubusercontent.com/414141/81552369-1a64fe00-93a3-11ea-8d06-e29de2e28bc7.png)


**Network Operating System (NOS)** is an operating system that runs on the management plane of a network device. This operating system is designed to drive the packet processor hardware chipset, such as a switch silicon, and perform the tasks required for forwarding, routing, and switching. It hosts networking applications that can communicate directly with networking device ASICs and packet processor chipsets. 
It may also have networking software modules such as Spanning Tree, OSPF (Open Shortest Path First), BGP (Border Gateway Protocol), SNMP (Simple Network Management Protocol), SSH server, etc. Examples: ONL (Open Network Linux), Facebook’s FBOSS (Facebook Open Switching System), Microsoft’s SONIC (Software for Open Network in the Cloud) and OpenSwitch from The Linux Foundation.

Disaggregated Network Hardware
![image](https://user-images.githubusercontent.com/414141/81552532-6021c680-93a3-11ea-9373-6e07bd03750c.png)


**The Network Control ( or SDN controllers layer )** is the brain that can manage multiple network operating systems via agents and protocols and has all the information about the network including the devices in the network. It consists of multiple physical or virtual switches and routers.

The Network Controller Can Manage Virtual or Hardware Switches/Network Devices
![image](https://user-images.githubusercontent.com/414141/81553475-d5da6200-93a4-11ea-9c8f-9849228d4471.png)

An SDN Network Can Scale Out
![image](https://user-images.githubusercontent.com/414141/81553721-4da88c80-93a5-11ea-84b9-e014cd49f3da.png)


**Network Virtualization** (or an overlay network ) is about creating virtual networks on top of physical networks. It is mainly based on encapsulation technologies. The endpoints send data packets to each other with addresses that may not be routable in an underlying network. The overlay hosts encapsulate the original data packets into other packets (such as GRE or VXLAN) and send the packets to a physical underlay network towards the destination host. The destination host decapsulates the packet and delivers it to the endpoint. It can be created using virtual switches or platforms such as OpenStack Neutron, OVS (Open vSwitch), VMware NSX, or a hardware VTEP (VXLAN Tunnel Endpoint) gateway.

Overlay networks are virtual networks on top of physical networks, built using encapsulation and tunnels
![image](https://user-images.githubusercontent.com/414141/81553885-919b9180-93a5-11ea-9f27-a8ce46b7de63.png)

Packet transfer steps in an overlay network
![image](https://user-images.githubusercontent.com/414141/81554015-c60f4d80-93a5-11ea-81ad-0abeceb379b9.png)


**Cloud and Virtual Management Layer** is normally referred to as PaaS (Platform as a Service) tools and products that communicate with the network control layer.

Cloud platforms such as OpenStack, open container orchestration, the CORD project and VMware vSphere can drive the network control layer. Such cloud platforms require you to set up virtual networks, create virtual network functions or perform service insertion. Cloud platforms communicate directly with the network controller to perform their required tasks.

Cloud and Virtual Management Layer Communicates with Network Controllers
![image](https://user-images.githubusercontent.com/414141/81554524-ae849480-93a6-11ea-8aa7-1f936b9a5eb5.png)

As a simple example, imagine that you are creating a new tenant (project) in OpenStack. You need to create a new virtual network for five virtual machines of this tenant to be able to communicate with each other. As a cloud platform, OpenStack will be able to call the required APIs on network controllers such as OpenDaylight to create a virtual tenant network.

OpenStack only requests the network controller (in this case OpenDaylight) to create a virtual network. It is the network controller’s job to translate this request into networking tasks, and find out which network devices (virtual or physical) are in the transit path between the five virtual machines. After it finds the relevant network devices, it will start programming their forwarding tables using the southbound protocols (such as OpenFlow, SNMP, NETCONF or CLI).


**Orchestration, Management, Policy Layer** is where most __NFV (Network Function Virtualization)__  agement platforms are operating. Orchestration tools such as Open Source MANO, Open Security Controller, Akraino Edge Stack, ONAP, and Apache ARIA TOSCA are automation platforms that can create virtual network functions and infrastructure.

Orchestration Platforms Communicate with Network Controller and Cloud Management Layers
![image](https://user-images.githubusercontent.com/414141/81555108-a2e59d80-93a7-11ea-9803-6374fdbc952d.png)

Such orchestration platforms are mainly used for automation and batch creation of a service.

For example, the Open Security Controller, which is a software-defined security platform, can enforce security in a network. It will communicate with virtualization management platforms to create virtual firewalls or virtual routers, and ask cloud virtualization platforms to create the required virtual machines for these new virtual firewalls (aka virtual network functions, or VNFs).

The Open Security Controller also communicates with the network controller to create service insertion. For example, it can tell OpenDaylight to create a policy to ensure that all traffic to a database server is always sent to a virtual IDS (Intrusion Detection System) before hitting the database server.


**Network Data Analytics** utilizes new technologies and tools such as Big Data, machine learning, and AI to provide knowledge about the network. Such projects and tools are different from the traditional monitoring and data gathering tools that most organizations use on a day-to-day basis.

3 projects form The Linux Foundation:

            SNAS (formerly OpenBMP, stands for Streaming Network Analytics System) is a framework to collect, track and access tens of millions of routing objects (routers, peers, prefixes) in real time. OpenBMP was created to analyze BGP (Border Gateway Protocol) and implemented IETF BMP (BGP Monitoring Protocol). OpenBMP converted to SNAS to provide more functions and use cases for a networking analytics platform.

            PNDA (Platform for Network Data Analytics) is a Big Data analytics tool with multiple networking use cases and integrations with network controllers such as OpenDaylight or log management platforms such as Logstash and SNAS.

            Acumos AI is The Linux Foundation’s artificial intelligence platform, with multiple use cases. It’s a modular platform that can access data from different sources. Developers can build AI applications on top of Acumos to analyze the data, find patterns and predict facts. Acumos has an open marketplace where developers upload their AI applications. Acumos gets integrated with ONAP (Open Networking Automation Platform) and OpenDaylight as data sources, allowing developers to create AI networking applications.

The Linux Foundation Network Analytics Projects
![image](https://user-images.githubusercontent.com/414141/81555502-69f9f880-93a8-11ea-9ffb-f4a115b3c85a.png)
