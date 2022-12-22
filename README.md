- Django Deployement on EC2 with Nginx and Gunicorn and controlling with Supervisor
- https://www.youtube.com/watch?v=vmMyuZSHt3c
> in case of simple Deployement of Flask and gunicorn you must face 502 bad gateway error so follow below trick for solution

 - Anyone who lands here from the Googles and is trying to run Flask on AWS using the default Ubuntu image after installing nginx and still can't figure out what the problem is:

Nginx runs as user "www-data" by default, but the most common Flask WSGI tutorial from Digital Ocean has you use the logged in user for the systemd service file. Change the user that nginx is running as from "www-data" (which is the default) to "ubuntu" in /etc/nginx/nginx.conf if your Flask/wsgi user is "ubuntu" and everything will start working. You can do this with one line in a script:

**sudo sed -i 's/user www-data;/user ubuntu;/' /etc/nginx/nginx.conf**
# Install Nvidia Driver ( [optional because Every CUDA toolkit also ships with an NVIDIA display driver package for convenience](https://docs.nvidia.com/deploy/cuda-compatibility/))

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
 -  check nvidia driver version 
 -  ```
    nvidia-smi --query-gpu=driver_version --format=csv
    ```
  - **Display all OEM enablement packages which apply to this system**
  - ```
     sudo ubuntu-drivers list-oem 
    ```
   -  **View all hardware NVidia devices which need drivers and which packages**
   - ```
      sudo ubuntu-drivers devices
     ```
   - **output**
   - ```
     == /sys/devices/pci0000:00/0000:00:1c.4/0000:0a:00.0 ==
     modalias : pci:v000010DEd00001292sv0000103Csd000021DAbc03sc02i00
     vendor   : NVIDIA Corporation
     model    : GK208M [GeForce GT 740M]
     manual_install: True
     driver   : nvidia-driver-455 - third-party non-free
     driver   : nvidia-340 - distro non-free
     driver   : nvidia-driver-450-server - distro non-free
     driver   : nvidia-driver-460 - third-party non-free recommended
     driver   : nvidia-driver-418-server - distro non-free
     driver   : nvidia-driver-470-server - distro non-free
     driver   : nvidia-driver-390 - distro non-free
     driver   : xserver-xorg-video-nouveau - distro free builtin

     ```
    - **Let us install recommended driver automatically 460**
    - ```
     sudo ubuntu-drivers install
      ```
   
# Cuda installation with any specific version 
- Before installtion of Cuda check your GPU compute compitibility in below link 
- [**Check for GPU compatibility with Cuda**](https://developer.nvidia.com/cuda-gpus)
- **outpu**
![alt text1](https://github.com/faridelya/Deplyoment/blob/42ce6929afadaabd87dff99c82a1c476d7f04bfa/Screenshot%20from%202022-12-22%2011-22-20.png)
![alt text2](https://github.com/faridelya/Deplyoment/blob/42ce6929afadaabd87dff99c82a1c476d7f04bfa/Screenshot%20from%202022-12-22%2011-22-41.png)
- The above picture show that My GPU has 3.0 compute compatability but we will deep dive and will see in below link.

- You can also make sure your GPU Architecture and GPU model and GPU name in below link
- [check for GPU ](https://en.wikipedia.org/wiki/CUDA)
- I found my GPU like this.
![alt textt](https://github.com/faridelya/Deplyoment/blob/6daa7b9b81bdb87bb6d5c65ed316f4ee92551027/Screenshot%20from%202022-12-22%2011-11-05.png)
![alt text](https://github.com/faridelya/Deplyoment/blob/6daa7b9b81bdb87bb6d5c65ed316f4ee92551027/Screenshot%20from%202022-12-22%2011-10-51.png)

- So above i find out that my GPU has **3.5** compute compatibility and **kelper** is architecture and GPU model is **GK208M** 
- Now i will check CUDA version computability with GPU you ccan check in above link  
- **Output**
![alt text4](https://github.com/faridelya/Deplyoment/blob/54cb724fa67a19268262413b3c64e207436a6841/Screenshot%20from%202022-12-22%2011-26-47.png)
- Now we can install different compatible version of cuda  on MY GPU so will Cuda installation in below link
- Before Further do we will also check Nvidia Display driver comaptibility with cuda version
- [vistit her](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html) and check tabel 2 and tabel 3 after that if you fullfill the requirement then you may proceed.
- Old version visit to below link
 >  [cuda toolkit archives](https://developer.nvidia.com/cuda-toolkit-archive)
 - reboot after installation
 - Set up the development environment by modifying the PATH and LD_LIBRARY_PATH variables:[you can check here ](https://docs.nvidia.com/cuda/cuda-quick-start-guide/index.html#linux)
 
 # Removing Nvidia driver and Cuda Toolkit
 - [Official link ](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#removing-cuda-toolkit-and-driver)
 - step1: To remove Cuda toolkit  
  ```
  sudo apt-get --purge remove "*cuda*" "*cublas*" "*cufft*" "*cufile*" "*curand*" \
 "*cusolver*" "*cusparse*" "*gds-tools*" "*npp*" "*nvjpeg*" "nsight*" "*nvvm*"
 ```
 - step2: To remove Nvidia Drivers 
  ```
  sudo apt-get --purge remove "*nvidia*" "libxnvctrl*"
  ```
 - step3: To clean up the uninstall  
  ```
  sudo apt-get autoremove
  ```
