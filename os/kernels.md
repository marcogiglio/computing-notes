# Kernel

The task of an operative system is managing the physical resources and expose some form of API that can be used by programs.  Kernel architecture can be very different:

* Monolithic Kernel, such as the use used in Linux, MacOS X, Windows, Android and iOS. In this architecture, programs run in user-space while the kernel and the drivers run in kernel space.
* Microkernel, such as seL4 and RedoxOS. Microkernel's philosophy is to have the keep only the minimal functionality in the kernel and move everything outside.
* Unikernel, such as MirageOS

## Virtual Memory

Both micro and monolithic kernels implement an architecture mechanism called virtual memory.  This mechanism creates a unique mapping for each running process, between a virtual address space and the physical address space which includes physical memory as well I/O ports and devices.  Althought virtual address space is seen by the process as a continuous series of linear addresses, there is no such requirement for the mapping to physical memory.  Each process virtual address space is split into two parts: kernel space and user-space. Virtual memory addresses within the kernel space cannot be accessed by the running process. 

Virtual memory is segmented into continuous block of addresses called virtual pages, whose size depends from the operative system and the architecture, but usually it is around 4kb.  The physical memory correspondent of a virtual page is called page frame. The mapping between virtual pages and page frame is kept into a data structure in memory called page table. As accessing the pa:ge table can be computationally expensive, to increase efficiency, a small number of recently translated pages is kept in a memory cache called Translation Lookaside Buffer (TLB).  When a process tries to access a virtual address, a page fault interrupt occurs and it is caught by the kernel: firstly, the kernel checks if the TLB has already performed the translation. If it didn't, then the page table is checked. 

Virtual paging enables a second kernel mechanism, called swapping or paging, which moves rarely accessed pages from physical memory to disk, freeing the physical memory, without any modification or action from the running process. 

In micro-kernels, paging and memory management is left to be implement by user processes with high-privilege. 
