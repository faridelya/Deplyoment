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
   
