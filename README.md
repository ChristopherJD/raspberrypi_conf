# raspberrypi_conf
Build configuration for poky on the raspberrypi

# Mount external hard drive with SSTATE and Download data

It is assumed that the directory /media/external_500 exists and contains the SSTATE cache and Downloads directories for the yocto build. In my setup I have this stored on an external 500 GB drive. This is hard coded in the conf/local.conf file and can be changed.

```bash
sudo mkdir /media/external_500
sudo chown -R $(id -u):$(id -u) /media/external_500/
```

Check the UUID of the hard drive.

```bash
sudo blkid
```

Add the device to the fstab file.

```bash
UUID=bc3040c9-ae5a-4ffc-93ab-466ca2444616 /media/external_500 ext4 rw,auto,nofail 0 0
```

# How to build Raspberry Pi Image
bitbake rpi-basic

# How to view the contents of the image on workstation
https://askubuntu.com/questions/445979/how-to-mount-sd-card-image-created-with-dd

```bash
$ fdisk -u -l rpi_image280914 

Disk rpi_image280914: 16.0 GB, 16012804096 bytes
255 heads, 63 sectors/track, 1946 cylinders, total 31275008 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x000cdac7

           Device Boot      Start         End      Blocks   Id  System
rpi_image280914p1   *        2048      514047      256000    c  W95 FAT32 (LBA)
rpi_image280914p2          540672    31242239    15350784   83  Linux

```


```bash
mkdir rpi_partition2
sudo mount -o loop,offset=276824064 rpi_image280914 rpi_partition2/
```

# How to build the SDK

bitbake rpi-basic -c populate_sdk

# How to use the SDK
./tmp/deploy/sdk/poky-glibc-x86_64-rpi-basic-cortexa7t2hf-neon-vfpv4-toolchain-2.6.1.sh

# How to modify the kernel source

See notes [here](https://www.yoctoproject.org/docs/2.5/kernel-dev/kernel-dev.html#kernel-dev-common). However you cannot use linux-yocto, this must be replaced with linux-raspberrypi.

```bash
devtool modify linux-raspberrypi
```

# Setup Eclipse
Version 4.8
http://downloads.yoctoproject.org/releases/eclipse-plugin/2.6.1/oxygen
Follow the instruction found at https://wiki.phytec.com/display/public/PRODUCTINFO/How-to%3A+Set+Up+Yocto+Plugin+for+Eclipse

# Troubleshooting

## Remote Execution
If you are getting the error that "no password was provided" even though you entered the user and password for the raspberry pi then you should do the following.

windows->preferences->General->Security->Secure Storage

In the tab Password ensure that only UI Prompt is checked. Linux Integration is currently throwing an error.
