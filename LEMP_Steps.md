# IMPLEMENTING LEMP STACK
Before we start we need to have an environemnt to work with. I will we using my AWS account to create an EC2 instance with an Ubuntu Server.

* First thing I'm going to do when I log in to AWS is look for the **EC2** services. There are various methods to navigate to it, here I'm using the **search bar** <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/ec2search.png)

* Once you navigate to the **EC2** page look for a **Launch instance** button <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/launchInstance.png)

* You will then be prompted to pick an OS Image. I will be using **Ubuntu Server 20.04 LTS (HVM), SSD Volume**. Once done click **Select**

* I will pick the **t2.micro** instace type <br /> 
 ![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/t2micro.png)

* I will leave default settings and click **Review and Launch** <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/reviewLaunch.png)

* As you can see we have a Security Group applied to the instance by default which allows SSH connections <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/sshDefault.png)

* After reviewing you can launch your instancing by clicking <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/launch.png)

* We are prompted to create or use an existing Key Pair. I will be creating a new one. I will use this .pem key to SSH into the instance later on. <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/keyPair.png)

* Once you have downloaded your key launch intance <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/LaunchInstances.png)

* To go to the instances dashboard <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/ViewInstances.png)

* If your instance is up and running you will see something like this <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/Running.png)

* To find information on how to connect click on your **Instance ID**

* In the top-right corner you should see the button **Connect**, click on it <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/Connect.png)

* Look for the **SSH client** tab <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/SSHclient.png)

* Under **Example** you'll find an **ssh** command with eveything you need to connect to the instance from a terminal

**Examaple:**
```bash
ssh -i "daro.io.pem" ubuntu@ec2-3-216-90-84.compute-1.amazonaws.com
```
Make sure when you run the command that your current working directory in the terminal is where your KeyPair/.pem is located because in the above example I'm using a relative path to point to my key

* A successful log-in <br /> 
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/ubunutuLogIn.png)

## Installing the Nginx Web Server
---
* We will install Nginx (a high-performance web server) using Ubuntu's package manager **apt**
    * Update list of packages in package manager
    ```bash
    sudo apt udpate
    ```
    * To install apache2 package
    ```bash
    sudo apt install nginx
    ```
    * Alternatively you can put everything together in one line
    ```bash
    sudo apt update && sudo apt install nginx -y
    ```
    The **-y** puts yes whenever the systems asks during install

    * To make sure apache is running 
    ```bash
    systemctl status nginx
    ```
* To test your page by accessing it locally you can run the following
```bash
curl http://localhost:80

curl http://127.0.0.1:80
```
This will just display HTML code in your terminal <br/>

* To test it externally use a browser with the server's public IP
```bash
http://<Public-IP-Address>:80
```
You can find your Public IP Address running this cmd on the terminal
```bash
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
* If your page is up and running you will see nginx sample page
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/welcomenginx.png) <br/>
The URL in browser shall also work if you do not specify port number since all web browsers use port 80 by default.

## Installing Mysql
---

* We will again use **apt** to install this software
```bash
sudo apt install mysql-server
```
* When prompted, confirm installation by typing **Y**, and then **ENTER**.
* Once installation is done we will run a script that removes some insecure default settings and lock down access to your database system. To start the interactive script run:
```bash
sudo mysql_secure_installation
```
And follow the wizard like questions
* Will ask if you want to configure the VALIDATE PASSWORD PLUGIN. I'll put yes for an additioanl layer of security <br />
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LAMP_SATCK/main/images/validate.png) <br />
Keep answering the questions till the end

* When you’re finished, test if you’re able to log in to the MySQL console by typing:
```bash
sudo mysql
```
You should see
```bash
mysql> 
```
* To exit type **exit**
```bash
mysql> exit
Bye
```

## Installing PHP
---
* Now you can install PHP to process code and generate dynamic content for the web server. Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. You’ll need to install:
    *  **php-fpm** which stands for “PHP fastCGI process manager 
        * Need to configure Nginx to pass PHP requests to this software for processing.
    * **php-mysql** a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

* To install these 2 packages at once, run:
```bash
sudo apt install php-fpm php-mysql -y
```
## Configuring Nginx to Use PHP Processor
---

*  To encapsulate configuration details and host more than one domain on a single server Nginx web server uses server block (the equivalent of virtual hosts in Apache)

* On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for the your_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.
Create the root web directory for your_domain as follows:
sudo mkdir /var/www/projectLEMP
