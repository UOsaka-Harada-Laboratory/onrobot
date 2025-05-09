# onrobot

[![support level: community](https://img.shields.io/badge/support%20level-community-lightgray.svg)](https://rosindustrial.org/news/2016/10/7/better-supporting-a-growing-ros-industrial-software-platform)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![repo size](https://img.shields.io/github/repo-size/UOsaka-Harada-Laboratory/onrobot)

ROS drivers for OnRobot Grippers.
This repository was inspired by [ros-industrial/robotiq](https://github.com/ros-industrial/robotiq).

## Features

- [Ubuntu 20.04 PC](https://ubuntu.com/certified/laptops?q=&limit=20&vendor=Dell&vendor=Lenovo&vendor=HP&release=20.04+LTS)
  - [ROS Noetic (Python3)](https://wiki.ros.org/noetic/Installation/Ubuntu)
- Controller for OnRobot [RG2](https://onrobot.com/en/products/rg2-gripper) / [RG6](https://onrobot.com/en/products/rg6-gripper) via Modbus/TCP
- Controller for OnRobot [VG10](https://onrobot.com/en/products/vg10-electric-vacuum-gripper) / [VGC10](https://onrobot.com/en/products/vgc10) via Modbus/TCP

## Dependency

- pymodbus==2.5.3  
- [roboticsgroup/roboticsgroup_upatras_gazebo_plugins](https://github.com/roboticsgroup/roboticsgroup_upatras_gazebo_plugins.git)  

## Installation

```bash
cd catkin_ws/src && git clone https://github.com/UOsaka-Harada-Laboratory/onrobot.git --depth 1 && git clone https://github.com/roboticsgroup/roboticsgroup_upatras_gazebo_plugins.git --depth 1 && cd ../ && sudo rosdep install --from-paths ./src --ignore-packages-from-source --rosdistro noetic -y --os=ubuntu:focal -y && sudo apt install ros-noetic-ros-control ros-noetic-ros-controllers && catkin build -DPYTHON_EXECUTABLE=/usr/bin/python3
```

## Usage

1. Connect the cable between Compute Box and Tool Changer
2. Connect an ethernet cable between Compute Box and your computer
3. Execute programs (Please refer to [onrobot/Tutorials](http://wiki.ros.org/onrobot/Tutorials))

### RG2 / RG6

#### Send motion commands
##### Interactive mode
```bash
roslaunch onrobot_rg_control bringup.launch gripper:=[rg2/rg6] ip:=XXX.XXX.XXX.XXX
```
```bash
rosrun onrobot_rg_control OnRobotRGSimpleController.py
```

##### ROS service call
```bash
roslaunch onrobot_rg_control bringup.launch gripper:=[rg2/rg6] ip:=XXX.XXX.XXX.XXX
```
```bash
rosrun onrobot_rg_control OnRobotRGSimpleControllerServer.py
```
```bash
rosservice call /onrobot_rg/set_command c
```
```bash
rosservice call /onrobot_rg/set_command o
```
```bash
rosservice call /onrobot_rg/set_command '!!str 300'
```
```bash
rosservice call /onrobot_rg/restart_power
```

#### Simulation
##### Display models
```bash
roslaunch onrobot_rg_description disp_rg6_model.launch
```
```bash
roslaunch onrobot_rg_description disp_rg2_model.launch
```

<img src=image/rg6_rviz.gif width=320>  <img src=image/rg2_rviz.gif width=320>  

##### Gazebo simulation
```bash
roslaunch onrobot_rg_gazebo bringup_rg6_gazebo.launch
```
```bash
rostopic pub -1 /onrobot_rg6/joint_position_controller/command std_msgs/Float64 "data: 0.5"
```
```bash
roslaunch onrobot_rg_gazebo bringup_rg2_gazebo.launch
```
```bash
rostopic pub -1 /onrobot_rg2/joint_position_controller/command std_msgs/Float64 "data: 0.5"
```

<img src=image/rg6_gazebo.gif width=320>  <img src=image/rg2_gazebo.gif width=320>  

### VG10 / VGC10

#### Send motion commands
##### Interactive mode
```bash
roslaunch onrobot_vg_control bringup.launch ip:=YYY.YYY.YYY.YYY
```
```bash
rosrun onrobot_vg_control OnRobotVGSimpleController.py  
```

##### ROS service call
```bash
roslaunch onrobot_vg_control bringup.launch ip:=YYY.YYY.YYY.YYY
```
```bash
rosrun onrobot_vg_control OnRobotVGSimpleControllerServer.py
```
```bash
rosservice call /onrobot_vg/set_command g
```
```bash
rosservice call /onrobot_vg/set_command r
```
```bash
rosservice call /onrobot_vg/set_command '!!str 128'
```

#### Simulation
##### Display models
```bash
roslaunch onrobot_vg_description disp_vgc10_1cup_model.launch
```
```bash
roslaunch onrobot_vg_description disp_vgc10_4cups_model.launch
```
```bash
roslaunch onrobot_vg_description disp_vg10_model.launch
```

## Author

[Takuya Kiyokawa](https://takuya-ki.github.io/)

## Contributors

[Roberto Mendivil C](https://github.com/Robertomendivil97)  

## License

This software is released under the MIT License, see [LICENSE](./LICENSE).
