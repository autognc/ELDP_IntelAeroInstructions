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

	- Install dependencies for min snap motion planner
	```
	sudo apt-get install python-wstool python-catkin-tools ros-kinetic-cmake-modules
	sudo apt-get install libsuitesparse-dev
	cd ~
	git clone https://github.com/RainerKuemmerle/g2o
	mkdir g2o/build
	cd build
	cmake ../
	make
	cd ~/catkin_ws/src
	git clone https://github.com/AkellaSummerResearch/nlopt.git 
	git clone https://github.com/AkellaSummerResearch/mav_comm.git
	git clone https://github.com/AkellaSummerResearch/catkin_simple.git
	git clone https://github.com/AkellaSummerResearch/eigen_catkin.git
	cd ~/catkin_ws
	catkin_make

	cd ~/catkin_ws/src
	git clone https://github.com/AkellaSummerResearch/glog_catkin.git
	cd ~/catkin_ws
	catkin_make

	cd ~/catkin_ws/src
	git clone https://github.com/AkellaSummerResearch/eigen_checks.git
	cd ~/catkin_ws
	catkin_make
	```

	- Install min snap motion planner

	```
	cd ~/catkin_ws/src
	git clone https://github.com/AkellaSummerResearch/mav_trajectory_generation.git
	cd ~/catkin_ws
	catkin_make
	```

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


8) Install barrier (optional: barrier allows mouse and keyboard on the jetson to be commanded within the network):

```
sudo apt install git cmake make xorg-dev g++ libcurl4-openssl-dev \
                 libavahi-compat-libdnssd-dev libssl-dev libx11-dev \
                 libqt4-dev qtbase5-dev xdotool
git clone https://github.com/AkellaSummerResearch/barrier
cd barrier
./clean_build.sh
cd build
sudo make install
```

- Run barrier once to save fingerprint

	```
	./barrier/build/bin/barrier
	```

- Configure barrier to start with terminator
	- Right-click on terminator window, click on Preferences, within Layouts add Custom command to desired window for barrier to run on. You will need two windows: on one, choose the following command (makes mouse visible on startup) ```gsettings set org.gnome.settings-daemon.plugins.cursor active false; bash```, while the following command goes on the second one: ```./barrier/build/bin/barrierc -f --enable-crypto 192.168.1.200; bash```

	- Make terminator open on startup: open ```Startup Applications``` and click on Add. On the desired command, just type ```terminator```, which is the command line to start Terminator.


8) Update Px4

- Download the file: https://raw.githubusercontent.com/PX4/Devguide/v1.9.0/build_scripts/ubuntu_sim_nuttx.sh

- Run the script: `source ubuntu_sim_nuttx.sh`

```
cd ~/Downloads
git clone https://github.com/PX4/Firmware.git
cd Firmware
git checkout v1.9.0
make intel_aerofc-v1_default
```

- Copy the file `/home/aero/Downloads/Firmware/build/intel_aerofc-v1_default/intel_aerofc-v1_default.px4` into `/etc/aerofc/px4` (note that the file might already exist in there, so you want to rename the old file if you want to have a backup of the original px4 version for the Intel Aero.

```
cd /etc/aerofc/px4/
sudo aerofc-update.sh intel_aerofc-v1_default.px4
```

Note: a compiled version of `intel_aerofc-v1_default.px4` can be found in this repository within `Files/intel_aerofc-v1_default.px4`.
