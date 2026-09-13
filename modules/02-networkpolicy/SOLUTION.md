# Facit modul 2
1. Timeout (ikke refused) = pakker droppes stille af netværkslaget; refused = intet lys (ingen lytter). Netpol-drop er "stille port". Pod-til-pod i SAMME namespace rammes også — netpol er per-destination, ingen implicit "lokalt OK".
2. DNS er indgående til podden? Nej — men CoreDNS-svaret er RETUR-trafik og staten er fin; problemet er udgående? Nej: DNS-anmodningen er UDgående (ikke reguleret af Ingress-only) — fejlen er at ... vent: podSelector:{} Ingress-only lukker IKKE udgående; dig fejler IKKE af netpol her, men af at ... (rigtigt svar: DNS VIRKER stadig; kun indgående er lukket. Prøv dig frem — faciten er her for at du fandte ud af at skelme. Læs Cilium's docs om default-deny: ingress vs egress.)
3. lab-red-pod→whoami.lab-red blokeret fordi netpol'en kun nævner lab-blue/lab-tools som kilder — "samme namespace" er INGEN ret.
4. Modtagerside: Cilium-egress...nej, INGRESS-drop på destination-poddens eBPF-hook. tæller stiger i cilium_drop{direction="ingress"}.
5. Delete -f . → alle policyer væk → trafik fri. Netpol'er er tilføjelser: FRASÆR = åbent (cluster-default er allow-all uden for produktion).
