# Enterprise Network

## Network Topology 

![Topology](images/topology.png)

### ISP_1
- Role: ISP provider 
- Routing: BGP neighbor with edge router 
- ASN: 65001
- Advertises: Default route 0.0.0.0/0

![BGP](images/ISP_1.png)

### ISP_2
- Role: ISP provider 
- Routing: BGP neighbor with edge router
- ASN: 65002
- Advertises: Default route 0.0.0.0/0

![BGP](images/ISP_2.png)

### Edge Router 
- Role: Internet edge device 
- Redundancy: Dual ISP upstream connectivity 
- Routing:
    - eBGP with ISP_1 and ISP_2
    - NOTE: loopback interfaces would be preferred for BGP peering to improve stability; however, Packet Tracer limitations required the use of directly connected interface IPs.

![BGP](images/Edge.png)

- Edge <--> Firewall
- Functions:
    - Route redistribution (BGP <--> OSPF)
    - Default route injection into OSPF for internet reachability to internal networks

![route_injection](images/bgp_ospf.png)

- NAT/PAT
    - Configured PAT to translate internal private IP space to a sinlge public IP

![pat](images/pat_2.png)


### Firewall

- Role: Firewall / Area Border Router (ABR)
- Routing: OSPF (Area 0 <--> Area 1)
- Functions: 
    - Connects backbone (Area 0) to LAN (Area 1)
    - Performs inter-area route summarization
    - Provides internal traffic path to edge --> internet 
- OSPF Design:
    - Area 0 --> Edge router, DMZ
    - Area 1 --> LAN core
- Route Handling: 
    - Receives default route (0.0.0.0/0) from edge via OSPF
    - Advertises summarixed LAN route (10.10.0.0/16) into Area 0
    - Maintains specific LAN routes internally (10.10.x.x/24)
- OSPF external default route (E2) learned from the edge router, used to direct outbound traffic to the Internet

![E2](images/e2.png)



### LAN Core

- Role: Layer 3 Core Switch
- Routing: OSPF (Area 1)
- Function:
    - Provides inter-VLAN routing for internal networks
    - Acts as the default gateway for end devices
    - Connects access layer switch to the routed network
- Layer 3 Services:
    - Inter-VLAN routing via SVIs
    - Default gateway assignments for each VLAN

![SVI](images/svi.png)

- OSPF design:
    - Participates in Area 1 (LAN)
    - Advertises all VLAN subnets (10.10.x.0/24) to the firewall (ABR)
- Layer 2 Integration:
    - Etherchannel (LACP) uplink to access switch
    - Trunk configured with allowed VLANs for endpoint connectivity 
    - DTP disabled for security and consistency 

![etherchannel_1](images/etherchannel_1.png)

- Infrastructure Services:
    - DHCP for dynamic IP address assignment
    - DHCP relay configured on SVIs
    - DNS for internal name resolution (enterprise.com)

![dhcp_pools](images/dhcp_pools.png)

![dhcp_lease](images/dhcp_lease.png)

![dns](images/dns.png)
