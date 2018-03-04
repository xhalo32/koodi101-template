## Raspberry pi first time setup
1. Login with **pi:raspberry**
2. Type ```sudo raspi-config```
3. Change the password for pi user to something else
4. Under network options, configure the wifi
5. Under localization options, configure keyboard layout
6. Under interfacing options, enable **SPI** and **I2C**
7. Exit the raspi-config

RPI should now connect to wifi. You can check the ip address by typing
```ip addr```

It should produce output like
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether b8:27:eb:f9:6a:c2 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.101/24 brd 192.168.1.255 scope global wlan0
       valid_lft forever preferred_lft forever
    inet6 fe80::799c:261a:a62:b56a/64 scope link 
       valid_lft forever preferred_lft forever
```
In this example, the ip address would be **192.168.1.101**

If you want to use ssh to connect to your RPI, you can do it by writing
```
sudo systemctl enable ssh
sudo systemctl start ssh
```

Now you can connect from your own machine with ```ssh pi@<ip>``` if you are in the same network.

## Run the project
Let's update our system and install some needed libraries.
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python3-requests python3-envirophat
```

To test if our application works, you can create a "request bin" for
testing purposes in [requestbin](https://requestb.in).
You will get bucket with url similiar to *https://requestb.in/v6lrggv6*

You can now try our app by starting it with
```
ENDPOINT=https://requestb.in/v6lrggv6 python3 phat.py
```

Your requests should appear into your browser after you refresh it.

## Starting the app on boot
Raspberry pi's operating system uses a thing called [systemd](https://www.freedesktop.org/wiki/Software/systemd/) to handle every process that is running in the background.

There is *phatsender.service* that describes how our process can be started.
Modify correct endpoint there and copy it into right place by
```
nano phatsender.service
sudo cp phatsender.service /etc/systemd/system/
systemctl start phatsender
```

Now you can view the status and the logs in following way
```
systemctl status phatsender
journalctl -u phatsender
```

To make the process start on boot, we have to enable it by
```
sudo systemctl enable phatsender
```