# cisco-nexus-bgp-evpn-vxlan
Multi-Tenant VXLAN fabric -konesaliverkon toteutus Spine-Leaf-arkkitehtuurilla ja Ciscon Nexus 9000v -virtuaalikytkimillä. Toteutettu Kaakkois-Suomen ammattikorkeakoulun VirtualLab-alustalla.

## Verkon topologia

![Verkon topologia](topologia/spine-leaf-vxlan.png)

Verkko on rakennettu hyödyntäen Spine Leaf -topologiaa. Leaf-tason Nexus 9000v -kytkimet on yhdistetty Trunk linkeillä Proxmox Virtual Environment -palvelimiin. Linkit Spine-tasolle ovat L3 reititettyjä portteja.

## OSPFv2 Underlay

Fyysinen **Underlay**-verkko on rakennettu hyödyntäen OSPFv2-protokollaa. Prosessi toimii yhdellä **OSPF** alueella (`BACKBONE`) ja sen toimintaa on optimoitu määrittämällä jokaisen siihen liitetyn linkin **OSPF** verkon tyypiksi **point-to-point**. Tällä tavoin **OSPF**:n **Link State Tietokantaan** tallennetaan vain **Router LSA**-tietoja, jolloin tietokannan koko pysyy maltillisena. Jokaisen kytkimen **OSPF Router-Id**ksi on konfiguroitu **loopback 0** -interface. Fyysiset linkit hyödyntävät `ip unnumbered loopback 0` -menetelmää. Lisäksi jokaisen linkin **MTU**-arvo on nostettu 1 550 tavuun, jotta **VXLAN** enkapsulointi ei aiheuta pakettien fragmentointia.

## BGP EVPN Control-plane ja iBGP

**VXLAN**-fabric on konfiguroitu hyödyntämään **BGP EVPN** -protokollaa päätelaitteiden MAC-osoitteiden (`EVPN Type 2 MAC route`) ja IPv4-osoitteiden (`EVPN Type 2 MAC-IP Route`) mainostamiseen muille **VTEP**-laitteille **BGP**-pohjaisina päivityksinä. Yksi ratkaisun hyödyistä on se, että se korvaa **Flood and Learn** toimintatavan. Koska päätelaitteiden MAC- ja IPv4-osoitteet ovat **VTEP**-laitteiden tiedossa, niiden ei tarvitse ohjata **ARP Request** tai **Unknown Unicast**-liikennettä **overlay**-verkon yli broadcast-liikenteenä.

Kytkimet hyödyntävät **iBGP**-protokollaa edellä mainittujen tietojen mainostamiseen. Päivityslooppien estämiseen kehitetty **iBGP Split Horizon** -sääntö määrittelee, että jokaisen **iBGP**-laitteen tulee muodostaa peer-suhde jokaisen **iBGP**-laitteen kanssa. Tämän kokoisessa verkossa peer-suhteiden määrä ei ole haitaksi, mutta verkon kasvaessa peer-suhteiden määrä muodostaisi nopeasti ongelman. Tämän takia verkon Spine-tason kytkimet on konfiguroitu toimimaan **Route Reflector** -roolissa.
