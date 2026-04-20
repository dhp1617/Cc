## EXPERIMENT 1: VPC NETWORKING (CUSTOM MODE – MULTI REGION)

### Step 1: Create Custom VPC Network
- Go to VPC Network → Create VPC  
- Name: custom-vpc  
- Subnet mode: Custom  

### Step 2: Create Subnet (us-central1)
- Name: subnet-us  
- Region: us-central1  
- IP range: 10.0.0.0/24  

### Step 3: Create Subnet (asia-east1)
- Name: subnet-asia  
- Region: asia-east1  
- IP range: 10.1.0.0/24  

### Step 4: Configure Firewall Rules
- Allow:  
  - SSH (22)  
  - ICMP (ping)  
- Source: 0.0.0.0/0  

### Step 5: Create VM Instances
- VM1 → us-central1  
- VM2 → asia-east1  
- Attach to respective subnets  

### Step 6: Test Connectivity
- SSH into VM  
- Ping internal IP of other VM  
- Verify communication across regions  


## EXPERIMENT 2: CREATING COMPUTE ENGINE VM (NO EXTERNAL IP)

### Step 1: Open Compute Engine
- Go to Google Cloud Console  
- Navigate to Compute Engine → VM Instances  

### Step 2: Create VM Instance
- Click Create Instance  
- Enter name: private-vm  
- Select region and zone  
- Choose machine type (e2-medium)  

### Step 3: Remove External IP
- Go to Networking → Network Interfaces  
- Set External IPv4 Address → None  
- Save settings  

### Step 4: Enable Private Google Access
- Go to VPC Network → Subnets  
- Select subnet → Click Edit  
- Enable Private Google Access  
- Save  

### Step 5: Connect to VM
- Use SSH via IAP or Cloud Shell  

### Step 6: Verify Connectivity
- Access Google APIs from VM  
- Confirm no external IP is assigned  


## EXPERIMENT 3: CREATING CLOUD STORAGE BUCKET

### Step 1: Open Cloud Storage
- Go to Google Cloud Console  
- Navigate to Cloud Storage → Buckets  

### Step 2: Create Bucket
- Click Create Bucket  
- Enter unique name (project-id)  
- Select location (US/EU/ASIA)  
- Choose storage class: Standard  
- Click Create  

### Step 3: Upload Object
- Open created bucket  
- Click Upload Files  
- Select file (e.g., image)  
- Upload successfully  

### Step 4: Make Object Public (Optional)
- Select file → Permissions  
- Add: allUsers → Storage Object Viewer  

### Step 5: Configure Lifecycle Rule
- Go to Lifecycle tab  
- Click Add Rule  
- Condition: Age = 30 days  
- Action: Set Storage Class → Nearline  
- Save rule  

### Step 6: Verify Configuration
- Check lifecycle rule  
- Confirm automatic transition after 30 days  


## EXPERIMENT 4: IMPLEMENTING CLOUD SQL

### Step 1: Create Cloud SQL Instance
- Go to Cloud SQL → Create Instance  
- Select MySQL  
- Enter:
  - Instance ID: wordpress-db  
  - Version: MySQL 8.0  
  - Set root password  
- Select region and zone  
- Choose machine type (1 vCPU, 3.75 GB RAM)  
- Enable Private IP  
- Click Create  

### Step 2: Create Database
- Open instance → Databases  
- Click Create Database  
- Name: wordpress  

### Step 3: Configure Cloud SQL Proxy
- Go to Compute Engine → VM  
- SSH into VM (wordpress-proxy)  
- Download proxy  
- Make executable  
- Start proxy using connection name  

### Step 4: Connect Application using Proxy
- Open browser using VM external IP  
- Enter database details:
  - Database: wordpress  
  - User: root  
  - Password: (root password)  
  - Host: 127.0.0.1  

### Step 5: Connect using Private IP
- Copy Cloud SQL private IP  
- Open another VM  
- Configure:
  - Host = Private IP  
- Verify connection  

### Step 6: Verify Application
- Open VM external IP  
- Check if application loads successfully  


## EXPERIMENT 5: CLOUD MONITORING

### Step 1: Create VM Instance
- Go to Compute Engine → VM Instances  
- Click Create Instance  
- Name: lamp-1-vm  
- Select machine type: e2-medium  
- Enable HTTP traffic  
- Click Create  

### Step 2: Install Apache using Startup Script / SSH
- Connect to VM using SSH  
- Update packages  
- Install Apache and PHP  
- Restart Apache server  

### Step 3: Verify Web Server
- Copy external IP  
- Open in browser  
- Check Apache default page  

### Step 4: Install Monitoring Agent
- Run installation script  
- Enable monitoring and logging  

### Step 5: Create Uptime Check
- Go to Monitoring → Uptime checks  
- Create HTTP check using VM IP  
- Set frequency = 1 minute  

### Step 6: Create Alert Policy
- Select metric (network traffic / CPU)  
- Set threshold  
- Add notification (email)  

### Step 7: Create Dashboard
- Add widgets (CPU load, network traffic)  
- Monitor VM performance  

### Step 8: View Logs
- Go to Logging → Logs Explorer  
- Filter VM logs  
- Observe start/stop events  


## EXPERIMENT 6: RESOURCE MONITORING (MIG & AUTOSCALING)

### Step 1: Create Instance Template
- Go to Compute Engine → Instance Templates  
- Click Create Template  
- Set machine type (e2-medium)  
- Add startup script (install web server)  
- Save template  

### Step 2: Create Managed Instance Group (MIG)
- Go to Instance Groups  
- Click Create Instance Group  
- Select template  
- Set group type → Managed  
- Define region/zone  
- Set initial number of instances  

### Step 3: Configure Autoscaling
- Enable autoscaling  
- Set:
  - Metric → CPU utilization  
  - Target → 60%  
- Save configuration  

### Step 4: Test Load
- Connect to instance via SSH  
- Generate CPU load  
- Observe scaling behavior  

### Step 5: Monitor Scaling
- Go to Monitoring  
- Check CPU usage and instance count  
- Verify scaling up/down  


## EXPERIMENT 7: EXPLORING IAM

### Step 1: Open IAM Console
- Go to Google Cloud Console  
- Navigate to IAM & Admin → IAM  

### Step 2: Create Cloud Storage Bucket
- Go to Cloud Storage → Buckets  
- Click Create Bucket  
- Enter unique name  
- Upload a sample file (sample.txt)  

### Step 3: Create Test User
- In IAM → Click Grant Access  
- Enter new user email (Test User)  
- Assign role: Viewer (initially)  

### Step 4: Remove Default Access
- Remove Project Viewer role from test user  
- Verify user cannot access resources  

### Step 5: Assign Storage Object Viewer Role
- Click Grant Access  
- Enter Test User  
- Select role:
  - Cloud Storage → Storage Object Viewer  
- Click Save  

### Step 6: Verify Access
- Login as Test User  
- Access Cloud Storage  
- Verify:
  - Can view bucket and objects  
  - Cannot upload/delete files  


## EXPERIMENT 8: SERVICE ACCOUNT AUTHENTICATION (COMPUTE VIEWER ROLE)

### Step 1: Create Service Account
- Go to IAM & Admin → Service Accounts  
- Click Create Service Account  
- Name: compute-viewer-sa  
- Click Create  

### Step 2: Assign Role
- Select role:
  - Compute Engine → Compute Viewer  
- Click Continue → Done  

### Step 3: Create VM Instance
- Go to Compute Engine → VM Instances  
- Click Create Instance  
- Set machine type (e2-medium)  
- Under Security → Attach service account  
- Select compute-viewer-sa  
- Click Create  

### Step 4: Connect to VM
- SSH into VM  
- Access resources using service account  

### Step 5: Verify Permissions
- List compute instances  
- Try modifying resource (will fail due to read-only access)  

### Step 6: Cloud Storage Access (Concept Link)
- Service account can access storage only if roles are assigned  
- Demonstrates role-based access control  


## EXPERIMENT 9: FOLDER-LEVEL IAM & POLICY INHERITANCE

### Step 1: Create Folder
- Go to Resource Manager  
- Click Create Folder  
- Enter folder name (e.g., test-folder)  

### Step 2: Assign IAM Role at Folder Level
- Open folder → IAM  
- Click Grant Access  
- Add user  
- Assign role (e.g., Viewer / BigQuery User)  
- Save  

### Step 3: Verify Policy Inheritance
- Create or move project under folder  
- Login as assigned user  
- Verify access to project resources  

### Step 4: BigQuery Setup
- Open BigQuery  
- Create dataset: billing_dataset  
- Create table: sampleinfotable  
- Import billing data from Cloud Storage  

### Step 5: Run Queries
- Execute SQL queries to analyze billing data  
- Filter and group data  

### Step 6: Custom Role (Concept)
- Create custom role (Instance Operator)  
- Add permissions:
  - compute.instances.start  
  - compute.instances.stop  
  - compute.instances.get  


## EXPERIMENT 10: HTTP(S) LOAD BALANCER WITH CLOUD ARMOR

### Step 1: Configure Firewall for Health Check
- Go to VPC → Firewall Rules  
- Create rule:
  - Allow TCP:80  
  - Source IP ranges:
    - 130.211.0.0/22  
    - 35.191.0.0/16  

### Step 2: Create NAT Configuration
- Go to Network Services → Cloud NAT  
- Create Cloud Router  
- Configure NAT Gateway  

### Step 3: Create Web Server VM
- Create VM without external IP  
- Install Apache web server  
- Enable auto-start  

### Step 4: Create Custom Image
- Stop VM  
- Create image from disk  

### Step 5: Create Instance Template
- Use custom image  
- Configure machine type  
- Disable external IP  

### Step 6: Create Managed Instance Groups (MIG)
- Create MIG in two regions  
- Enable autoscaling  
- Set load balancing utilization  

### Step 7: Configure Load Balancer
- Go to Network Services → Load Balancing  
- Create HTTP Load Balancer  
- Configure:
  - Frontend → IP + Port 80  
  - Backend → MIGs  
  - Health check  

### Step 8: Configure Cloud Armor
- Create security policy  
- Add rules to allow/deny traffic  
- Attach policy to backend service  

### Step 9: Test Load Balancer
- Access Load Balancer IP in browser  
- Perform stress testing  
- Monitor traffic distribution
