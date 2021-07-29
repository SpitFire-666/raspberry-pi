![image](https://user-images.githubusercontent.com/38451588/126574688-051a6b6a-df2e-494d-9aef-51e4c6470857.png)


<h1>Installation/OS Install (Windows) </h1>

Download an OS from https://www.raspberrypi.org/software/operating-systems/#raspberry-pi-os-32-bit

````
2018-06-27-raspbian-stretch-lite.zip
````

NOTE: No need to extract the .img - Etcher will read the .zip just fine


Flash using Etcher, use a USB->microSD Card reader:

https://github.com/balena-io/etcher

- Enable SSH (assumes BOOT volume is E:\ drive)
````powershell
new-item -ItemType File -Name ssh -Path e:\ 
````
- Auto-join WiFi (update ssid and psk) (assumes BOOT volume is E:\ drive

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
        
        
- Set WiFi locale
````
sudo raspi-config nonint do_wifi_country AU
````
	

- Set hostname
	
````bash
sudo hostnamectl set-hostname my-raspberry-pi	
````
- Set timezone

````bash
Sudo raspi-config 
```` 

<h1>📷Camera📷</h1>
        
- Enable camera

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

    Set motion detection to internal 

    Set Buffer to 4000 

 - Preview Quality

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
 
- Change site name/page title

![image](https://user-images.githubusercontent.com/38451588/126848437-fd06864b-52dd-419b-8ab5-27a60972fd2a.png)


````
sudo nano /var/www/html/config.php 
````
````define('CAM_STRING',"pi-camera"); ````


- Motion detection/Notifications 

 
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

sudo nano /etc/raspimjpeg 

video_buffer in ms 

 
- Add FAVICON 


- Disable camera LED

````disable_camera_led=1 >> /boot/config.txt````


- Dark/night mode GUI

System > Style > Night > OK


## Night Vision/IR camera ##

---
	
<h1>🚫Pi-Hole🚫</h1>

- Disable IPv6 queries showing up 
````
sudo nano /etc/pihole/pihole-FTL.conf 
````
AAAA_QUERY_ANALYSIS=no 

 
- Adlist location 

````/etc/pihole/adlists.list ````

- Remove password from web console 

````pihole -a -p````
	

- Install pihole 
````
curl -sSL https://install.pi-hole.net | bash 
````
 
- Tail pihole log 
````
tail -f /var/log/pihole.log 
````

- Tail pihole log (show only blocked requests) 
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

- Remove background image 
````
sudo mv /var/www/html/admin/img/boxed-bg-dark.jpg /var/www/html/admin/img/boxed-bg-dark.jpg2 
````

- Adjust HOSTS/DNS names 
````
sudo nano /etc/hosts 
````
- Show clients per IP
```` 
sqlite3 /etc/pihole/pihole-FTL.db "SELECT domain FROM queries WHERE client='192.168.1.12' AND timestamp>='$(($(date +%s) - 86400))'" | sort | uniq -c | sort -n -r | head -10
````

- Modify default recent queries (instead of just 10 by default) 
````sudo nano /var/www/html/admin/scripts/pi-hole/js/queries.js ````

First value is the default the page loads with.  Modify both the first and the 2nd array (eg 60):

![image](https://user-images.githubusercontent.com/38451588/127440901-5a2f37d0-3b60-41c0-87ea-9a72e03d451c.png)
 


# 🕹Retro-Pi #



# 🔌Pinout/GPIO 🔌


# 💡 LEDs 💡



# 🌡 Temperature Sensor 🌡

DSB18B20 - comes in transistor or probe form

![image](https://user-images.githubusercontent.com/38451588/127440391-a07df571-a7e8-408d-b965-bd270ae9b1a0.png)
