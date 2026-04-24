---

# Exp:1 Design and Configure a Custom Mode VPC with Multi-Region Subnets --->>>

## Steps

### Step 1 - Open the Google Cloud Console

1. Go to https://console.cloud.google.com
2. Select your project from the project dropdown.

### Step 2 - Navigate to VPC Networks

1. Click Navigation Menu, go to Networking, then VPC network, then VPC networks.

### Step 3 - Create a New VPC Network

1. Click Create VPC Network.
2. Name: custom-vpc-network
3. Subnet creation mode: select Custom.

### Step 4 - Add Subnet in us-central1

1. Under New subnet, fill in:
   - Name: subnet-us-central1
   - Region: us-central1
   - IP address range: 10.0.1.0/24
2. Click Done.

### Step 5 - Add Subnet in asia-east1

1. Click Add subnet.
2. Fill in:
   - Name: subnet-asia-east1
   - Region: asia-east1
   - IP address range: 10.0.2.0/24
3. Click Done.

### Step 6 - Create the VPC Network

1. Click Create.

### Step 7 - Verify

1. Click on the VPC network, then Subnets tab.
2. Confirm both subnets appear with correct regions and IP ranges.

---

# Exp:2 Launch a Compute Engine VM Without External IP and Enable Private Google Access --->>>

## Steps

### Step 1 - Enable Private Google Access on the Subnet

1. Go to Networking, then VPC network, then VPC networks.
2. Click on your VPC, then the Subnets tab.
3. Click on subnet-us-central1, then Edit.
4. Set Private Google Access to On.
5. Click Save.

### Step 2 - Create a VM Instance

1. Go to Compute Engine, then VM instances.
2. Click Create Instance.
3. Name: vm-no-external-ip
4. Region: us-central1, Zone: us-central1-a
5. Series: E2, Machine type: e2-micro
6. Boot disk: Debian GNU/Linux (latest)

### Step 3 - Remove External IP

1. Expand Advanced options, then Networking.
2. Click on the default network interface.
3. Set Network to your custom VPC.
4. Set Subnetwork to subnet-us-central1.
5. Set External IPv4 address to None.
6. Click Done.

### Step 4 - Create and Verify

1. Click Create.
2. On the VM instances page, confirm External IP shows None for your VM.

---

# Exp:3 Create a Cloud Storage Bucket, Upload Objects, and Configure Lifecycle Rules --->>>

## Steps

### Step 1 - Create a Bucket

1. Go to Cloud Storage, then Buckets.
2. Click Create bucket.
3. Enter a unique name, for example: my-storage-bucket-2024-lab
4. Location type: Region, select us-central1.
5. Default storage class: Standard.
6. Access control: Uniform.
7. Click Create.

### Step 2 - Upload Objects

1. Click on the bucket name.
2. Click Upload files, select files from your computer, click Open.

### Step 3 - Add a Lifecycle Rule

1. Click the Lifecycle tab inside the bucket.
2. Click Add a rule.
3. Action: Set storage class to Nearline, click Continue.
4. Condition: Age, enter 30, click Continue.
5. Click Create.

### Step 4 - Verify

1. Confirm the rule appears in the Lifecycle tab showing Nearline after 30 days.

---

# Exp:4 Deploy a Managed Cloud SQL MySQL Instance --->>>

## Steps

### Step 1 - Create a Cloud SQL Instance

1. Go to Navigation Menu, then Databases, then SQL.
2. Click Create instance, select MySQL, click Next.
3. Instance ID: mysql-instance-lab
4. Set a root password.
5. Database version: MySQL 8.0.

### Step 2 - Configure Edition and Region

1. Cloud SQL edition: Enterprise.
2. Preset: Sandbox (for testing).
3. Region: us-central1.
4. Zonal availability: Single zone (testing) or Multiple zones (production).

### Step 3 - Configure Machine and Storage

1. Machine type: Lightweight (1 vCPU, 3.75 GB RAM).
2. Storage type: SSD.
3. Storage capacity: 10 GB.
4. Enable automatic storage increases.

### Step 4 - Configure Connections

1. Enable Private IP and select your VPC network.
2. If prompted, enable the Service Networking API.

### Step 5 - Configure Backups

1. Enable automated backups.
2. Set backup window to a low-traffic period.
3. Retention: 7 days.
4. Enable point-in-time recovery.

### Step 6 - Create and Verify

1. Click Create instance and wait for Status to show Running.
2. Click on the instance, go to Databases, click Create database.
3. Name: app_database, click Create.

---

# Exp:5 Launch a Compute Engine VM and Automate Apache Web Server Installation via Startup Script --->>>

## Steps

### Step 1 - Create a VM Instance

1. Go to Compute Engine, then VM instances.
2. Click Create Instance.
3. Name: vm-apache-startup
4. Region: us-central1, Zone: us-central1-a
5. Series: E2, Machine type: e2-micro
6. Boot disk: Debian GNU/Linux 11, size 10 GB.
7. Under Firewall, check Allow HTTP traffic.

### Step 2 - Add Startup Script

1. Expand Advanced options, then Management.
2. In the Startup script field, enter:

#! /bin/bash
apt-get update
apt-get install -y apache2
systemctl start apache2
systemctl enable apache2
echo "<h1>Apache Server Running on $(hostname)</h1>" > /var/www/html/index.html

### Step 3 - Create and Verify

1. Click Create and wait for the VM to start.
2. Copy the External IP from the VM instances page.
3. Open a browser and go to http://EXTERNAL_IP
4. Confirm the Apache page loads.

---

# Exp:6 Build an Instance Template and Deploy a Managed Instance Group with Autoscaling --->>>

## Steps

### Step 1 - Create an Instance Template

1. Go to Compute Engine, then Instance templates.
2. Click Create instance template.
3. Name: web-server-template
4. Series: E2, Machine type: e2-micro.
5. Boot disk: Debian GNU/Linux 11, 10 GB.
6. Firewall: check Allow HTTP traffic.
7. Expand Advanced options, then Management. Add startup script:

#! /bin/bash
apt-get update
apt-get install -y apache2
systemctl start apache2
systemctl enable apache2

8. Click Create.

### Step 2 - Create a Managed Instance Group

1. Go to Compute Engine, then Instance groups.
2. Click Create instance group.
3. Select New managed instance group (stateless).
4. Name: web-server-mig
5. Instance template: web-server-template
6. Location: Single zone, Region: us-central1, Zone: us-central1-a

### Step 3 - Configure Autoscaling

1. Autoscaling mode: On: add and remove instances to the group.
2. Signal: CPU utilization.
3. Target CPU utilization: 60
4. Minimum instances: 1
5. Maximum instances: 4
6. Initialization period: 60 seconds

### Step 4 - Configure Health Check

1. Click Create a health check.
2. Name: http-health-check, Protocol: HTTP, Port: 80, Path: /
3. Click Save and continue.

### Step 5 - Create and Verify

1. Click Create and wait for the MIG to be ready.
2. Click on the MIG, check the Members tab for running instances.
3. Under Details tab, confirm autoscaling shows 60% CPU target.

---

# Exp:7 Create a Test User in IAM and Assign Storage Object Viewer Role --->>>

## Steps

### Step 1 - Grant Access at Bucket Level

1. Go to Cloud Storage, then Buckets.
2. Click on the target bucket.
3. Click the Permissions tab.
4. Click Grant access.
5. New principals: enter the Test User email, for example: testuser@gmail.com
6. Role: select Storage Object Viewer.
7. Click Save.

### Step 2 - Verify in IAM

1. Go to IAM and Admin, then IAM.
2. Search for the Test User email.
3. Confirm Storage Object Viewer role is listed for that user.

### Step 3 - Verify Access as Test User

1. Log in with the Test User account.
2. Go to Cloud Storage, then Buckets.
3. Confirm the user can only access the specific bucket where the role was granted.

---

# Exp:8 Create a Service Account with Compute Viewer Permissions and Attach It to a VM --->>>

## Steps

### Step 1 - Create a Service Account

1. Go to IAM and Admin, then Service Accounts.
2. Click Create Service Account.
3. Name: compute-viewer-sa, click Create and Continue.
4. Role: select Compute Viewer, click Continue, then Done.

### Step 2 - Create a VM and Attach the Service Account

1. Go to Compute Engine, then VM instances.
2. Click Create Instance.
3. Name: vm-with-service-account
4. Region: us-central1, Zone: us-central1-a, Machine type: e2-micro.
5. Expand Advanced options, then Security.
6. Under Service account, select compute-viewer-sa.
7. Access scopes: Allow full access to all Cloud APIs.
8. Click Create.

### Step 3 - Verify

1. Click on the VM name.
2. Confirm the Service account field shows compute-viewer-sa@PROJECT_ID.iam.gserviceaccount.com
3. Click SSH on the VM.
4. Run: gcloud compute instances list
5. Confirm the command returns instance data, verifying read access works.

---

# Exp:9 Create a Folder in Your Organization and Assign IAM Roles at the Folder Level --->>>

## Steps

### Step 1 - Create a Folder

1. Go to IAM and Admin, then Manage Resources.
2. Select your Organization node from the hierarchy.
3. Click Create Folder.
4. Name: dev-folder
5. Confirm Location is under the organization root.
6. Click Create.

### Step 2 - Assign an IAM Role at the Folder Level

1. Select the folder (dev-folder) in the hierarchy.
2. Click the three-dot menu next to the folder, then Manage folder permissions.
3. Click Grant Access.
4. Enter the principal email address.
5. Select a role, for example: Viewer or Editor.
6. Click Save.

### Step 3 - Create a Project Under the Folder

1. On Manage Resources, click Create Project.
2. Name: dev-project-01
3. Under Location, select dev-folder.
4. Click Create.

### Step 4 - Verify Policy Inheritance

1. Go to IAM and Admin, then IAM for the new project dev-project-01.
2. Confirm the role assigned at the folder level appears in the project's IAM list with an inherited indicator.
3. Attempt to remove the inherited role at the project level to confirm it is not editable there.

---

# Exp:10 Deploy an HTTP(S) Load Balancer and Configure a Cloud Armor Security Policy --->>>

## Steps

### Step 1 - Create a Backend Instance Group

1. Go to Compute Engine, then Instance templates.
2. Create a template named lb-backend-template:
   - Machine type: e2-micro, OS: Debian GNU/Linux 11
   - Firewall: Allow HTTP traffic
   - Startup script: apt-get update and apt-get install -y apache2
3. Go to Compute Engine, then Instance groups.
4. Click Create instance group, select New managed instance group (stateless).
5. Name: lb-backend-mig, Template: lb-backend-template
6. Region: us-central1, Zone: us-central1-a, Min: 1, Max: 2.
7. Click Create.

### Step 2 - Create a Load Balancer

1. Go to Network services, then Load balancing.
2. Click Create load balancer.
3. Select Application Load Balancer (HTTP/HTTPS), click Start configuration.
4. Select From Internet to my VMs (Public) and Global external Application Load Balancer.
5. Click Continue.
6. Name: http-load-balancer

### Step 3 - Configure Frontend

1. Click Add Frontend IP and port.
2. Name: lb-frontend, Protocol: HTTP, IP version: IPv4.
3. Under IP address, click Create IP address, name it lb-static-ip, click Reserve.
4. Port: 80, click Done.

### Step 4 - Configure Backend

1. Click Add backend, select Instance group.
2. Instance group: lb-backend-mig, Port: 80.
3. Balancing mode: Utilization, Maximum utilization: 80%.
4. Create a health check: Name: http-health-check, Protocol: HTTP, Port: 80, Path: /
5. Click Save, then Create.

### Step 5 - Create the Load Balancer

1. Leave default routing rules under Host and path rules.
2. Click Create and wait for provisioning to complete.
3. Copy the frontend IP and open http://LOAD_BALANCER_IP in a browser to confirm it works.

### Step 6 - Create a Cloud Armor Security Policy

1. Go to Network security, then Cloud Armor policies.
2. Click Create policy.
3. Name: lb-security-policy
4. Policy type: Backend security policy.
5. Default rule action: Allow.

### Step 7 - Add a Deny Rule

1. Click Add rule.
2. Mode: Basic mode.
3. Match: 192.0.2.0/24
4. Action: Deny, Deny status: 403.
5. Priority: 1000.
6. Click Done, then Create policy.

### Step 8 - Attach Policy to the Load Balancer Backend

1. Click on lb-security-policy, then the Targets tab.
2. Click Apply policy to target.
3. Select the backend service of your load balancer.
4. Click Add, then Save.

### Step 9 - Verify

1. Go to Network services, then Load balancing.
2. Click on http-load-balancer, then the backend service link.
3. Confirm lb-security-policy is listed under the Cloud Armor section.

---
