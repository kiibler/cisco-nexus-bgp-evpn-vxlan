# Control Plane validointi

### 1. VXLAN ja VNIt

```text
SW1# show nve vni
Interface VNI      Multicast-group   State Mode Type [BD/VRF]      Flags
--------- -------- ----------------- ----- ---- ------------------ -----
nve1      10010    UnicastBGP        Up    CP   L2 [10]                 
nve1      10011    UnicastBGP        Up    CP   L2 [11]                 
nve1      10020    UnicastBGP        Up    CP   L2 [20]                 
nve1      10021    UnicastBGP        Up    CP   L2 [21]                 
nve1      10100    n/a               Up    CP   L3 [TENANT-A]           
nve1      10200    n/a               Up    CP   L3 [TENANT-B]
```

### 2. VTEP-naapurit

```text
SW1# show nve peers
Interface Peer-IP                                 State LearnType Uptime   Route r-Mac       
--------- --------------------------------------  ----- --------- -------- -----------------
nve1      10.0.0.22                               Up    CP        00:02:11 n/a  
nve1      10.0.0.33                               Up    CP        00:02:11 n/a
```

### 3. BGP EVPN naapuruudet ja Route Reflector

```text
SW1# show bgp l2vpn evpn summary 
Neighbor        V    AS    MsgRcvd    MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
10.0.0.4        4 65000       3185       3007      624    0    0    2d01h 15    
10.0.0.5        4 65000       3185       3007      624    0    0    2d01h 15    

Neighbor        T    AS PfxRcd     Type-2     Type-3     Type-4     Type-5    
10.0.0.4        I 65000 15         7          8          0          0         
10.0.0.5        I 65000 15         7          8          0          0
```
