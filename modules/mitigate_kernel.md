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
