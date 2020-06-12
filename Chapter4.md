> Learning Objectives
By the end of this chapter, you should be able to:
    Explore the networking planes, hardware abstraction and data plane projects.
    Review data plane programming and acceleration.
    Review the software-based data planes of virtual switches.


### Types of Planes in a Network Device
You may have already heard about the different planes (Control Plane, Management Plane, Data Plane) in a network device (both virtual and physical network devices). In general, three main types of planes exist in a networking device:
    Control Plane
    The Control Plane is where the Network Operating System (NOS) and the networking application live. NOS controls and programs the Data Plane using drives, SDK, and hardware abstraction methods.
    Management Plane
    Normally, the Management Plane is a software that runs on NOS or on the Control Plane and provides a management interface of the networking device to the user.
    Data Plane
    The Data Plane is where network packets are floating. They enter from an interface (physical or virtual), come to the switch chipset (or virtual switch), get switched or routed (update the packet information, such as the destination MAC address, TTL, etc.), and then, they will be sent out via another interface. 

To allow communication between the Control Plane and the Data Plane, normally there is a hardware/IO abstraction software - this allows the software to directly call the hardware abstraction APIs to control the hardware (switch or network device chipset).
Communication between the Control Plane and the Data Plane
![image](https://user-images.githubusercontent.com/414141/84012070-b8de8200-a996-11ea-9455-a04fe4396b57.png)

In the following example, we can see how we can create a new VLAN on a physical Ethernet switch and add three ports to it. The tasks are initiated from the Control Plane by a software or CLI that issues that commands:

Task | API call from Control Plane to hardware abstraction | Call from hardware abstraction to chipset
-- | -- | --
Create new VLAN ID 500 | vlan_id=500;Create_vlan(vlan_id); | 0x8721;0x8734;0x876772829283;
Add port eth1, eth2 to vlan 500 | vlan_id=500;port=1;vlan_add_port(vlan_id,port)port=2;vlan_add_port(vlan_id,port) | 0x8722;0x8973;0x2389202;0x8722;0x8973;0x2389201;

Normally, the Control Plane programs and controls the Data Plane. However, in new architectures, the Data Plane is slowly moving to become software-defined. That means you can control the Data Plane directly, or write an application that interacts directly with the Data Plane, which can process packets at very high speeds (for example, terabits per second).
Note: The products and projects we will be discussing in this chapter are mostly related to Data Plane programming and IO abstraction. Some are related to hardware, and some are related to software-based, virtual networking. 


### Knowlegde Bridge
All networking products that you have worked with, such as routers, switches and wireless access points, have a similar setup. They are a bundle of hardware and software. For example, a Cisco 3750 switch comes with an operating system which works only with that switch model. You cannot change the software or get out of the switch CLI. These devices all use a switch chipset that performs the packet processing (classification, switching, routing). The switch chipset is also called the DataPlane. The switch silicon is controlled by a Control Plane, which is a software/firmware (for example, in a Cisco Catalyst switch, it is controlled by the IOS software which is running on a separate CPU chip).
Traditionally, the DataPlane has been built based on Not-Flexible ASICs that are controlled by the Control Plane by programming its tables with very basic functions.
The new switch chipset is coming with more flexible DataPlanes (switch chipset); this allows performing flexible packet processing and manipulation at very high speeds. This will help build more functional network devices: for example, a 100G switch which can perform NAT (network address translation) or in-line DDOS protection.


### DPDK (Data Plane Development Kit)
DPDK (Data Plane Development Kit) is a set of libraries and drivers for fast packet processing on a system, mainly using standard NIC (Network Interface Card) cards. DPDK is a tool that provides a framework for robust processing for building data plane applications. It is designed to run on any processors, such as x86, ARM, or IBM Power. It mainly runs in a Linux user space. DPDK is well-known on x86 computing devices (servers, computers, etc.) running Linux and using a DPDK-compliant NIC card.

DPDK - Quick Summary
Name | Data Plane Development Kit
By | The Linux Foundation
Where it runs | Linux, any x86, VM, or embedded system
What it does | Packet processing, routing, switching, encapsulation on the NIC card
Features | Environment-independent. Mostly used on appliances or x86 servers
What it can do out-of-the-box | You  can build networking applications using DPDK libraries that can process  packets at a high speed. DPDK APIs are very comprehensive, start from  NIC functions such as bonding and network protocol parsing (Ethernet,  ARP, ICMP, IPv4, IPv6, TCP, UDP, etc.), classification, QoS, ACL, etc.
Pros | DPDK  is a low-level library and one of the most robust frameworks for packet  processing. DPDK can achieve very high speeds in packet processing.
Caveats | Coding  and using DPDK requires low-level C programming skills, requires the  programmer to consider numerous parameters when using it.

DPDK Setup and Architecture
During the setup of DPDK, it creates an Environment Abstraction Layer (EAL), which will have specific components, such as CPU architecture, compilers, NIC drivers, etc. EAL abstracts and hides all of these underlying components, and provides a generic interface. Once the EAL is created, users will be able to link with DPDK libraries and use them to create their own packet processing applications.
DPDK architecture is based on a multi-core software with abilities to execute threads. The architecture might seem complex, but in summary, the following core components run DPDK:
  Memory Pool Manager
  Allocates memory for storing objects.
  Ring Manager
  Stores objects in memory allocated by the Memory Pool Manager. Also performs other functions.
  Network Packet Buffer Management
  The buffers in memory (allocated by the Memory Pool Manager). This library provides an API to allocate, destroy and manipulate packet buffers used to carry network packets.

![image](https://user-images.githubusercontent.com/414141/84012749-be889780-a997-11ea-9444-fb10b754dd9f.png)

DPDK Usage
To use DPDK, you need to initially set up the EAL environment. Once the EAL is set up, you can build your applications and compile them. When you are building an application with DPDK, you need to initialize the DPDK components and cores that you are using (for example, memory, buffers, ports, etc.).
DPDK libraries can be used in C language. You may find coding with DPDK a little complex as you need to take care of things such as memory and buffer allocation, and understand the logical cores. However, the applications that use DPDK can achieve a very high performance for packet processing in terms of number of packets per second or volume.
You can take a closer look at some DPDK sample applications provided in the DPDK documentation.
Since DPDK hooks from the user space directly to the NIC card, it is dependent on the NIC card to support DPDK functions. However, DPDK is NIC card-agnostic, meaning that the APIs that it abstracts are common. For example, an application that uses DPDK APIs to load frames from a NIC card buffer does not need any changes if the NIC card vendor is changed, but you may need to re-compile it.

Differences between a System with and without DPDK
![image](https://user-images.githubusercontent.com/414141/84012947-03143300-a998-11ea-8ec5-afe7e55110df.png)

The NIC cards that support DPDK are limited. However, the list is always changing as new NIC cards and vendors are added. Some of the NIC chipset manufacturers that provide DPDK-compliant chipsets for building NIC cards are Broadcom Inc., Cavium, Chelsio Communications, Intel, Marvell Technology Group, Mellanox Technologies, Netcope Technologies, Netronome, NXP Semiconductors, Solarflare, etc. Some examples of virtual NICs are virtio-net (QEMU), VMXNET3 (VMware ESXi), Cisco UCS eNIC, Amazon Elastic Network Adapter, etc.
[Video](...)


### FD.io (The Fast Data Project)
FD.io is a collaborative open source project hosted by The Linux Foundation. FD.io aims to establish a high performance Data Plane in the user space.

FD.io - Quick Summary

Name | The Fast Data Project (FD.io)
By | The Linux Foundation
Where it runs | Linux, any x86, VM, or embedded system (Runs in the User Space)
What it does | Packet processing, routing, switching, NAT
Features | It uses vector packet processing (VPP) mechanisms to achieve high performance
What it can do out-of-the-box | FD.io's VPP provides a command line tool called vppctl, which can be  used to interface with VPP to create interconnects, manage routing  tables, create tunnel interfaces, manage hardware acceleration, etc. You can use FD.io APIs to build virtual switches, virtual routers, virtual firewalls, or other packet processing applications.

FD.io is a packet processing engine that can run on a Linux-based host (such as an x86 or a VM). It provides functions such as routing, switching, NAT, filters, etc. It can use hardware acceleration when integrated with DPDK. 
Most of the FD.io code and plugins are in the User Space. The FD.io code has a very low dependency on the Linux kernel.

FD.io is based on three main components:
  Data Plane Management Agent: An agent software that allows a Control Plane software or an SDN controller (such as OpenDaylight) to control and communicate with FD.io.
  Packet Processing: The packet processing engine of FD.io to classify, transform, prioritize, forward, terminate packets.
  Network IO: The hardware acceleration driver, connecting FD.io with the network hardware (for example, DPDK).

FD.io Main Components
![image](https://user-images.githubusercontent.com/414141/84016929-b5022e00-a99d-11ea-8c4f-bc9514dc3a9a.png)

FD.io Communication with Other Networking Subsystems
![image](https://user-images.githubusercontent.com/414141/84017002-d19e6600-a99d-11ea-8a98-eaf0da984128.png)


FD.io can be used in servers to provide data plane functions to:
  Bare metal servers directly
  Virtual machines
  Containers.

![image](https://user-images.githubusercontent.com/414141/84017103-ebd84400-a99d-11ea-8f3f-b6dbcfdc25e7.png)

FD.io Deployment Model
![image](https://user-images.githubusercontent.com/414141/84017164-fe527d80-a99d-11ea-8eb2-b33ced0f5e1d.png)

FD.io - Vector Packet Processing
Vector Packet Processor (VPP) is the core component of FD.io. VPP can use DPDK for network IO and hardware-accelerated packet processing. VPP is a packet processing platform that can perform the following (below is a summarized list):
    Interfaces: VPP supports the following interfaces to be used as input an out: DPDK, TunTap, vhost.
    Tunnels/Encapsulation: VPP supports the following tunneling technologies: GRE, VXLAN, IPSec, MPLS over Ethernet or GRE, deep label stacks. User space applications can directly perform such encapsulation and decapsulation without the need to use the Linux kernel network controller.
    Routing and switching (no routing protocol): VPP provides direct access to many routing and switching features. Networking programs such as routing protocols (BGPD, OSPFD) will be able to use VPP to modify routing tables and other tasks: IPv4/IPv6, hierarchical FIB, VRFs, multi-paths, source RPF, segment routing, VLAN support, MAC learning, inbound ACL, proxy ARP.
    Network services and security: VPP supports NAT and other networking features and filters which can be used by security applications (source NAT, per-interface filters, DHCP, LLDP, BFD, policer, mirror/SPAN ports, IP flow export).

VPP reads the largest available vector of packets from the network IO layer.
Packet Vector
![image](https://user-images.githubusercontent.com/414141/84017373-48d3fa00-a99e-11ea-92f5-59976216b6a9.png)

VPP then processes the vector of packets through a packet processing graph.
VPP processes the entire packet vector. This method provides higher performance, as the first packet in the vector warms up the instructions cache, which helps the remaining packets get processes at a very high speed.

The following diagram illustrates the FD.io graph using VPP:
![image](https://user-images.githubusercontent.com/414141/84017440-630dd800-a99e-11ea-9476-ddc14f0b86f5.png)

> How Does FD.io Relate to DPDK?
  FD.io provides an abstract environment for building virtual routers, switches and packet processors. FD.io can use DPDK to communicate directly with NIC cards. DPDK provides hardware acceleration to FD.io. However, hardware acceleration and usage of DPDK is optional - an FD.io-based application can still run without DPDK.

> How Does FD.io Relate to IO Visor?
  The FD.io project and IO Visor (more about IO Visor in a little bit) are seen as complementary projects. The IO Visor Project focuses on dynamic runtime extensibility of data plane capabilities in the kernel. IO Visor aims to create a repository of IO modules that are portable across multiple possible data planes (like eBPF in the Linux kernel) and frameworks (like FD.io).


### IO Visor Project
According to IO Visor Project website, 
"The IO Visor Project is an open source project and a community of developers to accelerate the innovation, development, and sharing of virtualized in-kernel IO services for tracing, analytics, monitoring, security and networking functions".

The Open Networking Ecosystem
![image](https://user-images.githubusercontent.com/414141/84017719-c8fa5f80-a99e-11ea-8df6-5ed48e4d4abd.png)

IO Visor - Quick Summary

Name | IO Visor Project
By | The Linux Foundation
Where it runs | Linux, any x86, VM, or embedded system (in-kernel)
What it does | Packet processing, routing, switching, NAT
Features | Supports three data path methods: XDP (eXpress Data Path), BCC (BPF Compiler Collection), and eBPF
What it can do out-of-the-box | Since  IO Visor is mainly user in an in-kernel mode, it can help build robust  networking applications for networking within a host, between a host and  virtual machines or containers. It accelerates the in-host networking  functions.

FD.io and IO Visor are very similar. They both provide an abstraction layer to build networking applications. The main difference between FD.io and IO Visor is that an FD.io application runs in the User Space, but IO Visor runs in-kernel.

IO Visor Components
The IO Visor Project refers to a collection of open source components:
  At its heart, the IO Visor Engine is an abstraction of an IO execution engine
  A set of IO Visor Plugins to the Engine provides functionality to different areas
  IO Visor Project provides a set of development tools
  A set of IO Visor Tools for management and operations of the IO Visor Engine
  A set of Applications, Tools and open IO Modules built on top of the IO Visor framework.

![image](https://user-images.githubusercontent.com/414141/84017970-373f2200-a99f-11ea-8bc1-a6b720016f2b.png)

With IO Visor, you can write in-kernel programs that implement atomic networking, security, tracing, or any generic IO function. You can also attach these programs to sockets, so that they will be executed as traffic arrives in the kernel.

IO Visor - eBPF
eBPF provides an embedded universal in-kernel virtual machine, extending its use with networking and non-networking functions alike. Through this virtual machine structure, eBPF enables infrastructure developers to create any in-kernel IO module and load/unload them at runtime, without recompiling or rebooting.

eBPF - Loading New Modules
![image](https://user-images.githubusercontent.com/414141/84018157-71102880-a99f-11ea-9d3f-f8bdddb74a1d.png)

IO Visor - XDP
XDP, or eXpress Data Path, provides a high performance, programmable network data path in the Linux kernel as part of the IO Visor Project. XDP provides bare metal packet processing at the lowest point in the software stack, which makes it ideal for high-speed processing.
![image](https://user-images.githubusercontent.com/414141/84018300-9a30b900-a99f-11ea-8ae8-5b37e0f7d439.png)

Networking Based on IO Visor
With IO Visor, you can have a software instance of your switch, your router, your load balancer, or even a security appliance, which can be dynamically loaded to the Linux kernel and integrated with other components to form a complete network workflow.
Traffic can arrive there from a VM or a container and traverse the entire virtual in-kernel network locally, and leave the local compute node only when that’s required to reach an external destination. One of the biggest benefits of IO Visor is that you can program ANY network logic (present or future) for ANY version of your protocol, and then dynamically swap out the existing implementation for a new one.

eBPF Framework for Networking
![image](https://user-images.githubusercontent.com/414141/84018509-d6fcb000-a99f-11ea-8350-0cd531012bef.png)


### Open vSwitch (OVS)
According to the Open vSwitch website, 
"Open vSwitch is a production quality, multilayer virtual switch licensed under the open source Apache 2.0 license. It is designed to enable massive network automation through programmatic extension, while still supporting standard management interfaces and protocols (e.g. NetFlow, sFlow, IPFIX, RSPAN, CLI, LACP, 802.1ag). In addition, it is designed to support distribution across multiple physical servers similar to VMware's vNetwork distributed vswitch or Cisco's Nexus 1000V. See full feature list here".

OVS - Quick Summary

Name | Open Virtual Switch (OVS)
By | The Linux Foundation
Where it runs | Linux
What it does | Virtual switch/router with physical and virtual interfaces
Features | L2 switching, VXLAN encapsulation, distributed switches controlled by a controller
What it can do out-of-the-box | OVS is one of the core components of virtualization hypervisors

![image](https://user-images.githubusercontent.com/414141/84018784-22af5980-a9a0-11ea-9701-439f385082c9.png)

Open vSwitch Architecture
![image](https://user-images.githubusercontent.com/414141/84018795-280ca400-a9a0-11ea-8b58-15e170cc203a.png)

According to openvswitch.org, Open vSwitch supports the following features:

    Visibility into inter-VM communication via NetFlow, sFlow(R), IPFIX, SPAN, RSPAN, and GRE-tunnelled mirrors
    LACP (IEEE 802.1AX-2008)
    Standard 802.1Q VLAN model with trunking
    Multicast snooping
    IETF Auto-Attach SPBM and rudimentary required LLDP support
    BFD and 802.1ag link monitoring
    STP (IEEE 802.1D-1998) and RSTP (IEEE 802.1D-2004)
    Fine-grained QoS control
    Support for HFSC qdisc
    Per VM interface traffic policing
    NIC bonding with source-MAC load balancing, active backup, and L4 hashing
    OpenFlow protocol support (including many extensions for virtualization)
    IPv6 support
    Multiple tunnelling protocols (GRE, VXLAN, STT, and Geneve, with IPsec support)
    Remote configuration protocol with C and Python bindings
    Kernel and user-space forwarding engine options
    Multi-table forwarding pipeline with flow-caching engine
    Forwarding layer abstraction to ease porting to new software and hardware platforms.

Distributed Open vSwitch Instance
![image](https://user-images.githubusercontent.com/414141/84018891-4a9ebd00-a9a0-11ea-993f-68700023aea1.png)

OVS vs Linux Bridge
OVS supports advanced features and distributed environment comparing to standard Linux bridge.

Integration of DPDK Data Plane with Open vSwitch
![image](https://user-images.githubusercontent.com/414141/84019038-7fab0f80-a9a0-11ea-8e0e-a3c30676a93b.png)

[Video](...)


### OpenDataPlane (ODP)
OpenDataPlane is an open source project driven by the Linaro group. ODP is similar to DPDK, but its scope is wider and includes different hardware processors and SoC. According to OpenDataPlane's website, 

"The OpenDataPlane project has been established to produce an open-source, cross-platform set of application programming interfaces (APIs) for the networking data plane".

ODP - Quick Summary | 
Name | OpenDataPlane
By | The independent open source community is mainly driven by the Linaro Network Group
Where it runs | An abstraction layer, runs on x86, ARM and other embedded SoCs
What it does | Robust network programming for embedded systems
Features | Provides networking APIs
What it can do out-of-the-box | ODP  is similar to DPDK in x86, but it supports embedded systems and SoCs.  ODP provides a standard network abstraction for developers to write  their applications. Applications can be ported between different  hardware by re-compiling them. You can implement applications such as  NAT, switching, routing, classifications, IPsec encapsulation using ODP  over the supported platforms.

Currently, ODP is supported on the following chipsets:
  Cavium Octeon™ SoCs
  Cavium ThunderX™ SoCs
  Kalray MPPA (Massively Parallel Processor Array)
  Freescale QorIQ – ARM-based DPAA2 architecture LS2080, LS2085 QorIQ – ARM and PowerPC-based DPAA architecture LS1043
  Texas Instruments (TI): Keystone II SoCs
  Marvell ARMADA SoC implementation
  Linaro ODP-DPDK - software-optimized implementation using DPDK
  NXP QorIQ SoCs
  HiSilicon platforms.

Also, ODP is supported on Linux as:
  odp-linux: It's a pure software implementation which runs on any Linux kernel; not a performance target.
  odp-DPDK: Intel x86 using DPDK as a software acceleration layer.

ODP provides an open source API for data plane networking and programming mainly for SoC chipsets. The main goal of ODP is to create a common set of APIs to support different types of chipsets and SoCs. Applications that use ODP will be able to be ported between different platforms. ODP also allows applications to use hardware offloading features of different chipsets and processors without the need to write hardware-specific code for a specific processor.

The following diagram illustrates the relationship between ODP and other components:
![image](https://user-images.githubusercontent.com/414141/84022976-fcd98300-a9a6-11ea-8c56-1e564791ce68.png)

Currently, there are various available implementations of ODP across a different range of platforms and architectures, including x86, ARM, MIPS, and proprietary SoC architectures.
![image](https://user-images.githubusercontent.com/414141/84023093-34482f80-a9a7-11ea-95cb-4e6fc56e4230.png)

> ODP Use Cases
ODP can be used to build software-based network functions such as routers, firewalls, load balancers, and other networking functions in a software. 
The following diagram illustrates an example ODP flow, from receiving a packet, to processing and sending out.

ODP Packet Flow
![image](https://user-images.githubusercontent.com/414141/84023239-75d8da80-a9a7-11ea-8507-6e8ebf0b7c67.png)


### Open Container Initiative (OCI)
The Open Container Initiative (OCI) is a Linux Foundation collaborative project to design open standards for container systems. This project specifies standards for different aspects of containers in order to build a platform where all container engines are able to run containers from other container engines.
There are currently two specifications defined and developed by OCI:
  Runtime Specification (runtime-spec) 
  Image Specification (image-spec).


OCI - Quick Summary | 
Name | Open Container Initiative
By | The Linux Foundation
Where it runs | Linux
What it does | Standardizes the packaging and running of containers
Features | Currently, provides specs for exporting containers and running containers
What it can do out-of-the-box | OCI  includes a software called runC, which can be used to interact with  containers in different container platforms, such as Docker, Kata or  Linux containers.

OCI develops and maintains runC, which is a container runtime that implements the OCI specification and serves as a foundation for other high-level tools. The runC code was initially donated by Docker, Inc. OCI supports multiple container platforms, including Docker and Kata. Kata is a virtualization engine created by the OpenStack Foundation. It aims to create a platform to run lightweight virtual machines which are similar to containers. Kata also provides security and isolation between containers.

OCI - Filesystem Bundle
The filesystem bundle specification defines a format for encoding and saving a container's set of files. This bundle includes all the files, data, and metadata required to run a container.
The standard container bundle contains all the files and information needed to load and run a container package. This includes a configuration file that references the locations of the container root filesystem and other files related to the container, as well as the container's main filesystem root.

OCI - Runtime Bundle
The OCI runtime bundle is a standard specification for creating a bundle directory that includes all of the files required to launch an application as a container. The bundle contains an OCI configuration file where it can specify host-independent details such as which executable to launch and host-specific settings such as mount locations, hook paths, Linux namespaces and cgroups. Because the configuration includes host-specific settings, application bundle directories copied between two hosts may require configuration adjustments.

Open Container Initiative and Open Virtualization Format
You may find that the Open Container Initiative provides a similar function as the Open Virtualization Format (OVF) initiative did a few years ago. With OVF, you could export a virtual machine from a hypervisor (for example Xen) as an OVF file, and import the OVF file into another hypervisor, such as VMware.
- OVF defined open standards for virtual machines and hypervisors.
- OCI defines open standards for containers and container engines.


### SmartNIC 
SmartNIC is a new term in the industry, referring to network cards that are programmable in terms of packet processing. Most of us do not care about a network card (it is an IO device) and most people do not go beyond installing drivers or, in some cases, configuring bonding, EtherChannels or load balancing, and in some cases using them for offloading TCP checksums.

![image](https://user-images.githubusercontent.com/414141/84024519-c0f3ed00-a9a9-11ea-8169-22794f6277d7.png)

SmartNICs - Quick Summary
--
Name | Smart Network Interface Cards (SmartNICs)
By | Multiple manufacturers, such as Netronome, Napatech, Mellanox Technologies, etc.
Where it runs | It's a physical PCIe card
What it does | Offloads packet processing from the host, processes packets at high speed in NICs
Features | L2, L3 packet processing, custom application, etc.
What it can do out-of-the-box | You  can build custom applications that run on SmartNIC chipsets (for  example, a DDOS protector, IPS or packet encapsulators for IPsec, SSL,  firewall - NGFW -, load balancing, etc.

SmartNICs have additional capabilities when compared to traditional NIC cards. They are programmable to do additional functions. For example, you can program a SmartNIC card to add an 802.1q tag with a VLAN ID=100 to all packets being sent to a specific IP address.

SmartNICs are based on programmable silicons which can do high speed packet processing. Most of them are coming as 10G, 40G or 100G cards, and can perform line rate packet processing.

For example, you can build a DDOS protector at 100Gbps using a SmartNIC, or offload the virtual switch processes of a server to a SmartNIC.

SmartNIC with FPGA
![image](https://user-images.githubusercontent.com/414141/84024605-e54fc980-a9a9-11ea-9f39-ad32bb987555.png)

SmartNICs can be programmed using their SDK or using the standard P4 language. Most of SmartNICs now support P4 programming. However, they need the vendor’s compiler and tool chain in order to compile the P4 program. P4 programs are portable - you can compile the same P4 application for different SmartNIC models by compiling the P4 application using the relevant vendors’ compiler.

Programming Protocol-independent Packet Processors or P4 is a new language that was created to define and express how packets should be processed by the data plane. The data plane must be programmable and capable of supporting P4; it can belong to a switch, router, SmartNIC, SoC, etc.

The differences between a P4-compliant and a Traditional switch are:
  The data plane functionality in a traditional switch is fixed. However, in a P4 programmable switch it is not fixed and it will be defined by a P4 program.
  The communication method between the control plane and data plane in a traditional switch is fixed, with specific APIs and specific tables from the data plane exposed to the control plane. However, in a P4-compliant switch, the APIs between the control plane and the data plane are defined by the P4 program and are not fixed.

SmartNIC Programmability
![image](https://user-images.githubusercontent.com/414141/84024706-14fed180-a9aa-11ea-8269-8f5e41e7a9b8.png)


FPGAs and Xilinx SDNet
Xilinx, Inc. is a chipset maker well-known for their Field Programmable Gate Array (FPGA) products. FPGA is a programmable chipset which allows you to build your functions that run in a FPGA chip at a very high speed. FPGAs are expensive, but very flexible.

SDNet - Quick Summary
--
Name | SDNet
By | Xilinx Inc.
Where it runs | On Xilinx FPGA
What it does | SDNet  is a library used to build networking applications in Xilinx FPGA to  implement fast packet processing in FPGA at different speeds, such as  1Gbps, 10Gbps, 100Gbps, etc.
Features | Any packet processing function, such as switching, routing, classification, packet manipulation, ACL, etc.
What it can do out-of-the-box | You can use SDNet (which now supports P4 languages) to build your packet processing application inside a Xilinx FPGA chip. You can build a router, stateful firewall, IPs, IDs, DDOS protector, MPLS encapsulator, VXLAN encapsulator and router, etc.
Pros | You  can build packet processing applications that run at very high speed,  from 1Gbps to 400Gbps. Support of P4 language makes it easy to work with  FPGA.
Caveats | FPGAs  that can process packets at high speeds are generally expensive.  Building applications for FPGA requires FPGA skills and knowledge, and  the coding requires attention to numerous aspects.

FPGAs are costly chipsets that provide both performance and flexibility. The following illustration explains the key differences between CPU, ASIC and FPGA:
![image](https://user-images.githubusercontent.com/414141/84024821-46779d00-a9aa-11ea-8e82-4073bd864c13.png)

For example, you can build a basic Layer 2 switch using an FPGA:
![image](https://user-images.githubusercontent.com/414141/84024878-6018e480-a9aa-11ea-86c7-c1ad1c5c0b50.png)

When working with FPGA, you should know exactly what bytes you are reading or writing. You need to understand the 802.3 Ethernet frames, IP packets and fields, TCP, UDP, etc. From an FPGA point of view, the FPGA is performing a set of actions (matching, manipulating) on a stream of bytes. FPGA does not know about Ethernet or IP fields; instead, your application should implement that.

If you need to make any changes, you need to re-calculate the Cyclic Redundancy Check, or CRC, or the TCP checksum if you make changes in TCP, and add the new CRC amount. To make such calculations, you can use the freely available functions for CRC calculation inside your FPGA.

![image](https://user-images.githubusercontent.com/414141/84024915-6c04a680-a9aa-11ea-81e2-2d57e23eeefd.png)

Note: To build an FPGA-based network application, you don’t need to extract all fields of Ethernet, IP, etc. You only need to extract and compare what is relevant for you. For example, if you are building a DDOS protection application, you need to extract only fields such as the Source & Destination IP address and TCP ports (or, in some advanced methods, a pattern), and compare them against a predefined list in your FPGA. If the packet matches that pattern, FPGA should just drop the packet.

> Xilinx SDNet
Xilinx has created a framework called SDNet, which allows developers to build data plane programs to run in their FPGA chips. Initially, it was only supporting the Xilinx’s proprietary SDK (SDNet). However, it now supports the standard P4 language.
The use case of FPGAs is very similar to other platforms we talked about in this chapter. They can do fast packet processing and networking functions such as switching, routing, VXLAN offloading and routing, IPS/IDS, packet encapsulation (e.g. MPLS, GRE, VXLAN), etc.
FPGAs provide fast performance. However, they are expensive, and you may need to acquire some FPGA skills in order to set up the environment and be able to develop an application for FPGA.
Xilinx provides its own P4 compiler for compiling the users' P4 applications and run them in their FPGA chips.

To design a data plane application that runs on an FPGA, you can start by drawing  the state machine or a simple flow diagram showing how you would like your application to work. You can take a look at the following illustration of a simple routing application:
![image](https://user-images.githubusercontent.com/414141/84025191-d4ec1e80-a9aa-11ea-9dac-75bff7bb5c9e.png)

The above diagram shows a router that processes an incoming packet, checks the FIB and CAM tables to find what fields need to be changed in the packet (i.e. the destination MAC, TTL, etc.). The editor will build the output packet based on the new fields, new CRC and checksums, and then it will send it out.
By default, an FPGA has no specific hardware configuration. To start, you need to create your ports and interfaces and map them to your logics. The process of designing your FPGA can be done using the FPGA manufacturers' special software tools. In most cases, you will be creating a schema over Very High Level Design (VHDL) language. After you design the FPGA and have your ports ready, you can start building the application.

To test FPGAs, you can buy an FPGA development board, which normally comes with interfaces for testing your application.


### Barefoot Networks Tofino Programmable Switch Silicon

Barefoot Networks is a relatively new company with a new approach towards switch chipsets. They have created a hardware technology that allows software to directly program the forwarding plane on their switch chipset.

Barefoot Networks Tofino Switch Chipset - Quick Summary
--
Name | Barefoot Network Tofino Switch Chipset
By | Barefoot Networks
Where it runs | A physical chipset (ASIC) for Ethernet switches
What it does | Apart  from the standard features of an Ethernet switch silicon, it adds  flexibility for data plane programming in a switch by supporting the P4  language and allowing to directly program the data plane
Features | Up to 6.5 Tbps; supports 32 x 100G ports; supports P4 language
What it can do out-of-the-box | Tofino  is a switch chipset which can be used to build an Ethernet switch.  Hardware vendors can use this chipset to build an off-the-shelf Ethernet  switch. Software vendors can use the Barefoot software tools to build  networking software that can run and drive the chipset, as well as use  the chipset's advanced packet processing features.

Barefoot Tofino supports the P4 language for building data plane applications that can use the Tofino chipset to perform fast packet processing. Similar to other P4-compliant vendors, Barefoot Networks also provides its own P4 compiler, which allows the P4 programs to get compiled and built for Barefoot Tofino chipsets. As we learned earlier, the P4 program is hardware-agnostic, and can be ported from one platform to another. For example, you may be able to write a DDOS protection application using the P4 language, compile it for a SmartNIC, and also compile the same source code for a Barefoot Tofino switch chipset using Barefoot’s P4 compiler (keep in mind that the compiler tool chain will be different in each hardware). You may remember that the posting of P4 to different platforms is similar to moving a C program source code between different systems. You can compile your C program on Microsoft Windows, on x86 Linux, or on an ARM-based Linux. The compilers are different, but can compile and build a common C program.

Barefoot Tofino uses a Protocol Independent Switch Architecture (PISA). It is protocol-independent because the chip has no awareness of the network protocols it supports. Similar to FPGAs, the Tofino chipset only knows about byte streams.

The P4 program provides the logic for implementing different networking features and services. The switch manufacturer can use this flexible platform to build Ethernet switches which can support future software changes and additional features.

Below you can see a Wedge 100BF-32X switch, which is produced by Edge-Core using Barefoot's Tofino chipsets:
Switch based on Barefoot Tofino
![image](https://user-images.githubusercontent.com/414141/84025564-87bc7c80-a9ab-11ea-8dc1-b9046f8b5473.png)

As we learned, Barefoot Tofino is a switch silicon with additional support for data plane programming. Tofino can be used by switch hardware manufacturers such as Edge-Core, Quanta, etc., to build a complete Ethernet switch.

Switch software manufacturers are also using the Tofino software development chains to build a switch operating system that can drive the Tofino chipset, as well as utilizing the advanced functionalities of Tofino for data plane programming.

Software vendors will be able to build switch operating systems and network applications for Barefoot Tofino that can go beyond the standard L2/L3 routing and switching, but can provide extra features, such as firewalling, NAT, load balancing, DDOS protection, etc.

Arista Networks 7170 switches, which launched in 2018, are based on Barefoot Tofino chipsets. Arista Networks has extended their software to support additional features of Tofino, such as large scale Network Address Translation (NAT), large scale tunneling MPLS over GRE/UDP, Deep Packet Inspection (DPI); they also provide P4 features for users to allow building their own switches.


> Learning Objectives (Review)

You should now be able to:
  Explore the networking planes, hardware abstraction and data plane projects.
  Review data plane programming and acceleration.
  Review the software-based data planes of virtual switches.

> Summary

Project | Description
-- | --
DPDK | A  hardware abstraction library which can be used for fast communication  with supported NIC cards. The DPDK library allows robust communication  between kernel and NIC cards with less CPU cycles.
FD.io | A set of libraries in the user space  that can help you build packet processing applications. FD.io can use  hardware acceleration and DPDK to provide a high performance packet  processing. FD.io uses the VPP method, which processes an array of  packets at the same time.
IO Visor | Similar to FD.io, but it's a set of libraries that provide packet processing features within the kernel.
OpenDataPlane | A hardware abstraction for supported packet processing SoCs. The  library provides standard, common APIs which can be used by applications  to perform network tasks without the need to consider the hardware.  This library supports embedded chips from TI, Marvell, Freescale  Semiconductor, etc.
Barefoot Tofino | An Ethernet switch chipset which supports data plane abstraction and  programming using the P4 language. Tofino is currently the only switch  silicon that supports data plane programming and packet processing for  multiple 100G ports.
SmartNICs | NIC cards that allow you to use them for packet processing. You can  load a packet switching or modification program on the SmartNIC chipset  itself (without even using the system CPU) and let the SmartNIC perform.  You can use the SmartNICs for many off-loading functions such as  encapsulation (GRE, VXLAN, IPSec, etc.) or other packet classification  functions.
Xilinx SDNet | As one of the major FPGA providers, Xilinx is also offering FPGA  chipsets that can be used for data plane and fast packet processing.  Initially, Xilinx provided SDNet, which is their proprietary SDN for  building data plane applications. Later, they added support for the  standard P4 language. You can use Xilinx FPGA to build high performance  processing.
Open Container Initiative | An open source project for governance and standardizing container  formats and runtimes. OCI has defined the specification for filesystem  bundle (the open specification for exporting and storing container  files) and the runtime bundle (the open specification for running a  container).

Flexibility vs Performance in Different Platforms
![image](https://user-images.githubusercontent.com/414141/84025833-f3064e80-a9ab-11ea-8f03-6b468d9d3feb.png)



