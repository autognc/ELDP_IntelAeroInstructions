# Demo launch

## Fly with px4_control

1) T265 + Mavros
```
roslaunch odom_relay sauron.launch
```

2) px4_control + joystick relay
```
roslaunch px4_control sauron.launch
```

3) Desktop: joystick node

```
roslaunch joy joy.launch
```

4) Desktop: rviz visualization (px4_control)

```
rviz -d ~/lockheed_ws/src/px4_control/Extras/Sauron.rviz
```

## Record bags for ORB_SLAM2

1) Launch realsense R200 node (also downsamples color images)

```
roslaunch realsense2_camera r200_nodelet_rgbd.launch
```

2) Record bag

```
rosbag record /Sauron/camera/color/image_raw_low_freq /Sauron/camera/depth_registered/image_raw -O r200_rover
```

## Create map with ORB_SLAM2

1) Run ORB_SLAM2

```
roslaunch ORB_SLAM2 rgbd_ns_r200.launch
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

1) T265 + Mavros
```
roslaunch odom_relay sauron.launch
```

2) px4_control + joystick relay
```
roslaunch px4_control sauron_sim.launch
```

3) Launch realsense R200 node (also downsamples color images)

```
roslaunch realsense2_camera r200_nodelet_rgbd.launch
```

4) Run ORB_SLAM2

```
roslaunch ORB_SLAM2 rgbd_ns_r200.launch
```

5) Desktop: joystick node

```
roslaunch joy joy.launch
```

6) Desktop: rviz visualization

```
rviz -d ~/lockheed_ws/src/px4_control/Extras/Sauron.rviz
```

7) Mission planner

```
roslaunch mission_planner rover_inspection.launch
```
