# Trusted Execution Environments

The complexity of modern software and especially of modern operative systems, guarantees that they are not able to guarantee security, confidentiality and integrity to the applications running on top of them. As applications and software has become pervasive in our daily lives, handling and storing more sensitive information, from biometrics, to medical and financial data, there is the need for secure solution which can be deployed at scale. The trend is set, and security requirements are only going to increase going forward. 

In the last decade, this issue has lead to the approach of creating within general purpose hardware solution, called trusted execution enviroments (TEE) or enclaves. There are different ways of implementing an enclave from an architectural point of view:

* It can be implemented in a complete separate package (OpenTitan)

* It can be implemented within the same system-on-chip package but on a different physical chip (Apple Secure Enclave Coprocessor)

* It can be implemented on the same chip with special extensions to the ISA (Intel SGX, ARM Trustzone)

The enclave runs sensitive workloads protecting the confidentiality and integrity of the computation and exposes a minimal comunication API to the host operative system. The host operative system is considered untrusted, and depending from the architectural implementation, it can't either access enclave memory due to strong software restriction (ARM Trustzone) or because enclave memory is  encrypted at the hardware level with a key not accessable by the host operative system (Intel SGX). 

The security of the enclave and its workload is also responsability of the application itself: enclaves do not automatically protect from classical software bugs such as buffer overflow, but only from the untrusted operative system. Giving that the scheduling is still untrusted, enclaves cannot enforce availability. 

In order for the such a system to be useful, it is required a proof that a given piece of software is running within an enclave. This is called remote attestation, and it is a process happening within the enclave architecture which generate a cryptographically signed document binding the program and various other informations. The cryptographic key signing the document is either a key embedded in the hardware or it derived by that key, depending from the TEE implementation: such key is called a root of trust and it should be itself signed by a known key of the entitity building the enclave. 

Until recently, TEEs have been opaque piece of hardware and software, implemented by silicon vendors with proprietary, closed source software. This create questions on the effective guarantees of confidentiality, integrity that these platforms can actually provide, both from the actual security of the implementation, which cannot be openly audited by the security community, and from a trust point of view.  

Especially in a world where the silicon industry is an oligopoly of very large corporation with ties with national governments and the chip manufactoring is done by third-parties spread around the globe, it should be at least entertained the thought that , either with the complicity of the company or due to infiltration, these types of technology can have devastating backdoors. This thought has led efforts for far more open trusted execution enviroment architecture and hardware implementation, such as RISC-V Keystone and lowRISC OpenTitan. 

## Enclave Software Developing Framework

* Google Asylo

* MesaTEE 

* Graphene SGX: runs unmodified binary within SGX

* SCONE: run containers within SGX 

* Fortanix Enclave SDK 

* Intel SGX SDK

* Baidu Rust SGX

### ARM TrustZone OS

* Samsung TEEGris

* Qualcomm 

* Trusty TEE

* Trustonic OS

* Ledger BOLOS

## Enclave Building Framework:

* Keystone Enclave:  opensource, RISC-V architecture
* Intel SGX: proprietary,  x86 arch
* ARM TrustZone:  proprietary, ARM arch
