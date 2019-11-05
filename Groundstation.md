These are the instructions to install packages on the ground station desktop

- Create catkin workspace

```
mkdir -p ~/lockheed_ws
cd ~/lockheed_ws
catkin_make
```


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
cd ~/lockheed_ws/src
git clone https://github.com/AkellaSummerResearch/nlopt.git 
git clone https://github.com/AkellaSummerResearch/mav_comm.git
git clone https://github.com/AkellaSummerResearch/catkin_simple.git
git clone https://github.com/AkellaSummerResearch/eigen_catkin.git
git clone https://github.com/radionavlab/mg_msgs.git
cd ~/lockheed_ws
catkin_make

cd ~/lockheed_ws/src
git clone https://github.com/AkellaSummerResearch/glog_catkin.git
cd glog_catkin
git checkout 314b53e 
cd ~/lockheed_ws
catkin_make

cd ~/lockheed_ws/src
git clone https://github.com/AkellaSummerResearch/eigen_checks.git
cd ~/lockheed_ws
catkin_make
```

- Install min snap path planner

```
cd ~/lockheed_ws/src
git clone https://github.com/AkellaSummerResearch/mav_trajectory_generation.git
cd ~/lockheed_ws
catkin_make
```

- Install the desired version of mavros

```
cd ~/lockheed_ws/src
git clone https://github.com/mavlink/mavros.git
cd ~/lockheed_ws/src/mavros
git checkout 0.24.0
cd ~/lockheed_ws
catkin_make
```

- Install the desired version of px4_control

```
cd ~/lockheed_ws/src
git clone https://github.com/radionavlab/px4_control.git
cd ~/lockheed_ws/src/px4_control
git checkout lockheed_quads
```

- Install the desired version of joystick drivers

```
cd ~/lockheed_ws/src
git clone https://github.com/radionavlab/joystick_drivers.git
cd ~/lockheed_ws/src/joystick_drivers
git checkout indigo-devel
cd ~/lockheed_ws
catkin_make
```

- Install osqp
```
cd ~
git clone https://github.com/oxfordcontrol/osqp
cd osqp
git checkout release-0.5.0
git submodule update --init --recursive
mkdir build
cd build
cmake -G "Unix Makefiles" ..
cmake --build .
sudo cmake --build . --target install
```

- Install all other important packages

```
cd ~/lockheed_ws/src
git clone https://github.com/AkellaSummerResearch/tf_publisher
git clone https://github.com/radionavlab/vicon.git
git clone https://github.com/AkellaSummerResearch/batch_pose_estimator.git
git clone https://github.com/AkellaSummerResearch/capture_waypoints.git
git clone https://github.com/AkellaSummerResearch/yolo_triangulation.git
git clone --recursive https://github.com/AkellaSummerResearch/darknet_ros.git
git clone https://github.com/AkellaSummerResearch/vision_opencv.git
git clone https://github.com/marcelinomalmeidan/image_filters.git 
git clone https://github.com/marcelinomalmeidan/mapper.git
git clone --recursive https://github.com/AkellaSummerResearch/p4_ros.git
git clone https://github.com/AkellaSummerResearch/mission_planner.git
cd ~/lockheed_ws
catkin_make
```
