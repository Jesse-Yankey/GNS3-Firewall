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
README
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
