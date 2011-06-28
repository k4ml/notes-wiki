```bash
Mohammad Bahathir Hashim - Here is a better firewall rules

1) Drop everything.
# iptables -P INPUT DROP

2) Always accepts from local interface lo
# iptables -A INPUT -i lo -j ACCEPT

2) Accept only related links (statefull inspection)
# iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

3) Start poking/opening ports
# iptables -A INPUT -p tcp --dport 22 -j ACCEPT
# iptables -A INPUT -p tcp --dport 3306 -s 192.169.0.0/24 -j ACCEPT
# iptables -A INPUT -p tcp --dport 80 -j ACCEPT
Continue poking the fw :)

https://profiles.google.com/mypapit/posts/T87xQDqKjEn
Or if you don't want to use -P INPUT DROP, default drop, how about this 2 rules
# iptables -A INPUT -i lo -j ACCEPT
# iptables -A INPUT ! -s 192.168.0.0/24 -p tcp --dport 3306 -j DROP 

BTW, if the server is in DMZ and only use private IP, it is quite useless to block IP from outside, since public IP cannot reroutable to private LAN IP.5:56 pm (edited 6:04 pm)
```