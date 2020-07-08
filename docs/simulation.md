# ARGoS Simulation

Docker Image `lajoiepy/doorslam-argos-simulation`

### 1.
Create a folder named `datasets` inside your HOST_FOLDER.

### 2.
Run the following commands:
```
sudo x11docker --hostdisplay --hostnet --user=RETAIN -- --privileged -v HOST_FOLDER:/home/docker/robust_distributed_slam_simulation/argos_simulation/log -- lajoiepy/doorslam-argos-simulation:latest
```
### 3.
Open a shell in the container and run the simulation
```
docker exec -it $(docker container ls -q) /bin/bash
```

```
cd ../home/docker/robust_distributed_slam_simulation/argos_simulation/
argos3 -c robust_distributed_slam_random_walk.argos
```

### If you want to edit the code and configs
```
code --user-data-dir
```