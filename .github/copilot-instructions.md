# Copilot CLI — istruzioni prioritarie RAGNO

1. Leggi su `origin/lia` `AGENTS.md`, `ARCHITECTURE.md`, `docs/SWARM.md`, `docs/REPLICA-SPEC.md`. Esegui `git fetch origin`; se `~/laboratorio/mobile-traker-git` esiste, riutilizzarlo.
2. Il committente **non vuole un'app ridisegnata né funzioni inventate**: l'obiettivo è la massima fedeltà lecita a interfaccia/flussi del riferimento. **Unica modifica funzionale richiesta: sostituire l'accesso web normale con Tor .onion**. Implementazione indipendente, più leggera, Rust-first, completamente modulare.
3. **NON modificare l'APK originale**; studialo solo staticamente se autorizzato. Per la nuova app il solo adattamento di rete è il collegamento al servizio .onion con client/trasporto Tor; evitare di presumere che l'APK originale supporti Tor.
4. **Solo RAGNO** per sviluppo, sito, API, Tor, Android, test; niente altri PC e niente Docker imposto. Apache2 e Rust ascoltano su loopback, onion service è l'unico accesso web esterno.
5. Studia prima l'attuale nftables modulare, senza sostituzioni o `nft flush ruleset`; backup, test e rollback prima di cambiare regole. Non interrompere SSH/Yggdrasil.
6. Ogni subagente lavora in worktree/branch `agents/ragno/<ruolo>` da `origin/lia`, pubblica `reports/<ruolo>.md` e apre PR a `lia`. Niente commit/merge automatici su `main`.
7. Nessun APK, decompilato, codice/asset proprietario, credenziale o dato personale su GitHub pubblico. Non realizzare sorveglianza nascosta o raccolta dati senza consenso.
8. Mostra build, test e comparazioni effettive. Non dichiarare risultati non verificati.
