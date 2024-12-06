Create a droplet
make an account on DigitalOcean
make a droplet with the following:- Ubuntu 24.04, Basic, Regular Intel CPU, normal SSD
choose either SSH key or a password

SSH into droplet
install docker and docker compose
refer to [Installing docker](Installing docker.md)


setup wireguard
run these commands:-`mkdir -p ~/wireguard/`
`mkdir -p ~/wireguard/config/`
`nano ~/wireguard/docker-compose.yml`
copy and paste the following:-
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
```
    

modify the following
TZ is timezone so change it to yours.
SERVERURL must be changed to the one on your DigitalOcean dashboard.
enter CTRL + X, Y, ENTER to save and exit.
start wireguard:- `cd ~/wireguard/
docker-compose up -d`
connect to phone:- `docker-compose logs -f wireguard`
open wireguard app on phone, click on +, create from QR code and scan QR code.
enter a tunnel name
before turning on your VPN you should check what your IP is
before turning it on you should see something like this
![Screenshot_20241126_142526_Samsung Internet](https://github.com/user-attachments/assets/a01a3acd-484c-4d05-a020-3ce3f2c14597)
after turning on the VPN you should see that your IP has changed to something like this
![Screenshot_20241126_142625_Samsung Internet](https://github.com/user-attachments/assets/96518e6c-f941-4232-85df-22ccd035d8b2)


install wireguard app onto PC
locate config files:- `~/wireguard/configs/{username}`
in wireguard app, select new tunnel and paste the config file from previous step.
before turning on your VPN you should check what your IP is
before turning it on you should see something like this
![Screenshot 2024-11-25 232841](https://github.com/user-attachments/assets/42e17f57-7d56-48be-8f63-604c4f2653c7)
after turning on the VPN you should see that your IP has changed to something like this
![Screenshot 2024-12-02 165423](https://github.com/user-attachments/assets/c5b46dce-ea07-4b8b-a8b4-5e401e55bd54)

