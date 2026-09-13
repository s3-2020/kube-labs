# Facit modul 4
1. lan-pool (.60-.79, L2 mode). Cilium's LB-ipam tildeler; announcements via L2 (ARP-gratıs på switchen — ingen VRRP her; VIP er keepalived, LB-IP'er er Cilium).
2. UDP 8472 mellem NODE-IP'er, indeni: pod-IP'er — overlay. MTU: pod-net 1450 (8472-header).
3. På pod-interface: ren pod-trafik; pcap på eth er kun kapslet. Cilium vxcap... pointen: lagene er reelle — fejlfind dér hvor brodden sker.
4. monitor viser TRACE_TO_STACK/ALLOWED (allow-all uden netpol). Med netpol fra modul 2: DROP-verdict med policy-id. Hubble er præcis denne strøm, graphicalt.
5. IP returnerer til poolens allokeringsliste.
