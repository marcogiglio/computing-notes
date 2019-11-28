## Xen

In the following, we are going to analyze Xen architecture as a typical bare metal hypervisor. Xen separates the virtual machine its running in two type of domains: domain 0 or host domain (Dom0) and user domains (DomU). Dom0 is a full system, most of the time is Linux. Dom0 contains Xen management stack and depending from the configuration, it can have the drivers talking to the actual hardware.

In a typical configuration, each DomU will access devices using virtualized drivers which talk to Dom0 over a special cross-vm protocol. Dom0 will run the actual drivers. In a different configuration. called DriverDomain, hardware drivers are isolated within their own domain and the role of Dom0 is assigning and enforcing access to the device's PCI slot. This capability is called PCI pass-through.

There are different type of virtualization strategies, depending from the hardware and the software support:

- **PV**: require support from the guest OS.
- **HVM**: it requires QEMU to emulate the hardware.
- **PVH**: an hybrid between the two above

##### Paravirtualization

In paravirtualization mode (PV), the guest OS is aware of the hypervisor and the guest kernel is modified to support it.  

Virtual memory management for all guests is handled directly by Xen, not by the guest kernel, so that there is no need to perform dual address translation. Unfortunately, this implementation has been shown to be complex and responsable for many high profile Xen Security Advisory. 

Within the guest kernel there is also driver support for virtualized I/O block, networking and other devices: this driver called Frontend driver communicates to the Backend driver running in Dom0 using a combination of XenBus, XenStore, shared pages and event notifications.

The most recents version of Xen can be compiled without PV mode support to improve security.   

##### Hardware Virtualization Mode

In Hardware Virtualization Mode, the guest OS doesn't have to be aware of the hypervisor: it believes it is running on bare-metal hardware. 
This mode requires the virtual machine to run with a complete device model, a feat done using QEMU: when a kernel driver, for example the one for an IDE disk, tries to send a request by accessing the virtual memory address, QEMU traps the request and translate it to an host OS call. 

In the most simple architecture, one QEMU instance for each HVM guest would run within Dom0. Since QEMU has a large trusted-computing base (TCB), it can and it should be isolated isolated within its own user domain. This domain, called stub domain, uses a very light kernel running in PV mode. 

Since every guest manages its own virtualized memory, there are now two layers of address translation involted from a process virtual memory to the physical memory. To accelerate, modern CPU have extensions which can speed up second-layer address translation considerably: EPT/VT-x for Intel processors and RVI for AMD processors.

##### PVH

This mode is an hybrid of the previous ones and it requires support from the guest OS. It leverages in-kernel paravirtualized drivers, but also uses the native and more secure second-layer address translation implement in modern CPUs instead of letting Xen manage virtual memory for the guests. Therefore it doesn't require QEMU. 

##### References

[https://wiki.xen.org/wiki/X86_Paravirtualised_Memory_Management](https://wiki.xen.org/wiki/X86_Paravirtualised_Memory_Management)

[https://docs.openstack.org/security-guide/compute/hardening-the-virtualization-layers.html](https://docs.openstack.org/security-guide/compute/hardening-the-virtualization-layers.html)

[https://wiki.xen.org/wiki/Grant_Table](https://wiki.xen.org/wiki/Grant_Table)
