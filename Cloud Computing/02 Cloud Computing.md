# Major Components of AWS
#### 1. Computer Services
- Amazon EC2 : (Elastic Compute Cloud) : Virtual servers in the cloud for scalable compute capacity
- AWS Lambda : Serverless computing to run code without provisioning servers.
- Amazon Elastic Beanstalk : Platform as a Service PaaS for deploying and managing applications
- Amazon ECS (Elastic Container Service) : Container orchestration service for running Docker containers.
#### 2. Storage Services 
- Amazon S3 (Simple Storage Service) : Scalable object storage for data archiving, backups and more.
- Amazon EBS (Elastic Block Store) : Persistent block storage for EC2 instances
- Amazon Glacier : Low cost storage for data archiving and long term backups.
#### 3. Database Services 
- Amazon RDS (Relational Database Service) : Managed relational database service for MySQL, PostgreSQL, Oracle, etc.
- Amazon DynamoDB : Fully managed NoSQL database
#### 4. Networking and Content Delivery 
- Amazon VPC (Virtual Private Cloud) : Isolated cloud resources within a virtual network
- Amazon CloudFront : Content Delivery Network (CDN) to deliver content with low latency
- AWS Route 53 : Domain Name System and Traffic management service
#### 5. Security and Identity Management
- AWS IAM (Identity and Access Management) : Manage user access and permissions
- AWS Shield : DDoS protection service
- AWS WAF (Web Application Firewall) : Protects applications from common web exploits
#### 6. Management and Monitoring:
· AWS CloudWatch: Monitor applications and resources.
. AWS CloudTrail: Log AWS account activity for compliance.
#### 7. Developer Tools
. AWS CodePipeline: Automates the CI/CD workflow.
. AWS CodeBuild: Fully managed build service for compiling source code.
· AWS CodeDeploy: Automates software deployment.
#### 8. Machine Learning and AI
· Amazon SageMaker: Fully managed service for building and deploying ML models.
· Amazon Rekognition: Image and video analysis.


# Cloud Storage Architecture
With **Microsoft Azure** Storage Services example

##### 1. Blob Storage: 
- Stores unstructured data like images, videos, and large files.
- Example: A media company storing high-resolution videos for streaming.
##### 2. File Storage: 
- Provides fully managed shared file storage in the cloud.
- Example: A team sharing project files across multiple offices.
##### 3. Queue Storage: 
- Manages message queues for asynchronous communication.
- Example: A ride-hailing app queuing customer requests for processing.
##### 4. Table Storage: 
- Stores structured NoSQL data.
- Example: An loT system storing telemetry data from devices.
##### 5. Disk Storage: 
- Provides virtual hard drives for VMs.
- Example: A financial company running databases on Azure VMs.

#### Azure Storage Tiers : 
-  Azure provides different tiers to optimize storage costs based on usage patterns.
- **Hot Tier:** For frequently accessed data.
	Example: A SaaS app storing user activity logs for quick analysis.
- **Cool Tier:** For infrequently accessed data.
	 Example: Archiving marketing campaign data accessed quarterly.
- **Archive Tier:** For rarely accessed data (long-term storage).
	 Example: Storing compliance-related documents for 7+ years.

# OS Virtualization
- OS virtualization refers to creating virtual instances of operating systems that run concurrently on the same physical machine
- These instances are isolated from each other and managed by a virtualization layer, such as hypervisor (e.g. VMware, VirtualBox, or HyperV)
- **Host OS** : The primary operating system that runs the hypervisor
- **Guest OS** : The OS running inside the virtual machine
- Each virtual machine operates independently. One VM's crash or failure does not impact other VM.

Advantages : 
- Cost Efficiency
- Efficient Resource Utilization: Optimizes the use of CPU, memory and storage by sharing hardware resources among VMs
- Scalability : New VMs can be created quickly without buying additional hardware
- Flexibility and Portability : VMs can run different OSes on the same hardware. VMs can be moved between physical machines or clouds.
- Testing and Development: Developers can test software in isolated environments without risking the host OS.
- Disaster Recovery and Backup : Snapshots of VMs can be taken to restore states quickly after failures
- VMs can be replicated or migrated for backup purposes.


## Type 1 vs Type 2 Virualization 

| Type 1 Visualization                                                                                      | Type 2 Virtualization                                                                                 |
| --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| also called Bare Metal Hypervisor                                                                         | also called Hosted Hypervisor                                                                         |
| A  Type 1 hypervisor runs directly on the host's hardware, without needing an underlying operating system | A Type 2 hypervisor runs on top of a host operating system (OS) rather than directly on the hardware. |
| Ex : VMware ESXi, Microsoft Hyper-V, Xen Hypervisor, KVM                                                  | Ex : VMWare Workstation, Oracle VirtualBox, Parallels Desktop, QEMU                                   |
| High performance with direct hardware access                                                              | Lower performance, depends on host OS                                                                 |
| More complex setup, often used in data centers                                                            | Simpler setup, ideal for personal use                                                                 |
| Cloud provider, enterprise data centers, HPC uses                                                         | Development, testing, education uses                                                                  |
| Ideal Environment : Production servers with dedicated hardware                                            | Ideal Environment : Personal desktops and development setups                                          |

## Hypervisor : 
A hypervisor is software or firmware that allows you to create and manage multiple virtual machines (VMs) on a single physical machine (host).
It acts as a middle layer that allocates hardware resources (CPU, memory, storage) to each VM, ensuring they operate independently.
Examples:
1) VMware: Develops VMware ESXi.
2) Microsoft: Develops Hyper-V.
3) Oracle: Develops Oracle VM and VirtualBox.
4) Citrix: Develops XenServer.

### Hypervisor Ecosystem 
- **VMware vSphere** : VMware sphere is the core suite that includes ESXi and vCenter
- **vCenter Server** : It provides centralized **management** for multiple ESXi hosts and their VMs.
- **VMware vSAN**: vSAN virtualizes storage by pooling disks from multiple ESXi hosts into a shared datastore.
- **VMware NSX**: NSX provides software-defined **networking** (SDN), enabling virtual networks to be created, managed, and secured.
- **VMware Horizon**: Horizon delivers Virtual Desktop Infrastructure (VDI) and virtual applications to users.

### Different between Virtual Machines and Containers : 
