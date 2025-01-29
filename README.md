# Automated CI/CD Pipeline with Jenkins, Docker, Kubernetes, SonarQube, Nexus, and Monitoring Tools

## Project Overview

This project demonstrates the implementation of a fully automated **CI/CD pipeline** for a web application, integrating various tools such as **Jenkins**, **Docker**, **Kubernetes**, **SonarQube**, **Nexus**, and monitoring tools like **Prometheus** and **Grafana**. The pipeline automates the entire process of building, testing, deploying, and monitoring a containerized application, ensuring that quality checks and security scans are incorporated throughout.

---

## Technologies Used

- **Cloud Platform**: AWS EC2
- **CI/CD Tool**: Jenkins
- **Version Control**: GitHub
- **Containerization**: Docker
- **Container Orchestration**: Kubernetes
- **Build & Test Tool**: Maven, Trivy, SonarQube
- **Artifact Repository**: Nexus
- **Monitoring**: Prometheus, Grafana
- **Communication Protocols**: SSH, Kubernetes API
- **Email Notification**: Gmail App Passwords (for Jenkins email notifications)

---

## Steps to Set Up

### **Step 1: Infrastructure Setup**
1. **Set up EC2 Instances**:
   - Provision AWS EC2 instances for Jenkins, Kubernetes master and slave nodes, SonarQube, and Nexus.
2. **Install Required Tools**:
   - Install Docker, Kubernetes, Jenkins, SonarQube, Nexus, and other necessary tools on all servers.
   - Set up Docker containers for SonarQube and Nexus to manage code quality and store artifacts.
3. **Set up Monitoring Tools**:
   - Install Prometheus, Grafana, Node Exporter, and Blackbox Exporter for monitoring system and application performance.

---

### **Step 2: GitHub Integration**
1. **Clone Repository**:
   - Clone the project repository (e.g., `Boardgame`) into a private GitHub repository.
2. **Configure GitHub Token**:
   - Create a GitHub token to enable secure integration between Jenkins and the repository.

---

### **Step 3: Jenkins Pipeline Setup**
1. **Install Jenkins Plugins**:
   - Install necessary plugins in Jenkins: Docker, Kubernetes, SonarQube, Maven, etc.
2. **Configure Jenkins Tools**:
   - Configure tools like JDK, Maven, SonarQube Scanner, and Docker in Jenkins.
3. **Set up Jenkins Pipeline Script**:
   - Create a Jenkins pipeline with various stages for building, testing, and deploying the application.

---

### **Step 4: Define Pipeline Stages**
1. **Git Checkout**:
   - Pull the latest source code from GitHub repository.
2. **Compile & Test**:
   - Compile the code using Maven and run unit tests.
3. **File System Scan**:
   - Run a security scan using Trivy to check for vulnerabilities in the file system.
4. **SonarQube Analysis**:
   - Integrate SonarQube for static code analysis and ensure code quality is up to standard.
5. **Quality Gate**:
   - Wait for the SonarQube quality gate to pass before proceeding further in the pipeline.
6. **Build Artifact**:
   - Build the application artifact (JAR/WAR) using Maven.
7. **Publish to Nexus**:
   - Deploy the built artifact to the Nexus repository for storage and version management.
8. **Build and Tag Docker Image**:
   - Build and tag a Docker image for deployment to Kubernetes.
9. **Docker Image Scan**:
   - Run Trivy to scan the Docker image for vulnerabilities.
10. **Push Docker Image**:
   - Push the Docker image to Docker Hub or other container registry.
11. **Deploy to Kubernetes**:
   - Deploy the Docker image to the Kubernetes cluster using `kubectl`.
12. **Verify Deployment**:
   - Verify the deployment by checking the status of pods and services in the Kubernetes cluster.

---

### **Step 5: Set Up Kubernetes Deployment**
1. **Create Kubernetes Cluster**:
   - Set up a Kubernetes cluster using `kubeadm` on AWS EC2 instances.
2. **Create Service Account**:
   - Create a service account in Kubernetes for Jenkins to interact with the cluster.
3. **Role and RoleBinding**:
   - Define roles and role bindings to grant Jenkins the necessary permissions to deploy and manage resources in Kubernetes.
4. **Generate Token**:
   - Generate a token for Jenkins to authenticate with the Kubernetes cluster securely.
5. **Create Deployment YAML**:
   - Define the Kubernetes deployment and service configurations for the application.

---

### **Step 6: Set Up Monitoring Tools**
1. **Configure Prometheus**:
   - Set up Prometheus to monitor system metrics such as CPU usage, memory usage, and disk I/O.
2. **Set Up Grafana**:
   - Install and configure Grafana to visualize metrics collected by Prometheus.
3. **Set Up Exporters**:
   - Install Node Exporter to collect host-level metrics and Blackbox Exporter to probe website response times.

---

### **Step 7: Email Notifications and Reporting**
1. **Configure Email Notifications**:
   - Set up Jenkins to send email notifications with build status and reports after each pipeline run.
2. **Include Reports**:
   - Attach vulnerability scan reports (e.g., Trivy scan reports) in email notifications for transparency.

---

### **Step 8: Troubleshooting and Issue Resolution**
1. **Kubernetes Authentication Issues**:
   - Ensure that the Jenkins service account token is correctly configured and added to Jenkins credentials for successful authentication with Kubernetes.
2. **Docker Build Failures**:
   - Optimize Dockerfiles and ensure correct dependencies are defined to resolve Docker build issues.
3. **SonarQube Quality Gate Failures**:
   - Address code quality issues identified in SonarQube scans before proceeding to the next pipeline stage.
4. **Nexus Authentication Errors**:
   - Resolve Nexus authentication issues by ensuring proper credentials and repository configuration in Jenkins and Nexus.

---

### **Step 9: Final Deployment & Verification**
1. **Push and Deploy Docker Image**:
   - After successful build and vulnerability scan, the Docker image is pushed to Docker Hub, then deployed to the Kubernetes cluster.
2. **Verify Kubernetes Deployment**:
   - Ensure the deployment is successful by checking pod and service statuses in Kubernetes.

---

## Conclusion

By following these steps, the entire process from code commit to deployment is automated, ensuring a secure, scalable, and efficient **CI/CD pipeline** with quality checks, security scans, and real-time monitoring integrated throughout.
