# Quickassist

Openwrt 19.07.2 packages for Intel Quick Assist on c2xxx SoCs

# Functionality

* This repository contains kernel drivers and firmware for the Intel QAT v1.5 drivers for Rangeley SoCs (C2358, C2558, C2758) and the Intel openssl QAT engine, which allows hardware accelerated openssl
* The drivers are patched with an Intel-supplied unpublished patch to allow compilation against 4.x kernels
* The QAT 1.7 usdm_drv contiguous pinned memory driver is backported from QAT 1.7. This allows use of a production quality contiguous pinned memory driver rather than the example driver shipped with the QAT engine
* Configuration files and services to load and configure the drivers


# Requirements

* Openwrt 19.0.7 SNAPSHOT (openssl 1.1.1e is required due to broken engine related behaviour in 1.1.1d)
* Kernel configuration symbols: CONFIG_HUGETLBFS=y and CONFIG_HUGETLB_PAGE=y
* glibc

# Notes

This feed will support QAT accelerated openvpn if openvpn is compiled with openssl (invoked with --engine qat)
Please refer to https://github.com/intel/QAT_Engine for information on how to use and test 

I have disabled accelerated symmetric cipher acceleration in the openssl QAT engine. The QAT engine does not offload less than 2KB to the hardware as the time to transfer over the pci bus makes it less efficient than using AESNI. In real terms, using QAT offloading for anything less than an 8KB buffer results in slower performance than using AESNI. Additionally, the speed of the software implementation suffer considerably when not offloading due to engine related polling, resulting in up to a 40% performance hit for small buffer sized, software-based encryption/decryption. Since most use cases of this will be for network-acceleration such as Openvpn, which uses small buffers, it is better to completely disable symmetric cipher acceleration and retain the higher performance AESNI. Enabling symmetric ciphers in the openssl QAT engine would only make sense if using it to encrypt large blocks, for example some type of file-related encryption. You can always turn it back on by removing the relevant switch in the Makefile

