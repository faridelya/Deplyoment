- Django Deployement on EC2 with Nginx and Gunicorn and controlling with Supervisor
- https://www.youtube.com/watch?v=vmMyuZSHt3c
> in case of simple Deployement of Flask and gunicorn you must face 502 bad gateway error so follow below trick for solution

 - Anyone who lands here from the Googles and is trying to run Flask on AWS using the default Ubuntu image after installing nginx and still can't figure out what the problem is:

Nginx runs as user "www-data" by default, but the most common Flask WSGI tutorial from Digital Ocean has you use the logged in user for the systemd service file. Change the user that nginx is running as from "www-data" (which is the default) to "ubuntu" in /etc/nginx/nginx.conf if your Flask/wsgi user is "ubuntu" and everything will start working. You can do this with one line in a script:

**sudo sed -i 's/user www-data;/user ubuntu;/' /etc/nginx/nginx.conf**
# Install Nvidia Driver
 - [Official Link](https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html)
 -  Read Carefully and scroll down to Ubuntu section and follow th instruction for installation
# Cuda installation with any specific version 

  - Old version visit to below link
 >  [cuda toolkit archives](https://developer.nvidia.com/cuda-toolkit-archive)
 - reboot after installation
 - Set up the development environment by modifying the PATH and LD_LIBRARY_PATH variables:[you can check here ](https://docs.nvidia.com/cuda/cuda-quick-start-guide/index.html#linux)
 
 # Removing Nvidia driver and Cuda Toolkit
 - [Official link ](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#removing-cuda-toolkit-and-driver)
 - step1: to remove Cuda toolkit  
  ```
  sudo apt-get --purge remove "*cuda*" "*cublas*" "*cufft*" "*cufile*" "*curand*" \
 "*cusolver*" "*cusparse*" "*gds-tools*" "*npp*" "*nvjpeg*" "nsight*" "*nvvm*"
 ```
 - step2: To remove Nvidia Drivers 
  ```
  sudo apt-get --purge remove "*nvidia*" "libxnvctrl*"
  ```
 - To clean up the uninstall  
  ```
  sudo apt-get autoremove
  ```
