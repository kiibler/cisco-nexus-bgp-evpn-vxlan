# Asiakaseristys ja osoitetaulukot

### 1. Päällekkäisten IP-osoitteiden hallinta ARP-taulukossa

```text
SW1# show ip arp vrf TENANT-A
Address         Age       MAC Address     Interface       Flags
172.16.0.101    00:01:31  e622.243a.11c8  Vlan10                   

SW1# show ip arp vrf TENANT-B
Address         Age       MAC Address     Interface       Flags
172.16.0.101    00:01:51  762b.0ccd.3459  Vlan20
```

### 2. BGP EVPN reititystaulukon eristys (`Route Distinguishers`)

```text
SW1# show bgp l2vpn evpn vrf TENANT-A
Route Distinguisher: 10.0.0.1:9    (L3VNI 10100)
*>i[2]:[0]:[0]:[48]:[6e44.492a.f97a]:[32]:/272
                      10.0.0.22                         100          0 i
*>i[2]:[0]:[0]:[48]:[ee4a.dcfb.cdfc]:[32]:/272
                      10.0.0.33                         100          0 I

SW1# show bgp l2vpn evpn vrf TENANT-B
Route Distinguisher: 10.0.0.1:10    (L3VNI 10200)
*>i[2]:[0]:[0]:[48]:[6a98.4844.e3f3]:[32]:/272
                      10.0.0.22                         100          0 I
```

### 3. MAC-osoitetaulukot

```text
SW1# show mac address-table vlan 10
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
*   10     e622.243a.11c8   dynamic  NA         F      F    Eth1/10

SW1# show mac address-table vlan 11
   VLAN     MAC Address      Type      age     Secure NTFY Ports
---------+-----------------+--------+---------+------+----+------------------
C   11     6e44.492a.f97a   dynamic  NA         F      F    nve1(10.0.0.22)
C   11     ee4a.dcfb.cdfc   dynamic  NA         F      F    nve1(10.0.0.33)
```
