

## **This document provides step-by-step guidance for setting up, configuring, and using the Jenkins pipeline for automating code analysis, security scans, Docker image management, and container deployment.**

---

### **Pipeline Overview**  

The pipeline performs the following tasks:  
1. **Setup**: Ensures dependencies like JDK 17, SonarQube, and OWASP Dependency-Check are installed and configured.  
2. **Code Repository Cloning**: Pulls the latest source code from GitHub.  
3. **Code Quality Analysis**: Performs static code analysis using SonarQube.  
4. **Dependency Security Check**: Scans dependencies for vulnerabilities with OWASP Dependency-Check.  
5. **Docker Workflow**: Builds, tags, pushes, and deploys Docker containers.  
6. **Report Generation**: Archives and publishes reports from SonarQube and Dependency-Check.  

---

### **Setup**  

#### **Prerequisites**  
1. **Jenkins Installed**: Jenkins should be installed on a server (cloud or local).  
2. **Required Tools**:  
   - **Java Development Kit (JDK) 17**: Essential for building and running Java-based tools like SonarQube.  
   - **SonarQube Server**: Hosted on `http://13.61.93.10:9000`.  
   - **OWASP Dependency-Check**: Installed as a Jenkins tool.  
   - **Docker**: Installed and configured on the Jenkins agent.  

---

### **Step 1: Install JDK 17**  

1. **Install JDK 17** on the Jenkins server:  
   ```bash
   sudo apt update
   sudo apt install openjdk-17-jdk
   java -version
   ```
   Verify the output to ensure JDK 17 is installed successfully.  

2. **Add JDK in Jenkins**:  
   - Navigate to **Manage Jenkins > Global Tool Configuration**.  
   - Under the JDK section:  
     - **Name**: `jdk17`.  
     - Specify the installation path or let Jenkins automatically install it.  

---

### **Step 2: Configure SonarQube**  

1. **SonarQube Server Setup**:  
   - Install and start the SonarQube server. Ensure it’s accessible at `http://13.61.93.10:9000`.  

2. **SonarQube in Jenkins**:  
   - Navigate to **Manage Jenkins > Configure System**.  
   - Add a SonarQube server:  
     - **Name**: `SonarQube`.  
     - **Server URL**: `http://13.61.93.10:9000`.  
     - **Authentication Token**: Generated from the SonarQube server.  

---

### **Step 3: Install OWASP Dependency-Check**  

1. **Install the Plugin**:  
   - Go to **Manage Jenkins > Plugins** and search for "OWASP Dependency-Check". Install it.  

2. **Add Dependency-Check Tool**:  
   - Navigate to **Manage Jenkins > Global Tool Configuration**.  
   - Under the **Dependency-Check** section:  
     - **Name**: `OWASP-DC`.  
     - Ensure the correct installation path is provided.  

---

### **Step 4: Configure Docker**  

1. **Install Docker**:  
   - Follow the [official Docker installation guide](https://docs.docker.com/get-docker/) for your platform.  

2. **Add Jenkins to the Docker Group**:  
   ```bash
   sudo usermod -aG docker jenkins
   sudo systemctl restart jenkins
   ```  

3. **Docker Registry Credentials**:  
   - Go to **Manage Jenkins > Credentials**.  
   - Add a credential:  
     - **Type**: Username and password.  
     - **ID**: `docker-hub-credentials`.  

---

### **Pipeline Execution**  

#### **Stages of the Pipeline**  

1. **Clone Repository**:  
   - Pulls the source code from the GitHub repository.  
   - Ensure the repository URL and branch are correct.  

2. **Code Quality Analysis (SonarQube)**:  
   - Runs SonarQube Scanner on the codebase.  
   - Sends results to the SonarQube server for further analysis.  

3. **Dependency Security Check**:  
   - Uses OWASP Dependency-Check to identify vulnerabilities in dependencies.  
   - Archives and publishes the report in Jenkins.  

4. **Docker Workflow**:  
   - Builds a Docker image for the project.  
   - Tags and pushes the image to Docker Hub.  
   - Deploys the container to the environment.  

---

### **Usage**  

#### **Access Results**  

1. **SonarQube Dashboard**:  
   - Open `http://13.61.93.10:9000` in your browser.  
   - Navigate to the project `JenkinsSonarQubeDocker`.  

2. **OWASP Dependency-Check Report**:  
   - Available in Jenkins under **Build > HTML Report**.  

3. **Deployed Application**:  
   - Accessible at the server's public IP (`http://13.61.93.10`) on port 80.  

---

### **Troubleshooting**  

1. **JDK Issues**:  
   - If `java -version` does not return JDK 17, verify the installation path or set the default Java version:  
     ```bash
     sudo update-alternatives --config java
     ```  

2. **SonarQube Connection**:  
   - Check if the server is running and accessible at `http://13.61.93.10:9000`.  

3. **OWASP Dependency-Check**:  
   - If `dependency-check.sh` is not found, verify the tool’s installation path in Jenkins.  

4. **Docker Errors**:  
   - Ensure Docker is installed and running.  
   - Check if Jenkins has the required permissions to execute Docker commands.  

---

### **Initial Run Report**  

#### **Results**  

1. **SonarQube Analysis**:  
   - Successfully executed, and results were uploaded to SonarQube.  

2. **Dependency Security Check**:  
   - Reports generated and published in Jenkins.  

3. **Docker Workflow**:  
   - Docker image built and pushed to Docker Hub.  
   - Application container deployed successfully.  

#### **Screenshots**  


#### **Deployed Container**##
![Screenshot 2024 11 29 at 7.14.14 AM](https://freeimghost.net/images/2024/11/28/Screenshot-2024-11-29-at-7.14.14AM.png)


#### **SonarQube**##
![Screenshot 2024 11 29 at 1.55.50 AM](https://freeimghost.net/images/2024/11/28/Screenshot-2024-11-29-at-1.55.50AM.png)

![Screenshot 2024 11 29 at 1.57.22 AM](https://freeimghost.net/images/2024/11/28/Screenshot-2024-11-29-at-1.57.22AM.png)

#### **OWASP Dependency Check**##

![Screenshot 2024 11 29 at 2.00.18 AM](https://freeimghost.net/images/2024/11/28/Screenshot-2024-11-29-at-2.00.18AM.png)

![Screenshot 2024 11 29 at 2.00.39 AM](https://freeimghost.net/images/2024/11/28/Screenshot-2024-11-29-at-2.00.39AM.png)


#### **Jenkins**##

![Screenshot 2024 11 29 at 2.01.47 AM](https://freeimghost.net/images/2024/11/28/Screenshot-2024-11-29-at-2.01.47AM.png)

![Screenshot 2024 11 29 at 1.58.30 AM](https://freeimghost.net/images/2024/11/28/Screenshot-2024-11-29-at-1.58.30AM.png)