# Facit modul 3
1. `kubectl -n lab-blue describe httproute lab` — conditions Accepted/Programmed + ResolvedRefs; en forkert service viser sig her FØRST.
2. Traefik (Gateway på .60) med letsencrypt-cert (acme HTTP-01 via samme gateway). Issuer = LE.
3. ResolvedRefs=False på ruten; curl → 503 fra Traefik (den hører stadig værts-navnet, mangler target) — reset ville betyde host-match fejler.
4. Cross-namespace backendRefs kræver ReferenceGrant i lab-red der giver lab-blue lov (hvis ikke tilladt globalt — test først uden!).
5. Route+cert borte → 404/reset. Cert lever videre til cert-manager sletter den (prune-lignende adfærd: cert i samme ns slettet med delete -f).
