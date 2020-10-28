# Udacity-CloudDevOps-Capstone-project

### Environment Setup:
  - Install Jenkins on an Amazon EC2 Instance
  - Install Jenkins Plugins
  - Blue Ocean
  - Pipeline AWS plugin
  - Credentials for AWS and Docker Hub 
  - Install linter
  - Install and configure AWS CLI
  - Install eksctl
  - Install and configure kubectl
  - Install Docker

### Steps
  1. Install Jenkins and all the necessary plugins in the EC2 Instance
  2. Create EKS Cluster by building pipeline using `create_cluster/Jenkinsfile`
  3. Build, Push and Deploy container by building pipeline using `Jenkinsfile`

### Pipeline steps:

 1.   Lint HTML
 2.   Lint Dockerfile
 3.   Build Docker Images
 4.   Push Docker Images
 5.   Set kubectl config
 6.   Blue Deployment
 7.   Green Deployment
 8.   Redirect to Blue
 9.   Wait for user approval
 10.  Redirect to green

 ### URLS
 1. BLUE-IMAGE: https://hub.docker.com/repository/docker/sumitmann/devops_capstone_blue
 2. GREEN-IMGAGE: https://hub.docker.com/repository/docker/sumitmann/devops_capstone_green
 3. DEPLOYMENT: http://a548542a948c34c93a5f1d939dfdb8a3-383808979.eu-west-1.elb.amazonaws.com:8000/
 4. JENKINS: http://ec2-63-33-199-61.eu-west-1.compute.amazonaws.com:8080/

