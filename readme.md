The course from : https://developer.nvidia.com/embedded/learn/jetson-ai-certification-programs

# outline

The course consists of three main sections. Use the navigation and breadcrumb links at the top of each section to step through the lessons.

1. [Setting up your Jetson Nano](https://learn.next.courses.nvidia.com/courses/course-v1:DLI+S-RX-02+V2/jump_to_id/e5123a147f004743a9bf26bd700590fc)

   Step-by-step guide to set up your hardware and software for the course projects

   - ###### Introduction and Setup

     Video walk-through and instructions for setting up JetPack and what items you need  to get started

   - ###### Cameras

     Details on how to connect your camera to the Jetson Nano Developer Kit

   - ###### Headless Device Mode

     Video walk-through and instructions for running the Docker container for the course using headless device mode (remotely from your computer).

   - ###### Hello Camera

     How to test your camera with an interactive Jupyter notebook on the Jetson Nano Developer Kit

   - ###### JupyterLab

     A brief introduction to the JupyterLab interface and notebooks

2. [Image Classification](https://learn.next.courses.nvidia.com/courses/course-v1:DLI+S-RX-02+V2/jump_to_id/aabe204272214ba69309581d388b0734)

   Background information and instructions to create projects that classify images using Deep Learning

   - ###### AI and Deep Learning

     A brief overview of Deep Learning and how it relates to Artificial Intelligence(AI)

   - ###### Convolutional Neural Networks (CNNs)

     An introduction to the dominant class of artificial neural networks for computer vision tasks

   - ###### ResNet-18

     Specifics on the ResNet-18 network architecture used in the class projects

   - ###### Thumbs Project

     Video walk-through and instructions to work with the interactive image classification notebook to create your first project

   - ###### Emotions Project

     Build a new project with the same classification notebook to detect emotions from facial expressions

3. [Image Regression](https://learn.next.courses.nvidia.com/courses/course-v1:DLI+S-RX-02+V2/jump_to_id/76a2873eb69946b4928c4f8432e04314)

   Instructions to create projects that can localize and track image features in a live camera image

   - ###### Classification vs. Regression

     With a few changes, your the Classification model can be converted to a Regression model

   - ###### Face XY Project

     Video walk-through and instructions to build a project that finds the coordinates of facial features



# Setting up your Jetson Nano

Step-by-step guide to set up your hardware and software for the course projects

## Introduction and Setup

 硬件准备：

1. [Jetson Nano 4GB](https://developer.nvidia.com/embedded/jetson-nano-developer-kit)
2. 64GB MicroSD卡
3. 需要联网
4. usb Webcam
5. oak-d-lite



setting up your jetson nano with jetpack with mircosd 

> https://developer.nvidia.com/jetpack-sdk-461

此时使用的是jetpack 4.6.1，[jetpack 的所有版本](https://developer.nvidia.com/embedded/jetpack-archive),按照自己的硬件设备和项目需求来选版本

查看自己的jetpack的版本号：[查看jetpack版本](https://www.csdn.net/tags/MtTacg3sMjY4MzEtYmxvZwO0O0OO0O0O.html)

安装完成，需要配置下虚拟空间，添加4GB

Additional Notes about Setup

- The course will run JupyterLab in "headless device mode", which does not require a monitor, keyboard, or mouse. However, you may wish to use these items for the initial Jetson Nano Developer Kit setup procedure,  as described in the instructions for the graphical "display" setup. The "headless" setup instructions are also provided in the devkit setup instructions.  There is also a  [video from JetsonHacks](https://www.jetsonhacks.com/2019/08/21/jetson-nano-headless-setup/) about headless setup.            

- The course runs best if there is a "swap" file of size  4GB, so that if the Jetson Nano is a little short of RAM it can extend a bit by swapping with some of the (slower) disk  space.  After setting up the microSD card and booting the system,                check your memory and swap values with this command,  which shows the values in megabytes.  You should see 4071 as  the size of the swap if you have 4GB configured:                

  ```
  free -m
  ```

  If you don't have the right amount of swap, or want to  change the value, use the following procedure to do so (from a  terminal):                

  ```shell
  # Disable ZRAM:
  sudo systemctl disable nvzramconfig
  
  # Create 4GB swap file
  sudo fallocate -l 4G /mnt/4GB.swap
  sudo chmod 600 /mnt/4GB.swap
  sudo mkswap /mnt/4GB.swap
  
  # Append the following line to /etc/fstab
  sudo su
  echo "/mnt/4GB.swap swap swap defaults 0 0" >> /etc/fstab
  exit
  
  # REBOOT!
  ```



## Cameras

官方是推荐使用 Logitech C270 USB Webcam (recommended configuration)，都是免驱动

还有Raspberry Pi v2 Camera， USB webcam，这个要自己装驱动。

这里是OAK - D - Lite的摄像头。

这里先调通usb摄像头，参考：在jetson nano的平台调用OAK-D-LITE摄像头，[记录在readme.md-调用摄像头](D:\stephen\workspace\project\mlProject\ComputeVision\OAK)

先在nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1的镜像搭建usb摄像头







## Headless Device Mode

https://developer.nvidia.com/embedded/learn/tutorials/jetson-container

https://catalog.ngc.nvidia.com/orgs/nvidia/containers/l4t-ml

https://catalog.ngc.nvidia.com/orgs/nvidia/teams/dli/containers/dli-nano-ai

### Setup Steps

- 1. Connect your flashed JetPack microSD card and power supply (DC barrel jack for 4G kit, USBC  connector for 2G kit).  The Jetson Nano Developer Kit will power on and boot automatically.                

  2. A green LED next to the Micro-USB connector will light as soon as the developer kit powers on. Wait about 30 seconds. Then connect the USB cable from the Micro USB port on the Jetson Nano Developer Kit to the USB port                    on your computer.                

  3. Connect your USB camera to a USB port on the Jetson Nano. *(If using the alternate CSI camera,connect it to the CSI port.)*                

  4. If you are downloading the Docker container with the course notebooks in it for the first time, connect your Nano to the Internet using the Ethernet port or a compatible WiFi device.                

  5.   On your computer, open a terminal window if using Mac or Linux, and a PowerShell window if using Windows. In the terminal, log on to the Jetson Nano with the following command, where`<username>` is  the values you set up on your Nano during the operating system configuration:

     ```
     ssh <username>@192.168.55.1
     ```

       Enter the password you configured when asked.                

  6. Add a data directory for the course with the following command in the Jetson Nano terminal you've logged into:

     ```
     mkdir -p ~/nvdli/nvdli-data
     ```

  7. Run the Docker container with the following command, where `<tag>` is a combination of the course version and Jetson Nano JetPack L4T operating system version  (form is `<tag>` = `<course_version>-<L4T_version>`). A                    list of tags can be found in the [ course NVIDIA NGC cloud page](https://ngc.nvidia.com/catalog/containers/nvidia:dli:dli-nano-ai).          

     ```shell
     
     # create a reusable script
     echo "sudo docker run \
             --runtime nvidia\
             --name dli\
             -it --rm --network host\
             --memory=2G\
             --volume ~/nvdli/nvdli-data:/nvdli-nano/data\
             --volume /tmp/argus_socket:/tmp/argus_socket\
             --volume ~/nvdli/pip.conf:/root/.pip/pip.conf\
             --device /dev/video0 nvcr.io/nvidia/dli/dli-nano-ai:v2.0.1-r32.6.1" > docker_dli_run.sh
     
     ## jetpack 4.6
     # create a reusable script
     echo "sudo docker run \
             --runtime nvidia\
             --name dli\
             -it --rm --network host\
             --volume ~/nvdli/nvdli-data:/nvdli-nano/data\
             --device /dev/video0 nvcr.io/nvidia/dli/dli-nano-ai:v2.0.1-r32.6.1" > docker_dli_run.sh
     
     # jetpack 4.6.1
     # create a reusable script
     echo "sudo docker run \
             --runtime nvidia\
             --name dli\
             -it --rm --network host\
             --volume ~/nvdli/nvdli-data:/nvdli-nano/data\
             --device /dev/video0 nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1" > docker_dli_run.sh
     
     # make the script executable
     chmod +x docker_dli_run.sh
     
     # run the script
     ./docker_dli_run.sh
     ```
     
     

## JupyterLab

docker 内置





# [Image Classification](https://learn.next.courses.nvidia.com/courses/course-v1:DLI+S-RX-02+V2/jump_to_id/aabe204272214ba69309581d388b0734)

## Image Classification Project

点击classification_interactive.ipynb,运行

```python
import torchvision.transforms as transforms
....
```

jupyterlab无限重启无报错日志，进入python3环境运行

```python
import torchvision.transforms as transforms
Segmentation fault (core dumped)
# 参考：Segfault in pytorch 1.6 importing common libraries
# https://github.com/pytorch/pytorch/issues/44229
pip3 install --upgrade pip
# pip3 install torch --upgrade
pip3 install torchvision --upgrade
pip3 install transformers --upgrade
# done
```

task

data collection

define model

live execution

Training and Evaluation

**More Classification Projects**

To build another project, follow the pattern you did with the Emotions Project.  As an example, the Fingers project is provided as an example, but don't let that limit you!  To start a new project, save your previous work,  modify the `TASK` and `CATEGORIES` values, shutdown and restart the notebook, and run all the cells.  Then collect, train, and test!        

