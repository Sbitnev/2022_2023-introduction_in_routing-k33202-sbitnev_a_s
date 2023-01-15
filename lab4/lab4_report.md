##### University: [ITMO University](https://itmo.ru/ru/)
##### Faculty: [FICT](https://fict.itmo.ru)
##### Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)
##### Year: 2022/2023
##### Group: K33202
##### Author: Sbitnev Aleksandr
##### Lab: Lab4
##### Date of create: 10.01.2023
##### Date of finished: ???

***

# Отчёт по лабораторной работе №4 "Эмуляция распределенной корпоративной сети связи, настройка iBGP, организация L3VPN, VPLS"


**Цель работы:** Изучить протоколы BGP, MPLS и правила организации L3VPN и VPLS.

### **Результаты:**

#### **1** Схема связи:

![](https://github.com/Sbitnev/2022_2023-introduction_in_routing-k33202-sbitnev_a_s/blob/main/lab4/pics/sh.jpg "Схема связи")

#### **2** Файл yaml:

```
name: lab4

mgmt:
  network: statics
  ipv4_subnet: 172.20.20.0/24

topology:
  nodes:
    R01.NY:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.2

    R01.LND:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.3

    R01.HKI:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.4

    R01.SPB:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.5

    R01.LBN:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.6

    R01.SVL:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.7

    PC1:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.8

    PC2:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.9

    PC3:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.10
  

  links: 
    - endpoints: ["R01.NY:eth1","PC2:eth1"] 
    - endpoints: ["R01.NY:eth3","R01.LND:eth3"] 
    - endpoints: ["R01.LND:eth1","R01.HKI:eth2"]     
    - endpoints: ["R01.LND:eth2","R01.LBN:eth1"] 
    - endpoints: ["R01.LBN:eth2","R01.HKI:eth1"] 
    - endpoints: ["R01.LBN:eth3","R01.SVL:eth3"] 
    - endpoints: ["R01.SVL:eth1","PC3:eth1"]  
    - endpoints: ["R01.HKI:eth3","R01.SPB:eth3"]  
    - endpoints: ["R01.SPB:eth1","PC1:eth1"] 
```
#### **3** Результат запуска lab4.yaml:

![](https://github.com/Sbitnev/2022_2023-introduction_in_routing-k33202-sbitnev_a_s/blob/main/lab4/pics/clab.jpg)

### **Первая часть:**

#### **4.1** Текст конфигураций для каждого сетевого устройства:

**R01.NY:**
```
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default redistribute-connected=yes router-id=10.10.10.3
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.3
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.3 interface=Lo0 network=10.10.10.3
add address=192.168.20.10/24 interface=ether2 network=192.168.20.0
add address=10.10.3.1/30 interface=ether4 network=10.10.3.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route vrf
add export-route-targets=65530:100 import-route-targets=65530:100 interfaces=ether2 route-distinguisher=65530:100 routing-mark=VRF_DEVOPS
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether4
/routing bgp instance vrf
add redistribute-connected=yes routing-mark=VRF_DEVOPS
/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.6 remote-as=65530 update-source=Lo0
/routing ospf network
add area=backbone
/system identity
set name=R01.NY
```
**R01.SPB:**
```
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default router-id=10.10.10.1
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.1
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.1 interface=Lo0 network=10.10.10.1
add address=10.10.1.1/30 interface=ether4 network=10.10.1.0
add address=192.168.10.10/24 interface=ether2 network=192.168.10.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route vrf
add export-route-targets=65530:100 import-route-targets=65530:100 interfaces=ether2 route-distinguisher=65530:100 routing-mark=VRF_DEVOPS
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether4
/routing bgp instance vrf
add redistribute-connected=yes routing-mark=VRF_DEVOPS
/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.4 remote-as=65530 update-source=Lo0
/routing ospf network
add area=backbone
/system identity
set name=R01.SPB
```
**R01.SVL:**
```
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default router-id=10.10.10.2
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.2
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.2 interface=Lo0 network=10.10.10.2
add address=10.10.2.1/30 interface=ether4 network=10.10.2.0
add address=192.168.30.10/24 interface=ether2 network=192.168.30.0
/ip dhcp-client
add disabled=no interface=ether1
/ip route vrf
add export-route-targets=65530:100 import-route-targets=65530:100 interfaces=ether2 route-distinguisher=65530:100 routing-mark=VRF_DEVOPS
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether4
/routing bgp instance vrf
add redistribute-connected=yes routing-mark=VRF_DEVOPS
/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.5 remote-as=65530 update-source=Lo0
/routing ospf network
add area=backbone
/system identity
set name=R01.SVL
```
**R01.LND:**
```
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default router-id=10.10.10.6
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.6
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.6 interface=Lo0 network=10.10.10.6
add address=10.10.4.1/30 interface=ether2 network=10.10.4.0
add address=10.10.6.2/30 interface=ether3 network=10.10.6.0
add address=10.10.3.2/30 interface=ether4 network=10.10.3.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4
/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.5 remote-as=65530 route-reflect=yes update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer2 remote-address=10.10.10.4 remote-as=65530 route-reflect=yes update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer3 remote-address=10.10.10.3 remote-as=65530 update-source=Lo0
/routing ospf network
add area=backbone
/system identity
set name=R01.LND
```
**R01.LBN:**
```
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default router-id=10.10.10.5
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.5
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.5 interface=Lo0 network=10.10.10.5
add address=10.10.6.1/30 interface=ether2 network=10.10.6.0
add address=10.10.2.2/30 interface=ether4 network=10.10.2.0
add address=10.10.5.2/30 interface=ether3 network=10.10.5.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4
/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.6 remote-as=65530 route-reflect=yes update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer2 remote-address=10.10.10.4 remote-as=65530 route-reflect=yes update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer3 remote-address=10.10.10.2 remote-as=65530 update-source=Lo0
/routing ospf network
add area=backbone
/system identity
set name=R01.LBN
```
**R01.HKI:**
```
/interface bridge
add name=Lo0
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/routing bgp instance
set default router-id=10.10.10.4
/routing ospf instance
set [ find default=yes ] router-id=10.10.10.4
/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=10.10.10.4 interface=Lo0 network=10.10.10.4
add address=10.10.5.1/30 interface=ether2 network=10.10.5.0
add address=10.10.4.2/30 interface=ether3 network=10.10.4.0
add address=10.10.1.2/30 interface=ether4 network=10.10.1.0
/ip dhcp-client
add disabled=no interface=ether1
/mpls ldp
set enabled=yes
/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4
/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.6 remote-as=65530 route-reflect=yes update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer2 remote-address=10.10.10.5 remote-as=65530 route-reflect=yes update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer3 remote-address=10.10.10.1 remote-as=65530 update-source=Lo0
/routing ospf network
add area=backbone
/system identity
set name=R01.HKI
```

#### **5.1** Проверка связности между VRF:

![](https://github.com/Sbitnev/2022_2023-introduction_in_routing-k33202-sbitnev_a_s/blob/main/lab4/pics/v1.jpg)
![](https://github.com/Sbitnev/2022_2023-introduction_in_routing-k33202-sbitnev_a_s/blob/main/lab4/pics/v2.jpg)
![](https://github.com/Sbitnev/2022_2023-introduction_in_routing-k33202-sbitnev_a_s/blob/main/lab4/pics/v3.jpg)

### **Вторая часть:**

#### **4.2** Текст конфигураций для каждого сетевого устройства:

**R01.NY:**
```

```
**R01.SPB:**
```

```
**R01.SVL:**
```

```
**PC1:**
```

```
**PC2:**
```

```
**PC3:**
```

```

#### **5.2** Результаты пингов, проверки локальной связности:

![]()

### **Вывод:**
В результате выполнения работы были изучены протоколы BGP, MPLS и правила организации L3VPN и VPLS.
