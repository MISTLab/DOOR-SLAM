#### Batman

Now to use batman:
```
sudo batctl if add ra0
sudo ip link set up dev bat0
```

To obtain an IPv4 address associated to the batman interface, run:
```
sudo avahi-autoipd bat0 &
```

You can now check if you see other devices using batman either by using ping with their bat0 IPv4 addresses, or checking the neighbors:
```
sudo batctl n
```


### Setting ra0 and bat0 interfaces automatically on boot, with a static IP.
We have a script ready which can run on boot and sets up the interface with a static and manually set IP.
You can find the script here: [run_batman.sh](run_batman.sh).

You should copy this script in `/etc/init.d/`, and modify the `192.168.10.150` by the static IP address you want on the bat0 interface.

Then run the following commands for the script to run on boot:
```
cd /etc/init.d
sudo chmod +x run_batman.sh
sudo update-rc.d run_batman.sh defaults
```

Once you'll reboot you should be able to check that the ra0 interface is of type IBSS connected to the NVIDIA SSID with:
```
sudo iw dev ra0 info
```

You should see the bat0 interface when running `ifconfig`, with the static IP you chose.