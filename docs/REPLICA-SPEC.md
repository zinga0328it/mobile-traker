# Specifica: una sola differenza, sito via Tor

## Cosa vuole il committente
- Replicare **fedelmente le parti lecite osservabili** del sito e dell'esperienza dell'app di riferimento.
- Non cambiare di iniziativa grafica, struttura, pagine, navigazione o flussi.
- Scrivere codice nuovo, **più leggero e modulare, prevalentemente Rust**: è un vincolo di implementazione interno, non un redesign.
- **L'unica differenza di accesso al servizio deve essere Tor .onion, al posto del normale sito web clearnet.** Lo stesso PC **RAGNO** esegue frontend, backend, Apache2, Tor e strumenti di sviluppo.

## APK
- Il file APK originale **non va modificato, patchato, ricompilato né pubblicato**. Usarlo solo come riferimento per audit statico locale autorizzato.
- Se viene costruita una nuova applicazione, mantenere esperienza e flussi leciti il più fedelmente possibile.
- Passare a un endpoint `.onion` richiede che il **nuovo client** usi un trasporto Tor compatibile. Non affermare che l'APK originale possa connettersi senza adattamenti.

## Architettura
`Client Tor → Onion Service (RAGNO) → Apache2 loopback → API Rust loopback → moduli/storage`.
- Nessun sito clearnet aggiuntivo, nessun altro PC server.
- Salvaguardare nftables modulare e l'accesso SSH/Yggdrasil.
- Ogni agente documenta fedeltà, differenze necessarie, test e risultati in `reports/<ruolo>.md` su GitHub (PR verso `lia`).

## Limiti
Implementazione indipendente: no duplicazione di codice, marchi o asset proprietari; no funzionalità di sorveglianza occulta, esfiltrazione o raccolta dati senza consenso. Queste differenze devono essere esplicitate, non nascoste nel report.
