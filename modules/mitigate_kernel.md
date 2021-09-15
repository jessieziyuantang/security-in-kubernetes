# Info about how to mitigate your kernel security concerns


> [CIS-CAT Lite](https://learn.cisecurity.org/cis-cat-lite)is the free assessment tool developed by the CIS (Center for Internet Security, Inc.). CIS-CAT Lite helps users implement secure configurations for multiple technologies. With unlimited scans available via CIS-CAT Lite, your organization can download and start implementing CIS Benchmarks in minutes.

1. Blocking Dynamic Module for against rootkits

2. kernel.modules_disabled -- writing as "1" after the system has booted

3. Adaptive Stack Layout Randomization

This is set with kernel.randomize_va_space sysctl:

0: Disabled
1: Randomized stack, VDSO (Virtual Dynamic Shared Object), shared memory addresses
2: Randomized stack, VDSO, shared memory and data addresses.
This was adapted from the external PaX/grsecurity projects, along with several other software-based hardening features.

4. No eXecute

Look for the nx flag in /proc/cpuinfo. It may be enabled/disabled in BIOS.

5. ExecShield

No eXecute (NX) ancestor
Implemented in software. No hardware support is needed. Has more overhead than NX.
It is not always compiled in recent kernels
It is enabled as a sysctl:
# echo 0 > /proc/sys/kernel/exec-shield

6. Vt-d Virtualization
Intel’s VT-d is a technology that allows virtual machines to directly access the hardware. 
hardware feature to be enabled in BIOS - look for vmx or svm in /proc/cpuinfo.

7. Trusted Platform Module (TPM)
TPM is a hardware feature that provides a way to store checksums:

8. Trusted Executed Technology (TXT)
TXT is used to isolate the memory used by guest virtual machines. A guest VM could have a malicious kernel that tried to access memory owned by a host or another guest. TXT is an efficient, hardware-based method for preventing this from happening.

9. Integrity Management
The Integrity Measurement Architecture (IMA) component performs runtime integrity measurements of files using cryptographic hashes, comparing them with a list of valid hashes.
IMA is a Linux-trusted computing implementation that:
Detects if files have been accidentally or maliciously altered
Compares a file's checksum against a good value
Enforces local file integrity.

IMA was first introduced in kernel 2.6.30:

Look for CONFIG_IMA in /boot/config*
Boot with ima_tcb and ima=on kernel parameters.

10. Integrity Management with dm-verity
Check for CONFIG_DM_VERITY under /boot/config*

11. Linux Security Modules (LSM)
The Linux Security Modules (LSM) API implements hooks at all security-critical points within the kernel. A user of the framework (an LSM) can register with the API and receive callbacks from these hooks. All security-relevant information is safely passed to the LSM, avoiding race conditions, and the LSM may deny the operation. 
The LSM API allows different security modules to be plugged into the kernel-typically access control frameworks. To ensure compatibility with existing applications, the LSM hooks are placed so that the UNIX DAC checks are performed first, and only if they succeed, is LSM code invoked.

12. Seccomp
Secure computing mode (seccomp) is a mechanism which restricts access to system calls by processes. The idea is to reduce the attack surface of the kernel by preventing applications from entering system calls they do not need. 