# Modul 3 — Gateway, HTTPRoute & TLS
1. apply -f . → følg HTTPRoute'ens status (conditions!) indtil Programmed. Hvor ser du om parentRefs faktisk virkede?
2. `curl -s https://lab.kung.dk` — hvem besvarer TLS? find certifikatets udsteder: `echo | openssl s_client -connect lab.kung.dk:443 -servername lab.kung.dk 2>/dev/null | openssl x509 -noout -issuer -dates`
3. Fejlfindings-scenarie: ændr backendRefs til et service-navn der ikke findes. Hvad siger HTTPRoute-status, og hvad sker der reelt når du curl'er (503 vs connection reset — hvorfor)?
4. Tilføj to rules: `/blue` → whoami.lab-blue, `/red` → whoami.lab-red (navne på tværs kræver at lab-red også får en reference — hvordan løser clusteret det? kig på ReferenceGrant).
5. SLUK: delete -f . og bevis at lab.kung.dk er død igen.
