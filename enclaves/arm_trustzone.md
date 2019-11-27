# ARM TrustZone

Modern mobile device have strong requirements for security but at the same time a very large attack surface. Features such as bootloader integrity verification, full-disk encryption, biometric authentication. DRM and secure payments require storing and processing sensitive data.  To protect this data, ARM designed in their processor architecture a trusted execution enviroment solution called ARM TrustZone.  This architectural design implements two execution context for the processor: a Normal World and a Secure World.  The latter is protected from the former using a series of hardware and software enforced policies. Normal world runs what we know  

Within those execution enviroments, separation of privilege, i.e rings, still exist. In Normal World, EL0 is reserved for user application, EL1 for the OS, EL2 for the hypervisor if present. In the Secure World, S-EL0 is reserved for trusted apps or trustlets while S-EL1 is reversed for the trusted OS.

The highest privileged level EL3 is the level of the Secure Monitor, which handles the switch between normal and secure world. 

Specific kernel drivers, libraries and daemon running within normal world can be used by user applications or by the kernel to safely call trusted applications. 

The security model of the trusted execution enviroment relies on the assumption that the trusted application themselves and the trusted OS are secure. Unfortunately, this is often not the case. There is widespread use of memory-unsafe languages and the TEE kernels do not implement mitigations such as ASLR against common attacks. 

There are few families of TEE OS widely used:

* Trustonic TEE OS/ Kinibi / T-base: same thing different names

* Qualcomm QSEE

* Samsung TEEGris 

* Android Trusty

* OP-TEE 

**References:**

* [ARM TrustZone Firmware]([https://www.linaro.org/app/resources/Connect%20Events/Trusted_Firmware_Deep_Dive_v1.0_.pdf](https://www.linaro.org/app/resources/Connect%20Events/Trusted_Firmware_Deep_Dive_v1.0_.pdf)
* [Unbox Your Phone]([https://medium.com/taszksec/unbox-your-phone-part-i-331bbf44c30c](https://medium.com/taszksec/unbox-your-phone-part-i-331bbf44c30c)
* [Trust Issues]([https://googleprojectzero.blogspot.com/2017/07/trust-issues-exploiting-trustzone-tees.html](https://googleprojectzero.blogspot.com/2017/07/trust-issues-exploiting-trustzone-tees.html)
  
  


