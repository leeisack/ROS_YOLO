Yolo_ROS
===========

Guide to using the YOLO system in ROS (I felt that it did not work well even if I followed the articles posted on the Internet, and I recorded it mainly for solutions by situation.)
---------------------------
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
<pre>
<code>
$ mkdir -p catkin_workspace/src
$ cd catkin_workspace/src
$ git clone --recursive git@github.com:leggedrobotics/darknet_ros.git
$ cd ../
</code>
</pre>
#### compile
<pre>
<code>
$ catkin_make -DCMAKE_BUILD_TYPE=Release
</code>
</pre>
#### YOLO version modification
> find darknet_ros.launch file
<pre>
<code>
#(Maybe here)
$ cd catkin_ws/src/darknet_ros/darknet_ros/launch
#opend darknet_ros.launch with gedit 
$ gedit darknet_ros.launch
</code>
</pre>
![modify](https://user-images.githubusercontent.com/52061393/115366004-de1b5e00-a1ff-11eb-9f87-fae479135f4b.png)
<pre>
<code>
change from
arg name="network_param_file"         default="$(find darknet_ros)/config/yolov3.yaml"/
to
arg name="network_param_file"         default="$(find darknet_ros)/config/yolov3.yaml"/
</code>
</pre>


#### As hardware, I use a zed2 camera and a jetson xavier. Therefore, if you are using different hardware, following these instructions may not produce the same results.
