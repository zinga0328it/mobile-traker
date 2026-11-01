# Architettura proposta — moduli indipendenti

## Obiettivo
Prototipo originale e lecito, basato su analisi autorizzata di elementi pubblici di UI e di funzionalità trasparenti. Niente duplicazione di codice proprietario o funzionalità di sorveglianza occulta.

## Struttura (da creare progressivamente sui rami operativi)

```text
mobile-traker/
  AGENTS.md
  ARCHITECTURE.md
  Cargo.toml              # workspace Rust
  crates/
    core/                 # tipi, contratti, logica di dominio
    api/                  # Axum, route e handler HTTP
    adapters/             # integrazioni intercambiabili
    security/             # autenticazione, autorizzazioni, audit
  frontend/
    components/           # componenti UI autonomi
    styles/               # CSS organizzato
    assets/               # solo asset originali/licenziati
  config/                 # esempi TOML senza segreti
  deploy/
    apache2/              # reverse proxy del futuro server
    systemd/              # servizi non privilegiati
    nftables/             # esempi modulari, mai applicati automaticamente
    tor/                  # configurazione onion opzionale, inattiva
  research/
    web/                  # note pubbliche/sanitizzate
    android/              # metadati e risultati sanitizzati, NON APK
  tests/
  docs/
    SWARM.md
```

## Separazione delle macchine

```text
ALE / altri PC  -->  GitHub: agent/<host>/<ruolo>
RAGNO (REA, Codex, Copilot, test) --> GitHub: agents/ragno/<ruolo>
                  | Pull request + verifica
                  v
           GitHub: lia (integrazione)
                  | approvazione umana + CI
                  v
           GitHub: main (stabile)
                  | pacchetto versionato e verificato
                  v
        HOST PRODUZIONE FUTURO (Apache2 + Rust)
                  + Tor Onion Service facoltativo
```

## Contratti tra moduli
- `core`: nessuna dipendenza da HTTP, database o ambiente del server.
- `api`: espone solo endpoint documentati, usa interfacce di `core`.
- `adapters`: IO e provider incapsulati; config e test mockabili.
- `security`: autorizzazioni esplicite e log senza dati sensibili.
- Frontend indipendente dal framework: API versionate, asset separati.
- Nessun modulo deve accedere direttamente a Docker socket, host filesystem o reti di produzione.

## Rilascio
Build verificabile -> test unitari/integrati -> review sicurezza -> PR a `lia` -> approvazione utente -> PR verso `main` -> tag e release -> deploy su host separato. Niente deploy automatico da branch di ricerca.
