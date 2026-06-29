# Splunk Installation

## 📌 1. Project Objective
The objective of this lab was to gain hands-on experience with the AWS Identity and Access Management (IAM) that can be used to implement Role-Based Access Control (RBAC) and the Principle of Privilege Least (PoPL). Create different department that were assigned only the permission required their roles, and each IAM user  was tested to verify the access control worked as expected

The lab focused on :
- Create an account of AWS Cloud IAM
- Create groups and apply AWS manage policies
- Create IAM users
- Assign users to groups
- Enable MFA for privilege accounts
- Verify permissions by logging in each users
- Demonstrate Principle of Least Privilege
---

## ⚙️ 2. Lab Specifications & Tools

* **Cloud Platform:** Amazon Web Services (AWS)
* **Management Console:**
  - AWS Management Console
* **Account Type:**
  - AWS Free Plan (with promotional credits)
* **AWS Services Used:**
  - AWS Identity and Access Management (IAM)
  - Amazon EC2 (Permission Validation)
  - Amazon S3 (Permission Validation)
* **Security Concept Implemented:**
  - Multi-factor Authentication (MFA)
  - Role-Based Access Control (RBAC)
  - Principle of Least Privilege (PoLP)
  - IAM Groups
  - IAM Users
  - AWS Managed Policies

---

## ⚠️ 3. Engineering Challenges & Troubleshooting

### Incident / Roadblock: 
During the registration of the AWS account, the online transaction got decline by national bank caused of the nominal of the transaction too small.

* **The Resolution Workflow:**

After the account was verified, the following steps were performed:
  1.  Sign in to console as Root User

     <img src="Images/sudo-apt-update-upgrade.png" width="700">
     
  2. Enable Multi-Factor Authentication for the root account
     
       <img src="Images/hostnamectl.png" width="700">

   3.    
       <img src="Images/download-splunk-wget.png" width="700">

   4. Installed Splunk Enterprise package using dpkg:
     ``` bash
     sudo dpkg -i splunk*.deb
     ```
       <img src="Images/install-splunk-dpkg-splunk*.deb.png" width="700">

   5. Verified Splunk installation path using:
     ```bash
     ls /opt/splunk
     ```
     <img src = "Images/ls-opt-splunk.png" width="700">
  
   6. Started Splunk Enterprise using:
     ```bash
     sudo /opt/splunk/bin/splunk start --accept-license --answer-yes --no-prompt --run-as-root
     ```
      <img src = "Images/force-start-splunk.png" width="700">
   
   7. Accessed the Splunk Web Interface through a web browser :
      ```bash
      http://localhost:8000
      ```
      <img src = "Images/force-start-splunk-2.png" width="700">
       <img src="Images/login-page-splunk.png" width="700">
  
   8. Verified dashboard access by Logged into Splunk successfully:
     
      <img src="Images/dashboard-splunk.png" width="700">
               
   9. Checked service status of Splunk Enterprise using:
      ```bash
      sudo /opt/splunk/bin/splunk status
      ```
      <img src="Images/splunk-status-after-login.png" width="700">

  This confirmed that Splunk Enterprise was running successfully and operational..

### Result:
Splunk Enterprise was successfully installed on the Ubuntu Server virtual machine.

The Splunk service started successfully, the web interface was accessible through port 8000, and service status verification confirmed that Splunk was running correctly.

This environment is now ready for log ingestion, authentication monitoring, and security investigation activities in future projects.

---

## 📊 4. Practical Execution & Findings

* **Activity Executed:**
  - Installed Splunk Enterprise on Ubuntu Server
  - Verified service startup and operation
  - Accessed the Splunk Web Interface
  - Confirmed successful deployment through service status checks
* **Key Observations:**
  - Splunk Enterprise was installed successfully using the Linux terminal.
  - The Splunk Web Interface became accessible on port 8000.
  - Service status verification confirmed that Splunk was operational.
  - The environment was successfully prepared for future log ingestion and monitoring activities.
---

## 🚀 5. Key Takeaways & Career Alignment
* **Conclusion:**
Successfully deployed and configured Splunk Enterprise on an Ubuntu Server virtual machine. Through this lab, I gained hands-on experience with Linux administration, SIEM deployment, service verification, and troubleshooting.

This environment will be used as the foundation for future projects involving log ingestion, authentication monitoring, security investigations, detection engineering, and SOC analyst workflows.

* **L1 SOC Skill Demonstrated:**
  - Linux administration
  - Software installation and configuration
  - Service management
  - Basic SIEM deployment
  - Troubleshooting and problem resolution
    
* **Next Steps:**
  - Ingest Linux authentication logs (auth.log)
  - Ingest system logs (syslog)
  - Perform SSH authentication monitoring
  - Create Splunk alerts and dashboards
  - Conduct basic security investigations
    
## 🛠 Skills Practiced
 - Linux
 - VirtualBox
 - Splunk Enterprise
 - Package Management
 - System Administration
 - Troubleshooting
 - SIEM Fundamentals
