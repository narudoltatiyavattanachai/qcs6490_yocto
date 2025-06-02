# qcs6490_yocto project

# Download .repo into workspace enviroement:

repo init \
-u https://github.com/rubikpi-ai/rubikpi-manifest \
-b qcom-linux-kirkstone \
-m rubikpi-6.6.52-QLI.1.3-Ver.1.1.1_qim-product-sdk-1.1.2.xml

repo sync

# Edit recipe file for adding ros2 packages
nano ~/Winsurf/rubikpi_yocto/layers/meta-qcom-distro/recipes-products/images/qcom-multimedia-image.bb

# Add ros2 packages into files
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


#"ament_cmake.bbclass" cause below disable
#ros2-control \
#ros2-controllers \
#ros2-navigation2 \
#ros2-perception \
#ros2-robot-state-publisher \
#ros2-rosbag2-transport \
#ros2-rqt \
#ros2-rviz2 \
#ros2-image-tools \
#ros2-vision-opencv



# Manual set the compilation environment:

export EXTRALAYERS="meta-qcom-qim-product-sdk"
export MACHINE=qcm6490-idp
export DISTRO=qcom-wayland
export FWZIP_PATH="`pwd`/src/vendor/thundercomm/prebuilt/BP-BINs"
source setup-environment_RUBIKPi
export BB_ENV_PASSTHROUGH_ADDITIONS="$BB_ENV_PASSTHROUGH_ADDITIONS FWZIP_PATH CUST_ID"

# Manual compilation (Flash Image & Cross Compiler):

bitbake qcom-multimedia-image
bitbake qcom-qim-product-sdk

