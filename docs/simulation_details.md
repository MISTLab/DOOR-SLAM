# Simulation Build Details
Those instructions refer to this [Dockerfile](https://github.com/lajoiepy/robust_distributed_slam_simulation/blob/master/docker/devel/Dockerfile).

## Acces to GUI
The ARGoS simulation needs access to a graphical user interface to work. Many options are available to have access to the GUI of the apps running in your docker container. I propose using https://github.com/mviereck/x11docker, but feel free to use another one. Note that I will assume the usage of x11docker in some of the following commands.

## Build the image
For the first build execute: 
```
docker build . --tag rdpgo --network host
```
If you modified the code from this repository and you want to use the latest version, use the following command: 
```
docker build . --tag rdpgo --network host --build-arg CODE_UPDATE=<dummy-arg>
```
Be sure to change the `CODE_UPDATE` value every time to invalidate your build cache.

## ARGoS download link
The ARGoS download link on this [line](https://github.com/lajoiepy/robust_distributed_slam_simulation/blob/e55d28cd00d80e20d63629a4fda3938b1965c539/docker/devel/Dockerfile#L53)
might change overtime and might need to be updated. Look here for the latest link: https://www.argos-sim.info/core.php.

## To run a container
Use the following command while specifying a folder on your host machine to store the log files from the simulation. 
```
sudo x11docker --hostdisplay --hostnet --user=RETAIN -- --privileged -v <log-folder-on-host-computer>:/home/docker/robust_distributed_slam_simulation/argos_simulation/log -- rdpgo:latest
```

## Launching the simulation
If the correct `ENTRYPOINT` is specified in the Dockerfile, the simulation should start with the previous command. \
Otherwise, you can start a bash shell into the container with the following command: 
```
docker exec -w /home/docker/robust_distributed_slam_simulation/argos_simulation -it <container-name> /bin/bash
```
You can check the `<container-name>` using `docker ps`. 

Then you can start the simulation yourself by going into the folder `/home/docker/robust_distributed_slam_simulation/argos_simulation`
and executing :
```
argos3 -c robust_distributed_slam.argos
```

## To debug with Visual Studio Code
Some editing and debugging tools are installed in the image for development purposes (gedit, nano, strace, valgrind, and Visual Studio Code).
To debug the code from this repository:
1. Open Visual Studio Code in the folder `/home/docker/robust_distributed_slam_simulation` with the `--user-data-dir /home/docker/code/` argument.
2. Launch the simulation as explained previously.
3. Start the `(gdb) Attach` configuration in the Visual Studio Code debugging tool and select the running `argos3` process.
4. You are ready to place your breakpoints and debug!

## Questions?
For further questions or comments, feel free to contact me at : `lajoie.py@gmail.com`.
