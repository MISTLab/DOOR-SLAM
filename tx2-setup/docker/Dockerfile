FROM lajoiepy/realsense-tx2-px4


WORKDIR /root/

##################################################### Python catkin tools #####################################################
RUN apt-get update && apt-get install -y --no-install-recommends libopenni2-dev wget && \
    sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list' && \
    wget http://packages.ros.org/ros.key -O - | apt-key add - && \
    apt-get remove -y wget && \
    apt-get update && \
    apt-get install -y --no-install-recommends python-catkin-tools && \
    rm -rf /var/lib/apt/lists/*

##################################################### Tensorflow #####################################################
COPY tensorflow-1.11.0-cp27-cp27mu-linux_aarch64.whl /tmp/

RUN apt-get update && \
    apt-get install -y --no-install-recommends python-dev python-pip python-h5py python-wheel && \
    pip install /tmp/tensorflow-1.11.0-cp27-cp27mu-linux_aarch64.whl && \
    pip install --upgrade numpy && \
    apt-get install -y --no-install-recommends python-scipy && \
    apt-get remove -y python-pip python-wheel && \
    rm /tmp/tensorflow-1.11.0-cp27-cp27mu-linux_aarch64.whl && \
    rm -rf /var/lib/apt/lists/*

##################################################### GTSAM #####################################################
RUN git clone https://bitbucket.org/gtborg/gtsam  && \
    cd gtsam && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make -j2 && \
    make install && \
    cd ../.. && \
    rm -rf gtsam && \
    ldconfig

##################################################### RTAB-Map ROS #####################################################
RUN apt-get update && apt-get install -y \
    ros-melodic-rtabmap-ros \
    && apt-get remove -y \
    ros-melodic-rtabmap \
    && rm -rf /var/lib/apt/lists/*

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN git clone https://github.com/introlab/rtabmap.git && \
    source /ros_entrypoint.sh && \
    cd rtabmap/build && \
    cmake .. && \
    make -j4 && \
    make install && \
    cd ../.. && \
    rm -rf rtabmap && \
    ldconfig

RUN source /ros_entrypoint.sh && \
    mkdir -p catkin_ws/src && \
    cd catkin_ws/src && \
    git clone https://github.com/bramtoula/rtabmap_ros.git && \
    cd .. && \
    catkin_make -j2 -DCMAKE_INSTALL_PREFIX=/opt/ros/melodic install && \
    cd && \
    rm -rf catkin_ws

##################################################### Buzz #####################################################
RUN git clone https://github.com/MISTLab/Buzz.git /home/docker/buzz && \
    cd /home/docker/buzz && \
    mkdir build && \
    cd build && \
    cmake -DCMAKE_BUILD_TYPE=Debug ../src && \
    make && \
    make install && \
    ldconfig && \
    rm -r /home/docker/buzz

##################################################### DJI SDK #############################################################
# Install DJI_SDK
RUN apt-get update && apt-get install -y ros-melodic-nmea-msgs \
    && rm -rf /var/lib/apt/lists/*

ARG clone_dji_sdk=unknown

RUN mkdir ~/dji_ws && mkdir ~/dji_ws/src && cd ~/dji_ws/src &&\
    git clone https://github.com/lajoiepy/Onboard-SDK-ROS onboard-sdk-ros &&\
    cd onboard-sdk-ros &&\
    git checkout m100_record

RUN cd ~/dji_ws &&\
    source /opt/ros/melodic/setup.bash &&\
    catkin build

##################################################### Multi-robot SLAM separators package #####################################################
# --------------------------------------------------- Reset argument ------------------------------------------#
ARG rebuild_multi_robot_slam_separators=0
# -------------------------------------------------------------------------------------------------------------#

RUN git clone https://github.com/bramtoula/multi_robot_SLAM_separators.git  && \
    cd multi_robot_SLAM_separators/ros_ws/src/multi_robot_separators/scripts && \
    git clone https://github.com/uzh-rpg/netvlad_tf_open.git && \
    cd netvlad_tf_open/checkpoints && \
    apt-get update && \
    apt-get install -y --no-install-recommends unzip wget && \
    wget http://rpg.ifi.uzh.ch/datasets/netvlad/vd16_pitts30k_conv5_3_vlad_preL2_intra_white.zip && \
    unzip vd16_pitts30k_conv5_3_vlad_preL2_intra_white.zip && \ 
    mv vd16_pitts30k_conv5_3_vlad_preL2_intra_white/* . && \
    rm -r vd16_pitts30k_conv5_3_vlad_preL2_intra_white && \
    rm vd16_pitts30k_conv5_3_vlad_preL2_intra_white.zip && \
    apt-get remove -y unzip wget && \
    rm -rf /var/lib/apt/lists/* && \
    cd ~/multi_robot_SLAM_separators/ros_ws && \
    source ~/dji_ws/devel/setup.bash && \
    catkin build && \
    source devel/setup.bash && \
    mkdir ~/multi_robot_SLAM_separators/logs && \
    mkdir ~/multi_robot_SLAM_separators/bags && \
    chmod +x  ~/multi_robot_SLAM_separators/ros_ws/src/multi_robot_separators/scripts/find_separators.py 

ENV PYTHONPATH=":PYTHONPATH:/root/multi_robot_SLAM_separators/ros_ws/src/multi_robot_separators/scripts/netvlad_tf_open/python"

##################################################### Robust Distributed Mapper - RDPGO - Buzz SLAM #####################################################
# --------------------------------------------------- Reset argument ------------------------------------------#
ARG rebuild_rdpgo=0
# -------------------------------------------------------------------------------------------------------------#
# Clone RDPGO
RUN mkdir ~/rdpgo_ws && mkdir ~/rdpgo_ws/src && cd ~/rdpgo_ws/src &&\
    git clone https://github.com/lajoiepy/robust_distributed_slam_module.git robust_distributed_slam_module &&\
    cd robust_distributed_slam_module &&\
    git checkout DOOR-SLAM-1.0 &&\
    git submodule update --init --recursive

# Install Robust Distributed Mapper
RUN cd  ~/rdpgo_ws/src/robust_distributed_slam_module/buzz_slam/robust_distributed_mapper/cpp &&\
    mkdir build &&\
    cd build &&\
    cmake -DCMAKE_BUILD_TYPE=Debug ../ &&\
    make &&\
    make install

# Install Buzz SLAM
RUN cd ~/rdpgo_ws/ &&\
    source ~/multi_robot_SLAM_separators/ros_ws/devel/setup.bash &&\
    catkin build

# Compile buzz script
RUN cd ~/rdpgo_ws/src/robust_distributed_slam_module/scripts &&\
    bzzc ros_slam.bzz

##################################################### Debugging tools #####################################################
# Install and setup some editing and debugging tools
RUN apt-get update && apt-get install -y --no-install-recommends vim htop net-tools gedit nano wget strace valgrind && \
    rm -rf /var/lib/apt/lists/* 

#####################################################   Blabbermouth  #####################################################
RUN git clone https://github.com/lajoiepy/blabbermouth.git &&\
    cd blabbermouth &&\
    mkdir build &&\
    cd build &&\
    cmake ../src &&\
    make

##################################################### Repull and build #####################################################
# --------------------------------------------------- Reset argument ------------------------------------------#
ARG repull_and_build=0
# -------------------------------------------------------------------------------------------------------------#

RUN cd multi_robot_SLAM_separators/ros_ws/ && \
    git pull origin master && \
    source ~/dji_ws/devel/setup.bash && \
    catkin build && \
    cd ~/rdpgo_ws/src/robust_distributed_slam_module && \
    git pull --recurse-submodules && \
    cd  ~/rdpgo_ws/src/robust_distributed_slam_module/buzz_slam/robust_distributed_mapper/cpp/build && \
    cmake -DCMAKE_BUILD_TYPE=Debug ../ && \
    make && \
    make install && \
    cd ~/rdpgo_ws/ &&\
    source ~/multi_robot_SLAM_separators/ros_ws/devel/setup.bash &&\
    catkin build && \
    cd ~/rdpgo_ws/src/robust_distributed_slam_module/scripts &&\
    bzzc ros_slam.bzz

##################################################### Setup Entrypoint ##################################################### 
ENV TZ=America/New_York

COPY entrypoint.sh entrypoint.sh
RUN chmod +x entrypoint.sh
ENTRYPOINT ["/root/entrypoint.sh"]
