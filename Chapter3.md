# Chapter 3. Disaggregated Hardware

> Objectives
By the end of this chapter, you should be able to:
  Discuss about the hardware of disaggregated and open networking Ethernet switches and wireless access points.
  Review the Open Compute Project and the Telecom Infra Project.

You should remember that all Ethernet switches have a similar hardware design. They have two main components:
  A packet switching/processing ASIC chipset controls
  A CPU which hosts the software/firmware and the packet switching ASIC.


All commercial vendors lock their software with their hardware, meaning that you cannot make any changes to software, nor install a different software.
A bare metal switch includes the same hardware components, but allows you to install a network operating system.
A network operating system should support the hardware which you are planning to use. For example, if you have a 32 x 100G port switch based on Cavium, you need to choose a NOS that supports that hardware (for example Open Network Linux).
Network Operating Systems provide a hardware compatibility guide, which can help you understand what hardware they support.

In this chapter, we will take a more in-depth look at hardware disaggregation and open source hardware of network switches and wireless access points. 

Commercial Networking Products vs Open Source Hardware
![image](https://user-images.githubusercontent.com/414141/84004319-1371e100-a98b-11ea-994a-ab54b0a7d57d.png)

As we already mentioned, a disaggregated network device allows you to install your choice of operating system. You are not locked or bound to use a specific software. 


Proprietary Network Device | Details
-- | --
Cisco Catalyst 3750 Switch | A combination of hardware and Cisco IOS software
Juniper M10iRouter | A combination of hardware and Juniper Junos OS
Arista 7170 Switch | A combination of hardware and Arista EOS software


Disaggregated Network Device | Details
-- | --
Edge-Core AS5712 (48 x 10G switch) | Comes with no software/OS.You can check compatibility and install OpenSwitch/Open Network Linux (ONL)/Cumulus Linux/Pica8/Big Switch, etc.
Mellanox SN2700 | Comes with no software/OS.You can check compatibility and install Cumulus Linux.
Alpha Networks SNX-60x0-486F(48-port 10G SFP) | Comes with no software/OS.You can check compatibility and install ONL/OpenSwitch/Cumulus Linux, etc.
Inventec DCS7032Q2832 x 100GB | Comes with no software/OS.You can check compatibility and install ONL/OpenSwitch/Cumulus Linux, etc.


### Open Compute Project (OCP)

OCP was announced by Facebook, along with Intel, RackSpace, Goldman Sachs, and Andy Bechtolsheim, in April 2011. The effort was the result of a redesign of Facebook's data center in Prineville, Oregon. The aim of OCP is to create open source standards for high density and highly-efficient IT equipment for data centers, including server, storage, network, and security.

OCP publishes the open hardware specifications for the data center and enterprise IT systems. There are multiple project workgroups within OCP, each including a project charter and a team working towards producing and enhancing the open source technologies within that project.

The Data Center Facility Project focuses on the design of a data center facility that maximizes mechanical performance and thermal and electrical efficiency. The project focuses on five functional areas of the data center:
  Data Center Facility Power
  Data Center Facility Cooling
  IT Space Layout and Design
  Data Center Facility Monitoring and Control
  Data Center Facilities Operation.

The High Performance Computing (HPC) Project focuses on "developing a fully open heterogenous computing, networking and fabric platform for a multi-node processor that is agnostic to any computing group".

The Rack & Power Project specifies standards for OCP racks which are in 19” and 21” form factors. The racks are designed to provide direct power to the server in a highly efficient way.

The Hardware Management Project is related to small embedded systems that manage the hardware, either a PCB board or a rack. OpenBMC is an open source project hosted by The Linux Foundation that is part of this category. OpenBMC (Open Baseboard Management Controllers) is very similar to management systems you may have seen on most servers, such as HPE iLO (Integrated Lights Out), Dell DRAC (Dell Remote Access Controller), and IPMI (Intelligent Platform Management Interface), where there is a System on a Chip (SoC) that has its own CPU, memory and operating system, and sensors that are connected to the system that it's managing and monitoring (such as a server or a network switch).
![image](https://user-images.githubusercontent.com/414141/84004800-deb25980-a98b-11ea-849d-6901867578b5.png)

The Server Project aims to build highly efficient servers similar to blades that plug directly into a rack. OCP servers are designed to support high-density configurations and to have multiple servers within 1U of space. This project includes open source hardware designs for blade servers, chassis and management systems. 
![image](https://user-images.githubusercontent.com/414141/84004868-f5f14700-a98b-11ea-8a12-4a7d3ccf5a82.png)

The Storage Project provides open source specifications for building high-density storage systems with high performance and a simple hardware design such as JBOD (Just a Bunch of Disks). The redundancy and filesystem is handled by software (such as ZFS).
![image](https://user-images.githubusercontent.com/414141/84004975-1a4d2380-a98c-11ea-8c60-51ab8cbef7fc.png)

The Open Compute Networking Project provides open source hardware specifications to build networking devices, mainly Ethernet switches. OCP networking specifications are mostly contributed to companies such as Facebood, Mellanox Technologies, Edgecore Networks, etc. The OCP network switch specification provides switch hardware designs starting from 48 x 1G up to 32 x 400G ports. These switches are based on merchant silicon chipsets from companies such as Mellanox Technologies, Broadcom Inc., and Cavium, with a CPU board (based on Intel or ARM processors) which supports standard network operating systems.

A merchant silicon is a readymade chipset that delivers a standard set of functions, which is sold by a chip manufacturer. For example, a switch silicon provides standard L2/L3 switching functions.
![image](https://user-images.githubusercontent.com/414141/84005091-4b2d5880-a98c-11ea-9898-dabd0101ca8c.png)

Non-merchant silicons are mainly designed by vendors (such as Cisco, HPE, Juniper, etc.). The designs are delivered to a silicon manufacturer to build a proprietary chipset for that vendor.
![image](https://user-images.githubusercontent.com/414141/84005119-584a4780-a98c-11ea-9489-22a7ead93a0e.png)

The Open Network Install Environment (ONIE) is an open source initiative of the Open Compute Project which provides a standard method to install a network operating system on bare metal network switches. Once a bare metal switch starts, it loads ONIE firmware to help install a network operating system
![image](https://user-images.githubusercontent.com/414141/84005183-6e580800-a98c-11ea-96ff-9647fc59ee54.png)

Once ONIE starts, if no OS is installed on the switch, it shows a standard Linux GRUB boot loader menu.
![image](https://user-images.githubusercontent.com/414141/84005228-816ad800-a98c-11ea-85c4-fc2e59fc8df3.png)


### Telecom Infra Project
Similar to OCP, which addresses open source hardware and software used to build data centers and enterprise IT, the Telecom Infra Project is a workgroup meant to create open source standards for telecommunication and service provider companies. The Telecom Infra Project includes multiple project groups:
  Backhaul Projects
  The Backhaul project group includes projects such as Multimeter wave (mmWave) networks and Open Optical and Packet Transport projects. The aim of this project group is to enhance the functionalities in wireless and backhaul networks by introducing options for ease of configuration, faster convergence, and better scalability.
  
  Access Projects
  This project group includes projects such as RAN, Edge Computing and site optimization. The aim of this project group is to innovate and enhance the connectivity within the Radio Access Network (RAN) to make the internet connectivity easier and more stable. The RAN (Radio Access Network) project group itself includes projects such as OpenCellular, OpenRAN, CrowdCell and vRAN Fronthaul.
  
  Core & Management Projects
  This project group includes projects such as AI (Artificial Intelligence), AMI (Applied Machine Learning) and end-to-end network slicing. The aim of this project group is to improve the efficiency and flexibility of telecom core networks, while simplifying the processes and reducing the operation costs.


### How Ethernet Switches Are Built
Now, let’s take a look at how Ethernet switches are built. After reading this section, you will find out Ethernet switches are based on fairly simple design.
It starts with a silicon vendor; you need to choose which chipset you want to use to build your Ethernet switch.
To make it simple, there are very few companies worldwide producing merchant Ethernet switch chipset (Silicon). A merchant silicon is a chipset that is already designed, tested and built by a chipset manufacturer, which can be bought by anyone looking to build an Ethernet switch.
The following table provides a high level description of a chipset manufacturer and most recent chipsets:

Chip Manufacturer | Chipset
-- | --
Broadcom Inc. | Strata SGX Family:   Helix (1G) Trident 2, 2+, 3 (10G/40G) Tomahawk 2, 3 (100G/200G/400G)   Strata DNX Family (Large buffer):   Qumran Jericho
Mellanox Technologies | Spectrum, Spectrum 2 (10G/40G/100G/200G/400G)
Cavium | XPliant (1G/10G/40G/100G)
Barefoot Networks | Tofino (10G/40G/100G)
Marvell Technology Group | Prestera switching family
Microsemi (Vitesse) | Gigabit switch chipsets
Intel Corporation | FM6000 series (10G/40G)

Notes:
  We did not include the small SOHO chip manufacturers.
  If you would like to learn more about how a switch chipset silicon works, you can download the [Intel FM6000 datasheet](https://www.intel.com/content/www/us/en/ethernet-products/switch-silicon/ethernet-switch-fm5000-fm6000-datasheet.html)

Once you sign the non-disclosure agreement (NDA) with a chipset manufacturer, you will get access to a world of information (thousands of pages of datasheets, schematic designs, PCB layouts, software specifications, drivers, etc.).

An Ethernet switch hardware has a simple design and components. In simple terms, a switch consists of the following components:
    Chassis: ust the metal part, with your choice of colors and assembly (For example, a pink or lipstick red would make it unique).
    Power supplies:Two redundant AC-DC power supply systems, it's ready-made available from many factories.
    Fans: nough to cool down and dissipate the heat.
    Control System: o control fans, system management.
    CPU PCBA: n x86, Power PC ,or ARM-based processor with its RAM, flash, and PCIe, which runs the NOS.
    Switch main board PCBA: The main board which hosts the main switch silicon, interface cages, CPLDs (Complex Programmable Logic Devices), and PHYs (in the case of RJ45 interfaces), this PCB normally has between 16 to 22 layers.

Take a look at the below switch diagram. It’s a real picture of an Edge-Core AS5712 48 x 10G, 6 x 40G switch based on Broadcom Trident 2. The middle-fat heat sink is protecting the Broadcom Trident 2. Normally, these BGA ASICS are large, as they have many interface pins underneath the silicon.

An Edge-Core AS5712 48 x 10G, 6 x 40G Switch 
![image](https://user-images.githubusercontent.com/414141/84005744-546af500-a98d-11ea-894f-b9b427c8af70.png)

The CPLDs (Complex Programmable Logic Devices) are chipsets that have duties for system start up, managing the LEDs, fans, and temperature sensors. The CPLDs are connected back to the CPU through the UART (Universal Asynchronous Receiver-Transmitter) or the I2C bus. I2C (IC to IC Interconnect) is a digital communication protocol used mostly in electronic systems. It is similar to RS485, where you can daisy chain multiple devices, and each device will have a static IP.

The diagram below shows the connectivity between different components in the switch:
![image](https://user-images.githubusercontent.com/414141/84005814-6a78b580-a98d-11ea-91e9-9c221beae885.png)

The CPU board has a direct PCIe connection to the switch chipset. This connection requires a driver, which is provided by the chipset vendor. CPLD 1 has the function to control the fans, temperature, and LEDs. This CPLD has an exposed I2C interface which is connected to the CPU board. CPLD 2 and CPLD 3 control the SFPs. Remember that this switch doesn't have any on-board PHY, and uses the 10G/1G SFPs for interfaces. CPLD 2 and CPLD 3 control the SFPs.

### Types of Switches
You may have heard about the terms of white-box, brite-box, and bare metal switches. These terms are very similar, but have some minor differences.
A bare metal switch is an Ethernet switch without a NOS (Network Operating System). A bare metal switch normally covers both open source hardware, as well as proprietary hardware. Some examples of bare metal switches are:
  An Edge-Core AS4610 (open source hardware)
  A Mellanox SN2700 (proprietary hardware).

A white-box switch is similar to a bare metal switch, with a few optional features:
  You can ask the manufacturer to label it with your own brand (OEM)
  They are sold without any NOS
  In general, they cover open source hardware switches.
For example, an Edge-Core AS4610 (open source hardware) is a white-box switch.

Brite-box switches are switch hardware bundled with a NOS. This model was popular a few years ago, when companies like Pica8 were selling their NOS along an Edge-Core switch as a bundled product (switch hardware and NOS).

The following table summarizes the differences between white-, brite- and bare metal switches:

Type | You can install any NOS | What is included with purchase | Hardware Support | NOS | NOS Support
-- | -- | -- | -- | -- | --
Bare metal switch | Yes | Switch hardware only | Hardware manufacturer | Purchased separately | NOS vendor
White-box switch | Yes | Switch hardware only | Hardware manufacturer | Purchased separately | NOS vendor
Brite-box switch | Not supported by its vendor | Switch hardware and a NOS | Company selling the brite-box switch | Already included | Company selling the brite-box switch

There are multiple bare metal switches available on the market. These switches come with different speeds and port densities. Next, we will look at a few notable bare metal switches.
Edge-Core Switches: Edge-Core is the bare metal switch brand name of Accton Technology. The Edge-Core portfolio of bare metal switches includes many product lines based on different switch silicons. All Edge-Core switches are shipped with a CPU board and an ONIE environment for easy installation of a NOS. A few notable 

Edge-Core switches are highlighted below:
Switch Model | Main Ports | Switch Chipset
-- | -- | --
AS4610 | 48 x 1G RJ45 | Broadcom Helix 4
AS5712 | 48 x 10G | Broadcom Trident 2
AS5812 | 48 x 10G | Broadcom Trident 2+
AS5912 | 48 x 10G | Broadcom Qumran-MX
AS6712 | 32 x 40G | Broadcom Trident 2
AS6812 | 32 x 40G | Broadcom Trident 2+
AS7816 | 64 x 100G | Broadcom Tomahawk 2
AS7712 | 32 x 100G | Broadcom Tomahawk
AS7512 | 32 x 100G | Cavium Xpliant
AS7900 | 32 x 400G | Broadcom Tomahawk 3

Facebook Switches: They are Ethernet switches manufactured for Facebook by companies such as Accton Technologies. They are known as Wedge and Backpack Switches. Some examples of Facebook switches are highlighted below:

Switch Model | Main Ports | Switch Chipset
-- | -- | --
Wedge | 16 x 40G | Broadcom Trident 2
Wedge 100 | 32 x 100G | Broadcom Tomahawk
Backpack (Chassis-based) | 128 x 100G | Broadcom Tomahawk
Wedge 100C | 32 x 100G | Cavium Xpliant
Wedge 100B | 32 x 100G / 65 x 100G | Barefoot Tofino T10

Wedge System Building Blocks
![image](https://user-images.githubusercontent.com/414141/84006045-ce02e300-a98d-11ea-8caf-ee9735bd31c3.png)
 
Facebook Wedge 100B based on Barefoot Networks Tofino Chip 
![image](https://user-images.githubusercontent.com/414141/84006061-d6f3b480-a98d-11ea-9ae8-db1bda1950fb.png)

Mellanox Bare Metal Switches: They are based on Mellanox Spectrum/Switch-X2 series of chipsets. Below are a few examples:

Switch Model | Main Ports | Switch Chipset
-- | -- | --
MSX1410 | 48 x 10G / 12 x 40G | Mellanox Switch-X2
MSX1710 | 48 x 10G / 36 x 40G | Mellanox Switch-X2
SN 270 | 48 x 10G / 32 x 100G | Mellanox Spectrum

Barefoot Tofino-Based Switches: Barefoot Networks produce programmable switch silicons. Please take a look at the following example:

Switch Model | Main Ports | Switch Chipset
-- | -- | --
Wedge 100B | 32 x 100G / 65 x 100G | Barefoot Tofino T10

Recently, Arista Networks also announced their new line of switches, Arista 7710, which is based on Tofino chipsets.

[Video](TODO)


### Bare Metal Wireless Access Points
Wireless access points have become commodity hardware and there are initiatives to produce bare metal, open hardware wireless access points. Similar to Ethernet switches, wireless access points are based on a CPU system on a chip (SOC) controlling multiple wireless radio chips for 2.4GHz, 5GHz, and 802.11ad 60GHz radios.

Bare metal wireless access points come with no firmware/operating system. Customers can choose and install the operating system and software features they need. Currently, the only supported OS for wireless access points is Linux (mainly, the OpenWRT Linux).
![image](https://user-images.githubusercontent.com/414141/84007258-d52af080-a98f-11ea-84de-4448dcfdab33.png)

To better understand, you can imagine a wireless access point as a PC with single or multiple Wi-Fi cards. The CPU drives the wireless modules, creates SSIDs (Service Set Identifiers), sets encryption, authentication, etc. 

OpenWRT Linux is a open source distribution that aims to support wireless access points. It includes many drivers for CPU and Wi-Fi cards. You can find compatible OpenWRT Linux firmware, install it on your home access point and have fun controlling your access point, even for consumer-grade wireless access points (such as TP-Link, Linksys, etc.).

OpenWRT is an open platform for wireless access points. It allows loading different software modules to control the wireless. For example, on a wireless access point running OpenWRT:
  You can install CAPWAP (Control and Provisioning of Wireless Access Points protocol) software to create a lightweight access point that can be controlled by a wireless controller.
  You can install an SNMP or a NETCONF server software to allow your wireless access point to be managed by an SDN controller such as OpenDaylight.

Currently, only Edge-Core is producing bare-metal wireless access points. There are different models, based on different chipsets from Qualcomm, Broadcom, etc. Bare metal access points come with no operating system.

The other wireless access points are mostly sold with an operating system which can be changed to OpenWRT. You can take a look at the OpenWRT webpage to see what access points are supported. 

You can also find more details on the OCP Wiki for Networking/Specs and Designs page.

[Video](TODO)


> Learning Objectives (Review)
You should now be able to:
  Discuss about the hardware of disaggregated and open networking Ethernet switches and wireless access points.
  Review the Open Compute Project and the Telecom Infra Project.

> Summary
In this chapter, we reviewed the hardware architecture of Ethernet switches and wireless access points. We've presented some good hardware examples from the Open Compute Project, and we discovered how disaggregated Ethernet switches are built and how they can help the industry to improve the performance and software features by decoupling the hardware and software from each other.
