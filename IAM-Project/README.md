# AWS IAM Service Demo - Complete Project Guide

## Project Overview

This project demonstrates the AWS Identity and Access Management (IAM) service and its core functions for securing data through authentication and authorization. The demo consists of 9 practical steps that showcase how organizations manage user access and permissions in AWS.

**Key Concepts:**
- **Authentication**: Verifying who a user is (username and password)
- **Authorization**: Determining what resources an authenticated user can access (permissions and policies)

---

## Step 1: Root User Creates IAM User Account

**Objective**: Initialize IAM user creation for company employees

**Description**: 
The AWS root user (account administrator/developer) creates a new IAM user account for an employee. During this process, the root user generates a temporary random password for the new user. Only the IAM username and the random password are provided to the employee—the root user does not share AWS account credentials.

**Key Points**:
- Root user initiates the account creation process
- System generates a random, secure password
- Employee receives only their username and temporary password
- This is the first step in onboarding new team members

**Screenshot**: ![Step 1 - Root User Creates IAM User Account](./images/Step%201.png)

---

## Step 2: IAM User Logs In to AWS Console

**Objective**: First-time login and account verification

**Description**: 
The newly created IAM user accesses the AWS Management Console. They use the account number (AWS Account ID) along with the username and temporary random password provided by the root user to authenticate.

**Login Process**:
1. Navigate to AWS Management Console login page
2. Enter AWS Account Number/ID
3. Enter IAM Username
4. Enter the temporary password provided
5. Successfully log in to the console

**Key Points**:
- This is the authentication step
- The user must have correct credentials to access the AWS environment
- The temporary password is usually required to be changed on first login

**Screenshot**: ![Step 2 - IAM User Logs In](./images/Step%202.png)

---

## Step 3: IAM User Changes the Temporary Password

**Objective**: Secure the account by replacing the temporary password

**Description**: 
After the first successful login, the IAM user should immediately change the temporary random password to a permanent, secure password of their choice. This ensures only the user knows their password and increases account security.

**Change Password Process**:
1. Log in with temporary credentials
2. Navigate to IAM User Settings or Security Credentials
3. Select "Change Password"
4. Enter the current (temporary) password
5. Enter and confirm the new password
6. Save changes

**Key Points**:
- This is a security best practice
- Users should create strong, unique passwords
- The old temporary password becomes inactive
- Only the user now knows their account password

**Screenshot**: ![Step 3 - Change Password](./images/Step%203.png)

---

## Step 4: IAM User Opens Console with No Permissions

**Objective**: Demonstrate the lack of authorization before policies are attached

**Description**: 
Once the IAM user successfully logs in with their new password, they have access to the AWS console. However, they currently have no permissions to access any AWS services. When they attempt to use any service, they will encounter access denied errors.

**What the User Sees**:
- AWS console interface is accessible
- All AWS services are visible in the menu
- No actual service access or resource creation is possible
- This demonstrates the difference between authentication (being able to log in) and authorization (being able to use services)

**Key Points**:
- The user is authenticated but not authorized
- This is a security feature—users only get access to what they specifically need
- No services work without explicit permissions
- This step reinforces the principle of least privilege

**Screenshot**: ![Step 4 - No Permissions Access](./images/Step%204.png)

---

## Step 5: Attempting to Access a Service Without Permissions (S3 Example)

**Objective**: Demonstrate why permissions are necessary

**Description**: 
When the IAM user attempts to create an S3 bucket or access S3 services without the appropriate permissions, AWS displays a permission denied error. This shows the system's security mechanism preventing unauthorized actions.

**Example Scenario**:
1. IAM user clicks on "S3" service in the console
2. Tries to create a new bucket or list existing buckets
3. AWS returns an access denied error message
4. The error message indicates that the user lacks the necessary permissions

**Error Message Example**: "User is not authorized to perform: s3:ListBucket on resource: arn:aws:s3:::bucket-name"

**Key Points**:
- This demonstrates authorization failure
- The user cannot perform any S3 operations without explicit permissions
- Permission errors protect company data and resources
- This is where the need for policies becomes clear

**Screenshot Placeholder**: [Insert Step 5 image here - showing the access denied error when trying to access S3]

---

## Step 6: Root User Attaches Policies to IAM User

**Objective**: Grant specific permissions to the IAM user

**Description**: 
The root user now navigates to the IAM dashboard and attaches a policy to the user account. In this example, the root user attaches an S3 Full Access policy. Policies are JSON documents that define what actions a user can perform on which resources.

**Process**:
1. Root user opens IAM dashboard
2. Selects the specific IAM user
3. Goes to "Permissions" or "Add permissions"
4. Searches for and selects the desired policy (e.g., "AmazonS3FullAccess")
5. Attaches the policy to the user
6. Changes are applied immediately

**Common Policies**:
- AmazonS3FullAccess - Complete S3 permissions
- AmazonS3ReadOnlyAccess - Read-only S3 permissions
- Other service-specific policies for EC2, RDS, Lambda, etc.

**Key Points**:
- Policies are the authorization mechanism in IAM
- Multiple policies can be attached to one user
- Changes take effect immediately
- Best practice is to use the principle of least privilege (only grant necessary permissions)

**Screenshot Placeholder**: [Insert Step 6 image here - showing the policy attachment process in the IAM console]

---

## Step 7: IAM User Can Now Access S3 Service with Limited Permissions

**Objective**: Demonstrate that the user now has authorization to perform S3 operations

**Description**: 
Now that the S3 Full Access policy has been attached to the IAM user, they can successfully access and manage S3 services. However, their permissions are still limited to S3 only—they cannot access other AWS services.

**What the IAM User Can Do**:
1. Log in to the console
2. Navigate to S3 service
3. Create new S3 buckets
4. Upload and delete objects
5. Manage bucket properties
6. Perform all S3-related operations

**What the IAM User Cannot Do**:
- Access EC2 instances (no permission)
- Access RDS databases (no permission)
- Access other AWS services not covered by their policies
- Modify IAM settings (no permission)

**Key Points**:
- Authorization is now granted for S3 services
- The user has exactly the permissions they need for their role
- This demonstrates the principle of least privilege in action
- Other services remain inaccessible (secure by default)

**Screenshot Placeholder**: [Insert Step 7 image here - showing successful S3 bucket creation or access]

---

## Step 8: Creating User Groups for Efficient Permission Management

**Objective**: Simplify permission management for multiple users with similar roles

**Description**: 
As the organization grows and more employees join, manually assigning permissions to each individual user becomes time-consuming and error-prone. The solution is to create IAM User Groups. A User Group is a collection of users with shared policies and permissions.

**Creating User Groups**:
1. Root user opens the IAM dashboard
2. Selects "User Groups" or "Groups"
3. Creates a new group named "developers" (or other role names like "managers", "interns", etc.)
4. Attaches policies to the group (e.g., S3 Full Access, EC2 Full Access, etc.)
5. All users added to this group automatically inherit these policies

**Benefits of User Groups**:
- Faster onboarding of new team members
- Consistent permissions across similar roles
- Easier permission updates (change group policy once, affects all members)
- Reduced administration time
- Lower risk of configuration errors

**Example Groups**:
- Developer Group - S3, EC2, Lambda access
- Data Analyst Group - S3 read-only, Athena access
- DevOps Group - All service access
- Intern Group - Limited S3 access only

**Key Points**:
- Groups simplify permission management at scale
- Policies attached to groups apply to all group members
- Users can belong to multiple groups
- This is especially valuable in growing organizations

**Screenshot Placeholder**: [Insert Step 8 image here - showing user group creation and policy attachment to the group]

---

## Step 9: Adding New Users to Groups for Immediate Access

**Objective**: Demonstrate rapid onboarding using user groups

**Description**: 
When a new employee joins the organization, instead of individually configuring their permissions, the root user simply creates a new IAM user and adds them to the appropriate existing group. The new user immediately inherits all group permissions.

**Quick Onboarding Process**:
1. Root user creates a new IAM user account
2. Provides username and temporary password to the new employee
3. Root user adds the new user to the existing "developers" group
4. New user logs in and immediately has all necessary permissions
5. No need to manually attach individual policies

**Comparison**:
- Without Groups: Create user → Manually attach 5-10 policies → Test permissions → Fix issues → 20-30 minutes
- With Groups: Create user → Add to group → Done → 2-3 minutes

**Key Points**:
- Groups enable rapid user provisioning
- New users instantly get all necessary permissions
- Reduces onboarding time significantly
- Maintains consistent permission structure across the organization
- Scales efficiently as the organization grows

**Screenshot Placeholder**: [Insert Step 9 image here - showing a new user being added to the developer group]

---

## IAM Components Summary

This project demonstrates the three main IAM components:

### 1. **Users**
- Individual accounts for employees or applications
- Each user has unique credentials (username and password)
- Users are the ones performing actions in AWS
- Example: "john.developer", "sarah.analyst"

### 2. **Policies**
- JSON documents that define permissions
- Specify which actions can be performed on which resources
- Can be attached to users or groups
- Example: S3 Full Access, EC2 Read Only, Lambda Invoke

### 3. **User Groups**
- Collections of users with shared permissions
- Policies are attached to groups rather than individual users
- Users inherit all group permissions
- Example: "Developers Group", "Analytics Team", "Support Staff"

---

## Security Best Practices Demonstrated

1. **Authentication First**: Users must log in with credentials before accessing anything
2. **Authorization After**: Users only access services they have explicit permission for
3. **Least Privilege**: Users get only the minimum permissions needed for their role
4. **Scalability**: User groups allow efficient management as the organization grows
5. **Temporary Credentials**: Root user doesn't share permanent passwords; temporary passwords are changed immediately
6. **Centralized Management**: All permissions managed from one IAM dashboard

---

## Conclusion

This AWS IAM demo project showcases how modern cloud security works through the combination of authentication (proving who you are) and authorization (determining what you can access). By using users, policies, and groups, organizations can securely manage access at scale while maintaining strong security practices.

The progression from a single user with no permissions, to granting specific permissions, to using groups for efficient management, reflects real-world IAM best practices in AWS environments.