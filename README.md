# RS to Velodyne
A ros tool for converting Robosense pointcloud to Velodyne pointcloud format, which can be directly used for downstream algorithm, such as LOAM, LEGO-LOAM, LIO-SAM, etc.

## Currently support:


### 1. [robosense XYZIRT] to [velodyne XYZIRT / XYZIR / XYZI]: (Recommended)
RS-16, RS-32, RS-Ruby, RS-BP and RS-Helios LiDAR point cloud.

### 2. [robosense XYZI] to [velodyne XYZIR]:
RS-16 and RS-Ruby LiDAR point cloud, More LiDAR model support is coming soon. 

### Notice: For rslidar_sdk v1.5+(2022), minor modifications should be made in the code. Please see https://github.com/HViktorTsoi/rs_to_velodyne/issues/11. This fix will soon be merged into the main stream.

## Usage

### 1. XYZIRT input
For **XYZIRT** format point clouds from `/rslidar_points` (Notice that, you need the latest 
[rslidar_sdk](https://github.com/RoboSense-LiDAR/rslidar_sdk) driver to get this type of point cloud):
```
rosrun rs_to_velodyne rs_to_velodyne XYZIRT XYZIRT
# or
rosrun rs_to_velodyne rs_to_velodyne XYZIRT XYZIR
# or
rosrun rs_to_velodyne rs_to_velodyne XYZIRT XYZI
``` 
The output point clouds are **XYZIRT** / **XYZIR** / **XYZI** point cloud `/velodyne_points` in Velodyne's format.

### 2. XYZI input
For **XYZI** format point clouds from `/rslidar_points`:
```
rosrun rs_to_velodyne rs_to_velodyne XYZI XYZIR
``` 
The output point clouds are **XYZIR** point cloud `/velodyne_points` in Velodyne's format.


## Subscribes
`/rslidar_points`: sensor_msgs.PointCloud2, from Robosense LiDAR.

## Publishes
`/velodyne_points`: sensor_msgs.PointCloud2, the frame_id is `velodyne`.

---

## ROS2 Humble 运行说明

### 编译

1. 确保你已经安装了ROS2 Humble和必要的依赖：
```bash
sudo apt install ros-humble-rclcpp ros-humble-sensor-msgs ros-humble-std-msgs \
  ros-humble-pcl-conversions ros-humble-pcl-ros ros-humble-message-filters \
  libpcl-dev
```

2. 将本包放入你的ROS2工作空间（例如 `~/ros2_ws/src/`）：
```bash
cd ~/ros2_ws/src
# 假设你已经克隆或复制了rs_to_velodyne包到这里
```

3. 编译工作空间：
```bash
cd ~/ros2_ws
colcon build --packages-select rs_to_velodyne
```

4. 设置环境变量：

**临时设置（仅当前终端有效）：**
```bash
source install/setup.bash
```

**永久设置（所有新终端自动生效）：**
将以下内容添加到 `~/.bashrc` 文件末尾：
```bash
# ROS2 工作空间环境变量 (rs_to_velodyne)
if [ -f ~/ros2_ws/install/setup.bash ]; then
    source ~/ros2_ws/install/setup.bash
fi
```

然后执行：
```bash
source ~/.bashrc
```

或者直接运行：
```bash
echo '# ROS2 工作空间环境变量 (rs_to_velodyne)
if [ -f ~/ros2_ws/install/setup.bash ]; then
    source ~/ros2_ws/install/setup.bash
fi' >> ~/.bashrc
source ~/.bashrc
```

### 运行

#### 1. XYZIRT 输入格式
对于来自 `/rslidar_points` 的 **XYZIRT** 格式点云：
```bash
ros2 run rs_to_velodyne rs_to_velodyne XYZIRT XYZIRT
# 或
ros2 run rs_to_velodyne rs_to_velodyne XYZIRT XYZIR
# 或
ros2 run rs_to_velodyne rs_to_velodyne XYZIRT XYZI
```
输出点云为 `/velodyne_points` 话题上的 **XYZIRT** / **XYZIR** / **XYZI** 格式点云（Velodyne格式）。

#### 2. XYZI 输入格式
对于来自 `/rslidar_points` 的 **XYZI** 格式点云：
```bash
ros2 run rs_to_velodyne rs_to_velodyne XYZI XYZIRT
```
输出点云为 `/velodyne_points` 话题上的 **XYZIR** 格式点云（Velodyne格式）。

### 验证

运行后，你可以使用以下命令查看话题：
```bash
# 查看所有话题
ros2 topic list

# 查看点云话题信息
ros2 topic info /velodyne_points

# 查看点云数据（需要安装rviz2）
ros2 run rviz2 rviz2
```

### 注意事项

- 确保你的Robosense LiDAR驱动已经发布 `/rslidar_points` 话题
- 输出话题 `/velodyne_points` 的frame_id会被设置为 `velodyne`
- 如果遇到编译错误，请确保所有依赖都已正确安装
