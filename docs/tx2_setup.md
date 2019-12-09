# NVIDIA Jetson TX2 DOOR-SLAM Setup

Steps in order to use TX2 GPU in Docker, B.A.T.M.A.N. routing protocol, TP-Link Archer T2UH dongle, Intel RealSense D435.
The instructions assume the use of Jetpack 4.2.
Note that more recent Jetpack versions should make the use of Docker easier, but we haven't tested them.
We suggest to follow the steps in order, but you can skip some according to your hardware setup.

## Steps
- [Flash Jetpack 4.2](flash_jetpack4_2.md)
- [Connectech Elroy Carrier Board](connecttech_elroy.md)
- [Docker + GPU](gpu_docker.md)
- [Intel Realsense Camera](realsense_setup.md)
- [Additional Packages](additional_pkgs.md)
- [TP-Link Archer T2UH networking dongle](archer_dongle.md) (Optional)
- [B.A.T.M.A.N. ad-hoc network routing protocol](batman_setup.md) (Optional)
- [WiFi](wifi_setup.md) (Optional)