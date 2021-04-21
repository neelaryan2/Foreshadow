# Foreshadow Attack PoC Experiment 

Kindly note that this is just a stripped (and slightly edited) version of SGX-STEP for the Foreshadow PoC.\
All the PoC development credits go to [SGX-STEP](https://github.com/jovanbulck/sgx-step).

## CS 741 Course Project

### Team Members  -

Neel Aryan Gupta - 180050067\
Pulkit Agrawal - 180050081\
Tathagat Verma - 180050111

## Building and running 

NOTE : The code resides inside the src folder (inside src.zip) and these steps are to be executed inside src folder.

### 0. System requirements

SGX-Step requires an [SGX-capable](https://github.com/ayeks/SGX-hardware) Intel processor, and an off-the-shelf Linux kernel. 
Pass the desired boot parameters to the kernel as follows:

```bash
$ sudo vim /etc/default/grub
  # Add the following line: 
  GRUB_CMDLINE_LINUX_DEFAULT="nox2apic iomem=relaxed no_timer_check nosmep nosmap clearcpuid=514 isolcpus=1 nmi_watchdog=0 dis_ucode_ldr processor.max_cstate=1 intel_idle.max_cstate=0 noibrs noibpb nopti nospectre_v2 nospectre_v1 l1tf=off nospec_store_bypass_disable no_stf_barrier mds=off mitigations=off nokaslr kvm-intel.vmentry_l1d_flush=never"
$ sudo update-grub && reboot
```
Finally, in order to reproduce our experimental results, make sure to enable Intel SGX in the BIOS configuration.

### 1. Build and load `/dev/sgx-step`

To build and load the `/dev/sgx-step` driver, execute:

```bash
$ cd kernel/
$ chmod +x install_SGX_driver.sh
$ sudo ./install_SGX_driver.sh 
$ make clean load
```

### 2. Patch and install SGX SDK

To enable easy registration of a custom Asynchronous Exit Pointer (AEP) stub, we modified the untrusted runtime of the official Intel SGX SDK. 

```bash
$ cd sdk/intel-sdk/
$ chmod +x install_SGX_SDK.sh
$ chmod +x patch_sdk.sh
$ sudo ./install_SGX_SDK.sh
$ source /opt/intel/sgxsdk/environment # add to ~/.bashrc to preserve across terminal sessions
$ sudo service aesmd status            # stop/start aesmd service if needed
```

The above install scripts are tested on Ubuntu 20.04 LTS.

For other GNU/Linux distributions, please follow the instructions in the
[linux-sgx](https://github.com/01org/linux-sgx) project to build and install
the Intel SGX SDK and PSW packages. You will also need to build and load an
(unmodified) [linux-sgx-driver](https://github.com/01org/linux-sgx-driver) SGX
kernel module in order to use SGX-Step.


### 3. Running the PoC
To run the PoC for Foreshadow, do the following:
```bash
$ cd foreshadow
$ make run
```

You can run the make command multiple times to observe various runs of the attack.
