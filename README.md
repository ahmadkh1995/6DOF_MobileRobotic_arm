### Project based on a 6-DOF Robotic ARM & Jetson Nano & Kinect360

 **Assemble of Robotic Arm parts**
 
 <p align="center" >
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/1.jpg">
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/2.jpg">
  <img width="180" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/3.jpg">
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/4.jpg">
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/5.jpg">
  <img width="180" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/6.jpg">
  <img width="180" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/7.jpg">
  <img width="180" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/8.jpg">
  <img width="180" height="150" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/9.jpg">
   </p>
   
  <p align="center" >
  <img width="200" height="150"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Assembly/10.jpg">
  <img width="200" height="90" src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Logos/kinect360.jpg">

</p>

### Connect Kinect360 to Linux(Ubuntu 18.04) 

Required Libraries :  **Freenect**   **OpenNI**   **PrimeSensor Modules**  **NITE**


The Kinect360 sensor is equipped with a RGB camera(640x480 pixels at 30 FPS),an infra red projector and an infra red sensor.

**(Installation):**

**I) :**

       $ sudo apt-get install cmake libglut3-dev pkg-config build-essential libxmu-dev libxi-dev libusb-1.0-0-dev python
       
if you get “Unable to locate package libglut3-dev” error, use this command instead:

       $ sudo apt-get install cmake freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev libusb-1.0-0-dev python
       $ sudo apt-get install doxygen mono-complete graphviz
 

**II) :** **Freenect Library:** 

libfreenect is a cross-platform library that provides the necessary interfaces to activate, initialize, and communicate data with the Kinect hardware. Currently, the library supports access to RGB and depth video streams, motors, accelerometer and LED and provide binding in different languages (C++, Python...)

This library is the low level component of the OpenKinect project which is an open community of people interested in making use of the Xbox Kinect hardware with PCs and other devices.[1](http://neuro.debian.net/pkgs/freenect.html)

       $ git https://github.com/OpenKinect/libfreenect 
       $ cd libfreenect
       $ mkdir build
       $ cd build
       $ cmake ..
Check that it marks no errors and no missing applications. Install whatever it marks and then proceed with the making.   
       
       $ make
The make command will connect to the Internet in order to download the audios.bin which is a firmware needed for the MS Kinect to work, without it the device won't be able to function properly.  

       $ sudo make install
       $ cd ../src
       $ python fwfetcher.py
       
This will create audios.bin file.I should copy this into a directory then the system will open it as firmware .

       $ sudo ldconfig /usr/local/lib64/
       
Now I should create a new directory to add firmware to it.

       $ cp -r /usr/local/include/libfreenect /usr/include/libfreenect   
       $ sudo cp audios.bin /usr/local/share/libfreenect/
       
       
 Now plug the Kinect-360(v1) to the computer usb port and check the attached devices :
 
       $ lsusb | grep Xbox
       
  <p align="center" >
    <img width="360" height="37"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/usb_list.png">
  </p>

**Test by an example :**
       
        $ cd ../build/bin
        $ freenect-glview
        
   <p align="center" >
    <img width="300" height="200"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/libfreenect_0.png">
    <img width="350" height="280"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/libfreenect_1.png">
  </p>

**Note:** if you get an error :
 
     libusb couldn’t open USB device /dev/bus/usb/001/006: Permission denied.
Then run these commands:

         $ sudo adduser $USER video
         $ sudo nano /etc/udev/rules.d/51-kinect.rules
         
     # ATTR{product}=="Xbox NUI Motor" SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02b0", MODE="0666"
     # ATTR{product}=="Xbox NUI Audio" SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02ad", MODE="0666"
     # ATTR{product}=="Xbox NUI Camera" SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02ae", MODE="0666"

Then Logout and then Login again .

**III) :** **OpenNI Library:** 

       $ git https://github.com/OpenNI/OpenNI
       $ cd OpenNI
       $ cd Platform/Linux/CreateRedist
       $ chmod +x ./RedistMaker
       $ ./RedistMaker
       $ cd ../Redist/OpenNI-Bin-Dev-Linux-x64-v1.5.7.10/
       $ sudo ./install.sh

**IV) :** **PrimeSensor Modules for OpenNI :**
  
OpenNI is primarly developed by PrimeSence, which is the company behind Kinect's depth sensor's technology.

       $ git https://github.com/avin2/SensorKinect
       $ cd SensorKinect
       $ cd Platform/Linux/CreateRedist
       $ chmod a+x RedistMaker
       $ sudo ./RedistMaker
       $ cd ../Redist/Sensor-Bin-Linux-x64-v5.1.2.1/
       $ sudo sh install.sh

**V) :** **NITE :**(a SDK for joint tracking with the Microsoft Kinect)

       $ git https://github.com/arnaud-ramey/NITE-Bin-Dev-Linux-v1.5.2.23
       $ cd NITE-Bin-Dev-Linux-v1.5.2.23
       $ cd x64
       $ sudo bash install.sh
       
       
**VI) :** **Test the Kinect360 configuration by running an example**
 
Inside the OpenNI library :
 
       $ cd OpenNI/Platform/Linux/Bin/x64-Release/
       $ ./ Sample-NiHandTracker

<p align="center" >
    <img width="300" height="250"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/example_1.png">
</p>
       
       
### Project Development :

I want to develop a **Mobile Robotic ARM** and use PCL Library for SLAM section.

**PCL :** I developed another project in which I include tutorials for installing PCL Library and its dependencies [here](https://github.com/ahmadkh1995/PCL_Probabilistic_Robotic).

**I)** Project Folder For connecting the ROS to Kinect360 is located in Folder: **Kinect_ROS**

For using Kinect in ROS ,these packages are necessary :
- [openni_launch](http://wiki.ros.org/openni_launch) : Launch files to open an OpenNI device and load all nodelets to convert raw depth/RGB/IR streams to depth images
- [openni_camera packages](http://wiki.ros.org/openni_camera) : A ROS driver for OpenNI depth (+ RGB) cameras
- [openni_description](http://wiki.ros.org/openni_description) : Model files of OpenNI device
- [rgbd_launch](http://wiki.ros.org/rgbd_launch) : Launch files to open an RGBD device and load all nodelets to convert raw depth/RGB/IR streams to depth images 

In ROS(Melodic) workspace root directory  :

First terminal tab :

     $ catkin_make
     $ roscore 
Second terminal tab :
     
     $ source devel/setup.bash
     $ roslaunch openni_launch openni.launch depth_registration:=true device_id:=#2
**depth_registration :** indicates that we want to enable OpenNI registration and receive XYZRGB camera data (depth and color)

Third terminal tab :

     $ rosrun rviz rviz
After lunching the Rviz , in **Fixed frame** section choose :  *camera_link* .

and then **Add** new display *PointCloud2* and for **Topic** choose : *camera / depth_registered / points*

<p align="center" >
    <img width="400" height="250"  src="https://github.com/ahmadkh1995/My_Robotic/blob/master/Photos/Rviz_test_1.png">
</p>

4 types of representing  point cloud data in the PCL Library [Ref](http://wiki.ros.org/pcl/Tutorials) : 

- sensor_msgs::PointCloud — ROS message (deprecated)

- sensor_msgs::PointCloud2 — ROS message  (used in this project)

- pcl::PCLPointCloud2 — PCL data structure mostly for compatibility with ROS 

- pcl::PointCloud<T> — standard PCL data structure

Project's [Kinect_PCL](https://github.com/ahmadkh1995/My_Robotic/tree/master/Project_codes/catkin_ws/Kinect_PCL) package :

    $ catkin_make
    $ roscore
    $ source devel/setup.bash      
    $ roslaunch openni_launch openni.launch depth_registration:=true device_id:=#2
    $ rosrun Kinect_PCL Kinect_PCL_file input:=/camera/depth/points
    $ rosrun rviz rviz

Add **PointCloud2** display.||||||||For **Frame :** *camera_depth_frame* .|||||||For **Topic :** *output*

