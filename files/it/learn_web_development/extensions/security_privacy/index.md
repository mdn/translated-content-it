---
title: Sicurezza e privacy
slug: Learn_web_development/Extensions/Security_privacy
l10n:
  sourceCommit: 1b8805ce680f1fbb9dfbade6a64d4671cd04da80
---

> [!NOTE]
> Come vedrà di seguito, questo modulo è attualmente solo un piano di studi/sillabo, con alcuni collegamenti forniti a guide introduttive. Intendiamo convertirlo in un corso completo in futuro, quando il tempo lo permetterà.

È fondamentale comprendere come è possibile e come si dovrebbe proteggere i propri dati e quelli degli utenti da potenziali aggressori che potrebbero tentare di rubarli. Questo modulo copre sia il rafforzamento dei siti web per renderli più difficili da violare, sia la raccolta dei dati degli utenti in modo rispettoso evitando di tracciarli o condividerli con terzi inappropriati.

## Prerequisiti

Prima di iniziare questo modulo, dovrebbe essere familiare con [HTML](/it/docs/Learn_web_development/Core/Structuring_content) e [CSS](/it/docs/Learn_web_development/Core/Styling_basics).

## Risultati di apprendimento

### 5.1 Nozioni di base su sicurezza e privacy

> [!NOTE]
>
> - Conformarsi a tutti i criteri di questo modulo non farà di uno studente un ingegnere della sicurezza qualificato, ma è comunque importante per gli sviluppatori web comprendere le basi della sicurezza e privacy sul web.
> - È anche importante per gli studenti comprendere che molti problemi di sicurezza sono causati da problemi con il codice lato server, o da una combinazione di codice lato client e lato server. Una buona parte del codice dovrebbe presentare pochissimi rischi di sicurezza, a condizione che il browser svolga correttamente il suo lavoro.

Risultati di apprendimento:

- Comprendere la differenza tra sicurezza e privacy.

- Comprendere il modello HTTP generale da un alto livello.

- Imparare cosa è l'HTTPS e perché è importante.

- Sicurezza del medesimo origine:

  - Perché questo è fondamentale per il web.

  - Modi per aggirarlo in modo sicuro, come il Cross-Origin Resource Sharing (CORS).

- Come vengono memorizzati i cookie e le loro implicazioni sulla sicurezza e la privacy, come il tracciamento.

- Imparare riguardo a situazioni in cui di solito si verificano problemi di sicurezza:

  - Quando si chiede agli utenti di fornire dati sensibili (come password o dati delle carte di credito) e trasmetterli a un server.

  - Quando si richiedono dati da un server.

  - Quando si trasmettono dati tra server (per esempio, se un server richiede dati da un servizio web).

  - Quando si preserva lo stato dell'utente impostando un cookie o altri meccanismi.

- Imparare riguardo alle minacce comuni alla sicurezza e come mitigarle:

  - Cross-site scripting (XSS).

  - Cross-site request forgery (CSRF).

  - Clickjacking.

  - Denial of service (DoS).

- Comprendere lo scopo di altre importanti tecnologie, come:

  - Content Security Policy (CSP).

  - Permissions-Policy.

  - Il modello web per l'attivazione utente di "funzionalità potenti" (noto anche come attivazione transitoria).

### 5.2 Leggi sulla protezione dei dati

Risultati di apprendimento:

- Comprendere i concetti fondamentali relativi alla privacy degli utenti:

  - Informazioni personalmente identificabili (PII).

  - Riservatezza.

  - Tracciamento.

  - Fingerprinting.

- Essere a conoscenza delle leggi sulla privacy a livello regionale, ad esempio:

  - [Regolamento generale sulla protezione dei dati (GDPR)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679&from=EN) (UE).

  - [Legge sulla protezione dei dati 2018](https://www.gov.uk/data-protection) (Regno Unito), gov.uk.

  - [California Consumer Privacy Act (2018)](https://www.oag.ca.gov/privacy/ccpa) (USA, CA), ca.gov.

  - [Children's Online Privacy Protection Rule (COPPA)](https://www.ftc.gov/legal-library/browse/rules/childrens-online-privacy-protection-rule-coppa) (USA), ftc.gov.

- Comprendere come conformarsi a tali leggi, in termini di implementazione pratica.

> [!NOTE]
> Conformarsi ai criteri sopra indicati non richiede agli studenti di diventare esperti legali in materia di leggi sulla privacy, ma dovrebbero comprendere le implicazioni di queste leggi e come influenzano il loro lavoro.

## Risorse

- [Sicurezza sul web](/it/docs/Web/Security)
- [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security)
- [Privacy sul web](/it/docs/Web/Privacy)
- [Guida completa alla conformità GDPR](https://gdpr.eu/), gdpr.eu

## Vedi anche

- [Imparare la Privacy](https://web.dev/learn/privacy/), web.dev (2023)
