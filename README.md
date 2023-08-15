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
- Debian 11 (Bullseye) as OS on the PI
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
  - `sudo apt-get update`
  - `sudo apt-get upgrade` (Choose `Y` when prompted)
  - restart your PI using: `sudo shutdown -r now`
- Configure your PI to your pleasing using: `sudo raspi-config`
