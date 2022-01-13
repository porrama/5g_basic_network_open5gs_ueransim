## Simulation 5G network and Testing with Virtual UE

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

### 1. IP Setting (Ubuntu Server)

Command for IP Configuration
~~~
sudo vim /etc/netplan/00-installer-config.yaml
netplan apply
~~~

Static IP configuration of **00-installer-config.yaml**
~~~
network:
    version: 2
      ethernets:
          enp0s3:
              addresses: [192.168.0.111/24]
              dhcp4: false
~~~



### 2. Network Function (Open5GS)

### 3. RAN gNodeB (UERANSIM)

### 4. RAN UE (UERANSIM)

---

<div id='id-testing'/>

## Simulation Testing

---

<div id='id-reference'/>

## Reference
- [Open5GS Documentation](https://open5gs.org/open5gs/docs/guide/02-building-open5gs-from-sources/)
- [UERANSIM Documentation](https://github.com/aligungr/UERANSIM)


---
