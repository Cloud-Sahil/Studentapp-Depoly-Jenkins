# EasyCRUD Dockerized Project

This project demonstrates a complete CI/CD pipeline using **Jenkins**, **Docker**, **MariaDB**, and **AWS EC2** to build, push, and deploy frontend and backend applications.

---

##  Project Overview

- Frontend and Backend built with Docker  
- Database: MariaDB  
- CI/CD Pipeline managed by Jenkins  
- Images pushed to Docker Hub  
- Deployment on AWS EC2 Instance  

---

##  Prerequisites

- AWS EC2 instance (example IP: `54.224.153.198`)
- AWS EC2 Storage `30GB`
- Open port `3306` for MariaDB in EC2 Security Group  
- Docker Hub account
- Create RDS DataBase

---

##  Installation Steps & Commands
###  Root 
~~~sh
sudo -i
~~~
###  Update 
~~~sh
apt update
~~~
### Install Java
```bash
sudo apt install openjdk-17-jdk -y
```

###  Install Jenkins
```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

### Install Docker
```bash
apt install docker.io -y
```
```sh
sudo usermod -aG docker jenkins
```
### Grant Jenkins Sudo Privileges
```bash
sudo visudo
# Add the following line at the end of the file: # User privilege specification : root
jenkins ALL=(ALL) NOPASSWD: ALL
```
###  Install Mysql-client
```sh
apt install mysql-client -y
```
###  Mysql-client Database 
```sh
mysql -h (endpoint) -u (username) -p
```
```sh
Enter password (password)
```
#### RDS Database Endpoint copy & paste
#### Example: mysql -h database-1.ca9eie2mihs7.us-east-1.rds.amazonaws.com -u admin -p
#### Example: redhat123

```sh
CREATE DATABASE student_db;
```
```sh
GRANT ALL PRIVILEGES ON springbackend.* TO 'username'@'localhost' IDENTIFIED BY 'password';
```
#### Example: GRANT ALL PRIVILEGES ON student_db.* TO 'admin'@'%' IDENTIFIED BY 'redhat123';
```sh
USE student_db;
```
```sh
CREATE TABLE `students` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `email` varchar(255) DEFAULT NULL,
  `course` varchar(255) DEFAULT NULL,
  `student_class` varchar(255) DEFAULT NULL,
  `percentage` double DEFAULT NULL,
  `branch` varchar(255) DEFAULT NULL,
  `mobile_number` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=80 DEFAULT CHARSET=latin1 COLLATE=latin1_swedish_ci;
```
```sh
show databases;
```
```sh
exit;
```
## `Fork Github Repo. & Update application.properties & .env - <https://github.com/Cloud-Sahil/EasyCRUD-docker-updated.git>`
### backend = `application.properties`
```properties
server.port=8081
spring.datasource.url=jdbc:mysql://<RDS ENDPONIT>:3306/student_db
spring.datasource.username=admin
spring.datasource.password=redhat123
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MariaDBDialect
```
#### Example: RDS End Point 
### frontend = `.env`
```bash
VITE_API_URL="http://<EC2 PUBLIC IP>:8081/api"
```
#### Example: EC2 Public IP
### Docker Cleanup Command

```bash
docker kill $(docker ps -q) && docker rm -v $(docker ps -a -q) && docker rmi $(docker images -q)
```
---

##  Store DockerHub Credentials in Jenkins

1. Go to Jenkins Dashboard → Manage Jenkins → Manage Credentials  
2. Select domain: `(global)`  
3. Add Credentials:  
    - **Username:** `<dockerhub-username>`  
    - **Password:** `<dockerhub-password>`  
    - **ID:** `dockerhub-cred`  

---

##  Jenkins Pipeline Overview
```groovy
pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git url: "https://github.com/Cloud-Sahil/EasyCRUD-docker-updated.git", branch: "main"
            }
        }

        stage('Build Frontend Image') {
            steps {
                sh "docker build -t sahil0117/easycrud1-jenkins:frontend ./frontend"
            }
        }

        stage('Build Backend Image') {
            steps {
                sh "docker build --no-cache -t sahil0117/easycrud1-jenkins:backend ./backend"
            }
        }

        stage('Docker Hub Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin"
                }
            }
        }

        stage('Push Frontend Image to Docker Hub') {
            steps {
                sh "docker push sahil0117/easycrud1-jenkins:frontend"
            }
        }

        stage('Push Backend Image to Docker Hub') {
            steps {
                sh "docker push sahil0117/easycrud1-jenkins:backend"
            }
        }

        stage('Run Frontend Container') {
            steps {
                sh '''
                    docker rm -f easycrud1-frontend || true
                    docker run -d --name easycrud1-frontend -p 80:80 sahil0117/easycrud1-jenkins:frontend
                '''
            }
        }

        stage('Run Backend Container') {
            steps {
                sh '''
                    docker rm -f easycrud1-backend || true
                    docker run -d --name easycrud1-backend -p 8081:8081 sahil0117/easycrud1-jenkins:backend
                '''
            }
        }
    }
}
```

---

### Restart Jenkins
```sh
systemctl restart jenkins
```
---
## Build Jenkins Pipeline
---

### Access EC2 < Public IP>
