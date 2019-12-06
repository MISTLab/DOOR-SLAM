## Prepare the TX so that the GPU is usable with Docker.
First, let's make sure we don't need sudo to run Docker.
Run:
```
sudo groupadd docker
sudo usermod -aG docker $USER
```
Once you'll have rebooted, you should be able to run Docker without sudo.

For the GPU to be used within Docker containers, we use [Tegra-Docker](https://github.com/Technica-Corporation/Tegra-Docker).

On the TX, run the following:
```
git clone https://github.com/Technica-Corporation/Tegra-Docker.git
chmod +x Tegra-Docker/bin/tx2-docker
sudo cp Tegra-Docker/bin/tx2-docker /usr/bin
rm -rf Tegra-Docker
```

Now you can use the `tx2-docker` command instead of `docker` when running a container.

Doing this might overwrite some libraries installed in your images (since it replaces some directories). To fix this, you should install the required libraries locally on the TX2, and they will then be available in the shared libraries within the Docker container.
