## Simulation 5G network with Virtual RAN and UE

---

## Table of Contents
- [Software and Hardware](#id-specification)
- [Network Topology and Simulation Model](#id-overview)
- [Initial Preparation](#id-init)
- [Software Configulation](#id-configure)
- [Simulation Testing](#id-testing)
- [Reference](#id-reference)

---

<div id='id-specification'/>

## Software and Hardware

### Hardware Specification
| Module           | Detail                                         |
| -----------      | -----------                                    |
| Operating System | Windows 11 Home 21H2                           |
| Processer        | 11th Gen Intel(R) Core (TM) i5-1135G7 @2.40GHz |
| RAM              | 16.0 GB                                        |
| Graphics Card    | Intel(R) Iris(R) Xe Graphics                   |
| Connectivity     | Intel(R) Wi-Fi 6 AX201 160MHz                  |

### Software Used
| Software      | Version                 |
| -----------   | -----------             |
| VirsualBox    | VirtualBox 6.1          |
| Open5GS       | Updated Jan, 2022       |
| UERANSIM      | Release v3.2.5          |
| Ubuntu Server | Ubuntu Server 20.04 LTS |

---

<div id='id-overview'/>

## Network Topology and Simulation Model

---

<div id='id-init'/>

## Initial Preparation

### 1. [Open5GS software](https://github.com/porrama/install_open5gs) 
- VM#1 (Core Network)

### 2. [UERANSIM software](https://github.com/porrama/install_ueransim) 
- VM#2 (RAN gNodeB)
- VM#3 (RAN UE 1)
- VM#4 (RAN UE 2)

---

<div id='id-configure'/>

## Software Configuration

Clone this project
~~~
cd ~
git clone https://github.com/porrama/simulate_5g_basic_open5gs_ueransim
~~~

### 1. Network Setting of in VirtualBox

Setting -> Network -> Choose the Adapter and ***Enable Network Adapter*** Option

| Virsual Box Number | Role             | Adapter                                   |
| -----------        | -----------      | -----------                               |
| VM#1               | Core Network     | #1 Host Only Adapter <br> #2 NAT Network  |
| VM#2               | RAN gNodeB       | #1 Host Only Adapter                      |
| VM#3               | RAN UE 1         | #1 Host Only Adapter                      |
| VM#4               | RAN UE 2         | #1 Host Only Adapter                      |

### 2. IP Setting (Ubuntu Server)

2.1 Command for IP Configuration

- Configuration File of **Core Network - VM#1**
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/core_configuration_file
sudo rm /etc/netplan/00-installer-config.yaml
~~~
~~~
sudo cp 00-installer-config.yaml /etc/netplan/00-installer-config.yaml
sudo netplan apply
~~~

- Configuration File of **RAN gNodeB - VM#2** 
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/ran_configuration_file
sudo rm /etc/netplan/00-installer-config.yaml
~~~
~~~
sudo cp 00-installer-config-gnodeb.yaml /etc/netplan/00-installer-config.yaml
sudo netplan apply
~~~

- Configuration File of **RAN UE1 - VM#3** 
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/ran_configuration_file
sudo rm /etc/netplan/00-installer-config.yaml
~~~
~~~
sudo cp 00-installer-config-ue1.yaml /etc/netplan/00-installer-config.yaml
sudo netplan apply
~~~

- Configuration File of ***RAN UE2 - VM#4*** 
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/ran_configuration_file
sudo rm /etc/netplan/00-installer-config.yaml
~~~
~~~
sudo cp 00-installer-config-ue2.yaml /etc/netplan/00-installer-config.yaml
sudo netplan apply
~~~

2.2 Network settings < net.ipv4.ip_forward=1 >
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/core_configuration_file
sudo rm /etc/sysctl.conf
sudo cp sysctl.conf /etc/sysctl.conf
~~~

### 3. Network Function (Open5GS)

- Access and Mobility Management Function (AMF)
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/core_configuration_file
rm ../../open5gs/install/etc/open5gs/amf.yaml
cp amf.yaml ../../open5gs/install/etc/open5gs/amf.yaml
~~~

-  Session Management Function (SMF)
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/core_configuration_file
rm ../../open5gs/install/etc/open5gs/smf.yaml
cp smf.yaml ../../open5gs/install/etc/open5gs/smf.yaml
~~~

- User Plane Function (UPF)
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/core_configuration_file
rm ../../open5gs/install/etc/open5gs/upf.yaml
cp upf.yaml ../../open5gs/install/etc/open5gs/upf.yaml
~~~

- WebUI

[Run WebUI](#id-webui) -> **http://192.168.0.101:3000** -> Login -> Subscriber Menu -> Click + Button -> Fill **IMSI** -> SAVE 

> Username : admin <br>
> Password : 1423

| UE              | IMSI                | DNN                | OP/ OPc            |
| -----------     | -----------         | -----------        | -----------        |
| 1               | 001010000000000     | Internet           | OPc                | 
| 2               | 001010000000001     | Internet           | OPc                |


### 4. RAN gNodeB (UERANSIM)

- open5gs-gnb.yaml
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/ran_configuration_file
rm ../../UERANSIM/config/open5gs-gnb.yaml
cp open5gs-gnb.yaml ../../UERANSIM/config/open5gs-gnb.yaml
~~~

### 5. RAN UE (UERANSIM)

- open5gs-ue1.yaml
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/ran_configuration_file
rm ../../UERANSIM/config/open5gs-ue.yaml
cp open5gs-ue1.yaml ../../UERANSIM/config/open5gs-ue.yaml
~~~

- open5gs-ue1.yaml
~~~
cd ~/simulate_5g_basic_open5gs_ueransim/ran_configuration_file
rm ../../UERANSIM/config/open5gs-ue.yaml
cp open5gs-ue1.yaml ../../UERANSIM/config/open5gs-ue.yaml
~~~

---

<div id='id-testing'/>

## Simulation Testing

Run **runnfv_open5gs.sh**
~~~
cd ~/open5gs/install/bin
sudo sh ~/install_open5gs/runnfv_open5gs.sh
~~~ 

<div id='id-webui'/>

Run **runwebui_open5gs.sh** 
~~~
cd ~/open5gs/webui
sudo sh ~/install_open5gs/runwebui_open5gs.sh
~~~

---

<div id='id-reference'/>

## Reference
- [Open5GS Documentation](https://open5gs.org/open5gs/docs/guide/02-building-open5gs-from-sources/)
- [UERANSIM Documentation](https://github.com/aligungr/UERANSIM)


---
