# **_3-teir deployment with - AWS EC2 + RDS_**


Here we are deploying a 3 -teir project with the help of AWS services like , EC2 , RDS . 

---

### 3-Tier means project have have - 
1. Frontend
2. Backend
3. Database 

---

### So the steps are as follow : - 

<ins>**STEP 1 : CREATE DATABASE IN RDS**</ins>

i. configure setting as your need .
          
ii. after configuring DB , get the credentials value and copy or save it  - End point , username and password  of DB


<ins>**STEP 2 : LANUCH INSTANCE SERVER FOR FRONTEND & BACKEND**</ins>

i. create instance with setting that you want .

ii. Seup

<ins>**STEP 3 : SETUP MYSQL CLIENT TO RUN SQL COMMAND OR MANAGE DB USING EC2 SERVER**</ins>

i. RUN THE COMMAND TO INSTALL PACKAGES OF NYSQL 
          
    apt update && apt install mysql-client -y
    
ii. Login to MYSQL

      mysql -h <your ENDPOINT of DB> -u <admin name> -p<password of db>
iii. Then create a database for project 

     1.  show databases ;
     2.  create database <db-name> ;
     3.  use <db-name> ;

iv. Allow the port No 3306 to instance in Security Group ( inbound Rule )

<ins>**STEP 4 : SETUP FOR BACKEND IN EC2 SERVER**</ins> 

i. Install package - java , maven , git 
      
      apt update && apt install openjdk-17-jdk -y && apt install maven -y && apt install git 

ii. clone the project or source code from github repository 

    git clone <repo-url>

iii. 
