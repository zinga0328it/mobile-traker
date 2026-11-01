# Architettura — replica Rust-first, solo Tor su RAGNO

## Vincolo prioritario
L'unica differenza funzionale che il committente chiede è sostituire il **normale accesso web** con un **Onion Service Tor (.onion)**. Non cambiare di iniziativa le schermate, il design o i flussi leciti della nuova implementazione rispetto al riferimento. L'APK di riferimento non va modificato. Codice scritto ex novo, modulare, più leggero, prevalentemente Rust.

## Schema del servizio (un solo PC)
```text
Utente Tor Browser / client Android con trasporto Tor
           |
           v
     indirizzo .onion
           |
    Tor Onion Service
           |
           v
  Apache2 127.0.0.1:PORTA_WEB
           |
           v
  API Rust 127.0.0.1:PORTA_API
           |
     moduli Rust + storage
           |
        RAGNO
```
- Nessun hostname pubblico clearnet richiesto, nessun altro PC come server.
- Se il client Android necessita di accedere al servizio .onion, prevedere trasporto Tor compatibile **solo per la nuova implementazione**. L'APK originale resta in sola lettura; non assumere che supporti .onion senza adattamento.
- Mantenere nftables modulare esistente e raggiungibilità SSH/Yggdrasil.

## Struttura del repository
```text
Cargo.toml
crates/
  core/            # modelli e logica dominio
  api/             # Axum, handler, routing
  adapters/        # integrazioni sostituibili
  storage/         # persistenza
  security/        # autenticazione, consenso, audit
  mobile-shared/   # logica Rust per Android
frontend/
  components/      # schermate/componenti fedeli ai riferimenti leciti
  styles/          # stile organizzato per moduli
  assets/          # solo asset propri/licenziati
android/           # nuova app, sottile bridge piattaforma se necessario
config/            # TOML senza segreti
deploy/
  apache2/         # reverse proxy locale
  tor/             # Onion Service sullo stesso RAGNO
  nftables/        # eventuali moduli aggiuntivi, mai sostituzione globale
  systemd/         # unità locali
research/
  web/             # inventario delle pagine e dei flussi pubblici
  android/         # esiti sanitizzati di audit statico; NON APK
reports/           # report Markdown per ogni agente
tests/
docs/
  SWARM.md
  REPLICA-SPEC.md
```

## Criteri tecnici
1. Matrice di corrispondenza tra interfaccia originale e schermate/endpoint leciti ricostruiti; nessun redesign.
2. Build/test dei moduli Rust, più misure concrete per dimostrare alleggerimento.
3. Audit nftables **prima** di qualsiasi intervento: inventario di include e regole, backup, `nft -c`, rollback, test SSH/Yggdrasil.
4. Apache2 e Rust solo su loopback; accesso web esterno solo tramite Tor .onion.
5. Niente copia di codice/asset di terzi né funzionalità di sorveglianza occulta.

## GitHub
Worker `agents/ragno/<ruolo>` in worktree distinti → PR e `reports/<ruolo>.md` → `lia` (revisione) → `main` (approvazione).