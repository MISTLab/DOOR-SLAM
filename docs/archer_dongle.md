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
