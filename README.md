K8S-Lab Deployemnt Process
--------------------------


1. Infrastructure Deplopyment
  - Created an EKS cluster with a node group and two nodes.
  - Created an EC2 instance in same VPC public subnet to run Nexus Repository and SonarQube. Both were made accessible on internet to access its dashboards.
  - Created another EC2 instance in same VPC private subnet. This is used as a self hosted runner for Azure DevOps.
  - Installed all dependencies needed and kubectl within it. Gave required permission to access the EKS from this machine.
  - Created policy and roles to attach service account in K8S to allow it to created Application Load Balancers.


2. Code Analysis and Artifact pushing
  - Created a settings.xml file in pipeline for adding both the Sonar and Nexus contexts.
  - Did the sonar scanning and verified the report in SonarQube dashboard.
  - Later built the maven package and pushed the jar to Nexus.
  * Please refer azure-pipelines.yml file.


3. Deployment to EKS
  - Got the JAR from Nexus Repo and created a docker image of it and pushed it to ECR.
  - Created a secret in EKS with ECR credentials.
  - Used kubectl commands to deploy a Deployment, Service and an Ingress.
  - Verified the deployment success in EKS and connected to the application using the ALB.
  * Please refer k8s-deployment folder


4. Monitoring.
  - Used the Prometheus helm repository to install Prometheus and Grafana in the EKS.
  - Created an Ingress to route to the Prometheus Dashboard and Grafana dashboard from the internet.
  - Added the deployments endpoints to monitor.
  - Created an alert in Grafana to notify any anomaly in running of application pods.