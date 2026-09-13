# kube-labs — netværks- & fejlfindings-lab (kung.dk clusteret)

Lær at tænke som en CKA: DNS → Services → NetworkPolicy → Gateway → Cilium → chaos.
Gamle skole først (`exec`, `describe`, `events`, `tcpdump`), Hubble som bonus.

## Kom i gang
Base-platformen (namespaces, whoami, netshoot) deployes af Flux og er ALTID tændt —
du skal ikke gøre noget for at starte. Tjek at den lever:
```bash
kubectl get ns -l team=lab                  # tre Active
kubectl -n lab-tools get pods               # to netshoots
kubectl -n lab-tools exec -it deploy/netshoot -- bash    # din værktøjs-pod
```
Pr. modul:
```bash
kubectl apply -f modules/0X-.../     # tænd øvelsen (undtagen modul 1, den kører på basen alene)
# ...løs EXERCISE.md, kig først i SOLUTION.md bagefter...
kubectl delete -f modules/0X-.../    # sluk igen
```
Chaos-kataer: ét manifest fra `modules/05-chaos/` ad gangen, diagnose fra clusterets status/events/logs, `kubectl delete -f fil.yaml` når den er løst.

## Tænd/sluk hele labben
Labben er PARKET som standard (ingen Flux-tråd i clusteret). Genstart:
```bash
git -C ~/k8s-gitops checkout lab-wiring -- clusters/home/apps-sync/kube-labs-gitrepository.yaml clusters/home/apps-sync/lab-kustomization.yaml
# genindsæt de to linjer i clusters/home/apps-sync/kustomization.yaml, commit + push
```
Parkér igen (rigtig rækkefølge — Flux ellers genskaber): `kubectl delete kustomization lab -n flux-system` (finalizer rydder indhold), slet `gitrepository kube-labs`, fjern de to filer + referencer i k8s-gitops, commit + push.

## Røde linjer (alt uden for labben er produktion)
- Alle ressourcer lever kun i navneområderne `lab-*` — ingen netpol/route/ændring uden for dem.
- Lab-ressourcer labels `team: lab` — genoprettelse: se nedenunder.
- Bræk ALDRIG `default/kubernetes/kube-system/kube-flannel...` — fejlfindes med læsning, ikke skriving.

## Reset (nødbremse, virker altid)
```bash
flux delete kustomization lab -n flux-system   # GitOps-tråd væk
kubectl delete ns -l team=lab   # alt indhold væk (lab-* namespaces bærer label)
# genskab:_flux_/_apply_k8s-gitops_/_lab_kustomization_igen
```

## Moduler
1. `modules/01-dns-services` — DNS, ClusterIP, endpoints, FQDN-former
2. `modules/02-networkpolicy` — default-deny, cross-namespace, fejlfinding af blocked traffic
3. `modules/03-gateway-tls` — HTTPRoute, TLS, intern CA
4. `modules/04-cilium-under-pooden` — VXLAN, deny-verdicts, L2-annoncering
5. `modules/05-chaos/` — 5 katas (imagepull, pending-PVC, OOM, readiness-503, netpol-lockout) — apply én ad gangen
