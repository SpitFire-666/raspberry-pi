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

 
Login as pi, password raspberry 

 


Set hostname 

Enable ssh 
````
Sudo raspi-config 
```` 

<h1>📷Camera📷</h1>

<h1>🚫Pi-Hole🚫</h1>


<h1>🕹Retro-Pi</h1>
