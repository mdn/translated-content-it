---
title: Cos'è un server web?
slug: Learn_web_development/Howto/Web_mechanics/What_is_a_web_server
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

In questo articolo, spieghiamo cosa sono i server web, come funzionano e perché sono importanti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        È necessario avere già conoscenze su
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >, e
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web"
          >comprendere la differenza tra una pagina web, un sito web, un
          server web e un motore di ricerca</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparerai cosa è un server web e avrai una comprensione generale di
        come funziona.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

Il termine _server web_ può riferirsi all'hardware o al software, oppure entrambi che lavorano insieme.

1. Dal lato hardware, un server web è un computer che memorizza il software del server web e i file componenti di un sito web (per esempio, documenti HTML, immagini, fogli di stile CSS e file JavaScript). Un server web si connette a Internet e supporta l'interscambio fisico di dati con altri dispositivi connessi al web.
2. Dal lato software, un server web include diverse parti che controllano come gli utenti web accedono ai file ospitati. Al minimo, questo è un _server HTTP_. Un server HTTP è un software che comprende {{Glossary("URL", "URL")}} (indirizzi web) e {{Glossary("HTTP", "HTTP")}} (il protocollo che il tuo browser usa per visualizzare le pagine web). Un server HTTP può essere accessibile tramite i nomi di dominio dei siti web che ospita, e consegna il contenuto di questi siti web ospitati al dispositivo finale dell'utente.

Al livello più basilare, ogni volta che un browser necessita di un file ospitato su un server web, il browser richiede il file tramite HTTP. Quando la richiesta raggiunge il corretto server web (hardware), il _server HTTP_ (software) accetta la richiesta, trova il documento richiesto e lo invia nuovamente al browser, sempre tramite HTTP. (Se il server non trova il documento richiesto, restituisce invece una risposta [404](/it/docs/Web/HTTP/Reference/Status/404).)

![Rappresentazione di base di una connessione client/server attraverso HTTP](web-server.svg)

Per pubblicare un sito web, hai bisogno di un server web statico o dinamico.

Un **server web statico**, o stack, è composto da un computer (hardware) con un server HTTP (software). Lo chiamiamo "statico" perché il server invia i suoi file ospitati così come sono al tuo browser.

Un **server web dinamico** è composto da un server web statico più software extra, più comunemente un _application server_ e un _database_. Lo chiamiamo "dinamico" perché l'application server aggiorna i file ospitati prima di inviare il contenuto al tuo browser tramite il server HTTP.

Per esempio, per produrre le pagine web finali che vedi nel browser, l'application server potrebbe riempire un modello HTML con il contenuto di un database. Siti come MDN o Wikipedia hanno migliaia di pagine web. Tipicamente, questi tipi di siti sono composti solo da pochi modelli HTML e un enorme database, piuttosto che migliaia di documenti HTML statici. Questa configurazione facilita la manutenzione e la consegna del contenuto.

## Approfondimento

Per riepilogare: per recuperare una pagina web, il tuo browser invia una richiesta al server web, che cerca il file richiesto nel proprio spazio di archiviazione. Dopo aver trovato il file, il server lo legge, lo elabora se necessario e lo invia al browser. Esaminiamo questi passaggi in maggior dettaglio.

### Ospitare i file

Innanzitutto, un server web deve memorizzare i file del sito web, ovvero tutti i documenti HTML e i loro asset correlati, incluse immagini, fogli di stile CSS, file JavaScript, font e video.

Tecnicamente, potresti ospitare tutti quei file sul tuo computer, ma è molto più conveniente memorizzare i file tutti su un server web dedicato perché:

- Un server web dedicato è tipicamente più disponibile (operativo).
- Escludendo tempi di inattività e problemi di sistema, un server web dedicato è sempre connesso a Internet.
- Un server web dedicato può avere lo stesso indirizzo IP tutto il tempo. Questo è noto come _indirizzo IP dedicato_. (Non tutti gli {{Glossary("ISP", "ISP")}} forniscono un indirizzo IP fisso per le linee domestiche.)
- Un server web dedicato è tipicamente gestito da una terza parte.

Per tutti questi motivi, trovare un buon fornitore di servizi di hosting è una parte cruciale della costruzione del tuo sito web. Esamina i vari servizi offerti dalle aziende. Scegli uno che si adatta alle tue esigenze e al tuo budget. (I servizi variano da gratuiti a migliaia di dollari al mese.) Puoi trovare ulteriori dettagli [in questo articolo](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost#hosting).

Una volta che hai un servizio di hosting web, devi [caricare i tuoi file sul tuo server web](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server).

### Comunicare tramite HTTP

In secondo luogo, un server web fornisce supporto per {{Glossary("HTTP", "HTTP")}} (Hypertext Transfer Protocol). Come implica il nome, HTTP specifica come trasferire ipertesti (documenti web collegati) tra due computer.

Un {{Glossary("Protocol", "Protocollo")}} è un insieme di regole per la comunicazione tra due computer. HTTP è un protocollo testuale e senza stato.

- Testuale
  - : Tutti i comandi sono in testo semplice e leggibili dall'uomo.
- Senza stato
  - : Né il server né il client ricordano le comunicazioni precedenti. Ad esempio, affidandosi solo a HTTP, un server non può ricordare una password digitata o ricordare il progresso su una transazione incompleta. Per compiti come questi, hai bisogno di un application server. (Esploreremo questo tipo di tecnologia in altri articoli.)

HTTP fornisce regole chiare su come un client e un server comunicano.
Se vuoi saperne di più, puoi leggere la [documentazione HTTP](/it/docs/Web/HTTP).
Per ora, ci sono alcune cose da tenere a mente:

- I _client_ effettuano richieste HTTP ai _server_. I server _rispondono_ alla richiesta HTTP di un _client_.
- Quando si richiede un file tramite HTTP, i client devono fornire l' {{Glossary("URL", "URL")}} del file.
- Il server web _deve rispondere_ ad ogni richiesta HTTP, almeno con un messaggio di errore.

Su un server web, il server HTTP è responsabile dell'elaborazione e della risposta alle richieste in arrivo.

1. Alla ricezione di una richiesta, un server HTTP verifica se l'URL richiesto corrisponde a un file esistente.
2. In tal caso, il server web invia il contenuto del file al browser. In caso contrario, il server verificherà se deve generare un file dinamicamente per la richiesta (vedi [Contenuto statico vs. dinamico](#contenuto_statico_vs._dinamico)).
3. Se nessuna di queste opzioni è possibile, il server web restituisce al browser un messaggio di errore, più comunemente {{HTTPStatus("404", "404 Non Trovato")}}.
   L'errore 404 è così comune che alcuni web designer dedicano tempo ed energia considerevoli alla progettazione delle pagine di errore 404.
   ![La pagina 404 di MDN come esempio di tale pagina di errore](mdn-404.jpg)

### Contenuto statico vs. dinamico

Grosso modo, un server può servire contenuti statici o dinamici. Ricorda che il termine _statico_ significa "serviti così come sono". I siti web statici sono i più facili da impostare, quindi suggeriamo di fare il tuo primo sito come un sito statico.

Il termine _dinamico_ significa che il server elabora il contenuto o lo genera addirittura al volo da un database. Questo approccio fornisce maggiore flessibilità, ma lo stack tecnico è più complesso, rendendo notevolmente più difficile costruire un sito web.

È impossibile suggerire un server applicativo preconfezionato che sia la soluzione giusta per ogni possibile caso d'uso. Alcuni server applicativi sono progettati per ospitare e gestire blog, wiki o soluzioni di e-commerce, mentre altri sono più generali. Se stai costruendo un sito web dinamico, prenditi il tempo per analizzare i tuoi requisiti e trova la tecnologia che meglio si adatta alle tue esigenze.

La maggior parte degli sviluppatori di siti web non avrà bisogno di creare un server applicativo da zero, perché esistono molte soluzioni preconfezionate, molte delle quali altamente configurabili.
Ma se hai bisogno di creare un tuo server, allora probabilmente vorrai utilizzare un framework server, sfruttando il suo codice esistente e le sue librerie, ed estendendo solo le parti di cui hai bisogno per soddisfare il tuo caso d'uso.
Solo un numero relativamente piccolo di sviluppatori dovrebbe avere la necessità di sviluppare un server completamente da zero: ad esempio, per soddisfare vincoli di risorse stretti su un sistema embedded.
Se vuoi sperimentare con la costruzione di un server, dai un'occhiata alle risorse nel percorso di apprendimento [Programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side).

## Prossimi passi

Ora che sei familiare con i server web, potresti:

- leggere su [quanto costa fare qualcosa sul web](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost)
- saperne di più su [vari software necessari per creare un sito web](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need)
- passare a qualcosa di pratico come [come caricare file su un server web](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server).
