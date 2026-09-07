---
title: Archiviazione lato client
slug: Learn_web_development/Extensions/Client-side_APIs/Client-side_storage
l10n:
  sourceCommit: b5ee197a87ea18acbc4dd9544efa8c0e46253785
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}

I moderni browser web supportano diversi modi con cui i siti web possono archiviare dati sul computer dell'utente — con il permesso dell'utente — per poi recuperarli quando necessario. Questo consente di mantenere dati per l'archiviazione a lungo termine, salvare siti o documenti per l'uso offline, conservare impostazioni specifiche dell'utente per il sito e molto altro. Questo articolo spiega le basi del funzionamento di questi meccanismi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare con le <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">basi degli oggetti JavaScript</a> e con le API principali, quali lo <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">scripting del DOM</a> e le <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>I concetti dell'archiviazione lato client e le principali tecnologie che la rendono possibile: Web Storage API, cookie, Cache API e IndexedDB API.</li>
          <li>Casi d'uso principali: mantenere lo stato tra i ricaricamenti, rendere persistenti i dati di accesso e di personalizzazione dell'utente e lavorare localmente/offline.</li>
          <li>Uso di Web Storage per una semplice archiviazione di coppie chiave-valore, controllata da JavaScript.</li>
          <li>Uso di IndexedDB per archiviare dati strutturati più complessi.</li>
          <li>Uso della Cache API e dei service worker per casi d'uso offline.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Archiviazione lato client?

In altre parti dell'area di apprendimento di MDN, è stata illustrata la differenza tra [siti statici](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview#static_sites) e [siti dinamici](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview#dynamic_sites). La maggior parte dei principali siti web moderni è dinamica: archivia i dati sul server usando una qualche forma di database (archiviazione lato server), quindi esegue codice [lato server](/it/docs/Learn_web_development/Extensions/Server-side) per recuperare i dati necessari, inserirli in template di pagine statiche e fornire al client l'HTML risultante affinché venga visualizzato dal browser dell'utente.

L'archiviazione lato client funziona secondo principi simili, ma ha utilizzi diversi. È costituita da API JavaScript che consentono di archiviare dati sul client, ovvero sulla macchina dell'utente, e recuperarli quando necessario. Questo ha molti usi distinti, ad esempio:

- Personalizzare le preferenze del sito (ad esempio, mostrare la scelta dell'utente di widget personalizzati, schema di colori o dimensione del carattere).
- Rendere persistente l'attività precedente sul sito (ad esempio, archiviare il contenuto di un carrello degli acquisti di una sessione precedente, ricordare se un utente aveva già effettuato l'accesso).
- Salvare dati e risorse localmente affinché un sito sia più rapido (e potenzialmente meno costoso) da scaricare oppure utilizzabile senza una connessione di rete.
- Salvare localmente documenti generati da applicazioni web per l'uso offline.

Spesso l'archiviazione lato client e lato server viene usata insieme. Ad esempio, si potrebbe scaricare un insieme di file musicali, magari usati da un gioco web o da un'applicazione di riproduzione musicale, archiviarli in un database lato client e riprodurli quando necessario. L'utente dovrebbe scaricare i file musicali una sola volta: nelle visite successive verrebbero invece recuperati dal database.

> [!NOTE]
> Esistono limiti alla quantità di dati che è possibile archiviare usando le API di archiviazione lato client, eventualmente sia per singola API sia cumulativamente; il limite esatto varia in base al browser e potenzialmente alle impostazioni dell'utente. Per maggiori informazioni, vedere [quote di archiviazione del browser e criteri di espulsione](/it/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria).

### Vecchia scuola: cookie

Il concetto di archiviazione lato client esiste da molto tempo. Fin dai primi tempi del web, i siti hanno utilizzato i [cookie](/it/docs/Web/HTTP/Guides/Cookies) per archiviare informazioni e personalizzare l'esperienza dell'utente sui siti web. Sono la prima forma di archiviazione lato client comunemente usata sul web.

Oggi sono disponibili meccanismi più semplici per archiviare dati lato client, quindi questo articolo non insegnerà come usare i cookie. Ciò non significa però che i cookie siano completamente inutili nel web moderno: vengono ancora comunemente usati per archiviare dati relativi alla personalizzazione e allo stato dell'utente, ad esempio ID di sessione e token di accesso. Per maggiori informazioni sui cookie, vedere l'articolo [Uso dei cookie HTTP](/it/docs/Web/HTTP/Guides/Cookies).

### Nuova scuola: Web Storage e IndexedDB

Le funzionalità più semplici menzionate sopra sono le seguenti:

- La [Web Storage API](/it/docs/Web/API/Web_Storage_API) fornisce un meccanismo per archiviare e recuperare piccoli elementi di dati costituiti da un nome e un valore corrispondente. È utile quando occorre soltanto archiviare dati semplici, come il nome dell'utente, se ha effettuato l'accesso, quale colore usare per lo sfondo dello schermo e così via.
- La [IndexedDB API](/it/docs/Web/API/IndexedDB_API) fornisce al browser un sistema di database completo per archiviare dati complessi. Può essere usata per elementi che vanno da insiemi completi di record dei clienti fino a tipi di dati complessi come file audio o video.

Di seguito si apprenderà di più su queste API.

### La Cache API

L'API [`Cache`](/it/docs/Web/API/Cache) è progettata per archiviare risposte HTTP a richieste specifiche ed è molto utile, ad esempio, per salvare offline le risorse di un sito web affinché il sito possa essere usato successivamente senza una connessione di rete. Cache viene solitamente usata insieme alla [Service Worker API](/it/docs/Web/API/Service_Worker_API), anche se non è obbligatorio.

L'uso di Cache e dei Service Worker è un argomento avanzato e non verrà trattato in grande dettaglio in questo articolo, anche se verrà mostrato un esempio nella sezione [Archiviazione di risorse offline](#archiviazione_di_risorse_offline) qui sotto.

## Archiviare dati semplici — web storage

La [Web Storage API](/it/docs/Web/API/Web_Storage_API) è molto facile da usare: consente di archiviare semplici coppie nome/valore di dati (limitate a stringhe, numeri e così via) e di recuperare tali valori quando necessario.

### Sintassi di base

Vediamo come fare:

1. Per prima cosa, visitare il [template vuoto per web storage](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/web-storage/index.html) su GitHub (aprirlo in una nuova scheda).
2. Aprire la console JavaScript negli strumenti per sviluppatori del browser.
3. Tutti i dati di web storage sono contenuti in due strutture simili a oggetti nel browser: [`sessionStorage`](/it/docs/Web/API/Window/sessionStorage) e [`localStorage`](/it/docs/Web/API/Window/localStorage). La prima mantiene i dati finché il browser resta aperto (i dati vengono persi alla chiusura del browser), mentre la seconda mantiene i dati anche dopo aver chiuso e riaperto il browser. In questo articolo verrà usata la seconda, poiché in genere è più utile.

   Il metodo [`Storage.setItem()`](/it/docs/Web/API/Storage/setItem) consente di salvare un elemento di dati nell'archiviazione e accetta due parametri: il nome dell'elemento e il suo valore. Provare a digitare questo nella console JavaScript (modificando il valore con il proprio nome, se desiderato):

   ```js
   localStorage.setItem("name", "Chris");
   ```

4. Il metodo [`Storage.getItem()`](/it/docs/Web/API/Storage/getItem) accetta un parametro, ovvero il nome di un elemento di dati da recuperare, e restituisce il valore dell'elemento. Ora digitare queste righe nella console JavaScript:

   ```js
   let myName = localStorage.getItem("name");
   myName;
   ```

   Dopo aver digitato la seconda riga, la variabile `myName` dovrebbe ora contenere il valore dell'elemento di dati `name`.

5. Il metodo [`Storage.removeItem()`](/it/docs/Web/API/Storage/removeItem) accetta un parametro, ovvero il nome di un elemento di dati da rimuovere, e rimuove tale elemento dal web storage. Digitare le seguenti righe nella console JavaScript:

   ```js
   localStorage.removeItem("name");
   myName = localStorage.getItem("name");
   myName;
   ```

   La terza riga dovrebbe ora restituire `null`: l'elemento `name` non esiste più nel web storage.

### I dati persistono!

Una funzionalità fondamentale del web storage è che i dati persistono tra i caricamenti della pagina (e anche quando il browser viene chiuso, nel caso di `localStorage`). Vediamolo in azione.

1. Aprire di nuovo il template vuoto per web storage, ma questa volta in un browser diverso da quello in cui è aperto questo tutorial. Sarà più facile da gestire.
2. Digitare queste righe nella console JavaScript del browser:

   ```js
   localStorage.setItem("name", "Chris");
   let myName = localStorage.getItem("name");
   myName;
   ```

   Dovrebbe essere restituito l'elemento name.

3. Ora chiudere il browser e riaprirlo.
4. Immettere nuovamente le seguenti righe:

   ```js
   let myName = localStorage.getItem("name");
   myName;
   ```

   Il valore dovrebbe essere ancora disponibile, anche se il browser è stato chiuso e poi riaperto.

### Archiviazione separata per ogni dominio

Esiste un archivio dati separato per ogni dominio, ovvero per ogni indirizzo web distinto caricato nel browser. Caricando due siti web, ad esempio google.com e amazon.com, e provando ad archiviare un elemento su uno dei due siti, tale elemento non sarà disponibile sull'altro sito.

Questo è logico: si possono immaginare i problemi di sicurezza che sorgerebbero se i siti web potessero vedere i dati gli uni degli altri.

### Un esempio più articolato

Applichiamo queste nuove conoscenze scrivendo un esempio funzionante, per dare un'idea di come può essere usato il web storage. L'esempio consentirà di immettere un nome, dopodiché la pagina verrà aggiornata con un saluto personalizzato. Questo stato persisterà anche tra ricaricamenti della pagina o del browser, perché il nome viene archiviato nel web storage.

L'HTML di esempio è disponibile in [personal-greeting.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/web-storage/personal-greeting.html): contiene un sito web con intestazione, contenuto e piè di pagina, nonché un modulo per immettere il nome.

![Schermata di un sito web con sezioni di intestazione, contenuto e piè di pagina. L'intestazione presenta un testo di benvenuto sul lato sinistro e un pulsante denominato "forget" sul lato destro. Il contenuto presenta un'intestazione seguita da due paragrafi di testo segnaposto. Il piè di pagina riporta "Copyright nobody. Use the code as you like".](web-storage-demo.png)

Costruiamo l'esempio per comprendere come funziona.

1. Per prima cosa, creare una copia locale del file [personal-greeting.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/web-storage/personal-greeting.html) in una nuova directory sul computer.
2. Quindi, notare che l'HTML fa riferimento a un file JavaScript denominato `index.js`, con una riga come `<script src="index.js" defer></script>`. Occorre creare questo file e scrivervi il codice JavaScript. Creare un file `index.js` nella stessa directory del file HTML.
3. Si inizierà creando riferimenti a tutte le funzionalità HTML da manipolare nell'esempio. Verranno creati tutti come costanti, poiché questi riferimenti non devono cambiare durante il ciclo di vita dell'app. Aggiungere le seguenti righe al file JavaScript:

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

4. Successivamente, occorre includere un piccolo event listener per impedire al modulo di inviare effettivamente se stesso quando viene premuto il pulsante di invio, poiché non è il comportamento desiderato. Aggiungere questo frammento sotto il codice precedente:

   ```js
   // Stop the form from submitting when a button is pressed
   form.addEventListener("submit", (e) => e.preventDefault());
   ```

5. Ora occorre aggiungere un event listener la cui funzione gestore verrà eseguita quando viene fatto clic sul pulsante "Say hello". I commenti spiegano in dettaglio cosa fa ogni parte, ma in sostanza qui si prende il nome immesso dall'utente nella casella di input di testo e lo si salva nel web storage usando `setItem()`, quindi viene eseguita una funzione denominata `nameDisplayCheck()` che gestirà l'aggiornamento del testo effettivo del sito web. Aggiungere questo in fondo al codice:

   ```js
   // run function when the 'Say hello' button is clicked
   submitBtn.addEventListener("click", () => {
     // store the entered name in web storage
     localStorage.setItem("name", nameInput.value);
     // run nameDisplayCheck() to sort out displaying the personalized greetings and updating the form display
     nameDisplayCheck();
   });
   ```

6. A questo punto serve anche un gestore di eventi per eseguire una funzione quando viene fatto clic sul pulsante "Forget": viene visualizzato solo dopo aver fatto clic sul pulsante "Say hello" (i due stati del modulo si alternano). In questa funzione si rimuove l'elemento `name` dal web storage usando `removeItem()`, quindi si esegue di nuovo `nameDisplayCheck()` per aggiornare la visualizzazione. Aggiungere questo in fondo:

   ```js
   // run function when the 'Forget' button is clicked
   forgetBtn.addEventListener("click", () => {
     // Remove the stored name from web storage
     localStorage.removeItem("name");
     // run nameDisplayCheck() to sort out displaying the generic greeting again and updating the form display
     nameDisplayCheck();
   });
   ```

7. È ora il momento di definire la funzione `nameDisplayCheck()` stessa. Qui viene verificato se l'elemento name è stato archiviato nel web storage usando `localStorage.getItem('name')` come test condizionale. Se il nome è stato archiviato, questa chiamata viene valutata come `true`; in caso contrario, la chiamata viene valutata come `false`. Se la chiamata viene valutata come `true`, viene visualizzato un saluto personalizzato, viene mostrata la parte "forget" del modulo e viene nascosta la parte "Say hello" del modulo. Se la chiamata viene valutata come `false`, viene visualizzato un saluto generico e viene fatto l'opposto. Anche in questo caso, inserire il seguente codice in fondo:

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

8. Infine, occorre eseguire la funzione `nameDisplayCheck()` al caricamento della pagina. Senza farlo, il saluto personalizzato non persisterebbe tra i ricaricamenti della pagina. Aggiungere quanto segue in fondo al codice:

   ```js
   nameDisplayCheck();
   ```

L'esempio è completato, ottimo lavoro. Non resta che salvare il codice e testare la pagina HTML in un browser. È possibile vedere la [versione completata in esecuzione qui](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/web-storage/personal-greeting.html).

> [!NOTE]
> È disponibile un altro esempio, leggermente più complesso, da esplorare in [Uso della Web Storage API](/it/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API).

> [!NOTE]
> Nella riga `<script src="index.js" defer></script>` del sorgente della versione completata, l'attributo `defer` specifica che il contenuto dell'elemento {{htmlelement("script")}} non verrà eseguito finché la pagina non avrà completato il caricamento.

## Archiviare dati complessi — IndexedDB

La [IndexedDB API](/it/docs/Web/API/IndexedDB_API), talvolta abbreviata in IDB, è un sistema di database completo disponibile nel browser nel quale è possibile archiviare dati complessi e correlati, i cui tipi non sono limitati a valori semplici come stringhe o numeri. In un'istanza IndexedDB è possibile archiviare video, immagini e praticamente qualsiasi altra cosa.

La IndexedDB API consente di creare un database e poi creare object store all'interno di quel database.
Gli object store sono simili alle tabelle di un database relazionale e ciascun object store può contenere diversi oggetti.
Per ulteriori informazioni sulla IndexedDB API, vedere [Uso di IndexedDB](/it/docs/Web/API/IndexedDB_API/Using_IndexedDB).

Tuttavia, questo comporta un costo: IndexedDB è molto più complesso da usare rispetto alla Web Storage API. In questa sezione verrà solo scalfita la superficie delle sue capacità, ma saranno fornite informazioni sufficienti per iniziare.

### Esempio di archiviazione delle note

Qui verrà illustrato un esempio che consente di archiviare note nel browser, visualizzarle ed eliminarle in qualsiasi momento. L'esempio verrà costruito passo dopo passo, spiegando le parti più fondamentali di IDB lungo il percorso.

L'app ha un aspetto simile a questo:

![Schermata demo delle note IndexDB con 4 sezioni. La prima sezione è l'intestazione. La seconda sezione elenca tutte le note create. Contiene due note, ciascuna con un pulsante di eliminazione. Una terza sezione è un modulo con 2 campi di input per "Note title" e "Note text" e un pulsante denominato "Create new note". La sezione inferiore del piè di pagina riporta "Copyright nobody. Use the code as you like".](idb-demo.png)

Ogni nota ha un titolo e del testo del corpo, entrambi modificabili individualmente. Il codice JavaScript illustrato di seguito contiene commenti dettagliati per comprendere cosa sta accadendo.

### Per iniziare

1. Per prima cosa, creare copie locali dei file [`index.html`](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/index.html), [`style.css`](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/style.css) e [`index-start.js`](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/index-start.js) in una nuova directory sul computer locale.
2. Osservare i file. L'HTML definisce un sito web con intestazione e piè di pagina, nonché un'area del contenuto principale che contiene uno spazio per visualizzare le note e un modulo per inserire nuove note nel database. Il CSS fornisce alcuni stili per rendere più chiaro ciò che accade. Il file JavaScript contiene cinque costanti dichiarate con riferimenti all'elemento {{htmlelement("ul")}} in cui verranno visualizzate le note, agli elementi {{htmlelement("input")}} per il titolo e il corpo, al {{htmlelement("form")}} stesso e al {{htmlelement("button")}}.
3. Rinominare il file JavaScript in `index.js`. Ora è possibile iniziare ad aggiungervi codice.

### Configurazione iniziale del database

Vediamo ora cosa occorre fare inizialmente per configurare effettivamente un database.

1. Sotto le dichiarazioni delle costanti, aggiungere le seguenti righe:

   ```js
   // Create an instance of a db object for us to store the open database in
   let db;
   ```

   Qui viene dichiarata una variabile denominata `db`, che verrà poi utilizzata per archiviare un oggetto che rappresenta il database. Verrà usata in diversi punti, quindi è stata dichiarata qui globalmente per semplificare le cose.

2. Successivamente, aggiungere quanto segue:

   ```js
   // Open our database; it is created if it doesn't already exist
   // (see the upgradeneeded handler below)
   const openRequest = window.indexedDB.open("notes_db", 1);
   ```

   Questa riga crea una richiesta per aprire la versione `1` di un database denominato `notes_db`. Se non esiste già, verrà creato dal codice successivo. Questo modello di richieste viene usato molto spesso in IndexedDB. Le operazioni sul database richiedono tempo. Non si vuole bloccare il browser durante l'attesa dei risultati, quindi le operazioni sul database sono {{Glossary("asynchronous", "asincrone")}}, ovvero invece di avvenire immediatamente, avverranno in un momento futuro e verrà inviata una notifica quando saranno completate.

   Per gestire questo in IndexedDB, si crea un oggetto richiesta, che può avere qualsiasi nome: qui è stato chiamato `openRequest`, in modo che sia chiaro a cosa serve. Si utilizzano poi gestori di eventi per eseguire codice quando la richiesta viene completata, fallisce e così via, come verrà mostrato di seguito.

   > [!NOTE]
   > Il numero di versione è importante. Se si desidera aggiornare il database, ad esempio modificando la struttura della tabella, occorre eseguire di nuovo il codice con un numero di versione incrementato, uno schema diverso specificato all'interno del gestore `upgradeneeded` (vedere sotto) e così via. Questo tutorial non tratta l'aggiornamento dei database.

3. Ora aggiungere i seguenti gestori di eventi subito sotto l'aggiunta precedente:

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

   Il gestore dell'evento [`error`](/it/docs/Web/API/IDBRequest/error_event) verrà eseguito se il sistema restituisce un messaggio indicante che la richiesta non è riuscita. Ciò consente di rispondere al problema. Nell'esempio viene semplicemente stampato un messaggio nella console JavaScript.

   Il gestore dell'evento [`success`](/it/docs/Web/API/IDBRequest/success_event) verrà eseguito se la richiesta restituisce correttamente, ovvero se il database è stato aperto con successo. In questo caso, un oggetto che rappresenta il database aperto diventa disponibile nella proprietà [`openRequest.result`](/it/docs/Web/API/IDBRequest/result), consentendo di manipolare il database. Viene archiviato nella variabile `db` creata in precedenza, per usarlo successivamente. Viene inoltre eseguita una funzione denominata `displayData()`, che visualizza i dati del database all'interno dell'elemento {{HTMLElement("ul")}}. Viene eseguita ora affinché le note già presenti nel database vengano visualizzate non appena la pagina viene caricata. `displayData()` verrà definita più avanti.

4. Infine, per questa sezione, verrà aggiunto probabilmente il gestore di eventi più importante per configurare il database: [`upgradeneeded`](/it/docs/Web/API/IDBOpenDBRequest/upgradeneeded_event). Questo gestore viene eseguito se il database non è stato ancora configurato oppure se il database viene aperto con un numero di versione superiore a quello del database archiviato esistente, durante un aggiornamento. Aggiungere il seguente codice sotto il gestore precedente:

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

   Qui viene definito lo schema, ovvero la struttura, del database: l'insieme di colonne, o campi, che contiene. In questo caso viene innanzitutto ottenuto un riferimento al database esistente dalla proprietà `result` del target dell'evento (`e.target.result`), che è l'oggetto `request`. Questo equivale alla riga `db = openRequest.result;` all'interno del gestore dell'evento `success`, ma occorre farlo separatamente qui perché il gestore dell'evento `upgradeneeded`, se necessario, verrà eseguito prima del gestore dell'evento `success`; ciò significa che il valore `db` non sarebbe disponibile senza questa operazione.

   Si usa quindi [`IDBDatabase.createObjectStore()`](/it/docs/Web/API/IDBDatabase/createObjectStore) per creare un nuovo object store nel database aperto, denominato `notes_os`. Questo equivale a una singola tabella in un sistema di database convenzionale. Gli è stato assegnato il nome notes ed è stato specificato anche un campo chiave `autoIncrement` denominato `id`: a ogni nuovo record verrà assegnato automaticamente un valore incrementato, senza che lo sviluppatore debba impostarlo esplicitamente. Essendo la chiave, il campo `id` verrà usato per identificare univocamente i record, ad esempio quando si elimina o si visualizza un record.

   Vengono inoltre creati altri due indici, o campi, usando il metodo [`IDBObjectStore.createIndex()`](/it/docs/Web/API/IDBObjectStore/createIndex): `title`, che conterrà un titolo per ogni nota, e `body`, che conterrà il testo del corpo della nota.

Con questo schema di database configurato, quando si inizieranno ad aggiungere record al database, ciascuno sarà rappresentato da un oggetto simile al seguente:

```json
{
  "title": "Buy milk",
  "body": "Need both cows milk and soy.",
  "id": 8
}
```

### Aggiungere dati al database

Vediamo ora come aggiungere record al database. Questo verrà fatto usando il modulo nella pagina.

Sotto il gestore di eventi precedente, aggiungere la seguente riga, che configura un gestore dell'evento `submit` che esegue una funzione denominata `addData()` quando il modulo viene inviato, ovvero quando viene premuto il pulsante {{htmlelement("button")}} di invio portando a un invio corretto del modulo:

```js
// Create a submit event handler so that when the form is submitted the addData() function is run
form.addEventListener("submit", addData);
```

Ora definiamo la funzione `addData()`. Aggiungerla sotto la riga precedente:

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

Questo è piuttosto complesso; scomponendolo, vengono effettuate le seguenti operazioni:

- Viene eseguito [`Event.preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento per impedire che il modulo venga effettivamente inviato nel modo convenzionale, che causerebbe un aggiornamento della pagina e rovinerebbe l'esperienza.
- Viene creato un oggetto che rappresenta un record da inserire nel database, popolandolo con i valori degli input del modulo. Non è necessario includere esplicitamente un valore `id`: come spiegato in precedenza, viene popolato automaticamente.
- Viene aperta una transazione `readwrite` sull'object store `notes_os` usando il metodo [`IDBDatabase.transaction()`](/it/docs/Web/API/IDBDatabase/transaction). Questo oggetto transazione consente di accedere all'object store per effettuare un'operazione su di esso, ad esempio aggiungere un nuovo record.
- Si accede all'object store usando il metodo [`IDBTransaction.objectStore()`](/it/docs/Web/API/IDBTransaction/objectStore), salvando il risultato nella variabile `objectStore`.
- Il nuovo record viene aggiunto al database usando [`IDBObjectStore.add()`](/it/docs/Web/API/IDBObjectStore/add). Questo crea un oggetto richiesta, come già visto in precedenza.
- Viene aggiunto un insieme di gestori di eventi agli oggetti `request` e `transaction` per eseguire codice in punti critici del ciclo di vita. Una volta che la richiesta ha avuto successo, gli input del modulo vengono svuotati e resi pronti per l'immissione della nota successiva. Una volta completata la transazione, viene eseguita di nuovo la funzione `displayData()` per aggiornare la visualizzazione delle note nella pagina.

### Visualizzare i dati

`displayData()` è già stata richiamata due volte nel codice, quindi è opportuno definirla. Aggiungere questo al codice sotto la definizione della funzione precedente:

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

Anche qui, analizziamolo:

- Per prima cosa, viene svuotato il contenuto dell'elemento {{htmlelement("ul")}}, prima di riempirlo con il contenuto aggiornato. Senza questa operazione, a ogni aggiornamento verrebbe aggiunto contenuto duplicato, producendo un enorme elenco.
- Successivamente, viene ottenuto un riferimento all'object store `notes_os` usando [`IDBDatabase.transaction()`](/it/docs/Web/API/IDBDatabase/transaction) e [`IDBTransaction.objectStore()`](/it/docs/Web/API/IDBTransaction/objectStore), come fatto in `addData()`, ma qui vengono concatenati insieme in un'unica riga.
- Il passaggio successivo consiste nell'usare il metodo [`IDBObjectStore.openCursor()`](/it/docs/Web/API/IDBObjectStore/openCursor) per aprire una richiesta per un cursore: si tratta di un costrutto che può essere usato per iterare sui record in un object store. Alla fine di questa riga viene concatenato un gestore dell'evento `success` per rendere il codice più conciso: quando il cursore viene restituito correttamente, il gestore viene eseguito.
- Si ottiene un riferimento al cursore stesso, un oggetto [`IDBCursor`](/it/docs/Web/API/IDBCursor), usando `const cursor = e.target.result`.
- Successivamente, viene verificato se il cursore contiene un record dall'archivio dati (`if (cursor){ }`). In caso affermativo, viene creato un frammento DOM, popolato con i dati del record e inserito nella pagina, all'interno dell'elemento `<ul>`. Viene incluso anche un pulsante di eliminazione che, quando viene fatto clic su di esso, elimina quella nota eseguendo la funzione `deleteItem()`, che verrà esaminata nella sezione successiva.
- Alla fine del blocco `if`, viene usato il metodo [`IDBCursor.continue()`](/it/docs/Web/API/IDBCursor/continue) per avanzare il cursore al record successivo nell'archivio dati ed eseguire di nuovo il contenuto del blocco `if`. Se esiste un altro record su cui iterare, questo viene inserito nella pagina, quindi viene eseguito nuovamente `continue()` e così via.
- Quando non ci sono più record su cui iterare, `cursor` restituisce `undefined` e, di conseguenza, viene eseguito il blocco `else` anziché il blocco `if`. Questo blocco verifica se sono state inserite note nel `<ul>`; in caso contrario, inserisce un messaggio per indicare che non è stata archiviata alcuna nota.

### Eliminare una nota

Come indicato sopra, quando viene premuto il pulsante di eliminazione di una nota, la nota viene eliminata. Questo avviene tramite la funzione `deleteItem()`, che ha questo aspetto:

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

- La prima parte richiede una spiegazione: viene recuperato l'ID del record da eliminare usando `Number(e.target.parentNode.getAttribute('data-note-id'))`. Ricordare che l'ID del record è stato salvato in un attributo `data-note-id` sul `<li>` quando è stato visualizzato per la prima volta. Tuttavia, occorre passare l'attributo attraverso l'oggetto globale integrato [`Number()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number), poiché è di tipo stringa e quindi non sarebbe riconosciuto dal database, che si aspetta un numero.
- Viene quindi ottenuto un riferimento all'object store usando lo stesso modello visto in precedenza e viene usato il metodo [`IDBObjectStore.delete()`](/it/docs/Web/API/IDBObjectStore/delete) per eliminare il record dal database, passandogli l'ID.
- Al completamento della transazione del database, viene eliminato il `<li>` della nota dal DOM e viene effettuato nuovamente il controllo per verificare se il `<ul>` è ora vuoto, inserendo una nota se appropriato.

Questo è tutto. L'esempio dovrebbe ora funzionare.

In caso di difficoltà, è possibile [confrontarlo con l'esempio live](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/indexeddb/notes/) e consultare anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/index.js).

### Archiviare dati complessi tramite IndexedDB

Come menzionato sopra, IndexedDB può essere usato per archiviare più di semplici stringhe di testo. È possibile archiviare praticamente tutto ciò che si desidera, inclusi oggetti complessi come blob video o di immagini. Inoltre, non è molto più difficile da realizzare rispetto a qualsiasi altro tipo di dati.

Per dimostrare come farlo, è stato scritto un altro esempio chiamato [IndexedDB video store](https://github.com/mdn/learning-area/tree/main/javascript/apis/client-side-storage/indexeddb/video-store), disponibile anche [in esecuzione qui](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/indexeddb/video-store/). Quando l'esempio viene eseguito per la prima volta, scarica tutti i video dalla rete, li archivia in un database IndexedDB e poi visualizza i video nell'interfaccia utente all'interno di elementi {{htmlelement("video")}}. Alla seconda esecuzione, trova i video nel database e li recupera da lì prima di visualizzarli: questo rende i caricamenti successivi molto più rapidi e meno esigenti in termini di larghezza di banda.

Esaminiamo le parti più interessanti dell'esempio. Non verrà esaminato tutto: una buona parte è simile all'esempio precedente e il codice è ben commentato.

1. Per questo esempio, i nomi dei video da recuperare sono stati archiviati in un array di oggetti:

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

2. Per iniziare, una volta aperto correttamente il database viene eseguita una funzione `init()`. Questa scorre i diversi nomi dei video, cercando di caricare dal database `videos` un record identificato da ciascun nome.

   Se ciascun video viene trovato nel database, verificato controllando se `request.result` viene valutato come `true` — se il record non è presente, sarà `undefined` — i suoi file video, archiviati come blob, e il nome del video vengono passati direttamente alla funzione `displayVideo()` per posizionarli nell'interfaccia utente. In caso contrario, il nome del video viene passato alla funzione `fetchVideoFromNetwork()` per recuperare il video dalla rete.

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

3. Il seguente frammento è tratto dall'interno di `fetchVideoFromNetwork()`: qui vengono recuperate le versioni MP4 e WebM del video usando due richieste [`fetch()`](/it/docs/Web/API/Window/fetch) separate. Viene poi usato il metodo [`Response.blob()`](/it/docs/Web/API/Response/blob) per estrarre il corpo di ciascuna risposta come blob, ottenendo una rappresentazione a oggetti dei video che può essere archiviata e visualizzata in seguito.

   Tuttavia, qui c'è un problema: queste due richieste sono entrambe asincrone, ma si desidera provare a visualizzare o archiviare il video solo quando entrambe le promise sono state completate. Fortunatamente esiste un metodo integrato che gestisce questo problema: {{jsxref("Promise.all()")}}. Accetta un argomento: riferimenti a tutte le singole promise di cui si desidera verificare il completamento, inseriti in un array, e restituisce una promise che viene completata quando tutte le singole promise sono completate.

   All'interno del gestore `then()` di questa promise, viene chiamata la funzione `displayVideo()` come in precedenza per visualizzare i video nell'interfaccia utente, quindi viene chiamata anche la funzione `storeVideo()` per archiviare tali video nel database.

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

4. Vediamo prima `storeVideo()`. È molto simile al modello visto nell'esempio precedente per aggiungere dati al database: viene aperta una transazione `readwrite`, viene ottenuto un riferimento all'object store `videos_os`, viene creato un oggetto che rappresenta il record da aggiungere al database e quindi viene aggiunto usando [`IDBObjectStore.add()`](/it/docs/Web/API/IDBObjectStore/add).

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

5. Infine, c'è `displayVideo()`, che crea gli elementi DOM necessari per inserire il video nell'interfaccia utente e poi li aggiunge alla pagina. Le parti più interessanti sono quelle mostrate di seguito: per visualizzare effettivamente i blob video in un elemento `<video>`, occorre creare URL di oggetti, ovvero URL interni che puntano ai blob video archiviati in memoria, usando il metodo [`URL.createObjectURL()`](/it/docs/Web/API/URL/createObjectURL_static). Una volta fatto questo, è possibile impostare gli URL di oggetti come valori degli attributi `src` degli elementi {{htmlelement("source")}}, e funziona correttamente.

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

## Archiviazione di risorse offline

L'esempio precedente mostra già come creare un'app che archivia risorse di grandi dimensioni in un database IndexedDB, evitando la necessità di scaricarle più di una volta. Questo rappresenta già un grande miglioramento dell'esperienza utente, ma manca ancora un elemento: i file HTML, CSS e JavaScript principali devono comunque essere scaricati ogni volta che si accede al sito, il che significa che non funzionerà quando non è presente una connessione di rete.

![Schermata offline di Firefox con un'illustrazione di un personaggio dei cartoni animati sul lato sinistro che tiene una spina bipolare nella mano destra e una presa bipolare nella mano sinistra. Sul lato destro è presente un messaggio Offline Mode e un pulsante denominato "Try again".](ff-offline.png)

È qui che entrano in gioco i [Service worker](/it/docs/Web/API/Service_Worker_API) e la strettamente correlata [Cache API](/it/docs/Web/API/Cache).

Un service worker è un file JavaScript registrato per una particolare origine, ovvero un sito web o una parte di un sito web in un determinato dominio, quando viene raggiunto da un browser. Una volta registrato, può controllare le pagine disponibili in quell'origine. Lo fa posizionandosi tra una pagina caricata e la rete e intercettando le richieste di rete indirizzate a quell'origine.

Quando intercetta una richiesta, può eseguire qualsiasi operazione desiderata su di essa, vedere le [idee per casi d'uso](/it/docs/Web/API/Service_Worker_API#other_use_case_ideas), ma l'esempio classico consiste nel salvare offline le risposte di rete e poi fornirle in risposta a una richiesta al posto delle risposte dalla rete. In pratica, consente di far funzionare un sito web completamente offline.

La Cache API è un altro meccanismo di archiviazione lato client, con una piccola differenza: è progettata per salvare risposte HTTP e quindi funziona molto bene con i service worker.

### Un esempio di service worker

Vediamo un esempio per avere un'idea di come potrebbe apparire. È stata creata un'altra versione dell'esempio di archivio video visto nella sezione precedente: funziona in modo identico, tranne per il fatto che salva anche HTML, CSS e JavaScript nella Cache API tramite un service worker, consentendo all'esempio di funzionare offline.

Vedere [IndexedDB video store con service worker in esecuzione](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/) e anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/client-side-storage/cache-sw/video-store-offline).

#### Registrare il service worker

La prima cosa da notare è che è presente un ulteriore blocco di codice nel file JavaScript principale, vedere [index.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/cache-sw/video-store-offline/index.js). Innanzitutto, viene eseguito un test di rilevamento delle funzionalità per verificare se il membro `serviceWorker` è disponibile nell'oggetto [`Navigator`](/it/docs/Web/API/Navigator). Se restituisce true, allora si sa che almeno le funzionalità di base dei service worker sono supportate. Al suo interno, viene usato il metodo [`ServiceWorkerContainer.register()`](/it/docs/Web/API/ServiceWorkerContainer/register) per registrare un service worker contenuto nel file `sw.js` rispetto all'origine in cui risiede, affinché possa controllare le pagine nella stessa directory o nelle relative sottodirectory. Quando la sua promise viene completata, il service worker è considerato registrato.

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
> Il percorso fornito al file `sw.js` è relativo all'origine del sito, non al file JavaScript che contiene il codice. Il service worker si trova in `https://mdn.github.io/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js`. L'origine è `https://mdn.github.io`, pertanto il percorso fornito deve essere `/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js`. Per ospitare questo esempio sul proprio server, occorrerebbe modificarlo di conseguenza. Questo può risultare piuttosto confuso, ma deve funzionare in questo modo per ragioni di sicurezza.

#### Installare il service worker

La volta successiva che viene raggiunta una pagina sotto il controllo del service worker, ad esempio quando l'esempio viene ricaricato, il service worker viene installato rispetto a quella pagina, ovvero inizia a controllarla. Quando questo avviene, viene attivato un evento `install` rispetto al service worker; è possibile scrivere codice nel service worker stesso che risponda all'installazione.

Vediamo un esempio nel file [sw.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js), ovvero il service worker. Il listener di installazione è registrato su `self`. Questa parola chiave `self` consente di fare riferimento all'ambito globale del service worker dall'interno del file del service worker.

All'interno del gestore `install`, viene usato il metodo [`ExtendableEvent.waitUntil()`](/it/docs/Web/API/ExtendableEvent/waitUntil), disponibile sull'oggetto evento, per segnalare che il browser non deve completare l'installazione del service worker finché la promise al suo interno non viene completata correttamente.

Qui si vede la Cache API in azione. Viene usato il metodo [`CacheStorage.open()`](/it/docs/Web/API/CacheStorage/open) per aprire un nuovo oggetto cache nel quale possono essere archiviate risposte, in modo simile a un object store IndexedDB. Questa promise viene completata con un oggetto [`Cache`](/it/docs/Web/API/Cache) che rappresenta la cache `video-store`. Viene quindi usato il metodo [`Cache.addAll()`](/it/docs/Web/API/Cache/addAll) per recuperare una serie di risorse e aggiungere le relative risposte alla cache.

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

Per ora è tutto: installazione completata.

#### Rispondere alle richieste successive

Con il service worker registrato e installato rispetto alla pagina HTML e con tutte le risorse pertinenti aggiunte alla cache, è quasi tutto pronto. Resta solo un'ultima cosa da fare: scrivere del codice per rispondere alle ulteriori richieste di rete.

Questo è ciò che fa il secondo blocco di codice in `sw.js`. Viene aggiunto un altro listener all'ambito globale del service worker, che esegue la funzione gestore quando viene generato l'evento `fetch`. Ciò accade ogni volta che il browser effettua una richiesta per una risorsa nella directory su cui il service worker è registrato.

All'interno del gestore, viene innanzitutto registrato l'URL della risorsa richiesta. Viene quindi fornita una risposta personalizzata alla richiesta, usando il metodo [`FetchEvent.respondWith()`](/it/docs/Web/API/FetchEvent/respondWith).

All'interno di questo blocco, viene usato [`CacheStorage.match()`](/it/docs/Web/API/CacheStorage/match) per verificare se è possibile trovare in una qualsiasi cache una richiesta corrispondente, ovvero corrispondente all'URL. Questa promise viene completata con la risposta corrispondente se viene trovata una corrispondenza oppure con `undefined` in caso contrario.

Se viene trovata una corrispondenza, viene restituita come risposta personalizzata. In caso contrario, viene [recuperata](/it/docs/Web/API/Window/fetch) la risposta dalla rete e restituita invece quella.

```js
self.addEventListener("fetch", (e) => {
  console.log(e.request.url);
  e.respondWith(
    caches.match(e.request).then((response) => response || fetch(e.request)),
  );
});
```

Questo è tutto per il service worker.
Con essi è possibile fare molto altro: per molti più dettagli, vedere il [service worker cookbook](https://github.com/mdn/serviceworker-cookbook).
Un sentito ringraziamento a Paul Kinlan per il suo articolo [Adding a Service Worker and Offline into your Web App](https://developers.google.com/codelabs/pwa-training/pwa03--going-offline#0), che ha ispirato questo esempio.

#### Testare l'esempio offline

Per testare l'[esempio di service worker](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/), occorre caricarlo un paio di volte per assicurarsi che sia installato. Una volta fatto questo, è possibile:

- Provare a scollegare la rete o disattivare il Wi-Fi.
- Se si usa Firefox, selezionare _File > Work Offline_.
- Se si usa Chrome, andare agli strumenti di sviluppo, quindi scegliere _Application > Service Workers_ e selezionare la casella di controllo _Offline_.

Aggiornando nuovamente la pagina dell'esempio, dovrebbe comunque caricarsi correttamente. Tutto è archiviato offline: le risorse della pagina in una cache e i video in un database IndexedDB.

## Riepilogo

Per ora è tutto. Ci auguriamo che questa panoramica delle tecnologie di archiviazione lato client sia stata utile.

## Vedere anche

- [Web Storage API](/it/docs/Web/API/Web_Storage_API)
- [IndexedDB API](/it/docs/Web/API/IndexedDB_API)
- [Cookie](/it/docs/Web/HTTP/Guides/Cookies)
- [Service Worker API](/it/docs/Web/API/Service_Worker_API)

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}
