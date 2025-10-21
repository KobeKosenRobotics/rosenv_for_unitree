[![ros2 workflow](https://github.com/hrjp/rosenv/actions/workflows/ros2-humble-image-build.yml/badge.svg)](https://hub.docker.com/repository/docker/hrjp/ros2)

![license](https://img.shields.io/github/license/KobeKosenRobotics/rosenv_for_unitree)
![size](https://img.shields.io/github/repo-size/KobeKosenRobotics/rosenv_for_unitree)
![commit](https://img.shields.io/github/last-commit/KobeKosenRobotics/rosenv_for_unitree/main)

# rosenv_for_unitree
This repository is for making ROS2 docker on Unitree robots (Jetson ARM64)  
Docker container include ROS2humble, and necesarry packages for control Unitree robots with Low-Level (i checked Go2 and G1)
## reference
- [https://github.com/hrjp/rosenv](https://github.com/hrjp/rosenv)
- [https://github.com/IntelRealSense/realsense-ros](https://github.com/IntelRealSense/realsense-ros)
- [https://github.com/unitreerobotics/unitree_ros2](https://github.com/unitreerobotics/unitree_ros2)
- [https://github.com/jetsonhacksnano/installLibrealsense](https://github.com/jetsonhacksnano/installLibrealsense)

# Setup
## 1.git colne 
```bash
git clone https://github.com/KobeKosenRobotics/rosenv_for_unitree
```

## 2. make container
- Check Jetpack of your Jetson
```bash
cat /etc/nv_tegra_release
```
```bash
cd rosenv_for_unitree/docker
docker build . -t unitree/ros2:humble
```
```bash
cd 
./rosenv_for_unitree/docker/run.bash
```

## 3. build necessary packages
### Install Realsense SDK2.0
```bash
cd /home/colcon_ws/src/
git clone https://github.com/jetsonhacksnano/installLibrealsense
cd installLibrealsense/
./installLibrealsense.sh
``` 
```bash
# If your docker can use CUDA, this step can be skipped 
vi buildLibrealsense.sh
```
```bash
# 10  USE_CUDA=false
# 35  USE_CUDA=false
```
- Check Realsense connections
```bash
realsense-viewer
```
### Install ROS packages
```bash
cd /home/colcon_ws/src
git clone https://github.com/IntelRealSense/realsense-ros.git -b ros2-master
git clone https://github.com/unitreerobotics/unitree_ros2.git
```
### Install dependencies
```bash
cd /home/colcon_ws
sudo apt-get install python3-rosdep -y
sudo apt install ros-$ROS_DISTRO-rmw-cyclonedds-cpp
sudo apt install ros-$ROS_DISTRO-rosidl-generator-dds-idl
sudo apt install libyaml-cpp-dev
sudo rosdep init # "sudo rosdep init --include-eol-distros" for Foxy and earlier
rosdep update # "sudo rosdep update --include-eol-distros" for Foxy and earlier
rosdep install -i --from-path src --rosdistro $ROS_DISTRO --skip-keys=librealsense2 -y
```
```bash
source /opt/ros/humble/setup.bash
```
```bash
colcon build
source install/setup.bash
```

