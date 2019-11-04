# ELDP_IntelAeroInstructions

NOTE: Give yourself a full day of work to complete the instructions below!

1) Flash Intel Aero board as described [in here](https://github.com/intel-aero/meta-intel-aero/wiki/90-(References)-OS-user-Installation) (do not install the drivers for the D435 camera!). When installing Ubuntu, make sure to install WITHOUT using the internet, otherwise some drivers will break.

If you don't want the drone to create its own hotspot on boot, run the following command on Ubuntu:
```
nmcli c down hotspot
```


NOTE: When booting Ubuntu, DO NOT choose the most updated kernel version, you need to choose the Intel Aero kernel (it is actually the default one).

2) Install [ROS Kinetic](https://github.com/intel-aero/meta-intel-aero/wiki/05-Autonomous-drone-programming-with-ROS) with Realsense ROS nodes.

3) Install realsense 2.0 SDK by following the intructions [in here](https://github.com/IntelRealSense/librealsense/blob/master/doc/installation.md).

4) [Create a catkin workspace](http://wiki.ros.org/catkin/Tutorials/create_a_workspace).

5) Install realsense package in catkin_ws:

```
sudo apt-get install ros-kinetic-ddynamic-reconfigure
cd ~/catkin_ws/src
git clone https://github.com/autognc/realsense-ros
cd ~/catkin_ws/src/realsense
git checkout lockheed
cd ~/catkin_ws
catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release
```

6) Install desired ROS packages:

```
cd ~/catkin_ws/src
git clone https://github.com/radionavlab/mg_msgs.git
git clone https://github.com/radionavlab/px4_control.git
git clone https://github.com/autognc/odom_relay
git clone https://github.com/AkellaSummerResearch/pcl_compression.git
git clone https://github.com/marcelinomalmeidan/image_filters.git
cd ~/catkin_ws/src/px4_control
git checkout lockheed_quads
cd ~/catkin_ws
catkin_make
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

7) Update Px4

- NOTE: You can either compile Px4, or use the compiled version which is already in this repository. The compiled version of `intel_aerofc-v1_default.px4` can be found in this repository within `Files/intel_aerofc-v1_default.px4`. Copy this file into `/etc/aerofc/px4`, and execute:

```
cd /etc/aerofc/px4/
sudo aerofc-update.sh intel_aerofc-v1_default.px4
``` 

The instructions below guide you on compiling `.px4` (in case you want to compile a different version)

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

8) Load Px4 parameters into the drone using QGroundControl

Open `QGroundControl` (you can find it in the `Files` folder within this repository:
```
./Files/QGroundControl.AppImage
```

- On the top left corner, click on the three gears. Then, click on `parameters` (bottom left two gears).

- On the top right, click on `Tools`, then `Load from file...`. Add the file `aero.params`

9) Calibrate the sensors on the drone

- Open `QGroundControl`:
```
./Files/QGroundControl.AppImage
```

- On the top left corner, click on the three gears. Then, click on `Sensors`. Go through the procedure of calibrating the `Gyroscope`, `Accelerometer`, and `Level Horizon`.

10) Increase the speed for mavlink communication:

- Edit the file `/etc/mavlink-router/main.conf`, and change the TCP speed to `TcpServerPort=921600`.

11) Disable wireless power saving

The Aero board saves energy on Wifi by throttling it at times. In order to disable it, open `/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf` and change the `wifi.powersave` parameter from `3` to `2`. Values are 0 (use default), 1 (ignore/don't touch), 2 (disable) or 3 (enable).

Reboot the drone for these changes to go into effect.

12) Log Px4 flights (this is not an instruction, this is a reference to where to find log data)

- Px4 Flights are automatically logged in `/var/lib/mavlink-router`. You can upload the `*.ulg` files into [this link](https://logs.px4.io/) to visualize plots of the drone's data during flight.


13) Flash the TX2 board as described in [here](https://github.com/autognc/ELDP_IntelAeroInstructions/blob/master/Flash_TX2.md).

14) Install barrier (optional: barrier allows mouse and keyboard on the Intel Aero to be commanded within the network):

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
