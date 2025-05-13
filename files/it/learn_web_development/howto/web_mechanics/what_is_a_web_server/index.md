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
        Dovrebbe già sapere
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/How_does_the_Internet_work"
          >come funziona Internet</a
        >, e
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web"
          >comprendere la differenza tra una pagina web, un sito web, un server web, e un motore di ricerca</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparerà cosa è un server web e acquisirà una comprensione generale di come funziona.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

Il termine _server web_ può riferirsi all'hardware o al software, o entrambi che lavorano insieme.

1. Dal lato hardware, un server web è un computer che memorizza il software del server web e i file componenti di un sito web (ad esempio, documenti HTML, immagini, fogli di stile CSS e file JavaScript). Un server web si connette a Internet e supporta lo scambio fisico di dati con altri dispositivi connessi al web.
2. Dal lato software, un server web include diverse parti che controllano come gli utenti web accedono ai file ospitati. Al minimo, questo è un _server HTTP_. Un server HTTP è un software che comprende gli {{Glossary("URL", "URL")}} (indirizzi web) e l'{{Glossary("HTTP", "HTTP")}} (il protocollo che il tuo browser utilizza per visualizzare le pagine web). Un server HTTP può essere accessibile tramite i nomi di dominio dei siti web che conserva, e consegna il contenuto di questi siti web ospitati al dispositivo dell'utente finale.

Al livello più elementare, ogni volta che un browser ha bisogno di un file ospitato su un server web, il browser richiede il file tramite HTTP. Quando la richiesta raggiunge il corretto server web (hardware), il _server HTTP_ (software) accetta la richiesta, trova il documento richiesto e lo invia indietro al browser, sempre tramite HTTP. (Se il server non trova il documento richiesto, restituisce invece una risposta [404](/it/docs/Web/HTTP/Reference/Status/404)).

![Rappresentazione base di una connessione client/server tramite HTTP](web-server.svg)

Per pubblicare un sito web, è necessario un server web statico o dinamico.

Un **server web statico**, o stack, consiste di un computer (hardware) con un server HTTP (software). Lo chiamiamo "statico" perché il server invia i suoi file ospitati così come sono al tuo browser.

Un **server web dinamico** consiste di un server web statico più software aggiuntivo, più comunemente un _server applicativo_ e un _database_. Lo chiamiamo "dinamico" perché il server applicativo aggiorna i file ospitati prima di inviare il contenuto al tuo browser tramite il server HTTP.

Ad esempio, per produrre le pagine web finali che vede nel browser, il server applicativo potrebbe riempire un modello HTML con il contenuto di un database. Siti come MDN o Wikipedia hanno migliaia di pagine web. Tipicamente, questi tipi di siti sono composti da solo pochi modelli HTML e un vasto database, piuttosto che migliaia di documenti HTML statici. Questo approccio facilita la manutenzione e la distribuzione del contenuto.

## Approfondimento

Per rivedere: per recuperare una pagina web, il suo browser invia una richiesta al server web, che cerca il file richiesto nel proprio spazio di archiviazione. Una volta trovato il file, il server lo legge, lo elabora se necessario e lo invia al browser. Esaminiamo questi passaggi più nel dettaglio.

### Hosting dei file

Innanzitutto, un server web deve memorizzare i file del sito web, vale a dire tutti i documenti HTML e le relative risorse, inclusi immagini, fogli di stile CSS, file JavaScript, font e video.

Tecnicamente, potrebbe ospitare tutti quei file sul suo computer, ma è molto più conveniente memorizzare i file tutti su un server web dedicato perché:

- Un server web dedicato è tipicamente più disponibile (acceso e funzionante).
- Escludendo tempi di inattività e problemi di sistema, un server web dedicato è sempre connesso a Internet.
- Un server web dedicato può avere lo stesso indirizzo IP tutto il tempo. Questo è noto come _indirizzo IP dedicato_. (Non tutti gli {{Glossary("ISP", "ISP")}} forniscono un indirizzo IP fisso per le linee domestiche.)
- Un server web dedicato è tipicamente mantenuto da terze parti.

Per tutte queste ragioni, trovare un buon provider di hosting è una parte fondamentale della costruzione del suo sito web. Esamini i vari servizi offerti dalle aziende. Scelga uno che si adatta alle sue necessità e al suo budget. (I servizi vanno dal gratuito a migliaia di dollari al mese.) Può trovare più dettagli [in questo articolo](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost#hosting).

Una volta che dispone di un servizio di hosting web, deve [caricare i suoi file sul server web](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server).

### Comunicazione tramite HTTP

In secondo luogo, un server web fornisce supporto per {{Glossary("HTTP", "HTTP")}} (Hypertext Transfer Protocol). Come implica il suo nome, HTTP specifica come trasferire ipertesto (documenti web collegati) tra due computer.

Un {{Glossary("Protocol", "Protocollo")}} è un insieme di regole per la comunicazione tra due computer. HTTP è un protocollo testuale e senza stato.

- Testuale
  - : Tutti i comandi sono in testo semplice e leggibili dall'uomo.
- Senza stato
  - : Né il server né il client ricordano comunicazioni precedenti. Ad esempio, affidandosi solo a HTTP, un server non può ricordare una password digitata o ricordare i suoi progressi su una transazione incompleta. Ha bisogno di un server applicativo per compiti come quello. (Tratteremo questo tipo di tecnologia in altri articoli.)

HTTP fornisce regole chiare su come un client e un server comunicano.
Se vuole saperne di più, può leggere la [documentazione HTTP](/it/docs/Web/HTTP).
Per ora, ci sono alcune cose da tenere a mente:

- I _client_ fanno richieste HTTP ai _server_. I server _rispondono_ alla richiesta HTTP di un _client_.
- Quando si richiede un file tramite HTTP, i client devono fornire l'{{Glossary("URL", "URL")}} del file.
- Il server web _deve rispondere_ a ogni richiesta HTTP, almeno con un messaggio di errore.

Su un server web, il server HTTP è responsabile dell'elaborazione e rispondere alle richieste in arrivo.

1. Al ricevimento di una richiesta, un server HTTP verifica se l'URL richiesto corrisponde a un file esistente.
2. In tal caso, il server web invia il contenuto del file al browser. In caso contrario, il server verificherà se deve generare un file dinamico per la richiesta (vedi [Contenuto statico vs dinamico](#contenuto_statico_vs_dinamico)).
3. Se nessuna di queste opzioni è possibile, il server web restituisce un messaggio di errore al browser, più comunemente {{HTTPStatus("404", "404 Not Found")}}.
   L'errore 404 è così comune che alcuni designer web dedicano tempo e considerevoli sforzi alla progettazione delle pagine di errore 404.
   ![La pagina 404 di MDN come esempio di tale pagina di errore](mdn-404.jpg)

### Contenuto statico vs dinamico

In linea di massima, un server può servire contenuto statico o dinamico. Ricordi che il termine _statico_ significa "servito così com'è". I siti web statici sono i più semplici da impostare, quindi suggeriamo di creare il suo primo sito come sito statico.

Il termine _dinamico_ significa che il server elabora il contenuto o addirittura lo genera al volo da un database. Questo approccio fornisce una maggiore flessibilità, ma l'ambiente tecnico è più complesso, rendendo notevolmente più impegnativo costruire un sito web.

È impossibile suggerire un singolo server applicativo pronto all'uso che sarà la soluzione giusta per ogni possibile caso d'uso. Alcuni server applicativi sono progettati per ospitare e gestire blog, wiki o soluzioni di e-commerce, mentre altri sono più generici. Se sta costruendo un sito web dinamico, si prenda il tempo necessario per esaminare i suoi requisiti e trovare la tecnologia che meglio si adatta alle sue esigenze.

La maggior parte degli sviluppatori di siti web non avrà bisogno di creare un server applicativo da zero, poiché ci sono così tante soluzioni pronte all'uso, molte delle quali altamente configurabili.
Ma se dovesse aver bisogno di creare il suo server, probabilmente vorrà utilizzare un framework server, sfruttando il suo codice e le sue librerie esistenti, ed estendendo solo le parti di cui ha bisogno per soddisfare il suo caso d'uso.
Solo un numero relativamente piccolo di sviluppatori dovrebbe aver bisogno di sviluppare un server completamente da zero: ad esempio, per soddisfare stretti vincoli di risorse su un sistema embedded.
Se desidera sperimentare con la costruzione di un server, dia un'occhiata alle risorse nel percorso di apprendimento di [programmazione dei siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side).

## Prossimi passi

Ora che ha familiarità con i server web, potrebbe:

- documentarsi su [quanto costa fare qualcosa sul web](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost)
- saperne di più sui [vari software necessari per creare un sito web](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need)
- passare a qualcosa di pratico come [come caricare file su un server web](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server).
