Welcome to my detailed instructions of how I completed my Cloud VPN-Docker Project using WireGuard.
( Commands will be shown in parenthesis format just like this. )

First, I  created a DigitalOcean.com account. 
I used my account to create an Ubuntu 20.04 Droplet.
Once, I created the droplet, I logged into the server and went into the terminal. 
I followed the instructions on https://thematrix.dev/setup-wireguard-vpn-server-with-docker/ 
where it tells you detailed instructions on how to install docker within your droplet.
In the instructions, it says to setup WireGuard by doing a couple of commands to make directories and then changing around some file information using nano.
( mkdir -p ~/wireguard/ )
( mkdir -p ~/wireguard/config/ )
( nano ~/wireguard/docker-compose.yml )
After these commands, it opened the compose file where I copied and pasted,
(
version: '3.8'
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
)
I changed my time zone from Hong Kong to Chicago because that is the closest place to me.
I changed my SERVERURL from 1.2.3.4 to my actual IP address which was 128.199.4.182.
Lastly, I changed PEERS=3 to allow 3 user config files.
I saved and exitted the cofig file and moved on.
Right here, I quickly realized that I did not install docker yet and I was on the wireGuard install section, so I switched to the docker install.
I am pretty sure I did not do anything to compromise the docker install, so I believe everything will be alright.
I started by going to the correct Docker Installation instructions using the link https://thematrix.dev/install-docker-and-docker-compose-on-ubuntu-20-04/.
Then, I installed the necesary tools for the install.
( sudo apt install apt-transport-https ca-certificates curl software-properties-common -y )
Next, I added the docker key with the command,
( curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - )
Then, I added the Correct Docker Repository which for me was the 32bit/64bit OS Repo.
( sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable" )
I switched to the correct repo with,
( apt-cache policy docker-ce )
Finally, I installed Docker with,
( sudo apt install docker-ce -y )
Next, I installed the Docker-Compose file with,
( sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose )
Lastly, I set the permissions using,
( sudo chmod +x /usr/local/bin/docker-compose )
Now, Docker was successfully installed, but before I continue where I left off, I am going to check if what I did previously with WireGuard was correct.
I checked the config file and everything is how I left it, so it should be fine.
I started Wireguard by first getting into the directory I just made and then running docker-compose.
( cd ~/wireguard/ )
( docker-compose up -d )
The next section of this installation is connecting to WireGuard using my phone.
I downloaded the app WireGuard in the app store and opened it.
Then, I entered the command,
( docker-compose logs -f wireguard )
which showed me the execution logs and QR Codes of the VPN connection settings.
I went into my phone and first took a screenshot of my IP address on ipleak.net.

Then I turned on the VPN and did the same thing which showed that my ip address changed.

Next, I had to run the VPN on my computer.
I started by downloading wireguard and finding the config file I needed to connect.
I found the file by using the command,
( cat ~/wireguard/config/peer2/peer2.conf )
I copied and pasted the contents and then plugged that into the tunnel, which successfully gave me the VPN connection.

<img src="https://user-images.githubusercontent.com/29709211/144539352-4f3ef034-fa96-43a6-9246-918c2a38f424.PNG" width="300" height="500"> |<img src="https://user-images.githubusercontent.com/29709211/144539391-d8a7688d-7d94-4799-9a5c-3dd6a3b3ff68.PNG" width="300" height="500">|<img src="https://user-images.githubusercontent.com/29709211/144539462-c25687fb-6baf-40a8-aee5-1a6ff6b58120.png" width="500" height="500">

