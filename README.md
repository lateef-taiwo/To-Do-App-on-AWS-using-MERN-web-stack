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

* Since we are using Ubuntu as our server, we will get the location of Node.js software from Ubuntu repositories.

    `curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

![curl](./images/curl.png)

* Install Node.js with the command below

    `sudo apt-get install -y nodejs`

![node](./images/node.png)

* Verify the node installation with the command below

`node --version && npm --version` or `node -v && npm -v`

The command above checks the version of node and npm installed. npm is a package manager for node.

![node&npm](./images/node%20%26%20npm.png)

* Application Code Setup
Create a new directory for your To-Do project and run the `ls` command to verify that Todo directory is created.

    `mkdir TOdo`

    `ls`
![Todo](./images/todo.png)

* Now change your current directory to the newly created one:

    `cd Todo`
![Todo](./images/todo%20dir.png)

* Next, you will use the command `npm init` to initialise your project, so that a new file named package.json will be created. This file will  contain information about your application and the dependencies that it needs to run.

    `npm init`

    ![init](./images/npm%20init.png)

* Install ExpressJS
Express is a framework for Node.js, so we can install it using the npm package manager

    `npm install express`

    ![express](./images/express.png)

* Now create an file index.js and type `ls` to check the contents of the directory

    `touch index.js`
    `ls`

    ![index.js](./images/index.js.png)

* Next. Install the dotenv module

    `npm install dotenv`

    ![dotenv](./images/dotenv.png)

* Open the index.js file in the vim editor and type the code below into it and save.

    `vim index.js`

        const express = require('express');
        require('dotenv').config();
        
        const app = express();
        
        const port = process.env.PORT || 5000;
        
        app.use((req, res, next) => {
        res.header("Access-Control-Allow-Origin", "\*");
        res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
        next();
        });
        
        app.use((req, res, next) => {
        res.send('Welcome to Express');
        });
        
        app.listen(port, () => {
        console.log(`Server running on port ${port}`)
        });

* Now start the server to see if it works. Open your terminal in the same directory as your index.js file and type:

    `node index.js`

* Ensure you have port 5000 opened in EC2 security groups because the application will be running on that port. Also open port 80 for web access.

![Sec-groups](./images/sec%20groups.png)

* Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:

`http://<PublicIP-or-PublicDNS>:5000`

![Browser](./images/Browser1.png)

----------

### Routes
There are three actions that our To-Do app should be able to do:

* Create a new task
* Display list of all tasks
* Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. 

 * Create a folder routes .
 * Change directory to routes folder.
 * create a file api.js using the `touch api.js` commad

    `mkdir routes`

    `cd routes`

    `touch api.js`

    ![routes](./images/routes.png)

* Open the file in the vim editor and paste the code below in it.

        const express = require ('express');
        const router = express.Router();
        
        router.get('/todos', (req, res, next) => {
        
        });
        
        router.post('/todos', (req, res, next) => {
        
        });
        
        router.delete('/todos/:id', (req, res, next) => {
        
        })
        
        module.exports = router;

    ![api](./images/api.png)

*   Now we need to create models

    Models are data structures that we use to define the shape of our data. We need to create a model for the app to make use of Mongodb which is a NoSQL database. We will also use models to define the database schema. Schema is a representation of how the database will be constructed.

* Change directory to Todo folder using `cd ..` and install Mongoose.

    `npm install mongoose`

    ![mongoos](./images/mongoose.png)

 * Create a new directory models.

* Change directory into the newly created models directory.

* create a new file todo.js.
    
    Use the command below to do the above in one shot

    `mkdir models && cd models && touch todo.js`
   
* Open the todo.js file in the vim editor and paste the code below:

        const mongoose = require('mongoose');
        const Schema = mongoose.Schema;
        
        //create schema for todo
        const TodoSchema = new Schema({
        action: {
        type: String,
        required: [true, 'The todo text field is required']
        }
        })
        
        //create model for todo
        const Todo = mongoose.model('todo', TodoSchema);
        
        module.exports = Todo;

*  we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.
In Routes directory, open api.js with `vim api.js`, delete the code inside with `:%d` command.

        const express = require ('express');
        const router = express.Router();
        const Todo = require('../models/todo');
        
        router.get('/todos', (req, res, next) => {
        
        //this will return all the data, exposing only the id and action field to the client
        Todo.find({}, 'action')
        .then(data => res.json(data))
        .catch(next)
        });
        
        router.post('/todos', (req, res, next) => {
        if(req.body.action){
        Todo.create(req.body)
        .then(data => res.json(data))
        .catch(next)
        }else {
        res.json({
        error: "The input field is empty"
        })
        }
        });
        
        router.delete('/todos/:id', (req, res, next) => {
        Todo.findOneAndDelete({"_id": req.params.id})
        .then(data => res.json(data))
        .catch(next)
        })
        
        module.exports = router;

___
### MONGODB DATABASE
We will be storing the data for the app in a mongodb database.
* You will need to sign up for a shared clusters free account, which is ideal for our use case. Visit https://www.mongodb.com/atlas-signup-from-mlab. 

* Follow the sign up process, select AWS as the cloud provider, and choose a region near you.
Complete a get started checklist as shown on the image below

    ![mongo_user](./images/mongodbuser.png)

* Allow access to the MongoDB database from anywhere (Not secure, but it is ideal for testing)

    ![mongo-access](./images/mongodb_access.png)

    ![mongo-account](./images/mongodb-setup-done.png)

* Create a MongoDB database and collection inside mLab
    ![mongo-colllection](./images/Mongodb_collection.png)

* In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. We need to create it.
Create a file in your Todo directory and name it  `.env`.

    `touch .env`

    `vi .env`

* Add the connection string to it to access the database in it, just as below:

        DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
    ![.env](./images/vim%20.env.png)

* Ensure to update `<username>, <password>, <network-address> and <database>` according to your setup

    * On your project dashboard, click `Clusters`
    * Then click connect
    * Now, click connect your application
    ![node-connection](./images/cluster%20node.png)

* Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.

    ![vim index.js](./images/vim%20index.png)


* Simply delete existing content in the file, and update it with the entire code below.
To do that using vim, follow below steps
Open the file with vim index.js

    * Press esc
    * Type `:`
    * Type `%d`
    * Hit `Enter`

The entire content will be deleted, then,
Press i to enter the insert mode in vim
Now, paste the entire code below in the file.

        const express = require('express');
        const bodyParser = require('body-parser');
        const mongoose = require('mongoose');
        const routes = require('./routes/api');
        const path = require('path');
        require('dotenv').config();
        
        const app = express();
        
        const port = process.env.PORT || 5000;
        
        //connect to the database
        mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
        .then(() => console.log(`Database connected successfully`))
        .catch(err => console.log(err));
        
        //since mongoose promise is depreciated, we overide it with node's promise
        mongoose.Promise = global.Promise;
        
        app.use((req, res, next) => {
        res.header("Access-Control-Allow-Origin", "\*");
        res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
        next();
        });
        
        app.use(bodyParser.json());
        
        app.use('/api', routes);
        
        app.use((err, req, res, next) => {
        console.log(err);
        next();
        });
        
        app.listen(port, () => {
        console.log(`Server running on port ${port}`)
        });

* Storing information inside the environment variables is considered more secure and best practice to separate configuration and secret data from the application.

Start your server using the command:

`node index.js`

* You should see a message ‘Database connected successfully’, if so – we have our backend configured. Now we are going to test it.

![Successful](./images/db_conn_succesful.png)

___
### Testing Backend Code with PostmanAPI 
*Postman is an API platform for developers to design, build, test and iterate their APIs. 
We will use Postman to test the backend code withou frontend. You can perform CRUD (Create, Read, Update and Delete) operations on Postmand which is a germane function of our Todo app.

* Click https://www.postman.com/downloads/ to download and install postman on your machine.

* Now open your Postman, create a POST request to the API http://<PublicIP-or-PublicDNS>:5000/api/todos. This request sends a new task to our To-Do list so the application could store it in the database.

    Note: make sure your set header key Content-Type as application/json.

* Open your Postman, create a POST request to the API `http://<PublicIP-or-PublicDNS>:5000/api/todos`. This request sends a new task to our To-Do list so the application could store it in the database.

    ![POST](./images/postman.png)


* Create a GET request to your API on `http://<PublicIP-or-PublicDNS>:5000/api/todos`.
 This request retrieves all existing records from our To-do application (backend requests these records from the database and sends it us back as a response to GET request).

![GET](./images/postman%20test.png)












