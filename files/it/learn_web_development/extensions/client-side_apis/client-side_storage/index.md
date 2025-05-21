---
title: Memorizzazione lato client
slug: Learn_web_development/Extensions/Client-side_APIs/Client-side_storage
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}

I browser web moderni supportano diversi modi per consentire ai siti web di memorizzare dati sul computer dell'utente — con il permesso dell'utente — e recuperarli quando necessario. Questo permette di conservare dati per archiviazioni a lungo termine, salvare siti o documenti per l'utilizzo offline, mantenere le impostazioni specifiche dell'utente per il tuo sito, e altro ancora. Questo articolo spiega le basi su come funzionano queste tecnologie.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">Nozioni base sugli oggetti JavaScript</a> e API core come <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">Richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti di memorizzazione lato client e quali sono le tecnologie chiave per abilitarla — API Web Storage, cookies, Cache API e IndexedDB API.</li>
          <li>Casi d'uso principali — mantenimento dello stato tra i ricaricamenti, persistenza dei login e dati di personalizzazione dell'utente, e lavoro locale/offline.</li>
          <li>Utilizzo di Web Storage per la semplice memorizzazione di coppie chiave-valore, controllato tramite JavaScript.</li>
          <li>Uso di IndexedDB per memorizzare dati più complessi e strutturati.</li>
          <li>Uso della Cache API e dei service worker per usi offline.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Memorizzazione lato client?

Altrove nell'area di apprendimento di MDN, abbiamo parlato della differenza tra [siti statici](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview#static_sites) e [siti dinamici](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview#dynamic_sites). La maggior parte dei siti web moderni principali sono dinamici — memorizzano dati sul server utilizzando una sorta di database (memorizzazione lato server), eseguono codice [lato server](/it/docs/Learn_web_development/Extensions/Server-side) per recuperare i dati necessari, inserirli in modelli di pagina statici, e servire l'HTML risultante al client per essere visualizzato dal browser dell'utente.

La memorizzazione lato client funziona su principi simili, ma ha usi differenti. Consiste in API JavaScript che permettono di memorizzare dati sul client (cioè, sulla macchina dell'utente) e poi recuperarli quando necessario. Questo ha molti usi distinti, come:

- Personalizzare le preferenze del sito (es. mostrare la scelta di widget personalizzati, schema di colori o dimensione dei caratteri sceltI dall'utente).
- Mantenere l'attività precedente del sito (es. memorizzare il contenuto di un carrello da una sessione precedente, ricordare se l'utente era precedentemente loggato).
- Salvare dati e asset localmente così che un sito sarà più veloce (e potenzialmente meno costoso) da scaricare, o essere utilizzabile senza una connessione di rete.
- Salvare documenti generati da applicazioni web localmente per l'uso offline.

Spesso la memorizzazione lato client e lato server vengono utilizzate insieme. Ad esempio, si potrebbe scaricare un lotto di file musicali (forse usati da un gioco web o da un'applicazione di lettore musicale), memorizzarli all'interno di un database lato client, e riprodurli secondo necessità. L'utente dovrebbe scaricare i file musicali solo una volta — alle visite successive verrebbero recuperati dal database invece.

> [!NOTE]
> Ci sono limiti alla quantità di dati che si possono memorizzare utilizzando le API di memorizzazione lato client (possibilmente sia per API individuale che complessivamente); il limite esatto varia a seconda del browser e possibilmente in base alle impostazioni dell'utente. Vedere [Quote di archiviazione del browser e criteri di sfratto](/it/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria) per maggiori informazioni.

### Vecchia scuola: Cookies

Il concetto di memorizzazione lato client esiste da molto tempo. Fin dai primi giorni del web, i siti hanno usato i [cookies](/it/docs/Web/HTTP/Guides/Cookies) per memorizzare informazioni per personalizzare l'esperienza utente sui siti web. Sono la prima forma di memorizzazione lato client comunemente usata sul web.

Oggi, ci sono meccanismi più semplici disponibili per la memorizzazione dei dati lato client, quindi non insegneremo come usare i cookies in questo articolo. Tuttavia, ciò non significa che i cookies siano completamente inutili sul web moderno — vengono ancora comunemente utilizzati per memorizzare dati relativi alla personalizzazione e allo stato dell'utente, ad esempio, ID di sessione e token di accesso. Per maggiori informazioni sui cookies, vedere il nostro articolo [Uso dei cookies HTTP](/it/docs/Web/HTTP/Guides/Cookies).

### Nuova scuola: Web Storage e IndexedDB

Le funzionalità "più semplici" menzionate sopra sono le seguenti:

- La [Web Storage API](/it/docs/Web/API/Web_Storage_API) fornisce un meccanismo per memorizzare e recuperare elementi di dati più piccoli costituiti da un nome e un valore corrispondente. È utile quando devi solo memorizzare dati semplici, come il nome dell'utente, se l'utente è loggato, quale colore usare per lo sfondo dello schermo, ecc.
- La [IndexedDB API](/it/docs/Web/API/IndexedDB_API) fornisce al browser un sistema completo di database per la memorizzazione di dati complessi. Può essere usata per cose che vanno da set completi di record dei clienti a tipi di dati complessi come file audio o video.

Imparerai di più su queste API di seguito.

### La Cache API

La [`Cache`](/it/docs/Web/API/Cache) API è progettata per memorizzare le risposte HTTP a richieste specifiche, ed è molto utile per fare cose come memorizzare asset del sito web offline in modo che il sito possa essere successivamente utilizzato senza una connessione di rete. La Cache è solitamente utilizzata in combinazione con la [Service Worker API](/it/docs/Web/API/Service_Worker_API), anche se non deve essere necessariamente usata con essa.

L'uso di Cache e Service Workers è un argomento avanzato, e non lo tratteremo in grande dettaglio in questo articolo, anche se mostreremo un esempio nella sezione [Memorizzazione di asset offline](#memorizzazione_di_asset_offline) di seguito.

## Memorizzare dati semplici — web storage

La [Web Storage API](/it/docs/Web/API/Web_Storage_API) è molto facile da usare — memorizzi semplici coppie nome/valore di dati (limitati a stringhe, numeri, ecc.) e recuperi questi valori quando necessario.

### Sintassi di base

Vediamo come funziona:

1. Per prima cosa, vai al nostro [modello vuoto per il web storage](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/web-storage/index.html) su GitHub (apri questo in una nuova scheda).
2. Apri la console JavaScript degli strumenti di sviluppo del tuo browser.
3. Tutti i tuoi dati di web storage sono contenuti all'interno di due strutture simili a oggetti nel browser: [`sessionStorage`](/it/docs/Web/API/Window/sessionStorage) e [`localStorage`](/it/docs/Web/API/Window/localStorage). Il primo conserva i dati finché il browser è aperto (i dati vengono persi quando il browser viene chiuso) e il secondo conserva i dati anche dopo che il browser è stato chiuso e riaperto. In questo articolo useremo il secondo poiché è generalmente più utile.

   Il metodo [`Storage.setItem()`](/it/docs/Web/API/Storage/setItem) ti permette di salvare un elemento di dati in storage — prende due parametri: il nome dell'elemento e il suo valore. Prova a digitare questo nella console JavaScript (cambia il valore nel tuo nome, se lo desideri!):

   ```js
   localStorage.setItem("name", "Chris");
   ```

4. Il metodo [`Storage.getItem()`](/it/docs/Web/API/Storage/getItem) prende un parametro — il nome di un elemento di dati che vuoi recuperare — e restituisce il valore dell'elemento. Ora digita queste righe nella tua console JavaScript:

   ```js
   let myName = localStorage.getItem("name");
   myName;
   ```

   Digitando la seconda riga, dovresti vedere che la variabile `myName` ora contiene il valore dell'elemento di dati `name`.

5. Il metodo [`Storage.removeItem()`](/it/docs/Web/API/Storage/removeItem) prende un parametro — il nome di un elemento di dati che vuoi rimuovere — e rimuove quell'elemento dal web storage. Digita le seguenti righe nella tua console JavaScript:

   ```js
   localStorage.removeItem("name");
   myName = localStorage.getItem("name");
   myName;
   ```

   La terza linea dovrebbe ora restituire `null` — l'elemento `name` non esiste più nel web storage.

### I dati persistono!

Una caratteristica chiave del web storage è che i dati persistono tra i caricamenti delle pagine (e persino quando il browser viene spento, nel caso di `localStorage`). Vediamo questo in azione.

1. Apri nuovamente il nostro modello vuoto per il web storage, ma questa volta in un browser diverso rispetto a quello con cui hai aperto questo tutorial! Questo renderà più facile il lavoro.
2. Digita queste righe nella console JavaScript del browser:

   ```js
   localStorage.setItem("name", "Chris");
   let myName = localStorage.getItem("name");
   myName;
   ```

   Dovresti vedere l'elemento `name` restituito.

3. Ora chiudi il browser e aprilo di nuovo.
4. Inserisci di nuovo le seguenti righe:

   ```js
   let myName = localStorage.getItem("name");
   myName;
   ```

   Dovresti vedere che il valore è ancora disponibile, anche se il browser è stato chiuso e riaperto.

### Memoria separata per ogni dominio

C'è un archivio dati separato per ogni dominio (ogni indirizzo web separato caricato nel browser). Vedrai che se carichi due siti web (ad esempio google.com e amazon.com) e provi a memorizzare un elemento su un sito, non sarà disponibile sull'altro sito.

Questo ha senso — puoi immaginare i problemi di sicurezza che sorgerebbero se i siti web potessero vedere i dati degli altri!

### Un esempio più articolato

Applichiamo questa nuova conoscenza scrivendo un esempio funzionante per darti un'idea di come può essere usato il web storage. Il nostro esempio ti permetterà di inserire un nome, dopodiché la pagina si aggiornerà per offrirti un saluto personalizzato. Questo stato persisterà anche tra i ricaricamenti della pagina/del browser, perché il nome è memorizzato nel web storage.

Puoi trovare l'esempio HTML su [personal-greeting.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/web-storage/personal-greeting.html) — questo contiene un sito web con un'intestazione, un contenuto e un piè di pagina, e un modulo per inserire il tuo nome.

![Una schermata di un sito web che ha sezioni di intestazione, contenuto e piè di pagina. L'intestazione ha un testo di benvenuto sulla sinistra e un pulsante etichettato "forget" sulla destra. Il contenuto ha un'intestazione seguita da due paragrafi di testo fittizio. Il piè di pagina recita "Copyright nessuno. Usa il codice come vuoi".](web-storage-demo.png)

Costruiamo l'esempio in modo che tu possa capire come funziona.

1. Innanzitutto, fai una copia locale del nostro file [personal-greeting.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/web-storage/personal-greeting.html) in una nuova directory sul tuo computer.
2. Nota come il nostro HTML faccia riferimento a un file JavaScript chiamato `index.js`, con una linea simile a `<script src="index.js" defer></script>`. Dobbiamo creare questo file e scrivere il nostro codice JavaScript al suo interno. Crea un file `index.js` nella stessa directory del tuo file HTML.
3. Cominceremo creando riferimenti a tutte le funzionalità HTML che abbiamo bisogno di manipolare in questo esempio — le creeremo tutte come costanti, poiché questi riferimenti non hanno bisogno di cambiare nel ciclo di vita dell'app. Aggiungi le seguenti righe al tuo file JavaScript:

   ```js
   // create needed constants
   const rememberDiv = document.querySelector(".remember");
   const forgetDiv = document.querySelector(".forget");
   const form = document.querySelector("form");
   const nameInput = document.querySelector("#entername");
   const submitBtn = document.querySelector("#submitname");
   const forgetBtn = document.querySelector("#forgetname");

   const h1 = document.querySelector("h1");
   const personalGreeting = document.querySelector(".personal-greeting");
   ```

4. Successivamente, dobbiamo includere un piccolo event listener per impedire che il modulo si sottoponga effettivamente quando il pulsante di invio viene premuto, poiché questo non è il comportamento che vogliamo. Aggiungi questo frammento sotto il tuo codice precedente:

   ```js
   // Stop the form from submitting when a button is pressed
   form.addEventListener("submit", (e) => e.preventDefault());
   ```

5. Ora dobbiamo aggiungere un event listener, la cui funzione gestore verrà eseguita quando il pulsante "Say hello" viene cliccato. I commenti spiegano in dettaglio cosa fa ogni parte, ma in sostanza qui stiamo prendendo il nome che l'utente ha inserito nella casella di testo di input e salvandolo nel web storage usando `setItem()`, per poi eseguire una funzione chiamata `nameDisplayCheck()` che gestirà l'aggiornamento del testo effettivo del sito web. Aggiungi questo alla fine del tuo codice:

   ```js
   // run function when the 'Say hello' button is clicked
   submitBtn.addEventListener("click", () => {
     // store the entered name in web storage
     localStorage.setItem("name", nameInput.value);
     // run nameDisplayCheck() to sort out displaying the personalized greetings and updating the form display
     nameDisplayCheck();
   });
   ```

6. A questo punto abbiamo anche bisogno di un gestore di eventi per eseguire una funzione quando viene cliccato il pulsante "Forget" — questo viene visualizzato solo dopo che è stato cliccato il pulsante "Say hello" (i due stati del modulo si alternano avanti e indietro). In questa funzione rimuoviamo l'elemento `name` dal web storage usando `removeItem()`, poi di nuovo eseguiamo `nameDisplayCheck()` per aggiornare il display. Aggiungi questo alla fine:

   ```js
   // run function when the 'Forget' button is clicked
   forgetBtn.addEventListener("click", () => {
     // Remove the stored name from web storage
     localStorage.removeItem("name");
     // run nameDisplayCheck() to sort out displaying the generic greeting again and updating the form display
     nameDisplayCheck();
   });
   ```

7. È ora di definire la funzione `nameDisplayCheck()` stessa. Qui verifichiamo se l'elemento `name` è stato memorizzato nel web storage usando `localStorage.getItem('name')` come test condizionale. Se il nome è stato memorizzato, questa chiamata valuterà `true`; se no, valuterà `false`. Se la chiamata valuta `true`, visualizziamo un saluto personalizzato, mostriamo la parte del modulo "forget" e nascondiamo la parte del modulo "Say hello". Se la chiamata valuta `false`, mostriamo un saluto generico e facciamo il contrario. Di nuovo, inserisci il seguente codice alla fine:

   ```js
   // define the nameDisplayCheck() function
   function nameDisplayCheck() {
     // check whether the 'name' data item is stored in web Storage
     if (localStorage.getItem("name")) {
       // If it is, display personalized greeting
       const name = localStorage.getItem("name");
       h1.textContent = `Welcome, ${name}`;
       personalGreeting.textContent = `Welcome to our website, ${name}! We hope you have fun while you are here.`;
       // hide the 'remember' part of the form and show the 'forget' part
       forgetDiv.style.display = "block";
       rememberDiv.style.display = "none";
     } else {
       // if not, display generic greeting
       h1.textContent = "Welcome to our website ";
       personalGreeting.textContent =
         "Welcome to our website. We hope you have fun while you are here.";
       // hide the 'forget' part of the form and show the 'remember' part
       forgetDiv.style.display = "none";
       rememberDiv.style.display = "block";
     }
   }
   ```

8. Ultimo ma non meno importante, dobbiamo eseguire la funzione `nameDisplayCheck()` quando la pagina viene caricata. Se non lo facessimo, allora il saluto personalizzato non persisterebbe attraverso i ricaricamentidella pagina. Aggiungi il seguente codice alla fine del tuo codice:

   ```js
   nameDisplayCheck();
   ```

Il tuo esempio è finito — ben fatto! Tutto ciò che resta da fare è salvare il tuo codice e testare la tua pagina HTML in un browser. Puoi vedere la nostra [versione finale in esecuzione dal vivo qui](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/web-storage/personal-greeting.html).

> [!NOTE]
> Esiste un altro esempio leggermente più complesso da esplorare in [Using the Web Storage API](/it/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API).

> [!NOTE]
> Nella linea `<script src="index.js" defer></script>` del sorgente per la nostra versione finale, l'attributo `defer` specifica che il contenuto dell'elemento {{htmlelement("script")}} non verrà eseguito fino a quando la pagina non avrà finito di caricarsi.

## Memorizzare dati complessi — IndexedDB

L'[API IndexedDB](/it/docs/Web/API/IndexedDB_API) (a volte abbreviata in IDB) è un sistema di database completo disponibile nel browser nel quale puoi memorizzare dati complessi correlati, i cui tipi non sono limitati a semplici valori come stringhe o numeri. Puoi memorizzare video, immagini e praticamente qualsiasi altra cosa in un'istanza IndexedDB.

L'API IndexedDB ti consente di creare un database, quindi creare tabelle di oggetti all'interno di quel database.
Le tabelle di oggetti sono come tabelle in un database relazionale, e ciascuna tabella di oggetti può contenere un numero di oggetti.
Per saperne di più sull'API IndexedDB, vedi [Using IndexedDB](/it/docs/Web/API/IndexedDB_API/Using_IndexedDB).

Tuttavia, questo ha un costo: IndexedDB è molto più complesso da usare rispetto alla Web Storage API. In questa sezione, solo gratteremo la superficie di ciò che è capace di fare, ma ti daremo abbastanza per iniziare.

### Svolgendo un esempio di memorizzazione di note

Qui ti guideremo attraverso un esempio che ti permette di memorizzare note nel tuo browser e visualizzarle e eliminarle quando vuoi, portandoti a costruirlo da solo e spiegando le parti più fondamentali di IDB mentre andiamo avanti.

L'app appare così:

![Screenshot demo delle note IntexDB con 4 sezioni. La prima sezione è l'intestazione. La seconda sezione elenca tutte le note che sono state create. Ha due note, ognuna con un pulsante di eliminazione. Una terza sezione è un modulo con 2 campi di input per 'Titolo della nota' e 'Testo della nota' e un pulsante etichettato 'Crea nuova nota'. La sezione in fondo al piè di pagina recita "Copyright nessuno. Usa il codice come vuoi".](idb-demo.png)

Ogni nota ha un titolo e del testo del corpo, ciascuno modificabile singolarmente. Il codice JavaScript che vedremo di seguito ha commenti dettagliati per aiutarti a capire cosa sta succedendo.

### Per iniziare

1. Innanzitutto, fai copie locali dei nostri file [`index.html`](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/index.html), [`style.css`](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/style.css), e [`index-start.js`](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/index-start.js) in una nuova directory sulla tua macchina locale.
2. Dai un'occhiata ai file. Vedrai che l'HTML definisce un sito web con un'intestazione e un piè di pagina, così come un'area di contenuto principale che contiene un posto per visualizzare le note e un modulo per inserire nuove note nel database. Il CSS fornisce qualche stile per chiarire cosa sta succedendo. Il file JavaScript contiene cinque costanti dichiarate contenenti riferimenti all'elemento {{htmlelement("ul")}} in cui le note verranno visualizzate, agli elementi {{htmlelement("input")}} per titolo e corpo, al {{htmlelement("form")}} stesso, e al {{htmlelement("button")}}.
3. Rinomina il tuo file JavaScript in `index.js`. Ora sei pronto per iniziare ad aggiungere codice.

### Configurazione iniziale del database

Ora vediamo cosa dobbiamo fare in primo luogo per effettivamente configurare un database.

1. Sotto le dichiarazioni delle costanti, aggiungi le seguenti righe:

   ```js
   // Create an instance of a db object for us to store the open database in
   let db;
   ```

   Qui stiamo dichiarando una variabile chiamata `db` — verrà successivamente utilizzata per memorizzare un oggetto che rappresenta il nostro database. La useremo in alcuni posti, quindi l'abbiamo dichiarata globalmente qui per facilitare le cose.

2. Successivamente, aggiungi il seguente:

   ```js
   // Open our database; it is created if it doesn't already exist
   // (see the upgradeneeded handler below)
   const openRequest = window.indexedDB.open("notes_db", 1);
   ```

   Questa linea crea una richiesta per aprire la versione `1` di un database chiamato `notes_db`. Se questo non esiste già, verrà creato per te dal codice successivo. Vedrai questo schema di richieste spesso utilizzato in IndexedDB. Le operazioni sui database richiedono tempo. Non vuoi bloccare il browser mentre aspetti i risultati, quindi le operazioni sui database sono {{Glossary("asynchronous", "asincrone")}}, il che significa che invece di avvenire immediatamente, avverranno a un certo punto in futuro, e verrai notificato quando sono terminate.

   Per gestire questo in IndexedDB, si crea un oggetto di richiesta (che può essere chiamato come preferisci — l'abbiamo chiamato `openRequest` qui, in modo che sia ovvio per cosa è usato). Si utilizzano quindi i gestori degli eventi per eseguire il codice quando la richiesta viene completata, fallisce, ecc., come vedrai in uso di seguito.

   > [!NOTE]
   > Il numero di versione è importante. Se vuoi aggiornare il tuo database (ad esempio, modificando la struttura delle tabelle), devi eseguire di nuovo il tuo codice con un numero di versione incrementato, uno schema diverso specificato all'interno del gestore `upgradeneeded` (vedi sotto), ecc. Non tratteremo l'aggiornamento dei database in questo tutorial.

3. Ora aggiungi i seguenti gestori degli eventi subito sotto il tuo precedente:

   ```js
   // error handler signifies that the database didn't open successfully
   openRequest.addEventListener("error", () =>
     console.error("Database failed to open"),
   );

   // success handler signifies that the database opened successfully
   openRequest.addEventListener("success", () => {
     console.log("Database opened successfully");

     // Store the opened database object in the db variable. This is used a lot below
     db = openRequest.result;

     // Run the displayData() function to display the notes already in the IDB
     displayData();
   });
   ```

   Il gestore dell'evento [`error`](/it/docs/Web/API/IDBRequest/error_event) verrà eseguito se il sistema torna dicendo che la richiesta è fallita. Questo ti permette di rispondere a questo problema. Nel nostro esempio, stampiamo semplicemente un messaggio sulla console JavaScript.

   Il gestore dell'evento [`success`](/it/docs/Web/API/IDBRequest/success_event) verrà eseguito se la richiesta è restituita con successo, il che significa che il database è stato aperto con successo. In questo caso, un oggetto che rappresenta il database aperto diventa disponibile nella proprietà [`openRequest.result`](/it/docs/Web/API/IDBRequest/result), permettendoci di manipolare il database. Lo memorizziamo nella variabile `db` che abbiamo creato in precedenza per utilizzarla più tardi. Eseguiamo anche una funzione chiamata `displayData()`, che visualizza i dati del database all'interno del {{HTMLElement("ul")}}. La eseguiamo ora affinché le note già esistenti nel database siano visualizzate appena la pagina viene caricata. Vedrai `displayData()` definito più avanti.

4. Infine, per questa sezione, aggiungeremo probabilmente il gestore degli eventi più importante per la configurazione del database: [`upgradeneeded`](/it/docs/Web/API/IDBOpenDBRequest/upgradeneeded_event). Questo gestore viene eseguito se il database non è già stato configurato, o se il database viene aperto con un numero di versione maggiore rispetto al database memorizzato esistente (quando si esegue un aggiornamento). Aggiungi il seguente codice sotto il tuo gestore precedente:

   ```js
   // Set up the database tables if this has not already been done
   openRequest.addEventListener("upgradeneeded", (e) => {
     // Grab a reference to the opened database
     db = e.target.result;

     // Create an objectStore in our database to store notes and an auto-incrementing key
     // An objectStore is similar to a 'table' in a relational database
     const objectStore = db.createObjectStore("notes_os", {
       keyPath: "id",
       autoIncrement: true,
     });

     // Define what data items the objectStore will contain
     objectStore.createIndex("title", "title", { unique: false });
     objectStore.createIndex("body", "body", { unique: false });

     console.log("Database setup complete");
   });
   ```

   Qui è dove definiamo lo schema (struttura) del nostro database; cioè, il set di colonne (o campi) che contiene. Qui prima otteniamo un riferimento al database esistente dalla proprietà `result` dell'oggetto target dell'evento (`e.target.result`), che è l'oggetto `request`. Questo è equivalente alla linea `db = openRequest.result;` all'interno del gestore dell'evento `success`, ma dobbiamo farlo separatamente qui perché il gestore dell'evento `upgradeneeded` (se necessario) verrà eseguito prima del gestore dell'evento `success`, il che significa che il valore `db` non sarebbe disponibile se non lo facessimo.

   Usiamo quindi [`IDBDatabase.createObjectStore()`](/it/docs/Web/API/IDBDatabase/createObjectStore) per creare un nuovo negozio di oggetti all'interno del nostro database aperto chiamato `notes_os`. Questo è equivalente a una singola tabella in un sistema convenzionale di database. Gli abbiamo dato il nome di `notes`, e abbiamo anche specificato un campo chiave `autoIncrement` chiamato `id` — in ciascun nuovo record questo verrà automaticamente dato un valore incrementato — lo sviluppatore non ha bisogno di impostarlo esplicitamente. Essendo la chiave, il campo `id` verrà utilizzato per identificare univocamente i record, come quando si elimina o si visualizza un record.

   Creiamo anche due altri indici (campi) usando il metodo [`IDBObjectStore.createIndex()`](/it/docs/Web/API/IDBObjectStore/createIndex): `title` (che conterrà un titolo per ciascuna nota), e `body` (che conterrà il testo del corpo della nota).

Quindi con questo schema del database configurato, quando iniziamo ad aggiungere record al database, ciascuno verrà rappresentato come un oggetto lungo queste linee:

```json
{
  "title": "Buy milk",
  "body": "Need both cows milk and soy.",
  "id": 8
}
```

### Aggiungere dati al database

Ora vediamo come possiamo aggiungere record al database. Questo verrà fatto usando il modulo sulla nostra pagina.

Sotto il tuo precedente gestore degli eventi, aggiungi la seguente linea, che imposta un gestore dell'evento `submit` che esegue una funzione chiamata `addData()` quando il modulo viene inviato (quando il {{htmlelement("button")}} di invio viene premuto portando a un invio del modulo avvenuto con successo):

```js
// Create a submit event handler so that when the form is submitted the addData() function is run
form.addEventListener("submit", addData);
```

Ora definiamo la funzione `addData()`. Aggiungi questa sotto la tua precedente linea:

```js
// Define the addData() function
function addData(e) {
  // prevent default - we don't want the form to submit in the conventional way
  e.preventDefault();

  // grab the values entered into the form fields and store them in an object ready for being inserted into the DB
  const newItem = { title: titleInput.value, body: bodyInput.value };

  // open a read/write db transaction, ready for adding the data
  const transaction = db.transaction(["notes_os"], "readwrite");

  // call an object store that's already been added to the database
  const objectStore = transaction.objectStore("notes_os");

  // Make a request to add our newItem object to the object store
  const addRequest = objectStore.add(newItem);

  addRequest.addEventListener("success", () => {
    // Clear the form, ready for adding the next entry
    titleInput.value = "";
    bodyInput.value = "";
  });

  // Report on the success of the transaction completing, when everything is done
  transaction.addEventListener("complete", () => {
    console.log("Transaction completed: database modification finished.");

    // update the display of data to show the newly added item, by running displayData() again.
    displayData();
  });

  transaction.addEventListener("error", () =>
    console.log("Transaction not opened due to error"),
  );
}
```

Questo è piuttosto complesso; scomponiamolo:

- Eseguiamo [`Event.preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento per fermare il modulo dall'effettuare l'invio nel modo convenzionale (ciò causerebbe un refresh della pagina e rovina l'esperienza).
- Creiamo un oggetto che rappresenta un record da inserire nel database, popolandolo con i valori degli input del modulo. Nota che non dobbiamo esplicitamente includere un valore di `id` — come abbiamo spiegato prima, questo viene auto-popolato.
- Apriamo una transazione di `readwrite` contro l'oggetto store `notes_os` usando il metodo [`IDBDatabase.transaction()`](/it/docs/Web/API/IDBDatabase/transaction). Questo oggetto transazione ci consente di accedere all'oggetto store in modo che possiamo fare qualcosa ad esso, ad esempio, aggiungere un nuovo record.
- Accediamo all'oggetto store usando il metodo [`IDBTransaction.objectStore()`](/it/docs/Web/API/IDBTransaction/objectStore), salvando il risultato nella variabile `objectStore`.
- Aggiungiamo il nuovo record al database usando [`IDBObjectStore.add()`](/it/docs/Web/API/IDBObjectStore/add). Questo crea un oggetto di richiesta, nello stesso modo visto prima.
- Aggiungiamo un gruppo di gestori degli eventi agli oggetti `request` e `transaction` per eseguire il codice in punti critici del ciclo di vita. Una volta che la richiesta ha avuto successo, svuotiamo gli input del modulo per prepararli all'inserimento della nota successiva. Una volta che la transazione è completata, eseguiamo di nuovo la funzione `displayData()` per aggiornare la visualizzazione delle note sulla pagina.

### Visualizzare i dati

Abbiamo già fatto riferimento a `displayData()` due volte nel nostro codice, quindi sarebbe probabilmente meglio definirlo. Aggiungi questo al tuo codice, sotto la definizione della funzione precedente:

```js
// Define the displayData() function
function displayData() {
  // Here we empty the contents of the list element each time the display is updated
  // If you didn't do this, you'd get duplicates listed each time a new note is added
  while (list.firstChild) {
    list.removeChild(list.firstChild);
  }

  // Open our object store and then get a cursor - which iterates through all the
  // different data items in the store
  const objectStore = db.transaction("notes_os").objectStore("notes_os");
  objectStore.openCursor().addEventListener("success", (e) => {
    // Get a reference to the cursor
    const cursor = e.target.result;

    // If there is still another data item to iterate through, keep running this code
    if (cursor) {
      // Create a list item, h3, and p to put each data item inside when displaying it
      // structure the HTML fragment, and append it inside the list
      const listItem = document.createElement("li");
      const h3 = document.createElement("h3");
      const para = document.createElement("p");

      listItem.appendChild(h3);
      listItem.appendChild(para);
      list.appendChild(listItem);

      // Put the data from the cursor inside the h3 and para
      h3.textContent = cursor.value.title;
      para.textContent = cursor.value.body;

      // Store the ID of the data item inside an attribute on the listItem, so we know
      // which item it corresponds to. This will be useful later when we want to delete items
      listItem.setAttribute("data-note-id", cursor.value.id);

      // Create a button and place it inside each listItem
      const deleteBtn = document.createElement("button");
      listItem.appendChild(deleteBtn);
      deleteBtn.textContent = "Delete";

      // Set an event handler so that when the button is clicked, the deleteItem()
      // function is run
      deleteBtn.addEventListener("click", deleteItem);

      // Iterate to the next item in the cursor
      cursor.continue();
    } else {
      // Again, if list item is empty, display a 'No notes stored' message
      if (!list.firstChild) {
        const listItem = document.createElement("li");
        listItem.textContent = "No notes stored.";
        list.appendChild(listItem);
      }
      // if there are no more cursor items to iterate through, say so
      console.log("Notes all displayed");
    }
  });
}
```

Di nuovo, scomponiamo:

- Per prima cosa, svuotiamo il contenuto dell'elemento {{htmlelement("ul")}}, per poi riempirlo con il contenuto aggiornato. Se non lo facessi, finirai per avere una grande lista di contenuti duplicati che vengono aggiunti ad ogni aggiornamento.
- Successivamente, otteniamo un riferimento all'oggetto store `notes_os` usando [`IDBDatabase.transaction()`](/it/docs/Web/API/IDBDatabase/transaction) e [`IDBTransaction.objectStore()`](/it/docs/Web/API/IDBTransaction/objectStore) come abbiamo fatto in `addData()`, eccetto che qui li stiamo concatenando insieme in una singola linea.
- Il passo successivo è utilizzare il metodo [`IDBObjectStore.openCursor()`](/it/docs/Web/API/IDBObjectStore/openCursor) per aprire una richiesta di cursore — questo è un costrutto che può essere usato per iterare sui record di un oggetto store. Concatenamo un gestore dell'evento `success` alla fine di questa linea per rendere il codice più conciso — quando il cursore viene restituito correttamente, il gestore viene eseguito.
- Otteniamo un riferimento al cursore stesso (un oggetto [`IDBCursor`](/it/docs/Web/API/IDBCursor)) usando `const cursor = e.target.result`.
- Successivamente, controlliamo se il cursore contiene un record dal datastore (`if (cursor){ }`) — se sì, creiamo un frammento DOM, lo popoliamo con i dati del record e lo inseriamo nella pagina (all'interno dell'elemento `<ul>`). Includiamo anche un pulsante di eliminazione che, quando viene cliccato, eliminerà quella nota eseguendo la funzione `deleteItem()`, che vedremo nella prossima sezione.
- Alla fine del blocco `if`, utilizziamo il metodo [`IDBCursor.continue()`](/it/docs/Web/API/IDBCursor/continue) per avanzare il cursore al record successivo nel datastore, ed eseguire di nuovo il contenuto del blocco `if`. Se c'è un altro record su cui iterare, ciò provoca la sua inserzione nella pagina, e poi `continue()` viene eseguito di nuovo, e così via.
- Quando non ci sono più record su cui iterare, `cursor` restituirà `undefined`, e quindi verrà eseguito il blocco `else` invece del blocco `if`. Questo blocco verifica se qualche nota è stata inserita nel `<ul>` — se no, inserisce un messaggio per dire che non è stata memorizzata alcuna nota.

### Eliminare una nota

Come detto sopra, quando viene premuto il pulsante di eliminazione di una nota, la nota viene eliminata. Questo viene ottenuto dalla funzione `deleteItem()`, che appare così:

```js
// Define the deleteItem() function
function deleteItem(e) {
  // retrieve the name of the task we want to delete. We need
  // to convert it to a number before trying to use it with IDB; IDB key
  // values are type-sensitive.
  const noteId = Number(e.target.parentNode.getAttribute("data-note-id"));

  // open a database transaction and delete the task, finding it using the id we retrieved above
  const transaction = db.transaction(["notes_os"], "readwrite");
  const objectStore = transaction.objectStore("notes_os");
  const deleteRequest = objectStore.delete(noteId);

  // report that the data item has been deleted
  transaction.addEventListener("complete", () => {
    // delete the parent of the button
    // which is the list item, so it is no longer displayed
    e.target.parentNode.parentNode.removeChild(e.target.parentNode);
    console.log(`Note ${noteId} deleted.`);

    // Again, if list item is empty, display a 'No notes stored' message
    if (!list.firstChild) {
      const listItem = document.createElement("li");
      listItem.textContent = "No notes stored.";
      list.appendChild(listItem);
    }
  });
}
```

- La prima parte di questo potrebbe richiedere qualche spiegazione — recuperiamo l'ID del record da eliminare usando `Number(e.target.parentNode.getAttribute('data-note-id'))` — ricorda che l'ID del record è stato salvato in un attributo `data-note-id` sull'`<li>` quando è stato visualizzato per la prima volta. Abbiamo pero bisogno di far passare l'attributo attraverso l'oggetto integrato globale [`Number()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number) poiché è di tipo stringa, e quindi non sarebbe riconosciuto dal database, che si aspetta un numero.
- Otteniamo quindi un riferimento all'oggetto store usando lo stesso schema visto precedentemente, e utilizziamo il metodo [`IDBObjectStore.delete()`](/it/docs/Web/API/IDBObjectStore/delete) per eliminare il record dal database, passandogli l'ID.
- Quando la transazione del database è completata, eliminiamo l'`<li>` della nota dal DOM, ed eseguiamo di nuovo il controllo per vedere se il `<ul>` è ora vuoto, inserendo un messaggio come appropriato.

Ecco, il tuo esempio dovrebbe ora funzionare.

Se stai avendo problemi con esso, sentiti libero di [confrontarlo con il nostro esempio live](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/indexeddb/notes/) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/index.js)).

### Memorizzare dati complessi tramite IndexedDB

Come menzionato sopra, IndexedDB può essere utilizzato per memorizzare più che solo stringhe di testo. Puoi memorizzare praticamente qualsiasi cosa tu voglia, compresi oggetti complessi come blob video o immagini. E non è molto più difficile da fare rispetto a qualsiasi altro tipo di dato.

Per dimostrare come fare, abbiamo scritto un altro esempio chiamato [IndexedDB video store](https://github.com/mdn/learning-area/tree/main/javascript/apis/client-side-storage/indexeddb/video-store) (vedi anche [qui un'istanza live](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/indexeddb/video-store/)). Quando esegui l'esempio per la prima volta, scarica tutti i video dalla rete, li memorizza in un database IndexedDB e quindi visualizza i video nell'interfaccia utente all'interno degli elementi {{htmlelement("video")}}. La seconda volta che lo esegui, trova i video nel database e li recupera da lì prima di visualizzarli — questo rende i caricamenti successivi molto più veloci e meno assetati di banda.

Analizziamo le parti più interessanti dell'esempio. Non lo guarderemo tutto — molto è simile all'esempio precedente, e il codice è ben commentato.

1. Per questo esempio, abbiamo memorizzato i nomi dei video da recuperare in un array di oggetti:

   ```js
   const videos = [
     { name: "crystal" },
     { name: "elf" },
     { name: "frog" },
     { name: "monster" },
     { name: "pig" },
     { name: "rabbit" },
   ];
   ```

2. Per cominciare, una volta che il database è stato aperto con successo, eseguiamo una funzione `init()`. Questa esegue un loop attraverso i diversi nomi di video, cercando di caricare un record identificato da ciascun nome dal database `videos`.

   Se ciascun video viene trovato nel database (verificato vedendo se `request.result` valuta `true` — se il record non è presente, sarà `undefined`), i suoi file video (memorizzati come blob) e il nome del video vengono passati direttamente alla funzione `displayVideo()` per posizionarli nell'interfaccia utente. Se non vengono trovati, il nome del video viene passato alla funzione `fetchVideoFromNetwork()` per, indovina, recuperare il video dalla rete.

   ```js
   function init() {
     // Loop through the video names one by one
     for (const video of videos) {
       // Open transaction, get object store, and get() each video by name
       const objectStore = db.transaction("videos_os").objectStore("videos_os");
       const request = objectStore.get(video.name);
       request.addEventListener("success", () => {
         // If the result exists in the database (is not undefined)
         if (request.result) {
           // Grab the videos from IDB and display them using displayVideo()
           console.log("taking videos from IDB");
           displayVideo(
             request.result.mp4,
             request.result.webm,
             request.result.name,
           );
         } else {
           // Fetch the videos from the network
           fetchVideoFromNetwork(video);
         }
       });
     }
   }
   ```

3. Il seguente frammento è tratto dall'interno di `fetchVideoFromNetwork()` — qui recuperiamo versioni MP4 e WebM del video utilizzando due richieste [`fetch()`](/it/docs/Web/API/Window/fetch) separate. Utilizziamo quindi il metodo [`Response.blob()`](/it/docs/Web/API/Response/blob) per estrarre il corpo di ciascuna risposta come un blob, dandoci una rappresentazione oggetto dei video che può essere memorizzata e visualizzata più tardi.

   Abbiamo però un problema qui — queste due richieste sono entrambe asincrone, ma vogliamo solo provare a visualizzare o memorizzare il video quando entrambe le promesse sono state soddisfatte. Fortunatamente c'è un metodo integrato che gestisce un tale problema — {{jsxref("Promise.all()")}}. Questo prende un argomento — riferimenti a tutte le singole promesse che vuoi controllare per la conclusione posizionati in un array — e restituisce una promessa che viene soddisfatta quando tutte le singole promesse sono soddisfatte.

   All'interno del gestore `then()` per questa promessa, chiamiamo la funzione `displayVideo()` come abbiamo fatto prima per visualizzare i video nell'interfaccia utente, quindi chiamiamo anche la funzione `storeVideo()` per memorizzare quei video nel database.

   ```js
   // Fetch the MP4 and WebM versions of the video using the fetch() function,
   // then expose their response bodies as blobs
   const mp4Blob = fetch(`videos/${video.name}.mp4`).then((response) =>
     response.blob(),
   );
   const webmBlob = fetch(`videos/${video.name}.webm`).then((response) =>
     response.blob(),
   );

   // Only run the next code when both promises have fulfilled
   Promise.all([mp4Blob, webmBlob]).then((values) => {
     // display the video fetched from the network with displayVideo()
     displayVideo(values[0], values[1], video.name);
     // store it in the IDB using storeVideo()
     storeVideo(values[0], values[1], video.name);
   });
   ```

4. Guardiamo ora `storeVideo()`. Questo è molto simile allo schema visto nell'esempio precedente per aggiungere dati al database — apriamo una transazione di `readwrite` e otteniamo una riferimento al nostro oggetto store `videos_os`, creiamo un oggetto che rappresenta il record da aggiungere al database, poi lo aggiungiamo utilizzando [`IDBObjectStore.add()`](/it/docs/Web/API/IDBObjectStore/add).

   ```js
   // Define the storeVideo() function
   function storeVideo(mp4, webm, name) {
     // Open transaction, get object store; make it a readwrite so we can write to the IDB
     const objectStore = db
       .transaction(["videos_os"], "readwrite")
       .objectStore("videos_os");

     // Add the record to the IDB using add()
     const request = objectStore.add({ mp4, webm, name });

     request.addEventListener("success", () =>
       console.log("Record addition attempt finished"),
     );
     request.addEventListener("error", () => console.error(request.error));
   }
   ```

5. Infine, abbiamo `displayVideo()`, che crea gli elementi DOM necessari per inserire il video nell'interfaccia utente e quindi li aggiunge alla pagina. Le parti più interessanti di questo sono quelle mostrate di seguito — per visualizzare effettivamente i nostri blob video in un elemento `<video>`, abbiamo bisogno di creare URL oggetto (URL interni che puntano ai blob video memorizzati in memoria) usando il metodo [`URL.createObjectURL()`](/it/docs/Web/API/URL/createObjectURL_static). Una volta fatto ciò, possiamo impostare gli URL oggetto come valori degli attributi `src` del nostro elemento {{htmlelement("source")}}, e funziona bene.

   ```js
   // Define the displayVideo() function
   function displayVideo(mp4Blob, webmBlob, title) {
     // Create object URLs out of the blobs
     const mp4URL = URL.createObjectURL(mp4Blob);
     const webmURL = URL.createObjectURL(webmBlob);

     // Create DOM elements to embed video in the page
     const article = document.createElement("article");
     const h2 = document.createElement("h2");
     h2.textContent = title;
     const video = document.createElement("video");
     video.controls = true;
     const source1 = document.createElement("source");
     source1.src = mp4URL;
     source1.type = "video/mp4";
     const source2 = document.createElement("source");
     source2.src = webmURL;
     source2.type = "video/webm";

     // Embed DOM elements into page
     section.appendChild(article);
     article.appendChild(h2);
     article.appendChild(video);
     video.appendChild(source1);
     video.appendChild(source2);
   }
   ```

## Memorizzazione di asset offline

L'esempio sopra mostra già come creare un'app che memorizzerà grandi asset in un database IndexedDB, evitando la necessità di scaricarli più di una volta. Questo è già un grande miglioramento per l'esperienza dell'utente, ma manca ancora una cosa — i file HTML, CSS e JavaScript principali devono ancora essere scaricati ogni volta che il sito viene accesso, il che significa che non funzionerà quando non c'è una connessione di rete.

![Schermata offline di Firefox con un'illustrazione di un personaggio dei cartoni animati sulla sinistra che tiene in mano una spina a due pin con la mano destra e una presa a due pin con la mano sinistra. Sul lato destro c'è un messaggio Modalità offline e un pulsante etichettato "Riprova".](ff-offline.png)

Questo è dove entrano in gioco i [Service workers](/it/docs/Web/API/Service_Worker_API) e la vicina [Cache API](/it/docs/Web/API/Cache).

Un service worker è un file JavaScript che viene registrato contro un determinato origin (sito web, o parte di esso a un certo dominio) quando viene accesso da un browser. Quando registrato, può controllare le pagine disponibili in quell'origine. Lo fa sedendosi tra una pagina caricata e la rete e intercettando le richieste di rete mirate a quell'origine.

Quando intercetta una richiesta, può fare qualsiasi cosa desideri con essa (vedi [idee di casi d'uso](/it/docs/Web/API/Service_Worker_API#other_use_case_ideas)), ma l'esempio classico è salvare le risposte di rete offline e poi fornire quelle in risposta a una richiesta anziché le risposte della rete. In effetti, permette di far funzionare un sito web completamente offline.

La Cache API è un altro meccanismo di memorizzazione lato client, con una differenza — è progettata per salvare le risposte HTTP, e quindi funziona molto bene con i service workers.

### Un esempio di service worker

Guardiamo un esempio, per darti un'idea di cosa potrebbe sembrare. Abbiamo creato un'altra versione dell'esempio di video store che abbiamo visto nella sezione precedente — questa funziona in modo identico, tranne che memorizza anche l'HTML, il CSS e il JavaScript nella Cache API tramite un service worker, consentendo all'esempio di funzionare offline!

Vedi [IndexedDB video store con service worker live](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/), e anche [vedi il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/client-side-storage/cache-sw/video-store-offline).

#### Registrare il service worker

La prima cosa da notare è che c'è un pezzo di codice extra posizionato nel file JavaScript principale (vedi [index.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/cache-sw/video-store-offline/index.js)). Prima, eseguiamo un test di rilevamento delle funzionalità per vedere se il membro `serviceWorker` è disponibile nell'oggetto [`Navigator`](/it/docs/Web/API/Navigator). Se ciò restituisce vero, allora sappiamo che almeno le basi dei service workers sono supportate. All'interno di ciò utilizziamo il metodo [`ServiceWorkerContainer.register()`](/it/docs/Web/API/ServiceWorkerContainer/register) per registrare un service worker contenuto nel file `sw.js` contro l'origine su cui risiede, in modo che possa controllare le pagine nella stessa directory di esso, o sottodirectory. Quando la sua promessa viene soddisfatta, si ritiene che il service worker sia registrato.

```js
// Register service worker to control making site work offline
if ("serviceWorker" in navigator) {
  navigator.serviceWorker
    .register(
      "/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js",
    )
    .then(() => console.log("Service Worker Registered"));
}
```

> [!NOTE]
> Il percorso dato al file `sw.js` è relativo all'origine del sito, non al file JavaScript che contiene il codice. Il service worker è su `https://mdn.github.io/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js`. L'origine è `https://mdn.github.io`, e quindi il dato percorso deve essere `/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js`. Se desiderassi ospitare questo esempio sul tuo server, avresti bisogno di cambiarlo di conseguenza. Questo è piuttosto confuso, ma deve funzionare in questo modo per motivi di sicurezza.

#### Installare il service worker

La prossima volta che qualsiasi pagina sotto il controllo del service worker viene accesso (es., quando l'esempio viene ricaricato), il service worker è installato contro quella pagina, il che significa che inizierà a controllarla. Quando ciò si verifica, un evento `install` viene generato contro il service worker; puoi scrivere codice all'interno del service worker stesso che risponderà all'installazione.

Guardiamo un esempio, nel file [sw.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js) (il service worker). Vedrai che il listener di installazione è registrato contro `self`. Questa parola chiave `self` è un modo per fare riferimento allo scope globale del service worker dall'interno del file del service worker.

All'interno del gestore `install`, usiamo il metodo [`ExtendableEvent.waitUntil()`](/it/docs/Web/API/ExtendableEvent/waitUntil), disponibile sull'oggetto evento, per segnalare che il browser non dovrebbe completare l'installazione del service worker fino alla soddisfazione corretta della promessa al suo interno.

Qui è dove vediamo la Cache API in azione. Utilizziamo il metodo [`CacheStorage.open()`](/it/docs/Web/API/CacheStorage/open) per aprire un nuovo oggetto cache in cui possono essere memorizzate le risposte (simile a un oggetto store IndexedDB). Questa promessa si conclude con un oggetto [`Cache`](/it/docs/Web/API/Cache) che rappresenta la cache `video-store`. Utilizziamo poi il metodo [`Cache.addAll()`](/it/docs/Web/API/Cache/addAll) per recuperare una serie di asset e aggiungerne le risposte alla cache.

```js
self.addEventListener("install", (e) => {
  e.waitUntil(
    caches
      .open("video-store")
      .then((cache) =>
        cache.addAll([
          "/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/",
          "/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/index.html",
          "/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/index.js",
          "/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/style.css",
        ]),
      ),
  );
});
```

Questo è tutto per ora, l'installazione è fatta.

#### Rispondere alle richieste successive

Con il service worker registrato e installato contro la nostra pagina HTML, e i rilevanti asset tutti aggiunti alla nostra cache, siamo quasi pronti. Rimane solo una cosa da fare: scrivere del codice per rispondere alle ulteriori richieste di rete.

Questo è ciò che fa il secondo pezzo di codice in `sw.js`. Aggiungiamo un altro listener allo scope globale del service worker, che esegue la funzione gestore quando viene sollevato l'evento `fetch`. Questo accade ogni volta che il browser fa una richiesta per un asset nella directory in cui il service worker è registrato.

All'interno del gestore, prima logghiamo l'URL dell'asset richiesto. Forniamo quindi una risposta personalizzata alla richiesta, usando il metodo [`FetchEvent.respondWith()`](/it/docs/Web/API/FetchEvent/respondWith).

All'interno di questo blocco, utilizziamo [`CacheStorage.match()`](/it/docs/Web/API/CacheStorage/match) per verificare se una richiesta corrispondente (cioè corrisponde all'URL) può essere trovata in qualsiasi cache. Questa promessa viene conclusa con la risposta corrispondente se viene trovata una corrispondenza, o `undefined` se non lo è.

Se viene trovata una corrispondenza, la restituiamo come risposta personalizzata. Se no, effettuiamo il [fetch()](/it/docs/Web/API/Window/fetch) dalla rete e restituiamo quello al suo posto.

```js
self.addEventListener("fetch", (e) => {
  console.log(e.request.url);
  e.respondWith(
    caches.match(e.request).then((response) => response || fetch(e.request)),
  );
});
```

E questo è tutto per il nostro service worker.
C'è un mondo di altre cose che puoi fare con essi — per molti dettagli in più, vedi il [service worker cookbook](https://github.com/mdn/serviceworker-cookbook).
Molte grazie a Paul Kinlan per il suo articolo [Adding a Service Worker and Offline into your Web App](https://developers.google.com/codelabs/pwa-training/pwa03--going-offline#0), che ha ispirato questo esempio.

#### Testare l'esempio offline

Per testare il nostro [esempio di service worker](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/), dovrai caricarlo un paio di volte per assicurarti che sia installato. Una volta fatto ciò, puoi:

- Provare a scollegare la tua rete/spegnere il tuo Wi-Fi.
- Selezionare _File > Work Offline_ se stai usando Firefox.
- Andare agli strumenti di sviluppo, poi scegliere _Application > Service Workers_, quindi selezionare la casella _Offline_ se stai usando Chrome.

Se aggiorni la tua pagina di esempio di nuovo, dovresti vederla caricare bene ancora. Tutto è memorizzato offline — gli asset della pagina in una cache, e i video in un database IndexedDB.

## Riepilogo

Questo è tutto per ora. Speriamo che tu abbia trovato utile la nostra panoramica delle tecnologie di memorizzazione lato client.

## Vedi anche

- [Web storage API](/it/docs/Web/API/Web_Storage_API)
- [IndexedDB API](/it/docs/Web/API/IndexedDB_API)
- [Cookies](/it/docs/Web/HTTP/Guides/Cookies)
- [Service worker API](/it/docs/Web/API/Service_Worker_API)

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}
