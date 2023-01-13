##### University: [ITMO University](https://itmo.ru/ru/)
##### Faculty: [FICT](https://fict.itmo.ru)
##### Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)
##### Year: 2022/2023
##### Group: K33202
##### Author: Sbitnev Aleksandr
##### Lab: Lab1
##### Date of create: 03.09.2022
##### Date of finished: ???

***

# Отчёт по лабораторной работе №1 "Установка ContainerLab и развертывание тестовой сети связи"


**Цель работы:** Ознакомиться с инструментом ContainerLab и методами работы с ним, изучить работу VLAN и IP адресации.

### **Результаты:**

#### **1** Схема связи:

![](https://github.com/Sbitnev/2022_2023-introduction_in_routing-k33202-sbitnev_a_s/blob/main/lab1/pics/sh.jpg "Схема связи")

#### **2** Файл yaml:

```

name: lab1

mgmt:
  network: statics
  ipv4_subnet: 172.20.20.0/24

topology:
  nodes:
    R01.TEST:
      kind: vr-ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 172.20.20.2

    SW01.L3.01.TEST:
        kind: vr-ros
        image: vrnetlab/vr-routeros:6.47.9
        mgmt_ipv4: 172.20.20.3

    SW02.L3.01.TEST:
        kind: vr-ros
        image: vrnetlab/vr-routeros:6.47.9
        mgmt_ipv4: 172.20.20.4

    SW02.L3.02.TEST:
        kind: vr-ros
        image: vrnetlab/vr-routeros:6.47.9
        mgmt_ipv4: 172.20.20.5

    PC1:
      kind: linux
      image: ubuntu:latest
      mgmt_ipv4: 172.20.20.6

    PC2:
      kind: linux
      image: ubuntu:latest
      mgmt_ipv4: 172.20.20.7

  links:
    - endpoints: ["R01.TEST:eth1", "SW01.L3.01.TEST:eth1"]
    - endpoints: ["SW01.L3.01.TEST:eth2", "SW02.L3.01.TEST:eth1"]
    - endpoints: ["SW02.L3.01.TEST:eth2", "PC1:eth1"]
    - endpoints: ["SW01.L3.01.TEST:eth3","SW02.L3.02.TEST:eth1"]
    - endpoints: ["SW02.L3.02.TEST:eth2", "PC2:eth1"]
    
```
