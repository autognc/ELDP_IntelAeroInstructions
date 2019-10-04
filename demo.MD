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

4) Desktop: joystick node

```
roslaunch joy joy.launch
```

3) Desktop: rviz visualization (px4_control)

```
rviz -d ~/lockheed_ws/src/px4_control/Extras/Sauron.rviz
```
