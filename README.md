<h1 align="center"> Learnt from [Rues](https://github.com/ruesandora/waku) </h1>


<h1 align="center"> Waku </h1>

> How long will it last? No information - Is it rewarding? Not clear, it might be. I'm not expecting a guaranteed outcome, it's just a small hardware.

> Why am I setting it up? The project is worth joining, it didn't take much of my time, it installs in a few minutes.

> Here is the Rues Community:
> Community channels: [Twitter](https://twitter.com/Ruesandora0) - [Announcement](https://t.me/RuesAnnouncement) - [Chat](https://t.me/RuesChat) - [WP](https://whatsapp.com/channel/0029VaBcj7V1dAw1H2KhMk34) - [For Node questions](https://t.me/ruesshare/13003/13004) - [Waku Discord](https://discord.gg/4DBrFfyY)

<h1 align="center"> Hardware </h1>

Requirements:
```
2 CPU 2 RAM - 40 SSD
```

<h1 align="center"> Installation </h1>

```console
# Server update and necessary packages
sudo apt update && sudo apt upgrade -y
sudo apt-get install build-essential git libpq5 jq -y

# choose option 1 after entering the code.
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source "$HOME/.cargo/env"

sudo apt install docker.io -y
sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

```console
# Waku installation

# cloning waku
git clone https://github.com/waku-org/nwaku-compose
cd nwaku-compose
cp .env.example .env

# accessing .env file
nano .env
```

> Here are the parts we need to change:

* `ETH_CLIENT_ADDRESS = I got Sepolia RPC from Infura for free - https://www.infura.io/
* `ETH_TESTNET_KEY = I added the private key of the metamask account I opened for Waku - In the Metamask account information section
* `RLN_RELAY_CRED_PASSWORD = I set a password

> Save and exit with CTRL X Y ENTER.

```console
# and let's register.
./register_rln.sh
# Before registering, you must have Sepolia eth in your wallet
```

<h1 align="center"> Starting Waku node </h1>

```console
# opening ports

# say yes after entering this command
sudo ufw enable

# you can enter these port commands collectively.
sudo ufw allow 22    
sudo ufw allow 3000   
sudo ufw allow 8545   
sudo ufw allow 8645   
sudo ufw allow 9005   
sudo ufw allow 30304  
sudo ufw allow 8645

# docker up
docker-compose up -d

# you can check with these commands
docker-compose ps
docker-compose logs nwaku
```

```console
# let's enter it with this command:
nano ~/nwaku-compose/docker-compose.yml
```
> After entering, type ctrl w and search for 3000:3000.

> `Change 127.0.0.1:3000:3000 to 0.0.0.0:3000:3000.

> Save and exit with CTRL X Y ENTER.

```console
# let's restart again
docker-compose down
docker-compose up -d
```

> Your data will be updated within about 1 hour.

> 'http://YOUR_IP_ADDRESS:3000/d/yns_4vFVk/nwaku-monitoring

> Replace YOUR_IP_ADDRESS and search it on Google.

> Save the `keystore.json`  file for backup.

```console
# If you get output with these two commands, everything is okay.
curl --location 'http://127.0.0.1:8645/debug/v1/version'
curl --location 'http://127.0.0.1:8645/debug/v1/info'
```

<h1 align="center"> Update </h1>

First, go to the `nwaku-compose` repository we downloaded and stop the running containers.


```console
cd ~/nwaku-compose
docker-compose down
```

Pull the latest version of the repository (Assuming the update is on the main branch. They updated from version 0.25.0 to 0.26.0 on the main branch.)
```console
git pull
```

Bring Docker back up.
```console
docker-compose up -d
```

Don't forget to check the logs and Grafana. If you try to shut down and restart after updating, you encounter an error. You need to manually enter the `postgres:15.4-alpine3.18` container and delete the  `public.messages_backup` table. You can find more about this issue [here](https://github.com/waku-org/nwaku-compose/issues/75).



