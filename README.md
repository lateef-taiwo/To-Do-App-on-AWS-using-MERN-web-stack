# TO-DO-APP-ON-AWS-USING-MERN-WEB-STACK
This repository explains the steps involved in creating a to-do application on AWS using the MERN web stack. The MERN web stack consists of MongoDB, ExpressJS, ReactJS, and NodeJS as its working components. 
Full-stack online applications can be deployed more quickly and easily with the MERN Stack, a Javascript stack.
The MERN Stack is made up of the following 4 technologies: MongoDB, Express, React, and Node.js.

* Mongo-DB: MongoDB is a source-available cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with optional schemas.
* ExpressJS: A server-side Web Application framework for Node.js.
* ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.
* Nodejs:
It is intended to simplify and streamline the development process.
Each of these four technologies plays a significant role in the creation of web apps and offers developers an end-to-end environment in which to operate. 

![MERN image](./images/MERN%20img.jpeg)

---
____
## Step 1 - Create a Virtual Server on AWS
<!-- UL -->
* Login to the AWS console
* Search for EC2 (Elastic Compute Cloud) 
* Select your preferred region (the closest to you) and launch a new EC2 instance of t2.micro family with Ubuntu Server 20.04 LTS (HVM)
* Type a name e.g My_Lamp_Server
 Click create a new key pair, use any name of your choice as the name for the pem file and select .pem.
    * Linux/Mac users, choose .pem for use with openssh. This allows you to connect to your server using open ssh clients.
    * For windows users choose .ppk for use with putty. Putty is a software that lets you connect remotely to servers
* Save your private key (.pem file) securely and do not share it with anyone! If you lose it, you will not be able to connect to your server ever again! 

![server](./images/server.jpeg)

* On your local computer, open the terminal and change directory to the Downloads folder, type 
    
    `$ cd ~/Downloads` 
* Change permissions for the private key file (.pem), otherwise you can get an error “Bad permission”
    
    ` sudo chmod 0400 <private-key-name>. pem` 
* Connect to the instance by running
    ` ssh -i <private-key-name>. pem ubuntu@<Public-IP-address>`

![ssh](./images/ssh.jpeg)

<!-- Horizontal RUle -->
---
___
## Step 2 - Backend Configuration
* Update Ubuntu

    `sudo apt update`

![update](./images/update.png)

* Upgrade ubuntu

    `sudo apt upgrade`

![Upgrade](./images/upgrade.png)




