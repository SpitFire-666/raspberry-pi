# raspberry-pi


<h1>Installation/OS Install</h1>
Windows 


 

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
	


Set hostname, set timezone

````
Sudo raspi-config 
```` 

<h1>📷Camera📷</h1>
        
Enable camera

````
sudo raspi-config
```
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
````
<h1>🚫Pi-Hole🚫</h1>


<h1>🕹Retro-Pi</h1>
