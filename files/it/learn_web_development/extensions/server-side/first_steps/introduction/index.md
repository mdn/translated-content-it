---
title: Introduzione al lato server
short-title: Introduction
slug: Learn_web_development/Extensions/Server-side/First_steps/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps")}}

Benvenuti al corso di programmazione lato server per principianti di MDN! In questo primo articolo, esamineremo la programmazione lato server da una prospettiva di alto livello, rispondendo a domande come "cos'è?", "in che modo differisce dalla programmazione lato client?" e "perché è così utile?". Dopo aver letto questo articolo, comprenderai il potere aggiuntivo disponibile per i siti web attraverso la codifica lato server.

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
        Acquisire familiarità con cosa sia la programmazione lato server, cosa possa
        fare e in che modo differisce dalla programmazione lato client.
      </td>
    </tr>
  </tbody>
</table>

La maggior parte dei siti web su larga scala utilizza codice lato server per visualizzare dinamicamente dati diversi quando necessario, generalmente prelevati da un database memorizzato su un server e inviati al client per essere visualizzati tramite del codice (ad esempio, HTML e JavaScript).

Forse il vantaggio più significativo del codice lato server è che permette di personalizzare il contenuto del sito web per singoli utenti. I siti dinamici possono evidenziare contenuti più rilevanti basati sulle preferenze e abitudini dell'utente. Può anche rendere i siti più facili da usare memorizzando preferenze personali e informazioni — ad esempio, riutilizzando i dettagli della carta di credito memorizzati per semplificare i pagamenti successivi.

Consente anche di interagire con gli utenti del sito, inviando notifiche e aggiornamenti tramite email o altri canali. Tutte queste funzionalità consentono un coinvolgimento molto più profondo con gli utenti.

Nel mondo moderno dello sviluppo web, apprendere lo sviluppo lato server è altamente raccomandato.

## Cos'è la programmazione web lato server?

I browser comunicano con i [server web](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) utilizzando il **P**rotocollo di **T**rasferimento di **I**pertesti ({{Glossary("HTTP", "HTTP")}}). Quando clicchi un link su una pagina web, invii un modulo o esegui una ricerca, una **richiesta HTTP** viene inviata dal tuo browser al server di destinazione.

La richiesta include un URL che identifica la risorsa interessata, un metodo che definisce l'azione richiesta (ad esempio per ottenere, eliminare o pubblicare la risorsa) e può includere informazioni aggiuntive codificate nei parametri URL (le coppie campo-valore inviate tramite una [query string](https://en.wikipedia.org/wiki/Query_string)), come dati POST (dati inviati tramite il [metodo POST HTTP](/it/docs/Web/HTTP/Reference/Methods/POST)), o nei {{Glossary("Cookie", "cookie")}} associati.

I server web attendono i messaggi di richiesta del client, li elaborano quando arrivano e rispondono al browser web con un messaggio di **risposta HTTP**. La risposta contiene una riga di stato che indica se la richiesta è andata a buon fine (ad esempio, "HTTP/1.1 200 OK" per il successo).

Il corpo di una risposta di successo a una richiesta conterrebbe la risorsa richiesta (ad esempio, una nuova pagina HTML o un'immagine), che potrebbe poi essere visualizzata dal browser web.

### Siti statici

Il diagramma seguente mostra un'architettura di base di un server web per un sito _statico_ (un sito statico è uno che restituisce lo stesso contenuto hard-coded dal server ogni volta che viene richiesta una determinata risorsa). Quando un utente desidera navigare verso una pagina, il browser invia una richiesta HTTP "GET" specificando il suo URL.

Il server recupera il documento richiesto dal suo file system e restituisce una risposta HTTP contenente il documento e uno [stato di successo](/it/docs/Web/HTTP/Reference/Status#successful_responses) (solitamente 200 OK). Se il file non può essere recuperato per qualche motivo, viene restituito uno stato di errore (vedi [risposte di errore client](/it/docs/Web/HTTP/Reference/Status#client_error_responses) e [risposte di errore server](/it/docs/Web/HTTP/Reference/Status#server_error_responses)).

![Un diagramma semplificato di un server web statico.](basic_static_app_server.png)

### Siti dinamici

Un sito web dinamico è uno in cui parte del contenuto della risposta è generata _dinamicamente_, solo quando necessario. In un sito web dinamico, le pagine HTML vengono normalmente create inserendo dati da un database in segnaposto nei modelli HTML (questo è un modo molto più efficiente di memorizzare grandi quantità di contenuto rispetto all'uso di siti web statici).

Un sito dinamico può restituire dati diversi per un URL basati sulle informazioni fornite dall'utente o preferenze memorizzate e può eseguire altre operazioni come parte della risposta (ad esempio, inviare notifiche).

La maggior parte del codice per supportare un sito web dinamico deve essere eseguita sul server. Creare questo codice è noto come "programmazione **lato server**" (o a volte "scripting **back-end**").

Il diagramma seguente mostra un'architettura per un _sito web dinamico_. Come nel diagramma precedente, i browser inviano richieste HTTP al server, quindi il server elabora le richieste e restituisce risposte HTTP appropriate.

Le richieste per risorse _statiche_ vengono gestite nello stesso modo dei siti statici (le risorse statiche sono tutti quei file che non cambiano — tipicamente: CSS, JavaScript, immagini, file PDF pre-creati, ecc.).

![Un diagramma semplificato di un server web che utilizza la programmazione lato server per ottenere informazioni da un database e costruire HTML da template. Questo è lo stesso diagramma nella panoramica Client-Server.](web_application_with_html_and_steps.png)

Le richieste per risorse dinamiche vengono invece inoltrate (2) al codice lato server (mostrato nel diagramma come un' _applicazione web_). Per le "richieste dinamiche", il server interpreta la richiesta, legge le informazioni richieste dal database (3), combina i dati recuperati con i template HTML (4) e invia una risposta contenente l'HTML generato (5,6).

## La programmazione lato server e lato client sono la stessa cosa?

Rivolgiamo ora l'attenzione al codice coinvolto nella programmazione lato server e lato client. In entrambi i casi, il codice è significativamente diverso:

- Hanno scopi e preoccupazioni diversi.
- Generalmente non usano gli stessi linguaggi di programmazione (l'eccezione è JavaScript, che può essere utilizzato su entrambi i lati server e client).
- Vengono eseguiti in ambienti di sistema operativo diversi.

Il codice eseguito nel browser è noto come **codice lato client** ed è principalmente preoccupato di migliorare l'aspetto e il comportamento di una pagina web resa. Questo include la selezione e lo styling dei componenti UI, la creazione di layout, la navigazione, la convalida dei moduli, ecc. Per contrasto, la programmazione dei siti web lato server riguarda principalmente la scelta di _quale contenuto_ venga restituito al browser in risposta alle richieste. Il codice lato server gestisce attività come la validazione dei dati inviati e delle richieste, l'utilizzo di database per memorizzare e recuperare dati e l'invio dei dati corretti al client come richiesto.

Il codice lato client è scritto utilizzando [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting) — viene eseguito all'interno di un browser web e ha scarso o nessun accesso al sistema operativo sottostante (incluso un accesso limitato al file system).

Gli sviluppatori web non possono controllare quale browser ogni utente utilizzerà per visualizzare un sito web — i browser forniscono livelli di compatibilità incoerenti con le funzionalità del codice lato client, e parte della sfida della programmazione lato client è gestire le differenze nel supporto del browser in modo elegante.

Il codice lato server può essere scritto in un numero qualsiasi di linguaggi di programmazione — esempi di linguaggi web lato server popolari includono PHP, Python, Ruby, C# e JavaScript (NodeJS). Il codice lato server ha pieno accesso al sistema operativo del server e lo sviluppatore può scegliere quale linguaggio di programmazione (e versione specifica) desidera utilizzare.

Gli sviluppatori generalmente scrivono il loro codice utilizzando **framework web**. I framework web sono collezioni di funzioni, oggetti, regole e altre strutture di codice progettate per risolvere problemi comuni, accelerare lo sviluppo e semplificare i diversi tipi di attività affrontate in un particolare dominio.

Ancora una volta, mentre sia il codice lato client sia quello lato server utilizzano framework, i domini sono molto diversi, e quindi anche i framework lo sono. I framework web lato client semplificano i compiti di layout e presentazione mentre i framework web lato server forniscono molta "funzionalità web server comune" che altrimenti potresti dover implementare da solo (ad esempio, supporto per le sessioni, supporto per utenti e autenticazione, accesso facile al database, librerie per templating, ecc.).

> [!NOTE]
> I framework lato client sono spesso utilizzati per aiutare ad accelerare lo sviluppo del codice lato client, ma puoi anche scegliere di scrivere tutto il codice a mano; infatti, scrivere il tuo codice a mano può essere più veloce ed efficiente se hai bisogno solo di una UI del sito web piccola e semplice.
>
> Al contrario, quasi mai considereresti di scrivere la componente lato server di un'app web senza un framework — implementare una funzionalità vitale come un server HTTP è davvero difficile da fare da zero, in Python per esempio, ma i framework web di Python come Django ne offrono uno subito disponibile, insieme ad altri strumenti molto utili.

## Cosa puoi fare sul lato server?

La programmazione lato server è molto utile perché permette di fornire informazioni su misura per i singoli utenti in modo _efficiente_ e quindi creare un'esperienza utente molto migliore.

Aziende come Amazon utilizzano la programmazione lato server per costruire risultati di ricerca per i prodotti, fare suggerimenti di prodotti mirati basati sulle preferenze del cliente e abitudini di acquisto precedenti, semplificare gli acquisti, ecc.

Le banche usano la programmazione lato server per memorizzare le informazioni sui conti e permettere solo agli utenti autorizzati di visualizzare e effettuare transazioni. Altri servizi come Facebook, Twitter, Instagram e Wikipedia usano la programmazione lato server per evidenziare, condividere, e controllare l'accesso ai contenuti interessanti.

Alcuni degli usi comuni e dei benefici della programmazione lato server sono elencati di seguito. Noterai che c'è qualche sovrapposizione!

### Archiviazione ed erogazione efficiente delle informazioni

Immagina quanti prodotti siano disponibili su Amazon, e immagina quanti post siano stati scritti su Facebook? Creare una pagina statica separata per ogni prodotto o post sarebbe del tutto impraticabile.

La programmazione lato server ci permette invece di memorizzare le informazioni in un database e costruire dinamicamente e restituire HTML e altri tipi di file (ad esempio, PDF, immagini, ecc.). È anche possibile restituire i dati ({{Glossary("JSON", "JSON")}}, {{Glossary("XML", "XML")}}, ecc.) per il rendering da parte di framework web lato client appropriati (questo riduce il carico di elaborazione sul server e la quantità di dati che devono essere inviati).

Il server non è limitato a inviare informazioni dai database, e potrebbe alternativamente restituire il risultato di strumenti software o dati da servizi di comunicazione. Il contenuto può anche essere mirato al tipo di dispositivo client che lo sta ricevendo.

Poiché le informazioni sono in un database, possono anche essere più facilmente condivise e aggiornate con altri sistemi aziendali (ad esempio, quando i prodotti vengono venduti online o in un negozio, il negozio potrebbe aggiornare il suo database di inventario).

> [!NOTE]
> La tua immaginazione non deve lavorare duro per vedere il beneficio del codice lato server per l'archiviazione e l'erogazione efficiente delle informazioni:
>
> 1. Vai su [Amazon](https://www.amazon.com/) o un altro sito di e-commerce.
> 2. Cerca un numero di parole chiave e nota come la struttura della pagina non cambi, anche se i risultati sì.
> 3. Apri due o tre prodotti diversi. Nota come ancora una volta abbiano una struttura e layout comuni, ma il contenuto per diversi prodotti è stato estratto dal database.
>
> Per un termine di ricerca comune ("fish", per esempio), puoi vedere letteralmente milioni di valori restituiti. Utilizzare un database permette che questi vengano memorizzati e condivisi in modo efficiente, permettendo che la presentazione dell'informazione venga controllata in un solo posto.

### Esperienza utente personalizzata

I server possono memorizzare e utilizzare informazioni sui client per fornire un'esperienza utente comoda e personalizzata. Ad esempio, molti siti memorizzano le carte di credito in modo che i dettagli non debbano essere inseriti nuovamente. Siti come Google Maps possono usare posizioni salvate o attuali per fornire informazioni di routing, e cronologia di ricerca o di viaggio per evidenziare aziende locali nei risultati di ricerca.

Un'analisi più approfondita delle abitudini dell'utente può essere utilizzata per anticipare i loro interessi e personalizzare ulteriormente le risposte e le notifiche, ad esempio, fornendo una lista di località precedentemente visitate o popolari che potresti voler guardare su una mappa.

> **Nota:** [Google Maps](https://www.google.com/maps) salva la tua cronologia di ricerca e visita. Le località frequentemente visitate o cercate sono evidenziate più di altre.
>
> I risultati di ricerca Google sono ottimizzati in base alle ricerche precedenti.
>
> 1. Vai su [Google search](https://www.google.com/).
> 2. Cerca "calcio".
> 3. Ora prova a digitare "preferito" nella casella di ricerca e osserva i suggerimenti di ricerca completati automaticamente.
>
> Coincidenza? Niente affatto!

### Accesso controllato ai contenuti

La programmazione lato server permette ai siti di limitare l'accesso agli utenti autorizzati e di servire solo le informazioni che un utente è autorizzato a vedere.

Esempi reali includono i siti di social-networking che permettono agli utenti di determinare chi può vedere il contenuto che pubblicano sul sito e il cui contenuto appare nel loro feed.

> [!NOTE]
> Considera altri esempi reali in cui l'accesso ai contenuti è controllato. Ad esempio, cosa puoi vedere se accedi al sito online della tua banca? Accedi al tuo account — quali ulteriori informazioni puoi vedere e modificare? Quali informazioni puoi vedere che solo la banca può modificare?

### Memorizzazione delle informazioni di sessione/stato

La programmazione lato server permette agli sviluppatori di utilizzare le **sessioni** — fondamentalmente, un meccanismo che permette a un server di memorizzare informazioni associate all'utente corrente di un sito e inviare risposte diverse basate su quelle informazioni.

Questo permette, ad esempio, a un sito di sapere che un utente ha effettuato il login in precedenza e di visualizzare link alle loro email o alla cronologia degli ordini, o forse di salvare lo stato di un semplice gioco in modo che l'utente possa accedere nuovamente al sito e continuare da dove si era fermato.

> [!NOTE]
> Visita un sito di notizie che ha un modello di abbonamento e apri un mucchio di schede (ad esempio, [The Age](https://www.theage.com.au/)). Continua a visitare il sito per alcune ore/giorni. Alla fine, inizierai a essere reindirizzato a pagine che spiegano come iscriversi e sarai impossibilitato ad accedere agli articoli. Questa informazione è un esempio di informazioni di sessione memorizzate nei cookie.

### Notifiche e comunicazioni

I server possono inviare notifiche generali o specifiche per l'utente tramite il sito web stesso o via email, SMS, messaggistica istantanea, conversazioni video o altri servizi di comunicazione.

Alcuni esempi includono:

- Facebook e Twitter inviano email e messaggi SMS per notificarti di nuove comunicazioni.
- Amazon invia regolarmente email sui prodotti che suggeriscono prodotti simili a quelli già acquistati o visualizzati che potrebbero interessarti.
- Un server web potrebbe inviare messaggi di avviso agli amministratori del sito, avvertendo di memoria insufficiente sul server o di attività utente sospette.

> [!NOTE]
> Il tipo più comune di notifica è una "conferma della registrazione". Scegli quasi qualsiasi grande sito che ti interessa (Google, Amazon, Instagram, ecc.) e crea un nuovo account usando il tuo indirizzo email. Riceverai a breve un'email che conferma la tua registrazione o richiede un riconoscimento per attivare il tuo account.

### Analisi dei dati

Un sito web può raccogliere molti dati sugli utenti: cosa cercano, cosa comprano, cosa raccomandano, quanto tempo restano su ciascuna pagina. La programmazione lato server può essere utilizzata per affinare le risposte in base all'analisi di questi dati.

Ad esempio, sia Amazon che Google pubblicizzano prodotti basati su ricerche (e acquisti) precedenti.

> [!NOTE]
> Se sei un utente Facebook, vai al tuo feed principale e guarda il flusso di post. Nota come alcuni dei post siano fuori ordine numerico, in particolare, i post con più "mi piace" sono spesso più in alto nella lista rispetto a post più recenti.
>
> Guarda anche che tipo di annunci ti vengono mostrati — potresti vedere annunci per cose che hai guardato su altri siti. L'algoritmo di Facebook per evidenziare contenuti e pubblicità può essere un po' un mistero, ma è chiaro che dipende dai tuoi interessi e abitudini di visualizzazione!

## Sommario

Congratulazioni, hai raggiunto la fine del primo articolo sulla programmazione lato server.

Hai ora appreso che il codice lato server viene eseguito su un server web e che il suo ruolo principale è controllare _quale_ informazione viene inviata all'utente (mentre il codice lato client gestisce principalmente la struttura e la presentazione di quei dati all'utente).

Dovresti anche comprendere che è utile perché ci permette di creare siti web che _forniscono informazioni in modo efficiente_ su misura per singoli utenti e avere una buona idea di alcune delle cose che potresti essere in grado di fare quando sarai un programmatore lato server.

Infine, dovresti capire che il codice lato server può essere scritto in un numero di linguaggi di programmazione e che dovresti usare un framework web per rendere l'intero processo più facile.

In un articolo futuro ti aiuteremo a scegliere il miglior framework web per il tuo primo sito. Qui ti guideremo attraverso le principali interazioni client-server in solo un po' più di dettaglio.

{{NextMenu("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps")}}
