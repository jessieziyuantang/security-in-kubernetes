# Run trivy image scanning in your node


> [CIS-CAT Lite](https://learn.cisecurity.org/cis-cat-lite)is the free assessment tool developed by the CIS (Center for Internet Security, Inc.). CIS-CAT Lite helps users implement secure configurations for multiple technologies. With unlimited scans available via CIS-CAT Lite, your organization can download and start implementing CIS Benchmarks in minutes.


1.  Install dependency and other software:

   ```bash
   sudo apt-get install wget apt-transport-https gnupg lsb-release -y
   ```
    
2.  Pull down the public key from Aquasecurity:

   ```bash
   wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
   ```
    
3.  Update the local repo to get access to the packages.  

   ```bash
   echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | \sudo tee -a /etc/apt/sources.list.d/trivy.list
   ```
    
4. Update the cache from the repository:

   ```bash
   sudo apt-get update
   ```
    
5.  Install theTrivysoftware:

   ```bash
   sudo apt-get install trivy -y
   ```
    
6.  Test against a known flawed image. 

   ```bash
   trivy image knqyf263/vuln-image:1.2.3
   ```
    
7.  Now check an often used package.

   ```bash
   trivy image nginx
   ```
    

