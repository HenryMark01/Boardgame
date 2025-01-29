### **Project 2: Automated CI/CD Pipeline with Jenkins, Docker, Kubernetes, SonarQube, Nexus, and Monitoring Tools**

#### **Project Overview**

![](./media/image4.png){width="6.5in" height="4.513888888888889in"}

This project demonstrates the implementation of a fully automated CI/CD
pipeline for a web application using Jenkins, Docker, Kubernetes,
SonarQube, Nexus, and monitoring tools (Prometheus, Grafana). The
pipeline automates the process of building, testing, deploying, and
monitoring a containerized application, ensuring that quality checks and
security scans are conducted throughout the pipeline.

#### **Technologies Used**

-   **Cloud Platform**: AWS EC2 (for hosting the infrastructure)

-   **CI/CD Tool**: Jenkins

-   **Version Control**: GitHub

-   **Containerization**: Docker

-   **Container Orchestration**: Kubernetes

-   **Build & Test Tool**: Maven, Trivy, SonarQube

-   **Artifact Repository**: Nexus

-   **Monitoring**: Prometheus, Grafana

-   **Communication Protocols**: SSH, Kubernetes API

-   **Email Notification**: Gmail App Passwords (for Jenkins email
    notifications)

#### **Infrastructure Setup**

##### **Servers and Software Installed:**

![](./media/image13.png){width="6.5in" height="1.4583333333333333in"}

1.  **Jenkins Server**:

    -   Jenkins installed with necessary plugins (Docker, Kubernetes,
        SonarQube, Maven, etc.).

    -   Docker and Trivy installed for building and scanning Docker
        images.

2.  **Kubernetes Cluster** (3 AWS EC2 instances):

    -   1 master and 2 slave nodes with Kubernetes installed and
        configured.

3.  **SonarQube and Nexus Servers**:

    -   Docker containers for SonarQube and Nexus to manage code quality
        and store built artifacts.

4.  **Monitoring Tools**:

    -   Prometheus and Grafana for monitoring application performance.

    -   Blackbox Exporter for probing websites and monitoring response
        times.

##### **Network Configuration:**

-   **Security Groups** assigned for required ports (application,
    Kubernetes API, SSH, etc.).

![](./media/image3.png){width="6.5in" height="2.0277777777777777in"}

-   **Credentials** for secure communication between Jenkins,
    Kubernetes, Nexus, and SonarQube.

![](./media/image7.png){width="6.5in" height="2.2916666666666665in"}

-   **Kubernetes Authentication**: Service accounts and roles set up for
    Jenkins to interact with Kubernetes securely.

#### **Pipeline Development and Automation**

##### **Phase 1: Infrastructure Setup**

-   Set up and configured EC2 instances for Jenkins, Kubernetes master
    and slave nodes, SonarQube, and Nexus.

-   Installed Docker, Kubernetes, and necessary tools on all servers.

-   Created Docker containers for SonarQube and Nexus.

-   Installed and configured Prometheus, Grafana, Node Exporter and
    Blackbox Exporter for monitoring.

##### **Phase 2: GitHub Integration**

-   **Cloned the Boardgame repository** into a private GitHub
    repository.

-   **Created a GitHub token** for Jenkins integration to access the
    repository securely.

##### **Phase 3: Jenkins Pipeline Setup**

-   **Installed Jenkins Plugins**:

    -   Eclipse Temurin, Maven, SonarQube Scanner, Docker, Kubernetes,
        etc.

    -   Configured tools for JDK, SonarQube Scanner, Maven, and Docker.

-   **Jenkins Pipeline Script**:

    -   **Git Checkout**: Pull source code from GitHub repository.

    -   **Compile & Test**: Compile the source code using Maven and run
        tests.

    -   **Trivy Scan**: Perform a file system scan to check for
        vulnerabilities.

    -   **SonarQube Analysis**: Integrate SonarQube for code quality
        analysis.

    -   **Quality Gate**: Wait for the quality gate to pass before
        proceeding.

    -   **Build**: Build the Maven artifact (jar/war file).

    -   **Publish to Nexus**: Deploy the artifact to Nexus repository.

    -   **Build and Tag Docker Image**: Build and tag a Docker image for
        deployment.

    -   **Docker Image Scan**: Scan the Docker image for vulnerabilities
        using Trivy.

    -   **Push Docker Image**: Push the Docker image to Docker Hub.

    -   **Deploy to Kubernetes**: Use Kubernetes credentials to deploy
        the app to the Kubernetes cluster.

    -   **Verify Deployment**: Verify that the deployment and service
        are successfully running in Kubernetes.

    -   **Email:** Send the necessary files to the personal gmail

##### **Kubernetes Setup:**

-   **Creation of Kubernetes:** Kubernetes created using kubeadm on VM's
    on AWS Cloud

-   **Create Service Account**: Jenkins service account for Kubernetes
    interaction.

-   **Role and RoleBinding**: Define the roles and role bindings to
    grant necessary permissions.

-   **Token Generation**: Generate a token for Jenkins to authenticate
    with Kubernetes.

-   **Deployment YAML**: Define Kubernetes deployment and service for
    the Boardgame application.

#### **Jenkins Pipeline Script:**

[pipeline {]{.mark}

[agent any]{.mark}

[tools {]{.mark}

[jdk \'jdk17\']{.mark}

[maven \'maven3\']{.mark}

[}]{.mark}

[]{.mark}

[environment {]{.mark}

[SCANNER_HOME = tool \'sonar-scanner\']{.mark}

[}]{.mark}

[]{.mark}

[stages {]{.mark}

[stage(\'Git Checkout\') {]{.mark}

[steps {]{.mark}

[git branch: \'main\', credentialsId: \'git-cred\', url:
\'https://github.com/HenryMark01/Boardgame.git\']{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Compile\') {]{.mark}

[steps {]{.mark}

[sh \"mvn compile\"]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Test\') {]{.mark}

[steps {]{.mark}

[sh \"mvn test\"]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'File System Scan\') {]{.mark}

[steps {]{.mark}

[sh \"trivy fs \--format table -o trivy-fs-report.html .\"]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'SonarQube Analysis\') {]{.mark}

[steps {]{.mark}

[withSonarQubeEnv(\'sonar\') {]{.mark}

[sh \'\'\'\$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=BoardGame
-Dsonar.projectKey=BoardGame \\]{.mark}

[-Dsonar.java.binaries=. \'\'\']{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Quality Gate\') {]{.mark}

[steps {]{.mark}

[script {]{.mark}

[waitForQualityGate abortPipeline: false, credentialsId:
\'sonar-token\']{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Build\') {]{.mark}

[steps {]{.mark}

[sh \"mvn package\"]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Publish To Nexus\') {]{.mark}

[steps {]{.mark}

[withMaven(globalMavenSettingsConfig: \'global-settings\', jdk:
\'jdk17\', maven: \'maven3\', mavenSettingsConfig: \'\', traceability:
true) {]{.mark}

[sh \"mvn deploy\"]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Build and Tag Docker image\') {]{.mark}

[steps {]{.mark}

[script {]{.mark}

[withDockerRegistry(credentialsId: \'docker-cred\', toolName:
\'docker\') {]{.mark}

[sh \"docker build -t henrymarkdocker/boardshack:latest .\"]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Docker Image Scan\') {]{.mark}

[steps {]{.mark}

[sh \"trivy image \--format table -o trivy-image-report.html
henrymarkdocker/boardshack:latest\"]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Push Docker Image\') {]{.mark}

[steps {]{.mark}

[script {]{.mark}

[withDockerRegistry(credentialsId: \'docker-cred\', toolName:
\'docker\') {]{.mark}

[sh \"docker push henrymarkdocker/boardshack:latest\"]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Deploy To Kubernetes\') {]{.mark}

[steps {]{.mark}

[withKubeConfig(caCertificate: \'\', clusterName: \'kubernetes\',
contextName: \'\', credentialsId: \'k8-cred\', namespace: \'webapps\',
restrictKubeConfigAccess: false, serverUrl:
\'https://172.31.22.228:6443\') {]{.mark}

[sh \"kubectl apply -f deployment-service.yaml\"]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[stage(\'Verify Deployment in Kubernetes\') {]{.mark}

[steps {]{.mark}

[withKubeConfig(caCertificate: \'\', clusterName: \'kubernetes\',
contextName: \'\', credentialsId: \'k8-cred\', namespace: \'webapps\',
restrictKubeConfigAccess: false, serverUrl:
\'https://172.31.22.228:6443\') {]{.mark}

[sh \"kubectl get pods -n webapps\"]{.mark}

[sh \"kubectl get svc -n webapps\"]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[]{.mark}

[post {]{.mark}

[always {]{.mark}

[script {]{.mark}

[def jobName = env.JOB_NAME]{.mark}

[def buildNumber = env.BUILD_NUMBER]{.mark}

[def pipelineStatus = currentBuild.result ?: \'UNKNOWN\']{.mark}

[def bannerColor = pipelineStatus.toUpperCase() == \'SUCCESS\' ?
\'green\' : \'red\']{.mark}

[def body = \"\"\"]{.mark}

[\<html\>]{.mark}

[\<body\>]{.mark}

[\<div style=\"border: 4px solid \${bannerColor}; padding:
10px;\"\>]{.mark}

[\<h2\>\${jobName} - Build \${buildNumber}\</h2\>]{.mark}

[\<div style=\"background-color: \${bannerColor}; padding:
10px;\"\>]{.mark}

[\<h3 style=\"color: white;\"\>Pipeline Status:
\${pipelineStatus.toUpperCase()}\</h3\>]{.mark}

[\</div\>]{.mark}

[\<p\>Check the \<a href=\"\${BUILD_URL}\"\>console
output\</a\>.\</p\>]{.mark}

[\</div\>]{.mark}

[\</body\>]{.mark}

[\</html\>]{.mark}

[\"\"\"]{.mark}

[emailext(]{.mark}

[subject: \"\${jobName} - Build \${buildNumber} -
\${pipelineStatus.toUpperCase()}\",]{.mark}

[body: body,]{.mark}

[to: \'henrymarkgrace@gmail.com\',]{.mark}

[from: \'jenkins@example.com\',]{.mark}

[replyTo: \'jenkins@example.com\',]{.mark}

[mimeType: \'text/html\',]{.mark}

[attachmentsPattern: \'trivy-image-report.html\']{.mark}

[)]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

[}]{.mark}

#### **Problems Faced During Execution**

1.  **Kubernetes Authentication Issues**: Resolved by ensuring the
    service account token was correctly configured and added to Jenkins
    credentials.

2.  **Docker Build Failures**: Fixed by optimizing Dockerfiles and
    ensuring that dependencies were correctly defined.

3.  **SonarQube Quality Gate Failures**: Fixed by addressing code
    quality issues identified in the SonarQube scan before proceeding to
    the next stages.

4.  **Nexus Authentication Errors**: During the artifact publishing
    stage, the pipeline encountered authentication errors when
    attempting to deploy the artifact to Nexus. The error message
    indicated an issue with the credentials or repository configuration.
    Verified the Nexus repository URL and ensured the correct
    credentials were configured in Jenkins. Additionally, checked the
    pom.xml file to ensure the correct repository credentials (username,
    password) were specified for both release and snapshot versions.

#### **Snippets Of the Project**

1.  Web Application

> ![](./media/image9.png){width="6.5in" height="3.4166666666666665in"}

2.  Sonarqube

> ![](./media/image8.png){width="6.5in"
> height="3.486111111111111in"}![](./media/image2.png){width="6.5in"
> height="3.4166666666666665in"}
>
> ![](./media/image5.png){width="6.5in" height="3.4166666666666665in"}

3.  Nexus

> ![](./media/image6.png){width="6.5in" height="2.75in"}

4.  Prometheus

> ![](./media/image10.png){width="6.5in" height="3.236111111111111in"}

5.  Grafana

> ![](./media/image1.png){width="6.5in" height="1.7222222222222223in"}
>
> ![](./media/image12.png){width="6.5in" height="3.2916666666666665in"}
>
> ![](./media/image11.png){width="6.5in" height="3.2222222222222223in"}
