# Modul 1 — DNS, Services & endpoints

Alt foregår inde fra clusteret. Værksted = netshoot-podden:
```bash
kubectl -n lab-tools exec -it deploy/netshoot -- bash
```
> [!tip] Gamle skole
> Brug `kubectl describe`, `kubectl get events`, endpoints-objekter — ALDRIG loggen af en pod der "bare virker". Fejlfinding er at sammenligne TOSTAND, ikke at læse én ting ad gangen.

## Øvelse 1 — DNS-former
1. Løs `whoami.lab-red.svc.cluster.local` med `dig +short` fra netshoot. Sammenlign med ClusterIP fra `kubectl -n lab-red get svc`.
2. Prøv de kortere former: `whoami.lab-red`, `whoami.lab-red.svc`. Kender podden til search-listen der gør det muligt? (`cat /etc/resolv.conf` inde i podden.)
3. Løs en ren pod-IP baglæns (`dig -x <pod-ip>`). Virker det? Hvorfor/nederlag?

## Øvelse 2 — Hvem svarer?
4. `for i in $(seq 6); do curl -s whoami.lab-red:80 | grep -o "Hostname:.*"; done` — hvorfor skifter svaret, og hvad er det EGENTLIG der router (hvem rører IKKE pakken)?
5. Samme trick mod `whoami.lab-blue:80` — hvad er anderledes (ét svar, hvorfor)?

## Øvelse 3 — Endpoints, service'ens sande ansigt
6. `kubectl -n lab-red get endpointslices` — hvad ser du? Skaler deploy til 0 (`kubectl -n lab-red scale deploy/whoami --replicas=0`). Nu: curl igen. **Forkl forskellen mellem "service findes" og "service har mål"** med dine egne ord.
7. Skru op igen. Ændr Service-selector til `app: whoami-X` (kubectl edit). Hvad sker der med endpoints? Reparer.

## Øvelse 4 — Cross-namespace & fejlfindings-reflekser
8. Fra lab-red-pod: curl `whoami.lab-blue` (KORT navn). Fejler det — forklar hvorfor UDEN at slå op, og bevis din hypotese med resolv.conf.
9. Noget svarer ikke. Din procedure (brug den altid fremover): 1) `get pods` Ready? 2) Service selector→endpoints matcher? 3) port/targetPort? 4) DNS? 5) policy? Kør alle 5 trin på et offer du selv skaber (fejl selector).

Facit: `SOLUTION.md` — kun bagefter!
