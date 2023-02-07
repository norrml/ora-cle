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
