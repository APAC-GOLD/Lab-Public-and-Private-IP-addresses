# Internet Protocols - Public and Private IP addresses

After this lab you have investigated the customer’s environment and applied troubleshooting techniques that allowed you to resolve the customers’ issue. Within the scenario, you discovered that the customer’s EC2 instance (instance A) needed a public IP address to connect to the internet. This was tested by using an SSH utility to connect to the instance. Private IP addresses are used within the VPC and cannot establish a connection to the internet. As module 4 noted, you discovered that using a public range of IP addresses for a VPC can result in complications from having replies back from other unrelated resources.

Classless Inter-Domain Routing (CIDR) is a method for allocating IP addresses and for IP routing. The Internet Engineering Task Force introduced CIDR in 1993 to replace the previous classful network addressing architecture on the Internet. Its goal was to slow the growth of routing tables on routers across the Internet, and to help slow the rapid exhaustion of IPv4 addresses. IP addresses are described as consisting of two groups of bits in the address: the most significant bits are the network prefix, which identifies a whole network or subnet, and the least significant set forms the host identifier, which specifies a particular interface of a host on that network. This division is used as the basis of traffic routing between IP.

The Transmission Control Protocol (TCP) is one of the main protocols of the Internet protocol suite. It originated in the initial network implementation in which it complemented the Internet Protocol. Therefore, the entire suite is commonly referred to as TCP/IP. TCP provides reliable, ordered, and error-checked delivery of a stream of octets between applications running on hosts communicating via an IP network.

## OBJECTIVES

Summarize and investigate the customer scenario
Analyze the difference between a private and public IP address
Develop a solution to the customer’s issue in this lab
Summarize and describe your findings (group activity)
DURATION
This lab requires approximately 1 hour to complete.

SCENARIO
Your role is a cloud support engineer at Amazon Web Services (AWS). During your shift, a customer from a Fortune 500 company requests assistance regarding a networking issue within their AWS infrastructure. The following is the email and an attachment regarding their architecture:

TICKET FROM YOUR CUSTOMER
Hello, Cloud Support!

​ We currently have one virtual private cloud (VPC) with a CIDR range of 10.0.0.0/16. In this VPC, we have two Amazon Elastic Compute Cloud (Amazon EC2) instances: instance A and instance B. Even though both are in the same subnet and have the same configurations with AWS resources, instance A cannot reach the internet, and instance B can reach the internet. I think it has something to do with the EC2 instances, but I’m not sure. I also had a question about using a public range of IP address such as 12.0.0.0/16 for a VPC that I would like to launch. Would that cause any issues? Attached is our architecture for reference.

Thanks! Jess Cloud Admin

The customer's architecture, which consists of a VPC, internet gateway, public subnet with instance A, and a public subnet with instance B

Figure: The customer’s architecture, which consists of a VPC, internet gateway, public subnet with instance A, and a public subnet with instance B.

AWS service restrictions
In this lab environment, access to AWS services and service actions might be restricted to the ones that you need to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that this lab describes.

Start lab
To launch the lab, at the top of the page, choose Start lab.
You must wait for the provisioned AWS services to be ready before you can continue.

To open the lab, choose Open Console.
You are automatically signed in to the AWS Management Console in a new web browser tab.

Do not change the Region unless instructed.

COMMON SIGN-IN ERRORS
Error: You must first sign out

If you see the message, You must first log out before logging into a different AWS account:

Choose the click here link.
Close your Amazon Web Services Sign In web browser tab and return to your initial lab page.
Choose Open Console again.
Error: Choosing Start Lab has no effect
In some cases, certain pop-up or script blocker web browser extensions might prevent the Start Lab button from working as intended. If you experience an issue starting the lab:

Add the lab domain name to your pop-up or script blocker’s allow list or turn it off.
Refresh the page and try again.
Task 1: Investigate the customer’s environment
Recall that you previously covered public and private IP addresses and CIDRs. As you go through this lab, think about the differences between public and private IP addresses for task 1. For task 2, think about the importance of using private IP ranges rather than public IP ranges.

Note

For this lab, you have already checked the AWS architecture, and everything is routed and attached correctly. This lab does not cover any AWS architecture.

In the scenario, Jess, who is the customer requesting assistance, has two EC2 instances in one VPC. Instance A cannot reach the internet, and instance B can even though they are configured the same within the VPC. Currently, the customer’s AWS architecture seems sound because one of their instances works. Jess suspects that the instance configuration may be the issue.

She also has a question about using a public range of IP addresses for the new VPC and has asked if you could provide further insight on her question.

You currently have one VPC with the same CIDR of 10.0.0.0/16 with two instances — instance A and instance B — with the same configurations as the customer. When troubleshooting networking and AWS, you can apply a troubleshooting method where you start from the top and work your way down or start from the bottom and work your way up. You start troubleshooting from the bottom and work your way to the top in layers using an example such as the OSI model. At the very bottom of this architecture is the EC2 instance. Although the cloud architecture does not directly translate to the OSI model, the following is an example of how the OSI and cloud relate.

OSI Model AWS infrastructure
Layer 7 Application (how the end user sees it) Application
Layer 6 Presentation (translator between layers) Web Servers, application servers
Layer 5 Session (session establishment, security) EC2 instances
Layer 4 Transport (TCP, flow control) Security group. NACL
Layer 3 Network (Packets which contain IP addresses) Route Tables, IGW, Subnets
Layer 2 Data Link (Frames which contain physical MAC addresses) Route Tables, IGW, Subnets
Layer 1 Physical (cables, physical transmission bits and volts) Regions, Availability Zones
Table: This is an example of how the AWS infrastructure and its resources have similarities to the OSI model. This information can be beneficial when troubleshooting.

​ For task 1, you gain an understanding of the customer’s environment and replicate their issue.

Once you are in the AWS console, type and search for EC2 in the search bar on the top-left corner. Select EC2 from the list.
Tip: Alternatively, You can also find EC2 under Services - Compute in the top left corner

The search bar can be used to find the Amazon EC2 service. Once you find the service, select it.

Figure: The search bar can be used to find the Amazon EC2 service. Once you find the service, select it.

You are now in the Amazon EC2 dashboard. In the left navigation menu, choose Instances. This option takes you to your current EC2 instances. You should currently see two EC2 instances.
The Amazon EC2 dashboard: From this page, you can choose instance A or instance B and see the features of each instance. You can also launch instances from here.

Figure: Amazon EC2 dashboard

Please copy and paste the names and IP addresses of both instances for future reference in a text editor. Select the check box next to instance A. At the bottom of the page, choose the Networking tab, and note the Public and Private IPv4 addresses. Once you copy and paste the name and IP addresses, deselect the instance, and then select instance B and do the same. Did you notice any differences? Note them if you did.
You can select instance A and its networking configurations by selecting the checkbox next to the instance name and navigating down to the Networking tab. There you will find the associated IPv4 addresses, VPC, and subnet information.

Figure: Amazon EC2 instance A networking information. The IP address values may vary.

You can select instance B and its networking configurations by selecting the checkbox next to the instance name and navigating down to the Networking tab. There you will find the associated IPv4 addresses, VPC, and subnet information.

Figure: Amazon EC2 instance B networking information. The IP address values may vary.

Task 1: Use SSH to connect to an Amazon Linux EC2 instance
In this task, you will connect to a Amazon Linux EC2 instance. You will use an SSH utility to perform all of these operations. The following instructions vary slightly depending on whether you are using Windows or Mac/Linux.

WINDOWS USERS: USING SSH TO CONNECT
These instructions are specifically for Windows users. If you are using macOS or Linux, skip to the macOS instructions.

From the Lab Information pane, select the PPK link and save the file. Typically your browser will save it to the Downloads directory.

Make a note of the PublicIP address.

Download PuTTY to SSH into the Amazon EC2 instance. If you do not have PuTTY installed on your computer, download it here.

Open putty.exe

Configure PuTTY timeout to keep the PuTTY session open for a longer period of time.:

Select Connection
Set Seconds between keepalives to

30
Configure your PuTTY session:
Select Session
Host Name (or IP address): Paste the Public DNS or IPv4 address of the instance you made a note of earlier. Alternatively, return to the EC2 Console and select Instances. Check the box next to the instance you want to connect to and in the Description tab copy the IPv4 Public IP value.
Back in PuTTY, in the Connection list, expand SSH
Select Auth (don’t expand it)
Select Browse
Browse to and select the ppk file that you downloaded
Select Open to select it
Select Accept
Select Yes, to trust and connect to the host.

When prompted login as, enter:

ec2-user
This will connect you to the EC2 instance.

Windows Users: Skip ahead to the next task.

MACOS AND LINUX USERS
These instructions are specifically for Mac/Linux users. If you are a Windows user, skip ahead to the next task.

From the Lab Information pane, select the PEM link and save the file.

Make a note of the PublicIP address.

Open a terminal window, and change directory

cd
to the directory where the pem file was downloaded. For example, if the file was saved to your Downloads directory, run this command:

cd ~/Downloads
Change the permissions on the key file to be read-only, by running this command (replace <ssh-key> with the name of the file you downloaded earlier):

chmod 400 <ssh-key>
For example:

chmod 400 Ec2KeyPair-PEM.pem
Note: Your file name may be different from the example.

Run the below command. (replace <ssh-key> with the name of the file you downloaded earlier, and <public-ip> with the PublicIP address you copied earlier):

ssh -i <ssh-key> ec2-user@<public-ip>
Type

yes
when prompted to allow the first connection to this remote SSH server. Because you are using a key pair for authentication, you will not be prompted for a password.

Question - Were you able to use the SSH to connect to both instances? Why or why not?

Answer: If you were not able to connect to instance A, it was due to this instance being assigned only a private IP address. Private IP addresses cannot be accessed from outside the VPC. This is why you are only able to connect to instance B. Instance B has a public IP address assigned to it allowing access from outside the VPC, which allows you to use the SSH utility to connect to the instance. The customer asked for your insight regarding using a public CIDR for a new VPC that she would like to launch. Refer to module 4 and gather some evidence and summarize a short explanation of your findings to explain to the customer why or why not you recommend this approach.

Task 3: Send the Response to the customer (group activity)
In groups of two, submit your findings.

Person 2 will act as Jess the customer, and Person 1 will act as the cloud support engineer. Person 1 will talk over their findings with person 2.

Note This task should take only 5-10 minutes. If a group activity is not possible due to COVID, please have one student walk through their findings to the class.

End lab
Follow these steps to close the console and end your lab.

Return to the AWS Management Console.

At the upper-right corner of the page, choose AWSLabsUser, and then choose Sign out.

Choose End lab and then confirm that you want to end your lab.

Recap
In this lab you have investigated the customer’s environment and applied troubleshooting techniques that allowed you to resolve the customers’ issue. Within the scenario, you discovered that the customer’s EC2 instance (instance A) needed a public IP address to connect to the internet. This was tested by using an SSH utility to connect to the instance. Private IP addresses are used within the VPC and cannot establish a connection to the internet. As module 4 noted, you discovered that using a public range of IP addresses for a VPC can result in complications from having replies back from other unrelated resources.

Additional Resources
Amazon EC2 Instance IP addressing
VPC CIDR
RFC1918
For more information about AWS Training and Certification, see https://aws.amazon.com/training/.

Your feedback is welcome and appreciated. If you would like to share any suggestions or corrections, please provide the details in our AWS Training and Certification Contact Form.

© 2022 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

https://awsrestart.labs.awsevents.com/lab/arn%3Aaws%3Alearningcontent%3Aus-east-1%3A470679935125%3Ablueprintversion%2FCUR-TF-100-RSNETK-3%2F261-lab-NF-public-private-ip-addresses%3A3.0.0-158ac535/en-US/46a424b2-440d-424c-b01f-97df9bc7474a::fsAKvnKx5MKeqzrLt7HEVd
