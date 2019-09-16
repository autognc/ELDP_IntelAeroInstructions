# ELDP_IntelAeroInstructions

NOTE: Give yourself a full day of work to complete the instructions below!

1) Flash Intel Aero board as described [in here](https://github.com/intel-aero/meta-intel-aero/wiki/90-(References)-OS-user-Installation) (do not install the drivers for the D435 camera!).

2) Install [ROS Kinetic](https://github.com/intel-aero/meta-intel-aero/wiki/05-Autonomous-drone-programming-with-ROS) with Realsense ROS nodes.

3) Install realsense 2.0 SDK by following the intructions [in here](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md).

4) [Create a catkin workspace](http://wiki.ros.org/catkin/Tutorials/create_a_workspace).

5) Install realsense package in catkin_ws:

```
sudo apt-get install ros-kinetic-ddynamic-reconfigure
cd ~/catkin_ws/src
git clone https://github.com/intel-ros/realsense.git
cd ~/catkin_ws/src/realsense
git checkout `git tag | sort -V | grep -P "^\d+\.\d+\.\d+" | tail -1`
cd ~/catkin_ws
catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
```

6) Install desired version of px4 control:

```
cd ~/catkin_ws/src
git clone https://github.com/radionavlab/mg_msgs.git
git clone https://github.com/radionavlab/px4_control.git
cd ~/catkin_ws/src/px4_control
git checkout lockheed_quads
```

7) Install all other ROS nodes

	- Nodes to be installed within catkin_ws:

	```
	cd ~/catkin_ws/src
	git clone https://github.com/AkellaSummerResearch/batch_pose_estimator.git
	git clone https://github.com/marcelinomalmeidan/image_filters.git
	git clone https://github.com/ros-perception/image_transport_plugins.git
	git clone https://github.com/AkellaSummerResearch/pcl_compression.git
	cd ~/catkin_ws
	catkin_make -DCMAKE_BUILD_TYPE=Release
	```

	- Install ORB-SLAM2 (this is not within catkin_ws)

	- Add the following line to ```~/.bashrc```:

	```
	export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:~/ORB_SLAM2/Examples/ROS
	```	

	- Install ORB-SLAM2

	```
	sudo apt-get install libglew-dev
	cd ~
	git clone https://github.com/stevenlovegrove/Pangolin.git
	cd Pangolin
	mkdir build
	cd build
	cmake ..
	cmake --build .
	cd ~
	git clone https://github.com/marcelinomalmeidan/ORB_SLAM2
	source ~/.bashrc
	cd ORB_SLAM2
	./build.sh
	```
