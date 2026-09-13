# Facit modul 1
1. ClusterIP'en (fx 10.43.65.87). CoreDNS svarer på svc-FQDN.
2. `/etc/resolv.conf` search-liste: `lab-tools.svc.cluster.local svc.cluster.local cluster.local` — derfor virker `whoami.lab-red` men IKKE `whoami` fra en anden namespace (løses mod `<ns>.svc…` i poddns'ens egen ns — her: lab-tools).
3. Baglæns DNS for pod-IP'er findes ikke (10.42.x er ikke i DNS zonen); kun svc-navne er registreret.
4. Round-robin blandt de 2 Ready-pods. Der router: **kube-proxy-replacement i Cilium eBPF** (ingen iptables-regler at finde!) — ingen proxy rører pakken.
5. Étt svar: kun 1 replica.
6. EndpointsSlice tom → connection refused (ikke DNS-fejl!). Service = navngivnings-lag; endpoints = det virkelige mål.
7. Selector matcher ingen pods → endpoints tom → trafik dør, pods kører videre "sundt". (Lære: Ready pod ≠ tilgængelig service.)
8. KORT navn løses kun inde i samme namespace. Hypotese-bevis: search-listen starter med EGEN namespace, så `whoami.lab-blue` + search → `whoami.lab-blue.lab-tools.svc.cluster.local` (NXDOMAIN). FQDN virker.
9. De 5 trin er dit krydderurte-livlangt værktøj.
