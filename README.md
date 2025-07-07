## 🚀 Two-Tier Flask Application with Docker Compose on AWS EC2

[![Docker](https://img.shields.io/badge/Docker-Compose-blue?logo=docker)](https://www.docker.com/)
[![Python](https://img.shields.io/badge/Python-3.9-blue?logo=python)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![GitHub last commit](https://img.shields.io/github/last-commit/DevOps-Abinash/Two-tier-flask-devops-app)](https://github.com/DevOps-Abinash/Two-tier-flask-devops-app/commits/main)
[![AWS EC2](https://img.shields.io/badge/AWS-EC2-orange?logo=amazon-aws)](https://aws.amazon.com/ec2/)

---

## 📌 Project Overview

This project demonstrates how to deploy a two-tier Flask application with a MySQL backend using Docker and Docker Compose on an AWS EC2 instance. It showcases containerized application deployment, infrastructure-as-code practices, and a clean separation between the web and database tiers.

---

## 🛠️ Tech Stack

- **Flask (Python microframework)**
- **MySQL 5.7**
- **Docker & Docker Compose**
- **AWS EC2 (Ubuntu)**

---

## ⚙️ Deployment Steps

### 1️⃣ Launch an EC2 Instance

- Launch an Ubuntu-based EC2 instance.
- Open port **5000** (custom TCP) in the security group to allow inbound traffic for the Flask app.

---

### 2️⃣ Install Dependencies

```bash
sudo apt-get update
sudo apt-get install docker.io -y
sudo curl -L "https://github.com/docker/compose/releases/download/2.38.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose -v
```
3️⃣ Clone the Repository
```bash
git clone https://github.com/DevOps-Abinash/Two-tier-flask-devops-app.git
cd Two-tier-flask-devops-app
```
4️⃣ Build the Flask Docker Image
```bash
sudo docker build -t flaskapp .
sudo docker images
```
5️⃣ Create docker-compose.yml
Replace (or create) the docker-compose.yml file with the following contents:

```bash
version: '3.3'
services:
  backend:
    image: 'flaskapp:latest'
    ports:
      - '5000:5000'
    environment:
      - MYSQL_HOST=mysql
      - MYSQL_USER=admin
      - MYSQL_PASSWORD=admin
      - MYSQL_DB=myDb
    depends_on:
      - mysql

  mysql:
    image: 'mysql:5.7'
    environment:
      - MYSQL_DATABASE=myDb
      - MYSQL_USER=admin
      - MYSQL_PASSWORD=admin
      - MYSQL_ROOT_PASSWORD=admin
    ports:
      - '3306:3306'
    volumes:
      - './message.sql:/docker-entrypoint-initdb.d/message.sql'
      - 'mysql-data:/var/lib/mysql'

volumes:
  mysql-data:
```
6️⃣ Start the Application
```bash
sudo docker-compose up -d
sudo docker ps
```

🔐 Configure Security Groups
✅ On the AWS EC2 console, open the security group attached to your instance.
✅ Add a Custom TCP Rule to allow port 5000 from 0.0.0.0/0 or a specific IP.

🗄️ Check the MySQL Database
1️⃣ Access the MySQL container:

```bash
sudo docker ps
```
Find the container ID for MySQL (for example, d2ad3c1c1c05), then:

```bash
sudo docker exec -it d2ad3c1c1c05 bash
```
2️⃣ Log in to MySQL:

```bash
mysql -u admin -p
# password: admin
```
3️⃣ Run SQL commands:
```bash
show databases;
use myDb;
select * from messages;
```

✅ Outcome

Successfully deployed a two-tier web application with persistent MySQL storage

Automated the deployment with Docker Compose

Demonstrated infrastructure-as-code practices

Version-controlled and reusable for future deployments

🔗 Project Repository
```bash
https://github.com/DevOps-Abinash/Two-tier-flask-devops-app
```






