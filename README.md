# 📌 Number Guess Game - CI/CD Pipeline with Jenkins

## 🚀 Project Overview
This repository contains a **Java-based Number Guessing Game** that has been integrated with a **CI/CD pipeline using Jenkins**. The project automates building, testing, and deploying the application to an Apache Tomcat server running on an AWS EC2 instance.

## 🏗️ Tech Stack
- **Programming Language:** Java (Servlets & JSP)
- **Build Tool:** Apache Maven
- **CI/CD:** Jenkins
- **Version Control:** Git & GitHub
- **Deployment Server:** Apache Tomcat
- **Cloud Provider:** AWS (EC2 Instance)

## 📂 Project Structure
```bash
numbers-guess-game/
│── src/main/java/com/studentapp/   # Java Servlet source code
│── src/main/webapp/                # JSP files & static resources
│── src/test/java/com/studentapp/   # Unit tests using JUnit & Mockito
│── pom.xml                          # Maven build configuration
│── Jenkinsfile                      # Jenkins pipeline script
│── README.md                        # Documentation
```

## 🔥 Features
✅ Web-based Number Guessing Game
✅ Automated Build & Testing with Jenkins
✅ Continuous Deployment to Tomcat Server
✅ Infrastructure hosted on AWS EC2

---
## 🛠️ Setup Instructions

### 1️⃣ Clone the Repository
```sh
git clone -b dev https://github.com/SASowah/numbers-guess-gameApp.git
cd numbers-guess-gameApp
```

### 2️⃣ Install Dependencies
Ensure you have Java and Maven installed:
```sh
java -version  # Should be Java 11 or above
mvn -version   # Ensure Maven is installed
```

### 3️⃣ Build the Project
```sh
mvn clean package
```

### 4️⃣ Run the Application Locally (For Testing)
Start a local Tomcat server and deploy the `.war` file:
```sh
cp target/NumberGuessGame-1.0-SNAPSHOT.war /path/to/tomcat/webapps/
/path/to/tomcat/bin/startup.sh
```
Then access the app at:
```sh
http://localhost:8080/NumberGuessGame-1.0-SNAPSHOT
```

---
## 🔄 Jenkins CI/CD Pipeline
The project is automated using **Jenkins Pipeline as Code**. Below is an overview of the Jenkins stages:

### ✅ **Jenkins Pipeline Stages**
1️⃣ **Checkout Code** – Clones the latest code from GitHub
2️⃣ **Build** – Compiles and packages the `.war` file using Maven
3️⃣ **Run Tests** – Executes unit tests using JUnit & Mockito
4️⃣ **Deploy** – Transfers the `.war` file to the Tomcat server
5️⃣ **Post Actions** – Sends success/failure notifications

### 📌 **Jenkinsfile** (Pipeline Script)
```groovy
pipeline {
    agent {
        label 'build-agent' // Dedicated EC2 agent for builds
    }
    
    environment {
        PROJECT_NAME = "NumberGuessGame"
        ARTIFACT = "target/NumberGuessGame-1.0-SNAPSHOT.war"
        DEPLOY_DIR = "/home/ec2-user/apache-tomcat-7.0.94/webapps"
        TOMCAT_USER = "ec2-user"
        SERVER_IP = "your-ec2-public-ip"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'dev',
                    url: 'https://github.com/SASowah/numbers-guess-gameApp.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                sh "scp -i your-key.pem $ARTIFACT ${TOMCAT_USER}@${SERVER_IP}:${DEPLOY_DIR}"
            }
        }
    }
    
    post {
        success {
            echo '✅ Build and Deployment Successful!'
        }
        failure {
            echo '❌ Build Failed! Check logs.'
        }
    }
}
```

---
## 🚀 Deployment
After Jenkins completes the build and deployment, access the application via:
```sh
http://your-ec2-public-ip:8080/NumberGuessGame-1.0-SNAPSHOT
```

---
## 🛠️ Troubleshooting
### ❌ **Jenkins Fails to Build**
- Ensure Maven is installed: `mvn -version`
- Check the `.war` file path in Jenkins workspace:
  ```sh
  ls /var/lib/jenkins/workspace/numbers-guess-game-Build/target/
  ```
- Verify correct Java version: `java -version`

### ❌ **Tomcat Does Not Deploy App**
- Check logs: `cat /path/to/tomcat/logs/catalina.out`
- Ensure correct file permissions:
  ```sh
  sudo chown -R ec2-user:ec2-user /home/ec2-user/apache-tomcat-7.0.94/
  ```

### ❌ **App Not Accessible on Port 8080**
- Ensure Tomcat is running:
  ```sh
  sudo systemctl status tomcat
  ```
- Open port 8080 in AWS Security Groups

---
