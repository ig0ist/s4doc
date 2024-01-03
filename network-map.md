# Trusted subnet

```

subnet 10.2.0.0 netmask 255.255.0.0 {
  range 10.2.0.100 10.2.0.200;
  option routers 10.2.0.1;
  option domain-name-servers 10.2.0.1;
  option domain-search "fnv.net";
  option rfc3442-classless-static-routes
         16, 10,1,       10,2,0,1,
         24, 192,168,10, 10,2,0,1;
}


```


## BIND data file for fnv.net

```


$TTL    604800
@       IN      SOA     ecg.fnv.net. root.fnv.net. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      ecg.fnv.net.
ecg      IN      A       10.1.0.1
ecg      IN      A       10.2.0.1

; Trusted subnet
adas     IN      A       10.2.0.3
hud      IN      A       10.2.0.4

; Untrusted subnet
tcu      IN      A       10.1.0.2
sync     IN      A       10.1.0.3


;
@   IN      NS      ecg.fnv.net.
1.0.1      IN      PTR     ecg.fnv.net.
1.0.2      IN      PTR     ecg.fnv.net.

; Trusted subnet
3.0.2      IN      PTR     adas.fnv.net.
4.0.2      IN      PTR     hud.fnv.net.

; Untrusted subnet
2.0.1      IN      PTR     tcu.fnv.net.
3.0.1      IN      PTR     sync.fnv.net.
    

```

## arp_cache

```

10.1.0.2 02:f0:52:44:00:11 rgmii0
10.1.0.3 02:f0:52:44:00:12 rgmii1

10.2.0.4 02:F0:52:44:00:23 sgmii0			hud.fnv.net.
10.2.0.2 02:F0:52:44:00:21 sgmii1

10.2.0.3 02:F0:52:44:00:22 sgmii2			adas.fnv.net.
10.2.0.6 02:F0:52:44:00:25 sgmii2




```



## Linux network apply



```

ip link set dev enx0000120537cc down
ip link set dev enx0000120537cc address 02:f0:52:44:00:23
ip link set dev enx0000120537cc up
 
sudo netplan apply

```