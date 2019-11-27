# Intel SGX

Secure Guard Extensions (SGX ) is a group of technology implemented by Intel into the last 3 generation of Core architecture to provide developes with flexible Trusted Execution Environment platform. SGX is composed of new instructions, new hardware functions and an attestation service.

The threat model is an attacker with both control of the operative system and of the physical machine, up to non-invasive attack. Against such adversary, Intel SGX uses DRAM memory encryption,physically implemented in the MMU: virtual pages belonging to the enclave are encrypted as soon as they leave the chip package. The untrusted operative system can disrupt the availability of the enclave, as it still control scheduling, but confidentiality and integrity are protected. In its current form, SGX cannot protect from covert channel attack: it is up to the developer to protect from timing attacks, and it is feasable using the right algorithm and software implementation, but protecting against cache attacks is more difficult. 

Confidentality and integrity are protected if the software developers  correctly implement the software running in the enclaves. 

### Remote Attestation

It can be proved that a given piece of software is being run in an enclave by performing remote attestation. The running enclave can ask to the architectural enclave, which it is launched by the driver at system boot, to provide an enclave quote, which includes a hash of the enclave code, also called measurement. 

The quote is signed by a key derived by a key embeeded in the hardware with a function which takes in consideration the SGX firmware version running on the platform. 

To protect privacy of enclaves users, Intel has used a special cryptographic protocol called EPID, which enables to have many private keys from a single group public key. 

The provisioning of such key is made in the factory. Enclave quotes can only be verified using the Intel Attestation Service, as the signature of the quote is encrypted with a RSA key owned by Intel. 
