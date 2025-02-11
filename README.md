# AWS-CLOUD-SOLUTION-FOR-2-COMPANY-WEBSITES-USING-REVERSE-PROXY-TECHNOLOGY


## Let's Get Started

Great work so far! We've been implementing various web solutions while gaining hands-on experience with a range of DevOps tools. In previous projects, we utilized foundational Infrastructure as a Service (IaaS) offerings from AWS, such as EC2 (Elastic Compute Cloud) for virtual machines and EBS (Elastic Block Store) for storage. Additionally, we’ve learned to configure Key Pairs and Security Groups to enhance system security.

However, the true power of cloud computing goes beyond simply renting virtual machines. Moving forward, you'll begin to explore advanced cloud concepts and tools using AWS as an example. Don’t worry—these principles are widely applicable across other major cloud platforms like Microsoft Azure and Google Cloud Platform.

>Note: The next few projects will be implemented manually. It's essential to first understand how to build solutions step-by-step in the cloud before diving into automation. This foundational knowledge will make the process of programming automation much smoother and less frustrating.

For this project, we'll design and build a secure infrastructure within an AWS VPC (Virtual Private Cloud) network for a fictitious company. Feel free to come up with a creative name for the company! Their setup includes two main websites:

1. A WordPress CMS for the company’s primary business website.
2. A Tooling Website for the DevOps team. (Repository link: https://github.com/<your-name>/tooling)

To enhance security and performance, the company has opted to integrate NGINX reverse proxy technology into the architecture.
Project Goals:

   * Cost-Effectiveness: Ensure the infrastructure is budget-friendly while meeting performance requirements.
   * Security: Build a robust and secure system.
   * Scalability: Design the solution to handle traffic increases and web server failures effectively.

By implementing the architecture outlined, we'll deliver a resilient, secure, and scalable solution tailored to meet the company’s needs.

![image](https://github.com/user-attachments/assets/9985cd95-6bb7-432d-b690-04da87499522)


## Starting with your AWS Cloud Project.

### Prerequisites for the Project

Before you begin, make sure the following requirements are met to set up your AWS environment properly:

1. Configure Your AWS Account and OU:
   * Log in to your AWS Management Console.
   * Navigate to Organizations under the Services menu.
   * Click Create Organization and ensure you enable All Features.
   * Once created, proceed to add accounts and organize them into units (e.g., Dev OU).

2. Create the AWS Master Account (Root Account):
   * Go to AWS Sign-Up and create an AWS account using your primary email address.
   * Provide payment details and verify your account. 

3. Create a Sub-Account for DevOps:
   * In the Root account, go to AWS Organizations.
   * Click on Add Account > Create Account.
   * Provide a unique email address for the new account.
   * Name the account DevOps and wait for AWS to complete the account creation.

![image](https://github.com/user-attachments/assets/9801b636-68eb-41e7-bd16-98c4f47cd388)   

    
4. Set Up the Dev Organizational Unit (OU):
   * In AWS Organizations, click Organizational Units.
   * Create a new OU and name it Dev.
      

5. Move the DevOps Account into the Dev OU:
   * Go to Accounts under AWS Organizations.
   * Locate the DevOps account.
   * Drag and drop it into the Dev OU or use the Move Account option.

![image](https://github.com/user-attachments/assets/958f4d6d-c68f-4b9a-b333-6dccb32f5611)

6. Log In to the DevOps Account:
   * Go to the AWS Sign-In page.
   * Sign in as root email
   * Use the email address and password you provided during the DevOps account creation process.
   * Click on Forget Password
   * A link to create a new password would be sent to you.
   * Now you're logged in your DevOps account that is in your Dev OU

![image](https://github.com/user-attachments/assets/6e98cdb0-ea80-4e6f-a8f3-584106982b6a)


7. Create a free domain name for your fictitious company using [Cloudns free domian hosting site](https://www.cloudns.net/main/)
   * Signup and Login your account

![image](https://github.com/user-attachments/assets/5d916b0f-87f1-47aa-ae7f-01ff565d64ca)

   * Click on Dashboard > DNS hosting and follow the steps on the image below

![image](https://github.com/user-attachments/assets/de505922-119a-499c-842f-eaad84a68d34)

![image](https://github.com/user-attachments/assets/b1aa9abc-cfc1-4c3b-9a92-041e901b1bd0)


8. Create a hosted zone in AWS, and map it to your free domain using [Cloudns free domian hosting site](https://www.cloudns.net/main/).
   * From your Management console in AWS, type Route 53 in the search bar and select Route 53 from the results.
   * In the Route 53 dashboard, click on Hosted zones in the left-hand navigation pane.
   * Click Create hosted zone Fill in the following details:
     - Domain name: Enter your Freenom domain name (for example,  adebowaletest.ip-ddns.com).
     - Comment: Optional.
     - Type: Select Public hosted zone.
     - VPC: Leave this blank for a public hosted zone.
     - Click Create hosted zone

![image](https://github.com/user-attachments/assets/adcf0db6-444b-42d7-9814-f9aa9fc4a41d)

![image](https://github.com/user-attachments/assets/af180ea9-3e5c-45b5-bad6-25d22d7cfcea)


## SETUP A VIRTUAL PRIVATE NETWORK(VPC)

Set Up a Virtual Private Network (VPC). Always make reference to the architectural diagram below and ensure that your configuration is aligned with it.

![image](https://github.com/user-attachments/assets/9985cd95-6bb7-432d-b690-04da87499522)

1. Create a VPC:
   * Sign in to the AWS Management Console.
   * Navigate to the VPC from the search bar.

![image](https://github.com/user-attachments/assets/177a06b1-3cfe-4202-a5ab-22c7d9aedfb5)
     
   * Click on Your VPCs in the sidebar, then click Create VPC.
   * Fill in the following details:
     - Name Tag: Your_VPC_Name (e.g., adebowaletest)
     - IPv4 CIDR block: 10.0.0.0/16 (or any range based on your architecture).

![image](https://github.com/user-attachments/assets/077ac42f-c33b-4c3e-b125-ca461da5d608)
 
  * Click Create VPC.
    
  * Enable DNS resolution and DNS hostnames from VPC settings.

![image](https://github.com/user-attachments/assets/30bce831-7163-4d69-a407-1a3dbcb8ed76)    


![image](https://github.com/user-attachments/assets/b9515bec-99a1-4760-bdd7-f40097986f74)


2. Create Subnets based on the architecture. Ensure proper segmentation for public and private subnets across availability zones (AZs).

   * Public Subnet 1 (Availability Zone A):
     - CIDR Block: 10.0.1.0/24
     - Availability Zone: A
     - Name: Public-Subnet-1
       
![image](https://github.com/user-attachments/assets/9f9c9bb2-50c8-4e04-97d8-07328745035b)

![image](https://github.com/user-attachments/assets/f2c7f529-9ffe-4fd3-a130-6529afe02f1d)


   * Private Subnet 1 (Availability Zone A):
     - CIDR Block: 10.0.2.0/24
     - Availability Zone: A
     - Name: Private-Subnet-1
       
  ![image](https://github.com/user-attachments/assets/3713a31f-2976-4b85-827c-a157093b4991)

  ![image](https://github.com/user-attachments/assets/2a35da3f-0eb6-44ee-b590-0187e785590d)
  

   * Public Subnet 2 (Availability Zone B):
     - CIDR Block: 10.0.3.0/24
     - Availability Zone: B
     - Name: Public-Subnet-2

![image](https://github.com/user-attachments/assets/296dfdf4-3551-42d6-a290-1aad2a187178)

![image](https://github.com/user-attachments/assets/37d31861-55ef-422e-9191-26a61ad3f5bf)

   * Private Subnet 2 (Availability Zone B):
     - CIDR Block: 10.0.4.0/24
     - Availability Zone: B
     - Name: Private-Subnet-2

![image](https://github.com/user-attachments/assets/bf306b7b-96e5-417e-821a-a83190a3cfb1)

     
   * Private Subnet 3 (Availability Zone A):
     - CIDR Block: 10.0.5.0/24
     - Availability Zone: A
     - Name: Private-Subnet-3
 
 ![image](https://github.com/user-attachments/assets/d9266c3a-8919-4cf5-a03a-c132ce7ff55c)
    
   * Private Subnet 4 (Availability Zone B):
     - CIDR Block: 10.0.6.0/24
     - Availability Zone: B
     - Name: Private-Subnet-4

![image](https://github.com/user-attachments/assets/593afb82-76e7-4a45-8d68-a71335eeb220)


You should have them created and ready to go!
![image](https://github.com/user-attachments/assets/1054da25-bc53-4d9e-b3aa-02bf4659d573)


3. Create Route Tables:
   
   * Public Route Table:
     - Navigate to Route Tables.
     - Create a new route table and name it `Public-Route-Table`.
     - Associate Public-Subnet-1 and Public-Subnet-2.
     - Edit routes to include an Internet Gateway (which would be created in Step 4).
       
![image](https://github.com/user-attachments/assets/83f43925-2172-4f24-ada8-8702e5e62ee1)

![image](https://github.com/user-attachments/assets/4d6028bc-0aa0-4e38-8235-490bab1e9e38)




   * Private Route Table:
     - Create a new route table and name it `Private-Route-Table`.
     - Associate Private-Subnet-1, Private-Subnet-2, Private-Subnet-3, and Private-Subnet-4.
     - Do not add an Internet Gateway (because you do not want attackers to attack your private subnets).

![image](https://github.com/user-attachments/assets/f83eb1b9-f0d8-4ead-a246-a99cfdf84415)


![image](https://github.com/user-attachments/assets/a11accef-3a9f-4eea-83d2-c1bb255df889)


4. Create an Internet Gateway (IGW):
   * Go to Internet Gateways.
   * Click Create Internet Gateway.
   * Name: `Main-Internet-Gateway`.
   * Attach the Internet Gateway to the VPC created earlier.

![image](https://github.com/user-attachments/assets/15142c90-2087-47c1-938a-01d9a88fc2c8)

![image](https://github.com/user-attachments/assets/4ab24bf4-38a2-4ae5-9a89-4800d3c992fa)

![image](https://github.com/user-attachments/assets/1387c705-8bc0-46c9-8ca4-0a82be882b08)

![image](https://github.com/user-attachments/assets/409c2e72-4bd8-4714-a6fa-9d5b86545be9)


5. Edit Public Route Table to Enable Internet Access:
   * Go to the Public-Route-Table.
   * Add a route: Destination: 0.0.0.0/0
   * Target: Internet Gateway (IGW)

![image](https://github.com/user-attachments/assets/402e7da3-baa9-4729-8f24-5993b3064def)


6. Create Elastic IPs:
   * Navigate to Elastic IPs under EC2.
   * Allocate 3 Elastic IPs:
     - Assign one to the NAT Gateway.
     - Reserve two for Bastion hosts.


![image](https://github.com/user-attachments/assets/1a3bac2b-559f-402b-947e-eae785c98ab1)

![image](https://github.com/user-attachments/assets/4faa8eaa-1773-483f-abc2-c5131215360c)



7. Create NAT Gateway from VPC dashboard:
   * Create a new NAT Gateway and assign it to Public-Subnet-1.
   * Use one of the Elastic IPs allocated earlier.
   * Edit the Private-Route-Table to add a route for 0.0.0.0/0 with target being NAT Gateway.

![image](https://github.com/user-attachments/assets/d358520a-2ebc-4d4b-8517-12b9be47af47)

![image](https://github.com/user-attachments/assets/0b0f36a1-6ee9-4b33-ae6c-ba66c9cb9fb2)

![image](https://github.com/user-attachments/assets/4bf74dd4-2e3c-4296-83f3-b959bb295e06)

![image](https://github.com/user-attachments/assets/8b6e34bc-34d3-44c0-a474-27a5b8d3d28f)

## Quick on, What does a NAT-Gateway do to Private subnets?

A NAT Gateway (Network Address Translation Gateway) enables instances in a private subnet to access the internet while keeping those instances hidden from direct internet access. Here's how it works:

A. Outbound Internet Access: Instances in a private subnet, which don't have public IP addresses, cannot directly access the internet. The NAT Gateway allows these instances to initiate outbound traffic (such as downloading updates or accessing external services) by translating their private IP addresses to the NAT Gateway's public IP address.
B. Security: The NAT Gateway ensures that the instances in the private subnet are not exposed to incoming traffic from the internet. It only allows outbound traffic initiated from within the subnet and returns responses to those requests.
C. Internet Communication: When an instance in the private subnet sends a request to the internet, the NAT Gateway forwards the request to the internet using its own public IP address. When the response comes back, the NAT Gateway forwards it to the correct instance in the private subnet by mapping the traffic back to the instance's private IP address.

In summary, a NAT Gateway provides secure, one-way internet access for instances in a private subnet, allowing them to reach the internet without exposing them to external traffic.



8. Create Security Groups:
   * Nginx Servers:
     - Allow access only from an Application Load Balancer (ALB).
     - Use a placeholder for now; update once ALB is configured.

![image](https://github.com/user-attachments/assets/765b38fe-c604-44b3-aaa9-be4701e1e9eb)

   * Bastion Servers:
     - Allow SSH access only from your workstation.
     - To get your IP: Run `curl www.canhazip.com`.

![image](https://github.com/user-attachments/assets/3a1e05c2-7ce9-4514-9056-a373f825e1af)

![image](https://github.com/user-attachments/assets/4d6e9656-0fd0-44fe-82bb-ac0978ad76f3)


   * Application Load Balancer:
     - Allow access from the Internet (0.0.0.0/0).

![image](https://github.com/user-attachments/assets/6e04b732-97be-432d-ad92-e3165474e88a)


   * Web Servers:
     - Allow access only from Nginx servers.
     - Use a placeholder for now; update once Nginx servers are configured.

![image](https://github.com/user-attachments/assets/104b480d-8da2-4270-8dda-0ba1bf26bf02)


   * Data Layer:
     - RDS: Allow access only from Web Servers.
     - EFS: Allow access from both Nginx and Web Servers.

![image](https://github.com/user-attachments/assets/28714405-9564-4c03-874e-5c483bd46761)

## Setting up compute resources

Now, We proceed With **Compute Resources** You will need to set up and configure compute resources inside your VPC. The recources related to compute are:

1. EC2 Instances
2. Launch Templates
3. Target Groups
4. Autoscaling Groups
5. TLS Certificates
6. Application Load Balancers (ALB)


## Set Up Compute Resources for Nginx Provision EC2 Instances for Nginx

1. Launch EC2 Instances: Create an EC2 Instance based on CentOS Amazon Machine Image (AMI) in any 2 Availability Zones (AZ) in any AWS Region (it is recommended to use the Region that is closest to your customers). Use EC2 instance of T2 family (e.g. t2.micro or similar)
   
   * Log in to AWS Console and navigate to the EC2 Dashboard.

   * Click Launch Instances and configure the following:
       - Name and Tags: Assign meaningful names (e.g., Nginx-Instance-1 and Nginx-Instance-2).
![image](https://github.com/user-attachments/assets/866a8291-a56b-48aa-a767-9d50cc3c8aba)

       - AMI: Select a CentOS AMI (e.g., "CentOS 7" from the AWS Marketplace).
![image](https://github.com/user-attachments/assets/b970b61c-0456-4807-af16-78e7d1c09499)

       - Instance Type: Use t2.micro or a similar instance type.
![image](https://github.com/user-attachments/assets/26bf14dd-2b21-4db8-852f-38311e01106b)

       - Key Pair: Choose your existing key pair or create a new one for SSH access.

   * Configure Instance Network:
       - VPC: Select your existing VPC.
       - Subnet: Choose a subnet in one Availability Zone for the first instance and another subnet in a different Availability Zone for the second instance.
       - Auto-assign Public IP: Enable this if you plan to SSH directly or require external access during setup.
       - Security Group: Select your existing Security Group in the VPC.
![image](https://github.com/user-attachments/assets/2e24f04b-b4fb-4108-92bb-c00b7beee339)

   * Storage:
       - Allocate at least 8 GB of root volume storage.
       - For larger applications, consider adding extra storage volumes.
         
   * Review and Launch:
       - Confirm the details and click Launch to provision the instances.
![image](https://github.com/user-attachments/assets/0661c46c-b461-4f15-9bfa-d5994559691c)
*Nginx instances 1 & 2 has been created.*


2. Ensure that it has the following software installed:
   * python
   * ntp
   * net-tools
   * vim
   * wget
   * telnet
   * epel-release
   * htop

## Take the following steps to achieve the installation of the software above

### A. SSH into Instances: Use your key pair to SSH into each EC2 instance:- 

```
ssh -i /path/to/key.pem ec2-user@<Public-IP-of-Instance>

```
![image](https://github.com/user-attachments/assets/720fe877-4cb5-4f73-ab22-245f653cbe07)

### B. Install Required Software. Run the following commands on each instance to install the required packages:

* ***Step 1: Update the System. Before installing the required software, ensure your system is updated:***

```
sudo yum update -y
```
This updates all packages on the instance.

![image](https://github.com/user-attachments/assets/828dc8c0-e4eb-4758-89af-ddb7401bac80)


* ***Step 2: Install Python3. To install Python3:***

```
sudo yum install -y python3
```

![image](https://github.com/user-attachments/assets/56cfe559-3af0-4299-8173-55be421ddad5)


- Verification: Confirm installation with:

```
    python3 --version
```

![image](https://github.com/user-attachments/assets/d0af9eae-e9ab-44e0-8afc-f49ddf129594)


* ***Step 3: Install Chrony (for NTP). CentOS 9 uses chrony for time synchronization instead of ntp. Install it by running:***

```
sudo yum install -y chrony
```

Start and Enable Chrony:

```
sudo systemctl start chronyd
sudo systemctl enable chronyd
```

- Verification: Confirm service is running with:

```
sudo systemctl status chronyd
```

Check time synchronization:

```
    chronyc tracking
```

![image](https://github.com/user-attachments/assets/3488e5f2-c4c9-44b9-9b75-a864cd13a799)


* ***Step 4: Install Net-tools. Install the necessary network tools:***

```
sudo yum install -y net-tools
```

![image](https://github.com/user-attachments/assets/377f9951-237a-47d3-b7b9-bd9aa038e6b7)
    
- Verification: Confirm installation with:

```
ifconfig
```
![image](https://github.com/user-attachments/assets/c749cfb5-38ef-4583-ad39-d5db069cb9a6)


* ***Step 5: Install Vim. Install the Vim editor:***

```
sudo yum install -y vim
```

![image](https://github.com/user-attachments/assets/10763b17-7cd5-4018-9df3-72225d9ec403)

    
- Verification: Confirm installation with:

```
    vim --version
```

![image](https://github.com/user-attachments/assets/ec6e27b9-34da-470f-97d2-8655240123df)


* ***Step 6: Install Wget. Install Wget for downloading files from the web:***

```
sudo yum install -y wget
```

![image](https://github.com/user-attachments/assets/78d37f3c-47c3-443c-b0ec-33e5127a67bd)

    
- Verification: Confirm installation with:

```
    wget --version
```

![image](https://github.com/user-attachments/assets/554dc75b-b103-4bd7-80a1-682fe76207b3)


* ***Step 7: Install Telnet. Install Telnet to test network connectivity:***

```
sudo yum install -y telnet
```

![image](https://github.com/user-attachments/assets/77801cfa-d3ef-4ad3-8484-3cdf1f0bc1bf)

- Verification: Confirm installation with:

```
telnet
```

- To test connectivity, use:

```
    telnet 8.8.8.8 53
```

![image](https://github.com/user-attachments/assets/42b0ef81-9f8c-4513-8103-4a9fb7b61a66)


* ***Step 8: Install EPEL-release. The EPEL repository provides additional packages for CentOS. To install it:***

```
sudo yum install -y epel-release
```

![image](https://github.com/user-attachments/assets/b4d7cea4-344d-4c08-90ca-caad4f8c7291)
  
- Verification: Confirm installation with:

```
    yum repolist
```


* ***Step 9: Install Htop. Install Htop for monitoring system performance:***

```
sudo yum install -y htop
```

![image](https://github.com/user-attachments/assets/2cbe193b-1b38-4856-b6c6-52afa0d3c598)
    
- Verification: Confirm installation with:

```
    htop
```

![image](https://github.com/user-attachments/assets/492127e9-8747-41df-82a4-e9bc551e7db9)


* ***Step 10: Troubleshooting. If you encounter issues like connection timeouts or failed installations:***
    - Check the instance's network settings, including security groups and route tables.
    - Ensure that there are no firewall rules blocking the connection.
    - If yum repositories are not reachable, ensure that the instance has internet access (e.g., correct routing, VPC internet gateway).
    - If any software doesn’t install due to missing dependencies or errors, check the output messages for clues, and consider enabling additional repositories (like EPEL). 



### C. Create the AMI from the EC2 Instance

* ***Follow these steps to create an AMI:***

    - Log in to the AWS Management Console. Go to the EC2 Dashboard.   

    - Locate the Instance to Create the AMI From.
        - Navigate to Instances under the EC2 section.
        - Select the EC2 instance you've set up (e.g., the one configured with Nginx and other dependencies).

    - Initiate the AMI Creation Process.
        Click on the Actions dropdown at the top.
        Navigate to Image and templates > Create Image.
![image](https://github.com/user-attachments/assets/f171747c-8b1a-486e-ac8d-7639f632caa3)


    - Fill in the Image Details.
        - Image Name: Enter a descriptive name for the AMI (e.g., `Nginx-Server-AMI`).
        - Image Description: Provide a brief description (e.g., AMI of CentOS Stream 9 configured with Nginx and required dependencies).
        - No Reboot: Leave unchecked unless you want the instance to remain running during the AMI creation (not recommended as it might result in an inconsistent image).

    - Add Storage (Optional).
        - Review or modify the storage settings for the AMI. Typically, the default settings are sufficient.

    - Create the AMI.
        Click Create Image.

![image](https://github.com/user-attachments/assets/ec534c6a-2c1a-4637-9b07-9167291178e0)

Once done, AWS will start creating the AMI. This process might take a few minutes, depending on the size of your instance.

```
The AMI you created earlier is based on the EC2 instance where you installed and configured the required dependencies, such as python, nginx, vim, telnet, and others.
This AMI is essentially a snapshot of that instance, which allows you to reuse the pre-installed configuration and software without needing to set them up again on new instances.
Why Use That AMI for the Launch Template?
    - It saves time: No need to reinstall dependencies (nginx, etc.) manually.
    - Ensures consistency: Every instance launched from this AMI will have the same setup.
    - Simplifies automation: Pre-configured AMIs reduce the need for additional configurations in the User Data script.

When creating the launch template for Nginx, use this AMI as the base image, so any instance launched with the template will already have the essential configurations in place. The User Data script in the template will handle specific tasks like starting and enabling Nginx.

```
---
## Prepare launch template for Nginx 

***Step 1: Create a Launch Template***

1. Go to the AWS Console: Navigate to EC2 Dashboard > Launch Templates.

2. Create a New Launch Template:
    - Click Create Launch Template.
    - Launch Template Name: Give it a name like `nginx-launch-template`.
    - Template Version Description: Add something descriptive like `Template for Nginx instances with pre-installed dependencies`.

![image](https://github.com/user-attachments/assets/493a5448-5587-446e-9d7d-9eb2b776828c)

3. Select the AMI: Under Amazon Machine Image (AMI), search for and select the AMI you created earlier.

![image](https://github.com/user-attachments/assets/673e5f0a-dd1d-435c-9a76-cb05f906e03d)


4. Instance Type: Choose an appropriate instance type (e.g., t2.micro for testing or based on your workload).



5. Key Pair: Select an existing key pair for SSH access, or leave it blank if you're using a bastion host for access.

![image](https://github.com/user-attachments/assets/4c9dc7b9-87cf-4d7c-8049-f9031a2c3086)

7. Finalize the Template:
   * Review all configurations.
   * Click **Create Launch Template**.

8. Launch Nginx Instance in Public Subnet 2
   * Go to EC2 Dashboard → Launch Templates.
   * Select Nginx-Launch-Template.
   * Click Actions → Launch Instance from Template.

![image](https://github.com/user-attachments/assets/1362b848-aa5f-4bb9-8099-605d1d41b9aa)

   * Configure Instance Details:
     - Subnet: Select Public Subnet 2.
     - Security Group: Ensure SSH (22), HTTP (80), and HTTPS (443) are allowed.   
    
   * Click "Launch Instance" and wait for it to initialize.  
![image](https://github.com/user-attachments/assets/fee8f9af-d817-4c78-a5df-6f6e31d3b0eb)

***Verify that the Instance from the Launch Template is Running and Accessible***    

1. **Verify Instance is Running**
   * Navigate to EC2 Dashboard:
     - Go to the EC2 Dashboard in the AWS Management Console.
     - Under Instances, check the Status of your instance created from the launch template. The status should be running.
        
![image](https://github.com/user-attachments/assets/0e70857b-6891-493d-95e7-3950bba5b9b1)

2. **Verify Instance has a Public IP Address**
   * In the EC2 Dashboard, select the instance.
   * Look under the Public IPv4 address field.
     - Ensure it has a public IP address assigned.
     - If there is no IP assigned, you'll need to associate an Elastic IP with the instance.
        
3. **Verify Security Group Settings**
   * Check the Security Group assigned to the instance:
     - Make sure it has rules that allow inbound traffic on SSH (port 22), HTTP (port 80), and HTTPS (port 443).
     - Confirm that the source IP for these rules is appropriate (e.g., 0.0.0.0/0 for public access or a specific IP range for restricted access).        

![image](https://github.com/user-attachments/assets/19d9a98f-665e-48c7-9ea3-130444e03477)

4. **Verify Nginx Installation**
   - SSH into the instance:

```
ssh -i <your-key-pair.pem> ec2-user@<instance-public-ip>
```

  - Check Nginx status:

```
sudo systemctl status nginx
```    

5. **Verify Nginx is Accessible via HTTP**

  - Locally (on the instance):
        - Run `curl -I http://localhost` to confirm that the Nginx server is responding locally.
        - You should see a 200 OK response.

![image](https://github.com/user-attachments/assets/6d29d8fe-8137-4827-be58-3155cbb0340b)

  - From a Browser:
         - Open a web browser and type the public IP of your instance (e.g., http://<instance-public-ip>).
         - You should see the Nginx welcome page if everything is set up correctly.

![image](https://github.com/user-attachments/assets/8f306864-5c1a-40fc-80eb-5418b246efba)

## Next step is to create a target group. We need to create an ALB first.

1. ***Create the Load Balancer:***

    * Go to the AWS Console:
          - Navigate to EC2 Dashboard > Load Balancers.
          - Choose Create Load Balancer.
![image](https://github.com/user-attachments/assets/a2f4f97a-bc09-45bc-9b8f-b12d4a47787f)

     * Choose the Load Balancer Type:
          - You can choose between Application Load Balancer (ALB) or Network Load Balancer (NLB) based on your use case.
          - Since you're handling HTTP/HTTPS traffic (Nginx), Application Load Balancer (ALB) should work well.
![image](https://github.com/user-attachments/assets/928311ad-e0bd-47bc-93f8-6f29c5a1214a)

     * Configure Load Balancer Settings:
          - Name: Give the Load Balancer a descriptive name (e.g., Nginx-ALB).
          - Scheme: Select Internet-facing.
          - Listeners: Ensure HTTP (port 80) and/or HTTPS (port 443) listeners are selected depending on your configuration.

2. ***Obtain an SSL certificate***
   * Navigate to AWS Certificate Manager (ACM): In the AWS Management Console, search for Certificate Manager.

![image](https://github.com/user-attachments/assets/329cb560-b822-4645-be31-4e2c2dc5047f)

   * Request a Public SSL Certificate:
     - Click on Request a certificate.
     - Select Request a public certificate and click Next.
![image](https://github.com/user-attachments/assets/c7a85f3c-3980-4feb-be6e-c7a409be74c2)

![image](https://github.com/user-attachments/assets/f7d1590c-929e-4e41-95c3-a6b55029c8ce)

![image](https://github.com/user-attachments/assets/88f7db31-c4c2-4308-b27d-44941a9ae0fe)
    
![image](https://github.com/user-attachments/assets/d45fa1ea-e5e7-46d3-97e8-d8cd36116f54)
    

3. ***Back to creating the ALB***
   * **Here, you’ll need to set some basic settings:**
     - Name: Give your ALB a name (e.g., Nginx-ALB).
     - Scheme: Choose internet-facing if you want it to be accessible from the internet.
     - IP Address Type: Choose ipv4.
     - Listeners: Make sure you select the HTTPS listener for port 443 (because we’ll secure traffic using TLS).
    
*Kindly follow through with the images below*

![image](https://github.com/user-attachments/assets/4f8c19f2-82ae-4316-b29b-225237a535db)

![image](https://github.com/user-attachments/assets/0425f632-6e61-4f4b-99f9-e816403b3fb2)

![image](https://github.com/user-attachments/assets/42232b32-f250-47a8-9025-6d72a6255ce2)

![image](https://github.com/user-attachments/assets/b8275117-a865-447f-976f-83f950db2012)


***Before we finally create the ALB, we would need to attach a target group. Follow through with the steps below***

![image](https://github.com/user-attachments/assets/35da4810-52d4-4ce3-887d-b0e7da92285e)

![image](https://github.com/user-attachments/assets/ee4ec4eb-88ee-47a9-a028-b04078324cf6)

![image](https://github.com/user-attachments/assets/bcdde4f9-d843-4e2e-a5aa-28df0cd16b17)

![image](https://github.com/user-attachments/assets/4e29d81f-0ed1-4287-bbf7-942dffc25fb3)

![image](https://github.com/user-attachments/assets/dbbd336c-4c51-4a11-9ac1-16ed69ef0c12)

***Now we have the passed the health check status for the target groups***


## Now we Set-up the Auto-Scaling group for Nginx on EC2 with ALB

***Step-by-Step Guide to Setting Up Auto Scaling for Nginx on AWS EC2 with Application Load Balancer***

1. Configure Auto Scaling Group (ASG):
        * Create an Auto Scaling Group with the launch template, public subnets, and desired capacity.
        * Select the ALB as the load balancer.
        * Attach your EC2 instances to the Auto Scaling Group.
        * Enable health checks from both EC2 and ALB for better monitoring and fault tolerance.
        * Set desired capacity to 2, minimum capacity to 2, and maximum capacity to 4.
        * Configure scaling policies: Scale out when CPU utilization exceeds 90%.
        * Optionally, disable scale-in to avoid shrinking your infrastructure in this setup.

2. Set Up Scaling Policies:
    * Target Tracking Scaling Policy:
        - Configure a target tracking scaling policy based on the CPU Utilization metric, setting the target value to 90%.
        - Enable scale-out to automatically increase the instance count when CPU usage reaches 90%. This ensures your infrastructure can scale dynamically.
    * Disable Scale-In: Optionally, You can choose to disable the scale-in policy to ensure that only scaling out is triggered. This is useful in scenarios where you want to handle increased load without shrinking capacity. However, this is not compulsory.

3. Create and Configure SNS Notifications:
    * Create SNS Topic:
        - Go to the SNS console and create a new Standard Topic.
        - Name the topic (e.g., AutoScalingNotifications).
        - Create a Subscription to the topic (e.g., via email) to receive notifications.
    * Subscribe to SNS Topic:
        - After creating the subscription, confirm it through the link sent to your email or other contact method.
    * Configure SNS with Auto Scaling:
        - In the Auto Scaling configuration, under Notification, choose Add Notification.
        - Select your SNS Topic to send notifications related to scaling events (such as instance launch, terminate, etc.).

5. Verify Configuration
    * Monitor Auto Scaling:
        - Check the Auto Scaling Group's Activity History for any scaling activities.
        - Monitor the ALB to ensure healthy instances are serving traffic.

### See the images below for step by step process:

![image](https://github.com/user-attachments/assets/b4e37da0-a9c4-4563-ab5f-f7f6af4970a2)

![image](https://github.com/user-attachments/assets/fd351f60-001a-455a-83ee-9a6cbb70a959)

![image](https://github.com/user-attachments/assets/27dc2ed8-3bc4-4530-aec1-e83485da97e8)

![image](https://github.com/user-attachments/assets/8bfbd206-1335-49dc-acb4-586c6e1a4181)

![image](https://github.com/user-attachments/assets/9d070cd1-21f4-4819-88d2-edc656be3686)

![image](https://github.com/user-attachments/assets/2525d066-a4e5-4872-bf4b-37ab11f21351)

![image](https://github.com/user-attachments/assets/c6070f14-80c0-445d-a88c-bf3105b52dd5)

![image](https://github.com/user-attachments/assets/d9cf27b1-93a5-4f1f-be83-47c11f739caa)

![image](https://github.com/user-attachments/assets/6d41674a-f152-4433-ad5d-25f8e8bbab7a)

    
## Now, We setup the compute resources for the bastion server

***1. Provision the EC2 Instances for Bastion***
    
**Step 1: Launch EC2 Instance**

 1. Go to AWS EC2 Console
    Navigate to EC2 > Instances and click Launch Instance.

2. Choose an Amazon Machine Image (AMI)
        Select CentOS Amazon Machine Image (AMI).
        Ensure it matches the Region and Availability Zone (AZ) where your Nginx server is located.

3. Choose an Instance Type
        Select an instance type such as t2.micro (if using free tier) or t3.small for better performance.

4. Configure Instance Details
        Number of Instances: 1 (repeat for another AZ)
        Network: Select your VPC.
        Subnet: Choose the public subnet in the same AZ as your Nginx server.
        Auto-assign Public IP: Enable

5. Add Storage
        Root volume: Minimum 8 GB (Increase as needed).
        Select General Purpose SSD (gp3).

7. Configure Security Group
        Allow SSH (22) access from a trusted IP or your organization's VPN.
        (Optional) Restrict SSH to a specific CIDR range.

8. Review and Launch
        Select or create a key pair for SSH access.
        Click Launch Instance.

![image](https://github.com/user-attachments/assets/6c1bbc6c-f92b-4fc4-8636-f6db49a560cf)

![image](https://github.com/user-attachments/assets/a8864197-b0c6-4050-873c-d6fe13db3553)


**Step 2: Ensure that it has the following software installed**
- python
- ntp
- net-tools
- vim
- wget
- telnet
- epel-release
- htop

1. Connect to the Bastion Host. Use SSH to connect to your Bastion EC2 Instance:

```
ssh -i your-key.pem ec2-user@<Bastion_Public_IP>
```

2. Update the System

```
sudo yum update -y
```

3. Install Required Packages. Run the following command to install all required software:

```
sudo yum install -y python3 ntp net-tools vim wget telnet epel-release htop
```

4. Start and Enable NTP

```
sudo systemctl start ntpd
sudo systemctl enable ntpd
```

5. Verify Installations. Check if software is installed correctly:

```
python3 --version
ntpq -p
vim --version
wget --version
telnet --version
htop --version
```

![image](https://github.com/user-attachments/assets/36cb6f6c-6eba-471d-b5af-ab8f6782518f)

![image](https://github.com/user-attachments/assets/50db983b-a98e-4eb3-bc26-be6dab53af45)


## Next up is to associate an Elastic IP with the Bastion EC2 Instances we previously created then create an AMI out of the Bastion EC2 instance.

**Step 1: Associate an Elastic IP with Each Bastion EC2 Instance**

1. Allocate an Elastic IP Address
    - Open AWS Management Console and go to EC2 Dashboard.
    - In the left panel, click on Elastic IPs under Network & Security.
    - Click Allocate Elastic IP address.
    - Choose Amazon’s pool of IPv4 addresses and click Allocate.
    - Copy the allocated Elastic IP for later use.

2. Associate the Elastic IP with a Bastion EC2 Instance.
    - Still on the Elastic IPs page, select the IP address you just allocated.
    - Click Actions → Associate Elastic IP address.
    - Under Instance, select one of your Bastion EC2 Instances.
    - Leave Private IP as default (or select the primary private IP).
    - Click Associate.

![image](https://github.com/user-attachments/assets/f3f4ebf1-dc21-40e2-8f02-2e4d8e19a731)


***Step 2: Create an AMI from the Bastion EC2 Instance***

1. Create the AMI.
    - Open AWS Management Console and go to EC2 Dashboard.
    - Click Instances on the left panel.
    - Select your Bastion EC2 instance that you want to create an AMI from.
    - Click Actions → Image and templates → Create Image.
    - In the Create Image window:
        * Image name: Give it a descriptive name (e.g., Bastion-AMI-v1).
        * Image description: (Optional) Add details like OS type or purpose.
        * No reboot: Keep it unchecked (unless you want to avoid downtime).
    - Click Create Image.

2. Verify AMI Creation.
    - Go to EC2 Dashboard → Click AMIs under Images.
    - Look for your newly created AMI (it will be in a Pending state at first).
    - Once it changes to Available, your AMI is ready!

![image](https://github.com/user-attachments/assets/30212b2c-b5c8-4ddf-ad3d-d38c475b2d84)


***Step 3: Prepare Launch Template for Bastion (One per Subnet)***
1. Create a Launch Template
    - Open AWS Management Console → Go to EC2 Dashboard.
    - On the left panel, click Launch Templates.
    - Click Create launch template.
    - Enter Template Name (e.g., `Bastion-Launch-Template-SubnetA`).
    - Under Source template, select No template (since we're creating a new one).

![image](https://github.com/user-attachments/assets/8a7bf41d-4c46-4a3a-8091-019bd3a629e1)

2. Use the AMI You Created
    - Under Amazon Machine Image (AMI), click Browse AMIs.
    - Select the AMI you created earlier.

![image](https://github.com/user-attachments/assets/55cafdd0-ef46-4628-8ae7-7645f6b20518)

3. Configure Instance Settings
    - Instance type: Select an instance type (e.g., t2.micro or as required).
    - Key pair (login): Choose an existing key pair or create a new one.
    - Network settings:
        * VPC: Select your existing VPC.
        * Subnet: Choose a Public Subnet (we need public access for Bastion).
        * Auto-assign Public IP: Enabled (for external access).

4. Assign Security Group
    - Click Create a new security group (or choose an existing one).
    - Add the following inbound rules:
        * SSH (Port 22) → Source: Your trusted IP or Anywhere (0.0.0.0/0, ::/0). In this case, we would be allowing traffic from anywhere.

5. Configure User Data (Automate Setup on Launch)
    - Scroll down to Advanced details → Find User data section.
    - Enter the following User Data script to update yum and install Ansible & Git:

```
#!/bin/bash
# Update system packages
sudo yum update -y

# Install EPEL repository (required for Ansible)
sudo yum install -y epel-release

# Install Ansible and Git
sudo yum install -y ansible git

```
   - Click Create launch template.

![image](https://github.com/user-attachments/assets/796d3b37-3242-4c72-a485-95359171e082)

6. Create a second instance from the launch template. You don’t necessarily need a second launch template if both Bastion instances will have the same configuration.

**When to Use a Single Launch Template:**
>✅ If both instances share the same AMI, instance type, security group, and user data, you can launch multiple instances from the same launch template, just selecting different subnets during launch.
When a Second Launch Template is Needed:

>❌ If each Bastion instance requires different configurations (e.g., different instance types, security settings, or user data), then creating separate launch templates makes sense.
What You Can Do Instead of a Second Template:

- Go to Launch Template → Actions → Launch Instance from Template.
- Set the Number of Instances to 1.
- Select different public subnets for each instance.

7. Verify Installed Dependencies. Run the following commands inside the Bastion instance:

- Check if git is installed:

```
git --version
```

- If installed, you should see output like:

```
git version 2.x.x
```

- Check if Ansible is installed:

```
ansible --version
```

- If installed, you should see output like:

```
ansible 2.x.x
```

- Check if yum update ran successfully:

```
sudo yum list updates
```

![image](https://github.com/user-attachments/assets/445a7437-b3c6-4a8a-a6a6-1d35ef45f888)

>If there are no critical pending updates, it means the update command in User Data worked.
>***Troubleshoot if Packages are Missing:***
>If git or ansible is not installed, you can check the User Data execution logs:

```
sudo cat /var/log/cloud-init-output.log
```
>This log will show if there were any issues running the User Data script during instance launch.


***Configure Target Groups***

Step 1: Create a Target Group for Bastion Instances
    - Open AWS Management Console → Go to EC2 Dashboard.
    - On the left side, scroll down and click Target Groups under Load Balancing.
    - Click Create target group.

Step 2: Configure the Target Group
    - Target type: Select Instances.
    - Protocol: Select TCP.
    - Port: Enter 22 (for SSH).
    - VPC: Choose the VPC where your Bastion instances are located.
    - Health checks:
        * Set Protocol to TCP.
        * Set Port to 22.
        * For Health check path, leave it empty since it's TCP-based.

Step 3: Register Bastion Instances as Targets
    - After creating the target group, select it from the Target Groups list.
    - Click Targets under the Description tab.
    - Click Edit.
    - Select the Bastion instances that were launched via the template (ensure they are in running state).
    - Click Add to registered.

Step 4: Verify Health Check
    - After registering the targets, verify the health check status under the Target Group details.
    - The health check should eventually pass for your Bastion instances. If it doesn’t, make sure the instances are properly configured to accept inbound SSH traffic on port 22 and that they are running.


***Configure Autoscaling for Bastion***
1. Select the Right Launch Template
    - Go to EC2 Console > Auto Scaling Groups > Create Auto Scaling Group.
    - For Launch Template, select the one you previously created for the Bastion EC2 instances.

2. Select the VPC
    - Choose the VPC where your Bastion instances are deployed.

3. Select Both Public Subnets
    - Choose the two public subnets in your VPC where you want your Bastion instances to be launched. These subnets should have public IPs to ensure external access.

4. Enable Application Load Balancer for the Auto Scaling Group (ASG)
    - In the Load Balancer section, enable Application Load Balancer (ALB) for the Auto Scaling group.
    - Select the ALB you created earlier and ensure it’s attached.

5. Select the Target Group You Created Earlier
    - For Target Group, select the one you created earlier for the Bastion servers.

6. Health Checks for Both EC2 and ALB
    - Ensure you have both EC2 health check and ALB health check enabled. This way, your instances will be monitored for both instance health and load balancer health.

7. Set Desired Capacity to 2
    - Set the Desired Capacity to 2. This means you want two instances running by default.

8. Set Minimum Capacity to 2
    - Set the Minimum Capacity to 2, so that there will always be at least two instances running.

9. Set Maximum Capacity to 4
    - Set the Maximum Capacity to 4. This allows the Auto Scaling Group to scale up to four instances when needed.

10. Set Scaling Policy (Scale out if CPU utilization reaches 90%)
    - Set a Scaling Policy to scale out when CPU utilization reaches 90%. This ensures that additional instances will be launched when the load gets too high.

11. Ensure an SNS Topic for Scaling Notifications
    - Make sure you have an SNS topic to send scaling notifications when instances are launched or terminated due to scaling.
        * If you don’t have an SNS topic, you can create one directly in the Auto Scaling setup process.

**Step-by-Step Execution**
    - Navigate to Auto Scaling Groups in the EC2 console.
    - Click on Create Auto Scaling Group.
    - Follow the steps listed above for selecting the Launch Template, VPC, Subnets, Load Balancer, and Target Group.
    - Set the desired, minimum, and maximum capacities.
    - Set the Scaling Policy to use CPU utilization of 90%.
    - Add an SNS Topic for scaling notifications.

Once everything is set up, the Auto Scaling Group will manage your Bastion instances automatically based on the load!

## Setup Compute resources for Webservers
Now, you will need to create 2 separate launch templates for both the WordPress and Tooling websites

1. Create an EC2 Instance (Centos) each for WordPress and Tooling websites per Availability Zone (in the same Region).
2. Ensure that it has the following software installed
   - python
   - ntp
   - net-tools
   - vim
   - wget
   - telnet
   - epel-release
   - htop
   - php
  
3. Create an AMI out of the Ec2 instance.

***Steps Taken:***
1. Set Up EC2 Instances in Private Subnets:
   - Created 2 separate EC2 instances (CentOS) for WordPress and Tooling websites in 2 different Availability Zones within the same Region.
   - Instances were placed in private subnets.
   - Software Installation via User Data Script:



2. For both instances, we used a User Data script to automate the installation of the required software packages, including:
- python
- ntp
- net-tools
- vim
- wget
- telnet
- epel-release
- htop
- php

3. We made necessary adjustments to the User Data script to cater for the CentOS environment (such as using chrony instead of ntp for time synchronization).
- AMI Creation for Both Instances: After configuring the software on both the WordPress and Tooling EC2 instances, we created AMIs from both instances. These AMIs will serve as the base for future deployments.

***Challenges Faced:***

1. Accessing Instances in Private Subnets:

**- Challenge #1:** Private Subnet Instances Lack Direct Internet Access
Since the instances were placed in private subnets, they didn't have public IPs and thus couldn't be accessed directly from the internet.
**Solution:** We had to set up a Bastion Host in a public subnet to act as a bridge for SSH access to the instances in private subnets.

**- Challenge #2:** SSH Key Issues
Initially, there were problems with accessing the private instances via SSH because the key pair wasn't properly set up on the Bastion Host.
**- Solution:** We made sure that the private key (.pem file) was correctly placed in the ~/.ssh/ directory on the Bastion Host, with the proper permissions (chmod 400).

**- Challenge #3:** Incorrect IP/Connection Errors
There were connection errors when attempting to SSH into the private instances from the Bastion Host, due to incorrect key paths or permissions.
**- Solution:** Ensured the correct use of the key when SSH’ing through the Bastion Host, using the command:
```
ssh -i ~/.ssh/Webserverkey.pem ec2-user@<private-instance-ip>
```

***Verification of Installation:***
1. We verified the installation of all required packages on both the WordPress and Tooling EC2 instances using commands like:
python --version
yum list installed
php -v
systemctl status httpd
Troubleshooting Errors:

2. We faced issues where certain packages (e.g., htop, php, mariadb) were either not installed correctly or caused issues with service startup. These errors were addressed by:
Clearing package cache with yum clean all.
Re-running the installation commands.
Checking for space availability, as mariadb installation issues were suspected to be related to disk space.
Moving Forward:


## All these steps has been repeated multiple times throughtout the course of this project. By now, you should have known how to:
1. Create an AMI.
2. Launch an instance from an AMI.
3. Create an autoscaling group.
4. Launch an instance from a launch template and adding a user data using shebang.
5. Create a Load Balancer.
6. Understand how Private and Public instances works.
7. Get a better knowledge of bastion hosts etc.


## Deploy an internal ALB for both webservers(wordpress and tooling server)

**Step 1: Create an Internal Application Load Balancer (ALB)**
1. Go to AWS Console → EC2
2. In the left menu, click Load Balancers
3. Click Create Load Balancer
4. Choose Application Load Balancer (ALB)
5. Set Load Balancer Name (e.g., internal-alb)
6. Under Scheme, select Internal
7. Under IP Address Type, choose IPv4
8. Under VPC, select your existing VPC
9. Under Availability Zones, select the Private Subnets where your Web Servers are deployed(Private subnet 1&2 in this case)
10. Click Next: Configure Security Settings

Since the webservers are configured for auto-scaling, there is going to be a problem if servers get dynamically scalled out or in. Nginx will not know about the new IP addresses, or the ones that get removed. Hence, Nginx will not know where to direct the traffic.

To solve this problem, we must use a load balancer. But this time, it will be an internal load balancer. Not Internet facing since the webservers are within a private subnet, and we do not want direct access to them.

![image](https://github.com/user-attachments/assets/29786c75-97bc-410d-9f54-befdd3692496)

![image](https://github.com/user-attachments/assets/b1b64b38-cd6a-4f6b-a978-8199c3254bb3)

### NOTE: This process must be repeated for both WordPress and Tooling websites.

## Create an Amazon Elastic File System.

***Here is a step-by-step guide to setting up Amazon Elastic File System (EFS) and mounting it on both Nginx and Webservers to store data.***

---

### **1. Create an Amazon EFS Filesystem**

**Steps**:
1. **Log into AWS Management Console** and go to the **EFS** service.

![image](https://github.com/user-attachments/assets/960e6cb6-b71f-444e-833a-c152b7391764)

2. Click on **Create file system**.

3. **Configure the file system settings**:
   - Choose the **VPC** where your web servers are located.
   - Leave the **performance mode** and **throughput mode** at the default values unless your workload requires specific settings.
   - Select **One zone** or **General purpose** as needed.
4. Click on **Create** to create the EFS filesystem.

![image](https://github.com/user-attachments/assets/4f1fb377-d1b9-4125-8e7f-8742a92d5a92)

![image](https://github.com/user-attachments/assets/e68efebf-0b7d-4a24-b477-dded481403ea)

---

### **2. Create an EFS Mount Target per Availability Zone (AZ)**

**Steps**:
1. After the EFS filesystem is created, go to the **File Systems** page in the EFS Console.
2. Click on the filesystem you just created.
3. Under **Network**, select the **Mount Targets** tab.
4. Click **Add mount target**.
5. For each Availability Zone (AZ) in your VPC, perform the following:
   - **VPC Subnet**: Choose the subnet associated with your private web servers.
   - **Security group**: Associate the security group created for the data layer.
6. Click **Create**.

Repeat these steps for each subnet in your VPC to ensure a mount target is available in each Availability Zone.

![image](https://github.com/user-attachments/assets/26f9b764-ce73-4457-8550-62bd51e2d076)

---

### **3. Associate Security Groups for Data Layer**

When you create mount targets, ensure that the **Security Group** for your data layer is selected. This security group should allow inbound traffic on **NFS port (2049)** so the web servers can access the EFS filesystem.

**Steps**:
1. Go to **VPC** in the AWS Console.
2. Under **Security Groups**, create a security group for the data layer if you haven’t already.
3. Add a **rule** to allow inbound traffic on port **2049** for **NFS**.
4. Attach this security group to the mount targets created for the EFS filesystem.

---

### **4. Create an EFS Access Point**

**Steps**:
1. In the **EFS Console**, select your filesystem.
2. Under the **Access points** tab, click on **Create access point**.

![image](https://github.com/user-attachments/assets/a8731b37-aafb-430d-a54f-fbb692945548)


3. Provide a **name** for the access point. You can leave all other settings at their default values.
4. Select **Create**.

![image](https://github.com/user-attachments/assets/44dfddd9-f39f-4118-aa1b-00e83b85e5ec)

---

### **5. Mount EFS on Nginx and Webservers**

**On Nginx Server:**
1. SSH into the Nginx instance.
2. Install the **NFS client** package if it is not already installed:
   ```
   sudo yum install -y nfs-utils
   ```
   ![image](https://github.com/user-attachments/assets/8c5d1091-1c12-4329-a64e-fc7e67d0cfd7)

3. Create a directory to mount the EFS filesystem:
   ```
   sudo mkdir /mnt/efs
   ```
4. Mount the EFS filesystem using the **mount target DNS** of the appropriate AZ (you can get this DNS from the EFS console):
   ```
   sudo mount -t nfs4 <EFS_MOUNT_TARGET_DNS>:/ /mnt/efs
   ```
5. To mount EFS automatically after reboot, add it to the **/etc/fstab** file:
   ```
   echo "<EFS_MOUNT_TARGET_DNS>:/ /mnt/efs nfs4 defaults,_netdev 0 0" | sudo tee -a /etc/fstab
   ```

**On Web Server:**
1. SSH into the Web server instance.
2. Install the **NFS client** package if it’s not installed:
   ```
   sudo yum install -y nfs-utils
   ```
3. Create a directory to mount the EFS filesystem:
   ```
   sudo mkdir /mnt/efs
   ```
4. Mount the EFS filesystem similarly:
   ```
   sudo mount -t nfs4 <EFS_MOUNT_TARGET_DNS>:/ /mnt/efs
   ```
5. Edit **/etc/fstab** to auto-mount after reboot:
   ```
   echo "<EFS_MOUNT_TARGET_DNS>:/ /mnt/efs nfs4 defaults,_netdev 0 0" | sudo tee -a /etc/fstab
   ```

---

### **6. Verify EFS Mounting**

After mounting the EFS filesystem on both web servers:

1. **Check the mount**:
   ```
   df -h
   ```
   You should see the EFS filesystem mounted at `/mnt/efs`.

2. **Test File Access**:
   - On both the Nginx and Web servers, create a file to test if it’s shared across instances:
     ```
     sudo touch /mnt/efs/testfile.txt
     ```
   - Check if the file is visible from both the Nginx and Web servers.

---

### **Conclusion**

By following these steps, you have:
- Created an EFS filesystem.
- Set up mount targets for each Availability Zone.
- Associated appropriate security groups to allow NFS traffic.
- Created an EFS access point for simplified access management.
- Mounted the EFS filesystem on both the Nginx and Web servers.

The EFS is now successfully integrated into your architecture, providing shared storage for your web servers.





### **Amazon RDS Setup for MySQL**

In this guide, we will walk through the setup of an **Amazon Relational Database Service (RDS)** MySQL instance with Multi-AZ support to ensure high availability and failover protection. We will also include encryption using **KMS (Key Management Service)** for security. This setup will be done on two availability zones (AZs), with the potential to scale to three AZs as needed.

---

### **Prerequisite: Create a KMS Key for Encryption**
Before setting up your RDS database, you must create a KMS key to encrypt the database instance.

**Steps**:
1. Go to the **KMS Console** in AWS.
2. Select **Create Key**.
3. Choose **Symmetric Key** and click **Next**.
4. Enter a key name and description (e.g., `RDS-Encryption-Key`).
5. Set the **Key Administrative Permissions** (you can assign administrative users who will manage the key).
6. Set the **Key Usage Permissions** to allow the necessary users and roles to use the key.
7. Finish the creation and ensure you take note of the **Key ARN** (Amazon Resource Name) for later use during RDS instance creation.

---

### **1. Create a Subnet Group for RDS**

A **DB Subnet Group** is needed to specify which subnets Amazon RDS can use to deploy the database instance. You should use private subnets in your data layer for security.

**Steps**:
1. In the **RDS Console**, go to the **Subnet Groups** section.
2. Click **Create DB Subnet Group**.
3. Provide a **name** for the subnet group (e.g., `my-rds-subnet-group`).
4. Choose your **VPC** and select at least two **private subnets** in different availability zones for high availability.
5. Save the subnet group.

---

### **2. Create an RDS MySQL Instance**

**Steps**:
1. Go to the **RDS Console** and click on **Create database**.
2. Select **Standard Create**.
3. Choose **MySQL** as the engine type and select **MySQL 8.x.x**.
4. Under **Version**, select the desired version (e.g., **8.x**).
5. Select **Do not create a standby instance** under **Availability & Durability** to minimize costs for test environments (if you want multi-AZ for production, select **Create a standby instance**).
6. Under **Settings**:
   - Provide a **DB instance identifier** (e.g., `my-rds-instance`).
   - Enter the **Master username** (e.g., `admin`).
   - Provide the **Master password** and confirm it.
7. For **DB Instance Size**:
   - Select the instance class based on your requirements (e.g., `db.t3.micro` for testing purposes).
   - Set the **Allocated Storage** (e.g., 20 GB for testing purposes).
8. **VPC & Subnet**:
   - Ensure the **VPC** selected is the same as the one you created the subnet group in.
   - Select the **DB Subnet Group** created earlier.
9. **VPC Security Groups**:
   - Select the appropriate **security group** for the RDS instance to ensure it is not accessible from the internet. This should only allow connections from your web and application servers.
10. **Backup**:
    - Set the **Backup Retention Period** (e.g., 7 days).
    - Enable **Automated backups** for better data protection.
11. **Encryption**:
    - Enable **Encryption** using the KMS key created earlier.
    - Select the **KMS Key** that you created for encryption.
12. **Monitoring**:
    - Enable **Enhanced Monitoring** to get insights into DB instance performance.
    - Enable **CloudWatch Logs Export** for **Error Logs** and **Slow Query Logs**.
13. After reviewing the configuration, click **Create database**.

---

### **3. Final Configuration & Security**

Once your RDS instance is created, ensure the following configurations are in place for security and operational efficiency:

- **VPC Security**:
  - Ensure that your RDS instance is located in private subnets to restrict internet access.
  - Update your **Security Groups** to allow traffic only from the web and application servers that need to access the RDS instance.
  
- **Backup & Retention**:
  - The backup policy you configured will automatically create snapshots of your RDS instance on a regular basis.
  - Review the **backup retention period** to make sure it meets your requirements.

- **CloudWatch Monitoring**:
  - Ensure that **CloudWatch Monitoring** is enabled for metrics like CPU utilization, memory usage, and I/O operations.
  - Additionally, **Error Logs** and **Slow Query Logs** are exported for monitoring and troubleshooting.

---

### **4. Review Costs and Decommissioning**

- **Cost Considerations**:
  - Amazon RDS is a managed service, but it can incur significant costs depending on the instance type, storage size, and the additional features you enable.
  - Always monitor your monthly usage and costs in the **AWS Billing Dashboard** to ensure that you're not exceeding your budget.
  - **Do not leave services running for long periods** if they are not needed, especially in a test environment.

- **Decommissioning**:
  - Once the RDS instance is no longer needed, ensure to **delete the instance** and associated resources like automated backups and snapshots to prevent ongoing charges.

---

### **Conclusion**

This setup provides a **highly available MySQL database** running on **Amazon RDS** with **encryption** and **CloudWatch monitoring**. The **multi-AZ** configuration ensures that the database is resilient to failures of a single Availability Zone, and the use of **KMS** ensures data security.

With these steps, you are now equipped with a fully managed, scalable, and secure database for your application workloads.

### **Configuring DNS with Route 53 for WordPress and Tooling Website**

In this phase, we ensure that the WordPress **main domain** and the **subdomain for the Tooling website** are correctly configured in **Amazon Route 53** for proper accessibility.

---

## **1. Create Required DNS Records in Route 53**
You previously registered a **free domain** with **Freenom** and created a **hosted zone** in **Route 53**. Now, you'll create additional **DNS records** to correctly route traffic.

### **a. Create an Alias Record for the Root Domain**
This step ensures that your **WordPress website** is accessible via your main domain.

1. **Go to AWS Route 53**
2. **Select the hosted zone** associated with your domain.
3. **Click "Create Record".**
4. **Choose Record Name:** Leave it blank (this applies to the root domain, e.g., `yourdomain.com`).
5. **Select Record Type:** `A – IPv4 address`
6. **Alias:** **Yes**
7. **Route traffic to:** Select **"Alias to Application Load Balancer"** and choose the **ALB DNS name**.
8. **Click Create Record.**

✅ This will route traffic from `yourdomain.com` to the **Application Load Balancer (ALB)**.

---

### **b. Create an Alias Record for the Tooling Website Subdomain**
This step ensures that `tooling.yourdomain.com` correctly routes traffic.

1. **Go back to Route 53 Hosted Zone.**
2. **Click "Create Record".**
3. **Enter Record Name:** `tooling` (this applies to `tooling.yourdomain.com`).
4. **Select Record Type:** `A – IPv4 address`
5. **Alias:** **Yes**
6. **Route traffic to:** Select **"Alias to Application Load Balancer"** and choose the **ALB DNS name**.
7. **Click Create Record.**

✅ This will route traffic from `tooling.yourdomain.com` to the **Application Load Balancer (ALB)**.

---

## **2. Alternative Option: Use a CNAME Record Instead**
Instead of an **alias record**, you can use a **CNAME record** to achieve similar results.

- If you choose **CNAME**, follow the same steps, but select `CNAME` as the record type and point it to the **ALB DNS name** instead.

💡 **Key Differences:**
- **Alias Record**: Faster and can point to AWS resources like ALBs, S3 buckets, and CloudFront.
- **CNAME Record**: Works for domain-to-domain mapping but **cannot be used for root domains** (only subdomains).

---

## **3. Verify DNS Propagation**
Once you've created the records, test whether your DNS configurations have propagated correctly:

- Use **nslookup** or **dig** to check the DNS resolution:
  ```bash
  nslookup yourdomain.com
  nslookup tooling.yourdomain.com
  ```
  Or:
  ```bash
  dig yourdomain.com
  dig tooling.yourdomain.com
  ```

- If it returns the **ALB DNS name**, your setup is successful.
- If not, wait **a few minutes to hours** for propagation.

---

## **4. Confirm Website Accessibility**
- Open a web browser and visit:
  - `http://yourdomain.com`
  - `http://tooling.yourdomain.com`
- Both should resolve to your ALB, correctly loading the WordPress and Tooling websites.

---

## **5. (Optional) Enable HTTPS with an SSL Certificate**
To secure your websites, you can use **AWS Certificate Manager (ACM)** to issue an **SSL certificate** and enable HTTPS via **ALB**.

---

### **Final Checklist**
✅ **Alias record for root domain (`yourdomain.com`) pointing to ALB**  
✅ **Alias record for subdomain (`tooling.yourdomain.com`) pointing to ALB**  
✅ **DNS records verified using `nslookup` or `dig`**  
✅ **Website loads correctly in a browser**

This completes your **Route 53 DNS setup**! 🚀


















