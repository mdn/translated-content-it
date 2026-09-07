---
title: Framework web Express (Node.js/JavaScript)
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side")}}

Express è un popolare framework web non opinionato, scritto in JavaScript e ospitato nell'ambiente di runtime Node.js. Questo modulo illustra alcuni dei principali vantaggi del framework, come configurare l'ambiente di sviluppo e come svolgere attività comuni di sviluppo e distribuzione web.

## Prerequisiti

Prima di iniziare questo modulo è necessario comprendere cosa sono la programmazione web lato server e i framework web, idealmente leggendo gli argomenti del nostro modulo [Primi passi nella programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps). Una conoscenza generale dei concetti di programmazione e di [JavaScript](/it/docs/Web/JavaScript) è fortemente consigliata, ma non essenziale per comprendere i concetti fondamentali.

> [!NOTE]
> Questo sito web contiene molte risorse utili per imparare JavaScript _nel contesto dello sviluppo lato client_: [JavaScript](/it/docs/Web/JavaScript), [Guida a JavaScript](/it/docs/Web/JavaScript/Guide), [Fondamenti di JavaScript](/it/docs/Learn_web_development/Getting_started/Your_first_website/Adding_interactivity), [JavaScript](/it/docs/Learn_web_development/Core/Scripting) (apprendimento). Il linguaggio JavaScript di base e i suoi concetti sono gli stessi per lo sviluppo lato server su Node.js e questo materiale sarà pertinente. Node.js offre [API aggiuntive](https://nodejs.org/dist/latest-v10.x/docs/api/) per supportare funzionalità utili in ambienti senza browser (ad esempio, per creare server HTTP e accedere al file system), ma non supporta le API JavaScript per lavorare con il browser e il DOM.
>
> Questa serie di articoli fornirà alcune informazioni sul lavoro con Node.js ed Express; inoltre, sono disponibili numerose altre eccellenti risorse su Internet e nei libri — alcune delle quali sono collegate da [Come iniziare con Node.js](https://stackoverflow.com/questions/2353818/how-do-i-get-started-with-node-js/5511507) (Stack Overflow) e [Quali sono le migliori risorse per imparare Node.js?](https://www.quora.com/What-is-the-greatest-resource-for-learning-Node-js-for-a-newbie) (Quora).

## Tutorial

- [Introduzione a Express/Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction)
  - : In questo primo articolo su Express rispondiamo alle domande "Cos'è Node?" e "Cos'è Express?" e forniamo una panoramica di ciò che rende speciale il framework web Express. Illustreremo le caratteristiche principali e alcuni dei componenti fondamentali di un'applicazione Express (anche se a questo punto non sarà ancora disponibile un ambiente di sviluppo in cui testarla).
- [Configurazione di un ambiente di sviluppo Node (Express)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment)
  - : Ora che è chiaro a cosa serve Express, mostreremo come configurare e testare un ambiente di sviluppo Node/Express su Windows, Linux (Ubuntu) e macOS. Qualunque sia il sistema operativo comune in uso, questo articolo dovrebbe fornire tutto il necessario per iniziare a sviluppare app Express.
- [Tutorial Express: il sito web della biblioteca locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website)
  - : Il primo articolo della nostra serie di tutorial pratici spiega cosa verrà appreso e fornisce una panoramica del sito web di esempio della "biblioteca locale" su cui si lavorerà e che verrà sviluppato negli articoli successivi.
- [Tutorial Express Parte 2: creazione di un sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website)
  - : Questo articolo mostra come creare un progetto di sito web "scheletro", che potrà poi essere popolato con route, template/view e database specifici del sito.
- [Tutorial Express Parte 3: utilizzo di un database (con Mongoose)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose)
  - : Questo articolo introduce brevemente i database per Node/Express. Mostra poi come utilizzare [Mongoose](https://mongoosejs.com/) per fornire l'accesso al database per il sito web _LocalLibrary_. Spiega come dichiarare gli schemi degli oggetti e i modelli, i principali tipi di campo e la validazione di base. Mostra inoltre brevemente alcuni dei principali modi per accedere ai dati dei modelli.
- [Tutorial Express Parte 4: route e controller](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes)
  - : In questo tutorial configureremo le route (codice di gestione degli URL) con funzioni handler "fittizie" per tutti gli endpoint delle risorse che saranno infine necessari nel sito web _LocalLibrary_. Al termine, sarà disponibile una struttura modulare per il codice di gestione delle route, estendibile con funzioni handler reali negli articoli successivi. Sarà inoltre acquisita una solida comprensione di come creare route modulari usando Express.
- [Tutorial Express Parte 5: visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data)
  - : Ora è possibile aggiungere le pagine che visualizzano i libri e gli altri dati del sito web _LocalLibrary_. Le pagine includeranno una pagina iniziale che mostra quanti record sono presenti per ciascun tipo di modello, nonché pagine di elenco e di dettaglio per tutti i modelli. Durante il percorso, verrà acquisita esperienza pratica nel recupero dei record dal database e nell'uso dei template.
- [Tutorial Express Parte 6: lavoro con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms)
  - : In questo tutorial mostreremo come lavorare con i [moduli HTML](/it/docs/Learn_web_development/Extensions/Forms) in Express, utilizzando Pug e, in particolare, come scrivere moduli per creare, aggiornare ed eliminare documenti dal database.
- [Tutorial Express Parte 7: distribuzione in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment)
  - : Ora che è stato creato un fantastico sito web _LocalLibrary_, sarà necessario installarlo su un server web pubblico affinché possa essere accessibile al personale e agli iscritti della biblioteca tramite Internet. Questo articolo fornisce una panoramica su come individuare un host su cui distribuire il sito web e su cosa fare per preparare il sito alla produzione.

## Aggiungere altri tutorial

Tutti i tutorial esistenti sono elencati sopra, ma per estendere questo modulo alcuni altri argomenti interessanti da trattare includono:

- Uso delle sessioni.
- Autenticazione degli utenti.
- Autorizzazione degli utenti e permessi.
- Test di un'applicazione web Express.
- Sicurezza web per applicazioni web Express.

Anche una valutazione per il modulo sarebbe un'ottima aggiunta!

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side")}}
