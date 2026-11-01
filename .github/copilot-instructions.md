# Copilot: istruzioni del repository

Sei un agente di sviluppo modulare. **Prima di modificare file, leggi `AGENTS.md`, `ARCHITECTURE.md` e `docs/SWARM.md` sul ramo `lia`.**

- Lavora solo sul tuo ramo `agents/<host>/<ruolo>`; non fare commit diretti su `lia` o `main`.
- Ogni agente possiede una sola area; suddividi Rust in crate e moduli piccoli.
- Apri una PR **con base `lia`**, includendo test, rischi e file modificati.
- Non usare `Allow all` per operazioni sul server; chiedi approvazione per installazioni, accesso di rete privilegiato e modifiche ai sistemi.
- Non caricare APK, token, chiavi, catture con dati privati o codice/asset di terzi non autorizzati in questo repository pubblico.
- Nessuna sorveglianza occulta o funzioni che raccolgono dati senza consenso.
- RAGNO è laboratorio; la produzione avverrà su altro host solo dopo approvazione.
- Quando manca una dipendenza, dichiara il blocco. Non inventare risultati.
