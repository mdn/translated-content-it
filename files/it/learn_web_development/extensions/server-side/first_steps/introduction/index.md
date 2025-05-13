---
title: Introduzione al lato server
short-title: Introduction
slug: Learn_web_development/Extensions/Server-side/First_steps/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps")}}

Benvenuti al corso di programmazione lato server per principianti di MDN! In questo primo articolo, vediamo la programmazione lato server a un livello alto, rispondendo a domande come "cos'è?", "in che modo si differenzia dalla programmazione lato client?" e "perché è così utile?". Dopo aver letto questo articolo comprenderà la potenza aggiuntiva disponibile per i siti web attraverso la programmazione lato server.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di cosa sia un server web.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con cosa sia la programmazione lato server, cosa possa fare, e come si differenzi dalla programmazione lato client.
      </td>
    </tr>
  </tbody>
</table>

La maggior parte dei siti web su larga scala utilizza il codice lato server per visualizzare dinamicamente dati diversi quando necessario, generalmente estratti da un database memorizzato su un server e inviati al client per essere visualizzati tramite qualche codice (ad esempio, HTML e JavaScript).

Forse il vantaggio più significativo del codice lato server è che permette di personalizzare il contenuto del sito web per utenti individuali. I siti dinamici possono mettere in evidenza contenuti che sono più rilevanti in base alle preferenze e abitudini dell'utente. Può anche rendere i siti più facili da usare memorizzando preferenze personali e informazioni — ad esempio riutilizzando i dettagli della carta di credito memorizzati per semplificare i pagamenti successivi.

Può persino consentire l'interazione con gli utenti del sito, inviando notifiche e aggiornamenti tramite email o attraverso altri canali. Tutte queste capacità permettono un coinvolgimento molto più profondo con gli utenti.

Nel mondo moderno dello sviluppo web, imparare lo sviluppo lato server è altamente consigliato.

## Cos'è la programmazione lato server?

I browser web comunicano con i [web server](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) usando il **P**rotocollo di **T**rasferimento di **I**per**t**esto ({{Glossary("HTTP", "HTTP")}}). Quando clicca su un link su una pagina web, invia un modulo o esegue una ricerca, una **richiesta HTTP** viene inviata dal suo browser al server di destinazione.

La richiesta include un URL che identifica la risorsa interessata, un metodo che definisce l'azione richiesta (ad esempio ottenere, eliminare o pubblicare la risorsa), e può includere informazioni aggiuntive codificate nei parametri URL (le coppie campo-valore inviate tramite una [stringa di query](https://en.wikipedia.org/wiki/Query_string)), come dati POST (dati inviati tramite il [metodo HTTP POST](/it/docs/Web/HTTP/Reference/Methods/POST)), o in {{Glossary("Cookie", "cookies")}} associati.

I server web attendono i messaggi di richiesta client, li elaborano quando arrivano e rispondono al browser web con un messaggio di **risposta HTTP**. La risposta contiene una linea di stato che indica se la richiesta è stata o meno completata con successo (ad esempio, "HTTP/1.1 200 OK" per il successo).

Il corpo di una risposta di successo a una richiesta contiene la risorsa richiesta (ad esempio, una nuova pagina HTML, o un'immagine), che potrebbe poi essere visualizzata dal browser web.

### Siti statici

Il diagramma qui sotto mostra un'architettura di server web di base per un _sito statico_ (un sito statico è quello che restituisce lo stesso contenuto codificato lateralmente dal server ogni volta che una particolare risorsa è richiesta). Quando un utente desidera navigare verso una pagina, il browser invia una richiesta HTTP "GET" specificando il suo URL.

Il server recupera il documento richiesto dal suo file system e restituisce una risposta HTTP contenente il documento e uno [stato di successo](/it/docs/Web/HTTP/Reference/Status#successful_responses) (solitamente 200 OK). Se il file non può essere recuperato per qualche motivo, viene restituito uno stato di errore (vedere [risposte di errore client](/it/docs/Web/HTTP/Reference/Status#client_error_responses) e [risposte di errore del server](/it/docs/Web/HTTP/Reference/Status#server_error_responses)).

![Un diagramma semplificato di un server web statico.](basic_static_app_server.png)

### Siti dinamici

Un sito web dinamico è quello in cui parte del contenuto della risposta è generato _dinamicamente_, solo quando necessario. Su un sito web dinamico, le pagine HTML sono normalmente create inserendo dati da un database in segnaposto nei modelli HTML (questo è un modo molto più efficiente di memorizzare grandi quantità di contenuto rispetto all'uso di siti statici).

Un sito dinamico può restituire dati diversi per un URL basato su informazioni fornite dall'utente o preferenze memorizzate e può eseguire altre operazioni come parte del ritorno di una risposta (ad esempio, l'invio di notifiche).

La maggior parte del codice necessario per supportare un sito web dinamico deve essere eseguito sul server. Creare questo codice è noto come "**programmazione lato server**" (o talvolta "**scripting back-end**").

Il diagramma qui sotto mostra un'architettura per un _sito web dinamico_. Come nel diagramma precedente, i browser inviano richieste HTTP al server, quindi il server elabora le richieste e restituisce risposte HTTP appropriate.

Le richieste per risorse _statiche_ sono gestite nello stesso modo dei siti statici (le risorse statiche sono qualsiasi file che non cambia — tipicamente: CSS, JavaScript, Immagini, file PDF prematuramente creati, ecc.).

![Un diagramma semplificato di un server web che utilizza la programmazione lato server per ottenere informazioni da un database e costruire HTML da modelli. Questo è lo stesso diagramma presente nella panoramica Client-Server.](web_application_with_html_and_steps.png)

Le richieste per risorse dinamiche sono invece inoltrate (2) al codice lato server (mostrato nel diagramma come un _Web Application_). Per le "richieste dinamiche" il server interpreta la richiesta, legge le informazioni richieste dal database (3), combina i dati recuperati con modelli HTML (4) e invia indietro una risposta contenente l'HTML generato (5,6).

## La programmazione lato server e lato client sono la stessa cosa?

Volgiamo ora la nostra attenzione al codice coinvolto nella programmazione lato server e lato client. In ciascun caso, il codice è significativamente diverso:

- Hanno scopi e preoccupazioni diversi.
- Generalmente non utilizzano gli stessi linguaggi di programmazione (l'eccezione è JavaScript, che può essere usato sia lato server che lato client).
- Eseguono all'interno di ambienti di sistema operativo diversi.

Il codice che viene eseguito nel browser è noto come **codice lato client** ed è principalmente interessato a migliorare l'aspetto e il comportamento di una pagina web resa. Ciò include la selezione e lo stile dei componenti dell'interfaccia utente, la creazione di layout, la navigazione, la convalida dei moduli, ecc. Al contrario, la programmazione del sito web lato server coinvolge principalmente la scelta di _quale contenuto_ viene restituito al browser in risposta alle richieste. Il codice lato server gestisce compiti come la convalida dei dati e delle richieste inviate, l'utilizzo di database per memorizzare e recuperare dati e l'invio dei dati corretti al cliente come richiesto.

Il codice lato client viene scritto utilizzando [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting) — viene eseguito all'interno di un browser web e ha un acceso limitato o assente al sistema operativo sottostante (incluso un accesso limitato al file system).

Gli sviluppatori di siti web non possono controllare quale browser ogni utente potrebbe usare per visualizzare un sito web — i browser forniscono livelli incoerenti di compatibilità con le funzionalità del codice lato client, e parte della sfida della programmazione lato client è gestire con grazia le differenze nel supporto del browser.

Il codice lato server può essere scritto in un numero qualsiasi di linguaggi di programmazione — esempi di linguaggi web lato server popolari includono PHP, Python, Ruby, C# e JavaScript (NodeJS). Il codice lato server ha pieno accesso al sistema operativo del server e lo sviluppatore può scegliere quale linguaggio di programmazione (e specifica versione) desidera utilizzare.

Gli sviluppatori tipicamente scrivono il loro codice utilizzando **framework web**. I framework web sono collezioni di funzioni, oggetti, regole e altri costrutti di codice progettati per risolvere problemi comuni, accelerare lo sviluppo e semplificare i diversi tipi di compiti affrontati in un particolare dominio.

Ancora una volta, mentre sia il codice lato client che lato server utilizzano framework, i domini sono molto diversi e quindi lo sono anche i framework. I framework web lato client semplificano i compiti di layout e presentazione mentre i framework web lato server forniscono molta "funzionalità comune" che altrimenti dovrebbe essere implementata autonomamente (ad esempio, supporto per sessioni, supporto per utenti e autenticazione, facile accesso al database, librerie di templating, ecc.).

> [!NOTE]
> I framework lato client sono spesso utilizzati per aiutare ad accelerare lo sviluppo del codice lato client, ma lei può anche scegliere di scrivere tutto il codice a mano; infatti, scrivere il suo codice a mano può essere più veloce ed efficiente se ha solo bisogno di una interfaccia utente del sito web piccola e semplice.
>
> Al contrario, quasi mai prenderebbe in considerazione la scrittura della componente lato server di un'app web senza un framework — implementare una funzione vitale come un server HTTP è davvero difficile da fare partendo da zero in Python, per esempio, ma i framework web Python come Django ne forniscono uno pronto all'uso, insieme ad altri strumenti molto utili.

## Cosa può fare sul lato server?

La programmazione lato server è molto utile perché ci permette di fornire _efficientemente_ informazioni personalizzate per utenti individuali e quindi creare un'esperienza utente molto migliore.

Aziende come Amazon utilizzano la programmazione lato server per costruire i risultati di ricerca per i prodotti, fare suggerimenti di prodotti mirati basati sulle preferenze dei clienti e sulle abitudini di acquisto precedenti, semplificare gli acquisti, ecc.

Le banche utilizzano la programmazione lato server per memorizzare le informazioni sui conti e consentire solo agli utenti autorizzati la visualizzazione e la possibilità di effettuare transazioni. Altri servizi come Facebook, Twitter, Instagram e Wikipedia utilizzano la programmazione lato server per mettere in evidenza, condividere e controllare l'accesso ai contenuti interessanti.

Alcuni degli usi e benefici comuni della programmazione lato server sono elencati di seguito. Noterà che c'è una certa sovrapposizione!

### Memorizzazione e consegna efficiente delle informazioni

Immagini quanti prodotti siano disponibili su Amazon, e immagini quanti post siano stati scritti su Facebook? Creare una pagina statica separata per ciascun prodotto o post sarebbe del tutto impraticabile.

La programmazione lato server ci consente invece di memorizzare le informazioni in un database e costruire dinamicamente e restituire HTML e altri tipi di file (ad esempio, PDF, immagini, ecc.). È anche possibile restituire dati ({{Glossary("JSON", "JSON")}}, {{Glossary("XML", "XML")}}, ecc.) per essere resi da appropriati framework web lato client (questo riduce l'onere del processamento sul server e la quantità di dati che deve essere inviata).

Il server non è limitato all'invio di informazioni da database, e potrebbe alternativamente restituire il risultato di strumenti software, o dati da servizi di comunicazione. Il contenuto può persino essere indirizzato per il tipo di dispositivo client che lo sta ricevendo.

Poiché le informazioni sono in un database, possono anche essere più facilmente condivise e aggiornate con altri sistemi aziendali (ad esempio, quando i prodotti sono venduti online o in un negozio, il negozio potrebbe aggiornare il suo database di inventario).

> [!NOTE]
> La sua immaginazione non deve lavorare duramente per vedere il beneficio del codice lato server per la memorizzazione efficiente e la consegna delle informazioni:
>
> 1. Vada su [Amazon](https://www.amazon.com/) o su qualche altro sito di e-commerce.
> 2. Cerchi per una serie di parole chiave e osservi come la struttura della pagina non cambi, anche se i risultati sì.
> 3. Apre due o tre prodotti diversi. Noti di nuovo come abbiano una struttura e un layout comuni, ma il contenuto per prodotti diversi è stato estratto dal database.
>
> Per un termine di ricerca comune ("pesce", ad esempio) lei può vedere letteralmente milioni di valori restituiti. Utilizzando un database permette di memorizzare e condividere questi dati in modo efficiente, e permette di controllare la presentazione delle informazioni in un solo posto.

### Esperienza utente personalizzata

I server possono memorizzare e utilizzare informazioni sui clienti per fornire un'esperienza utente conveniente e personalizzata. Ad esempio, molti siti memorizzano le carte di credito in modo che i dettagli non debbano essere reinseriti. Siti come Google Maps possono utilizzare i luoghi salvati o attuali per fornire informazioni sui percorsi, e la storia di ricerche o di viaggi per evidenziare aziende locali nei risultati di ricerca.

Un'analisi più profonda delle abitudini degli utenti può essere utilizzata per anticipare i loro interessi e personalizzare ulteriormente le risposte e le notifiche, ad esempio fornendo un elenco di luoghi precedentemente visitati o popolari che potrebbe voler esaminare su una mappa.

> **Nota:** [Google Maps](https://www.google.com/maps) salva la cronologia delle ricerche e delle visite. I luoghi visitati di frequente o cercati frequentemente sono evidenziati più di altri.
>
> I risultati di ricerca di Google sono ottimizzati in base alle ricerche precedenti.
>
> 1. Vada a [Google ricerca](https://www.google.com/).
> 2. Cerchi "calcio".
> 3. Ora provi a digitare "preferito" nella casella di ricerca e osservi i suggerimenti di ricerca autocomplete.
>
> Coincidenza? Nada!

### Accesso controllato ai contenuti

La programmazione lato server consente ai siti di limitare l'accesso agli utenti autorizzati e servire solo le informazioni che un utente è autorizzato a vedere.

Gli esempi del mondo reale includono i siti di social network che permettono agli utenti di determinare chi può vedere il contenuto che pubblicano sul sito e di vedere il loro feed.

> [!NOTE]
> Consideri altri esempi reali in cui l'accesso ai contenuti è controllato. Ad esempio, cosa può vedere se va al sito online della sua banca? Accedi al suo conto — quali informazioni aggiuntive può vedere e modificare? Quali informazioni può vedere che solo la banca può cambiare?

### Memorizzare informazioni sulla sessione/stato

La programmazione lato server permette agli sviluppatori di utilizzare **sessioni** — fondamentalmente, un meccanismo che consente a un server di memorizzare informazioni associate con l'utente corrente di un sito e inviare risposte diverse basate su tali informazioni.

Questo permette, ad esempio, a un sito di sapere che un utente si è precedentemente autenticato e mostrare collegamenti alle loro email o alla cronologia degli ordini, o forse salvare lo stato di un semplice gioco in modo che l'utente possa tornare su un sito e riprendere da dove lo ha lasciato.

> [!NOTE]
> Visiti un sito di un giornale che ha un modello di abbonamento e apra un sacco di schede (ad esempio, [The Age](https://www.theage.com.au/)). Continua a visitare il sito per alcune ore/giorni. Alla fine, comincerà ad essere reindirizzato a pagine che spiegano come abbonarsi, e non sarà più in grado di accedere agli articoli. Queste informazioni sono un esempio di informazioni di sessione memorizzate nei cookie.

### Notifiche e comunicazioni

I server possono inviare notifiche generali o specifiche per l'utente attraverso il sito web stesso o tramite email, SMS, messaggistica istantanea, conversazioni video o altri servizi di comunicazione.

Alcuni esempi includono:

- Facebook e Twitter inviano email e messaggi SMS per avvisarla di nuove comunicazioni.
- Amazon invia regolarmente email di prodotto che suggeriscono prodotti simili a quelli già acquistati o visualizzati di cui potrebbe essere interessato.
- Un server web potrebbe inviare messaggi di avviso agli amministratori del sito avvisandoli di memoria bassa sul server o di attività utente sospetta.

> [!NOTE]
> Il tipo di notifica più comune è una "conferma di registrazione". Scegli quasi qualsiasi sito grande che le interessa (Google, Amazon, Instagram, ecc.) e crei un nuovo account utilizzando il suo indirizzo email. Riceverà presto un'email che conferma la sua registrazione, o che richiede un riconoscimento per attivare il suo account.

### Analisi dei dati

Un sito web può raccogliere molti dati sugli utenti: cosa cercano, cosa comprano, cosa raccomandano, quanto tempo trascorrono su ciascuna pagina. La programmazione lato server può essere utilizzata per affinare le risposte basate sull'analisi di questi dati.

Ad esempio, Amazon e Google pubblicizzano prodotti basati su ricerche precedenti (e acquisti).

> [!NOTE]
> Se è un utente di Facebook, vada al suo feed principale e dia un'occhiata al flusso di post. Noti come alcuni dei post sono fuori ordine numerico - in particolare, i post con più "mi piace" sono spesso più alti nella lista rispetto ai post più recenti.
>
> Dia anche un'occhiata a che tipo di annunci pubblicitari le vengono mostrati — potrebbe vedere annunci per cose che ha guardato su altri siti. L'algoritmo di Facebook per evidenziare contenuto e pubblicità può essere un po' un mistero, ma è chiaro che dipenda dai suoi "mi piace" e dalle sue abitudini di visualizzazione!

## Sommario

Congratulazioni, ha raggiunto la fine del primo articolo sulla programmazione lato server.

Ha ora appreso che il codice lato server viene eseguito su un server web e che il suo ruolo principale è di controllare _quale_ informazione viene inviata all'utente (mentre il codice lato client gestisce principalmente la struttura e la presentazione di tali dati all'utente).

Dovrebbe anche capire che è utile in quanto ci permette di creare siti web che _efficientemente_ forniscono informazioni personalizzate per utenti individuali e avere una buona idea di alcune delle cose che potrebbe essere in grado di fare quando sarà un programmatore lato server.

Infine, dovrebbe comprendere che il codice lato server può essere scritto in un numero di linguaggi di programmazione e che dovrebbe utilizzare un framework web per rendere l'intero processo più semplice.

In un futuro articolo la aiuteremo a scegliere il miglior framework web per il suo primo sito. Qui la guideremo attraverso le principali interazioni cliente-server in solo un po' più di dettaglio.

{{NextMenu("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps")}}
