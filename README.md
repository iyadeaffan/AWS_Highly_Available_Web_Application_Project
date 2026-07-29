# AWS_Highly_Available_Web_Application_Project

Highly Available Web Application:
Project Architecture
                     Internet
                         |
                  Classic Load Balancer
                  /                  \
             EC2 Instance        EC2 Instance
                  \              /
               Auto Scaling Group
                       |
              CloudWatch Monitoring
                       |
                  SNS Email Alerts

Learning Objectives
•	Launch and configure EC2 instances.
•	Install and host a web application.
•	Create an Amazon Machine Image (AMI).
•	Create a Launch Template.
•	Configure a Classic Load Balancer (CLB).
•	Configure an Auto Scaling Group (ASG).
•	Create CloudWatch alarms.
•	Configure SNS email notifications..
•	Verify High Availability.
AWS Services Used
Service	Purpose
EC2	Host the website
Classic Load Balancer	Distribute incoming traffic
Auto Scaling Group	Automatically add/remove EC2 instances
CloudWatch	Monitor CPU utilization
SNS	Send email notifications
AMI	Create reusable server image
Launch Template	Template for new EC2 instances

Prerequisites
•	AWS Account
•	Key Pair
•	Security Group
•	Email Address (for SNS)

Step 1 – Launch EC2 Instance
Launch an EC2 instance with the following configuration:
•	Amazon Linux 2
•	Security Group
o	SSH (22)
o	HTTP (80)
•	Key Pair
Step 2 – Connect to EC2
Connect using putty or Console connect

Step 3 – Install Apache Web Server
Update packages
sudo yum update -y
Install Apache
sudo yum install httpd -y
Enable Apache
sudo systemctl enable httpd
Start Apache
sudo systemctl start httpd
Verify
sudo systemctl status httpd
Expected Status
active (running)

Step 4 – Create Web Page
Download any template from https://bootstrapmade.com/ 
Verify
Open
http://Public-IP
Step 5 – Create Amazon Machine Image (AMI)
1.	Select EC2 Instance.
2.	Click Actions.
3.	Select Image and Templates.
4.	Click Create Image.
Wait until the AMI status changes to Available.
________________________________________
Step 6 – Create Launch Template
Navigate to
EC2
→ Launch Templates
→ Create Launch Template
Configuration:
•	Name
•	AMI
•	Instance Type
•	t2.micro
•	Key Pair
•	Select existing key pair
•	Security Group
•	Existing Security Group
•	Create Launch Template.
Step 7 – Create Classic Load Balancer
Navigate to
EC2
→ Load Balancers
→ Create Load Balancer
→ Classic Load Balancer
Configuration
Name
Listener
HTTP 80 
Availability Zone
Select same AZ
Security Group
Allow HTTP
Health Check
Ping Protocol : HTTP

Ping Port : 80

Ping Path : /
Healthy Threshold
2
Unhealthy Threshold
2
Timeout
5
Interval
30
Create Load Balancer.

Step 8 – Create Auto Scaling Group
Navigate
EC2
→ Auto Scaling Groups
→ Create Auto Scaling Group
Select Launch Template
Group Name
Network
Select VPC and Subnet.
Attach Existing Load Balancer
Group Size
Desired Capacity : 2

Minimum : 2

Maximum : 5
Health Check
ELB + EC2
Create Auto Scaling Group.
________________________________________
Step 9 – Verify Auto Scaling
Observe
EC2 Instances
Expected
Two running EC2 instances
Both should register under
Classic Load Balancer
Status
InService

Step 10 – Create SNS Topic
Navigate
SNS
→ Topics
→ Create Topic
Type
Standard
Topic Name
Create Topic.

Step 11 – Subscribe Email
Create Subscription
Protocol
Email
Endpoint
your-email@gmail.com
Confirm subscription from your inbox.

Step 12 – Create CloudWatch Alarm
Navigate
CloudWatch
→ Alarms
→ Create Alarm
Metric
EC2

CPUUtilization
Condition
Less than 20%
Evaluation
2 consecutive periods
Notification
Create Alarm.

Step 13 – Observe Auto Scaling
Navigate
CloudWatch Metrics
Observe
CPU increases
After a few minutes
Alarm changes

OK → ALARM
Navigate
Auto Scaling Activity
Observe
Launching new EC2 instance
Expected
Desired Capacity

2 → 3
Eventually
3 → 4
depending on CPU load.
________________________________________
Step 14 – Verify Email Notification
Check email.
Expected
CloudWatch Alarm State Changed

OK → ALARM
________________________________________
Step 15 – Test Load Balancer
Open
http://CLB-DNS
Refresh multiple times.
Application should remain available.


Step 16 – Test High Availability
Terminate one EC2 instance manually.
Navigate
EC2

Terminate Instance
Observe
Auto Scaling Activity
Expected
New EC2 automatically launched
Load Balancer
Registers new instance automatically
Website remains available.
