# Demo launch

## Check if firmware is properly installed

```
sudo aero-get-version.py
```

## Fly with px4_control

1) T265 + Mavros (Intel Aero)
```
roslaunch odom_relay sauron.launch
```

2) px4_control + joystick relay (Intel Aero)
```
roslaunch px4_control sauron.launch
```

3) Joystick node (Desktop)

```
roslaunch joy joy.launch
```

4) Desktop: rviz visualization (px4_control)

```
rviz -d ~/lockheed_ws/src/px4_control/Extras/SauronInspection.rviz
```

## Record bags for ORB_SLAM2

1) Launch realsense D435 node (TX2)

```
roslaunch realsense2_camera rs_slam_ns.launch
```

2) Record bag (TX2)

```
rosbag record /Sauron/camera/color/image_raw /Sauron/camera/depth_registered/image_raw -O rover_map
```

## Create map with ORB_SLAM2

1) Run ORB_SLAM2

```
roslaunch ORB_SLAM2 rgbd_ns.launch
```

2) Make sure that ORB_SLAM2 is mapping, not only localizing

```
rosservice call /Sauron/RGBD/is_mapping_mode true
```

3) Run bag

```
rosbag play r200_rover.bag
```

## Run Mission Planner Simulation

1) T265 + Mavros (Intel Aero)
```
roslaunch odom_relay sauron.launch
```

2) D435 + ORB_SLAM2 + Portrait mode (TX2)

```
roslaunch ORB_SLAM2 rgbd_ns.launch
```

3) px4_control + joystick relay (Intel Aero)
```
roslaunch px4_control sauron.launch
```

4) Joystick node (Desktop)

```
roslaunch joy joy.launch
```

4) Rviz visualization (Desktop)

```
rviz -d ~/lockheed_ws/src/px4_control/Extras/SauronInspection.rviz
```

5) Mission planner (Desktop)

```
roslaunch mission_planner rover_inspection.launch
```
