# raspberry-docker
This is a repo to document the steps set up &amp; run a variety of systems on one raspberry pi, that would need multiple pi's instead with custom OS. 

## The Goal
Have *one* Pi running at least one instance of home assistant within docker while having an OS, that can be used to install & run additional package like PiHole or similar. 
We want to achieve this without the usage of usb keyboard/mouse & external monitor as we do not like additional cables.

## The general setup
### Hardware
- PI 4b (8GB Ram) -> 4 should be enough as well
- an (micro) sd Card (min. 32GB) or SSD being at least of speed U3 or more ([what are speed classes?](https://www.kingston.com/en/blog/personal-storage/memory-card-speed-classes)) having a U1 might be enough as well but you might need to upgrade quite soon.
- opt.:
  - Ethernet cable
  - Heat sinks
  - Fan
  - PI case

### Software
#### On the PI itself
- Debian 11 (Bullseye)
- Docker

#### On your machine
- Raspberry Pi Imager


## The Steps we will take:
- Put together Hardware
- Set up PI
- Set up Docker for HA

### Hardware
- Install Heatsinks
- Install Fan
- Add into case
- Add power cable and opt. Ethernet if you do not plan on preconfiguring the Wifi (which I recommend though)

### Set up PI
- Insert Micro SD to your machine and make sure it is available
- Run Raspberry Pi Imager and select the following:
  - Debian 11 64 bit (lite if you want to go full headless, and full if you want to have the option to connect via VNC or an external monitor directly)
  - select the sd card as medium
  - use the settinga menu to adjust the initial configurations. Most importantly:
    - Set up SSH
    - Set up Wifi
    - Make sure you remember the credentials
- insert SD card to PI
- power on the PI
- Wait for about 2 minutes
- get the IP address of the PI (`arp -a | grep raspberry`)
- log in via SSH to the PI using your credentials `ssh <username>@<ip-adress>`
- update the packages using the following commands
  - `sudo apt update && sudo apt full-upgrade`
  - restart your PI using: `sudo reboot`
- Configure your PI to your pleasing using: `sudo raspi-config`

### Set up docker:
- Install docker: `curl -sSL https://get.docker.com | sh`
- add our user to root group to ditch sudo for docker compose: `sudo usermod -aG docker ${USER}`
- enable docker on system: `sudo systemctl enable docker`
- Create a dir structure like this:
```
- <username>@<ip>:~ $ tree -L 3
.
└── docker
    └── homeAssistant
        └── docker-compose.yml
```
(run: `mkdir docker && mkdir docker/homeAssistant && cd docker/homeAssistant && touch docker-compose.yml`)
- edit the docker-compose using nano or vim (install vim beforehand using `sudo apt install vim`) and add the following content:

```
version: '3'
services:
  homeassistant:
    image: "ghcr.io/home-assistant/home-assistant:stable"
    container_name: homeassistant
    network_mode: host
    privileged: true
    restart: unless-stopped
    volumes:
      - /home/pi/docker/homeAssistant/data:/config
      - /etc/localtime:/etc/localtime:ro
```
- start docker in the background: run `docker compose up -d`
- Access the home assistant on your local machine using the url `http://<PI-ip-adress>:8123`
read more about it [here](https://www.home-assistant.io/installation/raspberrypi/#docker-compose)
and [here](https://www.tim-kleyersburg.de/articles/home-assistant-with-docker-2023/)
note that the second link suggests using another image but I opted for the official doc in this case 


Optional stuff:

install vim
add aliases
Add proper terminal setup

Todos:
- [ ] Add docker folder structure etc to this repo so this can get cloned
- [ ] add GH ssh setup
