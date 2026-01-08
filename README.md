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
#### Example: mysql -h database-1.ca9eie2mihs7.us-east-1.rds.amazonaws.com -u linux -p
#### Example: redhat123

```sh
CREATE DATABASE student_db;
```
```sh
GRANT ALL PRIVILEGES ON springbackend.* TO 'username'@'localhost' IDENTIFIED BY 'password';
```
#### Example: GRANT ALL PRIVILEGES ON student_db.* TO 'admin'@'%' IDENTIFIED BY 'redhat123';

