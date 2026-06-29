# AWS Security IAM Lab

## 📌 1. Project Objective
The objective of this lab was to gain hands-on experience with AWS Identity and Access Management (IAM) by implementing Role-Based Access Control (RBAC) and the Principle of Least Privilege (PoLP). 
The lab simulates a small organization's AWS environment by Creating multiple departments, assigning only the permissions required for each role, and validating access controls through permission testing using different IAM users.


The lab focused on the following objectives :
- Create an AWS account
- Create IAM groups and attaching AWS managed policies
- Create IAM users
- Assigning IAM users to groups
- Enabling Multi-Factor Authentication (MFA) for privileged accounts
- Verifying permissions by logging in each IAM users
- Demonstrating Principle of Least Privilege (PoLP)
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
During AWS account registration,the payment verification transaction was declined by the bank because the authorization amount was below the bank's minimum transaction threshold.

* **The Implementation Workflow:**

After the AWS account was successfully created and verified, the following steps were performed to configure AWS IAM and implement Role-Based Access Control (RBAC).
  1.  Sign in to console as Root User

     <img src="Images/sudo-apt-update-upgrade.png" width="700">
     
  2. Enable Multi-Factor Authentication for the root account
     
       <img src="Images/hostnamectl.png" width="700">

   3. After being in Console home, search for IAM dashboard
  
   4. Created Adminstrators group with permission policy : `AdministratorAccess`
   5. Created Admin IAM user and put Admin user to Administrators group
   6. signed out from root user and login as Admin 
   7. enable Multi-Factor Authentication (MFA) for admin user
   8. Search IAM dashboard via Admin user
   9. Create groups for each department :
       - HR_Group included permission policy `AmazonS3ReadOnly`
       - Finance_Group included permission policy `AmazonS3ReadOnly`
       - IT_Group included permission policy `AmazonEC2FullAccess`
         
   10. Create users of each department andp put all users to each group:
       | User | Group | Policy | Directly Attached Policies |
       | :--- | :--- | :--- | :--- |
       | Admin | Administrators | AdministratorAccess | IAMUserChangePassword|
       | Alice_HR | HR_Group | AmazonS3ReadOnlyAccess | IAMUserChangePassword|
       | Bobby_IT | IT_Group | AmazonEC2FullAccess | IAMUserChangePassword|
       | Conan_Finance| Finance_Group| AmazonS3ReadOnlyAccess | IAMUserChangePassword|
      **Note:** Department-specific permissions were assigned through IAM   groups to follow Role-Based Access Control (RBAC). The `IAMUserChangePassword` policy was attached directly to each IAM user to allow users to change their own passwords after their initial login.
         
   ### 12. Permission Validation (Principle of Least Privilege)

#### Login as `Alice_HR`

**Assigned Permissions**
- Group: `HR_Group`
- Group Policy: `AmazonS3ReadOnlyAccess`
- User Policy: `IAMUserChangePassword`

**Validation Tests**

**1. AWS IAM**
- Attempted to access the IAM Dashboard.
- **Result:** Access denied.
- **Reason:** The user has no IAM permissions other than the ability to change their own password.

**2. Amazon S3**
- Attempted to access the Amazon S3 console and view the General Purpose Buckets page.
- **Result:** Access granted.
- **Reason:** The `AmazonS3ReadOnlyAccess` policy allows read-only access to Amazon S3 resources.

**3. Amazon EC2**
- Attempted to access the EC2 Instances page.
- **Result:** Access denied (`ec2:DescribeInstances`).
- **Reason:** The HR group was not granted any Amazon EC2 permissions.

**Conclusion**
- The permission tests confirmed that the HR user could only access the AWS services required for their role. Unauthorized access to IAM and EC2 was successfully blocked, demonstrating the implementation of the **Principle of Least Privilege (PoLP)**.

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
