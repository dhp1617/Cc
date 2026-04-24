# Exp:1 Design and Configure a Custom Mode VPC with Multi-Region Subnets --->>

## Objective

Create a Custom Mode VPC network and configure subnets in the us-central1 and asia-east1 regions to support multi-region networking.

---

## Steps

### Step 1 - Open the Google Cloud Console

1. Go to https://console.cloud.google.com
2. Sign in with your Google account.
3. Select your project from the project dropdown at the top of the page.

### Step 2 - Navigate to VPC Networks

1. Click the Navigation Menu (the three horizontal lines icon) on the top left.
2. Scroll down and click on Networking.
3. Under the Networking section, click on VPC network.
4. Click on VPC networks from the submenu.

### Step 3 - Create a New VPC Network

1. Click the Create VPC Network button at the top of the page.
2. In the Name field, enter a name for your VPC, for example: custom-vpc-network
3. In the Description field, optionally enter: Custom Mode VPC for multi-region networking

### Step 4 - Select Custom Subnet Mode

1. Under the Subnet creation mode section, select Custom.
   - This disables automatic subnet creation and allows you to manually define subnets in specific regions.

### Step 5 - Add the First Subnet (us-central1)

1. Under the New subnet section that appears, fill in the following:
   - Name: subnet-us-central1
   - Region: Select us-central1 from the dropdown
   - IP address range: Enter 10.0.1.0/24
   - Private Google Access: Set to Off (you can enable this later if needed)
   - Flow logs: Set to Off
2. Click Done to save this subnet.

### Step 6 - Add the Second Subnet (asia-east1)

1. Click the Add subnet button to add another subnet.
2. Fill in the following details:
   - Name: subnet-asia-east1
   - Region: Select asia-east1 from the dropdown
   - IP address range: Enter 10.0.2.0/24
   - Private Google Access: Set to Off
   - Flow logs: Set to Off
3. Click Done to save this subnet.

### Step 7 - Configure Firewall Rules (Optional but Recommended)

1. Scroll down to the Firewall rules section.
2. You may enable default rules like:
   - allow-internal
   - allow-ssh
   - allow-rdp
   - allow-icmp
3. Check the boxes next to the rules you want to apply.

### Step 8 - Set Dynamic Routing Mode

1. Under the Dynamic routing mode section, select Regional or Global based on your requirement.
   - Regional: Routes are propagated only within the same region.
   - Global: Routes are propagated to all regions.

### Step 9 - Create the VPC Network

1. Review all the settings.
2. Click the Create button at the bottom of the page.
3. Wait for the VPC network to be created. This may take a few seconds.

### Step 10 - Verify the VPC and Subnets

1. Once created, you will be redirected to the VPC networks list.
2. Click on your newly created VPC network (custom-vpc-network).
3. Click the Subnets tab to verify both subnets are listed:
   - subnet-us-central1 in the us-central1 region
   - subnet-asia-east1 in the asia-east1 region
4. Confirm the IP ranges are correct for each subnet.

---

# Exp:2 Launch a Compute Engine VM Without External IP and Enable Private Google Access --->>

## Objective

Launch a Compute Engine virtual machine without an external IP address and enable Private Google Access on the subnet to allow the VM to access Google services securely.

---

## Steps

### Step 1 - Enable Private Google Access on the Subnet

1. Go to https://console.cloud.google.com
2. Click the Navigation Menu and go to Networking, then VPC network, then VPC networks.
3. Click on the VPC network where your subnet is located.
4. Click the Subnets tab.
5. Click on the subnet you want to modify (for example: subnet-us-central1).
6. Click the Edit button (pencil icon) at the top.
7. Under Private Google Access, toggle the switch to On.
8. Click Save.

### Step 2 - Navigate to Compute Engine

1. Click the Navigation Menu.
2. Scroll down to Compute Engine.
3. Click on VM instances.

### Step 3 - Create a New VM Instance

1. Click the Create Instance button at the top.
2. In the Name field, enter a name for your VM, for example: vm-no-external-ip

### Step 4 - Configure Region and Zone

1. Under Region, select us-central1.
2. Under Zone, select any available zone such as us-central1-a.

### Step 5 - Choose Machine Configuration

1. Under Machine configuration, select the Series as E2.
2. Under Machine type, select e2-micro (suitable for testing purposes).

### Step 6 - Configure the Boot Disk

1. Under Boot disk, click Change if you want to modify the OS.
2. Select Debian GNU/Linux (latest version) as the operating system.
3. Click Select to confirm.

### Step 7 - Configure Networking to Remove External IP

1. Scroll down to the Advanced options section and expand it.
2. Click on Networking to expand the networking settings.
3. Under Network interfaces, click the default network interface to expand it.
4. In the Network field, select your custom VPC network.
5. In the Subnetwork field, select the subnet where Private Google Access is enabled (for example: subnet-us-central1).
6. Under External IPv4 address, select None from the dropdown.
   - This ensures the VM has no external IP address and cannot be accessed directly from the internet.
7. Click Done to close the network interface settings.

### Step 8 - Create the VM

1. Review all configurations.
2. Click the Create button at the bottom.
3. Wait for the VM to start. The Status column will show a green checkmark when the VM is running.

### Step 9 - Verify the VM Has No External IP

1. On the VM instances page, locate your VM (vm-no-external-ip).
2. In the External IP column, it should show None, confirming no external IP is assigned.
3. The Internal IP column will show the private IP address assigned from your subnet range.

### Step 10 - Verify Private Google Access

1. Go back to VPC network, then Subnets.
2. Click on your subnet (subnet-us-central1).
3. Confirm that Private Google Access shows as On.
4. This allows the VM to reach Google APIs and services (like Cloud Storage) using its internal IP.

---

# Exp:3 Create a Cloud Storage Bucket, Upload Objects, and Configure Lifecycle Rules --->>

## Objective

Create a Cloud Storage bucket, upload files into it, and configure lifecycle management rules to automatically transition objects from Standard storage class to Nearline after 30 days.

---

## Steps

### Step 1 - Navigate to Cloud Storage

1. Go to https://console.cloud.google.com
2. Click the Navigation Menu.
3. Scroll down and click on Cloud Storage.
4. Click on Buckets.

### Step 2 - Create a New Bucket

1. Click the Create bucket button at the top.
2. In the Name your bucket section, enter a globally unique bucket name, for example: my-storage-bucket-2024-lab
3. Click Continue.

### Step 3 - Choose Where to Store Your Data

1. Under Location type, select Region for a single-region bucket or Multi-region for higher availability.
2. If you selected Region, choose a region such as us-central1.
3. Click Continue.

### Step 4 - Choose a Default Storage Class

1. Under Choose a default storage class, select Standard.
   - Standard is suitable for frequently accessed data.
2. Click Continue.

### Step 5 - Control Access to Objects

1. Under Choose how to control access to objects, select Uniform (recommended).
   - Uniform applies bucket-level permissions for consistent access control.
2. Click Continue.

### Step 6 - Configure Advanced Settings

1. Leave the default settings for encryption and data protection unless you have specific requirements.
2. Click Create.
3. If prompted about public access, click Confirm to proceed.

### Step 7 - Upload Objects to the Bucket

1. Once the bucket is created, click on the bucket name to open it.
2. Click the Upload files button.
3. In the file browser that opens, select one or more files from your local computer.
4. Click Open to start the upload.
5. Wait for the upload to complete. Uploaded files will appear in the bucket with their names, sizes, and storage class (Standard).

### Step 8 - Navigate to Lifecycle Management

1. In your bucket, click the Lifecycle tab at the top of the page.
2. Click the Add a rule button.

### Step 9 - Configure the Lifecycle Rule

1. Under Select an action, choose Set storage class to Nearline.
2. Click Continue.
3. Under Select object conditions, choose Age.
4. In the Age field, enter 30 (this means 30 days after object creation).
5. Click Continue.

### Step 10 - Review and Save the Rule

1. Review the lifecycle rule summary:
   - Action: Set storage class to Nearline
   - Condition: Age greater than 30 days
2. Click Create to save the rule.
3. The rule will now appear in the Lifecycle rules list.

### Step 11 - Verify the Lifecycle Rule

1. On the Lifecycle tab, confirm you can see the rule listed.
2. The rule will automatically apply to all objects in the bucket that are older than 30 days and transition them from Standard to Nearline storage class to reduce costs.

---

# Exp:4 Deploy a Managed Cloud SQL MySQL Instance --->>

## Objective

Deploy a managed Cloud SQL instance running MySQL and configure database settings to support scalable and reliable application data storage.

---

## Steps

### Step 1 - Navigate to Cloud SQL

1. Go to https://console.cloud.google.com
2. Click the Navigation Menu.
3. Scroll down to Databases.
4. Click on SQL.

### Step 2 - Create a New Instance

1. Click the Create instance button.
2. Select MySQL as the database engine.
3. Click Next.

### Step 3 - Configure Instance ID and Password

1. In the Instance ID field, enter a name for your instance, for example: mysql-instance-lab
2. In the Password field, enter a strong root password.
   - You can also click Generate to let Google create a random password. Store this password securely.
3. Under Database version, select MySQL 8.0 (recommended for latest features and security).

### Step 4 - Choose Cloud SQL Edition

1. Under Choose Cloud SQL edition, select Enterprise (suitable for most production workloads).
2. Under Preset, select Sandbox for a low-cost option for testing, or Production for a full setup.

### Step 5 - Configure Region and Zone

1. Under Region, select us-central1.
2. Under Zonal availability, select Single zone for testing or Multiple zones (High availability) for production.
   - Multiple zones enables automatic failover to a standby instance in case of zone failure.

### Step 6 - Configure Machine Type

1. Under Machine configuration, click on the Machine type dropdown.
2. Select Lightweight (1 vCPU, 3.75 GB RAM) for testing purposes.
3. For production workloads, select a higher specification as required.

### Step 7 - Configure Storage

1. Under Storage, set the Storage type to SSD (recommended for better performance).
2. Set the Storage capacity to 10 GB for testing or increase it as needed.
3. Enable Enable automatic storage increases to allow Cloud SQL to automatically increase storage when it nears capacity.

### Step 8 - Configure Connections

1. Under Connections, you can configure:
   - Public IP: Enable this if you need to connect from external applications. A public IP address will be assigned.
   - Private IP: Enable this for secure internal connections within a VPC network.
2. For a secure setup, enable Private IP and select your VPC network.
3. If enabling Private IP for the first time, you may be prompted to enable the Service Networking API. Click Enable.

### Step 9 - Configure Backups and Maintenance

1. Under Data protection, ensure Enable automated backups is checked.
2. Set the Backup window to a low-traffic period (for example, 2:00 AM to 6:00 AM).
3. Set the backup Retention period to 7 days.
4. Enable Enable point-in-time recovery for additional data protection.

### Step 10 - Create the Instance

1. Review all configurations.
2. Click Create instance at the bottom.
3. Wait for the instance to be provisioned. This may take several minutes. The Status will change from Pending creation to Running when ready.

### Step 11 - Verify the Instance

1. Once the instance is running, click on the instance name.
2. On the Overview page, verify the following:
   - Instance ID matches what you entered.
   - Database version shows MySQL 8.0.
   - Region shows us-central1.
   - Status shows Running.
3. Note down the Public IP address or Private IP address for connecting your application.

### Step 12 - Create a Database

1. In the left panel of the instance details page, click on Databases.
2. Click Create database.
3. Enter a name such as: app_database
4. Click Create to confirm.

---

# Exp:5 Launch a Compute Engine VM and Automate Apache Web Server Installation via Startup Script --->>

## Objective

Launch a Compute Engine virtual machine and use a startup script to automatically update system packages and install the Apache web server on boot.

---

## Steps

### Step 1 - Navigate to Compute Engine

1. Go to https://console.cloud.google.com
2. Click the Navigation Menu.
3. Scroll down to Compute Engine.
4. Click on VM instances.

### Step 2 - Create a New VM Instance

1. Click the Create Instance button.
2. In the Name field, enter: vm-apache-startup

### Step 3 - Configure Region and Zone

1. Under Region, select us-central1.
2. Under Zone, select us-central1-a.

### Step 4 - Choose Machine Type

1. Under Machine configuration, select E2 series.
2. Under Machine type, select e2-micro.

### Step 5 - Configure Boot Disk

1. Under Boot disk, click Change.
2. Select Debian GNU/Linux 11 (Bullseye) as the operating system.
3. Set Boot disk size to 10 GB.
4. Click Select.

### Step 6 - Configure Firewall to Allow HTTP Traffic

1. Under Firewall, check the box next to Allow HTTP traffic.
   - This automatically creates a firewall rule to allow traffic on port 80 to your VM.
2. Optionally, check Allow HTTPS traffic if needed.

### Step 7 - Add the Startup Script

1. Scroll down and click on Advanced options to expand it.
2. Click on Management to expand that section.
3. Scroll down to find the Automation section.
4. In the Startup script field, enter the following script:

#! /bin/bash
apt-get update
apt-get install -y apache2
systemctl start apache2
systemctl enable apache2
echo "<h1>Apache Server Running on $(hostname)</h1>" > /var/www/html/index.html

5. This script will:
   - Update the package list
   - Install Apache web server
   - Start and enable Apache to run on boot
   - Create a simple HTML page showing the hostname

### Step 8 - Create the VM

1. Review all configurations.
2. Click Create at the bottom.
3. Wait for the instance to start. The Status column will show a green checkmark when running.

### Step 9 - Verify Apache Installation

1. Once the VM is running, locate the External IP address on the VM instances page.
2. Open a new browser tab and enter the external IP address in the address bar:
   http://EXTERNAL_IP_ADDRESS
3. You should see the message: Apache Server Running on [hostname]
4. This confirms the startup script ran successfully and Apache is serving web traffic.

### Step 10 - Verify via Serial Console (Optional)

1. Click on the VM name to open the instance details.
2. Click on Logs, then click on Serial port 1 (console).
3. Scroll through the boot logs to confirm the startup script executed without errors.
4. You should see lines indicating apt-get update and apache2 installation completed successfully.

---

# Exp:6 Build an Instance Template and Deploy a Managed Instance Group with Autoscaling --->>

## Objective

Create an instance template and use it to deploy a Managed Instance Group (MIG). Configure an autoscaling policy that scales the group when CPU utilization exceeds 60 percent.

---

## Steps

### Step 1 - Navigate to Instance Templates

1. Go to https://console.cloud.google.com
2. Click the Navigation Menu.
3. Go to Compute Engine.
4. In the left sidebar, click on Instance templates.

### Step 2 - Create a New Instance Template

1. Click the Create instance template button.
2. In the Name field, enter: web-server-template

### Step 3 - Configure the Machine Type

1. Under Machine configuration, select E2 series.
2. Under Machine type, select e2-micro.

### Step 4 - Configure Boot Disk

1. Under Boot disk, click Change.
2. Select Debian GNU/Linux 11 as the OS.
3. Set disk size to 10 GB.
4. Click Select.

### Step 5 - Configure Firewall

1. Under Firewall, check Allow HTTP traffic.

### Step 6 - Add a Startup Script to the Template

1. Scroll down and expand Advanced options.
2. Click on Management.
3. In the Startup script field, enter:

#! /bin/bash
apt-get update
apt-get install -y apache2
systemctl start apache2
systemctl enable apache2

### Step 7 - Create the Template

1. Click Create at the bottom.
2. Wait for the template to be created. It will appear in the Instance templates list.

### Step 8 - Navigate to Instance Groups

1. In the left sidebar under Compute Engine, click on Instance groups.

### Step 9 - Create a Managed Instance Group

1. Click Create instance group.
2. Select New managed instance group (stateless).
3. In the Name field, enter: web-server-mig
4. Under Instance template, select the template you just created: web-server-template.
5. Under Location, select Single zone.
6. Set Region to us-central1 and Zone to us-central1-a.

### Step 10 - Configure Autoscaling

1. Under Autoscaling, ensure Autoscaling mode is set to On: add and remove instances to the group.
2. Under Autoscaling signals, click Add signal if needed and select CPU utilization.
3. In the Target CPU utilization field, enter: 60
   - This means the autoscaler will add instances when average CPU usage exceeds 60 percent and remove them when it drops below.
4. Under Minimum number of instances, enter: 1
5. Under Maximum number of instances, enter: 4 (or your preferred maximum).
6. Under Initialization period, enter 60 (seconds) to allow new instances time to start up before autoscaling checks them.

### Step 11 - Configure Health Check (Optional but Recommended)

1. Under Health check, click Create a health check.
2. In the Name field, enter: http-health-check
3. Set Protocol to HTTP.
4. Set Port to 80.
5. Set Request path to /
6. Click Save and continue.

### Step 12 - Create the Instance Group

1. Review all settings.
2. Click Create at the bottom.
3. Wait for the MIG to be created and for the initial VM instances to start running.

### Step 13 - Verify the Managed Instance Group

1. On the Instance groups page, click on your MIG (web-server-mig).
2. Under the Members tab, confirm that at least one VM instance is running.
3. Click on the Monitoring tab to see CPU utilization graphs.
4. Under the Details tab, verify the autoscaling policy shows:
   - Target CPU utilization: 60%
   - Min instances: 1
   - Max instances: 4

---

# Exp:7 Create a Test User in IAM and Assign Storage Object Viewer Role --->>

## Objective

Create a second IAM user (Test User) and assign the Storage Object Viewer role to grant read-only access to a specific Cloud Storage bucket.

---

## Steps

### Step 1 - Navigate to IAM

1. Go to https://console.cloud.google.com
2. Click the Navigation Menu.
3. Scroll down to IAM and Admin.
4. Click on IAM.

### Step 2 - Grant Access to a New User at Project Level (Overview)

Note: IAM users are Google accounts. You cannot create a Google account from the GCP Console directly. Instead, you grant an existing Google account access to your project. For this lab, you need a second Google account email address to act as the Test User.

### Step 3 - Add the Test User to the Project

1. On the IAM page, click the Grant Access button at the top.
2. In the New principals field, enter the email address of the Test User's Google account.
   - For example: testuser@gmail.com
3. Under Assign roles, click on the Select a role dropdown.
4. In the search bar, type Storage and press Enter.
5. Select Storage Object Viewer from the list.
   - This role grants the user permission to view (list and read) objects in Cloud Storage buckets.
6. Click Save.

### Step 4 - Verify the Role Assignment

1. Back on the IAM page, search for the Test User's email in the filter box.
2. Confirm that the user appears with the role Storage Object Viewer listed.

### Step 5 - Restrict the Role to a Specific Bucket

Note: IAM at the project level grants access to all buckets. To restrict access to a specific bucket, you must set permissions at the bucket level.

1. Click the Navigation Menu and go to Cloud Storage, then Buckets.
2. Click on the specific bucket you want to grant access to.
3. Click the Permissions tab.
4. Click the Grant access button.
5. In the New principals field, enter the Test User's email address.
6. In the Select a role dropdown, select Storage Object Viewer.
7. Click Save.

### Step 6 - Remove Project-Level Access (If Restricting to One Bucket Only)

1. Go back to IAM and Admin, then IAM.
2. Find the Test User in the list.
3. Click the Edit (pencil) icon next to their name.
4. Remove the Storage Object Viewer role at the project level by clicking the delete (trash) icon next to the role.
5. Click Save.
   - Now the Test User only has access to the specific bucket, not all buckets in the project.

### Step 7 - Verify Bucket-Level Access

1. Log in to the Google Cloud Console using the Test User's Google account.
2. Navigate to Cloud Storage, then Buckets.
3. The Test User should only be able to see and access the specific bucket where the role was granted.
4. Attempting to access or list other buckets should result in a permission denied error.

---

# Exp:8 Create a Service Account with Compute Viewer Permissions and Attach It to a VM --->>

## Objective

Create a service account with the Compute Viewer IAM role and attach it to a Compute Engine virtual machine to enable secure service-to-service authentication.

---

## Steps

### Step 1 - Navigate to Service Accounts

1. Go to https://console.cloud.google.com
2. Click the Navigation Menu.
3. Go to IAM and Admin.
4. Click on Service Accounts.

### Step 2 - Create a New Service Account

1. Click the Create Service Account button at the top.
2. In the Service account name field, enter: compute-viewer-sa
3. The Service account ID will auto-populate based on the name. You can leave it as is.
4. In the Description field, optionally enter: Service account for Compute Viewer permissions
5. Click Create and Continue.

### Step 3 - Assign the Compute Viewer Role

1. Under Grant this service account access to project, click on the Select a role dropdown.
2. In the search field, type Compute Viewer.
3. Select Compute Viewer from the list under the Compute Engine section.
   - This role grants read-only access to all Compute Engine resources.
4. Click Continue.

### Step 4 - Grant Users Access to This Service Account (Optional)

1. In the Grant users access to this service account section, you can optionally add users or groups who can use this service account.
2. Leave this blank for now if not required.
3. Click Done.

### Step 5 - Verify the Service Account

1. On the Service Accounts page, confirm the new service account (compute-viewer-sa) appears in the list.
2. Note the full email address of the service account. It will look like:
   compute-viewer-sa@PROJECT_ID.iam.gserviceaccount.com

### Step 6 - Navigate to VM Instances

1. Click the Navigation Menu.
2. Go to Compute Engine, then VM instances.

### Step 7 - Create a New VM and Attach the Service Account

1. Click the Create Instance button.
2. Enter a name for the VM: vm-with-service-account
3. Set Region to us-central1 and Zone to us-central1-a.
4. Under Machine type, select e2-micro.

### Step 8 - Attach the Service Account to the VM

1. Scroll down and expand Advanced options.
2. Click on Security to expand it.
3. Under Identity and API access, find the Service account dropdown.
4. Click on the dropdown and select the service account you created: compute-viewer-sa
5. Under Access scopes, select Allow full access to all Cloud APIs (the service account role will restrict access anyway).

### Step 9 - Create the VM

1. Review all settings.
2. Click Create.
3. Wait for the VM to start running.

### Step 10 - Verify the Service Account is Attached

1. Once the VM is running, click on the VM name to open its details.
2. Scroll down to the Service account section.
3. Confirm it shows: compute-viewer-sa@PROJECT_ID.iam.gserviceaccount.com

### Step 11 - Test the Service Account Permissions

1. On the VM instances page, click SSH next to your VM to open a terminal.
2. In the SSH terminal, run the following command to list Compute Engine instances using the service account credentials:
   gcloud compute instances list
3. This should return a list of VM instances, confirming the Compute Viewer role is working.
4. Try a write operation such as deleting an instance to confirm the service account only has read access and the command is denied.

---

# Exp:9 Create a Folder in Your Organization and Assign IAM Roles at the Folder Level --->>

## Objective

Create a folder within your Google Cloud organization and assign IAM roles at the folder level to understand how IAM policy inheritance works across projects.

---

## Steps

### Step 1 - Ensure You Have an Organization

1. Go to https://console.cloud.google.com
2. Click the project selector dropdown at the top left of the page.
3. In the Select a resource dialog, look for your Organization node at the top of the hierarchy.
4. If you do not see an organization, this experiment requires a Google Workspace or Cloud Identity account. Free personal accounts do not have an organization.

### Step 2 - Navigate to Resource Manager

1. Click the Navigation Menu.
2. Go to IAM and Admin.
3. Click on Manage Resources.
   - This page shows the resource hierarchy including your organization, folders, and projects.

### Step 3 - Select Your Organization

1. On the Manage Resources page, click on your Organization node in the hierarchy tree on the left side.
2. The right panel will show the organization details.

### Step 4 - Create a New Folder

1. Click the Create Folder button at the top of the page.
2. In the Folder name field, enter a name for your folder, for example: dev-folder
3. Under Organization, confirm your organization is selected.
4. In the Location field, confirm the folder will be created under the organization root or select a parent folder if needed.
5. Click Create to create the folder.

### Step 5 - Verify the Folder

1. The new folder (dev-folder) will appear in the resource hierarchy under your organization.
2. Click on the folder to select it.

### Step 6 - Assign an IAM Role at the Folder Level

1. With your folder selected, click the three-dot menu (More actions) next to the folder name, or look for the Permissions panel on the right.
2. Click Manage folder permissions or Edit permissions.
   - Alternatively, you can go to IAM and Admin, then IAM, and switch the scope to the folder level by selecting the folder in the scope selector.
3. Click the Grant Access button.
4. In the New principals field, enter an email address (a Google account or group email).
5. In the Select a role dropdown, choose a role. For example:
   - Select Editor to grant edit access to all projects under this folder.
   - Or select Viewer to grant read-only access.
6. Click Save.

### Step 7 - Understand Policy Inheritance

1. Create a project under this folder:
   a. On the Manage Resources page, click Create Project.
   b. Enter a project name, for example: dev-project-01
   c. Under Location, click Browse and select your folder (dev-folder).
   d. Click Create.
2. Navigate to IAM and Admin, then IAM for the new project.
3. Observe that the role you assigned at the folder level is inherited by this project.
4. Inherited roles are shown with a folder icon or labeled as inherited from the parent in the IAM console.
5. These inherited roles cannot be removed at the project level; they must be modified or removed at the folder level.

### Step 8 - Verify Inheritance Behavior

1. In the project's IAM page, scroll through the list of principals.
2. The user or group that was granted a role at the folder level will appear with an indicator showing the policy is inherited.
3. Attempt to remove the inherited role from the project level. You will see that it is not editable here, confirming that policies flow down through the hierarchy.

---

# Exp:10 Deploy an HTTP(S) Load Balancer and Configure a Cloud Armor Security Policy --->>

## Objective

Deploy an HTTP(S) Application Load Balancer with a backend instance group and configure a Cloud Armor security policy to protect the backend services from unauthorized access.

---

## Steps

### Step 1 - Create a Managed Instance Group as the Backend

1. Go to https://console.cloud.google.com
2. Navigate to Compute Engine, then Instance templates.
3. Create an instance template named: lb-backend-template
   - Machine type: e2-micro
   - OS: Debian GNU/Linux 11
   - Allow HTTP traffic checked
   - Startup script: apt-get update and apt-get install -y apache2
4. Navigate to Compute Engine, then Instance groups.
5. Click Create instance group.
6. Select New managed instance group (stateless).
7. Name it: lb-backend-mig
8. Select the instance template: lb-backend-template
9. Set Region to us-central1 and Zone to us-central1-a.
10. Set minimum instances to 1 and maximum to 2.
11. Click Create.

### Step 2 - Navigate to Load Balancing

1. Click the Navigation Menu.
2. Go to Network services.
3. Click on Load balancing.

### Step 3 - Create a Load Balancer

1. Click Create load balancer.
2. Under Application Load Balancer (HTTP/HTTPS), click Start configuration.
3. Choose the following settings:
   - Internet facing or internal: Select From Internet to my VMs or serverless services (Public).
   - Global or regional: Select Global external Application Load Balancer.
4. Click Continue.

### Step 4 - Configure the Frontend

1. In the Name field, enter: http-load-balancer
2. Under Frontend configuration, click Add Frontend IP and port.
3. In the Name field, enter: lb-frontend
4. Set Protocol to HTTP.
5. Set IP version to IPv4.
6. Under IP address, click Create IP address.
   - Enter a name such as: lb-static-ip
   - Click Reserve.
7. Set Port to 80.
8. Click Done.

### Step 5 - Configure the Backend

1. Under Backend configuration, click Add backend.
2. Under Backend type, select Instance group.
3. Click Add backend.
4. Under Instance group, select your MIG: lb-backend-mig
5. Set Port numbers to 80.
6. Set Balancing mode to Utilization.
7. Set Maximum backend utilization to 80 percent.
8. Under Health check, click Create a health check.
   - Name: http-health-check
   - Protocol: HTTP
   - Port: 80
   - Request path: /
9. Click Save.
10. Click Create.
11. Check the Enable Cloud CDN option if needed or leave it unchecked.
12. Click Create to create the backend service.

### Step 6 - Configure URL Map and Routing Rules

1. Under Host and path rules, leave the default routing rule which sends all traffic to the backend service.
2. Click Next.

### Step 7 - Review and Create the Load Balancer

1. Review all settings.
2. Click Create.
3. Wait for the load balancer to be created. This may take a few minutes.

### Step 8 - Note the Load Balancer IP Address

1. On the Load balancing page, click on your load balancer (http-load-balancer).
2. Under the Frontend section, note the IP:Port value. This is the public IP address of the load balancer.
3. Open a browser tab and enter: http://LOAD_BALANCER_IP
4. You should see the Apache default page served from one of the backend VMs.

### Step 9 - Navigate to Cloud Armor

1. Click the Navigation Menu.
2. Go to Network security.
3. Click on Cloud Armor policies.

### Step 10 - Create a Cloud Armor Security Policy

1. Click the Create policy button.
2. In the Name field, enter: lb-security-policy
3. Under Policy type, select Backend security policy.
4. Under Default rule action, select Allow.
   - This allows all traffic by default. You will add specific deny rules below.

### Step 11 - Add Security Rules

1. Click the Add rule button.
2. To block a specific IP range (for example, to simulate blocking unauthorized access):
   - In the Mode field, select Basic mode.
   - In the Match field, enter an IP range to block, such as: 192.0.2.0/24 (this is a test/example IP range)
   - Under Action, select Deny.
   - Under Deny status, select 403 (Forbidden).
   - Set Priority to 1000 (lower number means higher priority).
3. Click Done.
4. You can add additional rules to:
   - Allow only specific geographic regions using the expression: origin.region_code == "US"
   - Block known malicious IP ranges.

### Step 12 - Save the Security Policy

1. Review the policy rules.
2. Click Create policy to save.

### Step 13 - Attach the Security Policy to the Load Balancer Backend

1. On the Cloud Armor policies page, click on your policy (lb-security-policy).
2. Click the Targets tab.
3. Click Apply policy to target.
4. Under Backend service, select the backend service associated with your load balancer (for example: http-load-balancer-backend).
5. Click Add.
6. Click Save.

### Step 14 - Verify Cloud Armor is Active

1. Navigate back to Network services, then Load balancing.
2. Click on your load balancer (http-load-balancer).
3. Click on the backend service link.
4. Scroll down to the Cloud Armor section.
5. Confirm that your security policy (lb-security-policy) is listed as attached.
6. Cloud Armor will now inspect all incoming traffic to the load balancer and apply the rules you configured before forwarding requests to the backend VMs.

---
