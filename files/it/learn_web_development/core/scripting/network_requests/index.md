---
title: Effettuare richieste di rete con JavaScript
short-title: Richieste di rete
slug: Learn_web_development/Core/Scripting/Network_requests
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/DOM_scripting","Learn_web_development/Core/Scripting/JSON", "Learn_web_development/Core/Scripting")}}

Un'altra attività molto comune nei siti web e nelle applicazioni moderne è effettuare richieste di rete per recuperare dati individuali dal server, al fine di aggiornare sezioni di una pagina web senza dover caricare un'intera nuova pagina. Questo apparente piccolo dettaglio ha avuto un enorme impatto sulle prestazioni e sul comportamento dei siti, quindi in questo articolo spiegheremo il concetto e esamineremo le tecnologie che lo rendono possibile: in particolare, la [Fetch API](/it/docs/Web/API/Fetch_API).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Richieste di rete asincrone, che sono di gran lunga il caso d'uso asincrono di JavaScript più comune sul web.</li>
          <li>Tipi comuni di risorse che vengono recuperate dalla rete: JSON, asset multimediali, dati da API RESTful.</li>
          <li>Come usare <code>fetch()</code> per implementare richieste di rete asincrone.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Qual è il problema qui?

Una pagina web è composta da una pagina HTML e (di solito) vari altri file, come fogli di stile, script e immagini. Il modello base di caricamento della pagina sul Web è che il tuo browser effettua una o più richieste HTTP al server per ottenere i file necessari per visualizzare la pagina, e il server risponde con i file richiesti. Se visiti un'altra pagina, il browser richiede i nuovi file e il server risponde con essi.

![Caricamento tradizionale della pagina](traditional-loading.svg)

Questo modello funziona perfettamente bene per molti siti. Ma considera un sito web che è molto orientato ai dati. Per esempio, un sito web di una biblioteca come la [Vancouver Public Library](https://www.vpl.ca/). Tra le altre cose, potresti considerare un sito come questo un'interfaccia utente per un database. Potrebbe permetterti di cercare un particolare genere di libro, oppure mostrarti raccomandazioni di libri che potresti apprezzare, basandosi sui libri che hai preso in prestito in precedenza. Quando fai questo, è necessario aggiornare la pagina con il nuovo set di libri da visualizzare. Ma nota che la maggior parte del contenuto della pagina — inclusi elementi come l'intestazione della pagina, la barra laterale e il piè di pagina — rimane lo stesso.

Il problema con il modello tradizionale qui è che dovremmo recuperare e caricare l'intera pagina, anche quando abbiamo bisogno di aggiornare solo una parte di essa. Questo è inefficiente e può risultare in una scarsa esperienza utente.

Quindi, invece del modello tradizionale, molti siti web utilizzano API JavaScript per richiedere dati dal server e aggiornare il contenuto della pagina senza un caricamento della pagina. Pertanto, quando l'utente cerca un nuovo prodotto, il browser richiede solo i dati necessari per aggiornare la pagina — il set di nuovi libri da mostrare, per esempio.

![Usare fetch per aggiornare le pagine](fetch-update.svg)

L'API principale qui è la [Fetch API](/it/docs/Web/API/Fetch_API). Questa permette a JavaScript in esecuzione su una pagina di effettuare una richiesta HTTP a un server per recuperare risorse specifiche. Quando il server le fornisce, JavaScript può utilizzare i dati per aggiornare la pagina, tipicamente utilizzando [API di manipolazione del DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting). I dati richiesti sono spesso [JSON](/it/docs/Learn_web_development/Core/Scripting/JSON), che è un buon formato per trasferire dati strutturati, ma possono anche essere HTML o semplicemente testo.

Questo è un modello comune per siti orientati ai dati come Amazon, YouTube, eBay, e così via. Con questo modello:

- Gli aggiornamenti delle pagine sono molto più rapidi e non devi aspettare che la pagina si aggiorni, il che significa che il sito sembra più veloce e reattivo.
- Meno dati vengono scaricati ad ogni aggiornamento, il che significa meno banda inutilmente sprecata. Questo potrebbe non essere un problema tanto grande su un desktop con una connessione a banda larga, ma è un problema significativo sui dispositivi mobili e nei paesi dove non c'è una disponibilità diffusa di un servizio internet veloce.

> [!NOTE]
> Nei primi giorni, questa tecnica generale era conosciuta come JavaScript e XML asincrono ({{Glossary("AJAX", "AJAX")}}), perché di solito richiedeva dati XML. Questo non è normalmente il caso oggi (saresti più propenso a richiedere JSON), ma il risultato è lo stesso, e il termine "AJAX" è ancora spesso usato per descrivere la tecnica.

Per accelerare ulteriormente le cose, alcuni siti memorizzano anche asset e dati sul computer dell'utente quando vengono richiesti per la prima volta, il che significa che nelle visite successive utilizzano le versioni locali invece di scaricare nuove copie ogni volta che la pagina viene caricata per la prima volta. Il contenuto viene ricaricato dal server solo quando è stato aggiornato.

## La Fetch API

In questa sezione esamineremo alcuni esempi della Fetch API.

Gli esempi seguenti hanno un certo livello di complessità e mostrano come utilizzare la Fetch API in alcuni contesti reali. Se non hai mai usato fetch prima, potresti voler iniziare lavorando attraverso il tutorial interattivo di Scrimba [Primo fetch](https://scrimba.com/frontend-path-c0j/~0lu?via=mdn) <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>, che fornisce un'introduzione molto semplice.

### Recuperare contenuti di testo

Per questo esempio, richiederemo dati da alcuni file di testo diversi e li useremo per popolare un'area di contenuto.

Questa serie di file fungerà da nostro falso database; in un'applicazione reale, saremmo più propensi a utilizzare un linguaggio server-side come PHP, Python o Node per richiedere i nostri dati da un database. Qui, tuttavia, vogliamo mantenerlo semplice e concentrarci sulla parte client-side di questo.

Per iniziare questo esempio, fai una copia locale di [fetch-start.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/fetch-start.html) e dei quattro file di testo — [verse1.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse1.txt), [verse2.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse2.txt), [verse3.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse3.txt), e [verse4.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse4.txt) — in una nuova directory sul tuo computer. In questo esempio, recupereremo una diversa strofa del poema (che potresti benissimo riconoscere) quando viene selezionato nel menu a tendina.

Appena all'interno dell'elemento {{htmlelement("script")}}, aggiungi il seguente codice. Questo memorizza riferimenti agli elementi {{htmlelement("select")}} e {{htmlelement("pre")}} e aggiunge un ascoltatore all'elemento `<select>`, in modo che quando l'utente seleziona un nuovo valore, questo nuovo valore venga passato alla funzione `updateDisplay()` come parametro.

```js
const verseChoose = document.querySelector("select");
const poemDisplay = document.querySelector("pre");

verseChoose.addEventListener("change", () => {
  const verse = verseChoose.value;
  updateDisplay(verse);
});
```

Definiamo la nostra funzione `updateDisplay()`. Per prima cosa, metti il seguente sotto il tuo blocco di codice precedente — questa è la struttura vuota della funzione.

```js-nolint
function updateDisplay(verse) {

}
```

Inizieremo la nostra funzione costruendo un URL relativo che punta al file di testo che vogliamo caricare, poiché ne avremo bisogno più avanti. Il valore dell'elemento {{htmlelement("select")}} in qualsiasi momento è lo stesso del testo all'interno dell'opzione selezionata {{htmlelement("option")}} (a meno che non specifichi un valore diverso in un attributo value) — quindi, per esempio, "Verse 1". Il file di testo della strofa corrispondente è "verse1.txt", ed è nella stessa directory del file HTML, quindi basta il nome del file.

Tuttavia, i server web tendono a essere case-sensitive, e il nome del file non ha uno spazio al suo interno. Per convertire "Verse 1" in "verse1.txt" dobbiamo convertire la 'V' in minuscolo, rimuovere lo spazio, e aggiungere ".txt" alla fine. Questo può essere fatto con {{jsxref("String.replace", "replace()")}}, {{jsxref("String.toLowerCase", "toLowerCase()")}}, e [template literal](/it/docs/Web/JavaScript/Reference/Template_literals). Aggiungi le seguenti righe all'interno della tua funzione `updateDisplay()`:

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

C'è parecchio da spiegare qui.

Per prima cosa, il punto di ingresso alla Fetch API è una funzione globale chiamata [`fetch()`](/it/docs/Web/API/Window/fetch), che prende l'URL come parametro (prende un altro parametro opzionale per impostazioni personalizzate, ma non lo usiamo qui).

Successivamente, `fetch()` è un'API asincrona che restituisce una {{jsxref("Promise")}}. Se non sai cos'è, leggi il modulo su [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS), e in particolare la lezione sulle [promesse](/it/docs/Learn_web_development/Extensions/Async_JS/Promises), poi torna qui. Troverai che quell'articolo parla anche dell'API `fetch()`!

Quindi, poiché `fetch()` restituisce una promessa, passiamo una funzione nel metodo {{jsxref("Promise/then", "then()")}} della promessa restituita. Questo metodo verrà chiamato quando la richiesta HTTP ha ricevuto una risposta dal server. Nel gestore, verifichiamo che la richiesta sia riuscita e lanciamo un errore se non lo è. Altrimenti, chiamiamo [`response.text()`](/it/docs/Web/API/Response/text), per ottenere il corpo della risposta come testo.

Risulta che `response.text()` è _anche_ asincrona, quindi restituiamo la promessa che restituisce, e passiamo una funzione nel metodo `then()` di questa nuova promessa. Questa funzione sarà chiamata quando il testo della risposta è pronto, e al suo interno aggiorneremo il nostro blocco `<pre>` con il testo.

Infine, colleghiamo un gestore {{jsxref("Promise/catch", "catch()")}} alla fine, per catturare eventuali errori lanciati in una delle funzioni asincrone che abbiamo chiamato o nei loro gestori.

Un problema con l'esempio così com'è, è che non mostrerà nessuna delle poesie quando si carica per la prima volta. Per risolvere questo, aggiungi le due righe seguenti alla fine del tuo codice (proprio sopra il tag di chiusura `</script>`) per caricare la strofa 1 di default, e assicurati che l'elemento {{htmlelement("select")}} mostri sempre il valore corretto:

```js
updateDisplay("Verse 1");
verseChoose.value = "Verse 1";
```

#### Servire il tuo esempio da un server

I browser moderni non eseguiranno richieste HTTP se esegui l'esempio da un file locale. Questo a causa delle restrizioni di sicurezza (per ulteriori informazioni sulla sicurezza web, leggi [Sicurezza del sito web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security)).

Per aggirare questo, dobbiamo testare l'esempio eseguendolo attraverso un server web locale. Per sapere come fare, vedi [Come impostare un server di test locale?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

### The can store

In questo esempio abbiamo creato un sito di esempio chiamato The Can Store — è un supermercato fittizio che vende solo prodotti in scatola. Puoi trovare questo [esempio attivo su GitHub](https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/), e [vedere il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/fetching-data/can-store).

![Un sito di e-commerce falso che mostra opzioni di ricerca nella colonna di sinistra e risultati di ricerca di prodotti nella colonna di destra.](can-store.png)

Per impostazione predefinita, il sito visualizza tutti i prodotti, ma puoi utilizzare i controlli del modulo nella colonna di sinistra per filtrarli per categoria, o termine di ricerca, o entrambi.

C'è parecchio codice complesso che si occupa di filtrare i prodotti per categoria e termini di ricerca, manipolando le stringhe in modo che i dati vengano visualizzati correttamente nell'interfaccia utente, ecc. Non discuteremo tutto nel dettaglio nell'articolo, ma puoi trovare commenti approfonditi nel codice (vedi [can-script.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/can-store/can-script.js)).

Spiegheremo comunque il codice Fetch.

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

La funzione `fetch()` restituisce una promessa. Se questa viene completata con successo, la funzione all'interno del primo blocco `.then()` contiene la `response` restituita dalla rete.

All'interno di questa funzione noi:

- verifichiamo che il server non abbia restituito un errore (come [`404 Not Found`](/it/docs/Web/HTTP/Reference/Status/404)). Se lo ha fatto, lanciamo l'errore.
- chiamiamo [`json()`](/it/docs/Web/API/Response/json) sulla risposta. Questo recupererà i dati come un [oggetto JSON](/it/docs/Learn_web_development/Core/Scripting/JSON). Restituiamo la promessa che `response.json()` restituisce.

Successivamente, passiamo una funzione nel metodo `then()` di quella promessa restituita. Questa funzione riceverà un oggetto contenente i dati della risposta come JSON, che passiamo alla funzione `initialize()`. È `initialize()` che avvia il processo di visualizzazione di tutti i prodotti nell'interfaccia utente.

Per gestire gli errori, aggiungiamo un blocco `.catch()` alla fine della catena. Questo viene eseguito se la promessa fallisce per qualche motivo. Al suo interno, includiamo una funzione che viene passata come parametro, un oggetto `err`. Questo oggetto `err` può essere usato per segnalare la natura dell'errore che si è verificato, in questo caso lo facciamo con un semplice `console.error()`.

Tuttavia, un sito web completo gestirebbe questo errore in modo più elegante visualizzando un messaggio sullo schermo dell'utente e magari offrendo opzioni per rimediare alla situazione, ma non abbiamo bisogno di nulla di più di un semplice `console.error()`.

Puoi testare il caso di fallimento tu stesso:

<!-- cSpell:ignore produc -->

1. Fai una copia locale dei file di esempio.
2. Esegui il codice attraverso un server web (come descritto sopra, in [Servire il tuo esempio da un server](#servire_il_tuo_esempio_da_un_server)).
3. Modifica il percorso del file da recuperare, in qualcosa come 'produc.json' (assicurati che sia scritto male).
4. Ora carica il file index nel tuo browser (via `localhost:8000`) e guarda nella console dello sviluppatore del tuo browser. Vedrai un messaggio simile a "Problema di fetch: errore HTTP: 404".

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

Questo funziona in modo molto simile al precedente, tranne il fatto che invece di usare [`json()`](/it/docs/Web/API/Response/json), usiamo [`blob()`](/it/docs/Web/API/Response/blob). In questo caso vogliamo restituire la nostra risposta come un file immagine, e il formato di dati che usiamo per questo è [Blob](/it/docs/Web/API/Blob) (il termine è un'abbreviazione di "Binary Large Object" e può essere sostanzialmente usato per rappresentare oggetti di file di grandi dimensioni, come immagini o file video).

Una volta che abbiamo ricevuto con successo il nostro blob, lo passiamo alla nostra funzione `showProduct()`, che lo visualizza.

## L'API XMLHttpRequest

A volte, specialmente nel codice più vecchio, vedrai un'altra API chiamata [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest) (spesso abbreviata in "XHR") utilizzata per effettuare richieste HTTP. Questa precede Fetch ed è stata veramente la prima API ampiamente utilizzata per implementare AJAX. Ti consigliamo di usare Fetch se puoi: è un'API più semplice e ha più funzionalità di `XMLHttpRequest`. Non passeremo attraverso un esempio che utilizza `XMLHttpRequest`, ma ti mostreremo come apparirebbe la versione `XMLHttpRequest` della nostra prima richiesta del can store:

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

1. Crea un nuovo oggetto `XMLHttpRequest`.
2. Chiama il suo metodo [`open()`](/it/docs/Web/API/XMLHttpRequest/open) per inizializzarlo.
3. Aggiungi un ascoltatore di eventi al suo evento [`load`](/it/docs/Web/API/XMLHttpRequest/load_event), che si attiva quando la risposta è stata completata con successo. Nell'ascoltatore chiamiamo `initialize()` con i dati.
4. Aggiungi un ascoltatore di eventi al suo evento [`error`](/it/docs/Web/API/XMLHttpRequest/error_event), che si attiva quando la richiesta incontra un errore
5. Invia la richiesta.

Dobbiamo anche avvolgere l'intero processo nel blocco [try...catch](/it/docs/Web/JavaScript/Reference/Statements/try...catch), per gestire eventuali errori lanciati da `open()` o `send()`.

Speriamo che pensi che la Fetch API sia un miglioramento rispetto a questo. In particolare, vedi come dobbiamo gestire gli errori in due posti diversi.

## Riepilogo

Questo articolo mostra come iniziare a lavorare con Fetch per recuperare dati dal server.

## Vedi anche

Tuttavia, ci sono molti argomenti diversi discussi in questo articolo, che hanno solo raschiato la superficie. Per molti più dettagli su questi argomenti, prova i seguenti articoli:

- [Usare Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch)
- [Promesse](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Lavorare con dati JSON](/it/docs/Learn_web_development/Core/Scripting/JSON)
- [Una panoramica di HTTP](/it/docs/Web/HTTP/Guides/Overview)
- [Programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/DOM_scripting","Learn_web_development/Core/Scripting/JSON", "Learn_web_development/Core/Scripting")}}
