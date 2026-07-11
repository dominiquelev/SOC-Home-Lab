# AWS IAM Security Monitoring

## 📌 1. Project Objective
The objective of this lab was to gain hands-on experience with AWS CloudTrail by implementing activity monitoring in a simulated small company environment.

This lab simulates a company's AWS environment by monitoring management events across multiple AWS Regions, analyzing CloudTrail event history, and configuring CloudWatch alarms with Amazon SNS email notifications for security monitoring.

The lab focused on the following objectives:
* Configuring AWS Identity and Access Management (IAM) users, groups, and permissions based on the Principle of Least Privilege.
* Creating a multi-Region AWS CloudTrail trail to capture management events.
* Creating an Amazon S3 bucket to securely store CloudTrail logs.
* Generating IAM activities (such as creating and deleting users) to produce audit events.
* Validating IAM permissions and analyzing recorded events using CloudTrail Event History.
* Integrating AWS CloudTrail with Amazon CloudWatch Logs.
* Creating Amazon CloudWatch metric filters to detect IAM **CreateUser** events.
* Creating Amazon CloudWatch alarms to monitor security events.
* Configuring Amazon SNS to send real-time email notifications when an alarm is triggered.
* Verifying the end-to-end monitoring workflow by generating test events and confirming that alerts were successfully delivered.

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
  - Billing and Cost Management (Permission Validation)
  - AWS CloudTrail (Recording and Auditing AWS management events)
  - AWS CloudWatch (Log monitoring, metric filters, and alarms)
  - Amazon Simple Notification Service (Amazon SNS) 
* **Security Concepts Implemented:**
  - Multi-factor Authentication (MFA)
  - Role-Based Access Control (RBAC)
  - Principle of Least Privilege (PoLP)
  - IAM Groups
  - IAM Users
  - AWS Managed Policies
  - Security Monitoring
  - Audit Logging
  - Event-Driven Alerting

---

## ⚠️ 3. Engineering Challenges & Troubleshooting

### Incident / Roadblock: 
During the creation of the Amazon CloudWatch metric filter, the initial JSON filter pattern:
`{ $.eventName = "CreateUser" }` 
did not match the sample CloudTrail log events when tested using the **Test Pattern** feature.

To verify the monitoring workflow, a simpler text-based filter `"CreateUser"`was used instead. This filter successfully matched the CloudTrail log events containing the **CreateUser** API call, allowing the metric filter, CloudWatch alarm, and Amazon SNS email notification to function correctly.

This experience highlighted the importance of validating log formats and choosing an appropriate filter pattern based on the available log data.
* **The Implementation Workflow:**

## AWS CloudTrail Configuration
The following steps were performed to configure AWS CloudTrail for recording management events across multiple AWS Regions and storing audit logs in Amazon S3.
  1.  Signed in to the AWS Management Console as the **Admin** user and open the **AWS CloudTrail** console.

      <img src="Images/dashboard-cloudtrail.png" width="700">
      
  2. Created a new CloudTrail trail with the following configuration:
      - Applied to **all AWS Regions (Multi-Region Trail)**
      - Enable **Management Events**
      - Recorded both **Read** and **Write** API activities
        
     <img src="Images/create_trail.png" width="700">
     
  3. Created a new Amazon S3 buckets to store CloudTrail log files.

     <img src="Images/create_trail_log_events.png" width="700">
     
  4. Enabled integration with **Amazon CloudWatch logs** by creating a new log group for monitoring CloudTrail events.
     
      <img src="Images/create_trail2.png" width="700">
  5. Reviewed the configuration and successfully created the CloudTrail trail.
      <img src="Images/create_trail_success.png" width="700">
      
  6. Created a new IAM user named **Intern_HR** and added the user to the **HR_Group** to generate IAM management events for CloudTrail analysis

      <img src="Images/created_intern_HR.png" width="700">
      <img src="Images/intern_HR_in_group.png" width="700">
     
  7. Verified the generated events in **AWS CloudTrail Event History**.

     The following IAM management events were successfully recorded:
     -`CreateUser` : Recorded when the `Intern_HR` IAM user was created.
      <img src="Images/createUserEvent_admin_user.png" width="700">

      The event details were available in the CloudTrail event record (JSON format).
      <img src="Images/eventRecord_create_user_admin.png" width="700">
      
     - `AddUserToGroup` : Recorded when the `Intern_HR` user was added to the **HR_Group**.
      <img src="Images/addUserToGroupDEvent_admin_user.png" width="700">
      
  8. Deleted the **Intern_HR** IAM user to generate addditional audit events.

     <img src="Images/delete_intern_HR.png" width="700">
     
     Before deleting the user, AWS automatically removed the user from the associated IAM group       because an IAM user cannot be deleted while group memberships still exist.

     The following events were recorded in **AWS CloudTrail Event History**:
      - **RemoveUserFromGroup** – Recorded when the user was automatically removed from the   **HR_Group**.
        <img src="Images/removeUserFromGroupEvent_admin_user.png" width="700">
        
     - **DeleteUser** – Recorded when the `Intern_HR` IAM user was successfully deleted.
      <img src="Images/deleteUserEvent_admin_user.png" width="700">

  9.
  10. Created an IAM administrator user (Admin) with console access and added the user to the Administrators group
      <img src="Images/create_admin_user.png" width="700">
 
  11. Signed out of the root account and signed in using the Admin IAM user. 
      <img src="Images/login-console-admin.png" width="700">
  
  12. Enabled Multi-Factor Authentication (MFA) for the Admin user
      <img src="Images/set-up-MFA-admin.png" width="700">
      <img src="Images/set-up-MFA-admin-enable.png" width="700">
  
  13. Opened the IAM dashboard using the Admin account
      <img src="Images/dashboard-IAM-Admin.png" width="700">
  14. Created IAM groups for each department and attached the appropriate AWS managed policies.
      
      **Screenshot - IAM Groups**
      <img src="Images/IAM_Groups.png" width="700">
      
      The following AWS managed policies were attached to each IAM group:
        | IAM Group | Attached Policy | Purpose |
        | :--- | :--- | :--- |
        | Administrators | `AdministratorAccess` | Full administrative access to AWS resources |
        | HR_Group | `AmazonS3ReadOnlyAccess` | Read-only access to Amazon S3 |
        | Finance_Group | `AWSBillingReadOnlyAccess` | Read-only access to Billing and Cost Management |
        | IT_Group | `AmazonEC2FullAccess` | Full access to Amazon EC2 |
   
    
  15. Created IAM users for each department and assigned each user to the appropriate IAM group.
    
      **Screenshot - IAM User**
      <img src="Images/IAM_Users.png" width="700">
    
      | IAM User | IAM Group | Group Policy | Directly Attached Policy |
       | :--- | :--- | :--- | :--- |
       | Admin@IAM | Administrators | AdministratorAccess | IAMUserChangePassword|
       | Alice_HR | HR_Group | AmazonS3ReadOnlyAccess | IAMUserChangePassword|
       | Bobby_IT | IT_Group | AmazonEC2FullAccess | IAMUserChangePassword|
       | Conan_Finance| Finance_Group| AWSBillingReadOnlyAccess | IAMUserChangePassword|

       **Note:** Department-specific permissions were assigned through IAM   groups to follow Role-Based Access Control (RBAC). The `IAMUserChangePassword` policy was attached directly to each IAM user to allow users to change their own passwords after their initial login.
              
16. Enable the **Require password reset at next sign-in** option when creating each IAM user and attached the `IAMUserChangePassword` policy to allow users to change their own passwords.
17. Validated permissions by signing in as each IAM user and testing access to AWS services to verify the implementation of the Principle of Least Privilege (PoLP)

Permission Validation - Alice_HR

Assigned Permissions
- Group: `HR_Group`
- Group Policy: `AmazonS3ReadOnlyAccess`
- Directly Attached Policy: `IAMUserChangePassword`

Validation Tests
1. First sign-in
- Attempted to sign-in as the `Alice_HR` IAM user. 
- The user was prompted to reset the password before accessing the AWS management Console.
- Result: Password reset required
- Reason: The **Require password reset at next sign-in** option was enabled when the IAM user was created.
  <img src="Images/user-HR_required_pass_reset_enable.png" width="700">
  
2. AWS IAM
- Attempted to access the IAM Dashboard.
- Result: Access denied.
- Reason: The user was not granted IAM administrative permissions. The only IAM permission available is the ability to change their own password the `IAMUserChangePassword`.
  <img src="Images/user_HR-dashboard-IAM-access-denied.png" width="700">
  
3. Amazon S3
- Attempted to access the Amazon S3 console and view the General Purpose Buckets page.
- **Result:** Access granted. The S3 console loaded successfully, although no buckets were available in the AWS account
- **Reason:** The `AmazonS3ReadOnlyAccess` policy allows read-only access to Amazon S3 resources.
  <img src="Images/user_HR-enable-access-s3-bucket-list.png" width="700">
 
4. Amazon EC2
- Attempted to access the EC2 Instances page.
- **Result:** Access denied (`ec2:DescribeInstances`).
- **Reason:** The HR group was not granted any Amazon EC2 permissions.
  <img src="Images/user_HR-EC2-instance-access-denied.png" width="700">

Conclusion
-The HR user was required to reset the password during the first sign-in and successfully accessed Amazon S3, while access to AWS IAM and Amazon EC2 was denied, confirming that the HR role follows the **Principle of Least Privilege (PoLP)**.

Permission Validation - Bobby_IT
Assigned Permissions
- Group: `IT_Group`
- Group Policy: `AmazonEC2FullAccess`
- Directly Attached Policy: `IAMUserChangePassword`

Validation Tests
1. First sign-in
- Attempted to sign-in as the `Bobby_IT` IAM user. 
- The user was prompted to reset the password before accessing the AWS management Console.
- Result: Password reset required
- Reason: The **Require password reset at next sign-in** option was enabled when the IAM user was created.
  <img src="Images/user-IT_required_pass_reset_enable.png" width="700">

2. AWS IAM
- Attempted to access the IAM Dashboard.
- Result: Access denied.
- Reason: The user was not granted IAM administrative permissions. The only IAM permission available is the ability to change their own password the `IAMUserChangePassword`.
  <img src="Images/user_IT-dashboard-IAM-access-denied.png" width="700">

3. Amazon S3
- Attempted to access the Amazon S3 console and view the General Purpose Buckets page.
- **Result:** Access denied.
- **Reason:** The IT Group was not granted any Amazon S3 permissions
  <img src="Images/user_IT-denied-access-s3.png" width="700">

4. Amazon EC2
- Attempted to access the EC2 Instances page.
- **Result:** Access granted 
- **Reason:** The `AmazonEC2FullAccess` policy allows full access to Amazon EC2 resources
  <img src="Images/user_IT-EC2-instance-access-enable.png" width="700">
  
Conclusion
- The IT user was required to reset the password during the first sign-in and successfully accessed Amazon EC2 while access to IAM and Amazon S3 remained restricted, validating the assigned role permissions.

Permission Validation - Conan_Finance

Assigned Permissions
- Group: `Finance_Group`
- Group Policy: `AWSBillingReadOnlyAccess`
- Directly Attached Policy: `IAMUserChangePassword`

Validation Tests
1. First sign-in
- Attempted to sign-in as the `Conan_Finance` IAM user. 
- The user was prompted to reset the password before accessing the AWS management Console.
- Result: Password reset required
- Reason: The **Require password reset at next sign-in** option was enabled when the IAM user was created.
  <img src="Images/user-Finance_required_pass_reset_enable.png" width="700">
  
2. AWS IAM
- Attempted to access the IAM Dashboard.
- Result: Access denied.
- Reason: The user was not granted IAM administrative permissions. The only IAM permission available is the ability to change their own password the `IAMUserChangePassword`.
  <img src="Images/user_Finance-dashboard-IAM-access-denied.png" width="700">

3. Amazon S3
- Attempted to access the Amazon S3 console and view the General Purpose Buckets page.
- **Result:** Access denied.
- **Reason:** The Finance group was not granted any Amazon S3 permissions
  <img src="Images/user_Finance-denied-access-s3.png" width="700">

4. Amazon EC2
- Attempted to access the EC2 Instances page.
- **Result:** Access denied (`ec2:DescribeInstances`).
- **Reason:** The Finance group was not granted any Amazon EC2 permissions.
  <img src="Images/user_Finance-EC2-instance-access-denied.png" width="700">

5. Billing and Cost Management
- Attempted to access the billing page.
- **Result:** Access Granted.
- **Reason:** The `AWSBillingReadOnlyAccess` policy allows read-only access to billing information, payment history, credits, and cost analysis without allowing billing modifications.
  <img src="Images/user_Finance-enable-access-billing-and-cost-management.png" width="700">  
  
Conclusion:
- The Finance user was required to reset the password during the first sign-in and successfully accessed the AWS Billing and Cost Management console while access to AWS IAM,Billing and Cost Management, Amazon S3, and Amazon EC2 remained restricted. This demonstrates the implementation of the Principle of Least Privilege (PoLP) by granting Finance personnel access only to billing-related information.

**Overall Permission Validation Summary**
- Permission validation was successfully completed for all IAM users. Each user was granted access only to the AWS services required for their assigned role while unauthorized access attempts were denied. These results confirm the successful implementation of Role-Based Access Control (RBAC) and the Principle of Least Privilege (PoLP) using AWS Identity and Access Management (IAM).

| IAM User | IAM | Amazon S3 | Amazon EC2 | Billing & Cost Management | Status|
| :--- | :--- | :--- | :--- |  :--- | :--- |
|Admin |✅|✅|✅|❌| ✅Pass |
|Alice_HR |❌|✅ Read-only|❌|❌| ✅Pass |
|Bobby_IT|❌|❌|✅ Full Access|❌| ✅Pass |
|Conan_Finance|❌|❌|❌| ✅ Read-only| ✅Pass |
---

## 📊 4. Practical Execution & Findings

* **Activity Executed:**
  - Created an AWS account and signed in as the Root User.
  - Enabled Multi-Factor Authentication (MFA) for both the Root User and the Admin IAM user.
  - Accessed the AWS IAM Dashboard.
  - Created IAM groups and attached the appropriate AWS managed policies :
      + `AdministratorAccess` -> Administrators
      + `AmazonS3ReadOnlyAccess` -> HR Group 
      + `AmazonEC2FullAccess` -> IT group
      + `AWSBillingReadOnlyAccess` -> Finance Group 
  - Created IAM users for each department
  - Attached the `IAMUserChangePassword` policy directly to each IAM user
  - Assigned each IAM user to the appropriate IAM group.
  - Validated permissions by signing in as each IAM user and testing access to AWS services.
  - Confirmed the successful implementation of Role-Based Access Control (RBAC) and the Principle of Least Privilege (PoLP).
    
* **Key Observations:**
  - IAM groups simplify permission management by allowing permissions to be assigned to groups instead of individual users.
  - The Root User should only be used for initial account configuration and emergency administrative tasks.
  - Multi-Factor Authentication (MFA) provides an additional layer of security for privileged accounts.
  - AWS managed policies enable quick implementation of common permission sets for different job roles.
  - Permission validation confirmed that users could access only the AWS services required for their assigned roles.
  - AWS returned Access Denied messages for unauthorized actions, confirming that the Principle of Least Privilege (PoLP) was correctly enforced.
  - Testing IAM permissions using multiple user accounts is an effective way to verify Role-Based Access Control (RBAC) configurations.
---

## 🚀 5. Key Takeaways & Career Alignment

* **Skill Demonstrated:**
  - AWS Identity and Access Management (IAM)
  - Role-Based Access Control (RBAC)
  - Principle of Least Privilege (PoLP)
  - Multi-Factor Authentication (MFA)
  - IAM Users and Groups
  - AWS Managed Policies
  - Permission Validation
  - Cloud Security Fundamentals
    
* **Next Steps:**
  - Configure AWS CloudTrail to monitor IAM activities.
  - Generate IAM login and permission events for audit analysis.
  - Analyze CloudTrail logs to understand user authentication and authorization events.
  - Implement Amazon CloudWatch monitoring for AWS account activity.
  - Replace AWS managed policies with custom IAM policies to provide more granular access control.
    
## 🛠 Skills Practiced
  - Amazon Web Services (AWS)
  - AWS Identity and Access Management (IAM)
  - Cloud Security
  - Role-Based Access Control (RBAC)
  - Principle of Least Privilege (PoLP)
  - Multi-Factor Authentication (MFA)
  - AWS Management Console
  - Security Validation
  - Permission Testing

### Career Alignment
  This project strengthened my understanding of AWS Identity and Access Management (IAM) by applying security best practices such as Role-Based Access Control (RBAC), Multi-Factor Authentication (MFA), and the Principle of Least Privilege (PoLP). These are foundational skills for entry-level roles such as Junior Cloud Security Engineer, Cloud Support Associate, and SOC Analyst.

