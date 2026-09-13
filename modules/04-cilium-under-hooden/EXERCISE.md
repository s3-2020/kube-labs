# Modul 4 — Cilium under motorhjelmen
Forudsætning: lab-tools netshoot (2 pods på FORSKELLIGE noder — bevis det først med -o wide).
1. apply svc-lb.yaml. Hvor kommer EXTERNAL-IP fra, og hvem ANNONCERER den (ARP)? Find kilde: kubectl -n kube-system logs ds/cilium | grep -iE "announce|lan-pool" | tail -5. Test: curl eksternt og internt.
2. Pod A (k1) → pod B (k2). Kør på BEGGE noder (via SSH): `sudo tcpdump -ni any port 8472` mens du curl'er fra A til B. Hvad ser du (VXLAN-kapsling: yderst node-IP:8472, indeni pod→pod)?
3. Samme trafik — kig på poddets eget interface: `sudo tcpdump -ni <netshoot> ...`? Forklar forskellen (ovl-læsbar på pod-side, kapslet på tråden).
4. Cilium's øjne: kubectl -n kube-system exec ds/cilium -- cilium-dbg status | head; find policy-verdikt for en lab-trafik: exec cilium-pod-på-destinationens-node -- cilium-dbg monitor --related-to <endpoint-id> (find id: cilium-dbg endpoint list | grep whoami). Uden netpol: hvad siger monitor? (FORBİD? tilladt?)
5. SLUK: delete -f . (LB-IP frigives til poolen — bevis med kubectl get ciliumloadbalancerippool lan-pool).
