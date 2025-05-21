---
title: Framework web Express (Node.js/JavaScript)
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side")}}

Express è un popolare framework web non opinato, scritto in JavaScript e ospitato all'interno dell'ambiente di runtime Node.js. Questo modulo spiega alcuni dei vantaggi chiave del framework, come configurare il proprio ambiente di sviluppo e come eseguire i compiti comuni di sviluppo e distribuzione web.

> [!WARNING]
> La documentazione e il tutorial di Express sono scritti per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Abbiamo in programma di aggiornare la documentazione nella seconda metà del 2025.

## Prerequisiti

Prima di iniziare questo modulo, è necessario comprendere cosa siano la programmazione web server-side e i framework web, idealmente leggendo gli argomenti nel nostro modulo [Primi passi nella programmazione di siti web server-side](/it/docs/Learn_web_development/Extensions/Server-side/First_steps). È altamente consigliata, ma non essenziale, una conoscenza generale dei concetti di programmazione e di [JavaScript](/it/docs/Web/JavaScript) per comprendere i concetti fondamentali.

> [!NOTE]
> Questo sito web ha molte risorse utili per imparare JavaScript _nel contesto dello sviluppo lato client_: [JavaScript](/it/docs/Web/JavaScript), [Guida JavaScript](/it/docs/Web/JavaScript/Guide), [Basi di JavaScript](/it/docs/Learn_web_development/Getting_started/Your_first_website/Adding_interactivity), [JavaScript](/it/docs/Learn_web_development/Core/Scripting) (apprendimento). Il linguaggio core di JavaScript e i concetti sono gli stessi per lo sviluppo server-side su Node.js e questo materiale sarà rilevante. Node.js offre [API aggiuntive](https://nodejs.org/dist/latest-v10.x/docs/api/) per supportare funzioni utili in ambienti senza browser (ad es., per creare server HTTP e accedere al file system), ma non supporta le API JavaScript per lavorare con il browser e il DOM.
>
> Questa serie di articoli fornirà alcune informazioni su come lavorare con Node.js ed Express, e ci sono numerose altre eccellenti risorse su Internet e nei libri — alcune di queste collegate da [Come posso iniziare con Node.js](https://stackoverflow.com/questions/2353818/how-do-i-get-started-with-node-js/5511507) (Stack Overflow) e [Quali sono le migliori risorse per imparare Node.js?](https://www.quora.com/What-is-the-greatest-resource-for-learning-Node-js-for-a-newbie) (Quora).

## Tutorial

- [Introduzione a Express/Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction)
  - : In questo primo articolo su Express rispondiamo alle domande "Cos'è Node?" e "Cos'è Express?" e ti diamo una panoramica di cosa rende speciale il framework web Express. Elencheremo le principali caratteristiche e ti mostreremo alcuni dei principali elementi costitutivi di un'applicazione Express (anche se a questo punto non avrai ancora un ambiente di sviluppo in cui testarla).
- [Configurazione di un ambiente di sviluppo Node (Express)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment)
  - : Ora che sai a cosa serve Express, ti mostreremo come impostare e testare un ambiente di sviluppo Node/Express su Windows, Linux (Ubuntu) e macOS. Qualunque sia il sistema operativo comune che stai utilizzando, questo articolo dovrebbe darti ciò di cui hai bisogno per iniziare a sviluppare app Express.
- [Tutorial Express: Il sito web della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website)
  - : Il primo articolo della nostra serie di tutorial pratici spiega cosa imparerai e fornisce una panoramica del sito web d'esempio "biblioteca locale" su cui lavoreremo e che svilupperemo nei successivi articoli.
- [Tutorial Express Parte 2: Creazione di un sito web scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website)
  - : Questo articolo mostra come creare un progetto di sito web "scheletro", che potrai poi completare con percorsi, modelli/viste e database specifici del sito.
- [Tutorial Express Parte 3: Utilizzo di un database (con Mongoose)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose)
  - : Questo articolo introduce brevemente i database per Node/Express. Poi mostra come possiamo utilizzare [Mongoose](https://mongoosejs.com/) per fornire accesso al database per il sito web _LocalLibrary_. Spiega come vengono dichiarati schema e modelli degli oggetti, i principali tipi di campo e la convalida di base. Mostra anche brevemente alcuni dei principali modi in cui puoi accedere ai dati del modello.
- [Tutorial Express Parte 4: Percorsi e controller](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes)
  - : In questo tutorial configureremo percorsi (codice di gestione degli URL) con funzioni gestori "fittizi" per tutti gli endpoint delle risorse di cui avremo bisogno nel sito web _LocalLibrary_. Al termine, avremo una struttura modulare per il nostro codice di gestione dei percorsi, che potremo estendere con vere funzioni gestori negli articoli seguenti. Avremo anche una buona comprensione di come creare percorsi modulari usando Express.
- [Tutorial Express Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data)
  - : Ora siamo pronti per aggiungere le pagine che mostrano i libri e altri dati del sito web _LocalLibrary_. Le pagine includeranno una home page che mostra quanti record abbiamo di ogni tipo di modello e pagine di elenco e dettaglio per tutti i nostri modelli. Nel frattempo, acquisiremo esperienza pratica nel recuperare record dal database e nell'utilizzare modelli.
- [Tutorial Express Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms)
  - : In questo tutorial ti mostreremo come lavorare con [Formulari HTML](/it/docs/Learn_web_development/Extensions/Forms) in Express, utilizzando Pug, in particolare come scrivere moduli per creare, aggiornare ed eliminare documenti dal database.
- [Tutorial Express Parte 7: Distribuzione in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment)
  - : Ora che hai creato un fantastico sito web _LocalLibrary_, vorrai installarlo su un server web pubblico in modo che possa essere accessibile dallo staff della biblioteca e dai membri via Internet. Questo articolo fornisce una panoramica di come trovare un host per distribuire il tuo sito web e cosa devi fare per preparare il tuo sito per la produzione.

## Aggiungere ulteriori tutorial

Tutti i tutorial esistenti sono elencati sopra, ma se desideri estendere questo modulo, alcuni altri argomenti interessanti da trattare includono:

- Uso delle sessioni.
- Autenticazione degli utenti.
- Autorizzazione e permessi degli utenti.
- Test di un'applicazione web Express.
- Sicurezza web per applicazioni web Express.

Una valutazione per il modulo sarebbe anche un'aggiunta fantastica!

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side")}}
