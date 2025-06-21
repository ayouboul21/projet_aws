# Projet AWS

This project demonstrates the deployment of a web application using AWS services, Docker, and Jenkins for continuous integration and delivery.

## 🛠️ Technologies Used

- **AWS**: Amazon Web Services for cloud infrastructure.
- **Docker**: Containerization of applications.
- **Jenkins**: Automation server for continuous integration and delivery.
- **GitHub Actions**: CI/CD workflows for automating build and deployment processes.

## 📁 Project Structure

The repository contains the following key files and directories:

- `.dockerignore`: Specifies files and directories to be ignored by Docker during build.
- `.gitignore`: Specifies files and directories to be ignored by Git.
- `Dockerfile`: Instructions for building the Docker image of the application.
- `Jenkinsfile`: Pipeline script for Jenkins to automate the build and deployment process.
- `site/`: Directory containing the website's source code.

## 🚀 Getting Started

### Prerequisites

- Docker installed on your local machine.
- Jenkins set up with the necessary plugins for building and deploying Docker images.
- AWS account with appropriate permissions for deploying resources.

### Setup Instructions

1. **Clone the Repository**:

   ```bash
   git clone https://github.com/ayouboul21/projet_aws.git
   cd projet_aws
   ```

2. **Build the Docker Image**:

   ```bash
   docker build -t *your-image-name* .
   ```

3. **Run the Docker Container**:

   ```bash
   docker run -p 8080:80 *your-image-name*
   ```

   Access the application by navigating to `http://localhost:8080` in your web browser.

4. **Configure Jenkins**:

   - Set up a Jenkins pipeline using the provided `Jenkinsfile`.
   - Ensure Jenkins has access to your AWS credentials and Docker daemon.

5. **Deploy to AWS**:

   Follow the instructions in the Jenkins pipeline to deploy the application to your AWS infrastructure.
