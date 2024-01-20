# README

## Initial Setup

```
ssh-keygen -f ./game-server -P "" -C ""
terraform apply
ssh -i ./game-server ubuntu@<ec2-address>
sudo add-apt-repository multiverse; sudo dpkg --add-architecture i386; sudo apt update
sudo apt install steamcmd
sudo useradd -m steam
sudo passwd steam
sudo -u steam -s
cd /home/steam
/usr/games/steamcmd
mkdir logs
steamcmd +login anonymous +app_update 2394010 validate +quit
```

## Check it works

```
cd ~/Steam/steamapps/common/PalServer
./PalServer.sh
```

## Setup Supervisord

```
sudo apt update && sudo apt install supervisor
sudo systemctl status supervisor
sudo vim /etc/supervisor/conf.d/game-server.conf
# Input content from below
sudo supervisorctl update
```

## Supervisord config

```
[program:palworld]
directory=/home/steam/Steam/steamapps/common/PalServer
command=/bin/bash PalServer.sh
user=steam
process_name=palworld*%(process_num)s
numprocs=1
autostart=true
autorestart=true
stopwaitsecs=30
stderr_logfile=/home/steam/logs/palworld_stderr.log
stdout_logfile=/home/steam/logs/palworld_stdout.log
```
