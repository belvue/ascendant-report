# Login and hosting layers (reference)

```
                    +------------------+
                    |  OVH 147.135.x   |
                    |  ascendanteq.com |
                    |  patch CDN       |
                    +--------+---------+
                             |
              HTTPS / patch zip / eqhost.txt
                             |
                    +--------v---------+
                    |  Player client   |
                    +--------+---------+
                             |
         +-------------------+-------------------+
         |                                       |
         v                                       v
+----------------------+              +----------------------+
| login.eqemulator.dev |              | eqemu.ascendanteq  |
| operator login DB    |              | .com -> 99.42.xxx.xxx |
| (PC-010 eqhost)      |              | AT&T residential   |
+----------------------+              +----------------------+
```

Probe date: 2026-06-12. See report chapters 03, 04, 11.
