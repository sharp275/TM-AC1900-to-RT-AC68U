<h1 align="center">
Whizlabs Challenge League
</h1>

<h2 align="center">
Challenge Lab One
</h2>

---

Whizlabs is running a cloud challenge between May and July 2022.  The challenge is to complete various tasks in either AWS, GCP, or Azure to test cloud skills.  Following is my solution to challenge lab one.

---
<h3>From Whizlabs</h3>

[Cloud Challenge Details](inst.png)

>Your company is considering migrating a web application from an on-premise server to Amazon Elastic Compute Cloud (EC2). You have been tasked with setting up example infrastructure for hosting the web application and testing the path based routing with the help of Elastic Load Balancing (ELB).
In this lab challenge, you will use the AWS Management Console to complete the tasks that result in provisioning infrastructure to fulfill your company's requirements.
Follow the instructions given below to work on the challenge.
>1.	Create TWO Amazon EC2 Instances.
>       - Admin EC2 Instance
>           - Select Amazon Linux 2 AMI and t2.micro instance type with basic configuration.
>           - SSH into the EC2 Instance and install apache server.
>           - Host the admin HTML page mentioned below in EC2 Instance.
>               - https://challengetask10049.s3.amazonaws.com/admin/index.html
>       - User EC2 Instance
>           - Select Amazon Linux 2 AMI and t2.micro instance type with basic configuration..
>           - SSH into the EC2 Instance and install apache server.
>           - Host the user HTML page mentioned below in EC2 Instance.
>               - https://challengetask10049.s3.amazonaws.com/user/index.html
>2.	Create an Elastic Load Balancer.
>       - If you enter the dns name with /admin, the request should be forwarded to EC2 Instance containing the admin HTML page.
>       - If you enter the dns name with /user, the request should be forwarded to EC2 Instance containing the user HTML page. 
>       - Any other path should be redirected to a success static plain text page with a note "Whizlabs ELB Challenge".


---

<h3>1. Create TWO Amazon EC2 Instances.</h3>


<h4>Create the Admin EC2 instance</h4> 

Login into AWS and search/choose *EC2*.

<p align="center">
  <img width="1000" src="AWS EC2 choose.jpeg">
</p>

Click *Launch instance* then *Launch instance*.
<p align="center">
  <img width="1000" src="Launch EC2.jpeg">
</p>

Enter a name, I used Admin.  The default **Application and OS images**, and the default **Instance type** should be correct.  Click *Create new key pair*.

<p align="center">
  <img width="1000" src="Admin EC2 One.jpg">
</p>

Create a new key pair approiate for the method to be used to connect by SSH to the instance.  The settings I used are for Putty.

<p align="center">
  <img src="Key Pair.jpg">
</p>

Back in the **Launch an instance** page under **Network settings** ensure **Auto-assign public IP** is set to *enable* and *Allow SSH traffic from Anywhere* is checked.  Also click the checkbox *Allow HTTP traffic from the internet*.
<p align="center">
  <img src="Network Settings.jpg">
</p>

Expand **Advanced details** and in **User data** enter the following:

    #!/bin/bash
    sudo su
    yum update -y
    yum install httpd -y
    mkdir /var/www/html/admin
    systemctl start httpd
    systemctl enable httpd

This updates the instance, installs and enables Apache on the instance, and makes a directory to host the webpage.

AWS gives instructions on how to connect to the instance using SSH.  
<p align="center">
  <img src="SSH Connect.jpg">
</p>

When connected to the Admin instance, enter

    sudo su
    nano /var/www/html/admin/index.html

Into index.html, paste the source code from https://challengetask10049.s3.amazonaws.com/admin/index.html. Press Crtl + X to save.

When you navigate to http://*your-instance-ipv4-address*/admin, you will see the following:
<p align="center">
  <img src="admin login page.jpg">
</p>

<h4>Create the User instance</h4>

For the user instance repeat the creation of the admin instance and substitute user for admin and the source code from https://challengetask10049.s3.amazonaws.com/user/index.html instead of the source used to create the admin webpage.

---

<h3>2. Create an Elastic Load Balancer.</h3>

Create a new security group to allow both inbound and outbound HTTP traffic.

<p align="center">
  <img width="1000" src="security group.jpg">
</p>

Create a target for the load balancer.

Select *Instances* under **Choose a target type**.  
Name the target group.  I used *user* for the target group for the user instance.

**Protocol** is *HTTP* and the **Port** is *80*.
Under **Health Checks**, the protocol is *HTTP* and the path is <code>/user</code>.

Click *Next*.

<p align="center">
  <img width="1000" src="target group 1.jpg">
</p>


On the next page, check the box for the user instance and click *Include as pending below*.

<p align="center">
  <img width="1000" src="target group 2.jpg">
</p>

The instance will appear in Review targets.  Click Create target group.

<p align="center">
  <img width="1000" src="target group 3.jpg">
</p>

Under **EC2** > >**Load Balancing** > **Load balancers**, click *Create Load Balancer*.

<p align="center">
  <img width="1000" src="create alb.jpg">
</p>

Select *Application Load Balancer*.

<p align="center">
  <img width="1000" src="Select App.jpg">
</p> 

Enter a name for the load balancer.

Ensure it is *Internet-facing* and *IPv4*.

Under network mapping > mappings, check all the availability zones.

Choose the security group created earlier.
<p align="center">
  <img width="1000" src="app load 1.jpg">
</p>

Under Listeners and routing, choose one of the earlier created target groups. Click *Create load balancer*.

<p align="center">
  <img width="1000" src="app load 2.jpg">
</p>

Back on **Load Balancers**, the address of the load balancer is listed by **DNS name**.

Click on **Listeners**.

<p align="center">
  <img width="1000" src="app load 3.jpg">
</p>

update the listener, choose View/edit rules.
Click the *Pencil* to edit the default rule.  Change it to **Return fixed response with...**, **Response code** *200*, **Content-Type** *text/plain*, and **Response body** *Whizlabs ELB Challenge*.
Click **Update**.

<p align="center">
  <img width="1000" src="app load 4.jpg">
</p>

<p align="center">
  <img width="1000" src="app load 5.jpg">
</p>

Click Plus <img src="plus.jpg"> to add a rule.

Add rules for admin and user.  Under **IF (all match)**, Choose Path and enter either <code>/admin*</code> or <code>/user*</code>.  Choose to forward to the correct target group

<p align="center">
  <img width="1000" src="app load 6.jpg">
</p>

Now requests to the load balancer that have either /admin or /user in them will go to the correct instance and all other will get a response of "Whizlabs ELB Challenge".

<p align="center">
  <img width="1000" src="Validation.jpg">
</p>
