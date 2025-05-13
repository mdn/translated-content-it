---
title: Express web framework (Node.js/JavaScript)
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side")}}

Express è un framework web non opinabile molto popolare, scritto in JavaScript e ospitato all'interno dell'ambiente runtime di Node.js. Questo modulo spiega alcuni dei principali benefici del framework, come impostare il suo ambiente di sviluppo e come eseguire compiti comuni di sviluppo e distribuzione web.

> [!WARNING]
> La documentazione e il tutorial di Express sono scritti per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Abbiamo in programma di aggiornare la documentazione nella seconda metà del 2025.

## Prerequisiti

Prima di iniziare questo modulo è necessario comprendere cosa siano la programmazione web lato server e i framework web, idealmente leggendo i temi nel nostro modulo [Primi passi nella programmazione dei siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps). Una conoscenza generale dei concetti di programmazione e di [JavaScript](/it/docs/Web/JavaScript) è altamente consigliata, ma non essenziale per comprendere i concetti di base.

> [!NOTE]
> Questo sito web offre molte risorse utili per apprendere JavaScript _nel contesto dello sviluppo lato client_: [JavaScript](/it/docs/Web/JavaScript), [Guida JavaScript](/it/docs/Web/JavaScript/Guide), [Basi di JavaScript](/it/docs/Learn_web_development/Getting_started/Your_first_website/Adding_interactivity), [JavaScript](/it/docs/Learn_web_development/Core/Scripting) (apprendimento). Il linguaggio JavaScript di base e i concetti sono gli stessi per lo sviluppo lato server su Node.js e questo materiale sarà rilevante. Node.js offre [API aggiuntive](https://nodejs.org/dist/latest-v10.x/docs/api/) per supportare funzionalità utili in ambienti senza browser (ad esempio, per creare server HTTP e accedere al file system), ma non supporta le API JavaScript per lavorare con il browser e il DOM.
>
> Questa serie di articoli fornirà alcune informazioni su come lavorare con Node.js ed Express, e ci sono numerose altre eccellenti risorse su Internet e nei libri — alcune di queste sono collegate da [Come fare per iniziare con Node.js](https://stackoverflow.com/questions/2353818/how-do-i-get-started-with-node-js/5511507) (Stack Overflow) e [Quali sono le migliori risorse per imparare Node.js?](https://www.quora.com/What-is-the-greatest-resource-for-learning-Node-js-for-a-newbie) (Quora).

## Tutorial

- [Introduzione a Express/Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction)
  - : In questo primo articolo su Express rispondiamo alle domande "Cos'è Node?" e "Cos'è Express?" e vi diamo una panoramica di ciò che rende speciale il framework web Express. Descriveremo le principali funzionalità e vi mostreremo alcuni dei principali elementi costitutivi di un'applicazione Express (anche se a questo punto non avrete ancora un ambiente di sviluppo in cui testarla).
- [Impostare un ambiente di sviluppo Node (Express)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment)
  - : Ora che sapete a cosa serve Express, vi mostreremo come impostare e testare un ambiente di sviluppo Node/Express su Windows, Linux (Ubuntu) e macOS. Qualunque sistema operativo comune stiate usando, questo articolo dovrebbe darvi ciò di cui avete bisogno per iniziare a sviluppare applicazioni Express.
- [Tutorial Express: Il sito web della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website)
  - : Il primo articolo della nostra serie di tutorial pratici spiega cosa imparerete e fornisce una panoramica del sito web di esempio "biblioteca locale" attraverso cui lavoreremo ed evolveremo nei successivi articoli.
- [Tutorial Express Parte 2: Creare un sito scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website)
  - : Questo articolo mostra come creare un progetto di sito "scheletro", che potrete poi riempire con rotte, template/visualizzazioni e database specifici del sito.
- [Tutorial Express Parte 3: Utilizzare un Database (con Mongoose)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose)
  - : Questo articolo introduce brevemente i database per Node/Express. Prosegue poi mostrando come possiamo usare [Mongoose](https://mongoosejs.com/) per fornire accesso al database per il sito web _LocalLibrary_. Spiega come dichiarare schemi e modelli di oggetti, i principali tipi di campo e la convalida di base. Mostra inoltre brevemente alcuni dei principali modi in cui è possibile accedere ai dati dei modelli.
- [Tutorial Express Parte 4: Route e controller](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes)
  - : In questo tutorial imposteremo le route (codice di gestione degli URL) con funzioni gestori "dummy" per tutti gli endpoint delle risorse che ci serviranno nel sito web _LocalLibrary_. Al completamento, avremo una struttura modulare per il nostro codice di gestione delle route, che potremo estendere con funzioni gestori reali negli articoli successivi. Avremo anche una buona comprensione di come creare route modulari usando Express.
- [Tutorial Express Parte 5: Visualizzare i dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data)
  - : Ora siamo pronti per aggiungere le pagine che visualizzano i libri del sito web _LocalLibrary_ e altri dati. Le pagine includeranno una pagina iniziale che mostra quanti record abbiamo di ciascun tipo di modello e pagine di elenco e dettaglio per tutti i nostri modelli. Lungo il percorso, acquisiremo esperienza pratica nell'ottenere record dal database e nell'usare template.
- [Tutorial Express Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms)
  - : In questo tutorial vi mostreremo come lavorare con i [moduli HTML](/it/docs/Learn_web_development/Extensions/Forms) in Express, usando Pug, e in particolare come scrivere moduli per creare, aggiornare e cancellare documenti dal database.
- [Tutorial Express Parte 7: Distribuire in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment)
  - : Ora che avete creato un fantastico sito web _LocalLibrary_, vorrete installarlo su un server web pubblico in modo che possa essere accessibile dal personale della biblioteca e dai membri tramite Internet. Questo articolo fornisce una panoramica su come trovare un host per distribuire il vostro sito web e cosa è necessario fare per preparare il sito per la produzione.

## Aggiungere più tutorial

Tutti i tutorial esistenti sono elencati sopra, ma se vorreste estendere questo modulo, alcuni altri argomenti interessanti da coprire includono:

- Utilizzo delle sessioni.
- Autenticazione degli utenti.
- Autorizzazione e permessi degli utenti.
- Testare un'applicazione web Express.
- Sicurezza web per applicazioni web Express.

Un esame per il modulo sarebbe anche una splendida aggiunta!

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side")}}
