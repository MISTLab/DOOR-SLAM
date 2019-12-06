# Connecttech Elroy Carrier Board

We used the Connectech Elroy Carrier Board on our drones for our online experiments.
The TX2 should work "out-of-the-box" with the newer version of the carrier board. 

### Older Connecttech Elroy Carrier Board (revision E and earlier) with USB 3.
In this case, you have to install the Connecttech Board Support Package.

First, flash the TX using the instructions found here: http://connecttech.com/resource-center/cti-l4t-nvidia-jetson-board-support-package-release-notes/.

Once the TX is flashed, it still doesn't have everything you might want installed. You'll have to reboot it and connect a keyboard and screen to it to finalize the setup (choose login, password, etc.).
To finish the process, make sure the TX2 is booted on the Ubuntu homepage and connected through USB to the desktop you use with the NVIDIA SDK manager. 

Open the SDK manager, and install the additional modules you want. Make sure you don't flash it again, but only install additional modules.

