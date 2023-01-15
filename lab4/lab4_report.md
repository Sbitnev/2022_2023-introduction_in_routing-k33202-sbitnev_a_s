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

```
**R01.SPB:**
```

```
**R01.SVL:**
```

```
**R01.LND:**
```

```
**R01.LBN:**
```

```
**R01.HKI:**
```

```

#### **5.1** Проверка связности между VRF:

![]()

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
