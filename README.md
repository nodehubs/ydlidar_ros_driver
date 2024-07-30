English| [简体中文](./README_cn.md)

# Function Introduction

YDLIDAR ROS2 driver sends LiDAR data in ROS2 standard message format.

# Inventory

For a comprehensive list of product models, please refer to [YDLIDAR official website](http://ydlidar.cn/lidars/triangulation.html). Here we use YDLIDAR X3 as an example for demonstration.

![YDLIDAR](images/YDLidar.jpg  "YDLIDAR")

| Item Options    | List      | 
| ------- | ------------ | 
| RDK X3  | [Purchase Link](https://developer.horizon.ai/sunrise) | 
| YDLIDAR X3 | [Purchase Link](http://ydlidar.cn/products/view/6.html) | 

# Instructions

## Preparation

1. Horizon RDK has been pre-installed with the provided Ubuntu 20.04 system image.

2. YDLIDAR is properly connected to RDK X3.

## Installing YDLIDAR Driver

Connect to RDK X3 via terminal or VNC, and execute the following commands:

tros foxy: 
```bash
sudo apt update
sudo apt install -y tros-ydlidar-ros2-driver
```
tros humble:
```bash
sudo apt update
sudo apt install -y tros-humble-ydlidar-ros2-driver
```

**Note: If YDLIDAR is connected to RDK X3 during the installation, it needs to be reconnected after installation**

## Running YDLIDAR

tros foxy:
```bash
source /opt/tros/setup.bash
ros2 launch ydlidar_ros2_driver ydlidar_launch.py
```
tros humble:
```bash
source /opt/tros/humble/setup.bash
ros2 launch ydlidar_ros2_driver ydlidar_launch.py
```

## Viewing LiDAR Data

### Method 1: Command Line

Open a new terminal and input the following command to view LiDAR output data:

tros foxy:
```bash
source /opt/tros/setup.bash
ros2 topic echo /scan
```
tros humble:
```bash
source /opt/tros/humble/setup.bash
ros2 topic echo /scan
```

### Method 2 RVIZ Method

Install ROS2 on a PC or environment supporting RVIZ. Here is an example using the foxy version, run the following commands:

tros foxy:
```bash
source /opt/ros/foxy/setup.bash
ros2 run rviz2 rviz2
```
tros humble:
```bash
source /opt/ros/humble/setup.bash
ros2 run rviz2 rviz2
```

Add LaserScan and configure the Reliability Policy as System Default.

![RVIZ](images/rviz.png  "CONFIG")

Set the Fixed Frame as base_link or laser_link to see the data collected by the LiDAR.

![RVIZ](images/lidar_rviz.png  "CONFIG")


# Interface Description

## Topics

### Published Topics
| Topic                | Type                    | Description                                      |
|----------------------|-------------------------|--------------------------------------------------|
| `scan`               | sensor_msgs/LaserScan   | Two-dimensional LaserScan data                |

## Services

### Subscribed Services
| Service                | Type                    | Description                                      |
|----------------------|-------------------------|--------------------------------------------------|
| `stop_scan`          | std_srvs::Empty   | Stop the LiDAR                                         |
| `start_scan`         | std_srvs::Empty   | Start the LiDAR                                          |

## Parameters
| Parameter name | Data Type | detail                                                       |
| -------------- | ------- | ------------------------------------------------------------ |
| port         | string | Set the port of the LiDAR device<br/>Example: `/dev/ttyUSB0` for serial port, `192.168.1.11` for ethernet port, default value: `/dev/ydlidar` |
| frame_id     | string | Frame name, default value: `laser_frame` |
| ignore_array | string | Filter measurement points<br/>eg: `-90, -80, 30, 40` |
| baudrate     | int | Serial port baudrate <br/>Default value: `230400` |
| lidar_type     | int | LiDAR model <br/>0 -- TYPE_TOF<br/>1 -- TYPE_TRIANGLE<br/>2 -- TYPE_TOF_NET <br/>Default value: `1` |
| device_type     | int | Device communication type <br/>0 -- YDLIDAR_TYPE_SERIAL<br/>1 -- YDLIDAR_TYPE_TCP<br/>2 -- YDLIDAR_TYPE_UDP <br/>Default value: `0` |
| sample_rate     | int | Sampling frequency. <br/>Default value: `9` |
| abnormal_check_count     | int | Retry count when startup fails. <br/>Default value: `4` |
| fixed_resolution     | bool | Enable correction of angular resolution. <br/>Default value: `true` |
| reversion     | bool | Change scan direction. <br/>Default value: `true` |
| inverted     | bool | Change scan direction, clockwise or counterclockwise.<br/>false -- Clockwise.<br/>true -- CounterClockwise  <br/>Default value: `true` || auto_reconnect     | bool | Auto-reconnect the LiDAR.<br/>true -- hot plug. <br/>Default value: `true` |
| isSingleChannel     | bool | Whether it is a single channel.<br/>Default value: `false` |
| intensity     | bool | Whether the LiDAR outputs measurement intensity.<br/>true -- G2 LiDAR.<br/>Default value: `false` |
| support_motor_dtr     | bool | Whether the LiDAR can be started and stopped via serial DTR.<br/>Default value: `false` |
| angle_min     | float | Minimum measurement angle.<br/>Default value: `-180` |
| angle_max     | float | Maximum measurement angle.<br/>Default value: `180` |
| range_min     | float | Minimum measurement range.<br/>Default value: `0.1` |
| range_max     | float | Maximum measurement range.<br/>Default value: `16.0` |
| frequency     | float | Scanning frequency.<br/>Default value: `10.0` |
| invalid_range_is_inf     | bool | Invalid Range is inf.<br/>true -- inf.<br/>false -- 0.0.<br/>Default value: `false` |