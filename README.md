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
   * Use the email address and password you provided during the DevOps account creation process.

    
    Use the email address and password you provided during the DevOps account creation process.
    Once logged in, you can begin managing resources under this account.


































