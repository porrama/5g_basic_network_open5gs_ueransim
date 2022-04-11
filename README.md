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
- VM#3 (RAN UE)

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
| VM#1               | Core Network     | #1 Host Only Adapter <br/> #2 NAT Network |
| VM#2               | RAN gNodeB       | #1 Host Only Adapter                      |
| VM#3               | RAN UE           | #1 Host Only Adapter                      |

### 2. IP Setting (Ubuntu Server)

2.1 Command for IP Configuration (Optional)
- Configuration File
~~~
cd ~/simulate_5g_basic_open5gs_ueransim
rm /etc/netplan/00-installer-config.yaml
cp 00-installer-config.yaml /etc/netplan/00-installer-config.yaml
netplan apply
~~~

- Manual
~~~
sudo vim /etc/netplan/00-installer-config.yaml
netplan apply
~~~

Options to configure **00-installer-config.yaml** file

> Static IP configuration
~~~
network:
    ethernets:
        enp0s3:
            addresses: [192.168.0.1/24]
        version: 2
~~~

> Automatic (DHCP) IP configuration
~~~
network:
    ethernets:
        enp0s3:
            dhcp4: true
        version: 2
~~~

2.2 Network settings < net.ipv4.ip_forward=1 >
~~~
cd ~/simulate_5g_basic_open5gs_ueransim
rm /etc/sysctl.conf
cp sysctl.conf /etc/sysctl.conf
~~~

### 3. Network Function (Open5GS)
- Access and Mobility Management Function (AMF)
~~~
cd ~/simulate_5g_basic_open5gs_ueransim
rm ../../open5gs/install/etc/open5gs/amf.yaml
cp amf.yaml ../../open5gs/install/etc/open5gs/amf.yaml
~~~

-  Session Management Function (SMF)
~~~
cd ~/simulate_5g_basic_open5gs_ueransim
rm ../../open5gs/install/etc/open5gs/smf.yaml
cp smf.yaml ../../open5gs/install/etc/open5gs/smf.yaml
~~~

- User Plane Function (UPF)
~~~
cd ~/simulate_5g_basic_open5gs_ueransim
rm ../../open5gs/install/etc/open5gs/umf.yaml
cp umf.yaml ../../open5gs/install/etc/open5gs/umf.yaml
~~~

### 4. RAN gNodeB (UERANSIM)

### 5. RAN UE (UERANSIM)

---

<div id='id-testing'/>

## Simulation Testing

---

<div id='id-reference'/>

## Reference
- [Open5GS Documentation](https://open5gs.org/open5gs/docs/guide/02-building-open5gs-from-sources/)
- [UERANSIM Documentation](https://github.com/aligungr/UERANSIM)


---
