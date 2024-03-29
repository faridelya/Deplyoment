# Deployement of Flask Web Application On AWS EC2 Instance

Following are the steps for deployement

1. ``` Chose right instance for your Application```
2. ``` Connect through ssh to your instance ```
3. ``` Upload your Application via ssh or upload to drive and download on instance with the help of gdown```
4. Install the following pakages.
```
- sudo apt update
- sudo apt install python3-pip python3-dev nginx
- sudo pip3 install virtualenv    # for enviroment installation
```
5.
 ```
 virtualenv env   # it will create env folder for enviroment inside project folder
 ```
 6. 
 ``` source env/bin/activate   # activate enviroment 
     pip3 install flask gunicorn    # after activation install flask and gunicorn pakages in environment
 ```
 7.   Directory structure are as under
 ``` project
      |__ env         # enviroment folder
      ├── file
      │   ├── abc.docx
      │__ wsgi.p      # the main gate point for incoming  request , use when we deploye with gunicorn
      ├── app.py      # main file
      ├── static
      │   ├── abc.css
      │   └── abc.js
      └── templates
          └── index.html

 ```
 8. First test local server in ec2 is working ```gunicorn --bind 0.0.0.0:5000 wsgi:app```
   - try to access from browser (  Ip:port ) eg: 15.12.35.5:5000
   - if it does not work open port 5000 protocol 
   ```
   - In AWS EC2 ins
   - Click the "Edit inbound rules" button to modify the rules.
   - Click the "Add rule" button.
   - Select "Custom TCP" from the "Type" dropdown.
   - In the "Port Range" field, enter 5000.
   - select Ip Schema  0.0.0.0
   ```
   9. **Creating a systemd service for deployment**
       you can give any name in my case app.service
   ```
   sudo nano /etc/systemd/system/app.service   
   ```
   10.
   ```
[Unit]

#  specifies metadata and dependencies

Description=Gunicorn instance to serve myproject

After=network.target

# tells the init system to only start this after the networking target has been reached
# We will give our regular user account ownership of the process since it owns all of the relevant files

[Service]

# Service specify the user and group under which our process will run.

User=ubuntu

# give group ownership to the www-data group so that Nginx can communicate easily with the Gunicorn processes.

Group=www-data

# We'll then map out the working directory and set the PATH environmental variable so that the init system knows where our the executables for the process are located (within our virtual environment).

WorkingDirectory=/home/ubuntu/project/
Environment="PATH=/home/ubuntu/project/env/bin"

# We'll then specify the commanded to start the service with time out and error log file

ExecStart=/home/ubuntu/project/env/bin/gunicorn --workers 3 --bind unix:/home/ubuntu/project/app.sock -m 007 wsgi:app --timeout 60 --error-logfile=/home/ubuntu/project/gunicorn_error.log

# This will tell systemd what to link this service to if we enable it to start at boot. We want this service to start when the regular multi-user system is up and running:
[Install]
WantedBy=multi-user.target
   ```
or check other sample example.
```
[Unit]
After=network.target

[Service]
User=ubuntu
Group=www-data
WorkingDirectory=/home/ubuntu/V2_chatbot/src/
Environment="PATH=/home/ubuntu/V2_chatbot/env/bin"
ExecStart=/home/ubuntu/V2_chatbot/env/bin/gunicorn -w 2 --bind unix:/home/ubuntu/V2_chatbot/app.sock -m 007 --timeout 60 --error-logfile /home/ubuntu/V2_chatbot/gunicorn_error.log wsgi:app

[Install]
WantedBy=multi-user.target
```
   11. For starting app. service file just execute the following
   ```
   sudo systemctl start app
   sudo systemctl enable app
   ```
### IF you made changes in app.service then use below command for changes
```
sudo systemctl daemon-reload
sudo systemctl restart app.service
```
   12. Check ```app.sock``` file will be created in you project directory if not check for error use this command ``` journalctl -u app.service``` is a command used to view the logs for the app.service unit. 
   
   13.  **Configuring Nginx**
     - here we will create configuration file for nginx server which willhandle incomming request then it will send to gunicron through app.sock file
  
    
   14. ```sudo nano /etc/nginx/sites-available/app```
```
    server {
    listen 80;
    server_name 23.20.159.130;  # here add your own public id of aws ec2 instance better if it is elestic ip

    client_max_body_size 10M;   # max file size are allowed to get in incoming request

    location / {
        include proxy_params;
        proxy_pass http://unix:/home/ubuntu/project/app.sock;
        proxy_connect_timeout 180s;  
        proxy_send_timeout 180s;  
        proxy_read_timeout 600s;

    }
}

```
if you are not socketio in application and want to communicate guncorn and nginx by sock then use this.
```                                                                              
server {
    listen 80;
    server_name 23.22.159.136;

    location / {
        include proxy_params;
        proxy_pass http://unix:/home/ubuntu/V2_chatbot/app.sock;
        proxy_send_timeout 180s;
        proxy_read_timeout 600s;

    }
}

```
15.  create smylink for nginx ```sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled```  some time if you change in nginx configuration after that symlink may not work then use -f key work in this command to work prop=erly.
16.  Restart nginx  
```
sudo systemctl restart nginx
``` 
aslo allow nginx in firewall ```sudo ufw allow 'Nginx Full'```

18.  For checking nginx error log.
   ```
   tail -f /var/log/nginx/error.log
   ```
### IF you made changes in nginx use the below command 
1. 
```
sudo systemctl restart nginx
```
check status of inginx use this.
```
sudo systemctl status nginx
```
