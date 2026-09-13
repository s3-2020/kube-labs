# Modul 2 — NetworkPolicy: deny, allow, og hvorfor alting er timeout
Forudsætning: modul 1. Arbejd altid med at SKITSE trafikken først: kilde-pod → destination, port, og HVEM afgør (netpol findes kun der destinationens pod vælges).

1. `kubectl apply -f netpol.yaml`. Test fra lab-tools: `curl whoami.lab-red` (med --max-time 5!) og fra lab-red selv pod-til-pod. Hvad fejler og hvorfor er svaret FØRST efter ~5 s (timeout ≠ refused)?
2. Hvorfor dør selv DNS nu? Test: `dig whoami.lab-red` inde i en lab-red-pod. Ret med allow-dns.yaml og verifiér.
3. apply allow-from-blue.yaml. Test fra lab-blue (OK), lab-tools (OK), lab-red-pod→egens namespace-service (FORVENTET BLOKERET — hvorfor?).
4. Kilder du IKKE har tilladt: hvor i packet-flowet droppes de — afsender- eller modtagerside? Bevis med tcpdump fra netshoot på destinationens node (Cilium-drop tæller: kubectl -n kube-system exec ds/cilium -- cilium-dbg metrics all 2>/dev/null | grep -E "drop|denied").
5. SLUK ALT: kubectl delete -f . — og bevis at trafikken er tilbage.
Rød linje: policyer med podSelector:{} i lab-red/blue — ALDRIG i produktions-navneområder.
