# AWS-THREE-TIER-PROJECT
project Three Tier Architecture using AWS services.

Step 1: Download Github project in your loacal machine  using below link 
https://github.com/aws-samples/aws-three-tier-web-architecture-workshop.git

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/a2628005-f729-40e3-b155-dc239272801f)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/3e692c04-9856-4762-bf6d-7d88f974eb33)


# Step 2 :
- Create a IAM Role  AWS-THREE-TIRE-ROLE add permession
- AmazonSSMManagedInstanceCore
- AmazonS3ReadOnlyAccess
-  Create a role 

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/33f37687-01cb-43a7-b676-6751ccc16f17)

# SETP 3:  create the S3 bucket
  
  S3 Bucket Name- aws-Three-Tire

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/3f37d186-e591-49a7-8501-8e0199443574)


# Step 4:  -	Now Networking and security
-	VPC
-	Subnets
-	Route Tables
-	Internet Gateway
-	NAT gateway
-	Security Groups

  # VPC and Subnets :

  Click on the VPC icon on the AWS Management Console, select ‘Your VPCs’, and click on Create VPC to initiate the creation process.
  Next, select the VPC-only option and fill out the Name tag and CIDR range with the following information:
- Name: Three-tire-application
-	CIDR range: 10.0.0.0/16
-	Accept the default tenancy and create the VPC.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/902b09e8-6062-4adc-be63-7163d1f29b50)

- Subnet Creation:  After creating the VPC, click Subnets on the left side of the dashboard and then click on Create Subnet to create the Subnets.

We will need 6 subnets across two availability zones. This means that three subnets will be in one availability zone and the other three in a second availability zone.

Using the previously created VPC, the 6 subnets with the appropriate configuration settings were successfully created as indicated below.

- Private-1a-db/Private-1b-db 
 (This subnet use for Data-base)


![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/e230cab9-0383-4fe2-9a9f-04f5fed23698)

# Internet Connectivity
Internet Gateway

To give our public subnets in our VPC internet access, we need to have an Internet Gateway and attach it to the VPC. On the left-hand side of the VPC dashboard, select Internet Gateway and click on Create internet gateway to create it.

name the Internet Gateway (three-tier-igw)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/8ab413d7-87b4-4da7-b175-ccf82de50f3d)

# NAT Gateway  
In order for our instances in the appLayer private subnet to be able to access the internet they will need to go through a NAT Gateway. For high availability, we’ll deploy one NAT gateway in each of our public subnets.

Let’s navigate to NAT Gateways on the left side of the current dashboard and click Create NAT Gateway.

Fill in the Name, choose one of the public subnets we created in part 2, allocate an Elastic IP, and then click Create NAT gateway.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/0a014eeb-bf02-45bc-97ce-8521852e174a)

Let’s repeat the same procedure for the other public subnet (public-web-subnet-az-1b).


# Routing Configuration :

Now click on route table , after clicking You can see default route table alreday created while you create VPC.
Edit name of route table (Public-ROUTE-TABLE)
Now open Route table At the details page, we’re going to add a route that directs traffic from the VPC to the internet gateway. In other words, for all traffic destined for IPs outside the VPC CDIR range, we need to add an entry that directs it to the internet gateway as a target and save the changes.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c255ee54-a7f2-4183-96b3-ac8962e22fc1)

Now, let’s edit the Explicit Subnet Associations of the route table by navigating to the route table details again. Select Subnet Associations and click Edit subnet associations. Associate two subnet.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/64399c4a-eac0-4906-96c0-532330d6e16c)

- Now Public route tabale work is finished.

- We’re now going to create 2 more route tables, one for each app layer private subnet in each availability zone. These route tables will route app layer traffic destined for outside the VPC to the NAT gateway in the public web subnet respective availability zone, so let’s add the appropriate routes for that. Ready? Let’s go! :)

- Remember we need to create two private route tables for the private app layers 1 and 2 which host the NAT Gateways.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/578a9cd3-bbf1-4e5e-befe-c29bb3b6ee55)

Now Open Private_TABLE -1a and ADD Routes select Nat-getway- AZ-1a and Subnet Associate select Private-1a

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/6c6f2378-1e7a-43a9-91a8-73b1e58db36d)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/ec64e564-65af-49f2-a2b8-27c18177723a)

- Follow same step for Private-table-1b  - open Private_TABLE -1b and ADD Routes select Nat-getway- AZ-1b and Subnet Associate select Private-1b



# Step 5: Security Groups

- Here the first security group we’ll create is for the public, internet-facing load balancer. After typing a name and description, let’s add an inbound rule to allow HTTP type traffic for our IP. See below!

   ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/94f6ae60-a09d-4882-9c63-a2ceddf60e2d)

- The second security group for  web tier. After typing a name and description, we need to add an inbound rule that allows HTTP-type traffic from our internet-facing load balancer security group we created in the previous step. This will allow traffic from our public-facing load balancer to hit our instances. After that, we’ll add an additional rule that will allow HTTP-type traffic for our IP. This will allow us to access our instance when we test.


  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/b12dbb93-0c28-4ebd-93a6-5b55491e4dea)

- Now create a Third Internal-load-balancer-sg and add inbond rule type HTTP AND SOURCE WEB tier, that allows HTTP-type traffic from your web tier security group and add another rule for only HTTP
 
  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/a8b0d5a8-6fdb-48fa-be9d-0d9d0072750b)
 
- The fourth security group we’ll For private-intance-sg add inbond rule type custom TCP and SOURCE Internal-load-balancer-sg, that allows all traffic from your Internal-load-balancer-sg security group and add another rule for only Custom TCP port number 4000 see below :
   
  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/36e84085-d94e-45e0-bceb-d671e975c826)

- The fifth security group we’ll configure protects our private database instances. For this security group, we’ll add an inbound rule that will allow traffic from the private instance security group to the MYSQL/Aurora port (3306).
 
  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c29b160b-9038-4b67-a1f3-26d43cb11c28)


# Step 6:  Database Deployment
  This section of the workshop will walk us through deploying the database layer of the three-tier architecture.

  Objectives:
  Deploy Database Layer
  Subnet Groups
  Multi-AZ Database

We are going to create database Subnet groups for the architecture. Navigate to the RDS dashboard in the AWS console, search for RDS, and click on Subnet groups on the left-hand side.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/f7658176-ff10-4f00-8dc1-3cfa2ae7612d)

Now, we need to add the subnets previously created in each availability zone for the database tier (layer). To ensure consistency, let’s navigate back to the VPC dashboard to verify so that we can select the correct subnet IDs.

Now that we’ve got the subnets IDs verified, let’s select the two subnets to add them and create the Subnet Group as indicated below

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/2052947d-9836-4a78-91c7-d885490ebb0b)

# Database Deployment :
Create database. We’re going to select MySQL/Aurora as our database choice. On the same RDS dashboard, navigate to Databases on the upper left-hand side 
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/689f8422-698e-46cf-8d85-cd11f1edb44d)

Under the Templates section, select the Dev/Test since this isn’t being used for production at the moment. Under Settings Database cluster identifier, keep the default name database-1.
We’ll keep the username as ‘admin, set a password to welcome-db’, and then write down the password on a notepad since we’ll be using password authentication to access our database.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/2c39027f-8460-47de-b686-6261a9093a4b)

Under the Cluster storage configuration section, we’ll keep Aurora Standard and keep the default option under Instance Configuration

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/76de64a9-327f-465c-953c-c9738c4b72a7)

Next, under Availability and Durability, we’ll keep the option to create an Aurora Replica or reader node in a different availability zone as recommended along with our VPC under Connectivity.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/69e2adbd-a3f4-4047-a0b0-373fd74f7109)


We’ll also choose the subnet group we created earlier, and select no for public access.
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/8c6560ae-72a2-45cd-ad47-725c498311ef)

Now, let’s set the security group we created for the database layer.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/f39448cd-475c-45ea-9214-19a4df58f240)

We’ll not make any changes under password authentication because password authentication is always on. Click Create to create the database.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/01498c80-f710-411a-a61b-82f69992acd3)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/394d2fc7-2b40-4266-84fc-1a3297b26da1)


# Step 7 :  App Tier Instance Deployment

In this section, we are going to create an EC2 instance for our app layer and make all the essential software configurations so that the app can run correctly. The app layer will consist of a Node.js application running on port 4000. In addition, we will configure our database with some data and tables.

Objectives:
Create App Tier Instance
Configure Software Stack
Configure Database Schema
Test DB connectivity

- App Instance Deployment
Using the EC2 service dashboard, click on Instances on the left-hand side and then Launch Instances to start the process.  Let’s name our app instance ‘appweb’ for the three-tier architecture and select the Amazon Linux 2 AMI for our application and operating system image.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c17c265b-9e40-41ea-af20-42e704bd97d2)

We’ll be using the free tier eligible T.2 micro instance type so let’s select that to proceed. Even though ‘Proceed without a key is (Not recommended)’, we’ll proceed without a key pair for architecture.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/f8b11cf0-3c2b-4c02-aac6-c7d6e3d28c7f)

Earlier we created a security group for our private app layer instances, so go ahead and select that along with our default VPC and the ‘private-app-subnet-az-1' under Network settings.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/1b1f7905-5a01-4c8a-ae77-41277a47db54)

We’ll keep the default configuration settings for Configure storage, and Advanced details as-is, but review the Summary, and click Launch instance to create the ‘appLayer’ instance.

The ‘appLayer’ instance is created.

Now, let’s navigate back to the Instance dashboard of the EC2, select the ‘appLayer’ instance, click on Actions, and then Modify IAM role under Security to update the IAM role.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/2b29e9f7-0982-40dd-9559-d1ec3e3ba67b)

Select the IAM role we created earlier from the ‘IAM role’ list and click ‘Update IAM role’ to update the role.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c32a6cc8-c436-4c84-a5dc-3409ee3680db)

- Connect to Instance
Let’s navigate to our list of running EC2 Instances by clicking on Instances on the left-hand side of the EC2 dashboard. When the instance state is running, connect to the instance by clicking the checkmark box to the left of the instance and clicking the connect button on the top right corner of the dashboard. Select the Session Manager tab, and click Connect. This will open a new browser tab for you.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/eace494d-2bc8-4627-8eb8-0caba083289e)

When you first connect to your instance like this, you will be logged in as ssm-user which is the default user as indicated below.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/e9c1489a-7228-48c1-b872-3ac1e0c94ff5)

Now, let’s switch to ec2-user using the below command in the same terminal:
sudo -su ec2-user

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/817f7421-68db-4cde-8ac3-e44f2dbf9e85)


Let’s take this moment to make sure that we are able to reach the internet via our NAT gateways. If our network is configured correctly up till this point, we should be able to ping the www.Google.com

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/454d1ae2-2c38-4cc8-93c3-310bfcacc368)

You can stop the ping by pressing ctrl + c.

- Configure Database

  sudo yum install mysql -y

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/d71f060d-84bc-496c-9fa1-ddea43a0be51)


- Now, let’s initiate our DB connection with our Aurora RDS writer endpoint. In the following command, replace the RDS writer endpoint and the username, and then execute it in the browser terminal:

  mysql -h CHANGE-TO-YOUR-RDS-ENDPOINT -u CHANGE-TO-USER-NAME -p

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/ae297f0c-ff73-41cd-aee6-354279bc2260)


  We successfully connected to the database


  Now that we’ve successfully connected to MySQL database, let’s create a database called ‘webappdb’ with the following command using the MySQL CLI:

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/40504295-0290-441b-b49a-9d61c833a5b4)


  Let’s verify that the database was created correctly with the following command:

  SHOW DATABASES;

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/9db71d3a-5e51-42ce-b04d-ed56cdd8538b)

  Now, let’s create a data table by first navigating to the database we just created with the command below:
  USE webappdb;

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/3112c9c1-e8ca-451f-8861-5b0e8405fe01)


Now that we’ve changed to the database (webappdb), let’s create the following transactions table by executing this create table command:

CREATE TABLE IF NOT EXISTS transactions(id INT NOT NULL
AUTO_INCREMENT, amount DECIMAL(10,2), description
VARCHAR(100), PRIMARY KEY(id));

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/d038f465-51ca-4888-b2e2-dcd238970a88)


Let’s verify if the tables were created with the below command:   SHOW TABLES;
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/cf18b8f7-3cd3-4280-b10b-f9e31aca6b0e)

Now that we have the tables created, let’s insert data into the table with the command below for use/testing later:

INSERT INTO transactions (amount,description) VALUES ('400','groceries');

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/e100014f-b223-4457-ab26-d7b8fbbfb632)


Let’s verify that our data was added by executing the following command:

SELECT * FROM transactions;

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/f71d8057-c29e-4252-ae1f-6da784fd37f4)


To exit the MySQL client, just type exit and hit enter.


# Configure App Instance :

- Go to your S3 bucket (aws-three-tire)
- Click on upload
- In download section select AWS-thre-tier-main folder open
- select app-tier and web-tire files
- upload

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/667bc82a-8f24-435a-9521-800244f17414)

- Now install aws cli
- sudo yum install aws-cli
- For check aws cli is working or not use command- aws s3 ls

- Now Need to change DbConfig.js, go in  AWS-thre-tier-main folder open select app-tier open DbConfig.js file in visual studio and change as below screen shot and save 

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/999ced81-9bee-4a46-84f9-e4d624f54455)

- Now again open s3 bucket and upload again DbConfig.js file in app-tier folder in s3. 

- Now With the ‘app-tier’ folder and contents uploaded to the S3 bucket, let’s go back to our SSM session via our EC2 ‘appLayer’ instance to install all of the necessary components to run our backend application. Start by installing NVM (node version manager) using the command below:

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/91dd4f83-6761-4579-be30-33b5e28f1c65)


source ~/.bashrc
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/18a6afdd-e8ee-4afb-bc51-33464e0efce8)

- Next, let’s install a compatible version of Node.js and make sure it’s being used with the below commands:

nvm install 16
nvm use 16

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/93407c2f-0787-406c-af3e-003a4662a134)


- PM2 is a daemon process manager that will keep our node.js app running when we exit the instance or if it is rebooted. Let’s install that as well.
npm install -g pm2

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/12fd2cd0-20e1-4ef5-adfc-c39697513fe8)


- Now, we need to download our code from our S3 buckets onto our instance. With the command below, let’s replace BUCKET_NAME with the name of the bucket we uploaded the app-tier folder to:
  cd ~/
  aws s3 cp s3://BUCKET_NAME/app-tier/ app-tier --recursive

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/2154295a-a9c4-4f57-b793-ee4b498f2247)


  - Next, navigate to the app directory, install dependencies, and start the app with pm2 with the command below:
cd ~/app-tier
npm install
pm2 start index.js

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/fef3f1c1-3533-4523-b86b-07b680803e40)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/15262d52-92b0-4d8b-bc34-a574bd5aa803)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/1feecd54-9b51-4348-8b22-a96ee4044126)


- To make sure the app is running correctly run the following:  pm2 list

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/72caf301-575b-405b-8f73-5b426d24a5df)

- The app is running. If you see an error, then you need to do some troubleshooting. To look at the latest errors, use this command:
pm2 logs
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/0bb1aefe-053e-4fdc-ad0d-60a4dc68c5a0)

You can also see that the AB3 backend app is listening at http://localhost:4000.

- PM2 will make sure our app stays running when we leave the SSM session. However, if the server is interrupted for some reason, we still want the app to start and keep running. This is also important for the AMI we will create so let’s keep PM2 running by executing the command below:

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c39e8782-2b7c-49fb-ba9d-711b89f30fd3)

- The below message appears on the command prompt above after running the ‘pm2 startup’ command.

- sudo env PATH=$PATH:/home/ec2-user/.nvm/versions/node/v16.0.0/bin /home/ec2-user/.nvm/versions/node/v16.0.0/lib/node_modules/pm2/bin/pm2 startup systemd -u ec2-user --hp /home/ec2-user

  DO NOT run the above command, let’s copy and past the command in the output you see in your own terminal. After you run it, save the current list of node processes with the following command:

   pm2 save

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/681a1d0a-6b8c-4c96-b8bf-2474a35bd67e)



  # Test App Tier:
Now let’s run a couple tests to see if our app is configured correctly and can retrieve data from the database.

To check out our health check endpoint, copy this command into your SSM terminal. This is our simple health check endpoint that tells us if the app is simply running.

The command responded with the following message: “This is the health check” which means our health check is running correctly as indicated below.

sudo curl http://localhost:4000/health  

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/381d1e19-1d88-4569-ac94-d833cb2e74d7)

Next, let’s test our database connection. We can do that by hitting the following endpoint locally:

curl http://localhost:4000/transaction

Received the following response after executing the command:

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/fc285e19-8f23-4716-999a-4155e4963ef2)


# Step 8 : Internal Load Balancing and Auto Scaling

In this section of the workshop, we will create an Amazon Machine Image (AMI) of the app tier instance we just created, and use that to set up autoscaling with a load balancer in order to make this tier highly available.

Objectives:
Create an AMI of our App Tier
Create a Launch Template
Configure Autoscaling
Deploy Internal Load Balancer


- App Tier AMI
  Let’s navigate to Instances on the left-hand side of the EC2 dashboard. Select the app tier instance we created and under Actions select Image and Templates. Click Create Image.

 ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/be949b2d-5abd-4f78-9069-bdeca0f1dcd0)


Let’s give the image a name and description and then click Create image. This will take a few minutes, but if you want to monitor the status of image creation you can see it by clicking AMIs under Images on the left-hand navigation panel of the EC2 dashboard.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/cd4eb32e-e66d-4615-8120-bea9d96004dd)


- Target Group
While the AMI is being created, let’s go ahead and create our target group to use with the load balancer. On the EC2 dashboard, navigate to Target Groups under Load Balancing on the left-hand side. Click on Create Target Group.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/fa8c75da-6556-4905-aa93-73e2bdb30b33)


The purpose of forming this target group is to use our load balancer so it may balance traffic across our private app tier instances. Let’s select Instances as the target type and give it a name.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/51dc6e6f-0da8-4acb-8228-53ca85c15c36)


Let’s set the protocol to HTTP and the port to 4000. Remember that this is the port our Node.js app is running on. Select the VPC we’ve been using thus far, and then change the health check path to be /health to indicate the health check endpoint of our app and click Next.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/20c99fe3-c343-4697-bd52-da085b4a001a)



We’ll NOT register any targets for now, so let’s just skip that step, click Next and create the target group.
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/a38b4d31-8a66-4386-9a4b-47a69ff2a3dd)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/04920613-7733-4f3e-9f59-1fd622fd1162)


# Internal Load Balancer

We’re going to create an internal load balancer for our three-tier. On the left-hand side of the EC2 dashboard select Load Balancers under Load Balancing and click Create Load Balancer.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/99f25a10-1d44-46bf-a05a-c8031501aa53)


We’ll be using an Application Load Balancer for our HTTP traffic, give it a name (App-Tier-Internal-lg), select Internal under Scheme, and click the create button for that option.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c31a898d-09de-499f-96e7-c57f46843896)

Let’s select the correct network configuration for our VPC and private subnets.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/eb19778d-7493-47b7-94df-65e847739dff)

Next, select the security group we created for this internal ALB. Now, this ALB will be listening for HTTP traffic on port 80. It will be forwarding the traffic to our target group that we just created. Let’s select it from the dropdown list, and create the load balancer.


![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/38373efd-150c-4f08-a63e-b4f4c80b571c)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/f63ef996-90dd-46f9-a3a2-b7f9217d33d1)

Internal Load Balancer is created with two availability zones.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/8b1ab7e4-dc16-4ea8-bbc3-c4a412ef6002)


# Launch Template

Now, we need to create a Launch template with the AMI we created earlier before we configure Auto Scaling. 
Name the Launch Template, and then under Application and OS Images include the app tier AMI we previously created.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/9458348c-050d-43e0-b6f1-14917f57be83)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/6c759267-72c0-4ea2-a9e2-cef1f8ce30f3)


Under Instance Type select t2.micro. For Key pair and Network Settings don’t include it in the template. We don’t need a key pair to access our instances and we’ll be setting the network information in the autoscaling group.


![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/026eacea-d059-4d78-847b-e0047a875944)

Set the correct security group for our app tier, and then under Advanced details use the same IAM instance profile we have been using for our EC2 instances.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/9aa9c495-3673-4140-afbb-6f75eaf8043b)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/b0d77613-712e-48b4-8fac-559c84a79ff0)


# Auto Scaling :

We will now create the Auto Scaling Group for our app instances. On the left-hand side of the EC2 dashboard, navigate to Auto Scaling Groups under Auto Scaling and click Create Auto Scaling Group.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/98c24618-dc40-4c93-b6a9-3c57a404b49a)


Let’s give our Auto Scaling group a name, and then select the Launch Template we just created and click Next.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/5cf3d704-6923-4d4b-8c2b-81d42dc320bd)

Next, we’re going to select on the Choose instance launch options page, set our VPC, and the private instance subnets for the app tier, and continue.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/12b52f6b-e319-4848-a942-c13495eeba25)


For this next step, we’ll attach this Auto Scaling Group to the Load Balancer we just created by selecting the existing load balancer’s target group from the dropdown. Then, click next.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/54d1852c-fc72-455a-be81-3b1d53a3f4b5)

We’re going to set the desired, minimum, and maximum capacity of our Auto Scaling Group size and Scaling policies. Review and then Create Auto Scaling Group.


![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/9a0a680c-6d6d-4016-a888-568a7742b8c0)

- click on next and click om create Auto scalling Group



![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/aa68bebd-ac79-4fd5-a33e-9592ee84155c)



- Now check Your Auto scaliing Instances create or not
- Two instance has been created
  
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/fb51876a-f9bf-480d-9f42-c9631e852ded)


# Step-9 

- Web Tier Instance Deployment
In this section, we will deploy an EC2 instance for the web tier and make all necessary software configurations for the NGINX web server and React.js website.

- Objectives
- Update NGINX Configuration Files
- Create a Web Tier Instance
- Configure Software Stack

- Now create a New Instance for web-tier
  Back on the EC2 dashboard, select Instances on the left-hand side under Instances, and click on Launch instances.

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/515989a9-6c35-4472-a94f-79593f0ca881)

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/47892b64-eedd-4339-b972-b53be6cf50ea)


  We’ll proceed without a key pair for this instance.

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/628d63bf-c0ee-47ca-a201-a99a04caa680)

  - Click on Launch Instance
    ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/b2903010-764c-4f2c-800c-1f72999055d1)


- Let’s head back to the EC2 dashboard now that we’ve the Web Tier instance creation in process. Select the instance and click Actions | Security | Modify IAM role. Select the (ec2-three-tier-access-role) from the IAM role dropdown list and Update IAM role to update the instance.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/875310b5-16c8-42ba-b536-825bdb0aa5af)


Connect to Instance
Let’s follow the same steps we used to connect to the first app instance and change the user to ec2-user. Test connectivity here via ping as well since this instance should have internet connectivity:

- Note- Make sure add inbound rule ssh port  in instance security group
 ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/406d0cfb-aad8-4674-a2ae-fe86677cc845)

- use comand to go in /bin - cd /bin

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/a7f6fa42-4ea0-4a5d-83cd-03d07c96aa12)


# Configure Web Instance
We now need to install all of the necessary components needed to run our front-end application. Let’s start by installing NVM and node on the instance


curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
source ~/.bashrc
nvm install 16
nvm use 16

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/87b811fd-fb8b-4c4d-821a-ea7f9579280d)


- Now we need to download our web tier code from our s3 bucket:

cd ~/
aws s3 cp s3://BUCKET_NAME/web-tier/ web-tier --recursive

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/d3c71949-1972-4514-a428-4fba252a9f06)


- Navigate to the web-layer folder and create the build folder for the react app so we can serve our code using the below commands:

cd ~/web-tier
npm install (Its takea a time to install)
npm run build

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/112f5553-8edd-4c7c-af3e-23dd3df1f6a7)

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/0883b948-2f6c-4f3d-aefe-7cb223243664)

- NGINX can be used for different use cases like load balancing, content caching etc, but we will be using it as a web server that we will configure to serve our application on port 80, as well as help direct our API calls to the internal load balancer. Let’s run the below command to proceed

sudo amazon-linux-extras install nginx1 -y

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/620d9ee0-b651-4209-a78b-42ab1b6f7a4d)



- We will now have to configure NGINX. Navigate to the Nginx configuration file with the following commands and list the files in the directory:

cd /etc/nginx
ls

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/b3f4b351-bb83-44a5-8844-9c0dcb4f853f)


- Let’s update the ‘nginx.conf’ file with the one we uploaded to S3 bucket. We’ll remove the file and replace the bucket name below:

sudo rm nginx.conf
- Now download nginx.congf in our terminal, file is in s3 bucket.
sudo aws s3 cp s3://BUCKET_NAME/nginx.conf.

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/5b55f12b-8641-499a-840c-f1966da844dd)

- Now we download nginx.conf. from s3 bucket - check using command
- ls

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/c2f78b13-dbb7-4c22-b493-4b510dc85050)

  - Now use ommand sudo vi nginx.cong
  - repalce load balancer DNS in file
    ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/4158b219-69fc-4f8c-9d69-a543c2033168)

- Let’s restart Nginx with the following command:
![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/0cc63c62-c238-40aa-a4eb-b3803c495da3)

- Let’s make sure Nginx has permission to access our files by executing the command:
- And to make ensure the service starts on boot, run this command:

  chmod -R 755 /home/ec2-user
  sudo chkconfig nginx on

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/36aa3745-fcb2-43a1-b70e-a78b68c89f64)



# Step 10- Now let’s copy and plug in the public IP of our web tier instance to see our website. The public IP can be found on the App Tier Instance details page on the EC2 dashboard. Voila, the website is working correctly.


![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/aa1bafee-6906-45fd-a868-04845fd83a93)


![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/d6f9b772-2615-44ff-8bee-2f42c37d0d4e)

Now add some entry 

![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/0798d93f-cf04-4010-ac90-54bfd7f007d8)



- Now check your data is save in databse or not
- Connect to your private instance/ app tier instance to check database

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/1d87731a-bfdf-4c4a-addb-49d28b4ea98e)

  ![image](https://github.com/Patni123/AWS-THRRE-TIRE-PROJECT/assets/46121108/878dc545-964b-488f-a687-86d73259463f)


  
  








  


  

































































































  

















  




  









  









  

  
  









































    


  


      

      

 
    



























   


  




  


  

  

  


  



