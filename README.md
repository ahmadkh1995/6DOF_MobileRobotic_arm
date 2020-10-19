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

    $ git clone https://github.com/OpenKinect/libfreenect 
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
 Add these lines :        
- Rules for Kinect-for-Xbox model 1414:

      SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="02ad", TAG+="uaccess"
      SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="02ae", TAG+="uaccess"
      SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="02b0", TAG+="uaccess"
    
- Rules for Kinect-for-Xbox model 1473 and Kinect-for-Windows model 1517:

      SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="02be", TAG+="uaccess"
      SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="02bf", TAG+="uaccess"
      SUBSYSTEMS=="usb", ATTRS{idVendor}=="045e", ATTRS{idProduct}=="02c2", TAG+="uaccess"

now after saving it , try theses commands before running ’freenect-glview’;

    $ freenect-micview
    $ freenect-camtest
    $ freenect-glview
Then Logout and then Login again .

**III) :** **OpenNI Library:** 

    $ git clone https://github.com/OpenNI/OpenNI
    $ cd OpenNI
    $ cd Platform/Linux/CreateRedist
    $ chmod +x ./RedistMaker
    $ ./RedistMaker
    $ cd ../Redist/OpenNI-Bin-Dev-Linux-x64-v1.5.7.10/
    $ sudo ./install.sh
Note: If ./RedistMaker failed,most probably you should configure gcc and g++ to versions:4.8,6,7 to fix the problem :

    $ sudo update-alternatives --remove-all gcc 
    $ sudo update-alternatives --remove-all g++

    $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 10
    $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 20
    $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 30

    $ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 10
    $ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-6 20
    $ sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 30

    $ sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 40
    $ sudo update-alternatives --set cc /usr/bin/gcc

    $ sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 40
    $ sudo update-alternatives --set c++ /usr/bin/g++



**IV) :** **PrimeSensor Modules for OpenNI :**
  
OpenNI is primarly developed by PrimeSence, which is the company behind Kinect's depth sensor's technology.

    $ git clone https://github.com/avin2/SensorKinect
    $ cd SensorKinect
    $ cd Platform/Linux/CreateRedist
    $ chmod a+x RedistMaker
    $ sudo ./RedistMaker
    $ cd ../Redist/Sensor-Bin-Linux-x64-v5.1.2.1/
    $ sudo sh install.sh

Note: It is possible that if you have changed versions of gcc ,builld of ./RedistMaker fail ,so you should return to default version of gcc (or remove all of them and install new one)

**V) :** **NITE :**(a SDK for joint tracking with the Microsoft Kinect)

    $ git clone https://github.com/arnaud-ramey/NITE-Bin-Dev-Linux-v1.5.2.23
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
       
### Configure kinect for ros depending on your ros version(kinetic,melodic etc.) :     
    
    $ sudo apt-get install ros-<rosdistro>-openni-camera
    $ sudo apt-get install ros-<rosdistro>-openni-launch
 to test in ros(Also copy packages of openni-camera and openni-launch to src folder of your catkin workspace) :
    
    $ catkin_make
    $ roscore
    $ roslaunch openni_launch openni.launch    
    $ rosrun rviz rviz    
    
Set the Fixed Frame (top left of rviz window) to /camera_depth_optical_frame.
Add a PointCloud2 display, and set the topic to /camera/depth/points. Turning the background to light gray can help with viewing. This is the unregistered point cloud in the frame of the depth (IR) camera. It is not matched with the RGB camera images.[src](http://wiki.ros.org/openni_launch). 
       
### Project Development :

I want to develop a **Mobile Robotic ARM** and use PCL Library for SLAM section.

**PCL :** I developed another project in which I include tutorials for installing PCL Library and its dependencies [here](https://github.com/ahmadkh1995/PCL_Probabilistic_Robotic).

**I)** Project Folder For connecting the ROS to Kinect360 is located in Folder: **Kinect_ROS**

For using Kinect in ROS ,these packages are necessary :
- [openni_launch](http://wiki.ros.org/openni_launch) : Launch files to open an OpenNI device and load all nodelets to convert raw depth/RGB/IR streams to depth images
- [openni_camera packages](http://wiki.ros.org/openni_camera) : A ROS driver for OpenNI depth (+ RGB) cameras
- [openni_description](http://wiki.ros.org/openni_description) : Model files of OpenNI device
- [rgbd_launch](http://wiki.ros.org/rgbd_launch) : Launch files to open an RGBD device and load all nodelets to convert raw depth/RGB/IR streams to depth images 

### PointCloud :
      //In ROS(Melodic) workspace root directory  :
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
    $ rosrun kinect_pcl kinect_pcl_file input:=/camera/depth/points
    $ rosrun rviz rviz

Add **PointCloud2** display.||||||||For **Frame :** *camera_depth_frame* .|||||||For **Topic :** *output*

### SLAM (*Simultaneous localization and mapping*) :

ROS associated package : [GMapping](http://www.openslam.org)

which requires odometry data and a source of depth data.To use the depth image for SLAM ,the point cloud should be converted  to a faked laser scan signal by cutting a horizontal slice out of the image and using the nearest distance (closest depth) in each column.
      
**Create a 2-D map from logged transform and laser scan data :**
Required Packages:

**map_server:** allows dynamically generated maps to be saved to file.

**openslam_gmapping** ||| **slam_gmapping** 

    //catkin_ws/src  directory
    $ git clone https://github.com/ros-perception/openslam_gmapping
    $ git clone https://github.com/ros-perception/slam_gmapping
    $ git clone https://github.com/ros-planning/navigation
    $ catkin_make
    $ roscore
    $ rosrun gmapping slam_gmapping scan:=base_scan
I need a ROS bag file.So first I should create a new one : 

    $ rosbag record -O mylaserdata /base_scan /tf
or if you have your own run this command:

    $ rosbag play --clock <name of the bag>
      //Wait for rosbag to finish and exit....
    $ rosrun map_server map_saver -f <my map_name>





     




