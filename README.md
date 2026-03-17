# Enterprise Network

## Network Topology 

![Topology](images/topology.png)

### ISP_1
- Role: ISP provider 
- Routing: BGP neighbor with edge router 
- ASN: 65001
- Advertises: Default route 0.0.0.0/0

### ISP_2
- Role: ISP provider 
- Routing: BGP neighbor with edge router
- ASN: 65002
- Advertises: Default route 0.0.0.0/0

### Edge Router 
- Role: Internet edge device 
- Routing:
    - eBGP with ISP_1 and ISP_2
    - OSPF with Core (Lan + DMZ)
- Functions:
    - Route redistribution (BGP <--> OSPF)
    - Default route injection into OSPF
- Redundancy: Dual ISP upstream connectivity 

### VLANs, etherchannels, SVIs
