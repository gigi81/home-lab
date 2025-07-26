This is the magic that NATs the packets to the web server correctly.
This needs to be added to the `wg0.conf`
Replace `172.18.0.3` with the web server IP address in the docker network.

```
PostUp = iptables -t nat -A PREROUTING -i wg0 -p tcp --dport 80 -j DNAT --to-destination 172.18.0.3:80
PostUp = iptables -t nat -A PREROUTING -i wg0 -p tcp --dport 443 -j DNAT --to-destination 172.18.0.3:443
PostUp = iptables -t nat -A POSTROUTING -o wg0 -j MASQUERADE
PostUp = iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

PreDown = iptables -t nat -D PREROUTING -i wg0 -p tcp --dport 80 -j DNAT --to-destination 172.18.0.3:80
PreDown = iptables -t nat -D PREROUTING -i wg0 -p tcp --dport 443 -j DNAT --to-destination 172.18.0.3:443
PreDown = iptables -t nat -D POSTROUTING -o wg0 -j MASQUERADE
PreDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
```

