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
 
 
Prerequisites:

Before starting, ensure you have the following:

1-AWS Account with programmatic access.
2-Terraform installed (terraform -version).
3-Ansible installed (ansible --version).
4-Node.js and npm installed (node -v and npm -v).
5-MongoDB Atlas account and access to create a cluster.

Step 1: Clone the Repository
git clone https://github.com/cw-barry/blog-app-MERN.git
cd blog-app-MERN

Step 2: Infrastructure Deployment with Terraform
cd terraform
terraform init
terraform apply

-This will create the required resources on AWS:
1-EC2 instance (Ubuntu 22.04).
2-Security group (SSH, HTTP, port 5000).
3-S3 buckets (frontend and media).
4-IAM user for programmatic access.

Step 3: Set Up MongoDB Atlas:

1-Log in to MongoDB Atlas.
2-Create a free-tier cluster.
4-Whitelist your EC2 instance public IP.
5-Create a database user.
6-Copy the connection string for use in the backend .env file.


Step 4: Backend Provisioning with Ansible

cd ../ansible
# Update the inventory file with your EC2 instance public IP and private key
# Update `roles/backend/vars/main.yml` with your MongoDB and S3 credentials
ansible-playbook -i inventory backend-playbook.yml

The playbook performs the following:

-Installs Node.js and PM2.
-Clones the backend repository.
-Generates a .env file using variables.
-Installs dependencies.
-Starts the app with PM2.


Step 5: Frontend Deployment to S3

5.1 Create .envFile:
cd ../frontend

cat > .env << EOF
VITE_BASE_URL=http://<your-ec2-dns>:5000/api
VITE_MEDIA_BASE_URL=https://<sda2016-s3-media-bucket-name>.eu-north-1.amazonaws.com
EOF

5.2 Configure AWS CLI:
aws configure
# Enter your Access Key, Secret Key, Region: eu-north-1

5.3 Install Frontend Dependencies & Build:
npm install -g pnpm@latest-10
pnpm install
pnpm run build

5.4 Upload to S3 (Frontend Bucket)
aws s3 sync dist/ s3://<sda2016-s3-frontend-bucket-name>/

Step 6: Verify the Deployment:
Open the EC2 public DNS/IP with port 5000 to verify backend is running.
Open your S3 frontend bucket URL in the browser to see the blog app frontend.
Test uploading images to confirm media S3 bucket works.

Step 7: Cleanup :

"terraform destroy"

-Delete .env files containing sensitive credentials.
-Remove the IAM user credentials from AWS.
-Delete MongoDB Atlas IP rules and users.
-Remove EC2 key pairs if not needed.


Conclusion:
In this project, I used Terraform and Ansible on AWS to effectively install a scalable and production-ready MERN stack blog application. The frontend was deployed to an S3 bucket with static website hosting, and the backend was housed on an EC2 machine with automated provisioning through Ansible. Using a different S3 bucket with the proper IAM rules, media uploads were managed safely. The cloud-based database, MongoDB Atlas, provided scalability and high availability.
This deployment improved the dependability and reproducibility of cloud environments by showcasing the capabilities of automation and Infrastructure as Code (IaC). I obtained practical expertise with cloud provisioning, deployment pipelines, safe access control, and configuration management by finishing this project. These are all critical competencies for contemporary DevOps and cloud engineering positions.

