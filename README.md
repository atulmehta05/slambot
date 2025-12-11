# 🛠️ Explorer Bot V2 Setup (ROS2 Humble)

## 🔧 Prerequisites

- Ubuntu 22.04
- ROS 2 Humble installed (https://docs.ros.org/en/humble/Installation.html)
---

## ⚙️ Add to .bashrc (ROS 2 Environment Setup)

Append the following lines to your ~/.bashrc for seamless ROS 2 usage:
```bash
source /opt/ros/humble/setup.bash
source ~/ros2_ws/install/setup.bash
source /usr/share/colcon_cd/function/colcon_cd.sh
source /usr/share/colcon_argcomplete/hook/colcon-argcomplete.bash
export _colcon_cd_root=/opt/ros/humble/
```
---

## ⚙️ Debugging Commands

Run this command to add user to i2c group:
```bash
sudo usermod -aG i2c explorer
```
Run this command to check i2c devices address:
```bash
sudo i2cdetect -y 1
```
Run this command to enable i2c access:
```bash
sudo chmod 666 /dev/i2c-1         #give R/W to everyone
sudo chown root.gpio /dev/gpiomem #give access to GPIO
sudo chmod g+rw /dev/gpiomem      #give access to GPIO
```
---

## ⚙️ Commands to run the package/bot

> Explorerbot:
```bash
ros2 run explorerbot_v2_control motor_controller
```
```bash
ros2 launch explorerbot_v2 explorerbot_rplidar.launch.py
```
> VM-Ware:
```bash
ros2 launch explorerbot_v2 explorerbot.launch.py
```
```bash
ros2 launch explorerbot_v2 explorerbot_cartographer.launch.py
```
```bash
ros2 launch explorerbot_v2 explorerbot_navigation.launch.py
```
```bash
ros2 launch explorerbot_v2 explorerbot_occupancy_grid.launch.py
```
---

## 📥 RealVNC Server Setup for Raspberry Pi 4 (Ubuntu 22.04 Desktop)
- Refer this YT Vid: https://www.youtube.com/watch?v=qxey8eKi9bE
- Refer this BLOG Post: https://omar2cloud.github.io/rasp/realvnc/
- Download ARM64 Version of RealVNC Server: https://www.realvnc.com/en/connect/download/vnc
> Follow the Installation commands below:
```bash
sudo dpkg -i realvnc-vnc-server_6.7.2.43081_arm64.deb
sudo systemctl enable vncserver-virtuald.service
sudo systemctl enable vncserver-x11-serviced.service
sudo systemctl start vncserver-virtuald.service
sudo systemctl start vncserver-x11-serviced.service
```
---
## 📥 One-line Installation Command

Install all required packages in one go:

```bash
sudo apt install i2c-tools -y
sudo apt install python3-colcon-common-extensions -y
sudo apt install terminator -y
sudo apt install ros-humble-xacro -y
sudo apt install ros-humble-joint-state-publisher -y
sudo apt install ros-humble-joint-state-publisher-gui -y
sudo apt install ros-humble-ros2-control -y
sudo apt install ros-humble-ros2-controllers -y
sudo apt install ros-humble-rplidar-ros -y
sudo apt install ros-humble-slam-toolbox -y
sudo apt install ros-humble-nav2-amcl -y
sudo apt install ros-humble-navigation2 -y
sudo apt install ros-humble-twist-mux -y
sudo apt install ros-humble-cartographer-ros -y

#Install if Support for 3D Mapping is required
sudo apt install ros-humble-rtabmap-ros -y 

#Install if Support for Gazebo is required
sudo apt install gazebo -y
sudo apt install ros-humble-gazebo-ros-pkgs -y
sudo apt install ros-humble-gazebo-ros2-control -y
sudo apt install ros-humble-ros-gz-sim -y
sudo apt install ros-humble-ros-gz-bridge -y
```
---

# Installation Instructions for WiringPi

```sh
# fetch the source
cd
sudo apt install git
git clone https://github.com/WiringPi/WiringPi.git
cd WiringPi

# build the package
./build debian
cd debian-template 

# install it
sudo apt install ./wiringpi_3.16_amd64.deb
```
---

# Installation Instructions for Orbbec Astra Pro plus package

> If your ROS 2 command does not auto-complete, put the following two lines into your `.bashrc`
> or `.zshrc`

```bash
eval "$(register-python-argcomplete3 ros2)"
eval "$(register-python-argcomplete3 colcon)"
```

Create `colcon` workspace

```bash
mkdir -p ~/ros2_ws/src
```

Get source code

```bash
cd ~/ros2_ws/src
git clone https://github.com/orbbec/OrbbecSDK_ROS2.git
cd OrbbecSDK_ROS2/
git checkout main
```

Install deb dependencies

```bash
# assume you have sourced ROS environment, same blow
sudo apt install libgflags-dev nlohmann-json3-dev  \
ros-$ROS_DISTRO-image-transport  ros-${ROS_DISTRO}-image-transport-plugins ros-${ROS_DISTRO}-compressed-image-transport \
ros-$ROS_DISTRO-image-publisher ros-$ROS_DISTRO-camera-info-manager \
ros-$ROS_DISTRO-diagnostic-updater ros-$ROS_DISTRO-diagnostic-msgs ros-$ROS_DISTRO-statistics-msgs \
ros-$ROS_DISTRO-backward-ros libdw-dev
```

Install udev rules.

```bash
cd  ~/ros2_ws/src/OrbbecSDK_ROS2/orbbec_camera/scripts
sudo bash install_udev_rules.sh
sudo udevadm control --reload-rules && sudo udevadm trigger
```
