Create a droplet
make an account on DigitalOcean
make a droplet with the following:- Ubuntu 24.04, Basic, Regular Intel CPU, normal SSD
choose either SSH key or a password

SSH into droplet
install docker and docker compose
refer to [[Installing docker]]


setup wireguard
run these commands:-`mkdir -p ~/wireguard/
`mkdir -p ~/wireguard/config/`
`nano ~/wireguard/docker-compose.yml`
copy and paste the following:- ```
```version: '3.8'
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
    
````
modify the following
TZ is timezone so change it to yours.
SERVERURL must be changed to the one on your DigitalOcean dashboard.
enter CTRL + X, Y, ENTER to save and exit.
start wireguard:- `cd ~/wireguard/
`docker-compose up -d
connect to phone:- `docker-compose logs -f wireguard`
open wireguard app on phone, click on +, create from QR code and scan QR code.
enter a tunnel name

install wireguard app onto PC
locate config files:- `~/wireguard/configs/{username}`

error handling
make directory:- `mkdir -p ~/wireguard/configs/pc1`
create configuration file:- `touch ~/wireguard/configs/pc1.conf`

