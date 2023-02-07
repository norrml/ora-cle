## starting
```
sudo apt update && sudo apt upgrade -y 
sudo apt install netfilter-persistent
sudo iptables -I INPUT -p tcp -m tcp --dport 2523 -j ACCEPT
sudo netfilter-persistent save
sudo timedatectl set-timezone Asia/Iraq
```

### Screen
```
screen -S taskname #start new putty withtaskname
Ctrl+a d
screen -r taskname # return to taskname
screen -ls  # list task name
```

## firewall iptables

```
sudo iptables -I INPUT -p tcp -m tcp --dport 51820-j ACCEPT
sudo iptables -I INPUT -p tcp -m tcp --dport 2400 -j ACCEPT
sudo iptables -I INPUT -p tcp -m tcp --dport 32400 -j ACCEPT
sudo iptables -I INPUT -p tcp -m tcp --dport 2523 -j ACCEPT
sudo iptables -I INPUT -p tcp -m tcp --dport 443 -j ACCEPT
sudo iptables -I INPUT -p tcp -m tcp --dport 80 -j ACCEPT
sudo iptables -I INPUT -p tcp -m tcp --dport 8989 -j ACCEPT
sudo iptables -I INPUT -p tcp -m tcp --dport 7878 -j ACCEPT
```

## ssh change port

```
sudi -i
sudo echo "Port 2523" >> /etc/ssh/sshd_config
sudo systemctl restart sshd

```

change ssh port 
`sudo nano /etc/ssh/sshd_config`

```
sudo systemctl restart sshd
```
