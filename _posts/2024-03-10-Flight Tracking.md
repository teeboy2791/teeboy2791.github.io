---
title: Flight Tracking
date: 2024-03-10 12:00:00 +0000
categories: [geek, web]
tags: [web, n100, docker, ultrafeed, piaware, flightradar]     # TAG names should always be lowercase
---

## Home Server

I have purchased a N100 Mini PC with the intention of setting up as a Home Server which is always running. 

![MiniPC]({{ site.url }}\assets\img\Firebat.png)

It will run: 

- Ubuntu as the base OS
- Docker to host the applications
	- Homeassistant
	- Ultrafeeder to collect ADS-B data from local aircraft
	- piaware to send flight data to FlightAware
	- fr24 to send flight data to FlightRadar24

![HomeServer]({{ site.url }}\assets\img\N100.png)

Plugged into the server is an SDR USB dongle, which I have installed in the loft in my house:

![USB Dongle]({{ site.url }}\assets\img\SDR_Dongle.jpg)


## Install Ubuntu

- Download: ubuntu-22.04.3-live-server-amd64
- rufus-4.4 to create bootable USB from dowloaded file
- DEL to enter BIOS
- Boot from USB Drive 

- Install Ubuntu Server

- Language: English (UK)
- Continue without updating Installer
- Keyboard: English (UK)

- Select (x) Ubuntu Server

- Ensure NIC is connected or WIFI is configured

- Configure Disk - delete all partitions and let Ubuntu create its own

- Create a User

> 
	Your Name: *Yourname*
	Server Name: *servername*
	Username: *setup_a_user*
	Password: *set_a_secure_password*

- Dont enable Ubuntu Pro

- Install OpenSSH Server (allow password auth)

- Don't select any Server Snaps

## Configure Network

My N100 PC is configured with two network cards. Since this will always be running I decided to setup those two NICs as a bridge, so my desktop PC can cascade off the N100:

- /etc/netplan = interface config

- edit 00-installer-config.yaml

>
	bridges:
		br0:
			dhcp4: true
			interfaces:
				- enp1s0
				- enp3s0

- netplan apply to apply changes


- When docker is installed bridge will fail due to iptables rules so fix this issue by adding the following line to /usr/lib/systemd/system/docker.service:

>
	[Service]
	ExecStartPost=iptables -I DOCKER-USER -i br0 -o br0 -j ACCEPT


-For local name resolution install avahi:
-	apt install avahi-daemon

Will make server respond on *servername*.local

## Install Docker and Flight Tracking Software

To install the flight tracking docker images I followed this guide:

> <https://sdr-enthusiasts.gitbook.io/ads-b/> 

You can see my stats on the Flightaware site:

> <https://www.flightaware.com/adsb/stats/user/tonygodson#stats-220830>

As I live on the top of a hill I get pretty good coverage - upto around 200 Nautical Miles on most days:

![Range Map]({{ site.url }}\assets\img\RangeMap.png)

The spikey outline shows my typical range in all directions.




## Install Home Assistant

As the MiniPC will be running 24x7 it makes sense to also have HomeAssistant running to support my Hue, Ring and Sonos devices:
> <https://www.home-assistant.io/installation/>

Specifically I installed the docker container version:
> <https://www.home-assistant.io/installation/linux#install-home-assistant-container>

There are some limitations to running HomeAssitant within a container:

- Supervisor is not supported
- Addons are not supported
- Managed OS and Managed Restore are not supported
	
These limitations can normally be worked around using other docker containers.
	









