# GNS3-Firewall
## Cisco ASA Firewall - PAT (Port Address Translation) Configuration

### Overview

This documentation provides a step-by-step guide on configuring **Port Address Translation (PAT)** on a Cisco ASA firewall in GNS3. PAT allows multiple devices on a private network to access external networks (like the internet) using a single public IP address.

### Network Topology

```
Inside Network (192.168.0.4/31)Firewall
        |
    [ ASA Firewall ]
        |
Outside Network (DHCP Assigned IP- Public)
```

- **Inside Interface**: `GigabitEthernet0/1`, IP: `192.168.0.4/31`
- **Outside Interface**: `GigabitEthernet0/0`, IP: `DHCP Assigned IP`
- **Internal Subnet**: `192.168.80.0/26`

## Configuration Steps

### Configure Interfaces

```bash
interface GigabitEthernet0/0
 description INT-RT-1
 nameif inside
 security-level 100
 ip address 192.168.0.4 255.255.255.254 

interface GigabitEthernet0/2
 description INTERNET
 nameif outside
 security-level 0
 ip address dhcp setroute
```

### Define NAT Rules

```bash
! Create a network object for inside subnet
object network INSIDE-NAT
 subnet 192.168.80.0 255.255.255.192
 nat (inside,outside) dynamic interface
```

> `dynamic interface` tells the ASA to use the IP address assigned to the outside interface for PAT.

---

### Create a route for the internal  network (I configured a static route)

```
route outside 0.0.0.0 0.0.0.0 192.168.122.0 1
route inside 192.168.80.0 255.255.255.192 192.168.0.5 1
route inside 192.168.80.64 255.255.255.192 192.168.0.5 1
```

## Verification Commands

```bash
! Show NAT rules
show nat

! Show current NAT translations
show xlate

! Show running NAT config
show run nat
```

## 👨‍💻 Author

**Jesse Yankey**  
📧 njyankey123@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/jesse-yankey)  
🐙 [GitHub](https://www.github.com/Jesse-Yankey)


Would you like me to generate the GitHub `repo structure` or even prepare a zipped project directory with a sample `.cfg` file too?
