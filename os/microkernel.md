# Microkernels

Microkernel do not provide high-level abstraction to access devices and file system. They have minimal mechanism to control access to address space, interrupts and processor time.

## seL4

seL4 is an microkernel-based operative system aiming to achieve the highest guarantees of security without compromising on performance. To date, it is the only formally verified operative system in production.  

When seL4 boots it gives the root task (initial user-level process) all the physical resources left. This task then can set up other task and grants rights. Access rights are based on capabilities (unforgeable tokens representing privileges).

seL4 runs all device drivers in user mode. The exceptions are a timer driver, which seL4 needs to enforce time-slice preemption, and the interrupt controller access to which seL4 mediates. When compiled with debugging enabled, the kernel also contains a serial driver.

seL4 provides the mechanisms for user-mode device drivers, especially the ability to map device memory to drivers and forward IRQs as (asynchronous) messages.

The formal verification of seL4 on the ARM platform assumes that the MMU has complete control over memory, which means the proof assumes that DMA is off. DMA devices can in theory overwrite any memory on the machine, including kernel data and code.

You can still use DMA devices safely, but you have to separately assure that they are well-behaved, that is, that they don’t overwrite kernel code or data structures. drivers and hardware for DMA devices need to be trusted.

The unverified x86 version of seL4 supports VT-d extensions on the experimental branch. The VT-d extensions allow the kernel to restrict DMA and thereby enable DMA devices with untrusted user-level drivers. There is unverified support for the SystemMMU on multiple ARM boards.

On x86, seL4 can be configured to support multiple CPUs. Current multicore support is through a multikernel configuration where each booted CPU is given a portion of available memory. Cores can then communicate via limited shared memory and kernel supported IPIs. 

seL4 can be used as an hypervisor.
