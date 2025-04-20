# Cisco ASA Firewall - PAT (Port Address Translation) Configuration

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
- **Enable Password**: `qwertY!!`
- **Inside Interface**: `GigabitEthernet0/1`, IP: `192.168.0.4/31`
- **Outside Interface**: `GigabitEthernet0/0`, IP: `DHCP Assigned IP`# Cisco ASA Firewall - PAT (Port Address Translation) Configuration

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
```
! Create a network object for inside subnet
object network INSIDE-NAT
 subnet 192.168.80.0 255.255.255.192
 nat (inside,outside) dynamic interface
```
> `dynamic interface` tells the ASA to use the IP address assigned to the outside interface for PAT.

### Create a route for the internal  network (I configured a static route)
```
route outside 0.0.0.0 0.0.0.0 192.168.122.0 1
route inside 192.168.80.0 255.255.255.192 192.168.0.5 1
route inside 192.168.80.64 255.255.255.192 192.168.0.5 1
```

## Verification CommandsCan you help me with some documentation so i can post in my GitHub account
```
! Show NAT rules
show nat

! Show current NAT translations
show xlate

! Show running NAT config
show run nat
```

**Jesse Yankey**  
 njyankey123@gmail.com  
 [LinkedIn](https://www.linkedin.com/in/jesse-yankey)  
 [GitHub](https://www.github.com/Jesse-Yankey)
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
```
! Create a network object for inside subnet
object network INSIDE-NAT
 subnet 192.168.80.0 255.255.255.192
 nat (inside,outside) dynamic interface
```
> `dynamic interface` tells the ASA to use the IP address assigned to the outside interface for PAT.

### Create a route for the internal  network (I configured a static route)
```
route outside 0.0.0.0 0.0.0.0 192.168.122.0 1
route inside 192.168.80.0 255.255.255.192 192.168.0.5 1
route inside 192.168.80.64 255.255.255.192 192.168.0.5 1
```

## Verification CommandsCan you help me with some documentation so i can post in my GitHub account
```
! Show NAT rules
show nat

! Show current NAT translations
show xlate

! Show running NAT config
show run nat
```

**Jesse Yankey**  
 njyankey123@gmail.com  
 [LinkedIn](https://www.linkedin.com/in/jesse-yankey)  
 [GitHub](https://www.github.com/Jesse-Yankey)



## OPNSense Firewall - PAT (Port Address Translation) Configuration

### Overview
This project documents the process of configuring **PAT (Port Address Translation)** and **Static Routes** on an OPNsense firewall to enable internal network devices to access the internet and route traffic to remote networks through another router.

### Network Topology

```
Inside Network (192.168.0.10/31)Firewall
        |
[ OPNSense Firewall ]
        |
Outside Network (DHCP Assigned IP- Public)
```

- **Login Details**: Username: `root`, password: `qwertY!!`
- **Inside Interface**: `GigabitEthernet0/1`, IP: `192.168.0.10/31`
- **Outside Interface**: `GigabitEthernet0/0`, IP: `DHCP Assigned IP`
- **Internal Subnet**: `192.168.80.128/26`
- **Internal Subnet**: `192.168.80.192/26`

## Network Topology Overview
- **LAN Subnet**: `192.168.0.12/31`
- **WAN Interface IP**: `DHCP`
- **INTRT2 Interface IP**: `192.168.0.10/31` *(Point-to-point link)*
- **NAT Mode**: Hybrid
- **Goal**: Enable internet access for LAN clients using PAT and route to a remote network via static route.

## PAT Configuration (Outbound NAT)
1. Navigate to: `Firewall > NAT > Outbound`
2. Set NAT Mode to: **Hybrid Outbound NAT rule generation**
3. Click **Add** to create a rule:
   - **Interface**: `WAN`
   - **Source Network**: `192.168.80.128/26`
   - **Translation / target**: `Interface address`
   - **Description**: `PAT for 192.168.80.128/26 subnet`
4. Click **Add** to create a rule:
   - **Interface**: `WAN`
   - **Source Network**: `192.168.80.192/26`
   - **Translation / target**: `Interface address`
   - **Description**: `PAT for 192.168.80.192/26 subnet`
5. Save and Apply Changes

## Firewall Rule for LAN (INTRT2 Interface)
1. Go to: `Firewall > Rules > INTRT2`
2. Add a new rule:
   - **Action**: Pass
   - **Protocol**: Any
   - **Source**: `192.168.80.128/26`
   - **Destination**: Any
   - **Description**: `PAT for 80.128/26`
3. Add a new rule:
   - **Action**: Pass
   - **Protocol**: Any
   - **Source**: `192.168.80.192/26`
   - **Destination**: Any
   - **Description**: `PAT for 80.192/26`

4. Move the rule to the top if needed
5. Save and Apply Changes

## WAN Gateway Configuration
1. Go to: `System > Route > Configuration`
2. Add new gateway:
   - **Interface**: WAN
   - **Gateway IP**: (e.g., `192.168.122.1`)
   - **Mark as Default Gateway**: ✅
3. Save and Apply## OPNSense Firewall - PAT (Port Address Translation) Configuration

### Overview
This project documents the process of configuring **PAT (Port Address Translation)** and **Static Routes** on an OPNsense firewall to enable internal network devices to access the internet and route traffic to remote networks through another router.

### Network Topology

```
Inside Network (192.168.0.10/31)Firewall
        |
[ OPNSense Firewall ]
        |
Outside Network (DHCP Assigned IP- Public)
```

- **Inside Interface**: `GigabitEthernet0/1`, IP: `192.168.0.10/31`
- **Outside Interface**: `GigabitEthernet0/0`, IP: `DHCP Assigned IP`
- **Internal Subnet**: `192.168.80.128/26`
- **Internal Subnet**: `192.168.80.192/26`

## Network Topology Overview
- **LAN Subnet**: `192.168.0.12/31`
- **WAN Interface IP**: `DHCP`
- **INTRT2 Interface IP**: `192.168.0.10/31` *(Point-to-point link)*
- **NAT Mode**: Hybrid
- **Goal**: Enable internet access for LAN clients using PAT and route to a remote network via static route.

## PAT Configuration (Outbound NAT)
1. Navigate to: `Firewall > NAT > Outbound`
2. Set NAT Mode to: **Hybrid Outbound NAT rule generation**
3. Click **Add** to create a rule:
   - **Interface**: `WAN`
   - **Source Network**: `192.168.80.128/26`
   - **Translation / target**: `Interface address`
   - **Description**: `PAT for 192.168.80.128/26 subnet`
4. Click **Add** to create a rule:
   - **Interface**: `WAN`
   - **Source Network**: `192.168.80.192/26`
   - **Translation / target**: `Interface address`
   - **Description**: `PAT for 192.168.80.192/26 subnet`
5. Save and Apply Changes

## Firewall Rule for LAN (INTRT2 Interface)
1. Go to: `Firewall > Rules > INTRT2`
2. Add a new rule:
   - **Action**: Pass
   - **Protocol**: Any
   - **Source**: `192.168.80.128/26`
   - **Destination**: Any
   - **Description**: `PAT for 80.128/26`
3. Add a new rule:
   - **Action**: Pass
   - **Protocol**: Any
   - **Source**: `192.168.80.192/26`
   - **Destination**: Any
   - **Description**: `PAT for 80.192/26`

4. Move the rule to the top if needed
5. Save and Apply Changes

## WAN Gateway Configuration
1. Go to: `System > Route > Configuration`
2. Add new gateway:
   - **Interface**: WAN
   - **Gateway IP**: (e.g., `192.168.122.1`)
   - **Mark as Default Gateway**: ✅
3. Save and Apply

## Static Route Configuration

### Route traffic to INT-RT-2 interface `192.168.0.11` as the next-hop.
1. Go to: `System > Gateways > Configuration`
   - Create a gateway with:Can you help me with some documentation so i can post in my GitHub account
     - **Interface**: RT
     - **Name**: `INTRT2`
     - **Gateway IP**: `192.168.80.11`
2. Go to: `System > Routing > Configuration`
   - Add a static route:
     - **Destination Network**: `192.168.80.128/26`
     - **Gateway**: `RT-192.168.0.11`
3. Go to: `System > Routing > Configuration`
   - Add a static route:
     - **Destination Network**: `192.168.80.192/26`
     - **Gateway**: `RT-192.168.0.11`
4. Apply changes.

## Author
**Jesse Yankey**  
Email: [jesse.yankey01@gmail.com](mailto:jesse.yankey01@gmail.com)  
GitHub: [Jesse-Yankey](https://github.com/Jesse-Yankey)  
LinkedIn: [Jesse Yankey](https://www.linkedin.com/in/jesse-yankey)

```

## Static Route Configuration

### Route traffic to INT-RT-2 interface `192.168.0.11` as the next-hop.
1. Go to: `System > Gateways > Configuration`
   - Create a gateway with:Can you help me with some documentation so i can post in my GitHub account
     - **Interface**: RT
     - **Name**: `INTRT2`
     - **Gateway IP**: `192.168.80.11`
2. Go to: `System > Routing > Configuration`
   - Add a static route:
     - **Destination Network**: `192.168.80.128/26`
     - **Gateway**: `RT-192.168.0.11`
3. Go to: `System > Routing > Configuration`
   - Add a static route:
     - **Destination Network**: `192.168.80.192/26`
     - **Gateway**: `RT-192.168.0.11`
4. Apply changes.

## Author
**Jesse Yankey**  
Email: [jesse.yankey01@gmail.com](mailto:jesse.yankey01@gmail.com)  
GitHub: [Jesse-Yankey](https://github.com/Jesse-Yankey)  
LinkedIn: [Jesse Yankey](https://www.linkedin.com/in/jesse-yankey)

```
