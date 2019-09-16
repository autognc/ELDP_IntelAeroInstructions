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
