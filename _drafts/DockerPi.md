Been playing with Docker on a spare Pi for a while, currently it runs Portainer, PiHole and a NGINX web server hosting a basic HTML page with links to all the services running on the network (so I don't have to remember all their IP addresses!). 

Inspired by [this](https://youtu.be/a6mjt8tWUws) youtube video I decided to see if I could rebuild all the services I run onto a single Raspberry Pi 4, that is also my primary media centre. 

Even though the setup I have is already working, there are some issues with is. For example all the containers were set up manually, so I'd like to get to grips with [Docker Compose](https://docs.docker.com/compose/) so I can automate the builds should I need to update any of these contaners. Also, while the config data is stored locally on the Pi, not in the containers, its not currenlty backed up anywhere and the above video uses a simple Dropbox script to backup the config data so the whole lot can be rebuilt quite easily, or at least in theory. 

This is a list of the bits I think I'll need to get fully set up. I have a normal installation of NodeRed running on another Pi which I plan to migrate to the container once this is set up and running, and I have a dedicated Pi running a SSH gateway, which I plan to replace with the PiVPN. 

Containers:
[x] Portainer 
[x] PiHole
[x] NGINX (Web)
[x] NodeRed 
[ ] Home Assistant 
[ ] PiVPN 
[ ] Dillinger (Markdown Editor)
[ ] Home Control (Heatmiser)
[ ] Motion Eye 
[ ] Dropbox Backup 

I'll keep this post updated as I progress! 

While its great to have a tool like this to help configure and set up a stack of Docker applications on a Raspberry Pi, I thought I'd have a go at building it all myself. I started with, possibly the most complicated, PiHole. Originally I'd followed [this](https://homenetworkguy.com/how-to/install-pihole-on-raspberry-pi-with-docker-and-portainer/) guide to set up Portainer and Pihole, however, it required a lot of manual configuration in the GUI to get Pihole up and running (with not a small amount of troubleshooting!). Seeing as the whole objective of this is to learn Docker Compose I dived into the [Pihole Docker repository](https://hub.docker.com/r/pihole/pihole) and used the Docker Compose script that they suggested. After a couple of hours trying to get it to work, I eventualy gave up and added network_mode: host to the Pihole config, just before the ports specification. This makes the container run as if its using the same IP as the host container. While not the best way, it was the only way I could get the Pihole DHCP server running without messing about with reverse proxies. 


Nginx 
https://hub.docker.com/_/nginx Docker Imaange and documentation 
https://dev.to/aminnairi/quick-web-server-with-nginx-on-docker-compose-43ol docker compose example 
Portainer 
https://portainer.readthedocs.io/en/stable/deployment.html docker compose example (changed to version '3')
https://portainer.readthedocs.io/en/stable/deployment.html - from here 

Now that I had all the services I'd previously set up manually running via docker compose the next step was to move the volumes I'd mapped to a cenral location so I could use the Dropbox backup script I was going to install later. 

Nodered 
https://nodered.org/docs/getting-started/docker (docker compose example) 
copied the .nodered folder from my existing pi installation to my data folder on the docker Pi and mapped that folder in the above docker compose file. 

Had to remember to move all the custome python scripts I use for my weird thermostat to the data directory then chnage the path in the individual nodes. Time consuming, but worth it. 

Funnily enough as I moved my nodered instance to a new IP I had to update the Nginx site above to reflect the new link, this worked flawlessly! 


OpenVPN 
https://hub.docker.com/r/linuxserver/openvpn-as 
https://docs.linuxserver.io/images/docker-openvpn-as




