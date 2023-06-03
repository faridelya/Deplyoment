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
5. ``` virtualenv env   # it will create env folder for enviroment inside project folder
   ```
