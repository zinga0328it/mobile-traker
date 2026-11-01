# Copilot CLI — istruzioni prioritarie RAGNO

1. Leggi su `origin/lia` `AGENTS.md`, `ARCHITECTURE.md`, `docs/SWARM.md`, `docs/REPLICA-SPEC.md`. Esegui `git fetch origin`; se `~/laboratorio/mobile-traker-git` esiste, riutilizzarlo.
2. Il committente **non vuole un'app ridisegnata né funzioni inventate**: l'obiettivo è la massima fedeltà lecita a interfaccia/flussi del riferimento. **Unica modifica funzionale richiesta: sostituire l'accesso web normale con Tor .onion**. Implementazione indipendente, più leggera, Rust-first, completamente modulare.
3. **NON modificare l'APK originale**; studialo solo staticamente se autorizzato. Per la nuova app il solo adattamento di rete è il collegamento al servizio .onion con client/trasporto Tor; evitare di presumere che l'APK originale supporti Tor.
4. **Solo RAGNO** per sviluppo, sito, API, Tor, Android, test; niente altri PC e niente Docker imposto. Apache2 e Rust ascoltano su loopback, onion service è l'unico accesso web esterno.
5. **Coordina Copilot CLI con Codex CLI** per sviluppo e revisione; ogni agente resta su RAGNO. Android-build deve generare l'APK **nuovo** su RAGNO e provarlo su telefono Android autorizzato **collegato via USB allo stesso RAGNO** (ADB); il telefono e' solo client di prova. Verificare il trasporto Tor verso l'onion service sullo stesso RAGNO. Non cambiare/installare l'APK originale, non inventare test se non c'e' un dispositivo disponibile.
7. Studia prima l'attuale nftables modulare, senza sostituzioni o `nft flush ruleset`; backup, test e rollback prima di cambiare regole. Non interrompere SSH/Yggdrasil.
7. Ogni subagente lavora in worktree/branch `agents/ragno/<ruolo>` da `origin/lia`, pubblica `reports/<ruolo>.md` e apre PR a `lia`. Niente commit/merge automatici su `main`.
8. Nessun APK, decompilato, codice/asset proprietario, credenziale o dato personale su GitHub pubblico. Non realizzare sorveglianza nascosta o raccolta dati senza consenso.
9. Mostra build, test e comparazioni effettive. Non dichiarare risultati non verificati.
