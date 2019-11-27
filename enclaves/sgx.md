# Intel SGX

Secure Guard Extensions (SGX ) is a set of technologies implemented by Intel into the last 3 generations of Intel Core Processor architecture to provide developers with a flexible Trusted Execution Environment platform. SGX is composed of new instructions in the x86 ISA, new hardware functions and an attestation service. The objective of Intel SGX is to protect the confidentiality and integrity of the application, called enclave, running within the TEE.

SGX can defend enclaves from attackers who have gained controls both of the operative system and of the physical machine. To defend against cold-boot attacks, SGX uses DRAM memory encryption, physically implemented in the MMU: virtual pages belonging to the enclave are encrypted as soon as they leave the chip package.

The untrusted operative system can disrupt the availability of the enclave, as it still controls the scheduling of threads and processes, but confidentiality and integrity of the enclave application are protected. 

In its current form, SGX cannot protect confidentiality from covert side-channel attacks. Some of these attacks can be prevented by the application developer by using the right algorithm, i.e such as constant-timing cryptographic algorithms, and software implementation. 

Confidentality and integrity are protected if the software developers  correctly implement the software running in the enclaves. 

### Remote Attestation

It can be proved that a given piece of software is being run in an enclave by performing remote attestation. The running enclave can ask to the architectural enclave, which it is launched by the SGX driver at system boot, to provide an enclave quote, which includes a hash of the enclave code, also called measurement. 

The quote is signed by a key derived from a key embeeded in the hardware with a function which takes in consideration, among other things, the version of the SGX firmware running on the platform. The signature protocol is called EPID, and it enables to have many private keys from a single group public key: in this way Intel is prevended from linking a quote to the unique Intel processor running the workload. The signature within the quote is encrypted to an Intel-owned RSA key. 
To verify the authenticity of a quote, Intel has a permissioned service called Intel Attestation Service, to which the quote can be send over an encrypted connection. A valid quote will receive a positive response. 

The key embeeded in the hardware is provisioned in the manufactoring facility.
