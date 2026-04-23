# Exp1 : Custom Mode VPC with Multi-Region Subnets (Google Cloud) -->

## Step 1: Create Custom VPC

1. Open Google Cloud Console
2. Navigate to: VPC Network → VPC networks
3. Click "Create VPC Network"

Configuration:
- Name: custom-vpc
- Subnet creation mode: Custom

Click "Create"

---

## Step 2: Create Subnet (us-central1)

1. Open the created VPC (custom-vpc)
2. Click "Add Subnet"

Configuration:
- Name: subnet-us
- Region: us-central1
- IP range: 10.0.1.0/24

Click "Add"

---

## Step 3: Create Subnet (asia-east1)

1. Click "Add Subnet" again

Configuration:
- Name: subnet-asia
- Region: asia-east1
- IP range: 10.0.2.0/24

Click "Add"

---

## Step 4: Configure Firewall Rules

Go to: VPC Network → Firewall

### Rule 1: Allow Internal Traffic
- Name: allow-internal
- Network: custom-vpc
- Targets: All instances
- Source IP ranges: 10.0.0.0/16
- Protocols: TCP, UDP, ICMP

Click "Create"

### Rule 2: Allow SSH
- Name: allow-ssh
- Network: custom-vpc
- Targets: All instances
- Source IP ranges: 0.0.0.0/0
- Protocols: TCP (port 22)

Click "Create"

---

## Step 5: (Optional) Create VM Instances

Go to: Compute Engine → VM instances

Create two VMs:

VM 1:
- Region: us-central1
- Subnet: subnet-us

VM 2:
- Region: asia-east1
- Subnet: subnet-asia

---

## Step 6: Verification

- Confirm both subnets exist under the same VPC
- (Optional) SSH into one VM and ping the internal IP of the other

---

# Exp:2 Launch VM without External IP and Enable Private Google Access -->

## Step 1: Enable Private Google Access on Subnet

1. Open Google Cloud Console
2. Navigate to: VPC Network → VPC networks
3. Click your VPC (e.g., custom-vpc)
4. Select the subnet you want to use (e.g., subnet-us)
5. Click "Edit"

Configuration:
- Enable: Private Google Access

6. Click "Save"

---

## Step 2: Create VM without External IP

1. Navigate to: Compute Engine → VM instances
2. Click "Create Instance"

Configuration:

### Basic
- Name: vm-private
- Region: us-central1 (or desired region)
- Zone: us-central1-a (or matching zone)

### Networking
- Click "Networking"
- Network: custom-vpc
- Subnet: subnet-us

### External IP
- Set: None

3. Click "Create"

---

## Step 3: Verify Internal Connectivity

1. Open the VM details page
2. Confirm:
   - No external IP is assigned
   - Internal IP is present

---

## Step 4: Test Private Google Access (Optional)

1. Use SSH via browser (Identity-Aware Proxy will be used automatically)
2. Run:
   curl https://www.googleapis.com

3. Successful response confirms Private Google Access is working


# Exp:3 Create Cloud Storage Bucket, Upload Objects, and Configure Lifecycle Rule -->

## Step 1: Create a Cloud Storage Bucket

1. Open Google Cloud Console
2. Navigate to: Cloud Storage → Buckets
3. Click "Create"

Configuration:
- Name: unique-bucket-name
- Location type: Region
- Location: us-central1 (or desired region)
- Storage class: Standard

Leave other settings as default

4. Click "Create"

---

## Step 2: Upload Objects

1. Open the created bucket
2. Click "Upload Files" or "Upload Folder"
3. Select files from your local system
4. Wait for upload to complete

---

## Step 3: Configure Lifecycle Management Rule

1. Inside the bucket, go to the "Lifecycle" tab
2. Click "Add rule"

Configuration:

### Action
- Select: Set storage class to Nearline

### Condition
- Age: 30 days

3. Click "Create"

---

## Step 4: Verify Lifecycle Rule

1. Go to the "Lifecycle" tab
2. Confirm rule is listed:
   - Action: Set storage class to Nearline
   - Condition: Age = 30 days



# Exp:4 Deploy Cloud SQL (MySQL) Instance and Configure for Scalability & Reliability -->

## Step 1: Create Cloud SQL Instance

1. Open Google Cloud Console
2. Navigate to: SQL → Databases
3. Click "Create Instance"
4. Select: MySQL

Configuration:

### Instance Info
- Instance ID: mysql-instance
- Root password: set a secure password

### Region and Zonal Availability
- Region: us-central1 (or desired region)
- Zonal availability: Multiple zones (High Availability)

### Machine Configuration
- Choose a suitable machine type (e.g., db-custom or preset based on workload)

### Storage
- Enable: Automatically increase storage

5. Click "Create Instance"

---

## Step 2: Create Database

1. Open the created instance
2. Go to "Databases" tab
3. Click "Create database"

Configuration:
- Database name: app-db

Click "Create"

---

## Step 3: Create User

1. Go to "Users" tab
2. Click "Add user account"

Configuration:
- Username: app-user
- Password: set password

Click "Add"

---

## Step 4: Configure Connectivity

1. Go to "Connections" tab

### Internal Access (Recommended)
- Ensure Private IP is enabled (for secure internal communication)

### External Access (Optional)
- Add authorized networks only if needed

Click "Save" if changes made

---

## Step 5: Verify Instance

1. Check instance status is "Running"
2. Confirm:
   - High availability enabled
   - Storage auto-increase enabled
   - Database and user created



# Exp:5 Launch VM with Startup Script to Install Apache -->

## Step 1: Create VM Instance

1. Open Google Cloud Console
2. Navigate to: Compute Engine → VM instances
3. Click "Create Instance"

Configuration:

### Basic
- Name: vm-apache
- Region: us-central1 (or desired region)
- Zone: us-central1-a

### Machine
- Choose default or small machine type (e.g., e2-micro)

### Boot Disk
- Image: Debian or Ubuntu

---

## Step 2: Add Startup Script

1. Scroll to "Advanced options"
2. Open "Management" tab
3. Find "Startup script" field

Paste the following script:
#!/bin/bash
apt-get update
apt-get install -y apache2
systemctl start apache2
systemctl enable apache2

---

## Step 3: Configure Network Access

1. Go to "Networking" section
2. Enable:
   - Allow HTTP traffic

---

## Step 4: Create Instance

1. Click "Create"
2. Wait for VM to start

---

## Step 5: Verify Apache Installation

1. After VM is running, copy the External IP
2. Open browser and visit:

   http://EXTERNAL_IP

3. Apache default page should load


# Exp:6 Create Instance Template and Managed Instance Group with Autoscaling -->

## Step 1: Create Instance Template

1. Open Google Cloud Console
2. Navigate to: Compute Engine → Instance templates
3. Click "Create Instance Template"

Configuration:

### Basic
- Name: web-template
- Machine type: e2-micro (or as required)

### Boot Disk
- Image: Debian or Ubuntu

### Networking
- Enable: Allow HTTP traffic

### (Optional) Startup Script
- Go to "Advanced options" → "Management"
- Add script to install Apache:
#!/bin/bash
apt-get update
apt-get install -y apache2
systemctl start apache2
systemctl enable apache2


4. Click "Create"

---

## Step 2: Create Managed Instance Group (MIG)

1. Navigate to: Compute Engine → Instance groups
2. Click "Create Instance Group"

Configuration:

### Type
- Select: New managed instance group (stateless)

### Basic
- Name: web-mig
- Location: Multiple zones (recommended for reliability)

### Template
- Select: web-template

### Autoscaling
- Turn ON autoscaling

Settings:
- Autoscaling metric: CPU utilization
- Target CPU utilization: 60%
- Minimum number of instances: 1
- Maximum number of instances: 5 (or as required)

3. Click "Create"

---

## Step 3: Verify Deployment

1. Open the instance group
2. Confirm:
   - Instances are running
   - Autoscaling is enabled
   - Target CPU is set to 60%

---

## Step 4: (Optional) Test Autoscaling

1. Generate load on instances (e.g., using a load testing tool)
2. Monitor:
   - CPU usage increases
   - New instances are automatically added when CPU exceeds 60%

# Exp:7 Create Test User and Assign Storage Object Viewer Role for a Bucket -->

## Step 1: Create Test User

1. Open Google Cloud Console
2. Navigate to: IAM & Admin → IAM
3. Click "Grant Access"

Configuration:
- New principals: test-user@example.com (enter the test user's email)
- Role: leave empty for now or assign basic Viewer (optional)

4. Click "Save"

---

## Step 2: Assign Bucket-Level Permission

1. Navigate to: Cloud Storage → Buckets
2. Click the target bucket

3. Go to the "Permissions" tab
4. Click "Grant Access"

Configuration:
- New principals: test-user@example.com
- Role: Storage → Storage Object Viewer

5. Click "Save"

---

## Step 3: Verify Access

1. Confirm the user appears in the bucket permissions list
2. Ensure role is:
   - Storage Object Viewer
3. Scope should be limited to the selected bucket only


# Exp:8 Create Service Account with Compute Viewer Role and Attach to VM -->

## Step 1: Create Service Account

1. Open Google Cloud Console
2. Navigate to: IAM & Admin → Service Accounts
3. Click "Create Service Account"

Configuration:
- Name: compute-viewer-sa
- Service account ID: auto-generated
- Description: Service account for VM access to Compute resources

4. Click "Create and Continue"

---

## Step 2: Assign Role

1. In "Grant this service account access to project"

Configuration:
- Role: Compute → Compute Viewer

2. Click "Continue"
3. Click "Done"

---

## Step 3: Attach Service Account to VM (New VM)

1. Navigate to: Compute Engine → VM instances
2. Click "Create Instance"

Configuration:

### Basic
- Name: vm-with-sa
- Region/Zone: as required

### Identity and API Access
- Service account: select `compute-viewer-sa`
- Access scopes: Allow default access

3. Click "Create"

---

## Step 4: Attach Service Account to Existing VM (Optional)

1. Go to: Compute Engine → VM instances
2. Select the VM
3. Click "Edit"

### Identity and API Access
- Change Service account to `compute-viewer-sa`

4. Click "Save"

---

## Step 5: Verify

1. Open VM details page
2. Confirm attached service account is `compute-viewer-sa`
3. Ensure role assigned is:
   - Compute Viewer


# Exp:9 Create Folder and Assign IAM Roles (Policy Inheritance) -->

## Step 1: Create a Folder

1. Open Google Cloud Console
2. Navigate to: IAM & Admin → Manage Resources
3. Ensure you are at the Organization level (top of hierarchy)
4. Click "Create Folder"

Configuration:
- Name: test-folder

5. Click "Create"

---

## Step 2: Assign IAM Role at Folder Level

1. Click the created folder (test-folder)
2. Go to the "Permissions" tab
3. Click "Grant Access"

Configuration:
- New principals: user@example.com
- Role: Viewer (or any required role)

4. Click "Save"

---

## Step 3: Move Project into Folder (Optional for Inheritance)

1. In "Manage Resources", locate a project
2. Select the project
3. Click "Move"
4. Choose destination: test-folder
5. Confirm move

---

## Step 4: Verify Policy Inheritance

1. Open the project inside the folder
2. Go to: IAM & Admin → IAM
3. Check permissions

Expected:
- The user assigned at folder level appears in project IAM
- Role is inherited from the folder


# Exp:10 Deploy HTTP(S) Load Balancer with Cloud Armor Security Policy -->

## Step 1: Create Backend (Instance Group)

1. Open Google Cloud Console
2. Navigate to: Compute Engine → Instance groups
3. Create or use an existing Managed Instance Group (MIG) with HTTP enabled
4. Ensure instances are running and serving traffic (e.g., Apache on port 80)

---

## Step 2: Create HTTP(S) Load Balancer

1. Navigate to: Network Services → Load balancing
2. Click "Create Load Balancer"
3. Select: HTTP(S) Load Balancing → "Start configuration"

### Basic Configuration
- Name: http-lb
- Scheme: External (internet-facing)

### Backend Configuration
1. Click "Backend configuration"
2. Create backend service:
   - Name: web-backend
   - Backend type: Instance group
   - Select your MIG
   - Port: 80
3. Click "Done"

### Health Check
1. Create health check:
   - Name: http-health-check
   - Protocol: HTTP
   - Port: 80
2. Save

### Frontend Configuration
1. Click "Frontend configuration"
2. Protocol: HTTP
3. IP: Ephemeral (or reserve static IP)
4. Port: 80

### Routing
- Leave default (route all traffic to backend)

4. Click "Create"

---

## Step 3: Create Cloud Armor Security Policy

1. Navigate to: Security → Cloud Armor
2. Click "Create Policy"

Configuration:
- Name: web-security-policy
- Default rule: Allow

3. Click "Create"

---

## Step 4: Add Rule to Restrict Access

1. Open the created policy
2. Click "Add Rule"

Configuration example:
- Priority: 1000
- Action: Deny
- Condition: Source IP ranges (e.g., 0.0.0.0/0 to block all or specify ranges)

3. Click "Save"

---

## Step 5: Attach Policy to Backend Service

1. Go to: Network Services → Load balancing
2. Select your load balancer
3. Click backend service (web-backend)
4. Click "Edit"

### Security
- Enable: Cloud Armor security policy
- Select: web-security-policy

5. Click "Update"

---

## Step 6: Verify

1. Access load balancer IP in browser
2. Based on rules:
   - Allowed traffic should reach backend
   - Denied traffic should be blocked
