---
title: Effettuare richieste di rete con JavaScript
short-title: Richieste di rete
slug: Learn_web_development/Core/Scripting/Network_requests
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/DOM_scripting","Learn_web_development/Core/Scripting/JSON", "Learn_web_development/Core/Scripting")}}

Un altro compito molto comune nei siti e nelle applicazioni moderne è effettuare richieste di rete per recuperare dati individuali dal server e aggiornare sezioni di una pagina web senza dover caricare un'intera nuova pagina. Questo dettaglio apparentemente piccolo ha avuto un enorme impatto sulle prestazioni e sul comportamento dei siti, quindi in questo articolo spiegheremo il concetto e analizzeremo le tecnologie che lo rendono possibile: in particolare, la [Fetch API](/it/docs/Web/API/Fetch_API).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Richieste di rete asincrone, che è di gran lunga il caso d'uso asincrono di JavaScript più comune sul web.</li>
          <li>Tipi comuni di risorse recuperate dalla rete: JSON, risorse multimediali, dati da API RESTful.</li>
          <li>Come usare <code>fetch()</code> per implementare richieste di rete asincrone.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Qual è il problema qui?

Una pagina web è costituita da una pagina HTML e (di solito) da vari altri file, come fogli di stile, script e immagini. Il modello di base del caricamento delle pagine sul web è che il suo browser effettua una o più richieste HTTP al server per i file necessari alla visualizzazione della pagina, e il server risponde con i file richiesti. Se si visita un'altra pagina, il browser richiede i nuovi file e il server risponde con essi.

![Caricamento tradizionale delle pagine](traditional-loading.svg)

Questo modello funziona perfettamente per molti siti. Ma consideriamo un sito web molto orientato ai dati. Ad esempio, un sito web di una biblioteca come la [Vancouver Public Library](https://www.vpl.ca/). Tra le altre cose, si può pensare a un sito come questo come a un'interfaccia utente per un database. Potrebbe permettere di cercare un particolare genere di libro o potrebbe mostrare raccomandazioni per libri che si potrebbero apprezzare, basate su libri precedentemente presi in prestito. Quando si fa ciò, è necessario aggiornare la pagina con il nuovo set di libri da visualizzare. Ma si noti che la maggior parte del contenuto della pagina – inclusi elementi come l'intestazione della pagina, la barra laterale e il piè di pagina – rimane invariata.

Il problema con il modello tradizionale è che dovremmo recuperare e caricare l'intera pagina, anche quando abbiamo solo bisogno di aggiornare una parte di essa. Ciò è inefficiente e può comportare un'esperienza utente scadente.

Quindi, invece del modello tradizionale, molti siti utilizzano le API JavaScript per richiedere dati dal server e aggiornare il contenuto della pagina senza un caricamento della pagina. Quindi, quando l'utente cerca un nuovo prodotto, il browser richiede solo i dati necessari per aggiornare la pagina – il set di nuovi libri da visualizzare, per esempio.

![Utilizzo di fetch per aggiornare le pagine](fetch-update.svg)

L'API principale qui è la [Fetch API](/it/docs/Web/API/Fetch_API). Ciò consente a JavaScript che gira in una pagina di effettuare una richiesta [HTTP](/it/docs/Web/HTTP) a un server per recuperare risorse specifiche. Quando il server le fornisce, il JavaScript può usare i dati per aggiornare la pagina, tipicamente utilizzando le [API di manipolazione del DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting). I dati richiesti sono spesso [JSON](/it/docs/Learn_web_development/Core/Scripting/JSON), che è un buon formato per trasferire dati strutturati, ma possono anche essere HTML o solo testo.

Questo è un modello comune per siti orientati ai dati come Amazon, YouTube, eBay, e così via. Con questo modello:

- Gli aggiornamenti della pagina sono molto più rapidi e non è necessario attendere il refresh della pagina, il che significa che il sito sembra più veloce e reattivo.
- Meno dati vengono scaricati a ogni aggiornamento, il che significa meno larghezza di banda sprecata. Ciò potrebbe non essere un grande problema su un desktop con connessione a banda larga, ma è un problema importante sui dispositivi mobili e nei paesi che non dispongono di un servizio internet veloce onnipresente.

> [!NOTE]
> Nei primi tempi, questa tecnica generale era conosciuta come {{Glossary("Asynchronous", "Asynchronous")}} JavaScript e XML ({{Glossary("AJAX", "AJAX")}}), perché tendeva a richiedere dati XML. Questo in genere non è più il caso al giorno d'oggi (sarebbe più probabile richiedere JSON), ma il risultato è lo stesso e il termine "AJAX" è ancora spesso utilizzato per descrivere la tecnica.

Per velocizzare ulteriormente le cose, alcuni siti memorizzano anche risorse e dati sul computer dell'utente quando vengono richiesti per la prima volta, il che significa che alle visite successive usano le versioni locali invece di scaricare nuove copie ogni volta che la pagina viene caricata per la prima volta. Il contenuto viene ricaricato dal server solo quando è stato aggiornato.

## La Fetch API

Vediamo un paio di esempi della Fetch API.

### Recupero di contenuti di testo

Per questo esempio, richiederemo dati da alcuni file di testo diversi e li utilizzeremo per popolare un'area di contenuto.

Questa serie di file fungerà da nostro falso database; in una vera applicazione, saremmo più propensi a usare un linguaggio lato server come PHP, Python o Node per richiedere i nostri dati da un database. Tuttavia, qui vogliamo mantenerlo semplice e concentrarci sulla parte client-side di questo.

Per iniziare questo esempio, fai una copia locale di [fetch-start.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/fetch-start.html) e dei quattro file di testo — [verse1.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse1.txt), [verse2.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse2.txt), [verse3.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse3.txt) e [verse4.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse4.txt) — in una nuova directory sul suo computer. In questo esempio, recupereremo un verso diverso della poesia (che potrebbe ben riconoscere) quando viene selezionato nel menu a discesa.

All'interno dell'elemento {{htmlelement("script")}}, aggiunga il seguente codice. Questo memorizza riferimenti agli elementi {{htmlelement("select")}} e {{htmlelement("pre")}} e aggiunge un ascoltatore all'elemento `<select>`, così che quando l'utente seleziona un nuovo valore, il nuovo valore viene passato alla funzione chiamata `updateDisplay()` come parametro.

```js
const verseChoose = document.querySelector("select");
const poemDisplay = document.querySelector("pre");

verseChoose.addEventListener("change", () => {
  const verse = verseChoose.value;
  updateDisplay(verse);
});
```

Definiamo la nostra funzione `updateDisplay()`. Innanzitutto, aggiungere il seguente sotto il suo blocco di codice precedente — questo è lo scheletro vuoto della funzione.

```js-nolint
function updateDisplay(verse) {

}
```

Inizieremo la nostra funzione costruendo un URL relativo che punta al file di testo che vogliamo caricare, poiché ne avremo bisogno più avanti. Il valore dell'elemento {{htmlelement("select")}} in qualsiasi momento è lo stesso del testo all'interno dell'opzione selezionata {{htmlelement("option")}} (a meno che non si specifichi un valore diverso in un attributo value) — quindi per esempio "Verse 1". Il file di testo del verso corrispondente è "verse1.txt", ed è nella stessa directory del file HTML, quindi basta il nome del file.

Tuttavia, i server web tendono a essere case-sensitive, e il nome del file non ha uno spazio al suo interno. Per convertire "Verse 1" in "verse1.txt", dobbiamo convertire la 'V' in minuscolo, rimuovere lo spazio e aggiungere ".txt" alla fine. Questo può essere fatto con {{jsxref("String.replace", "replace()")}}, {{jsxref("String.toLowerCase", "toLowerCase()")}} e [template literal](/it/docs/Web/JavaScript/Reference/Template_literals). Aggiungere le seguenti righe all'interno della sua funzione `updateDisplay()`:

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

C'è molto da spiegare in questo punto.

Innanzitutto, il punto d'ingresso alla Fetch API è una funzione globale chiamata [`fetch()`](/it/docs/Web/API/Window/fetch), che prende l'URL come parametro (prende un altro parametro opzionale per le impostazioni personalizzate, ma non lo usiamo qui).

Successivamente, `fetch()` è un'API asincrona che restituisce un {{jsxref("Promise")}}. Se non sa cosa sia, legga il modulo su [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS), e in particolare la lezione sui [promises](/it/docs/Learn_web_development/Extensions/Async_JS/Promises), poi torni qui. Troverà che quell'articolo parla anche dell'API `fetch()`!

Poiché `fetch()` restituisce una promessa, passiamo una funzione nel metodo {{jsxref("Promise/then", "then()")}} della promessa restituita. Questo metodo verrà chiamato quando la richiesta HTTP ha ricevuto una risposta dal server. Nell'handler, verifichiamo che la richiesta sia andata a buon fine e lanciamo un errore se non lo è stata. Altrimenti, chiamiamo [`response.text()`](/it/docs/Web/API/Response/text), per ottenere il corpo della risposta come testo.

Si scopre che `response.text()` è _anche_ asincrono, quindi restituiamo la promessa che restituisce, e passiamo una funzione nel metodo `then()` di questa nuova promessa. Questa funzione verrà chiamata quando il testo della risposta sarà pronto, e al suo interno aggiorneremo il nostro blocco `<pre>` con il testo.

Infine, concatenamo un gestore {{jsxref("Promise/catch", "catch()")}} alla fine, per catturare eventuali errori lanciati in una delle funzioni asincrone che abbiamo chiamato o nei loro handler.

Un problema con l'esempio così com'è, è che non mostrerà alcun verso della poesia quando si carica per la prima volta. Per risolverlo, aggiunga le seguenti due righe in fondo al suo codice (appena sopra il tag di chiusura `</script>`) per caricare il verso 1 per impostazione predefinita e assicurarsi che l'elemento {{htmlelement("select")}} mostri sempre il valore corretto:

```js
updateDisplay("Verse 1");
verseChoose.value = "Verse 1";
```

#### Servire il suo esempio da un server

I browser moderni non eseguiranno richieste HTTP se si esegue l'esempio da un file locale. Questo a causa delle restrizioni di sicurezza (per maggiori informazioni sulla sicurezza del web, legga [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security)).

Per ovviare a questo, dobbiamo testare l'esempio eseguendolo attraverso un server web locale. Per sapere come fare ciò, veda [Come impostare un server di test locale?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

### The can store

In questo esempio abbiamo creato un sito di esempio chiamato The Can Store — è un supermercato immaginario che vende solo prodotti in scatola. Può trovare questo [esempio in diretta su GitHub](https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/), e [vedere il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/fetching-data/can-store).

![Un sito di e-commerce fittizio che mostra opzioni di ricerca nella colonna di sinistra e risultati di ricerca dei prodotti nella colonna di destra.](can-store.png)

Per impostazione predefinita, il sito visualizza tutti i prodotti, ma può utilizzare i controlli del modulo nella colonna di sinistra per filtrarli per categoria, termine di ricerca, o entrambi.

C'è parecchio codice complesso che si occupa di filtrare i prodotti per categoria e termini di ricerca, manipolando le stringhe in modo che i dati vengano visualizzati correttamente nell'interfaccia utente, ecc. Non discuteremo di tutto nell'articolo, ma può trovare commenti estensivi nel codice (vedi [can-script.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/can-store/can-script.js)).

Spiegheremo, tuttavia, il codice Fetch.

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

- controlliamo che il server non abbia restituito un errore (come [`404 Not Found`](/it/docs/Web/HTTP/Reference/Status/404)). Se l'ha fatto, lanciamo l'errore.
- chiamiamo [`json()`](/it/docs/Web/API/Response/json) sulla risposta. Questo recupererà i dati come un [oggetto JSON](/it/docs/Learn_web_development/Core/Scripting/JSON). Restituiamo la promessa restituita da `response.json()`.

Successivamente passiamo una funzione nel metodo `then()` di quella promessa restituita. A questa funzione verrà passato un oggetto contenente i dati della risposta come JSON, che passiamo alla funzione `initialize()`. È `initialize()` che inizia il processo di visualizzazione di tutti i prodotti nell'interfaccia utente.

Per gestire gli errori, concatenamo un blocco `.catch()` alla fine della catena. Questo viene eseguito se la promessa fallisce per qualche motivo. Al suo interno includiamo una funzione che viene passata come parametro, un oggetto `err`. Questo oggetto `err` può essere utilizzato per riportare la natura dell'errore avvenuto, in questo caso lo facciamo con un semplice `console.error()`.

Tuttavia, un sito web completo gestirebbe questo errore in modo più elegante visualizzando un messaggio sullo schermo dell'utente e magari offrendo opzioni per rimediare alla situazione, ma non abbiamo bisogno di niente di più di un semplice `console.error()`.

Può testare il caso di fallimento da solo:

1. Faccia una copia locale dei file di esempio.
2. Esegua il codice attraverso un server web (come descritto sopra, in [Servire il suo esempio da un server](#servire_il_suo_esempio_da_un_server)).
3. Modifichi il percorso del file da recuperare, in qualcosa come 'produc.json' (si assicuri che sia scritto male).
4. Ora carichi il file index nel suo browser (tramite `localhost:8000`) e guardi nella console degli sviluppatori del suo browser. Vedrà un messaggio simile a "Fetch problem: HTTP error: 404".

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

Funziona in modo molto simile al precedente, tranne che invece di usare [`json()`](/it/docs/Web/API/Response/json), usiamo [`blob()`](/it/docs/Web/API/Response/blob). In questo caso vogliamo restituire la nostra risposta come file immagine, e il formato dati che usiamo per quello è [Blob](/it/docs/Web/API/Blob) (il termine è un'abbreviazione di "Binary Large Object" e può essere usato fondamentalmente per rappresentare grandi oggetti simili a file, come immagini o file video).

Una volta ricevuto correttamente il nostro blob, lo passiamo alla nostra funzione `showProduct()`, che lo visualizza.

## La XMLHttpRequest API

A volte, soprattutto in codice più vecchio, vedrà un'altra API chiamata [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest) (spesso abbreviata come "XHR") usata per effettuare richieste HTTP. Questo ha preceduto Fetch, ed è stata veramente la prima API ampiamente usata per implementare AJAX. Si raccomanda di usare Fetch se può: è un'API più semplice e ha più funzionalità di `XMLHttpRequest`. Non passeremo attraverso un esempio che usa `XMLHttpRequest`, ma le mostreremo come apparirebbe la versione `XMLHttpRequest` della nostra prima richiesta al can store:

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

Ci sono cinque fasi in questo:

1. Creare un nuovo oggetto `XMLHttpRequest`.
2. Chiamare il suo metodo [`open()`](/it/docs/Web/API/XMLHttpRequest/open) per inizializzarlo.
3. Aggiungere un event listener al suo evento [`load`](/it/docs/Web/API/XMLHttpRequest/load_event), che si attiva quando la risposta è stata completata con successo. Nell'ascoltatore chiamiamo `initialize()` con i dati.
4. Aggiungere un event listener al suo evento [`error`](/it/docs/Web/API/XMLHttpRequest/error_event), che si attiva quando la richiesta incontra un errore.
5. Inviare la richiesta.

Dobbiamo anche avvolgere l'intera cosa nel blocco [try...catch](/it/docs/Web/JavaScript/Reference/Statements/try...catch), per gestire eventuali errori lanciati da `open()` o `send()`.

Speriamo che pensi che la Fetch API sia un miglioramento rispetto a questo. In particolare, si noti come dobbiamo gestire gli errori in due posti diversi.

## Riepilogo

Questo articolo mostra come iniziare a lavorare con Fetch per recuperare dati dal server.

## Vedi anche

Ci sono tuttavia molti argomenti diversi discussi in questo articolo, che ha solo veramente sfiorato la superficie. Per molti più dettagli su questi argomenti, provi i seguenti articoli:

- [Utilizzare Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch)
- [Promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Lavorare con dati JSON](/it/docs/Learn_web_development/Core/Scripting/JSON)
- [Panoramica di HTTP](/it/docs/Web/HTTP/Guides/Overview)
- [Programmazione lato server del sito web](/it/docs/Learn_web_development/Extensions/Server-side)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/DOM_scripting","Learn_web_development/Core/Scripting/JSON", "Learn_web_development/Core/Scripting")}}
