# Piano di coordinamento degli sciami

## Branching model
- `main`: stabile, nessuna scrittura diretta dei worker.
- `lia`: orchestrazione e integrazione.
- `agents/ragno/web-audit`: rilevazione delle sole risorse pubbliche e inventario UI.
- `agents/ragno/android-audit`: analisi **statica** APK con metadati sanitizzati.
- `agents/ragno/rust-core`: workspace Cargo, moduli Rust, test.
- `agents/ragno/rea-mcp`: strumenti REA, configurazioni di collegamento MCP, documentazione.
- `agents/ale/frontend`: frontend originale e componentizzato.
- `agents/ragno/security`: verifiche, permessi, supply-chain, controllo qualità.

Creare nuovi rami `agents/<host>/<ruolo>` solo per lavori assegnati, sempre dalla testa aggiornata di `lia`. Ogni ramo ha un singolo responsabile operativo.

## Procedura per un nuovo agente
1. Leggere `AGENTS.md`, `ARCHITECTURE.md`, questo documento e issue assegnata.
2. Clonare su cartella separata, eseguire `git fetch origin`.
3. Creare un ramo UNICO da `origin/lia`: `git switch -c agents/<host>/<ruolo> origin/lia`.
4. Cambiare solo i file della propria area e includere test/documentazione.
5. Pubblicare il ramo, aprire **PR verso `lia`** con report dei controlli.
6. Attendere review di Lia prima dell'integrazione. Non effettuare cherry-pick di modifiche di altri agenti senza coordinamento.

## Proprietà e collisioni
- `AGENTS.md`, `ARCHITECTURE.md`, `docs/SWARM.md`, `.github/*`: coordinatore Lia.
- `research/web/`: web-audit.
- `research/android/`: android-audit (mai file APK grezzi).
- `crates/core/`: rust-core.
- `crates/api/`, `crates/adapters/`: assegnazione esplicita prima dell'intervento.
- `frontend/`: frontend.
- `deploy/`, `crates/security/`: security/deploy con revisione.
- File condivisi `Cargo.toml` e `Cargo.lock`: un integratore per volta.

## Accettazione
Una PR è integrabile solo se: diff confinato, test documentati, nessun segreto o APK, dipendenze verificate, nessuna funzione di tracciamento occulto e nessun cambio infrastrutturale non autorizzato.

## Stato iniziale
- RAGNO: Docker con REA 6.8.0 testato (`mcp doctor` healthy), Copilot CLI e Codex CLI installati.
- Accesso MCP REA dai singoli agenti e componenti opzionali (JADX/Chromium) da verificare.
- Repository pubblico: non caricare copie integrali del sito o asset di terzi privi di licenza.

## Promemoria per il coordinatore
Controllare nuove PR, conflitti, test e documentazione quando viene richiesto un ciclo di integrazione. Non presumere che il coordinamento avvenga autonomamente tra una sessione e la successiva.
