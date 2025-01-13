Kprofile Actions
This repository contains the CI/CD pipeline for the Kprofile project using GitHub Actions.

Workflow Overview
This workflow is triggered by:

Push events to any branch.

The workflow performs testing, builds and uploads a Docker image to ECR, and deploys the application to an EKS cluster.

Environment Variables
The following environment variables are used in the workflow:

AWS_ACCESS_KEY_ID: AWS Access Key ID for deployment to AWS.

AWS_SECRET_ACCESS_KEY: AWS Secret Access Key for deployment to AWS.

AWS_REGION: AWS region (default: us-east-2).

ECR_REPOSITORY: ECR repository name (kprofileapp).

EKS_CLUSTER: EKS cluster name (kprofile-eks).

Workflow Jobs
Testing
Runs-on: ubuntu-latest

Steps:

Code checkout Uses: actions/checkout@v4

Maven test Runs Maven tests.

Checkstyle Runs Checkstyle to ensure code style compliance.

Set Java 11 Sets Java 11 as the default version if needed. Uses: actions/setup-java@v3

Build and Publish
Needs: Testing Runs-on: ubuntu-latest

Steps:

Code checkout Uses: actions/checkout@v4

Build & Upload image to ECR Builds the Docker image and uploads it to ECR. Uses: appleboy/docker-ecr-action@master

Deploy to EKS
Needs: BUILD_AND_PUBLISH Runs-on: ubuntu-latest

Steps:

Code checkout Uses: actions/checkout@v4

Configure AWS credentials Uses: aws-actions/configure-aws-credentials@v1

Get Kube config file Updates the kubeconfig file to interact with the EKS cluster.

Print config file Prints the kubeconfig file for verification.

Login to ECR Creates a Kubernetes secret for Docker registry credentials.

Deploy Helm Deploys the application using Helm. Uses: bitovi/github-actions-deploy-eks-helm@v1.2.8

Key Points
Automatic Triggers:

The workflow is automatically triggered by pushes to any branch.

Testing:

The workflow runs Maven tests and Checkstyle to ensure code quality.

Build and Publish:

The workflow builds and uploads a Docker image to ECR.

Deploy to EKS:

The workflow deploys the application to an EKS cluster using Helm.

How to Use
Clone the repository:

sh
git clone <repository-url>
cd <repository-directory>
Set up secrets in the GitHub repository:

AWS_ACCESS_KEY_ID

AWS_SECRET_ACCESS_KEY

Push changes to the repository:

The workflow will automatically run based on the triggers defined.
