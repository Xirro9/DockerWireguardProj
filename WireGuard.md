First I made a digital ocean account from the link provided and got $200 in credit. 

I made an Ubuntu 20.0.4 droplet and gave it a password.

I launched the console for the droplet and installed docker.

To install docker I literally copied and pasted all the commands from the provided guide: https://thematrix.dev/install-docker-and-docker-compose-on-ubuntu-20-04/ 

for simplicity I will just list all the commands I used:

>sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

>curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

>sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
>apt-cache policy docker-ce

>sudo apt install docker-ce -y

>sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

>sudo chmod +x /usr/local/bin/docker-compose

that is docker installed

Now to install wireguard

>mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
nano ~/wireguard/docker-compose.yml

inside of the yml I pasted this:
>version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Asia/Hong_Kong
      - SERVERURL=1.2.3.4
      - SERVERPORT=51820
      - PEERS=pc1,pc2,phone1
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
      
For this to work I had to chang the TZ(time zone) to my current time zone. 
and I had to change SERVERURL to my droplets ipv4 address.
    
then I ran:
>cd ~/wireguard/
docker-compose up -d

this installed wireguard, now to get the config files to my pc and phone

I installed the apps on my phone and PC, I ran: 
>docker-compose logs -f wireguard

and got a qr code for my phone

I made a new tunnel, naming it lab3, on my phone and went to ipleak.net

I took a screenshot before turning it on, saying I was in oklahoma, then after I turtned it on, it said I was in New Jersy.

I then had to figure out how to get the conf file from the droplet vm to my machine so the desktop app could work

>cd config
>cd peer_PC1

this led me to the peer 1 config file, I nanoed in

>nano peer_PC1

giving me the config file
I copied the file contents into a text document on my pc and named the text file peer_PC1.conf

I selected that file in the wireguard app and it allowed me to activate the vpn.

I then took a screenshot of ipleak.net before and after turning on wireguard.

This concludes the wireguard project for SysAdmin.
