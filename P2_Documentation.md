# Pihole documentation
These are the commands to set up docker. Extra steps will be required to get it to work on ubuntu, namely disabling the dns stub listener
1) download docker w *sudo apt install docker-compose*
2) sudo systemctl start docker
3) sudo docker pull pihole/pihole
4) sudo nano /etc/resolve.conf _This is changed so that the DNS reroutes properly, change it to something like "DNS 8.8.8.8"_
5) sudo nano docker-compose.yml _Copy the code to this file_
6) *Fix stub listener if applicable*
7) sudo docker compose up -d
8) hostname -I
9) *Take the given IP address from the previous command, and insert it into https://IP:port(if was changed)/admin. Log in with the password from the command*
10) *Change your DNS so that way it routes through pihole*

While docker didn't want to cooperate, so there are no working pictures, here's a few partial progress photos. Instructions based on _https://pimylifeup.com/pi-hole-docker/_

#Docker Compose YML
services:
  pihole: 
    container_name: pihole
    image: pihole/pihole:latest
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "8080:80/tcp"
      - "443:443/tcp"
    environment:
      TZ: 'America/Chicago' #this is the time zone
      WEBPASSWORD: 'SECUREPASSWORD' _change this to more secure password_
    volumes:
       - './etc-pihole/:/etc/pihole/'
       - './etc-dnsmasq.d/:/etc/dnsmasq.d/'
    dns:
      - 127.0.0.1
      - 1.1.1.1
    cap_add:
      - NET_ADMIN
    restart: unless-stopped

  
