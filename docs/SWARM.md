# Sciame su RAGNO — lavoro parallelo e report GitHub

**Vincolo di prodotto:** massima fedeltà lecita di sito e app di riferimento, senza redesign; la sola differenza funzionale richiesta è **sito e API via Tor .onion anziché web clearnet**. L'APK originale non si modifica. Codice nuovo, modulare, prevalentemente Rust.

## Distribuzione, solo su RAGNO
| Agente | Branch da `origin/lia` | Area + report |
|---|---|---|
| Web audit | `agents/ragno/web-audit` | `research/web/`, `reports/web-audit.md` |
| Android audit | `agents/ragno/android-audit` | `research/android/`, `reports/android-audit.md` |
| Rust core | `agents/ragno/rust-core` | `crates/core/`, `crates/mobile-shared/`, `reports/rust-core.md` |
| API + storage | `agents/ragno/api-storage` | `crates/api/`, `crates/storage/`, `crates/adapters/`, `reports/api-storage.md` |
| Frontend | `agents/ragno/frontend` | `frontend/`, `reports/frontend.md` |
| Riscrittura e integrazione Android | `agents/ragno/android-build` | `android/`, client API/Tor, test con backend RAGNO, `reports/android-build.md` |
| Apache2/Tor/nftables | `agents/ragno/infra` | `deploy/`, `reports/infra.md` |
| Sicurezza/test | `agents/ragno/security-test` | `crates/security/`, `tests/`, `reports/security-test.md` |
| Integrazione | `agents/ragno/integration` | `Cargo.toml`, `Cargo.lock`, file condivisi, `reports/integration.md` |
| REA/MCP opzionale | `agents/ragno/rea-mcp` | `research/rea/`, `reports/rea-mcp.md` |

## Vincolo Android e responsabilita' degli agenti
- `android-build`: **riscrivere concretamente la nuova app Android**, con moduli, UI e chiamate API, e integrarla con i servizi Rust su RAGNO attraverso Tor `.onion`. Compilare su RAGNO, usare ADB/USB per installarla esclusivamente sul telefono di test autorizzato e verificare scambio bidirezionale di dati di laboratorio con il backend. **Un APK che si installa ma non comunica non conclude il lavoro.**
- `android-audit`: studiare l'APK originale in sola lettura; non modificarlo e non pubblicarlo.
- `infra`: tutto il servizio `.onion`, Tor, Apache2 e API Rust risiede su **RAGNO**, non sul telefono.
- `integration`: eseguire test **end-to-end app ↔ Tor .onion ↔ Apache2 ↔ API Rust ↔ storage** sul solo RAGNO, insieme a `android-build` e `infra`; registrare il risultato realmente visibile nell'app, gli errori e i test non eseguiti. Pubblicare `reports/integration.md`.
- **Copilot CLI coordina gli agenti; Codex CLI esegue i compiti di sviluppo e revisione Rust**, ciascuno in proprio branch/worktree e con report GitHub. Non presumere che possano operare fuori dalla sessione attiva.

## Protocollo
1. Verificare checkout locale `~/laboratorio/mobile-traker-git` su RAGNO e `git fetch origin`; non clonare di nuovo sopra checkout esistente.
2. Leggere `AGENTS.md`, `ARCHITECTURE.md`, `docs/REPLICA-SPEC.md`, `.github/copilot-instructions.md` da `origin/lia`.
3. Aprire per ogni agente un worktree distinto con ramo proprio creato da `origin/lia`.
4. Produrre **codice testabile**, matrice di fedeltà e report Markdown `reports/<ruolo>.md` per ogni agente.
5. Pubblicare branch + PR verso `lia`; nessun merge automatico su `main`.
6. Integrazione dei file condivisi affidata solo a integration; evitare collisioni tra worker.

## Regole macchina e rete
- Tutto su RAGNO, niente Docker richiesto, Python venv solo se necessario.
- Apache2 locale → Rust locale; esterno solo via Onion Service. Non pubblicare clearnet.
- Studiare il firewall nftables modulare esistente prima di proporre qualsiasi variazione; non azzerarlo o rimpiazzarlo, non compromettere SSH/Yggdrasil.
- APK originale solo analisi statica in sola lettura; niente copia di codice proprietario, segreti, APK o decompilati nel repo pubblico, e niente sorveglianza occulta.
- Test e scostamenti reali documentati; mai inventare prove di funzionamento.
