# Specifica: una sola differenza, sito via Tor

## Cosa vuole il committente
- Replicare **fedelmente le parti lecite osservabili** del sito e dell'esperienza dell'app di riferimento.
- Non cambiare di iniziativa grafica, struttura, pagine, navigazione o flussi.
- Scrivere codice nuovo, **più leggero e modulare, prevalentemente Rust**: è un vincolo di implementazione interno, non un redesign.
- **L'unica differenza di accesso al servizio deve essere Tor .onion, al posto del normale sito web clearnet.** Lo stesso PC **RAGNO** esegue frontend, backend, Apache2, Tor e strumenti di sviluppo.

## APK
- Il file APK originale **non va modificato, patchato, ricompilato né pubblicato**. Usarlo solo come riferimento per audit statico locale autorizzato.
- **Riscrivere la nuova applicazione Android è obbligatorio**: non basta compilare uno scheletro né documentare l'APK originale. Implementare in modo autonomo l'esperienza e i flussi leciti del riferimento, senza redesign arbitrarie.
- Passare a un endpoint `.onion` richiede che il **nuovo client** usi un trasporto Tor compatibile. Non affermare che l'APK originale possa connettersi senza adattamenti.

## Nuova app Android COMPLETA e collegata al server su RAGNO
- **Copilot + Codex devono riscrivere e integrare davvero la nuova app Android**, non limitarsi alla build di un APK. L'app deve dialogare tramite API con il server Rust ospitato su RAGNO.
- RAGNO compila il nuovo APK Android. Un telefono Android di laboratorio collegato a RAGNO via USB serve per installazione e debug ADB, non come server. Nessun secondo PC o server.
- Verificare `adb devices -l` prima di installare e registrare il dispositivo autorizzato nei report senza pubblicarne identificativi.
- Testare un percorso **end-to-end bidirezionale**, con dati di laboratorio: nuova app → Tor → servizio `.onion` su RAGNO → Apache2 → API Rust → storage → risposta all'app. Non basta verificare l'apertura della pagina o una risposta ping; l'interfaccia Android deve mostrare il risultato della chiamata.
- L'APK originale di riferimento NON va modificato o installato come parte della replica.
- Se il dispositivo non e' presente, compilare e preparare il pacchetto, ma dichiarare il test di installazione non effettuato.

## Architettura
`Client Tor → Onion Service (RAGNO) → Apache2 loopback → API Rust loopback → moduli/storage`.
- Nessun sito clearnet aggiuntivo, nessun altro PC server.
- Salvaguardare nftables modulare e l'accesso SSH/Yggdrasil.
- Ogni agente documenta fedeltà, differenze necessarie, test e risultati in `reports/<ruolo>.md` su GitHub (PR verso `lia`).

## Limiti
Implementazione indipendente: no duplicazione di codice, marchi o asset proprietari; no funzionalità di sorveglianza occulta, esfiltrazione o raccolta dati senza consenso. Queste differenze devono essere esplicitate, non nascoste nel report.
