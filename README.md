# qcs6490_yocto project

# Download .repo into workspace:

repo init \
-u https://github.com/rubikpi-ai/rubikpi-manifest \
-b qcom-linux-kirkstone \
-m rubikpi-6.6.52-QLI.1.3-Ver.1.1.1_qim-product-sdk-1.1.2.xml

repo sync


# Quick Rubik Pi Board compilation:

./rubikpi_build.sh

# Manual set the compilation environment
export EXTRALAYERS="meta-qcom-qim-product-sdk"
export MACHINE=qcm6490-idp
export DISTRO=qcom-wayland
export FWZIP_PATH="`pwd`/src/vendor/thundercomm/prebuilt/BP-BINs"
source setup-environment_RUBIKPi
export BB_ENV_PASSTHROUGH_ADDITIONS="$BB_ENV_PASSTHROUGH_ADDITIONS FWZIP_PATH CUST_ID"

# Manual compilation (Flash Image & Cross Compiler)
bitbake qcom-multimedia-image
bitbake qcom-qim-product-sdk