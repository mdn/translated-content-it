---
title: Programmazione di siti web lato server
short-title: Siti web lato server
slug: Learn_web_development/Extensions/Server-side
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Il **_Siti Web Dinamici_** – Tema di **programmazione lato server** è una serie di moduli che mostrano come creare siti web dinamici; siti web che forniscono informazioni personalizzate in risposta a richieste HTTP. I moduli forniscono un'introduzione generale alla programmazione lato server, insieme a tutorial specifici per principianti su come utilizzare i framework web Django (Python) ed Express (Node.js/JavaScript) per creare applicazioni di base.

La maggior parte dei principali siti web utilizza una tecnologia lato server per visualizzare dinamicamente i dati come richiesto. Ad esempio, immaginate quanti prodotti sono disponibili su Amazon, e immaginate quanti post sono stati scritti su Facebook. Visualizzare tutto questo usando diverse pagine statiche sarebbe estremamente inefficiente, quindi tali siti visualizzano modelli statici (costruiti usando [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics), e [JavaScript](/it/docs/Learn_web_development/Core/Scripting)), e poi aggiornano dinamicamente i dati visualizzati all'interno di quei modelli quando necessario, ad esempio quando vuoi visualizzare un prodotto diverso su Amazon.

Nel mondo moderno dello sviluppo web, è altamente consigliato apprendere lo sviluppo lato server.

## Prerequisiti

Iniziare con la programmazione lato server è solitamente più facile rispetto allo sviluppo lato client, poiché i siti web dinamici tendono a eseguire molte operazioni simili (recupero di dati da un database e visualizzazione su una pagina, convalida di dati inseriti dall'utente e salvataggio in un database, controllo dei permessi utente e accesso degli utenti, ecc.), e sono costruiti utilizzando framework web che semplificano queste e altre operazioni comuni del server web.

Una conoscenza base dei concetti di programmazione (o di un particolare linguaggio di programmazione) è utile, ma non essenziale. Allo stesso modo, non è richiesta una competenza nel coding lato client, ma una conoscenza di base ti aiuterà a lavorare meglio con gli sviluppatori che stanno creando il "front end" web del lato client.

Avrai bisogno di capire "come funziona il web". Ti consigliamo di leggere prima i seguenti argomenti:

- [Cos'è un server web](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server)
- [Quale software mi serve per costruire un sito web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need)
- [Come si caricano i file su un server web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server)

Con quella comprensione di base, sarai pronto a seguire i moduli in questa sezione.

## Moduli

Questo argomento contiene i seguenti moduli. Dovresti iniziare con il primo modulo, quindi passare ad uno degli altri moduli, che mostrano come lavorare con due lingue lato server molto popolari utilizzando i relativi framework web.

- [Primi passi nella programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side/First_steps)
  - : Questo modulo fornisce informazioni prive di tecnologia specifica sulla programmazione di siti web lato server come "cos'è?", "in cosa differisce dalla programmazione lato client?", e "perché è utile?". Questo modulo descrive anche alcuni dei framework web lato server più popolari e fornisce una guida su come selezionare il migliore per il tuo sito. Infine, viene fornita un'introduzione alla sicurezza dei server web.
- [Django Web Framework (Python)](/it/docs/Learn_web_development/Extensions/Server-side/Django)
  - : Django è un framework web lato server estremamente popolare e completo, scritto in Python. Il modulo spiega perché Django sia un buon framework per il server web, come impostare un ambiente di sviluppo e come eseguire compiti comuni con esso.
- [Express Web Framework (Node.js/JavaScript)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs)
  - : Express è un popolare framework web, scritto in JavaScript e ospitato all'interno dell'ambiente di runtime di Node.js. Il modulo spiega alcuni dei principali benefici di questo framework, come impostare il tuo ambiente di sviluppo e come eseguire compiti comuni di sviluppo web e distribuzione.

## Vedi anche

- [Server Node senza framework](/it/docs/Learn_web_development/Extensions/Server-side/Node_server_without_framework)
  - : Questo articolo fornisce un semplice server di file statico costruito con puro Node.js, per coloro che non vogliono utilizzare un framework.
- [Configurare correttamente i tipi MIME del server](/it/docs/Learn_web_development/Extensions/Server-side/Configuring_server_MIME_types)
  - : Configurare il tuo server per inviare i corretti {{Glossary("MIME_type", "tipi MIME")}} (noti anche come tipi di media o tipi di contenuto) ai browser è importante affinché i browser possano elaborare e visualizzare correttamente il contenuto. 
    È anche importante prevenire che contenuti dannosi si mascherino da contenuti benigni.
- [Configurazione di Apache: .htaccess](/it/docs/Learn_web_development/Extensions/Server-side/Apache_Configuration_htaccess)
  - : I file .htaccess di Apache consentono agli utenti di configurare le directory del server web che controllano senza modificare il file di configurazione principale.
