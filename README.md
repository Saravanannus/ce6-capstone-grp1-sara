# ce6-capstone-grp1
Introduction
This is a sample application used to demonstrate a POC of using GitHub Actions to deploy to AWS ECR and Fargate.
## The Team Consist of:
- VVenila
- Kerenp
- Jyoti
- Saravanan.B

# Architecture Diagram

![image](https://github.com/user-attachments/assets/ac446041-38a4-4615-a08f-5942960dacdc)
Deployment Process
This project is a Node.js web application containerized with Docker and deployed on AWS Elastic Container Service (ECS) using Fargate. The infrastructure and service deployment are managed with Terraform, while the CI/CD pipeline is set up using GitHub Actions to automate the build and deployment process.

### Prerequisites :
 - Before you begin the deployment, ensure the following are set up:

**AWS Account:** 
  
- You’ll need an AWS account with permissions to create ECS clusters, services, task definitions, and ECR (Elastic Container Registry) repositories.

**Terraform Installed:** 

- Ensure Terraform is installed on your local machine for infrastructure provisioning.
AWS CLI: Install the AWS CLI for managing AWS resources.

**GitHub Secrets:** 

- Set up the following GitHub secrets in your repository for authentication and deployments:

AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

AWS_REGION
ECR_REPOSITORY_URI (URI of your ECR repository)

**Deployment Steps:**

**1.** Clone the Repository
Start by cloning this repository to your local machine:bash Copy code
git clone https://github.com/your-username/your-repo.git
cd your-repo

**2.** Build the Docker Image
This project is containerized using Docker. The Dockerfile is already provided, which defines how the application is built. You can build the Docker image locally by running:

bash
Copy code
docker build -t your-application-name .
If you want to test the application locally, run the container:

bash
Copy code
docker run -p 8080:8080 your-application-name
This will expose the application on http://localhost:8080.

**3.** Infrastructure Provisioning with Terraform
The project uses Terraform to create and manage the AWS resources, including ECS, ECR, and networking components.

**To initialize and provision the infrastructure:**

bash
Copy code
cd terraform
terraform init
terraform apply
The Terraform script will:

Set up an ECS Cluster.
Create an ECS Service that runs your Docker container.
Manage networking configurations like Security Groups and Ingress/Egress rules.
Once the Terraform provisioning is complete, your ECS environment is ready to deploy the application.

**4.** Continuous Integration & Deployment with GitHub Actions
The project uses GitHub Actions for CI/CD, automatically building the Docker image, pushing it to ECR, and updating the ECS service.

The workflow is defined in .github/workflows/dev.yml and it performs the following steps:

Build Docker Image:
The workflow builds the Docker image for the application based on the Dockerfile.
Push to ECR:
Once the image is built, it is pushed to the AWS Elastic Container Registry (ECR).
Update ECS Task Definition:
After pushing the image to ECR, the ECS task definition is updated with the new image version.
Deploy to ECS:
The ECS service is updated to use the latest task definition, and the new container is deployed to Fargate.
You can trigger the GitHub Actions workflow by pushing changes to the main branch. The pipeline will automatically handle the deployment.

**5.** Health Check & Monitoring
We recommend adding a health check endpoint (e.g., /health) to your application so ECS can verify the status of your service. ECS will periodically send health check requests to this endpoint and ensure the application is running smoothly.

**Also, set up CloudWatch for logging and monitoring ECS tasks:**

ECS logs can be streamed to CloudWatch to track any errors or important application events.
You can also set up CloudWatch Alarms to monitor the health of your ECS service, such as task failures or high CPU/memory usage.
6. Ingress Traffic & Security Considerations
For security purposes, ensure that ingress traffic is only allowed from specific IP ranges or secured over HTTPS. The ECS service security group currently allows traffic from all sources (0.0.0.0/0), but for production environments, it’s recommended to restrict access:

Limit to specific trusted IP addresses or regions.
Enforce HTTPS by integrating AWS Certificate Manager (ACM) with Application Load Balancer (ALB) or CloudFront for SSL/TLS termination.
Summary
This project leverages Docker for containerization, Terraform for AWS resource management, and GitHub Actions for automating the deployment pipeline. By following the steps outlined above, your Node.js application will be deployed on ECS using AWS Fargate, and the pipeline will automatically rebuild and deploy any changes pushed to the main branch.

Make sure to monitor the application’s logs and performance in CloudWatch and ensure security best practices by restricting ingress traffic and enforcing HTTPS for production environments.
