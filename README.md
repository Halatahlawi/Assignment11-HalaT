# Assignment11-HalaT
Clarusway SDA Bootcamp Deployment Project
Week-11 Assignment | SDA2016 | Hala Tahlawi
MERN Stack Blog App Deployment
---------------------------------------------------------------------------------------------------------------
Project Overview:
This project leverages Terraform and Ansible to automate the deployment of a blog application based on the MERN stack (MongoDB, Express.js, React.js, Node.js) within the Amazon Web Services (AWS) cloud environment. The frontend component of the application is deployed to an Amazon S3 bucket configured for static website hosting. The backend services are hosted on an EC2 instance, provisioned and configured via Infrastructure as Code (IaC) and configuration management tools. For data persistence, the project integrates MongoDB Atlas, a cloud-hosted NoSQL database solution. Additionally, a separate S3 bucket is dedicated to managing media file uploads, with appropriate IAM policies applied to ensure secure and controlled access.

Architecture Overview:
This MERN stack blog app is deployed with the following components:
1- Frontend (S3 Static Hosting):
The React-based frontend is hosted on an S3 bucket with static website hosting and public read access.
2- Backend (EC2 + Node.js):
A single Ubuntu 22.04 EC2 instance runs the Node.js backend. It's provisioned via Terraform and configured using Ansible. PM2 ensures the app stays running.
3- Database (MongoDB Atlas):
A free-tier MongoDB cluster stores app data. EC2 IP is whitelisted, and credentials are injected via Ansible.
4- Media Storage (S3 Bucket):
A separate S3 bucket stores uploaded media. Access is controlled by an IAM user and CORS policy is applied for browser uploads.
5- Security:
Only essential ports (22, 80, 5000) are open. IAM and .env variables are managed securely using Ansible and Terraform outputs.

+-------------+       +---------------------+
|  User       | <---> |  S3 (Frontend Host) |
+-------------+       +---------------------+
                           |
                           | API Calls
                           v
                      +------------+        +------------------+
                      |  EC2       | <----> | MongoDB Atlas DB |
                      |  Backend   |        +------------------+
                      +------------+
                           |
                      +------------+
                      |  S3 (Media)|
                      +------------+


-------------------------------------------------------------------------------------------------------------------------
 


