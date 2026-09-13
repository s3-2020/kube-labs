# kube-labs — netværks- & fejlfindings-lab (kung.dk clusteret)

Lær at tænke som en CKA: DNS → Services → NetworkPolicy → Gateway → Cilium → chaos.
Gamle skole først (`exec`, `describe`, `events`, `tcpdump`), Hubble som bonus.

## Røde linjer (alt uden for labben er produktion)
- Alle ressourcer lever kun i navneområderne `lab-*` — ingen netpol/route/ændring uden for dem.
- Lab-ressourcer labels `team: lab` — genoprettelse: se nedenunder.
- Bræk ALDRIG `default/kubernetes/kube-system/kube-flannel...` — fejlfindes med læsning, ikke skriving.

## Reset (nødbremse, virker altid)
```bash
flux delete kustomization lab -n flux-system   # GitOps-tråd væk
kubectl delete ns lab-red lab-blue lab-tools   # alt indhold væk
# genskab:_flux_/_apply_k8s-gitops_/_lab_kustomization_igen
```

## Moduler
1. `modules/01-dns-services` — DNS, ClusterIP, endpoints, FQDN-former
2. `modules/02-networkpolicy` — default-deny, cross-namespace, fejlfinding af blocked traffic
3. `modules/03-gateway-tls` — HTTPRoute, TLS, intern CA
4. `modules/04-cilium-under-pooden` — VXLAN, deny-verdicts, L2-annoncering
5. `modules/05-chaos` — bevidst ødelagte workloads i `broken/`
