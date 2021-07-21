# raspberry-pi


<h1>Installation/OS Install</h1>

Download an OS eg: 

````
2018-06-27-raspbian-stretch-lite.zip
````

NOTE: No need to extract the .img - Etcher will read the .zip just fine


Flash using Etcher, use a USB->microSD Card reader 

https://github.com/balena-io/etcher

````
new-item -ItemType File -Name ssh -Path e:\ 
````


````
@"
country=AU
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
	ssid="GlobalEliteHackingCrew"
	psk="password"
	key_mgmt=WPA-PSK
}
"@
$config | out-file c:\temp\text.txt
```````

Copy files to BOOT 
````
ssh 

wpa_supplicant.conf 
````

ssh pi@<ipaddress>
password: raspberry
        
        
Set WiFi locale
````
        sudo raspi-config nonint do_wifi_country AU
````
	


Set hostname
	
````
sudo hostnamectl set-hostname my-raspberry-pi	
````
	, set timezone

````
Sudo raspi-config 
```` 

<h1>📷Camera📷</h1>
        
Enable camera

````
sudo raspi-config
````
	
	Test camera
	
	raspistill -o  image.jpg 
	
3, interface options
	

Update and upgrade

````
sudo apt-get update -y && sudo apt-get dist-upgrade -y
````
	
Install Git
	
````        
sudo apt-get install git -y
git clone https://github.com/silvanmelchior/RPi_Cam_Web_Interface.git 
sudo ./RPi_Cam_Web_Interface/install.sh
sudo reboot
````
	
WEB INTERFACE 

    Set motion detection to internal 

    Set Buffer to 4000 

 

Custom annotation 

````
%h:%m:%s %a 
````
 

Use ````%a````, this references ````/dev/shm/mjpeg/user_annotate.txt````

Use a cron job to update this file automatically (eg for day of week): 

````sudo crontab -e````

````
  * * * * * date +"\%a \%d \%b \%Y"  > /dev/shm/mjpeg/user_annotate.txt 
````
 

 

Change site name/page title 
````
sudo nano /var/www/html/config.php 
````
define('CAM_STRING',"CATCAM - BACKYARD"); 

 

 

Motion detection/Notifications 

 

 
	

sudo nano /var/www/html/macros/motion_event.sh 

 
	

sudo "#!/bin/bash" >>  /var/www/html/macros/motion_event.sh 

 
 
	

#!/bin/bash 

curl -u o.aaB9N2D4gBixznT7o7gM9FKm8FVzB9R7: https://api.pushbullet.com/v2/pushes -d type=note -d title="CATCAM - Motion Detected" -d body='Potential wild animal sighting!' 

 
	

sudo chown www-data:www-data /var/www/html/macros/motion_event.sh 

sudo chmod 764 /var/www/html/macros/motion_event.sh 

 

Add buffer  

sudo nano /etc/raspimjpeg 

video_buffer in ms 

 
FAVICON 



 

Tap solenoid 
20mm 

 
Disable camera LED

	I've disabled the main camera LED by adding disable_camera_led=1 to my /boot/config.txt, and that works fine.

 
	
<h2>Night Vision/IR camera</h2>

	
<h1>🚫Pi-Hole🚫</h1>

Disable IPv6 queries showing up 

sudo nano /etc/pihole/pihole-FTL.conf 

AAAA_QUERY_ANALYSIS=no 

 

 

Adlist location 

/etc/pihole/adlists.list 

 

Remove password from web console 

pihole -a -p 
	
	
<h1>🕹Retro-Pi</h1>
