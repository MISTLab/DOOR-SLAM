# ARGoS Simulation

Docker Image `lajoiepy/doorslam-argos-simulation`

```
sudo x11docker --hostdisplay --hostnet --user=RETAIN -- --privileged -v /home/lajoiepy/Documents/masters/research_code/robust_distributed_slam/robust_distributed_slam_simulation/argos_simulation/log:/home/docker/robust_distributed_slam_simulation/argos_simulation/log -- lajoiepy/doorslam-argos-simulation:latest
```

```
docker exec -it $(docker container ls -q) /bin/bash
```

```
cd ../home/docker/robust_distributed_slam_simulation/argos_simulation/
argos3 -c robust_distributed_slam_random_walk.argos
```

```
code --user-data-dir
```