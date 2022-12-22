- Django Deployement on EC2 with Nginx and Gunicorn and controlling with Supervisor
- https://www.youtube.com/watch?v=vmMyuZSHt3c
> in case of simple Deployement of Flask and gunicorn you must face 502 bad gateway error so follow below trick for solution

 - Anyone who lands here from the Googles and is trying to run Flask on AWS using the default Ubuntu image after installing nginx and still can't figure out what the problem is:

Nginx runs as user "www-data" by default, but the most common Flask WSGI tutorial from Digital Ocean has you use the logged in user for the systemd service file. Change the user that nginx is running as from "www-data" (which is the default) to "ubuntu" in /etc/nginx/nginx.conf if your Flask/wsgi user is "ubuntu" and everything will start working. You can do this with one line in a script:

**sudo sed -i 's/user www-data;/user ubuntu;/' /etc/nginx/nginx.conf**
# Install Nvidia Driver
 - Before installation find GPU Model by runing below command
```
sudo lshw -C display
```
 - **output**
 ```
 *-display
       description: 3D controller
       product: GK208M [GeForce GT 740M]   # this is my card Name and GK208M is GPUs Model
       description: 3D controller
       product: GK208M [GeForce GT 740M]
       vendor: NVIDIA Corporation
       physical id: 0
       bus info: pci@0000:0a:00.0
       version: a1
       width: 64 bits
       clock: 33MHz
       capabilities: pm msi pciexpress bus_master cap_list rom
       configuration: driver=nouveau latency=0
       resources: irq:49 memory:b3000000-b3ffffff memory:a0000000-afffffff memory:b0000000-b1ffffff ioport:3000(size=128)

 ```
 
 - [Installation Guide Official Link](https://docs.nvidia.com/datacenter/tesla/tesla-installation-notes/index.html)
 -  Read Carefully and scroll down to Ubuntu section and follow th instruction for installation
# Cuda installation with any specific version 
- Before instaltion of Cuda check your GPU compute compitibility in below link 
- [**Check for GPU compatibility with Cuda**](https://developer.nvidia.com/cuda-gpus)

- You can also make sure your GPU Architecture and GPU model and GPU name in below link
- [check for GPU ](https://en.wikipedia.org/wiki/CUDA)
-  I found my GPU like this.
![alt text](https://github.com/faridelya/Deplyoment/blob/6daa7b9b81bdb87bb6d5c65ed316f4ee92551027/Screenshot%20from%202022-12-22%2011-11-05.png)
![alt text](https://github.com/faridelya/Deplyoment/blob/6daa7b9b81bdb87bb6d5c65ed316f4ee92551027/Screenshot%20from%202022-12-22%2011-10-51.png)

- So above i find out that my GPU has 3.5 compute compatibility 
- now i will check CUDA version computability with GPU you ccan check in above link  
- **Output**
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
