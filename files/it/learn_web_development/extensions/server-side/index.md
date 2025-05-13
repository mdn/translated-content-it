---
title: Programmazione di siti web lato server
short-title: Siti web lato server
slug: Learn_web_development/Extensions/Server-side
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Il tema **_Siti Web Dinamici_** – **Programmazione lato server** è una serie di moduli che mostrano come creare siti web dinamici; siti web che offrono informazioni personalizzate in risposta a richieste HTTP. I moduli forniscono un'introduzione generale alla programmazione lato server, insieme a tutorial specifici a livello principiante su come utilizzare i framework web Django (Python) ed Express (Node.js/JavaScript) per creare applicazioni di base.

La maggior parte dei principali siti web utilizza una qualche forma di tecnologia lato server per visualizzare i dati in modo dinamico secondo le necessità. Ad esempio, immaginate quanti prodotti sono disponibili su Amazon e quanti post sono stati scritti su Facebook. Visualizzare tutto questo utilizzando diverse pagine statiche sarebbe estremamente inefficiente, pertanto tali siti presentano modelli statici (costruiti utilizzando [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics), e [JavaScript](/it/docs/Learn_web_development/Core/Scripting)) e poi aggiornano dinamicamente i dati visualizzati all'interno di quei modelli quando necessario, ad esempio quando si desidera visualizzare un prodotto diverso su Amazon.

Nel mondo moderno dello sviluppo web, è vivamente consigliato imparare la programmazione lato server.

## Prerequisiti

Iniziare con la programmazione lato server è solitamente più semplice rispetto allo sviluppo lato client, perché i siti web dinamici tendono a eseguire molte operazioni molto simili (recuperare dati da un database e visualizzarli in una pagina, validare dati inseriti dall'utente e salvarli in un database, verificare i permessi dell'utente e eseguire il login degli utenti, ecc.) e sono costruiti utilizzando framework web che rendono semplici queste e altre comuni operazioni sui server web.

Una conoscenza di base dei concetti di programmazione (o di un particolare linguaggio di programmazione) è utile, ma non essenziale. Allo stesso modo, non è richiesta una conoscenza approfondita del codice lato client, ma una conoscenza di base aiuterà a lavorare meglio con gli sviluppatori che creano il "front end" web lato client.

Sarà necessario comprendere "come funziona il web". Consigliamo di leggere prima i seguenti argomenti:

- [Cos'è un server web](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server)
- [Quale software mi serve per costruire un sito web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need)
- [Come si caricano i file su un server web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server)

Con questa comprensione di base, sarà pronto per attraversare i moduli di questa sezione.

## Moduli

Questo tema contiene i seguenti moduli. Dovrebbe iniziare con il primo modulo, per poi passare a uno dei seguenti moduli, che mostrano come lavorare con due linguaggi lato server molto popolari utilizzando i framework web appropriati.

- [Primi passi nella programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps)
  - : Questo modulo fornisce informazioni agnostiche sulla tecnologia nella programmazione di siti web lato server, come "cos'è?", "in cosa differisce dalla programmazione lato client?" e "perché è utile?". Questo modulo delinea anche alcuni dei framework web lato server più popolari e fornisce indicazioni su come selezionare il migliore per il suo sito. Infine, viene fornita un'introduzione alla sicurezza del server web.
- [Django Web Framework (Python)](/it/docs/Learn_web_development/Extensions/Server-side/Django)
  - : Django è un framework web lato server estremamente popolare e completo, scritto in Python. Il modulo spiega perché Django è un così buon framework per server web, come impostare un ambiente di sviluppo e come svolgere compiti comuni con esso.
- [Express Web Framework (Node.js/JavaScript)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs)
  - : Express è un popolare framework web, scritto in JavaScript e ospitato all'interno dell'ambiente di runtime di Node.js. Il modulo spiega alcuni dei principali vantaggi di questo framework, come configurare il suo ambiente di sviluppo e come eseguire comuni compiti di sviluppo e distribuzione web.

## Vedere anche

- [Server Node senza framework](/it/docs/Learn_web_development/Extensions/Server-side/Node_server_without_framework)
  - : Questo articolo fornisce un semplice server di file statici costruito con il solo Node.js, per coloro che non desiderano utilizzare un framework.
- [Configurazione corretta dei tipi MIME del server](/it/docs/Learn_web_development/Extensions/Server-side/Configuring_server_MIME_types)
  - : Configurare il suo server per inviare i corretti {{Glossary("MIME_type", "tipi MIME")}} (conosciuti anche come tipi di media o tipi di contenuto) ai browser è importante per permettere ai browser di elaborare e visualizzare correttamente il contenuto.
    È anche importante per prevenire che contenuti dannosi si mascherino da contenuti innocui.
- [Configurazione di Apache: .htaccess](/it/docs/Learn_web_development/Extensions/Server-side/Apache_Configuration_htaccess)
  - : I file .htaccess di Apache permettono agli utenti di configurare le directory del server web che controllano senza modificare il file di configurazione principale.
