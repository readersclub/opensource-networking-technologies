
# Chapter 1. Introduction to Open Source Networking


> Objectives
- Familiarize yourself with current networking landscape.
- Analyze the limitations of legacy networks when they are pushed to their boundaries in cloud environments.
- Discuss modern networking components, and how they will shape and integrate with the cloud.


Network Device

![image](https://user-images.githubusercontent.com/414141/81547979-4f218700-939c-11ea-9a98-11b6d4c93fb8.png)

![image](https://user-images.githubusercontent.com/414141/81547860-27caba00-939c-11ea-953c-9e964ddcfbba.png)

Any networking device consists of the following main components on their chassis:
- Packet processor and forwarding
- CPU, along with RAM and flash
- Other controllers, such as a fan controller, console port, LED controller, interface controllers, etc.
- Software

Legacy networking devices currently on the market, such as routers and switches, are all sold as a bundle of hardware and software.

The cloud environment virtualizes Compute, Storage and Network. The move to the cloud and building private clouds was the main driver of change for the modern disaggregated networking industry. To get the network virtualized, the community created an standard elastic, flexible Software Defined Network (SDN) on top of the hardware network. This was only possible by decoupling the networking devices, hardware and software which was done by using BIOS (Basic Input Output System).

BIOS is a standard hardware abstraction layer which exists in the servers. It is the key piece that allows basic communication between the operating system and the underlying hardware. It helps the server or computer to boot and run an OS kernel. Once the kernel is booted and ran, it starts communicating with hardware via different methods.

SDN enabled dynamic, programmatically efficient network configuration and improved network performance and monitoring making it more like cloud computing than traditional network management.

Different ways to build a SDN:
- Rip-and-Replace, Direct Fabric Programming: Remove existing
- Overlay: On top of existing
- Hybrid: Combine with other protocols like OpenFlow

The impact of SDN and modern networking for service providers is more significant, as it can help a service provider implement a flexible traffic engineering policy at the core backbone network. This allows the core network to forward the traffic in a way the service provider wants, instead of the way that the routing protocol dictates.

Along with the evolution of SDN and open networking, we started seeing bare metal switches with *Open Source Hardware*. Those switches were equipped with a standard BIOS-like system called ONIE (Open Network Install Environment). ONIE is similar to BIOS in computers, and allows an operating system to get installed on a switch or router and take control of the switch packet processing.


> Summary
- Reviewed the virtualization revolution on servers and storage. 
- Discussed the driving factors that are pushing the networking industry to evolve. 


*Modern networking and SDN provides an option to enhance the functionalities of the network, making it more change-friendly and integrated with cloud and virtualization platforms.*
