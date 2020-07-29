# ARGoS Simulation

Welcome to the ARGoS multi-robot slam simulation.
The goal of this simulation is to showcase our distributed implementation of the
Gauss-Seidel pose graph otimization technique combined with our distributed 
inter-robot loop closures outliers rejection technique. 

Since the setup is quite complex we provide a Docker Image 
`lajoiepy/doorslam-argos-simulation` to easily run the simulation. Although we
don't recommend it, you can look at the [Dockerfile]() for the setup 
instructions.

The following instructions indicate how to start the simulation.
## 1. Log/result folder setup
Designate a folder ( `HOST_FOLDER` ) to share files between the host machine and 
the Docker container. The simulation results and logs will be store in this location.


Create a folder named `datasets` inside the `HOST_FOLDER`.

## 2. Launch container
Run the following commands:
```
sudo x11docker --hostdisplay --hostnet --user=RETAIN -- --privileged -v HOST_FOLDER:/home/docker/robust_distributed_slam_simulation/argos_simulation/log -- lajoiepy/doorslam-argos-simulation:latest
```

## 3. Start simulation
Open a shell in the container and run the simulation
```
docker exec -it $(docker container ls -q) /bin/bash
```

```
cd ../home/docker/robust_distributed_slam_simulation/argos_simulation/
argos3 -c robust_distributed_slam_random_walk.argos
```

Press the simulator play button.

[//]: # (## Simulation Explanations
Visual output
The simulation presents 
ARGoS logs
log file results)

## Bonus: If you want to edit the code and configs
Start a shell in the container and run the following command that will start VSCode.
```
code --user-data-dir
```