#Update repo
sudo apt update && sudo apt upgrade -y

#install some utilities
sudo apt install curl build-essential git wget jq make gcc tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y

#Fail2ban
apt install fail2ban -y && \
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local && \
nano /etc/fail2ban/jail.local

Ctrl+X,Y,Enter

#restart fail2ban
systemctl restart fail2ban

#install docker
apt update && \
apt install apt-transport-https ca-certificates curl software-properties-common -y && \
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && \
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && \
apt update && \
apt-cache policy docker-ce && \
sudo apt install docker-ce -y && \
docker --version

#open tmux session
tmux new-session -s tenderduty

mkdir tenderduty && cd tenderduty
docker run --rm ghcr.io/blockpane/tenderduty:latest -example-config >config.yml

#change Name,Valloper and Chain ID
nano $HOME/tenderduty/config.yml

#change config
docker run -d --name tenderduty -p "8888:8888" -p "28686:28686" --restart unless-stopped -v $(pwd)/config.yml:/var/lib/tenderduty/config.yml ghcr.io/blockpane/tenderduty:latest

#check logs
docker logs -f --tail 20 tenderduty

#lets check our web address for monitoring
echo -e "\033[0;32mhttp://$(wget -qO- eth0.me):8888/\033[0m"

____________________
#useful commands
____________________
#stop container
docker stop tenderduty

#check logs
docker logs -f --tail 20 tenderduty

#restart container
docker restart tenderduty

___________________________________
#to create alarm channel on discord
___________________________________

1) create discord group
2) go to server settings>integration
3) create webhoot
4) get link
5) paste the link from discord to config.yml

________________________________
#to create alarm bot on telegram
________________________________

1) talk with botfather
2) enter /newbot command
3) enter the name of your bot (Must end with a bot) ex: alarmbot
4) copy the api
5) send random message to JsonViewBot
6) copy ID
7) go to the config.yml
8) change id and api key

ready
