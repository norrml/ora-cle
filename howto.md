## starting
```
sudo apt update && sudo apt upgrade -y 
sudo apt install netfilter-persistent
sudo iptables -I INPUT -p tcp -m tcp --dport 2523 -j ACCEPT
sudo netfilter-persistent save
sudo timedatectl set-timezone Asia/Iraq
```

## ssh change port

```
sudo -i
sudo echo "Port 2523" >> /etc/ssh/sshd_config
sudo systemctl restart sshd
```

change ssh port 
`sudo nano /etc/ssh/sshd_config`

```
sudo systemctl restart sshd
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


### nginx block ip access
```
sudo nano /etc/nginx/sites-enabled/default 
```
```
location / {
    return 444 https://$host$request_uri;
  }
  if ($host != "domain.com") {
  return 444;
  }
```


### fail2ban block 444,404,401

``` sudo nano /etc/fail2ban/jail.local ```

```   sudo nano /etc/fail2ban/filter.d/nuker.conf ```

```
[Definition] 
failregex = ^<HOST>.-.-.*.*.444.*
            ^<HOST>.-.-.*.*.404.*
            ^<HOST>.-.-.*.*.400.*
            ^<HOST>*.*USERNAME*.*401*
           
ignoreregex = ^<HOST>*.*USERNEMR*.*200*
             ^<HOST>*.*USERNAME*.*
             ^<HOST>.-.-.*.*.200.*
             ^<HOST>.-.-.*.*.302.* 
            # ^<HOST>.-.-.*.*.*.*.*domain.com*
```

` sudo systemctl restart fail2ban.service `

```
[nuker]
enabled  = true
port    = http,https
filter   = caddy404
logpath = /var/log/nginx/access.log
bantime = 600d
maxretry  = 1
findtime  = 10
```

## ssh block attempts

set sshd.conf to aggressive in jail.local
 
```
[sshd]
enabled  = true
mode = aggressive
port    = ssh,2523   #change port
logpath = %(sshd_log)s
backend = %(sshd_backend)s
bantime = 60d
maxretry  = 0
findtime  = 1d
```


### wireguard things

```
sudo wget https://git.io/wireguard -O wireguard-install.sh && sudo bash wireguard-install.sh
Add the following to wg0.conf:
PostUp = /etc/wireguard/helper/add-nat-routing.sh 
PostDown = /etc/wireguard/helper/remove-nat-routing.sh
Create two corresponding scripts in /etc/wireguard/helper/ and add execution permissions.   add-nat-routing.sh: https://pastebin.com/raw/DWRcUjX2 remove-nat-routing.sh: https://pastebin.com/raw/pkf5Vv8Z
sudo chmod +x /etc/wireguard/helper/{remove-nat-routing.sh,add-nat-routing.sh}
echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf ;\
sysctl -p
 ## ifconfig, get wg0 inet 10.7.0.1
 ```
