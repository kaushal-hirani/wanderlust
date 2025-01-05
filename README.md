# DevOps Engineer Assignment: Jenkins CI/CD Pipeline

## Objective
The goal of this project is to create a Jenkins CI/CD pipeline that integrates various open-source tools to assess code coverage, code quality, cyclomatic complexity, and security vulnerabilities. This pipeline demonstrates the ability to implement an effective CI/CD process for maintaining high-quality standards in a software development project.

## Steps Followed

### 1. AWS Server Setup
- Created an AWS server.
  <img width="1680" alt="image" src="https://github.com/user-attachments/assets/4512339d-37fb-4ba6-845f-810c26760eff" />

- Installed Jenkins and Docker on the server.
  Docker : 
  ```
  sudo apt-get update
  sudo apt-get install docker.io -y
  sudo usermod -aG docker ubuntu && newgrp docker
  ```
  Jenkins: 
  ```
  sudo apt update -y
  sudo apt install fontconfig openjdk-17-jre -y
  sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt-get update -y
  sudo apt-get install jenkins -y
  ```
  
  <img width="1677" alt="image" src="https://github.com/user-attachments/assets/aafb684a-c87e-4926-b9c8-610098d4576b" />

### 2. Tool Installation
- Installed Trivy for security vulnerability scanning.
  ```
  sudo apt-get install wget apt-transport-https gnupg lsb-release -y
  wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
  echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
  sudo apt-get update -y
  sudo apt-get install trivy -y
  ```
  
- Installed Docker for running the SonarQube container.

  <img width="1679" alt="image" src="https://github.com/user-attachments/assets/0df3c181-5cfb-40df-b5ed-90b74c21cd71" />


### 3. SonarQube Setup
- Ran the SonarQube container using Docker.
  ```
  docker run -itd --name SonarQube-Server -p 9000:9000 sonarqube:lts-community
  ```
- Connected Jenkins to SonarQube using a token and configured access under `Manage Jenkins`.
<img width="1624" alt="image" src="https://github.com/user-attachments/assets/637c8bc6-4981-4d2c-98bd-4b7e5ba98c2f" />
<img width="1654" alt="image" src="https://github.com/user-attachments/assets/fbc51f6e-748d-428b-8ed2-c8b165d525a6" />


### 4. Plugin Installation in Jenkins
The following plugins were installed in Jenkins:
- **Git**
- **Docker**
- **OWASP Dependency Check**
- **SonarQube Scanner**
- **SonarQube Server**

### 5. Plugin Configuration
Configured the installed plugins to integrate seamlessly with Jenkins.

### 6. Pipeline Creation
- Created a pipeline script and uploaded it to the GitHub repository.
- Added a `Jenkinsfile` in the repository for the pipeline configuration.
<img width="1411" alt="image" src="https://github.com/user-attachments/assets/18d710fb-db12-42c6-88f0-1c9c220200df" />

### 7. Webhook Configuration
- Created a webhook in GitHub to enable Jenkins to automatically trigger builds upon code changes in the repository.
  <img width="1679" alt="image" src="https://github.com/user-attachments/assets/28a11947-894f-4390-b488-a753b3a12b1b" />

- Configured SCM polling to ensure updates are detected.
<img width="1406" alt="image" src="https://github.com/user-attachments/assets/29ab2659-9fe1-43ae-a9e5-ced64420f003" />

### 8. Repository Details
- Repository URL: [wanderlust](https://github.com/kaushal-hirani/wanderlust.git)
- Branch used: `devops`

### 9. Tools Integrated
- **Jenkins**: CI/CD automation
- **SonarQube**: Code quality and technical debt analysis
- **Trivy**: Security vulnerability scanning
- **Git**: Source code management
- **OWASP Dependency Check**: Security dependency scanning

## Pipeline Features
- **Code Quality Checks**: Configured SonarQube to analyze code quality and fail builds if quality gates are not met.
- **Security Vulnerability Scan**: Used Trivy and OWASP Dependency Check to scan for vulnerabilities in project dependencies.
- **Automated Triggering**: Configured a webhook for triggering the pipeline on commits to the `devops` branch.
- **Email Notifications**: Configured Email Notification after successful build and failures.


## Repository Structure
```
- wanderlust/
  |- Jenkinsfile
  |- README.md
  |- Other project files
```

## Troubleshooting Tips
- Ensure the SonarQube container is running before triggering the Jenkins pipeline.
- Verify plugin installations in Jenkins if integrations fail.
- Check GitHub webhook configurations if builds are not triggered automatically.
- Review the logs in Jenkins for debugging build failures.

---

For more details, visit the repository: [wanderlust](https://github.com/kaushal-hirani/wanderlust.git).
