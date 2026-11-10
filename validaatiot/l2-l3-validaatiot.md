# L2 ja L3 -tasojen validointi

### 1. VNI-to-VNI reititys (Tenant A)

```text
app-a-11-pve2:~# ping 172.16.1.103
PING 172.16.1.103 (172.16.1.103): 56 data bytes
64 bytes from 172.16.1.103: seq=0 ttl=64 time=6.590 ms

app-a-11-pve2:~# ping 172.16.0.101
PING 172.16.0.101 (172.16.0.101): 56 data bytes
64 bytes from 172.16.0.101: seq=0 ttl=62 time=7.633 ms
```

### 2. Kytkimen sisäinen ping-testi

```text
SW2# ping 172.16.1.102 vrf TENANT-A
PING 172.16.1.102 (172.16.1.102): 56 data bytes
64 bytes from 172.16.1.102: icmp_seq=0 ttl=63 time=3.59 ms
```

### 3. Asiakasverkkojen eristystesti
Virtuaalikone `web-a-10-pve1` (`172.16.0.101`, Tenant A) pystyy pingaamaan onnistuneesti oman reitityskontekstinsa laitteita, mutta epäonnistuu pingaamaan osoitetta `172.16.0.103` joka kuuluu **TENANT-B**:n Web-palvelimelle PVE3-palvelimella.

```text
web-a-10-pve1:~# ping 172.16.1.103
PING 172.16.1.103 (172.16.1.103): 56 data bytes
64 bytes from 172.16.1.103: seq=0 ttl=62 time=14.432 ms

web-a-10-pve1:~# ping 172.16.0.103
PING 172.16.0.103 (172.16.0.103): 56 data bytes
--- 172.16.0.103 ping statistics ---
5 packets transmitted, 0 packets received, 100% packet loss
```
