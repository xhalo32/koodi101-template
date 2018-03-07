# Koodi101 Project Template

## Frontend & Backend

### How to run backend and frontend using docker-compose?

```shell
docker-compose build
docker-compose up

Hit Ctrl+C to stop the process
```

What if I want to run the project in the background?

```shell
docker-compose build
docker-compose up -d
```

**DO NOTE:** when you run the project with docker-compose on your own server, you need to do the following

```shell
export ENDPOINT='http://195.201.28.137:9000'
docker-compose build
docker-compose up -d
```

Replace 195.201.28.137 with the ip address of your team's server.

Nice, how can I now see if the project is already running?

```shell
docker-compose ps
```

Cool, how can I stop the project from running and start from scratch

```shell
docker-compose down --rmi all --remove-orphans
```

This also deletes the built docker images, if you do not want this, you can also just run

```shell
docker-compose down
````

### How to run the backend and frontend without Docker?

Prerequisites
* [nodejs](http://nodejs.org)

#### Backend

```shell
    cd backend
    npm install
    npm run dev
```

* The backend is now running in [http://localhost:9000](http://localhost:9000/api/greeting)

#### Frontend

```shell
    cd frontend
    npm install
    npm start
```

* The frontend is now running in [http://localhost:8000](http://localhost:8000)


## Internet of Things (IoT)

### Raspberry pi – first time setup
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

### Run the project
Let's update our system and install some needed libraries.
```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install python3-requests python3-envirophat git
```

To test if our application works, you can create a "request bin" for
testing purposes in [requestbin](https://requestb.in).
You will get a bucket with url similiar to *https://requestb.in/v6lrggv6*

First clone your forked version of this repository to the Rasperry pi and go to the iot folder
```
git clone <url>
```

You can now try our app by starting it with
```
ENDPOINT=https://requestb.in/v6lrggv6 python3 phat.py
```

Your requests should appear into your browser after you refresh it.

### Starting the app automatically

One way to keep raspberry sending information without manual
work, is to use cron to run our script every minute.

That can be achieved by opening crontab editor by typing ```crontab -e```

Window will open and you can append the following line at end of the file,
of course replacing the endpoint with a correct one.
```
* * * * * ENDPOINT=https://requestb.in/1ljyjs61 python3 /home/pi/koodi101-template/iot/phat.py >> /home/pi/phat.log 2>&1
```
So what does this mean?
* **\* \* \* \* \*** ENDPOINT=https://requestb.in/1ljyjs61 python3 /home/pi/koodi101-template/iot/phat.py >> /home/pi/phat.log 2>&1
    * When to run the script, *see below*
* \* \* \* \* \* **ENDPOINT=https://requestb.in/1ljyjs61** python3 /home/pi/koodi101-template/iot/phat.py >> /home/pi/phat.log 2>&1
    * Pass environmental variable for script to be executed
* \* \* \* \* \* ENDPOINT=https://requestb.in/1ljyjs61 **python3 /home/pi/koodi101-template/iot/phat.py** >> /home/pi/phat.log 2>&1
    * Normal command to run a script with python
* \* \* \* \* \* ENDPOINT=https://requestb.in/1ljyjs61 python3 /home/pi/koodi101-template/iot/phat.py **>> /home/pi/phat.log** 2>&1
    * **Append** output to a file
* \* \* \* \* \* ENDPOINT=https://requestb.in/1ljyjs61 python3 /home/pi/koodi101-template/iot/phat.py >> /home/pi/phat.log **2>&1**
    * By default, error messages wouldn't be logged into the file.
      With this definition, we redirect the to standard output and
      therefore into the same file.

Cron works by comparing current time to parameters at the beginning of every line.
If it matches, it will run the script.

Our example is run every minute, although
it should match at anytime, why is it so? Well, cron runs only every minute, so
that is why this will work and that is why we can't schedule it to run more often.

There exist cool tool to "translate" cron time entries into human readable form:
[https://crontab.guru](https://crontab.guru)
