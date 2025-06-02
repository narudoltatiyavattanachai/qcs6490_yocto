# QCS6490 Yocto Project

## Overview

```bash
sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool
```


## Host PC Prerequisites

Before starting the setup, ensure your host PC has the following packages installed:
Install all prerequisites with the following command:

```bash
sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool
```

This repository contains setup instructions for the QCS6490 Yocto build environment with ROS2 integration. Follow these steps to set up, configure, and build the system.

## Setup Instructions

### 1. Initialize and Sync Repository
Download `.repo` into your workspace environment:

```bash
# Initialize the repository
repo init \
    -u https://github.com/rubikpi-ai/rubikpi-manifest \
    -b qcom-linux-kirkstone \
    -m rubikpi-6.6.52-QLI.1.3-Ver.1.1.1_qim-product-sdk-1.1.2.xml

# Synchronize the repository
repo sync
```

### 2. Configure ROS2 Packages
Edit the recipe file to add ROS2 packages:

```bash
# Open the recipe file in an editor
nano ~/Winsurf/rubikpi_yocto/layers/meta-qcom-distro/recipes-products/images/qcom-multimedia-image.bb
```

Add the following ROS2 packages to the file:

```
IMAGE_INSTALL:append = " \
        ros-core \
        ros-base \
        ros2cli \
        ros2launch \
        demo-nodes-cpp \
        demo-nodes-py \
        tf2 \
        tf2-tools \
        vision-opencv \
        image-tools \
        image-transport \
        launch \
        launch-ros \
        examples-rclcpp-minimal-publisher \
        examples-rclcpp-minimal-subscriber \
        examples-rclpy-minimal-publisher \
        examples-rclpy-minimal-subscriber \
        diagnostics \
        rosbag2 \
        rqt \
        rqt-common-plugins \
        perception-pcl \
        pcl-msgs \
        pcl-conversions \
        cv-bridge \
        sensor-msgs \
        can-msgs \
        ackermann-msgs \
        geometry-msgs \
        builtin-interfaces \
        lifecycle-msgs \
"
```

### Note: Disabled ROS2 Packages
The following packages are currently disabled due to issues with `ament_cmake.bbclass`:

```
# ros2-control
# ros2-controllers
# ros2-navigation2
# ros2-perception
# ros2-robot-state-publisher
# ros2-rosbag2-transport
# ros2-rqt
# ros2-rviz2
# ros2-image-tools
# ros2-vision-opencv
```



## Build Configuration

### 3. Set the Compilation Environment
Set up the required environment variables:


```bash
# Export build configuration variables
export EXTRALAYERS="meta-qcom-qim-product-sdk"
export MACHINE=qcm6490-idp
export DISTRO=qcom-wayland
export FWZIP_PATH="`pwd`/src/vendor/thundercomm/prebuilt/BP-BINs"

# Source the environment setup script
source setup-environment_RUBIKPi

# Configure additional environment pass-through variables
export BB_ENV_PASSTHROUGH_ADDITIONS="$BB_ENV_PASSTHROUGH_ADDITIONS FWZIP_PATH CUST_ID"
```

### 4. Build the Image
Compile the flash image and cross compiler:

```bash
# Build the multimedia image
bitbake qcom-multimedia-image

# Build the QIM product SDK
bitbake qcom-qim-product-sdk
```
