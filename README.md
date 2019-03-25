# raspberrypi_conf
Build configuration for poky on the raspberrypi

# How to build the SDK

bitbake rpi-basic -c populate_sdk

# How to use the SDK
source tmp/deploy/sdk/poky-glibc-x86_64-rpi-basic-cortexa7t2hf-neon-vfpv4-toolchain-2.6.1.sh
