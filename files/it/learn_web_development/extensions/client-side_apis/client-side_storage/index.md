---
title: Archiviazione lato client
slug: Learn_web_development/Extensions/Client-side_APIs/Client-side_storage
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}

I browser web moderni supportano diversi modi per i siti web di archiviare dati sul computer dell'utente — con il permesso dell'utente — e successivamente recuperarli quando necessario. Questo permette di conservare i dati per l'archiviazione a lungo termine, salvare siti o documenti per l'uso offline, mantenere le impostazioni specifiche dell'utente per il suo sito e altro ancora. Questo articolo spiega le basi di come funzionano queste tecnologie.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare le <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">basi degli oggetti JavaScript</a> e la copertura delle API di base come il <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e le <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti di archiviazione lato client e quali sono le tecnologie chiave per abilitarlo — Web Storage API, cookie, Cache API e l'API IndexedDB.</li>
          <li>Casi d'uso principali — mantenimento dello stato tra ricaricamenti, conservazione dei dati di accesso e personalizzazione utente, e lavoro locale/offline.</li>
          <li>Uso del Web Storage per l'archiviazione di semplici coppie chiave-valore, controllato da JavaScript.</li>
          <li>Uso di IndexedDB per archiviare dati più complessi e strutturati.</li>
          <li>Uso della Cache API e dei service workers per casi d'uso offline.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Archiviazione lato client?

In altre parti dell'area di apprendimento MDN, abbiamo parlato della differenza tra i [siti statici](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview#static_sites) e i [siti dinamici](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview#dynamic_sites). La maggior parte dei principali siti web moderni è dinamica — archiviano i dati sul server utilizzando un qualche tipo di database (archiviazione lato server), quindi eseguono codice [lato server](/it/docs/Learn_web_development/Extensions/Server-side) per recuperare i dati necessari, inserirli nei modelli di pagina statici e fornire l'HTML risultante al client per essere visualizzato dal browser dell'utente.

L'archiviazione lato client funziona su principi simili, ma ha usi diversi. Consiste in API JavaScript che consentono di archiviare dati sul client (cioè, sulla macchina dell'utente) e poi recuperarli quando necessario. Questo ha molti usi distinti, come:

- Personalizzazione delle preferenze del sito (ad esempio, mostrare una scelta di widget personalizzati, schema di colori o dimensione del carattere di un utente).
- Persistenza dell'attività precedente del sito (ad esempio, memorizzare il contenuto di un carrello della spesa da una sessione precedente, ricordare se un utente era precedentemente loggato).
- Salvataggio di dati e risorse localmente in modo che un sito sia più rapido (e potenzialmente meno costoso) da scaricare, o utilizzabile senza una connessione di rete.
- Salvataggio di documenti generati da applicazioni web localmente per l'uso offline.

Spesso l'archiviazione lato client e lato server vengono utilizzate insieme. Ad esempio, potrebbe scaricare un pacchetto di file musicali (forse utilizzati da un gioco web o da un'applicazione di riproduzione musicale), archiviarli all'interno di un database lato client e riprodurli secondo necessità. L'utente dovrebbe scaricare i file musicali solo una volta — nelle visite successive sarebbero recuperati dal database.

> [!NOTE]
> Ci sono limiti alla quantità di dati che si possono archiviare utilizzando le API di archiviazione lato client (possibilmente sia per singola API che cumulativamente); il limite esatto varia a seconda del browser e possibilmente delle impostazioni dell'utente. Vedere [Quote di archiviazione del browser e criteri di eliminazione](/it/docs/Web/API/Storage_API/Storage_quotas_and_eviction_criteria) per ulteriori informazioni.

### Vecchia scuola: Cookies

Il concetto di archiviazione lato client esiste da molto tempo. Fin dai primi giorni del web, i siti hanno utilizzato i [cookie](/it/docs/Web/HTTP/Guides/Cookies) per memorizzare informazioni e personalizzare l'esperienza utente sui siti web. Sono la forma più antica di archiviazione lato client comunemente usata sul web.

Al giorno d'oggi, ci sono meccanismi più facili disponibili per l'archiviazione dei dati lato client, pertanto non insegneremo come utilizzare i cookie in questo articolo. Tuttavia, ciò non significa che i cookie siano totalmente inutili sul web moderno — vengono ancora comunemente utilizzati per memorizzare dati relativi alla personalizzazione e allo stato dell'utente, ad esempio ID di sessione e token di accesso. Per ulteriori informazioni sui cookie, vedere il nostro articolo [Usare i cookie HTTP](/it/docs/Web/HTTP/Guides/Cookies).

### Nuova scuola: Web Storage e IndexedDB

Le caratteristiche "più facili" menzionate sopra sono le seguenti:

- La [Web Storage API](/it/docs/Web/API/Web_Storage_API) fornisce un meccanismo per l'archiviazione e il recupero di piccoli elementi di dati costituiti da un nome e un valore corrispondente. Questo è utile quando è necessario archiviare solo alcuni dati semplici, come il nome dell'utente, se sono connessi, quale colore usare per lo sfondo dello schermo, ecc.
- La [IndexedDB API](/it/docs/Web/API/IndexedDB_API) fornisce al browser un sistema di database completo per la memorizzazione di dati complessi. Può essere utilizzata per cose che vanno da set completi di record dei clienti a tipi di dati complessi come file audio o video.

Imparerà di più su queste API di seguito.

### La Cache API

La [`Cache`](/it/docs/Web/API/Cache) API è progettata per memorizzare risposte HTTP a richieste specifiche, ed è molto utile per fare cose come memorizzare risorse del sito offline in modo che il sito possa successivamente essere utilizzato senza una connessione di rete. La cache viene di solito usata in combinazione con la [Service Worker API](/it/docs/Web/API/Service_Worker_API), anche se non deve per forza esserlo.

L'uso della Cache e dei Service Workers è un argomento avanzato, e non lo tratteremo in grande dettaglio in questo articolo, anche se mostreremo un esempio nella sezione [Archiviazione delle risorse offline](#archiviazione_delle_risorse_offline) qui sotto.

## Archiviazione di dati semplici — Web Storage

La [Web Storage API](/it/docs/Web/API/Web_Storage_API) è molto facile da usare — consente di archiviare semplici coppie nome/valore di dati (limitati a stringhe, numeri, ecc.) e recuperare questi valori quando necessario.

### Sintassi di base

Ecco come fare:

1. Prima, accedere al nostro [template vuoto di archiviazione web](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/web-storage/index.html) su GitHub (aprirlo in una nuova scheda).
2. Aprire la console JavaScript degli strumenti per sviluppatori del browser.
3. Tutti i dati di archiviazione web sono contenuti all'interno di due strutture simili ad oggetti all'interno del browser: [`sessionStorage`](/it/docs/Web/API/Window/sessionStorage) e [`localStorage`](/it/docs/Web/API/Window/localStorage). Il primo persiste i dati finché il browser è aperto (i dati vengono persi quando il browser viene chiuso) e il secondo persiste i dati anche dopo che il browser viene chiuso e poi riaperto. Useremo il secondo in questo articolo in quanto generalmente più utile.

   Il metodo [`Storage.setItem()`](/it/docs/Web/API/Storage/setItem) consente di salvare un elemento di dati in archiviazione — prende due parametri: il nome dell'elemento e il suo valore. Provate a digitare questo nella vostra console JavaScript (potete cambiare il valore con il vostro nome, se desiderate!):

   ```js
   localStorage.setItem("name", "Chris");
   ```

4. Il metodo [`Storage.getItem()`](/it/docs/Web/API/Storage/getItem) prende un parametro — il nome di un elemento di dati che si vuole recuperare — e restituisce il valore dell'elemento. Ora inserite queste righe nella vostra console JavaScript:

   ```js
   let myName = localStorage.getItem("name");
   myName;
   ```

   Dopo aver digitato la seconda riga, dovreste vedere che la variabile `myName` ora contiene il valore dell'elemento `name`.

5. Il metodo [`Storage.removeItem()`](/it/docs/Web/API/Storage/removeItem) prende un parametro — il nome di un elemento di dati che si vuole rimuovere — e rimuove quell'elemento dall'archiviazione web. Digitate le seguenti righe nella vostra console JavaScript:

   ```js
   localStorage.removeItem("name");
   myName = localStorage.getItem("name");
   myName;
   ```

   La terza riga dovrebbe ora restituire `null` — l'elemento `name` non esiste più nell'archiviazione web.

### I dati persistono!

Una caratteristica chiave dell'archiviazione web è che i dati persistono tra i caricamenti delle pagine (e persino quando il browser viene chiuso, nel caso del `localStorage`). Vediamo questo in azione.

1. Aprite di nuovo il nostro template vuoto di archiviazione web, ma stavolta in un browser diverso da quello in cui avete aperto questo tutorial! Ciò renderà più facile gestirlo.
2. Digitare queste righe nella console JavaScript del browser:

   ```js
   localStorage.setItem("name", "Chris");
   let myName = localStorage.getItem("name");
   myName;
   ```

   Si dovrebbe vedere l'elemento name restituito.

3. Ora chiudere il browser e riaprirlo.
4. Inserire di nuovo le seguenti righe:

   ```js
   let myName = localStorage.getItem("name");
   myName;
   ```

   Si dovrebbe vedere che il valore è ancora disponibile, anche se il browser è stato chiuso e poi riaperto.

### Archiviazione separata per ogni dominio

Esiste un archivio dati separato per ogni dominio (ogni indirizzo web separato caricato nel browser). Vedrete che se caricate due siti web (diciamo google.com e amazon.com) e provate a memorizzare un elemento su un sito web, non sarà disponibile per l'altro sito web.

Questo ha senso — è possibile immaginare i problemi di sicurezza che sorgerebbero se i siti web potessero vedere i dati degli altri!

### Un esempio più coinvolto

Applichiamo questa conoscenza appena acquisita scrivendo un esempio pratico per darvi un'idea di come può essere utilizzato il web storage. Il nostro esempio vi permetterà di inserire un nome, dopodiché la pagina verrà aggiornata per fornirvi un saluto personalizzato. Questo stato persisterà anche attraverso i ricaricamenti delle pagine/browser, poiché il nome viene memorizzato nel web storage.

Potete trovare l'HTML dell'esempio su [personal-greeting.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/web-storage/personal-greeting.html) — questo contiene un sito web con un'intestazione, contenuto e piè di pagina, e un modulo per inserire il vostro nome.

![Uno screenshot di un sito web che ha un'intestazione, sezioni di contenuto e piè di pagina. L'intestazione ha un testo di benvenuto sul lato sinistro e un pulsante etichettato 'forget' sul lato destro. Il contenuto ha un'intestazione seguita da due paragrafi di testo fittizio. Il piè di pagina riporta 'Copyright nobody. Use the code as you like'.](web-storage-demo.png)

Costruiamo l'esempio, in modo che possiate comprendere come funziona.

1. In primo luogo, fare una copia locale del nostro file [personal-greeting.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/web-storage/personal-greeting.html) in una nuova directory sul vostro computer.
2. Successivamente, notate come il nostro HTML riferisca a un file JavaScript chiamato `index.js`, con una riga come `<script src="index.js" defer></script>`. Dobbiamo creare questo e scrivere il nostro codice JavaScript al suo interno. Creare un file `index.js` nella stessa directory del file HTML.
3. Inizieremo creando riferimenti a tutte le funzionalità HTML che dobbiamo manipolare in questo esempio — li creeremo tutti come costanti, poiché questi riferimenti non devono cambiare nel ciclo di vita dell'app. Aggiungere le seguenti righe al file JavaScript:

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

4. Successivamente, dobbiamo includere un piccolo event listener per impedire al modulo di inviare effettivamente se stesso quando viene premuto il pulsante di invio, poiché questo non è il comportamento che vogliamo. Aggiungere questo frammento sotto il vostro codice precedente:

   ```js
   // Stop the form from submitting when a button is pressed
   form.addEventListener("submit", (e) => e.preventDefault());
   ```

5. Ora dobbiamo aggiungere un event listener, la cui funzione handler verrà eseguita quando viene cliccato il pulsante "Say hello". I commenti spiegano in dettaglio cosa fa ciascun pezzo, ma in sostanza qui prendiamo il nome dell'utente inserito nella casella di input del testo e lo salviamo nel web storage utilizzando `setItem()`, quindi eseguiamo una funzione chiamata `nameDisplayCheck()` che gestirà l'aggiornamento del testo effettivo del sito web. Aggiungere questo alla fine del vostro codice:

   ```js
   // run function when the 'Say hello' button is clicked
   submitBtn.addEventListener("click", () => {
     // store the entered name in web storage
     localStorage.setItem("name", nameInput.value);
     // run nameDisplayCheck() to sort out displaying the personalized greetings and updating the form display
     nameDisplayCheck();
   });
   ```

6. A questo punto abbiamo anche bisogno di un event handler per eseguire una funzione quando viene cliccato il pulsante "Forget" — questo è visualizzato solo dopo che è stato cliccato il pulsante "Say hello" (i due stati del modulo si alternano). In questa funzione rimuoviamo l'elemento `name` dal web storage utilizzando `removeItem()`, quindi eseguiamo `nameDisplayCheck()` per aggiornare il display. Aggiungere questo alla fine:

   ```js
   // run function when the 'Forget' button is clicked
   forgetBtn.addEventListener("click", () => {
     // Remove the stored name from web storage
     localStorage.removeItem("name");
     // run nameDisplayCheck() to sort out displaying the generic greeting again and updating the form display
     nameDisplayCheck();
   });
   ```

7. È ora il momento di definire la funzione `nameDisplayCheck()` stessa. Qui controlliamo se l'elemento name è stato archiviato nel web storage utilizzando `localStorage.getItem('name')` come test condizionale. Se il nome è stato memorizzato, questa chiamata tornerà `true`; se no, tornerà `false`. Se la chiamata valuta `true`, visualizziamo un saluto personalizzato, visualizziamo la parte "Forget" del modulo e nascondiamo la parte "Say hello" del modulo. Se la chiamata valuta `false`, visualizziamo un saluto generico e facciamo il contrario. Infine, mettere il seguente codice alla fine:

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

8. Ultimo ma non meno importante, dobbiamo eseguire la funzione `nameDisplayCheck()` quando la pagina viene caricata. Se non facciamo questo, allora il saluto personalizzato non persisterà tra i ricaricamenti delle pagine. Aggiungere il seguente alla fine del codice:

   ```js
   nameDisplayCheck();
   ```

Il vostro esempio è finito — ben fatto! Tutto ciò che rimane ora è salvare il codice e testare la vostra pagina HTML in un browser. Potete vedere la nostra [versione finale funzionante dal vivo qui](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/web-storage/personal-greeting.html).

> [!NOTE]
> Esiste un altro esempio, leggermente più complesso da esplorare su [Usare l'API Web Storage](/it/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API).

> [!NOTE]
> Nella riga `<script src="index.js" defer></script>` del sorgente per la nostra versione finale, l'attributo `defer` specifica che il contenuto dell'{{htmlelement("script")}} non verrà eseguito fino a quando la pagina non avrà finito di caricarsi.

## Archiviazione di dati complessi — IndexedDB

L'[API IndexedDB](/it/docs/Web/API/IndexedDB_API) (a volte abbreviata in IDB) è un sistema di database completo disponibile nel browser in cui è possibile archiviare dati correlati complessi, i cui tipi non sono limitati a valori semplici come stringhe o numeri. È possibile memorizzare video, immagini e praticamente qualsiasi altra cosa in un'istanza IndexedDB.

L'API IndexedDB consente di creare un database, quindi creare archivi di oggetti all'interno di quel database.
Gli archivi di oggetti sono come tabelle in un database relazionale, e ciascun archivio può contenere un numero di oggetti.
Per saperne di più sull'API IndexedDB, vedere [Usare IndexedDB](/it/docs/Web/API/IndexedDB_API/Using_IndexedDB).

Tuttavia, ciò ha un costo: IndexedDB è molto più complessa da utilizzare rispetto alla Web Storage API. In questa sezione, graffieremo davvero solo la superficie di ciò di cui è capace, ma forniremo abbastanza per cominciare.

### Esempio di archiviazione di note

Qui vi guideremo attraverso un esempio che vi permette di memorizzare note nel vostro browser e di visualizzarle e cancellarle ogni volta che volete, facendovi costruire da soli e spiegandovi le parti più fondamentali di IDB mentre andiamo avanti.

L'app appare qualcosa del genere:

![Screenshot di demo con note di IndexDB con 4 sezioni. La prima sezione è l'intestazione. La seconda sezione elenca tutte le note che sono state create. Ha due note, ciascuna con un pulsante di eliminazione. Una terza sezione è un modulo con 2 campi di input per 'Titolo della nota' e 'Testo della nota' e un pulsante etichettato 'Crea nuova nota'. La sezione del piè di pagina in basso dice 'Copyright nessuno. Usa il codice come desideri'.](idb-demo.png)

Ogni nota ha un titolo e del testo del corpo, ciascuno modificabile individualmente. Il codice JavaScript che vedremo qui sotto ha commenti dettagliati per aiutarvi a capire cosa sta succedendo.

### Per cominciare

1. Prima di tutto, fate copie locali dei nostri file [`index.html`](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/index.html), [`style.css`](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/style.css), e [`index-start.js`](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/index-start.js) in una nuova directory sulla vostra macchina locale.
2. Date un'occhiata ai file. Vedrete che l'HTML definisce un sito web con un'intestazione e un piè di pagina, nonché un'area di contenuto principale che contiene un posto per visualizzare le note e un modulo per inserire nuove note nel database. Il CSS fornisce una formattazione per rendere più chiaro ciò che sta succedendo. Il file JavaScript contiene cinque costanti dichiarate contenenti riferimenti alla lista {{htmlelement("ul")}} in cui le note verranno visualizzate, agli elementi di input del titolo e del corpo {{htmlelement("input")}}, al modulo {{htmlelement("form")}} stesso, e al pulsante {{htmlelement("button")}}.
3. Rinominate il vostro file JavaScript in `index.js`. Ora siete pronti per iniziare ad aggiungere codice.

### Configurazione iniziale del database

Ora vediamo cosa dobbiamo fare in primo luogo per configurare effettivamente un database.

1. Sotto le dichiarazioni delle costanti, aggiungere le seguenti righe:

   ```js
   // Create an instance of a db object for us to store the open database in
   let db;
   ```

   Qui stiamo dichiarando una variabile chiamata `db` — questa verrà usata successivamente per memorizzare un oggetto che rappresenta il nostro database. La useremo in alcuni luoghi, quindi l'abbiamo dichiarata globalmente qui per facilitare le cose.

2. Successivamente, aggiungere quanto segue:

   ```js
   // Open our database; it is created if it doesn't already exist
   // (see the upgradeneeded handler below)
   const openRequest = window.indexedDB.open("notes_db", 1);
   ```

   Questa riga crea una richiesta per aprire la versione `1` di un database chiamato `notes_db`. Se questo non esiste già, verrà creato da te dal successivo codice. Vedrai questo pattern di richiesta usato molto spesso in IndexedDB. Le operazioni sui database richiedono tempo. Non vuoi bloccare il browser mentre aspetti i risultati, quindi le operazioni sui database sono {{Glossary("asynchronous", "asincrone")}}, il che significa che invece di accadere immediatamente, accadranno a un certo punto in futuro e riceverai una notifica quando saranno fatte.

   Per gestire questo in IndexedDB, crei un oggetto di richiesta (che può essere chiamato come preferisci — qui l'abbiamo chiamato `openRequest`, in modo da essere chiaro a cosa serve). Poi usi i gestori di eventi per eseguire del codice quando la richiesta è completata, fallisce, ecc., che vedrai in uso qui sotto.

   > [!NOTE]
   > Il numero di versione è importante. Se desideri aggiornare il tuo database (ad esempio, cambiando la struttura della tabella), devi eseguire nuovamente il tuo codice con un numero di versione aumentato, specifica schema diversa all'interno del gestore `upgradeneeded` (vedi sotto) ecc. Non copriremo l'aggiornamento dei database in questo tutorial.

3. Ora aggiungere i seguenti gestori di eventi appena sotto la tua precedente aggiunta:

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

   Il gestore di eventi [`error`](/it/docs/Web/API/IDBRequest/error_event) verrà eseguito se il sistema torna dicendo che la richiesta è fallita. Questo ti consente di rispondere a questo problema. Nel nostro esempio, stampiamo semplicemente un messaggio nella console JavaScript.

   Il gestore di eventi [`success`](/it/docs/Web/API/IDBRequest/success_event) verrà eseguito se la richiesta torna con successo, il che significa che il database è stato aperto con successo. In tal caso, un oggetto che rappresenta il database aperto diventa disponibile nella proprietà [`openRequest.result`](/it/docs/Web/API/IDBRequest/result), permettendoci di manipolare il database. Lo memorizziamo nella variabile `db` che abbiamo creato in precedenza per uso futuro. Eseguiamo anche una funzione chiamata `displayData()`, che visualizza i dati nel database all'interno dell'{{HTMLElement("ul")}}. Lo eseguiamo ora in modo che le note già presenti nel database siano visualizzate non appena la pagina viene caricata. Vedrai `displayData()` definito più avanti.

4. Infine per questa sezione, aggiungeremo probabilmente il più importante gestore di eventi per la configurazione del database: [`upgradeneeded`](/it/docs/Web/API/IDBOpenDBRequest/upgradeneeded_event). Questo gestore viene eseguito se il database non è già stato configurato, o se il database è stato aperto con un numero di versione maggiore rispetto al database memorizzato esistente (quando si effettua un aggiornamento). Aggiungi il seguente codice, sotto il tuo precedente gestore:

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

   Qui definiamo lo schema (struttura) del nostro database; cioè, l'insieme di colonne (o campi) che contiene. Qui prima otteniamo un riferimento al database esistente dalla proprietà `result` del target dell'evento (`e.target.result`), che è l'oggetto `request`. Questo è equivalente alla riga `db = openRequest.result;` all'interno del gestore di eventi `success`, ma dobbiamo farlo separatamente qui perché il gestore di eventi `upgradeneeded` (se necessario) verrà eseguito prima del gestore di eventi `success`, il che significa che il valore `db` non sarebbe disponibile se non lo facessimo.

   Poi usiamo [`IDBDatabase.createObjectStore()`](/it/docs/Web/API/IDBDatabase/createObjectStore) per creare un nuovo archivio di oggetti all'interno del nostro database aperto chiamato `notes_os`. Questo è equivalente a una singola tabella in un sistema di database convenzionale. Gli abbiamo dato il nome note e abbiamo anche specificato un campo chiave `autoIncrement` chiamato `id` — in ogni nuovo record questo riceverà automaticamente un valore incrementato — lo sviluppatore non ha bisogno di impostarlo esplicitamente. Essendo la chiave, il campo `id` verrà utilizzato per identificare univocamente i record, come quando si elimina o si visualizza un record.

   Creiamo anche altri due indici (campi) usando il metodo [`IDBObjectStore.createIndex()`](/it/docs/Web/API/IDBObjectStore/createIndex): `title` (che conterrà un titolo per ogni nota) e `body` (che conterrà il testo del corpo della nota).

Con questo schema del database configurato, quando iniziamo ad aggiungere record al database, ciascuno sarà rappresentato come un oggetto lungo queste linee:

```json
{
  "title": "Buy milk",
  "body": "Need both cows milk and soy.",
  "id": 8
}
```

### Aggiungere dati al database

Ora vediamo come possiamo aggiungere record al database. Questo sarà fatto utilizzando il modulo sulla nostra pagina.

Sotto il tuo precedente gestore di eventi, aggiungi la seguente riga, che imposta un gestore di eventi `submit` che esegue una funzione chiamata `addData()` quando il modulo viene inviato (quando viene premuto il pulsante {{htmlelement("button")}}, portando a un invio effettivo del modulo):

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

Questo è abbastanza complesso; scomponiamolo:

- Eseguiamo [`Event.preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento per impedire al modulo di inviare effettivamente nel modo convenzionale (questo causerebbe un aggiornamento della pagina e rovinerebbe l'esperienza).
- Creiamo un oggetto che rappresenta un record da inserire nel database, popolandolo con valori dai campi di input del modulo. Notare che non dobbiamo includere esplicitamente un valore `id` — come abbiamo spiegato prima, questo è auto-popolato.
- Apriamo una transazione `readwrite` contro l'archivio di oggetti `notes_os` usando il metodo [`IDBDatabase.transaction()`](/it/docs/Web/API/IDBDatabase/transaction). Questo oggetto transazione ci consente di accedere all'archivio di oggetti in modo da poter fare qualcosa, ad esempio, aggiungere un nuovo record.
- Accediamo all'archivio di oggetti usando il metodo [`IDBTransaction.objectStore()`](/it/docs/Web/API/IDBTransaction/objectStore), salvando il risultato nella variabile `objectStore`.
- Aggiungiamo il nuovo record al database usando [`IDBObjectStore.add()`](/it/docs/Web/API/IDBObjectStore/add). Questo crea un oggetto richiesta, nello stesso modo che abbiamo visto prima.
- Aggiungiamo una serie di gestori di eventi agli oggetti `request` e `transaction` per eseguire del codice in punti critici del ciclo di vita. Una volta che la richiesta è riuscita, svuotiamo i campi di input del modulo, in modo da essere pronti per inserire la nota successiva. Una volta che la transazione è stata completata, eseguiamo nuovamente la funzione `displayData()` per aggiornare la visualizzazione delle note sulla pagina.

### Visualizzazione dei dati

Abbiamo già fatto riferimento a `displayData()` due volte nel nostro codice, quindi probabilmente dovremmo definirlo. Aggiungi questo al tuo codice, sotto la precedente definizione di funzione:

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

Ancora, analizziamolo:

- In primo luogo, svuotiamo il contenuto dell'elemento {{htmlelement("ul")}}, prima di riempirlo con il contenuto aggiornato. Se non lo facessimo, finiremmo con una lista enorme di contenuti duplicati che vengono aggiunti ad ogni aggiornamento.
- Successivamente, otteniamo un riferimento all'archivio di oggetti `notes_os` usando [`IDBDatabase.transaction()`](/it/docs/Web/API/IDBDatabase/transaction) e [`IDBTransaction.objectStore()`](/it/docs/Web/API/IDBTransaction/objectStore), come abbiamo fatto in `addData()`, eccetto che qui li stiamo concatenando insieme in una sola riga.
- Il passo seguente usa il metodo [`IDBObjectStore.openCursor()`](/it/docs/Web/API/IDBObjectStore/openCursor) per aprire una richiesta per un cursore — questo è un costrutto che può essere usato per iterare sui record in un archivio di oggetti. Colleghiamo un gestore di eventi `success` alla fine di questa riga per rendere il codice più conciso — quando il cursore viene restituito con successo, il gestore viene eseguito.
- Otteniamo un riferimento diretto al cursore stesso (un oggetto [`IDBCursor`](/it/docs/Web/API/IDBCursor)) usando `const cursor = e.target.result`.
- Successivamente, controlliamo per vedere se il cursore contiene un record dal datastore (`if (cursor){ }`) — se così, creiamo un frammento DOM, lo popoliamo con i dati del record e lo inseriamo nella pagina (all'interno dell'elemento `<ul>`). Includiamo anche un pulsante di eliminazione che, quando cliccato, eliminerà quella nota eseguendo la funzione `deleteItem()`, che vedremo nella prossima sezione.
- Alla fine del blocco `if`, usiamo il metodo [`IDBCursor.continue()`](/it/docs/Web/API/IDBCursor/continue) per avanzare il cursore al successivo record nel datastore ed eseguire di nuovo il contenuto del blocco `if`. Se c'è un altro record su cui iterare, questo lo fa inserito nella pagina, e quindi `continue()` viene nuovamente eseguito, e così via.
- Quando non ci sono più record su cui iterare, `cursor` restituirà `undefined`, e quindi il blocco `else` verrà eseguito invece del blocco `if`. Questo blocco controlla se qualche nota è stata inserita nel `<ul>` — in caso contrario, inserisce un messaggio per dire che nessuna nota è stata memorizzata.

### Eliminare una nota

Come indicato sopra, quando viene premuto il pulsante di eliminazione di una nota, la nota viene eliminata. Questo è ottenuto dalla funzione `deleteItem()`, che appare così:

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

- La prima parte di questo potrebbe necessitare di alcune spiegazioni — recuperiamo l'ID del record da eliminare usando `Number(e.target.parentNode.getAttribute('data-note-id'))` — ricorda che l'ID del record è stato salvato in un attributo `data-note-id` sull'`<li>` quando è stato inizialmente visualizzato. Tuttavia, dobbiamo passare l'attributo attraverso l'oggetto globale incorporato [`Number()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number) poiché è di tipo string, e quindi non sarebbe riconosciuto dal database, che si aspetta un numero.
- Poi otteniamo un riferimento all'archivio di oggetti usando lo stesso pattern che abbiamo visto in precedenza, e usiamo il metodo [`IDBObjectStore.delete()`](/it/docs/Web/API/IDBObjectStore/delete) per eliminare il record dal database, passando l'id.
- Quando la transazione del database è completa, eliminiamo l'`<li>` della nota dal DOM, e di nuovo facciamo il check per vedere se il `<ul>` ora è vuoto, inserendo una nota appropriata.

E questo è tutto! Il vostro esempio dovrebbe ora funzionare.

Se avete problemi con esso, sentitevi liberi di [confrontarlo con il nostro esempio dal vivo](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/indexeddb/notes/) (vedere anche [il codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/indexeddb/notes/index.js)).

### Archiviazione di dati complessi tramite IndexedDB

Come abbiamo detto sopra, IndexedDB può essere usato per memorizzare più delle semplici stringhe di testo. Puoi memorizzare praticamente qualsiasi cosa tu voglia, compresi oggetti complessi come blob di video o immagini. E non è molto più difficile da realizzare rispetto a qualsiasi altro tipo di dati.

Per dimostrare come fare, abbiamo scritto un altro esempio chiamato [IndexedDB video store](https://github.com/mdn/learning-area/tree/main/javascript/apis/client-side-storage/indexeddb/video-store) (vedi anche [la versione dal vivo qui](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/indexeddb/video-store/)). Quando esegui per la prima volta l'esempio, scarica tutti i video dalla rete, li memorizza in un database IndexedDB e quindi visualizza i video nell'interfaccia utente all'interno di elementi {{htmlelement("video")}}. La seconda volta che lo esegui, trova i video nel database e li prende da lì prima di visualizzarli — questo rende i successivi caricamenti molto più veloci e meno dispendiosi in termini di banda larga.

Esaminiamo le parti più interessanti dell'esempio. Non lo guarderemo tutto — molto è simile all'esempio precedente, e il codice è ben commentato.

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

2. Per cominciare, una volta che il database è stato aperto con successo eseguiamo una funzione `init()`. Questo scorre tra i vari nomi dei video, cercando di caricare un record identificato da ciascun nome dal database `videos`.

   Se ogni video viene trovato nel database (controllato vedendo se `request.result` valuta `true` — se il record non è presente, sarà `undefined`), i suoi file video (memorizzati come blob) e il nome del video vengono passati direttamente alla funzione `displayVideo()` per collocarli nell'interfaccia utente. Se non è trovato, il nome del video viene passato alla funzione `fetchVideoFromNetwork()` per, indovina, recuperare il video dalla rete.

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

3. Il seguente frammento è preso da dentro `fetchVideoFromNetwork()` — qui recuperiamo le versioni MP4 e WebM del video usando due richieste separate [`fetch()`](/it/docs/Web/API/Window/fetch). Poi usiamo il metodo [`Response.blob()`](/it/docs/Web/API/Response/blob) per estrarre il corpo di ogni risposta come blob, dandoci una rappresentazione oggettiva dei video che può essere memorizzata e visualizzata più avanti.

   Abbiamo però un problema qui — queste due richieste sono entrambi asincrone, ma vogliamo solo cercare di visualizzare o memorizzare il video quando entrambe le promesse sono state adempiute. Fortunatamente c'è un metodo interno che gestisce tale problema — {{jsxref("Promise.all()")}}. Questo prende un argomento — riferisce a tutte le promesse individuali che vuoi controllare per l'adempimento inserite in un array — e restituisce una promessa che viene adempiuta quando tutte le promesse individuali sono adempiute.

   All'interno dell'handler `then()` per questa promessa, chiamiamo la funzione `displayVideo()` come abbiamo fatto prima per mostrare i video nell'interfaccia utente, poi chiamiamo anche la funzione `storeVideo()` per memorizzare quei video all'interno del database.

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

4. Guardiamo per primo `storeVideo()`. Questo è molto simile al pattern visto nell'esempio precedente per aggiungere dati al database — apriamo una transazione `readwrite` e otteniamo un riferimento al nostro archivio di oggetti `videos_os`, creiamo un oggetto che rappresenta il record da aggiungere al database, poi lo aggiungiamo usando [`IDBObjectStore.add()`](/it/docs/Web/API/IDBObjectStore/add).

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

5. Infine, abbiamo `displayVideo()`, che crea gli elementi DOM necessari per inserire i video nell'interattività utente e quindi li appende alla pagina. Le parti più interessanti di questo sono quelle mostrate sotto — per visualizzare effettivamente i nostri blob di video in un elemento `<video>`, dobbiamo creare URL di oggetti (URL interni che indicano i blob di video memorizzati in memoria) tramite il metodo [`URL.createObjectURL()`](/it/docs/Web/API/URL/createObjectURL_static). Una volta fatto ciò, possiamo impostare gli URL degli oggetti per essere i valori degli attributi `src` del nostro elemento {{htmlelement("source")}}, e funziona bene.

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

## Archiviazione delle risorse offline

L'esempio precedente mostra già come creare un'app che memorizzerà grandi risorse in un database IndexedDB, evitando la necessità di scaricarle più di una volta. Questo è già un grande miglioramento per l'esperienza utente, ma manca ancora qualcosa — i principali file HTML, CSS e JavaScript devono ancora essere scaricati ogni volta che il sito viene accesso, il che significa che non funzionerà quando non c'è una connessione di rete.

![Schermata offline di Firefox con un'illustrazione di un personaggio dei cartoni animati sul lato sinistro che tiene in mano una spina a due poli nella mano destra e una presa a due poli nella mano sinistra. Sul lato destro c'è un messaggio di Modalità Offline e un pulsante etichettato 'Riprova'.](ff-offline.png)

Questo è dove entrano in gioco i [Service workers](/it/docs/Web/API/Service_Worker_API) e l'API [Cache](/it/docs/Web/API/Cache) strettamente correlata.

Un service worker è un file JavaScript che viene registrato contro un particolare origin (sito web, o parte di un sito web a un certo dominio) quando viene accesso da un browser. Quando registrato, può controllare le pagine disponibili a quell'origine. Fa questo posizionandosi tra una pagina caricata e la rete e intercettando le richieste di rete mirate a quell'origine.

Quando intercetta una richiesta, può fare qualsiasi cosa tu desideri ad essa (vedi [idee per casi d'uso](/it/docs/Web/API/Service_Worker_API#other_use_case_ideas)), ma l'esempio classico è quello di salvare le risposte di rete offline e quindi fornire quelle in risposta a una richiesta invece delle risposte dalla rete. In effetti, ti permette di far funzionare un sito completamente offline.

L'API Cache è un altro meccanismo di archiviazione lato client, con una leggera differenza — è progettata per salvare risposte HTTP, e quindi funziona molto bene con i service workers.

### Un esempio di service worker

Guardiamo un esempio, per darti un'idea di come potrebbe sembrare. Abbiamo creato un'altra versione dell'esempio del negozio video che abbiamo visto nella sezione precedente — questo funziona in modo identico, tranne per il fatto che salva anche l'HTML, il CSS e il JavaScript nell'API Cache tramite un service worker, permettendo all'esempio di funzionare offline!

Vedi [IndexedDB video store con service worker in esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/), e vedi anche [il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/client-side-storage/cache-sw/video-store-offline).

#### Registrare il service worker

La prima cosa da notare è che c'è un extra frammento di codice inserito nel file JavaScript principale (vedi [index.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/cache-sw/video-store-offline/index.js)). Inizialmente, facciamo un test di rilevamento delle funzionalità per vedere se il membro `serviceWorker` è disponibile nell'oggetto [`Navigator`](/it/docs/Web/API/Navigator). Se questo restituisce true, allora sappiamo che almeno le basi dei service workers sono supportate. Qui dentro usiamo il metodo [`ServiceWorkerContainer.register()`](/it/docs/Web/API/ServiceWorkerContainer/register) per registrare un service worker contenuto nel file `sw.js` contro l'origine in cui risiede, in modo che possa controllare pagine nella stessa directory in cui è, o sottodirectory. Quando la sua promessa si avvera, il service worker è considerato registrato.

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
> Il percorso dato al file `sw.js` è relativo all'origine del sito, non al file JavaScript che contiene il codice. Il service worker è a `https://mdn.github.io/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js`. L'origine è `https://mdn.github.io`, e quindi il percorso dato deve essere `/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js`. Se volessi ospitare questo esempio sul tuo server, dovresti cambiarlo di conseguenza. Questo è piuttosto confuso, ma deve funzionare in questo modo per motivi di sicurezza.

#### Installare il service worker

La prossima volta che qualsiasi pagina sotto il controllo del service worker è accessa (ad esempio, quando l'esempio è ricaricato), il service worker è installato contro quella pagina, il che significa che inizierà a controllarla. Quando questo accade, un evento `install` viene lanciato contro il service worker; puoi scrivere del codice all'interno del file del service worker stesso che risponderà all'installazione.

Vediamo un esempio, nel file [sw.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/client-side-storage/cache-sw/video-store-offline/sw.js) (the service worker). Vedrai che il listener di installazione è registrato contro `self`. Questa parola chiave `self` è un modo di riferirsi all'ambito globale del service worker da dentro il file del service worker.

All'interno del gestore `install`, usiamo il metodo [`ExtendableEvent.waitUntil()`](/it/docs/Web/API/ExtendableEvent/waitUntil), disponibile sull'oggetto evento, per indicare che il browser non dovrebbe completare l'installazione del service worker finché la promessa al suo interno non si è avverata con successo.

Qui vediamo l'API Cache in azione. Usiamo il metodo [`CacheStorage.open()`](/it/docs/Web/API/CacheStorage/open) per aprire un nuovo oggetto cache in cui le risposte possono essere memorizzate (simile a un archivio di oggetti IndexedDB). Questa promessa si avvera con un oggetto [`Cache`](/it/docs/Web/API/Cache) che rappresenta la cache `video-store`. Poi usiamo il metodo [`Cache.addAll()`](/it/docs/Web/API/Cache/addAll) per recuperare una serie di risorse e aggiungere le loro risposte alla cache.

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

Questo è tutto per ora, installazione effettuata.

#### Rispondere a ulteriori richieste

Con il service worker registrato e installato contro la nostra pagina HTML, e le relative risorse tutte aggiunte alla nostra cache, siamo quasi pronti per andare. C'è solo un'altra cosa da fare: scrivere del codice per rispondere a richieste di rete ulteriori.

Questo è quello che il secondo pezzo di codice in `sw.js` fa. Aggiungiamo un altro listener all'ambito globale del service worker, che esegue la funzione handler quando l'evento `fetch` viene scatenato. Questo accade quando il browser fa una richiesta per una risorsa nella directory per cui il service worker è registrato.

All'interno del gestore, prima registriamo l'URL della risorsa richiesta. Poi forniamo una risposta personalizzata alla richiesta, usando il metodo [`FetchEvent.respondWith()`](/it/docs/Web/API/FetchEvent/respondWith).

All'interno di questo blocco, usiamo [`CacheStorage.match()`](/it/docs/Web/API/CacheStorage/match) per verificare se una richiesta corrispondente (cioè corrisponde all'URL) può essere trovata in qualsiasi cache. Questa promessa si avvera con la risposta corrispondente se si trova una corrispondenza, o `undefined` se non lo è.

Se si trova una corrispondenza, la restituiamo come risposta personalizzata. In caso contrario, recuperiamo la risposta dalla rete e la restituiamo invece.

```js
self.addEventListener("fetch", (e) => {
  console.log(e.request.url);
  e.respondWith(
    caches.match(e.request).then((response) => response || fetch(e.request)),
  );
});
```

E questo è tutto per il nostro service worker.
C'è un'intera serie di altre cose che si possono fare con loro — per molti più dettagli, vedi il [service worker cookbook](https://github.com/mdn/serviceworker-cookbook).
Molte grazie a Paul Kinlan per il suo articolo [Adding a Service Worker and Offline into your Web App](https://developers.google.com/codelabs/pwa-training/pwa03--going-offline#0), che ha ispirato questo esempio.

#### Testare l'esempio offline

Per testare il nostro [esempio service worker](https://mdn.github.io/learning-area/javascript/apis/client-side-storage/cache-sw/video-store-offline/), dovrai caricarlo un paio di volte per assicurarti che sia installato. Una volta fatto, puoi:

- Provare a scollegare la connessione di rete/disattivare il Wi-Fi.
- Selezionare _File > Lavora Offline_ se stai usando Firefox.
- Andare sugli strumenti per sviluppatori, quindi scegliere _Application > Service Workers_, quindi selezionare la casella _Offline_ se stai usando Chrome.

Se ricarichi di nuovo la tua pagina dell'esempio, dovresti ancora vederla caricare perfettamente. Tutto è memorizzato offline — le risorse della pagina in una cache e i video in un database IndexedDB.

## Sommario

Questo è tutto per ora. Speriamo che tu abbia trovato utile il nostro riepilogo delle tecnologie di archiviazione lato client.

## Vedi anche

- [Web storage API](/it/docs/Web/API/Web_Storage_API)
- [IndexedDB API](/it/docs/Web/API/IndexedDB_API)
- [Cookies](/it/docs/Web/HTTP/Guides/Cookies)
- [Service worker API](/it/docs/Web/API/Service_Worker_API)

{{PreviousMenuNext("Learn_web_development/Extensions/Client-side_APIs/Drawing_graphics", "Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs", "Learn_web_development/Extensions/Client-side_APIs")}}
