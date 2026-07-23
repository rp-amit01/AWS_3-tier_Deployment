# **_3-teir deployment with - AWS EC2 + RDS + Load Balancer + Cloud Watch + SNS & Event Bridge**


Here we are deploying a 3-teir project with the help of AWS services like EC2 , RDS , Load Balancer , Cloud Watch , SNS & Event Bridge. 

---

### 3-Tier means project have have - 
1. Frontend
2. Backend
3. Database 

---

### So the steps are as follow : - 

 <br>
 
<ins>**STEP 1 : CREATE DATABASE IN RDS**</ins>

i. configure setting as your need.
          
ii. After configuring DB , get the credentials value and copy or save it . 

- End point of db
- username of db
- password of DB

 <br>
 
<ins>**STEP 2 : LANUCH THREE ( 3 ) INSTANCE SERVER FOR FRONTEND & BACKEND WITH SAME CONFIGURATION**</ins>

i. create instance with setting that you want like 

- Name
- AMI
- Instance Type
- key pair
- Networking - VPC , subnet , Security Group

 but dont forget to add Userdata / bash script to install packages and setup all configuration .
 
 # 
 ### Sample Bash Script 
 #

    #!/bin/bash

    # Update packages
    apt update
    
    # Install required packages
    apt install -y mariadb-client git nodejs npm apache2 openjdk-17-jdk maven
    
    # -------------------------------
    # Create Database in RDS
    # -------------------------------
    
    mysql -h database-1.cr0ygqq8gcfw.ap-southeast-2.rds.amazonaws.com \
    -u admin \
    -pDubai123 <<EOF
    
    CREATE DATABASE IF NOT EXISTS studentdb;
    USE studentdb;
    
    EOF
    
    # -------------------------------
    # Clone Project
    # -------------------------------
    
    cd /root
    git clone https://github.com/SanjayTomar22/EasyCRUD.git
    
    # -------------------------------
    # Frontend Setup
    # -------------------------------
    
    cd /root/EasyCRUD/frontend
    
    PUBLIC_IP=$(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)
    
    cat > .env <<EOF
    VITE_API_URL=http://${PUBLIC_IP}:8080/api
    EOF
    
    npm install
    npm run build
    
    systemctl enable apache2
    systemctl start apache2
    
    cp -r dist/* /var/www/html/
    
    # -------------------------------
    # Backend Configuration
    # -------------------------------
    
    cd /root/EasyCRUD/backend/src/main/resources
    
    cat >> application.properties <<EOF
    
    server.port=8080
    spring.datasource.url=jdbc:mariadb://database-1.cr0ygqq8gcfw.ap-southeast-2.rds.amazonaws.com:3306/studentdb
    spring.datasource.username=admin
    spring.datasource.password=Dubai123
    spring.jpa.hibernate.ddl-auto=update
    spring.jpa.show-sql=true
    
    EOF
    
    # -------------------------------
    # Build Backend
    # -------------------------------
    
    cd /root/EasyCRUD/backend
    
    mvn clean package -DskipTests
    
    # -------------------------------
    # Run Spring Boot
    # -------------------------------
    
    cd target
    
    java -jar student-registration-backend-0.0.1-SNAPSHOT.jar


<ins>**STEP 3 : Setup or create Load Balancer ( Here we use Classic LoadBalancer )**</ins>

i. open the Load balancer page -> select Classic Load Balancer 

ii. fill the details like - 

- Name
- Select Scheme ( internet facing )
- Networking ( select VPC , tick all 3 Subnet , select security group )
- instance ( select instances that you want to add )

iii. click on " create LB "

iv. Now get the "DNS Name" of Load Balancer from : LoadBalancer -> your LB -> details -> DNS Name

<ins>**STEP 4 : create a Dashboard in CloudWatch for Monitoring Resources**</ins>

i. create dashboard 

ii. add metrics , then monitor resources 


<ins>**STEP 5 : Set up Email Notification Feature using SNS and EventBridge**</ins>

i. create topic in SNS 

ii. create subscription inside Topic

iii. Then go to Event Bridge Service , and create Rule 
