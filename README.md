# Setup OpenVPN (truenas-scale)

![image](https://user-images.githubusercontent.com/37401242/180915686-b4e703c6-a901-4de8-b942-521da8d46fa8.png)
![image](https://user-images.githubusercontent.com/37401242/180915869-c3a4d2e0-7990-48a7-9abc-87319a82fa6c.png)

Above setup allowed your client VPN to connect your private network. However, the client unable to browse internet through the VPN. 

In order to browse internet, you need to add MASQUERADE.
```
iptables -t nat -A POSTROUTING -s 10.10.0.0/24 -o eno1 -j MASQUERADE
```

To add the route permanently, you can create a startup script.
1) Dump the current routing
```
iptables-legacy-save > /etc/iptables/rules.v4
```

2) create a startup script in /etc/systemd/system
```
[Unit]
Description=To restore iptable routing after reboot
After=network.target

[Service]
ExecStart=/usr/bin/sh -c '/usr/sbin/iptables-legacy-restore < /etc/iptables/rules.v4'

[Install]
WantedBy=default.target
```
