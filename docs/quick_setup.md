# Quick Setup
Those instructions can be used to reproduce the same setup used in our field 
experiments.

# Hardware Requirements
- [NVIDIA Jetson TX2](https://developer.nvidia.com/embedded/jetson-tx2)
- [Intel Realsense D435](https//www.intelrealsense.com/depth-camera-d435/)
- [DJI Matrice 100](https://www.dji.com/ca/matrice100?gclid=CjwKCAiA8qLvBRAbEiwAE_ZzPUsslCzK26_TNCYhazw-pswp8oyLU29d8VnYrAOUlDPAR7XsSxS3gxoC0noQAvD_BwE)

<!-- Computer basic setup (link to other file) -->
# Setup 
Follow the following instructions to setup each robot computer:
[TX2 Basic Setup](tx2_setup.md).

<!-- Pull docker images -->
Pull the following Docker image on each robot: `lajoiepy/doorslam-realsensed435-tx2-dji`

# Launch commands
<!-- Install bash commands -->
- First you need to setup the [aliases and high-level parameters](https://github.com/bramtoula/multi_robot_SLAM_separators/blob/2ce1fa45107e92bde5329d023565cf5eebe26004/docker/camera_and_slam_tx2/aliases.txt).
    - MASK & OWN_IP: IP address info `192.168.MASK.OWN_IP`.
    - MASTER: OWN_IP of the robot with the ROS master.
    - LOCAL_ROBOT_ID: Robot ID (from 0 to 2)
    - OTHER_ROBOT_ID: Neighbor ID (Assign 1 neighbor to each robot).
    - The other parameters do not need to be modified. Look to the paper for more details on those parameters.


- Make sure [this script](https://github.com/bramtoula/multi_robot_SLAM_separators/blob/2ce1fa45107e92bde5329d023565cf5eebe26004/docker/camera_and_slam_tx2/entrypoint.sh) is at the root level to run the commands.

- A typical sequence:
    - Robot 0: `camera`, `blabbermouth2`, `optimization`, `separators`
    - Robot 1: `camera`, `optimization`, `separators`
    - Pilot the robots around the test area.
    - When you are ready for the distributed pose graph optimization execute `trigger` on either Robot 0 or 1.

- Reboot between each run.

- Commands description:
    - `camera` : Launches the Docker container and the onboard camera.
    - `enter_bash` : Opens a bash command line in the Docker container already running.
    - `optimization` : Launches the optimization process.
    - `blabbermouth` : Launches communication hub for 3 robots.
    - `blabbermouth2` : Launches communication hub for 2 robots.
    - `trigger` : Triggers the distributed pose graph optimization process.
    - `separators` : Launches the inter-robot loop closures detection process.
    - `record` : Launches the data recording using rosbags.
    - `docker_bash` : Launches the Docker container and open a bash command line in it.
    - `bag_sep` : Launches the inter-robot loop closures detection process with a rosbag dataset (not using the camera).
    - `bag_sep_params` : Launches the inter-robot loop closures detection process with a rosbag dataset (not using the camera). Uses the parameters set [here](https://github.com/bramtoula/multi_robot_SLAM_separators/blob/2ce1fa45107e92bde5329d023565cf5eebe26004/docker/camera_and_slam_tx2/aliases.txt) instead of the default ones.
    - `cleancache` : Cleans the TX2 cache. This usually has a positive effect on the computer performance. We recommend to run it often.

## Good luck and have fun!

## Building the Docker image for TX2
To build the Docker image yourself to replicate our exact setup, use this Dockerfile and instructions: https://github.com/bramtoula/multi_robot_SLAM_separators/tree/master/docker/camera_and_slam_tx2

