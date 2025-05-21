---
title: Effettuare richieste di rete con JavaScript
short-title: Richieste di rete
slug: Learn_web_development/Core/Scripting/Network_requests
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/DOM_scripting","Learn_web_development/Core/Scripting/JSON", "Learn_web_development/Core/Scripting")}}

Un altro compito molto comune nei moderni siti web e applicazioni è effettuare richieste di rete per ottenere dati individuali dal server al fine di aggiornare sezioni di una pagina web senza dover ricaricare una pagina intera nuova. Questo apparente piccolo dettaglio ha avuto un enorme impatto sulle prestazioni e sul comportamento dei siti, quindi in questo articolo spiegheremo il concetto e guarderemo alle tecnologie che lo rendono possibile: in particolare, la [Fetch API](/it/docs/Web/API/Fetch_API).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione dell'<a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Richieste di rete asincrone, che è di gran lunga il caso d'uso asincrono di JavaScript più comune sul web.</li>
          <li>Tipi comuni di risorse che vengono prelevate dalla rete: JSON, risorse multimediali, dati da API RESTful.</li>
          <li>Come usare <code>fetch()</code> per implementare richieste di rete asincrone.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Qual è il problema qui?

Una pagina web è costituita da una pagina HTML e (solitamente) altri vari file, come fogli di stile, script e immagini. Il modello base di caricamento della pagina sul Web è che il browser effettua una o più richieste HTTP al server per i file necessari a visualizzare la pagina e il server risponde con i file richiesti. Se si visita un'altra pagina, il browser richiede i nuovi file e il server risponde con essi.

![Caricamento della pagina tradizionale](traditional-loading.svg)

Questo modello funziona perfettamente per molti siti. Ma considera un sito web che sia molto orientato ai dati. Ad esempio, un sito di biblioteca come la [Biblioteca Pubblica di Vancouver](https://www.vpl.ca/). Tra le altre cose, potresti considerare un sito come questo una interfaccia utente per un database. Potrebbe permetterti di cercare un genere particolare di libro, o potrebbe mostrarti raccomandazioni per libri che potrebbero piacerti, basandosi sui libri che hai già preso in prestito. Quando fai questo, il sito deve aggiornare la pagina con il nuovo set di libri da visualizzare. Ma nota che la maggior parte del contenuto della pagina — compresi elementi come l'intestazione della pagina, la barra laterale e il piè di pagina — resta lo stesso.

Il problema con il modello tradizionale è che dovremmo recuperare e caricare l'intera pagina, anche quando abbiamo bisogno di aggiornare una sola parte di essa. Questo è inefficiente e può portare a una pessima esperienza utente.

Quindi, invece del modello tradizionale, molti siti web usano le API JavaScript per richiedere dati dal server e aggiornare i contenuti della pagina senza un caricamento della pagina. Quindi, quando l'utente cerca un nuovo prodotto, il browser richiede solamente i dati necessari per aggiornare la pagina — il set di nuovi libri da mostrare, per esempio.

![Usare fetch per aggiornare le pagine](fetch-update.svg)

L'API principale qui è la [Fetch API](/it/docs/Web/API/Fetch_API). Questa consente a JavaScript che gira in una pagina di effettuare una richiesta [HTTP](/it/docs/Web/HTTP) a un server per ottenere risorse specifiche. Quando il server le fornisce, il JavaScript può usare i dati per aggiornare la pagina, tipicamente utilizzando le [API di manipolazione del DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting). I dati richiesti sono spesso [JSON](/it/docs/Learn_web_development/Core/Scripting/JSON), che è un buon formato per trasferire dati strutturati, ma possono essere anche HTML o solo testo.

Questo è uno schema comune per i siti orientati ai dati come Amazon, YouTube, eBay, e così via. Con questo modello:

- Gli aggiornamenti della pagina sono molto più rapidi e non è necessario attendere il refresh della pagina, il che significa che il sito appare più veloce e reattivo.
- Viene scaricata meno quantità di dati ad ogni aggiornamento, il che significa meno larghezza di banda sprecata. Questo può non essere un grosso problema su un desktop con connessione a banda larga, ma è un problema importante su dispositivi mobili e in paesi che non hanno un servizio di internet veloce ubiquo.

> [!NOTE]
> Nei primi giorni, questa tecnica generale era conosciuta come {{Glossary("Asynchronous", "JavaScript asincrono e XML")}} ({{Glossary("AJAX", "AJAX")}}), perché solitamente richiedeva dati XML. Normalmente non è così al giorno d'oggi (saresti più incline a richiedere JSON), ma il risultato è ancora lo stesso, e il termine "AJAX" è ancora spesso usato per descrivere la tecnica.

Per velocizzare ulteriormente le cose, alcuni siti memorizzano anche risorse e dati sul computer dell'utente quando vengono richiesti per la prima volta, significando che nelle visite successive usano le versioni locali invece di scaricare copie nuove ogni volta che la pagina viene primo caricata. Il contenuto viene ricaricato solo dal server quando è stato aggiornato.

## La Fetch API

Camminiamo attraverso un paio di esempi della Fetch API.

### Recuperare contenuto testuale

Per questo esempio, richiederemo dati da alcuni diversi file di testo e li useremo per popolare un'area di contenuto.

Questa serie di file fungerà da nostro falso database; in una vera applicazione, saremmo più inclini a usare un linguaggio lato server come PHP, Python o Node per richiedere i nostri dati da un database. Qui, tuttavia, vogliamo tenerlo semplice e concentrarci sulla parte lato client di questo.

Per iniziare questo esempio, crea una copia locale di [fetch-start.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/fetch-start.html) e dei quattro file di testo — [verse1.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse1.txt), [verse2.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse2.txt), [verse3.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse3.txt), e [verse4.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse4.txt) — in una nuova directory sul tuo computer. In questo esempio, preleveremo un diverso verso della poesia (che potresti ben riconoscere) quando viene selezionato nel menu a discesa.

Proprio all'interno dell'elemento {{htmlelement("script")}}, aggiungi il seguente codice. Questo memorizza riferimenti agli elementi {{htmlelement("select")}} e {{htmlelement("pre")}} e aggiunge un ascoltatore all'elemento `<select>`, in modo che quando l'utente seleziona un nuovo valore, il nuovo valore venga passato alla funzione chiamata `updateDisplay()` come parametro.

```js
const verseChoose = document.querySelector("select");
const poemDisplay = document.querySelector("pre");

verseChoose.addEventListener("change", () => {
  const verse = verseChoose.value;
  updateDisplay(verse);
});
```

Definiamo la nostra funzione `updateDisplay()`. Prima di tutto, inserisci il seguente blocco vuoto di codice sotto il blocco precedente — questo è il guscio vuoto della funzione.

```js-nolint
function updateDisplay(verse) {

}
```

Inizieremo la nostra funzione costruendo un URL relativo che punta al file di testo che vogliamo caricare, in quanto ne avremo bisogno successivamente. Il valore dell'elemento {{htmlelement("select")}} in qualsiasi momento è lo stesso del testo all'interno dell'{{htmlelement("option")}} selezionata (a meno che tu non specifichi un valore diverso in un attributo value) — quindi per esempio "Verse 1". Il corrispondente file di testo del verso è "verse1.txt", ed è nella stessa directory del file HTML, quindi basta solo il nome del file.

Tuttavia, i server Web tendono a essere case-sensitive, e il nome del file non ha spazio tra le parole. Per convertire "Verse 1" in "verse1.txt" occorre convertire la 'V' in minuscolo, rimuovere lo spazio, e aggiungere ".txt" alla fine. Questo può essere fatto con {{jsxref("String.replace", "replace()")}}, {{jsxref("String.toLowerCase", "toLowerCase()")}}, e il [template literal](/it/docs/Web/JavaScript/Reference/Template_literals). Aggiungi le seguenti linee all'interno della tua funzione `updateDisplay()`:

```js
verse = verse.replace(" ", "").toLowerCase();
const url = `${verse}.txt`;
```

Finalmente siamo pronti per usare la Fetch API:

```js
// Call `fetch()`, passing in the URL.
fetch(url)
  // fetch() returns a promise. When we have received a response from the server,
  // the promise's `then()` handler is called with the response.
  .then((response) => {
    // Our handler throws an error if the request did not succeed.
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    // Otherwise (if the response succeeded), our handler fetches the response
    // as text by calling response.text(), and immediately returns the promise
    // returned by `response.text()`.
    return response.text();
  })
  // When response.text() has succeeded, the `then()` handler is called with
  // the text, and we copy it into the `poemDisplay` box.
  .then((text) => {
    poemDisplay.textContent = text;
  })
  // Catch any errors that might happen, and display a message
  // in the `poemDisplay` box.
  .catch((error) => {
    poemDisplay.textContent = `Could not fetch verse: ${error}`;
  });
```

C'è parecchio da esplorare qui.

Innanzitutto, il punto di ingresso per la Fetch API è una funzione globale chiamata [`fetch()`](/it/docs/Web/API/Window/fetch), che prende l'URL come parametro (prende un altro parametro opzionale per impostazioni personalizzate, ma non lo usiamo qui).

Successivamente, `fetch()` è un'API asincrona che restituisce un {{jsxref("Promise")}}. Se non sai cos'è, leggi il modulo su [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS), e in particolare la lezione sulle [promesse](/it/docs/Learn_web_development/Extensions/Async_JS/Promises), quindi torna qui. Troverai che quell'articolo parla anche dell'API `fetch()`!

Poiché `fetch()` restituisce una promessa, passiamo una funzione nel metodo {{jsxref("Promise/then", "then()")}} della promessa restituita. Questo metodo verrà chiamato quando la richiesta HTTP ha ricevuto una risposta dal server. Nel gestore, controlliamo che la richiesta sia riuscita, e lanciamo un errore se non lo è stata. Altrimenti, chiamiamo [`response.text()`](/it/docs/Web/API/Response/text), per ottenere il corpo della risposta come testo.

Si scopre che `response.text()` è _anche_ asincrona, quindi restituiamo la promessa che restituisce, e passiamo una funzione nel metodo `then()` di questa nuova promessa. Questa funzione verrà chiamata quando il testo della risposta è pronto, e al suo interno aggiorneremo il nostro blocco `<pre>` con il testo.

Infine, concatenamo un gestore {{jsxref("Promise/catch", "catch()")}} alla fine, per catturare eventuali errori lanciati in una delle funzioni asincrone che abbiamo chiamato o nei loro gestori.

Un problema con l'esempio così com'è è che non mostrerà nessuna parte della poesia quando viene caricato per la prima volta. Per risolvere questo, aggiungi le seguenti due righe alla fine del tuo codice (appena sopra l'etichetta di chiusura `</script>`) per caricare il verso 1 di default, e assicurarti che l'elemento {{htmlelement("select")}} mostri sempre il valore corretto:

```js
updateDisplay("Verse 1");
verseChoose.value = "Verse 1";
```

#### Servire il tuo esempio da un server

I browser moderni non eseguiranno richieste HTTP se esegui l'esempio da un file locale. Questo è dovuto a restrizioni di sicurezza (per maggiori informazioni sulla sicurezza web, leggi [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security)).

Per aggirare questo, dobbiamo testare l'esempio eseguendolo attraverso un server web locale. Per sapere come fare, leggi [Come si configura un server di test locale?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

### The Can Store

In questo esempio abbiamo creato un sito di esempio chiamato The Can Store — è un supermercato immaginario che vende solo prodotti in scatola. Puoi trovare questo [esempio dal vivo su GitHub](https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/), e [vedere il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/fetching-data/can-store).

![Un falso sito di e-commerce che mostra le opzioni di ricerca nella colonna di sinistra e i risultati di ricerca dei prodotti nella colonna di destra.](can-store.png)

Per default, il sito mostra tutti i prodotti, ma puoi usare i controlli del modulo nella colonna di sinistra per filtrarli per categoria, o termine di ricerca, o entrambi.

C'è parecchio codice complesso che si occupa di filtrare i prodotti per categoria e termini di ricerca, manipolando le stringhe in modo che i dati vengano visualizzati correttamente nell'interfaccia utente, ecc. Non discuteremo tutto nel dettaglio nell'articolo, ma puoi trovare commenti estesi nel codice (vedi [can-script.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/can-store/can-script.js)).

Spiegheremo però il codice Fetch.

Il primo blocco che utilizza Fetch può essere trovato all'inizio del JavaScript:

```js
fetch("products.json")
  .then((response) => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then((json) => initialize(json))
  .catch((err) => console.error(`Fetch problem: ${err.message}`));
```

La funzione `fetch()` restituisce una promessa. Se questa si completa con successo, la funzione all'interno del primo blocco `.then()` contiene la `response` restituita dalla rete.

All'interno di questa funzione noi:

- controlliamo che il server non abbia restituito un errore (come [`404 Not Found`](/it/docs/Web/HTTP/Reference/Status/404)). Se lo ha fatto, lanciamo un errore.
- chiamiamo [`json()`](/it/docs/Web/API/Response/json) sulla risposta. Questo recupererà i dati come un [oggetto JSON](/it/docs/Learn_web_development/Core/Scripting/JSON). Restituiamo la promessa restituita da `response.json()`.

Successivamente passiamo una funzione nel metodo `then()` di quella promessa restituita. Questa funzione riceverà un oggetto che contiene i dati della risposta come JSON, che passiamo alla funzione `initialize()`. È `initialize()` che inizia il processo di visualizzazione di tutti i prodotti nell'interfaccia utente.

Per gestire gli errori, colleghiamo un blocco `.catch()` alla fine della catena. Questo esegue il tutto se la promessa fallisce per qualche motivo. Al suo interno, includiamo una funzione che viene passata come parametro, un oggetto `err`. Questo oggetto `err` può essere usato per riportare la natura dell'errore che si è verificato, in questo caso lo facciamo con un semplice `console.error()`.

Tuttavia, un sito web completo gestirebbe questo errore in modo più elegante mostrando un messaggio sullo schermo dell'utente e forse offrendo opzioni per porre rimedio alla situazione, ma non abbiamo bisogno di nulla di più di un semplice `console.error()`.

Puoi testare il caso di fallimento da solo:

<!-- cSpell:ignore produc -->

1. Fai una copia locale dei file di esempio.
2. Esegui il codice attraverso un server web (come descritto sopra, in [Servire il tuo esempio da un server](#servire_il_tuo_esempio_da_un_server)).
3. Modifica il percorso del file da recuperare, in qualcosa come 'produc.json' (assicurati che sia scritto male).
4. Ora carica il file index nel tuo browser (attraverso `localhost:8000`) e guarda la console per sviluppatori del tuo browser. Vedrai un messaggio simile a "Fetch problem: HTTP error: 404".

Il secondo blocco Fetch può essere trovato all'interno della funzione `fetchBlob()`:

```js
fetch(url)
  .then((response) => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.blob();
  })
  .then((blob) => showProduct(blob, product))
  .catch((err) => console.error(`Fetch problem: ${err.message}`));
```

Questo funziona in modo molto simile al precedente, eccetto che invece di usare [`json()`](/it/docs/Web/API/Response/json), usiamo [`blob()`](/it/docs/Web/API/Response/blob). In questo caso vogliamo restituire la nostra risposta come un file immagine, e il formato di dati che usiamo per quello è [Blob](/it/docs/Web/API/Blob) (il termine è un'abbreviazione di "Binary Large Object" e può essere utilizzato per rappresentare oggetti di file di grandi dimensioni, come immagini o file video).

Una volta che abbiamo ricevuto correttamente il nostro blob, lo passiamo alla nostra funzione `showProduct()`, che lo visualizza.

## L'API XMLHttpRequest

A volte, specialmente nel codice più vecchio, vedrai usata un'altra API chiamata [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest) (spesso abbreviata come "XHR") per effettuare richieste HTTP. Questa precede Fetch, ed è stata realmente la prima API ampiamente usata per implementare AJAX. Ti consigliamo di usare Fetch se puoi: è un'API più semplice e ha più funzioni di `XMLHttpRequest`. Non passeremo attraverso un esempio che usa `XMLHttpRequest`, ma ti mostreremo come sarebbe la versione `XMLHttpRequest` della nostra prima richiesta del Can Store:

```js
const request = new XMLHttpRequest();

try {
  request.open("GET", "products.json");

  request.responseType = "json";

  request.addEventListener("load", () => initialize(request.response));
  request.addEventListener("error", () => console.error("XHR error"));

  request.send();
} catch (error) {
  console.error(`XHR error ${request.status}`);
}
```

Ci sono cinque fasi a questo:

1. Crea un nuovo oggetto `XMLHttpRequest`.
2. Chiama il suo metodo [`open()`](/it/docs/Web/API/XMLHttpRequest/open) per inizializzarlo.
3. Aggiungi un ascoltatore di eventi al suo evento [`load`](/it/docs/Web/API/XMLHttpRequest/load_event), che viene attivato quando la risposta è stata completata con successo. Nell'ascoltatore chiamiamo `initialize()` con i dati.
4. Aggiungi un ascoltatore di eventi al suo evento [`error`](/it/docs/Web/API/XMLHttpRequest/error_event), che viene attivato quando la richiesta incontra un errore.
5. Invia la richiesta.

Abbiamo anche bisogno di incapsulare l'intera cosa nel blocco [try...catch](/it/docs/Web/JavaScript/Reference/Statements/try...catch), per gestire qualsiasi errore lanciato da `open()` o `send()`.

Si spera che tu pensi che la Fetch API sia un miglioramento rispetto a questo. In particolare, guarda come dobbiamo gestire gli errori in due luoghi diversi.

## Riassunto

Questo articolo mostra come iniziare a lavorare con Fetch per recuperare dati dal server.

## Vedi anche

Tuttavia ci sono molti argomenti diversi discussi in questo articolo, che ha solo veramente grattato la superficie. Per molti più dettagli su questi argomenti, prova i seguenti articoli:

- [Usare Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch)
- [Promesse](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Lavorare con dati JSON](/it/docs/Learn_web_development/Core/Scripting/JSON)
- [Una panoramica di HTTP](/it/docs/Web/HTTP/Guides/Overview)
- [Programmazione lato server per siti web](/it/docs/Learn_web_development/Extensions/Server-side)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/DOM_scripting","Learn_web_development/Core/Scripting/JSON", "Learn_web_development/Core/Scripting")}}
