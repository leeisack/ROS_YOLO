Guide to using the YOLO system in ROS (I felt that it did not work well even if I followed the articles posted on the Internet, and I recorded it mainly for solutions by situation.) H1
===============================================================================================================
Ros installation
--------------------------------------
What is ROS? 
> ROS(Robot Operating System)is an open-source, meta-operating system for your robot. It provides the services you would expect from an operating system, including hardware abstraction, low-level device control, implementation of commonly-used functionality, message-passing between processes, and package management. Basically, it works in the Ubunto environment.
Follow as directed here
 http://wiki.ros.org/ROS/Installation
 
 
Yolo istallation
---------------------------------------------
(On the teminal)
#### download
$ mkdir -p catkin_workspace/src
$ cd catkin_workspace/src
$ git clone --recursive git@github.com:leggedrobotics/darknet_ros.git
$ cd ../
#### compile
$ catkin_make -DCMAKE_BUILD_TYPE=Release


#### As hardware, I use a zed2 camera and a jetson xavier. Therefore, if you are using different hardware, following these instructions may not produce the same results.
