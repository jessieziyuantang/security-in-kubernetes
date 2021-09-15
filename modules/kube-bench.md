# Run kube benchmark in your cluster


> 

1. Download the Aquasecuritykube-benchbinary.   

   ```bash
   curl -L https://github.com/aquasecurity/kube-bench/releases/download/v0.3.1/kube-bench_0.3.1_linux_amd64.deb \
   -o kube-bench_0.3.1_linux_amd64.deb
   ```

2. Install and verify the binary is in your search path.

   ```bash
   sudo apt install ./kube-bench_0.3.1_linux_amd64.deb

   sleep 10

   which kube-bench

   ```

3. Run kube-bench in your master node

   ```bash
   sudo kube-bench
   ```
   
   Similar output is:
   ```bash
   == Summary ==
   0 checks PASS
   0 checks FAIL
   24 checks WARN
   0 checks INFO
   ```