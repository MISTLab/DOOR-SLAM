### wlan0 interface on our router
To connect to a network through the wifi interface, we use wpa supplicant, and set the IP to be static. In our example, we assume that the IP addresses your network gives are `192.168.12.XX`.

Open the interfaces file to setup the IP to be static :
`sudo nano /etc/network/interfaces`
You should have the following inside, with the IP you defined for your Spiri :
```
# interfaces(5) file used by ifup(8) and ifdown(8)
# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d

# The loopback network interface
auto lo
iface lo inet loopback

auto wlan0
iface wlan0 inet static
address 192.168.12.<the number you chose for your drone’s IP>
netmask 255.255.255.0
gateway 192.168.12.1
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

Second, open the wpa supplicant conf file to define which networks to connect to :
`sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

In this file, you can set up one or more networks you want to connect to. If the network is open and has no password, you should use the configuration like the first network below. If it does require a password, use the second network below. You can add as many networks as you want, defining different priorities. If a robot sees different available networks it will connect to the one with highest priority.
If you are using some kind of filtering on your router (MAC filtering for example), don't forget to allow your Spiri!
```
network={
        ssid="<open-network-name>"
        scan_ssid=1
        key_mgmt=NONE
        priority=6
        }

network={
        ssid="<password-protected-network-name>"
        scan_ssid=1
        psk="network-password"
        key_mgmt=WPA-PSK
        priority=5
        }
```

Once you have these files changed, reboot the drone and make sure it connects to the network.
You can check your drone gets the expected IP using `ifconfig`.
You can verify that it is connected to the network by trying to ping the router from the TX :
`ping 192.168.12.1`
If you get no response then there is an issue somewhere.
The next test you should make is to try to SSH to the TX. From another computer on the same network use :
`ssh nvidia@192.168.12.XX`
replacing XX by the IP of your TX. If you didn’t change the password yet, it should still be `nvidia`.

One helpful thing you can do is add the drone to your ssh config file to easily connect to it later. On your computer, run :
`sudo nano ~/.ssh/config`
And add the following at the end, replacing <spiri-name> and <spiri-IP> by the actual name and static IP of your Spiri :
```
Host <spiri-name>
        User nvidia
        Hostname 192.168.12.<spiri-IP>
```

Now you'll be able to run the following command on your computer to access the drone :
`ssh <spiri-name>`

It is important that this works before going forward since it will be our only way to access the TX once it is inserted in the drone.

### Setting ra0 and bat0 interfaces manually (better for debugging)

#### ra0 in a mesh network for B.A.T.M.A.N

First, make sure ra0 is visible when you run `iwconfig`.
Then run the following commands to join (or create if you are the first to join) a mesh network:
```
sudo iw dev ra0 set type ibss
sudo iw dev ra0 ibss join NVIDIA 5220 
```

To check whether it has been set correctly, you can run
```
sudo iw dev ra0 info
```