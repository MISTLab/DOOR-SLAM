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