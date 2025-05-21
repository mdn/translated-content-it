---
title: Sicurezza e privacy
slug: Learn_web_development/Extensions/Security_privacy
l10n:
  sourceCommit: 1b8805ce680f1fbb9dfbade6a64d4671cd04da80
---

> [!NOTE]
> Come vedrai di seguito, questo modulo è attualmente solo un programma di studi/sillabus, con alcuni link forniti alle guide introduttive. Abbiamo l'intenzione di convertirlo in un corso completo in futuro, quando il tempo lo permetterà.

È fondamentale avere una comprensione di come puoi e dovresti proteggere i tuoi dati e quelli dei tuoi utenti dagli aggressori che potrebbero tentarne il furto. Questo modulo copre sia il rafforzamento dei siti web per renderne più difficile il furto di dati, sia la raccolta dei dati degli utenti in modo rispettoso, evitando di tracciarli o condividerli con terze parti inadeguate.

## Prerequisiti

Prima di iniziare questo modulo, dovresti avere familiarità con [HTML](/it/docs/Learn_web_development/Core/Structuring_content) e [CSS](/it/docs/Learn_web_development/Core/Styling_basics).

## Obiettivi di apprendimento

### 5.1 Fondamenti di sicurezza e privacy

> [!NOTE]
>
> - Conformarsi a tutti i criteri di questo modulo non farà di uno studente un ingegnere di sicurezza qualificato, ma è altrettanto importante per gli sviluppatori web comprendere le basi della sicurezza e della privacy del web.
> - È anche importante che gli studenti comprendano che molti problemi di sicurezza sono causati da problemi con il codice lato server, o da una combinazione di codice lato client e server. Molto codice dovrebbe presentare pochissimi rischi di sicurezza, a patto che il browser svolga correttamente il suo lavoro.

Obiettivi di apprendimento:

- Comprendere la differenza tra sicurezza e privacy.

- Comprendere il modello HTTP generale a un livello alto.

- Imparare cos'è HTTPS e perché è importante.

- Sicurezza same-origin:

  - Perché è fondamentale per il web.

  - Modi per superarla in sicurezza, come il Cross-Origin Resource Sharing (CORS).

- Come vengono memorizzati i cookie e le loro implicazioni sulla sicurezza e privacy, come il tracciamento.

- Imparare su quali situazioni si verificano generalmente problemi di sicurezza:

  - Quando si chiede agli utenti di fornire dati sensibili (come password o dati delle carte di credito) e trasmetterli a un server.

  - Quando si richiedono dati da un server.

  - Quando si trasmettono dati tra server (per esempio, se un server richiede dati da un servizio web).

  - Quando si preserva lo stato dell'utente impostando un cookie o altri meccanismi.

- Imparare a conoscere le minacce comuni alla sicurezza e come mitigarle:

  - Cross-site scripting (XSS).

  - Cross-site request forgery (CSRF).

  - Clickjacking.

  - Denial of service (DoS).

- Comprendere lo scopo di altre tecnologie importanti, come:

  - Content Security Policy (CSP).

  - Permissions-Policy.

  - Il modello web per l'attivazione da parte dell'utente di "funzionalità potenti" (aka attivazione transitoria).

### 5.2 Leggi sulla protezione dei dati

Obiettivi di apprendimento:

- Comprendere i concetti fondamentali relativi alla privacy degli utenti:

  - Informazioni personali identificabili (PII).

  - Riservatezza.

  - Tracciamento.

  - Fingerprinting.

- Essere consapevoli delle leggi regionali sulla privacy, ad esempio:

  - [Regolamento generale sulla protezione dei dati (GDPR)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679&from=EN) (UE).

  - [Data Protection Act 2018](https://www.gov.uk/data-protection) (UK), gov.uk.

  - [California Consumer Privacy Act (2018)](https://www.oag.ca.gov/privacy/ccpa) (US, CA), ca.gov.

  - [Children's Online Privacy Protection Rule (COPPA)](https://www.ftc.gov/legal-library/browse/rules/childrens-online-privacy-protection-rule-coppa) (US), ftc.gov.

- Comprendere come conformarsi a tali leggi, in termini di implementazione pratica.

> [!NOTE]
> Conformarsi ai criteri sopra non richiede agli studenti di diventare esperti legali nelle leggi sulla privacy, ma dovrebbero comprendere le implicazioni di queste leggi e come ciò influenzi il loro lavoro.

## Risorse

- [Sicurezza sul web](/it/docs/Web/Security)
- [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security)
- [Privacy sul web](/it/docs/Web/Privacy)
- [Guida completa alla conformità al GDPR](https://gdpr.eu/), gdpr.eu

## Vedi anche

- [Learn Privacy](https://web.dev/learn/privacy/), web.dev (2023)
