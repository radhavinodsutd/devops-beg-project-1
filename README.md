# DevOps Beginner Project: Simple CI/CD Pipeline and Web Application Deployment

## Project Overview
This beginner-level DevOps project focuses on setting up a CI/CD pipeline to automate the deployment of a simple web application. The key tools used include **GitHub Actions**, **AWS S3**, and **AWS Elastic Beanstalk**. The main objectives were to set up automated testing, integrate CI/CD, and deploy a basic application onto a cloud platform.

## Project Steps

### Step 1: Setting Up the Application
1. **Create a Simple Web Application**:
   - Developed a basic **Flask** web application.
   - Added an endpoint (`/`) that displays **"Hello Elastic Beanstalk!"**.
   - Configured the app to run on `host='0.0.0.0'` and `port=8080`.

### Step 2: Create GitHub Repository
- Created a GitHub repository named **`devops-beg-project-1`**.
- Added all necessary files, including:
  - **`app.py`**: The main Python script for the Flask application.
  - **`requirements.txt`**: Lists all dependencies for the application.
  - **`.github/workflows/deploy.yml`**: The workflow configuration file for GitHub Actions.

### Step 3: Set Up GitHub Actions Workflow
1. **Create the `.github/workflows/deploy.yml`** file:
   - Defined the workflow to trigger on a push to the **main** branch.
2. **Workflow Steps**:
   - **Checkout Code**: Use **`actions/checkout@v3`** to pull the latest code from the repository.
   - **Set Up Python**: Use **`actions/setup-python@v4`** to set up Python version **3.8**.
   - **Install Dependencies**: Install requirements listed in **`requirements.txt`**.
   - **Run Tests**: Use **`pytest`** to test the application.
   - **Deploy to AWS**:
     - **S3**: Upload static assets to an S3 bucket.
     - **Elastic Beanstalk**: Deploy the application to an Elastic Beanstalk environment.

### Step 4: Configure AWS Elastic Beanstalk
- **Create an Environment** in AWS Elastic Beanstalk for the Flask application.
- Set up **security groups** and **IAM roles** to grant permissions to access required AWS resources.
- **Upload Deployment Package**: Initially uploaded a ZIP file manually, and then integrated with **GitHub Actions** to automate this step.

### Step 5: Establishing Continuous Integration and Deployment
- **Test Automation**: GitHub Actions automatically runs tests on each push.
- **Deployment Automation**: Once the tests pass, GitHub Actions deploys the updated version to **Elastic Beanstalk**, ensuring the latest version is always running.

## Nuances and Challenges Encountered

### 1. **GitHub Actions and CI/CD Workflow Configuration**
- **Issue**: Initially, the deployment was failing because **`pytest`** was not recognized as a command.
- **Solution**: Added **`pytest`** to the **`requirements.txt`** file to ensure it was installed in the environment during the workflow.

### 2. **Environment Variables for AWS Credentials**
- **Issue**: Encountered **AccessDenied** errors during deployment.
- **Solution**: Added **AWS credentials** (`AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`) securely to **GitHub Secrets**. These secrets were then used in the workflow to authenticate GitHub Actions with AWS.

### 3. **Elastic Beanstalk Deployment Issues**
- **Issue**: The initial deployment failed due to an incorrect **launch template** configuration.
- **Solution**: Updated the **launch template** to match the **instance configuration** required by Elastic Beanstalk, specifically making sure that the EC2 instance type was available in the selected region.

### 4. **Manual vs. Automated Deployment**
- **Challenge**: Initially uploaded the application package manually, which was cumbersome.
- **Solution**: Automated the deployment process using **`eb deploy`** in the GitHub Actions workflow. This allowed a seamless deployment directly from GitHub.

### 5. **Bucket Access for Static Assets**
- **Issue**: Initially, uploading static files to **S3** failed due to permissions issues.
- **Solution**: Added **`s3:PutObject`** permission to the IAM user used by GitHub Actions, allowing it to upload files to the S3 bucket.

### 6. **Using SSH Key for EC2 Access**
- **Issue**: Needed **SSH key-based access** to the EC2 instance for troubleshooting.
- **Solution**: Configured **key pair** in AWS and used it for accessing the instance without passwords, which simplified remote troubleshooting.

## Final Notes and Best Practices
- **GitHub Secrets for Credentials**: Store AWS credentials in **GitHub Secrets** to keep them secure and prevent unauthorized access.
- **Automate as Much as Possible**: Automating both testing and deployment ensures that the latest code is always tested and deployed, minimizing human errors.
- **Test Your Infrastructure Changes**: For changes involving **launch templates** or **IAM roles**, make sure to test configurations to avoid deployment issues.
- **Elastic Beanstalk for Simple Deployments**: AWS Elastic Beanstalk was a good choice for a beginner project, as it abstracts much of the underlying infrastructure complexity while providing the required deployment functionality.

## Summary of Lessons Learned
- Setting up a **CI/CD pipeline** can significantly improve the development and deployment process by reducing manual effort.
- **Permissions and Secrets** are critical in any automation involving cloud resources. Incorrect permissions were a recurring issue, but using **IAM roles** and **GitHub Secrets** helped resolve those problems.
- Using **Elastic Beanstalk** was ideal for this project as it provided a managed service that allowed easy deployment without having to deal with the nitty-gritty details of the underlying infrastructure.

Feel free to contribute to the project or suggest improvements!

---
**Author**: Your Name | **Date**: [Date] | **License**: MIT License

