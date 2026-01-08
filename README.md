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
### EC2
####  Root 
~~~sh
sudo -i
~~~
####  Update 
~~~sh
apt update
~~~
#### Install Java
```bash
sudo apt update
sudo apt install openjdk-17-jdk
java -version
```

####  Install Jenkins
```bash
sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

#### Install Docker
```bash
sudo apt install docker.io -y
sudo usermod -aG docker jenkins
```

