[English](./README.md) | 简体中文

# 功能介绍

YDLIDAR ROS2驱动，以ROS2标准消息格式发送激光雷达数据。

# 物品清单

完善的产品型号请以[ydlidar官网](http://ydlidar.cn/lidars/triangulation.html)为准，这里以YDLIDAR X3为例进行演示。

![YDLIDAR](images/YDLidar.jpg  "YDLIDAR")

| 物料选项    | 清单      | 
| ------- | ------------ | 
| RDK X3  | [购买链接](https://developer.horizon.ai/sunrise) | 
| YDLIDAR X3 | [购买链接](http://ydlidar.cn/products/view/6.html) | 

# 使用方法

## 准备工作

1. 地平线RDK已烧录好地平线提供的Ubuntu 20.04系统镜像。

2. YDLIDAR正确链接RDK X3

## 安装YDLIDAR驱动

通过终端或者VNC连接RDK X3，执行以下命令

tros foxy 版本 
```bash
sudo apt update
sudo apt install -y tros-ydlidar-ros2-driver
```
tros humble 版本
```bash
sudo apt update
sudo apt install -y tros-humble-ydlidar-ros2-driver
```

**注意：如果安装s时YDLIDR已连接在RDK X3上，则安装完后需要重新拔插一次**

## 运行YDLIDAR

tros foxy 版本
```bash
source /opt/tros/setup.bash
ros2 launch ydlidar_ros2_driver ydlidar_launch.py
```
tros humble 版本
```bash
source /opt/tros/humble/setup.bash
ros2 launch ydlidar_ros2_driver ydlidar_launch.py
```

## 查看雷达数据

### 方式1 命令行方式

新打开一个终端，在里面输入以下命令查看激光雷达输出数据

tros foxy 版本
```bash
source /opt/tros/setup.bash
ros2 topic echo /scan
```
tros humble 版本
```bash
source /opt/tros/humble/setup.bash
ros2 topic echo /scan
```

### 方式2 RVIZ方式

在PC或者支持RVIZ的环境下安装ROS2，这里以foxy版本为例，运行

tros foxy 版本
```bash
source /opt/ros/foxy/setup.bash
ros2 run rviz2 rviz2
```
tros humble 版本
```bash
source /opt/ros/humble/setup.bash
ros2 run rviz2 rviz2
```

添加LaserScan，配置Reliability Policy为System Default

![RVIZ](images/rviz.png  "CONFIG")

设置Fixed Frame为base_link或者laser_link即可看到激光雷达采集数据

![RVIZ](images/lidar_rviz.png  "CONFIG")


# 接口说明

## 话题

### 发布话题
| Topic                | Type                    | Description                                      |
|----------------------|-------------------------|--------------------------------------------------|
| `scan`               | sensor_msgs/LaserScan   | 二维激光雷达扫描数据                |

## 服务

### 订阅服务
| Service                | Type                    | Description                                      |
|----------------------|-------------------------|--------------------------------------------------|
| `stop_scan`          | std_srvs::Empty   | 关闭雷达                                         |
| `start_scan`         | std_srvs::Empty   | 打开雷达                                          |

## 参数
| Parameter name | Data Type | detail                                                       |
| -------------- | ------- | ------------------------------------------------------------ |
| port         | string | 设置激光雷达设备端口号<br/>例如串口 `/dev/ttyUSB0`, 网口`192.168.1.11`，默认值`/dev/ydlidar` |
| frame_id     | string | frame名称默认值: `laser_frame` |
| ignore_array | string | 过滤测量点<br/>eg: `-90, -80, 30, 40` |
| baudrate     | int | 串口波特率 <br/>默认值: `230400` |
| lidar_type     | int | 雷达型号 <br/>0 -- TYPE_TOF<br/>1 -- TYPE_TRIANGLE<br/>2 -- TYPE_TOF_NET <br/>默认值: `1` |
| device_type     | int | 设备通信类型 <br/>0 -- YDLIDAR_TYPE_SERIAL<br/>1 -- YDLIDAR_TYPE_TCP<br/>2 -- YDLIDAR_TYPE_UDP <br/>默认值: `0` |
| sample_rate     | int | 采样频率. <br/>默认值: `9` |
| abnormal_check_count     | int | 启动异常时重试次数 <br/>默认值: `4` |
| fixed_resolution     | bool | 使能修正角分辨率 <br/>默认值: `true` |
| reversion     | bool | 改变扫描方向. <br/>默认值: `true` |
| inverted     | bool | 改变扫描方向，顺时针或者逆时针.<br/>false -- ClockWise.<br/>true -- CounterClockWise  <br/>默认值: `true` |
| auto_reconnect     | bool | 自动重新连接雷达.<br/>true -- hot plug. <br/>默认值: `true` |
| isSingleChannel     | bool | 是否单通道.<br/>默认值: `false` |
| intensity     | bool | 激光雷达是否输出测量强度.<br/>true -- G2 LiDAR.<br/>默认值: `false` |
| support_motor_dtr     | bool | 激光雷达是否可以通过串行DTR启动和停止.<br/>默认值: `false` |
| angle_min     | float | 最小测量角度.<br/>默认值: `-180` |
| angle_max     | float | 最大测量角度.<br/>默认值: `180` |
| range_min     | float | 最小测量范围.<br/>默认值: `0.1` |
| range_max     | float | 最大测量范围.<br/>默认值: `16.0` |
| frequency     | float | 扫描频率.<br/>默认值: `10.0` |
| invalid_range_is_inf     | bool | Invalid Range is inf.<br/>true -- inf.<br/>false -- 0.0.<br/>默认值: `false` |

