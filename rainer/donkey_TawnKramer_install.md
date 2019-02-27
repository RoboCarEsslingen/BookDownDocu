# Installation

following Tawn Kramer
https://github.com/tawnkramer/donkey/blob/master/docs/guide/install_software.md

## 0. Environment
```
1. conda create --name donkey_3tk4 tensorflow-gpu==1.10.0
2. source activate donkey_3tk4
3. git clone https://github.com/tawnkramer/donkey.git
4. pip install -e ./donkey
5. conda install -c conda-forge keras==2.2.2

activate: source activate donkey_3tk4
get installed software: conda env export > environment.yml 
```
content of [environment.yml](https://github.com/Heavy02011/50-donkey/blob/master/environment.yml)

## 1. sudo raspi-config 

```
a. generate DE locales 
b. set german keyboard layout 
c. enable i2c, spi 
d. expand SDcard 
e. set hostname = siliconpi2 
``` 

## 2. etc/wpa_supllicant/wpa_supplicant.conf 

```
a. setup SSID & password 
b. (env) pi@siliconpi2:/etc/wpa_supplicant $ sudo more wpa_supplicant.conf 

country=US 
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev 
update_config=1 

network={ 
    ssid="XXXXXXXXXXX" 
    psk="dontstorepasswords" 
    id_str="home" 
} 

network={ 
    ssid="shack" 
    psk="xxxx" 
    id_str="shack" 
} 
```
 
## 3. installs 

```
a. sudo apt-get install vim 
b. sudo apt-get upgrade 
c. sudo apt-get update 
d. sudo rpi-update 
```

## 4. install donkey on pi 

follow: https://github.com/tawnkramer/donkey/blob/master/docs/guide/install_software.md 


## 5. config.piy 

```
#STEERING 
STEERING_CHANNEL = 1 
STEERING_LEFT_PWM = 480 #500 #420 
STEERING_RIGHT_PWM = 290 #360 

#THROTTLE 
THROTTLE_CHANNEL = 0 
THROTTLE_FORWARD_PWM = 500 #400 
THROTTLE_STOPPED_PWM = 360 
THROTTLE_REVERSE_PWM = 310 
```


## 7. bluetooth 
```
sudo bluetoothctl  
agent on 
power on 
scan on 

connect <Mac> 
trust <Mac> 
```
 

## 8. wii u controller 

setup joystick: https://github.com/tawnkramer/donkey/blob/master/docs/parts/controllers.md#physical-joystick-controller 
 

## 9. autostart donkeycar 

https://gist.github.com/r7vme/9159c52ec72660d8ace02793a5cee788 

```
# Allows to automatically start donkey car on boot.
#
# 1. Put contents to /etc/systemd/system/donkey.service
# 2. sudo systemctl daemon-reload
# 3. sudo systemctl start donkey
# 4. sudo journactl -u donkey
#
# TIP: Grab logs via ssh with 
# 
# ssh pi@d2.local "sudo journalctl -u donkey -f"
#
[Unit]
Description=Donkey car 

[Service]
Restart=always
ExecStart=/bin/su - pi bash -c "python -u d2/manage.py drive --model /home/pi/d2/models/mypilot"

[Install]
WantedBy=multi-user.target
```

## 10. Steering Calibration
```
Make sure your car is off the ground to prevent a runaway situation.
1. Turn on your car.
2. Find the servo cable on your car and see what channel it's plugged into the PCA board. It should be 1 or 0.
3. Run donkey calibrate --channel <your_steering_channel>
4. Enter 360 and you should see the wheels on your car move slightly. If not enter 400 or 300.
5. Next enter values +/- 10 from your starting value to find the PWM setting that makes your car turn all the way left and all the way right. Remember these values.
6. Enter these values in config.py script as STEERING_RIGHT_PWM and STEERING_LEFT_PWM.

LEFT =500
RIGHT = 290
```

## 11. drive 

```
siliconpi2:
cd d2
python manage.py drive 
```
