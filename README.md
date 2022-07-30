![image](https://user-images.githubusercontent.com/38451588/126574688-051a6b6a-df2e-494d-9aef-51e4c6470857.png)


<h1>Installation/OS Install (Windows) </h1>

Download an OS from https://www.raspberrypi.org/software/operating-systems/#raspberry-pi-os-32-bit

````
2018-06-27-raspbian-stretch-lite.zip
````

NOTE: No need to extract the .img - Etcher will read the .zip just fine

Flash using Etcher, use a USB->microSD Card reader:

https://github.com/balena-io/etcher

### Enable SSH (assumes BOOT volume is E:\ drive)
````powershell
new-item -ItemType File -Name ssh -Path e:\ 
````
### Auto-join WiFi (update ssid and psk) (assumes BOOT volume is E:\ drive

````powershell
$config = @"
country=AU
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
	ssid="GlobalEliteHackingCrew"
	psk="password"
	key_mgmt=WPA-PSK
}
"@
$config | out-file E:\wpa_supplicant.conf
```````
OS install done

---

Fire up the pi 🚀

Find the pi on the network - use Fing for Android/iOS

---

```
ssh pi@ipaddress
```
password: raspberry
        
        
### Set WiFi locale
````
sudo raspi-config nonint do_wifi_country AU
````
	

### Set hostname
	
````bash
sudo hostnamectl set-hostname my-raspberry-pi	
````
### Set timezone

````bash
Sudo raspi-config 
```` 

# 📷Camera📷
        
### Enable camera

````
sudo raspi-config
````
	
	Test camera
	
	raspistill -o  image.jpg 
	
3, interface options
	

- Update and upgrade

````
sudo apt-get update -y && sudo apt-get dist-upgrade -y
````
	
- Install Git
	
````        
sudo apt-get install git -y
git clone https://github.com/silvanmelchior/RPi_Cam_Web_Interface.git 
sudo ./RPi_Cam_Web_Interface/install.sh
sudo reboot
````

- Documentation

http://elinux.org/RPi-Cam-Web-Interface

- WEB INTERFACE 

Camera > Motion Detection: internal

Camera > Buffer: 4000

Camera > Preview Quality: 15 



Quality is the jpeg compression factor (0-100). Lower numbers give better compression at the expense of quality. It is set to 25 by default which gives pretty good compression and not too much degradation. You can try lowering it to say 15 which will significantly lower sizes further but you will start to see some degradation. Lower than this it will start to get bad.

Divider is the rate at which new data is fetched. It is the video fps (default 25) divided by 'Divider'. Nominally that would mean it is trying to update at 25fps but in practice other delays lower this a bit. Increasing the divider lowers the fetch frame rate so 3 would give a nominal rate of 8fps. This will also have a dramatic effect on bandwidth. 

- Custom annotation 

![image](https://user-images.githubusercontent.com/38451588/126848389-ea5cd216-89a9-4d28-815e-20a0fa21dafa.png)

````
%h:%m:%s %a 
````
 
Use ````%a````, this references ````/dev/shm/mjpeg/user_annotate.txt````

Use a cron job to update this file automatically (eg for day of week): 

````sudo crontab -e````

````
  * * * * * date +"\%a \%d \%b \%Y"  > /dev/shm/mjpeg/user_annotate.txt 
````
 
### Change site name/page title

![image](https://user-images.githubusercontent.com/38451588/126848437-fd06864b-52dd-419b-8ab5-27a60972fd2a.png)


````
sudo nano /var/www/html/config.php 
````
````define('CAM_STRING',"pi-camera"); ````


### Motion detection/Notifications 

 
````
sudo nano /var/www/html/macros/motion_event.sh 
````
 
````
sudo "#!/bin/bash" >>  /var/www/html/macros/motion_event.sh 
````
 
Replace Xs with your pushbullet API key
	
````
#!/bin/bash 
curl -u XXXXXXXXXXXXXXXXXXXXXXXXX: https://api.pushbullet.com/v2/pushes -d type=note -d title="ALERT" -d body='Motion detected!' 
`````

````
sudo chown www-data:www-data /var/www/html/macros/motion_event.sh 

sudo chmod 764 /var/www/html/macros/motion_event.sh 
````
 

Add buffer  
````
sudo nano /etc/raspimjpeg 
````
video_buffer in ms 

 
### Add FAVICON 

- Upload a favicon.ico to HOME

````
sudo cp ~/favicon.ico /var/www/
````

### Disable camera LED

````disable_camera_led=1 >> /boot/config.txt````


### Dark/night mode GUI

System > Style > Night > OK


## Night Vision/IR camera ##

---
	
<h1>🚫Pi-Hole🚫</h1>

### Disable IPv6 queries showing up 
````
sudo nano /etc/pihole/pihole-FTL.conf 
````
AAAA_QUERY_ANALYSIS=no 

### Disable PTR records showing up

````
ANALYZE_ONLY_A_AND_AAAA=true
````

### Add multiple domains to blacklist

![image](https://user-images.githubusercontent.com/38451588/149241259-e91e49be-581c-4f35-9a1c-cc9d2e50b083.png)


### Adlist location 

````
/etc/pihole/adlists.list 
````

### Remove password from web console 

````
pihole -a -p
````
	

### Install pihole 
````
curl -sSL https://install.pi-hole.net | bash 
````
 
### Tail pihole log 
````
tail -f /var/log/pihole.log 
````

### Tail pihole log (show only blocked requests) 
````
tail -f /var/log/pihole.log | grep 0.0.0.0 
````

*improved version: 
````
tail -f /var/log/pihole.log | cut -c 31-100 | grep 0.0.0.0 
````
- Show last entries added to blocklist 
````
tail /etc/pihole/black.list 
````

### Remove background image 
````
sudo mv /var/www/html/admin/img/boxed-bg-dark.png /var/www/html/admin/img/boxed-bg-dark.png2
````

### Adjust HOSTS/DNS names 
````
sudo nano /etc/hosts 
````
### Top 50 blocked domains (past day)
```sh
sqlite3 /etc/pihole/pihole-FTL.db "SELECT domain FROM queries WHERE  timestamp>='$(($(date +%s) - 86400))'" | sort | uniq -c | sort -n -r | head -50
```


### Show clients per IP
```` 
sqlite3 /etc/pihole/pihole-FTL.db "SELECT domain FROM queries WHERE client='192.168.1.12' AND timestamp>='$(($(date +%s) - 86400))'" | sort | uniq -c | sort -n -r | head -10
````

### Modify default recent queries (instead of just 10 by default) 
````sudo nano /var/www/html/admin/scripts/pi-hole/js/queries.js ````

First value is the default the page loads with.  Modify both the first and the 2nd array (eg 60):

![image](https://user-images.githubusercontent.com/38451588/127440901-5a2f37d0-3b60-41c0-87ea-9a72e03d451c.png)
 
### Setup exclusions for specific devices
Happy wife, happy life


# 🕹Retro-Pi #
````
sudo update-locale LC_ALL="en_GB.UTF8" 
sudo update-locale LANGUAGE="en_GB:en" 
sudo apt-get install git lsb-release -y 
sudo apt-get update 
sudo apt-get -f install 
sudo apt-get dist-upgrade 
git clone --depth=1 https://github.com/RetroPie/RetroPie-Setup.git 

cd RetroPie-Setup 
chmod +x retropie_setup.sh 
sudo ./retropie_setup.sh 
````
Download the img 


Stretch the aspect ratio 

 

# 🔌Pinout/GPIO 🔌

### Zero
![image](https://user-images.githubusercontent.com/38451588/128586000-983690dc-aba4-41ef-a676-669e12613928.png)

### 2B
![image](https://user-images.githubusercontent.com/38451588/128586020-bae1d6be-99b6-427c-a07b-d72a80d02570.png)


# 💡 LEDs 💡



# 🌡 Temperature Sensor 🌡


### DSB18B20
- comes in transistor or waterproof probe form
- is a digital probe
- -55°C to 125°C range
- 3.0V to 5.0V operating voltage

## Enable 1-Wire sensor

````
sudo raspi-config
````


#### Wiring

![image](https://user-images.githubusercontent.com/38451588/127440391-a07df571-a7e8-408d-b965-bd270ae9b1a0.png)


![image](https://user-images.githubusercontent.com/38451588/128584637-6f2e95ba-4ad6-4130-a032-451c5bb3ce11.png)


DS18B20 Wiring: Red = VCC, Yellow = Data, Black = GND

#### Temp readout
````
cat /sys/bus/w1/devices/28*/w1_slave | grep t | cut -d= -f2
````

## Stepper motor


https://www.rototron.info/raspberry-pi-stepper-motor-tutorial/

https://thinkingofpi.com/getting-started/raspberry-pi-stepper-motor/


## Servo

![image](https://user-images.githubusercontent.com/38451588/145665441-4514152d-7f88-43de-b93b-58b291b3a66d.png)


![image](https://user-images.githubusercontent.com/38451588/145664477-f506e719-35d2-4a39-9622-b8cdead1684e.png)


![image](https://user-images.githubusercontent.com/38451588/145664624-7e92ec48-ce2f-42b3-9ac7-af17bef66fa6.png)



### Anti-Clockwise
````python
import RPi.GPIO as GPIO
import time

#print("doing stuff")

servoPIN = 17
GPIO.setmode(GPIO.BCM)
GPIO.setup(servoPIN, GPIO.OUT)

p = GPIO.PWM(servoPIN, 50) # GPIO 17 for PWM with 50Hz

p.start(2.5) # clockwise
time.sleep(1) # run for x seconds
p.stop()
````


sudo apt install python3-gpiozero -y

