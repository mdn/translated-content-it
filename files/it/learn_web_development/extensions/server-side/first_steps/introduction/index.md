---
title: Introduzione al lato server
short-title: Introduction
slug: Learn_web_development/Extensions/Server-side/First_steps/Introduction
l10n:
  sourceCommit: 710372d69095aaeadfba6c892f3e39ed63df4c54
---

{{NextMenu("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps")}}

Benvenuti al corso di MDN sulla programmazione lato server per principianti! In questo primo articolo, verrà esaminata la programmazione lato server a un livello generale, rispondendo a domande quali "che cos'è?", "in che modo differisce dalla programmazione lato client?" e "perché è così utile?". Dopo aver letto questo articolo, sarà chiara la potenza aggiuntiva resa disponibile ai siti web tramite il codice lato server.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di cosa sia un web server.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con la programmazione lato server, con ciò che può
        fare e con il modo in cui differisce dalla programmazione lato client.
      </td>
    </tr>
  </tbody>
</table>

La maggior parte dei siti web di grandi dimensioni usa codice lato server per visualizzare dinamicamente dati diversi quando necessario, in genere estratti da un database archiviato su un server e inviati al client per essere visualizzati tramite del codice (ad esempio HTML e JavaScript).

Forse il vantaggio più significativo del codice lato server è che consente di adattare il contenuto del sito web ai singoli utenti. I siti dinamici possono mettere in evidenza contenuti più pertinenti in base alle preferenze e alle abitudini degli utenti. Possono anche rendere i siti più semplici da usare memorizzando preferenze e informazioni personali, ad esempio riutilizzando i dati della carta di credito archiviati per semplificare i pagamenti successivi.

Può persino consentire l'interazione con gli utenti del sito, inviando notifiche e aggiornamenti tramite email o altri canali. Tutte queste funzionalità permettono un coinvolgimento molto più profondo degli utenti.

Nel moderno mondo dello sviluppo web, è fortemente consigliato imparare lo sviluppo lato server.

## Che cos'è la programmazione lato server per siti web?

I browser web comunicano con i [web server](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) usando l'**H**yper**T**ext **T**ransfer **P**rotocol ({{Glossary("HTTP", "HTTP")}}). Quando si fa clic su un link in una pagina web, si invia un modulo o si esegue una ricerca, dal browser viene inviata al server di destinazione una **richiesta HTTP**.

La richiesta include un URL che identifica la risorsa interessata, un metodo che definisce l'azione richiesta (ad esempio ottenere, eliminare o pubblicare la risorsa) e può includere informazioni aggiuntive codificate nei parametri URL (le coppie campo-valore inviate tramite una [query string](https://en.wikipedia.org/wiki/Query_string)), come dati POST (dati inviati dal [metodo HTTP POST](/it/docs/Web/HTTP/Reference/Methods/POST)) o nei {{Glossary("Cookie", "cookie")}} associati.

I web server attendono i messaggi di richiesta dei client, li elaborano al loro arrivo e rispondono al browser web con un messaggio di **risposta HTTP**. La risposta contiene una riga di stato che indica se la richiesta ha avuto successo o meno (ad esempio, "HTTP/1.1 200 OK" in caso di successo).

Il corpo di una risposta riuscita a una richiesta contiene la risorsa richiesta (ad esempio, una nuova pagina HTML o un'immagine), che può quindi essere visualizzata dal browser web.

### Siti statici

Il diagramma seguente mostra un'architettura di base di un web server per un _sito statico_ (un sito statico restituisce lo stesso contenuto codificato staticamente dal server ogni volta che viene richiesta una particolare risorsa). Quando un utente desidera navigare a una pagina, il browser invia una richiesta HTTP "GET" che ne specifica l'URL.

Il server recupera il documento richiesto dal proprio file system e restituisce una risposta HTTP contenente il documento e uno [stato di successo](/it/docs/Web/HTTP/Reference/Status#successful_responses) (solitamente 200 OK). Se il file non può essere recuperato per qualche motivo, viene restituito uno stato di errore (vedere [risposte di errore del client](/it/docs/Web/HTTP/Reference/Status#client_error_responses) e [risposte di errore del server](/it/docs/Web/HTTP/Reference/Status#server_error_responses)).

![Diagramma semplificato di un web server statico.](basic_static_app_server.png)

### Siti dinamici

Un sito web dinamico è un sito in cui parte del contenuto della risposta viene generato _dinamicamente_, solo quando necessario. In un sito web dinamico, le pagine HTML vengono normalmente create inserendo dati da un database nei segnaposto dei template HTML (questo è un modo molto più efficiente per archiviare grandi quantità di contenuto rispetto all'uso di siti web statici).

Un sito dinamico può restituire dati diversi per un URL in base alle informazioni fornite dall'utente o alle preferenze archiviate e può eseguire altre operazioni come parte della restituzione di una risposta (ad esempio, inviare notifiche).

La maggior parte del codice necessario per supportare un sito web dinamico deve essere eseguita sul server. La creazione di questo codice è nota come "**programmazione lato server**" (o talvolta "**back-end scripting**").

Il diagramma seguente mostra un'architettura per un _sito web dinamico_. Come nel diagramma precedente, i browser inviano richieste HTTP al server, che quindi elabora le richieste e restituisce risposte HTTP appropriate.

Le richieste di risorse _statiche_ vengono gestite nello stesso modo dei siti statici (le risorse statiche sono tutti i file che non cambiano, in genere: CSS, JavaScript, immagini, file PDF pre-creati e così via).

![Diagramma semplificato di un web server che usa la programmazione lato server per ottenere informazioni da un database e costruire HTML dai template. È lo stesso diagramma presente nella panoramica client-server.](web_application_with_html_and_steps.png)

Le richieste di risorse dinamiche vengono invece inoltrate (2) al codice lato server (mostrato nel diagramma come _Web Application_). Per le "richieste dinamiche", il server interpreta la richiesta, legge le informazioni necessarie dal database (3), combina i dati recuperati con i template HTML (4) e invia una risposta contenente l'HTML generato (5,6).

## La programmazione lato server e lato client è la stessa cosa?

Passiamo ora al codice coinvolto nella programmazione lato server e lato client. In ciascun caso, il codice è significativamente diverso:

- Ha scopi e problematiche diversi.
- Generalmente non usa gli stessi linguaggi di programmazione (l'eccezione è JavaScript, che può essere usato sia sul lato server sia sul lato client).
- Viene eseguito in ambienti di sistemi operativi diversi.

Il codice eseguito nel browser è noto come **codice lato client** e riguarda principalmente il miglioramento dell'aspetto e del comportamento di una pagina web renderizzata. Ciò include la selezione e lo stile dei componenti UI, la creazione di layout, la navigazione, la validazione dei moduli e così via. Al contrario, la programmazione lato server per siti web consiste principalmente nello scegliere _quale contenuto_ viene restituito al browser in risposta alle richieste. Il codice lato server gestisce attività quali la validazione dei dati e delle richieste inviate, l'uso di database per archiviare e recuperare dati e l'invio al client dei dati corretti secondo necessità.

Il codice lato client viene scritto usando [HTML](/it/docs/Learn_web_development/Core/Structuring_content), [CSS](/it/docs/Learn_web_development/Core/Styling_basics) e [JavaScript](/it/docs/Learn_web_development/Core/Scripting): viene eseguito all'interno di un browser web e ha accesso minimo o nullo al sistema operativo sottostante (incluso un accesso limitato al file system).

Gli sviluppatori web non possono controllare quale browser ogni utente potrebbe usare per visualizzare un sito web: i browser offrono livelli di compatibilità non uniformi con le funzionalità del codice lato client e parte della sfida della programmazione lato client consiste nel gestire in modo adeguato le differenze nel supporto dei browser.

Il codice lato server può essere scritto in numerosi linguaggi di programmazione: esempi di linguaggi web lato server popolari includono PHP, Python, Ruby, C# e JavaScript (Node.js). Il codice lato server ha accesso completo al sistema operativo del server e lo sviluppatore può scegliere quale linguaggio di programmazione (e quale versione specifica) desidera usare.

Gli sviluppatori in genere scrivono il proprio codice usando **web framework**. I web framework sono raccolte di funzioni, oggetti, regole e altri costrutti di codice progettati per risolvere problemi comuni, accelerare lo sviluppo e semplificare i diversi tipi di attività affrontati in un particolare dominio.

Ancora una volta, sebbene sia il codice lato client sia quello lato server usino framework, i domini sono molto diversi e quindi lo sono anche i framework. I web framework lato client semplificano le attività di layout e presentazione, mentre i web framework lato server forniscono molte funzionalità "comuni" di un web server che altrimenti potrebbe essere necessario implementare autonomamente (ad esempio, supporto per le sessioni, supporto per utenti e autenticazione, facile accesso ai database, librerie di templating e così via).

> [!NOTE]
> I framework lato client vengono spesso usati per accelerare lo sviluppo del codice lato client, ma è anche possibile scegliere di scrivere tutto il codice manualmente; infatti, scrivere il codice manualmente può essere più rapido ed efficiente se serve soltanto una UI per sito web piccola e semplice.
>
> Al contrario, non si prenderebbe quasi mai in considerazione la scrittura del componente lato server di un'app web senza un framework: implementare da zero una funzionalità essenziale come un server HTTP è davvero difficile, ad esempio in Python, ma web framework Python come Django ne forniscono uno pronto all'uso, insieme ad altri strumenti molto utili.

## Cosa si può fare lato server?

La programmazione lato server è molto utile perché consente di fornire _in modo efficiente_ informazioni personalizzate per singoli utenti e quindi creare un'esperienza utente molto migliore.

Aziende come Amazon usano la programmazione lato server per costruire risultati di ricerca dei prodotti, proporre suggerimenti mirati sui prodotti in base alle preferenze del client e alle abitudini di acquisto precedenti, semplificare gli acquisti e così via.

Le banche usano la programmazione lato server per archiviare informazioni sugli account e consentire solo agli utenti autorizzati di visualizzare ed eseguire transazioni. Altri servizi come Facebook, Twitter, Instagram e Wikipedia usano la programmazione lato server per mettere in evidenza, condividere e controllare l'accesso a contenuti interessanti.

Di seguito sono elencati alcuni degli utilizzi e vantaggi comuni della programmazione lato server. Si noterà che vi è una certa sovrapposizione!

### Archiviazione e distribuzione efficiente delle informazioni

Immaginare quanti prodotti sono disponibili su Amazon e quanti post sono stati scritti su Facebook. Creare una pagina statica separata per ciascun prodotto o post sarebbe completamente impraticabile.

La programmazione lato server consente invece di archiviare le informazioni in un database e di costruire dinamicamente e restituire HTML e altri tipi di file (ad esempio PDF, immagini e così via). È inoltre possibile restituire dati ({{Glossary("JSON", "JSON")}}, {{Glossary("XML", "XML")}} e così via) da renderizzare tramite appropriati web framework lato client (ciò riduce il carico di elaborazione sul server e la quantità di dati da inviare).

Il server non è limitato all'invio di informazioni provenienti dai database e potrebbe invece restituire il risultato di strumenti software o dati provenienti da servizi di comunicazione. Il contenuto può persino essere destinato al tipo di dispositivo client che lo riceve.

Poiché le informazioni si trovano in un database, possono anche essere condivise e aggiornate più facilmente con altri sistemi aziendali (ad esempio, quando i prodotti vengono venduti online o in un negozio, il negozio potrebbe aggiornare il proprio database di inventario).

> [!NOTE]
> Non serve molta immaginazione per comprendere il vantaggio del codice lato server per l'archiviazione e la distribuzione efficiente delle informazioni:
>
> 1. Andare su [Amazon](https://www.amazon.com/) o su un altro sito di e-commerce.
> 2. Cercare alcune parole chiave e notare come la struttura della pagina non cambi, anche se i risultati cambiano.
> 3. Aprire due o tre prodotti diversi. Notare nuovamente come abbiano una struttura e un layout comuni, ma il contenuto dei diversi prodotti sia stato estratto dal database.
>
> Per un termine di ricerca comune ("fish", ad esempio) è possibile vedere letteralmente milioni di valori restituiti. L'uso di un database consente di archiviarli e condividerli in modo efficiente e permette di controllare la presentazione delle informazioni in un solo punto.

### Esperienza utente personalizzata

I server possono archiviare e usare informazioni sui client per offrire un'esperienza utente pratica e personalizzata. Ad esempio, molti siti archiviano le carte di credito in modo che non sia necessario inserire nuovamente i dettagli. Siti come Google Maps possono usare le posizioni salvate o correnti per fornire informazioni sul percorso e la cronologia delle ricerche o dei viaggi per mettere in evidenza le attività locali nei risultati di ricerca.

Un'analisi più approfondita delle abitudini degli utenti può essere usata per anticiparne gli interessi e personalizzare ulteriormente risposte e notifiche, ad esempio fornendo un elenco di località visitate in precedenza o popolari che potrebbero essere di interesse su una mappa.

> [!NOTE]
> [Google Maps](https://www.google.com/maps) salva la cronologia delle ricerche e delle visite. Le località visitate o cercate frequentemente vengono messe in evidenza più delle altre.
>
> I risultati di ricerca di Google sono ottimizzati in base alle ricerche precedenti.
>
> 1. Andare alla [ricerca Google](https://www.google.com/).
> 2. Cercare "football".
> 3. Ora provare a digitare "favorite" nella casella di ricerca e osservare le previsioni di ricerca del completamento automatico.
>
> Coincidenza? Macché!

### Accesso controllato ai contenuti

La programmazione lato server consente ai siti di limitare l'accesso agli utenti autorizzati e di fornire solo le informazioni che un utente è autorizzato a vedere.

Esempi reali includono i siti di social networking, che consentono agli utenti di determinare chi può vedere il contenuto che pubblicano sul sito e di chi sia il contenuto che appare nel loro feed.

> [!NOTE]
> Considerare altri esempi reali in cui l'accesso ai contenuti è controllato. Ad esempio, cosa si può vedere andando sul sito online della propria banca? Accedere al proprio account: quali informazioni aggiuntive è possibile visualizzare e modificare? Quali informazioni è possibile vedere che solo la banca può cambiare?

### Archiviare informazioni di sessione/stato

La programmazione lato server consente agli sviluppatori di usare le **sessioni**: in sostanza, un meccanismo che permette a un server di archiviare informazioni associate all'utente corrente di un sito e di inviare risposte diverse in base a tali informazioni.

Ciò consente, ad esempio, a un sito di sapere che un utente ha effettuato l'accesso in precedenza e di visualizzare link alle sue email o alla cronologia degli ordini, oppure di salvare lo stato di un semplice gioco affinché l'utente possa tornare a un sito e riprendere da dove aveva interrotto.

> [!NOTE]
> Visitare un sito di giornale con un modello di abbonamento e aprire numerose schede (ad esempio, [The Age](https://www.theage.com.au/)). Continuare a visitare il sito nel corso di alcune ore o giorni. Alla fine, si inizierà a essere reindirizzati a pagine che spiegano come abbonarsi e non sarà possibile accedere agli articoli. Queste informazioni sono un esempio di informazioni di sessione archiviate nei cookie.

### Notifiche e comunicazione

I server possono inviare notifiche generali o specifiche per l'utente tramite il sito web stesso oppure via email, SMS, messaggistica istantanea, videochiamate o altri servizi di comunicazione.

Alcuni esempi includono:

- Facebook e Twitter inviano email e messaggi SMS per notificare nuove comunicazioni.
- Amazon invia regolarmente email sui prodotti che suggeriscono prodotti simili a quelli già acquistati o visualizzati e che potrebbero essere di interesse.
- Un web server potrebbe inviare messaggi di avviso agli amministratori del sito, avvisandoli di memoria ridotta sul server o di attività utente sospette.

> [!NOTE]
> Il tipo più comune di notifica è una "conferma di registrazione". Scegliere quasi qualsiasi sito di grandi dimensioni di interesse (Google, Amazon, Instagram e così via) e creare un nuovo account usando il proprio indirizzo email. Poco dopo si riceverà un'email che conferma la registrazione o richiede una conferma per attivare l'account.

### Analisi dei dati

Un sito web può raccogliere molti dati sugli utenti: cosa cercano, cosa acquistano, cosa consigliano, quanto tempo rimangono su ogni pagina. La programmazione lato server può essere usata per perfezionare le risposte in base all'analisi di questi dati.

Ad esempio, Amazon e Google pubblicizzano entrambi prodotti in base alle ricerche precedenti (e agli acquisti).

> [!NOTE]
> Per gli utenti Facebook, andare al feed principale e osservare il flusso dei post. Notare come alcuni post non siano in ordine numerico: in particolare, i post con più "mi piace" sono spesso più in alto nell'elenco rispetto ai post più recenti.
>
> Osservare anche il tipo di annunci mostrati: potrebbero apparire annunci relativi a cose visualizzate su altri siti. L'algoritmo di Facebook per mettere in evidenza contenuti e pubblicità può essere in parte misterioso, ma è chiaro che dipende dai "mi piace" e dalle abitudini di visualizzazione!

## Riepilogo

Congratulazioni, è stata raggiunta la fine del primo articolo sulla programmazione lato server.

Ora è noto che il codice lato server viene eseguito su un web server e che il suo ruolo principale è controllare _quali_ informazioni vengono inviate all'utente (mentre il codice lato client gestisce principalmente la struttura e la presentazione di tali dati all'utente).

Dovrebbe essere inoltre chiaro che è utile perché consente di creare siti web che forniscono _in modo efficiente_ informazioni personalizzate per singoli utenti e di avere una buona idea di alcune delle attività possibili per chi programma lato server.

Infine, dovrebbe essere chiaro che il codice lato server può essere scritto in diversi linguaggi di programmazione e che è consigliabile usare un web framework per semplificare l'intero processo.

In un articolo futuro verrà spiegato come scegliere il miglior web framework per il primo sito. Qui verranno illustrate con un po' più di dettaglio le principali interazioni client-server.

{{NextMenu("Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview", "Learn_web_development/Extensions/Server-side/First_steps")}}
