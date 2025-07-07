🚀 Two-Tier Flask Application with Docker-Compose on AWS EC2

🔹 Project Overview:
I built and deployed a two-tier Flask application with a MySQL backend using Docker and Docker Compose on an AWS EC2 instance. This project demonstrates containerized application deployment, infrastructure-as-code practices, and integration of a relational database with a Python-based web application.

🔧 Tech Stack:


Flask (Python microframework)

MySQL 5.7

Docker & Docker Compose

AWS EC2 (Ubuntu instance)



⚙️ Deployment Steps (highlights):

✅ Provision an EC2 instance (Ubuntu 22.04)

✅ Install dependencies

sudo apt-get update

sudo apt-get install docker.io -y

sudo curl -L "https://github.com/docker/compose/releases/download/2.38.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose -v



✅ Clone the project

git clone https://github.com/DevOps-Abinash/Two-tier-flask-devops-app.git

cd Two-tier-flask-devops-app



✅ Build the Flask Docker image

sudo docker build -t flaskapp .

sudo docker images



✅ Configure docker-compose.yml:

vim docker-compose.yml

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
  
✅ Start the stack

sudo docker-compose up -d

sudo docker ps



✅ Update Security Groups

Allow inbound TCP traffic on port 5000 to access the Flask app externally.


🌟 Outcome:

✅ Successfully deployed a two-tier web application with persistent MySQL storage

✅ Demonstrated container orchestration via Docker Compose

✅ Automated multi-container application deployment on AWS infrastructure

✅ Version-controlled the project on GitHub


🔗 GitHub Repo: https://github.com/DevOps-Abinash/Two-tier-flask-devops-app

