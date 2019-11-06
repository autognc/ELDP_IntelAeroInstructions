# Demo launch

This page assumes that you are running the T265 on the TX2 and R200 on the Intel Aero.

## Check if firmware is properly installed

```
sudo aero-get-version.py
```

## Fly with px4_control (Instructions for how to use px4_control using an xbox 360 controller can be found in [here](https://github.com/radionavlab/px4_control.git))

1) Mavros (Intel Aero)
```
roslaunch odom_relay sauron_mavros.launch
```

2) px4_control + joystick relay (Groundstation)
```
roslaunch px4_control sauron.launch
```

3) T265 (TX2)
```
roslaunch odom_relay sauron_t265.launch
```

4) Joystick node (Groundstation)
```
roslaunch joy joy.launch
```

5) rviz visualization (Groundstation)
```
rviz -d ~/lockheed_ws/src/px4_control/Extras/SauronInspection.rviz
```

## Record bags for ORB_SLAM2

1) Launch realsense R200 node (Intel Aero - also downsamples color images)

```
roslaunch odom_relay r200_nodelet_rgbd.launch
```

2) Record bag (Intel Aero)

```
rosbag record /Sauron/camera/color/image_raw_low_freq /Sauron/camera/depth_registered/image_raw -O r200_rover
```

Press Ctrl+C to stop recording the bag.

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

## Capture Waypoints

1) Run ORB-SLAM2
```
roslaunch ORB_SLAM2 rgbd_ns_r200.launch
```

2) Run capture waypoints node (push enter to capture waypoins)
```
roslaunch capture_waypoints capture.launch
```

3) Run bag if not capturing images in real time
```
rosbag play r200_rover.bag
```

- Capture waypoints following the instruction in the `capture_waypoints` terminal. As the waypoint capturing ends, it will save the waypoints into a file `wps.txt`. This has to be copied into the mission planner in the groundstation. Copy it into `mission_planner/waypoints/wps_r200.txt`. 

## Run Mission Planner Inspection

1) Mavros + R200 + ORB_SLAM2 + Portrait Mode (Intel Aero)
```
roslaunch odom_relay inspection_no_t265.launch
```

2) T265 (TX2)
```
roslaunch odom_relay sauron_t265.launch
```

3) px4_control + joystick relay (Groundstation)
```
roslaunch px4_control sauron.launch
```

4) Joystick node (Groundstation)

```
roslaunch joy joy.launch
```

5) Rviz visualization (Groundstation)

```
rviz -d ~/lockheed_ws/src/px4_control/Extras/SauronInspection.rviz
```

6) Mission planner (Groundstation)

```
roslaunch mission_planner rover_inspection.launch
```

## Map the environment (for collision avoidance)

1) Mavros + R200 + ORB_SLAM2 + Portrait Mode (Intel Aero)
```
roslaunch odom_relay inspection_no_t265.launch
```

2) T265 (TX2)
```
roslaunch odom_relay sauron_t265.launch
```

3) Joystick node (Groundstation)

```
roslaunch joy joy.launch
```

4) Rviz visualization (Groundstation)

```
rviz -d ~/lockheed_ws/src/px4_control/Extras/SauronInspection.rviz
```

5) Mission planner (Groundstation)

```
roslaunch mission_planner mapper_inspection.launch
```

## Run Mission Planner Collision Avoidance

1) Mavros + R200 + ORB_SLAM2 + Portrait Mode (Intel Aero)
```
roslaunch odom_relay inspection_no_t265.launch
```

2) T265 (TX2)
```
roslaunch odom_relay sauron_t265.launch
```

3) px4_control + joystick relay (Groundstation)
```
roslaunch px4_control sauron.launch
```

4) Joystick node (Groundstation)

```
roslaunch joy joy.launch
```

5) Rviz visualization (Groundstation)

```
rviz -d ~/lockheed_ws/src/px4_control/Extras/SauronInspection.rviz
```

6) Mission planner (Groundstation)

```
roslaunch mission_planner collision_avoidance_example.launch
```
