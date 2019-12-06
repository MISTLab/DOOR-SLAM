# Steps we used to setup our TX2s using Jetpack 4.2.

Steps in order to use a TX2 GPU in Docker, with B.A.T.M.A.N., a TP-Link Archer T2UH dongle, a RealSense D435, based on Jetpack 4.2.
Note that more recent Jetpack versions should make the use of Docker easier, but we haven't tested them.

## Flash the OS using Jetpack 4.2
For this step, we use NVIDIA's SDK manager.

Download the SDK manager here: https://developer.nvidia.com/embedded/jetpack.

Install it, and then you can open it typing `sdkmanager` in a terminal.

If you don't plan on using an older Connectech Elroy carrier board (revision E and earlier), or don't care about using USB 3 on it, you can follow the steps to install Jetpack 4.2 (not 4.2.1 in our case, since we had issues with librealsense with this version), making sure to flash the OS and install the packages you'll need.

### If you want to use an older Connectech Elroy carrier board (revision E and earlier) with USB 3.
In this case, you have to install the Connecttech Board Support Package.

First, flash the TX using the instructions found here: http://connecttech.com/resource-center/cti-l4t-nvidia-jetson-board-support-package-release-notes/.

Once the TX is flashed, it still doesn't have everything you might want installed. You'll have to reboot it and connect a keyboard and screen to it to finalize the setup (choose login, password, etc.).
To finish the process, make sure the TX2 is booted on the Ubuntu homepage and connected through USB to the desktop you use with the NVIDIA SDK manager. 

Open the SDK manager, and install the additional modules you want. Make sure you don't flash it again, but only install additional modules.


## Rebuild the kernel to have the B.A.T.M.A.N. module, and librealsense.
We have scripts in order to rebuild the kernel with the necessary modules.
This will take a while to run though.
```
git clone https://github.com/bramtoula/buildLibrealsense2AndBatmanTX2.git
cd buildLibrealsense2AndBatmanTX2
cd buildLibrealsense2AndBatmanTX2
./buildPatchedKernel.sh
sudo cp ./image/Image /boot
```

Reboot after this.

Now activate the batman-adv module and install some necessary packages:
```
sudo modprobe batman-adv
sudo apt-get install batctl bridge-utils
```


## Install drivers for the TP-Link Archer T2UH dongle.
```
git clone https://github.com/xtknight/mt7610u-linksys-ae6000-wifi-fixes.git
cd mt7610u-linksys-ae6000-wifi-fixes
make clean
make
sudo make install
```

Now if you connect the dongle and reboot, you should see it, probably on ra0, when running `iwconfig`.

## Clean up
We can clean the repos used to rebuild the kernel and the dongle driver, and remove the kernel sources:
```
cd ~/buildLibrealsense2AndBatmanTX2
./removeAllKernelSources.sh
cd ..
rm -rf mt7610u-linksys-ae6000-wifi-fixes buildLibrealsense2AndBatmanTX2
```

## Prepare the TX so that the GPU is usable with Docker.
First, let's make sure we don't need sudo to run Docker.
Run:
```
sudo groupadd docker
sudo usermod -aG docker $USER
```
Once you'll have rebooted, you should be able to run Docker without sudo.

For the GPU to be used within Docker containers, we use [Tegra-Docker](https://github.com/Technica-Corporation/Tegra-Docker).

On the TX, run the following:
```
git clone https://github.com/Technica-Corporation/Tegra-Docker.git
chmod +x Tegra-Docker/bin/tx2-docker
sudo cp Tegra-Docker/bin/tx2-docker /usr/bin
rm -rf Tegra-Docker
```

Now you can use the `tx2-docker` command instead of `docker` when running a container.

Doing this might overwrite some libraries installed in your images (since it replaces some directories). To fix this, you should install the required libraries locally on the TX2, and they will then be available in the shared libraries within the Docker container.


## Install libraries needed for our code
On the TX2 itself, we need libraries used in our Docker containers.
```
sudo apt-get install rhash curl libuv1 libsqlite3-dev libsuitesparse-dev libfreenect-dev libdc1394-22-dev libglvnd-dev libopencv-contrib3.2 libmetis-dev libboost-all-dev libpcl-dev liblz4-dev libogre-1.9-dev liburdfdom-dev liblog4cxx-dev libtinyxml2-dev libassimp4 libyaml-cpp0.5v5 geographiclib-tools
```


