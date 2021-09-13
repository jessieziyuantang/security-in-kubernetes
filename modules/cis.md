# Run CIS-CAT benchmark in your node


> [CIS-CAT Lite](https://learn.cisecurity.org/cis-cat-lite)is the free assessment tool developed by the CIS (Center for Internet Security, Inc.). CIS-CAT Lite helps users implement secure configurations for multiple technologies. With unlimited scans available via CIS-CAT Lite, your organization can download and start implementing CIS Benchmarks in minutes.

1. Fill out the form and you will receive a CIS-CAT benchmark download link in your email. Inside the email you will find the option to download version 3 or version.

2. Download it to our exercise node. Hover with the mouse over versionv4and copy the link. Then log into your exercise node and usethewgetcommand to retrieve the file.

   <img src="../img/cis_download.png" width="80%" height="80%"/>



   ```bash

   ##Run a wget cli in your node, similar as below; 
   wget https://learn.cisecurity.org/e/799323/l-799323-2019-11-15-3v7x/7xxxxxxxx
   mv 7xxxxxxx CIS-Cat.zip
   ```

3. Install the 'unzip' command and extract all the files.

   ```bash
   sudo apt-get update ; sudo apt-get install unzip

   sleep 10

   unzip CIS-Cat.zip

   cd Assessor-CLI
   ```

4. The assessment tool requires JAVA. Install the software then set the JAVA_PATH variab

   ```bash
   sudo apt-get install openjdk-11-jdk -y

   sleep 10

   export JAVA_PATH=/usr/lib/jvm/java-11-openjdk-amd64/bin/
   ```


5. Run the program again, this time pass the-ioption. When theSelect Contentprompt appears enter the number5 to assess Ubuntu.
   ```bash
   sudo bash Assessor-CLI.sh -i
   ```

6. Copy the report(listed HTML file) to your local machine. Similar output is :

   ```bash
   - Reports saving to /home/student/Assessor-CLI/reports38-- single-CIS_Ubuntu_Linux_18.04_LTS_Benchmark-20201107T041401Z.html39Assessment Complete for Checklist: CIS Ubuntu Linux 18.04 LTS Benchmark
   ```
