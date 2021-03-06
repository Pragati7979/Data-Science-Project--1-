# Data-Science-Project--1-
Real Estate Price Prediction model.

![](Look%20of%20the%20website.png)

## Deploy this app to cloud (AWS EC2)

Create EC2 instance using amazon console using following steps :

1)Search EC2 and the launch instance, select ubuntu as your server machine.

2)Go to configure security group tab,add rule http and https.

3)Review and launch.

4)Create a new key pair.

5)Download that key and store it into the folder c:\users\praga\.ssh

6)Click on launch instances

7)Connect to that instance.

Now connect to your instance using a command like this,

ssh -i "C:\Users\praga\.ssh\bhp.pem" ubuntu@ec2-13-59-157-119.us-east-2.compute.amazonaws.com

Install nginx on EC2 instance using these commands,

sudo apt-get update

sudo apt-get install nginx

Above will install nginx as well as run it. Check status of nginx using

sudo service nginx status

Here are the commands to start/stop/restart nginx

sudo service nginx start

sudo service nginx stop

sudo service nginx restart

Now when you load cloud url in browser you will see a message saying "welcome to nginx" This means your nginx is setup and running.

Now you need to copy all your code to EC2 instance. You can do this either using git or copy files using winscp. We will use winscp. You can download winscp from here: https://winscp.net/eng/download.php

Once you connect to EC2 instance from winscp (instruction given below), you can now copy all code files into /home/ubuntu/ folder. The full path of your root folder is now: /home/ubuntu/Banglore-Home-Prices

1)Paste the hostname ec2-13-59-157-119.us-east-2.compute.amazonaws.com

2)Fill username as ubuntu , password : click on advance ->autentication ->browse private key file ->locate bhp.pem ->click various ok's and then login

3)Copy your project folder to ubuntu


After copying code on EC2 server now we can point nginx to load our property website by default. For below steps,

Create this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,

server {

    listen 80;
    
        server_name bhp;
        
        root /home/ubuntu/Banglore-Home-Prices/client;
        
        index index.html;
        
        location /api/ {
        
             rewrite ^/api(.*) $1 break;
             
             proxy_pass http://127.0.0.1:5000;
            
        }
        
}

Create symlink for this file in /etc/nginx/sites-enabled by running this command,

sudo ln -v -s /etc/nginx/sites-available/bhp.conf

Remove symlink for default file in /etc/nginx/sites-enabled directory,

sudo unlink default

Restart nginx,

sudo service nginx restart

Now install python packages and start flask server

sudo apt-get install python3-pip

sudo pip3 install -r /home/ubuntu/BangloreHomePrices/server/requirements.txt

python3 /home/ubuntu/BangloreHomePrices/client/server.py

Running last command above will prompt that server is running on port 5000. 8. Now just load your cloud url in browser (for me it was http://ec2-13-59-157-119.us-east-2.compute.amazonaws.com/) and this will be fully functional website running in production cloud environment
