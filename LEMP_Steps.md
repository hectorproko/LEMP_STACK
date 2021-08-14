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
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/LEMP_STACK/main/images/ubunutuLogIn.png)