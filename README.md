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
 
Zed_ros installation
--------------------------
> What is Zed?
> https://www.stereolabs.com/docs/installation/
<br>
detail information for installation: https://www.stereolabs.com/docs/ros/
<br>
#### installation
```
$ cd ~/catkin_ws/src
$ git clone https://github.com/stereolabs/zed-ros-wrapper.git
$ cd ../
$ rosdep install --from-paths src --ignore-src -r -y
$ catkin_make -DCMAKE_BUILD_TYPE=Release
$ source ./devel/setup.bash
```
#### run
ZED camera: 
```
$ roslaunch zed_wrapper zed.launch
```
ZED Mini camera:
```
$ roslaunch zed_wrapper zedm.launch
```
ZED 2 camera: 
```
$ roslaunch zed_wrapper zed2.launch
```

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
#### result
![스크린샷, 2021-04-21 14-37-07](https://user-images.githubusercontent.com/52061393/115502208-7e7c8b80-a2af-11eb-91c8-1c55980b51b1.png)
yolov2-tiy
<br>
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
> For example, if you want to change from yolov2.tiny version to yolov3
> Modify the dragged part
> #### result
> ![스크린샷, 2021-04-21 11-35-47](https://user-images.githubusercontent.com/52061393/115488651-d7d7c100-a295-11eb-91c1-f41bdc7b6ca6.png)


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
#### You have to set it up for each camera you use. Currently, I am using a ZED2 camera, so I will first tell you how to set up a ZED camera, and then write the code for the USB_CAM that is commonly used.
> > What is zed2 cmera?
> > <br>
> > https://www.stereolabs.com/docs/
### First, if you use the zed2 camera
> Since the zed2 camera provides various functions such as depth, tracking, object recognition, etc., the user has to set the required topic.
> <br>
> > Details on the topic:
> > <br>
> > https://www.stereolabs.com/docs/ros/zed-node/#zed-parameters
> <br>
> The topic code of the desired function must be written in the ros.yaml file.
> <br>
![스크린샷, 2021-04-21 11-21-07](https://user-images.githubusercontent.com/52061393/115501283-dd410580-a2ad-11eb-9087-2571d6eba089.png)

> <br>
> ```
> subscribers:
> camera_reading:
> topic: /zed/left/image_raw_color
> queue_size: 1
> ```
### Second, the most commonly used USB_CAM case
> #### install
> ```
> sudo apt-get install ros-kinetic-usb-cam
> ```
> #### topic setting
> ```
> subscribers:
> camera_reading:
>   topic: /usb_cam/image_raw
>   queue_size: 1
> ```
> #### run
> ```
> roslaunch usb_cam usb_cam-test.launch
> ```


Error occurrence situation
===========================

## CASE1
> ### When Darknet doesn't work
> ![스크린샷, 2021-04-21 13-42-54](https://user-images.githubusercontent.com/52061393/115497947-918b5d80-a2a7-11eb-9097-26dc6d31615b.png)
> #### build
> ```
> $ cd catkin_ws
> $ source devel/setup.bash
> ```
> #### run
> ```
> $ roslaunch darknet_ros darknet_ros.launch
> ```

## CASE2
> ### When the Zed camera doesn't work
> ![스크린샷, 2021-04-21 13-42-20](https://user-images.githubusercontent.com/52061393/115497923-89332280-a2a7-11eb-871e-d9ff224e14f0.png)
> #### build
> ```
> $ cd catkin_ws
> $ source ./devel/setup.bash
> ```
> #### run
> ```
> $ roslaunch zed_wrapper zed2.launch 
> ```

## CASE3
> ### Darkent_ROS is working, but When the desired camera (now using zed2) is not recognized and only wait for image is displayed on the terminal.
> In this case, you need to find the ros.yaml file and yolov3.yaml file and make changes.
> The ros.yaml,yolov3.yaml files are here.
> ```
> $ cd catkin_ws/src/darknet_ros/darknet_ros/config
> ```
> ### Change the code in ros.yaml like this
> ```
> subscribers:
> camera_reading:
> topic: /zed/left/image_raw_color
> queue_size: 1
> actions:
> camera_reading:
> name: /darknet_ros/check_for_objects
> publishers:
> object_detector:
> topic: /darknet_ros/found_object
> queue_size: 1
> latch: false
> bounding_boxes:
> topic: /darknet_ros/bounding_boxes
> queue_size: 1
> latch: false
> detection_image:
> topic: /darknet_ros/detection_image
> queue_size: 1
> latch: true
> image_view:
> enable_opencv: true
> wait_key_delay: 1
> enable_console_output: true
> ```
> ### Change the code in yolov3.yaml like this(You can see the screenshot at the bottom)
> Add the dragged part.
> <br>
> ![스크린샷, 2021-04-21 11-31-45](https://user-images.githubusercontent.com/52061393/115488394-5c760f80-a295-11eb-88de-d7450c9c72e8.png)

RVIZ
===========
> #### Additionally, it can be visually helpful if you use RVIZ. RVIZ is a 3D visualization tool for ROS applications.
> install
> The installation command differs depending on your ROS (in my case it was the melodic version).
> ```
> $ sudo apt-get install ros-groovy-rviz
> # or
> $ sudo apt-get install ros-hydro-rviz
> # or
> $ sudo apt-get install ros-indigo-rviz
> # or
> $ sudo apt-get install ros-kinetic-rviz
> # or
> $ sudo apt-get install ros-melodic-rviz
> # or
> $ sudo apt-get install ros-noetic-rviz
> ```
> build
> ```
> $ source /opt/ros/melodic/setup.bash
> $ roscore
> ```
> run
> '''
> $ rosrun rviz rviz
> '''
> ![스크린샷, 2021-04-21 13-45-02](https://user-images.githubusercontent.com/52061393/115498058-ca2b3700-a2a7-11eb-94fb-e785dda9f632.png)
> If yolo_ros, zed camera is on, press ADD in RVIZ to see various functions.
> ![스크린샷, 2021-04-21 13-46-55](https://user-images.githubusercontent.com/52061393/115500616-9d2d5300-a2ac-11eb-80a9-5cfd8878ffae.png)
![스크린샷, 2021-04-21 14-11-49](https://user-images.githubusercontent.com/52061393/115500625-a1597080-a2ac-11eb-9aea-65258729c97e.png)
