Yolo_ROS
===========
### Guide to using the YOLO system in ROS 
#### (I felt that it did not work well even if I followed the articles posted on the Internet, and I recorded it mainly for solutions by situation.)
---------------------------
Ros installation
--------------------------------------
What is ROS? 
> ROS(Robot Operating System)is an open-source, meta-operating system for your robot. It provides the services you would expect from an operating system, including hardware abstraction, low-level device control, implementation of commonly-used functionality, message-passing between processes, and package management. Basically, it works in the Ubunto environment.

> > If you want to install, go here and follow the instructions.
> > http://wiki.ros.org/ROS/Installation
 
 
Yolo istallation
---------------------------------------------
(On the teminal)

#### download
```
$ mkdir -p catkin_ws/src
$ cd catkin_ws/src
$ git clone --recursive git@github.com:leggedrobotics/darknet_ros.git
$ cd ../
```
#### compile
```
$ catkin_make -DCMAKE_BUILD_TYPE=Release
```
#### run
```
$ roslaunch darknet_ros darknet_ros.launch
```
>I mention that there are some people who have problems sometimes, but if there is a yolov3 folder in catkin_ws/src rather than a darknet_ros folder, you need to change the code a little and run it.
```
$ roslaunch darknet_ros yolov3.launch
```

YOLO version modification
-------------------------------------------
> The default version is yolov2tiny, but
If you want to use a different version of the weights (go to https://github.com/leoll2/darknet_ros_zed and follow the weights download guide) and Find the darknet_ros.launch file among the files in catkin_ws.
```
#(Maybe here)
$ cd catkin_ws/src/darknet_ros/darknet_ros/launch
#opend darknet_ros.launch with gedit 
$ gedit darknet_ros.launch
```
![modify](https://user-images.githubusercontent.com/52061393/115366004-de1b5e00-a1ff-11eb-9f87-fae479135f4b.png)

> For example, if you want to change from yolov2.tiny version to yolov3

change from
```
arg name="network_param_file"         default="$(find darknet_ros)/config/yolov2-tiny.yaml"/
```
to
```
arg name="network_param_file"         default="$(find darknet_ros)/config/yolov3.yaml"/
```
> yolov3 launch
```
$ roslaunch darknet_ros darknet_ros.launch
```
result
-----------------------------------------
![result](https://user-images.githubusercontent.com/52061393/115367393-21c29780-a201-11eb-96b1-a9ba339c380e.png)
-------------------------------

Settings for each camera used
============================

Error occurrence situation
===========================


