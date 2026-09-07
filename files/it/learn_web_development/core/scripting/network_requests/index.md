---
title: Effettuare richieste di rete con JavaScript
short-title: Richieste di rete
slug: Learn_web_development/Core/Scripting/Network_requests
l10n:
  sourceCommit: 952d0a3a076d16f0cf7566040e5cbe059996138d
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Image_gallery","Learn_web_development/Core/Scripting/JSON", "Learn_web_development/Core/Scripting")}}

Un'altra attività molto comune nei siti web e nelle applicazioni moderne consiste nell'effettuare richieste di rete per recuperare singoli elementi di dati dal server e aggiornare sezioni di una pagina web senza dover caricare un'intera nuova pagina. Questo dettaglio apparentemente piccolo ha avuto un enorme impatto sulle prestazioni e sul comportamento dei siti; in questo articolo verranno illustrati il concetto e le tecnologie che lo rendono possibile: in particolare, la [Fetch API](/it/docs/Web/API/Fetch_API).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Richieste di rete asincrone, che rappresentano di gran lunga il caso d'uso più comune di JavaScript asincrono sul Web.</li>
          <li>Tipi comuni di risorse recuperate dalla rete: JSON, risorse multimediali, dati da API RESTful.</li>
          <li>Come usare <code>fetch()</code> per implementare richieste di rete asincrone.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Qual è il problema?

Una pagina web è composta da una pagina HTML e, solitamente, da vari altri file, come fogli di stile, script e immagini. Il modello di base per il caricamento delle pagine sul Web prevede che il browser effettui una o più richieste HTTP al server per ottenere i file necessari a visualizzare la pagina, e che il server risponda con i file richiesti. Se si visita un'altra pagina, il browser richiede i nuovi file e il server risponde inviandoli.

![Caricamento tradizionale della pagina](traditional-loading.svg)

Questo modello funziona perfettamente per molti siti. Si consideri però un sito web fortemente basato sui dati. Per esempio, un sito di una biblioteca come la [Vancouver Public Library](https://www.vpl.ca/). Tra le altre cose, un sito di questo tipo può essere considerato come un'interfaccia utente per un database. Potrebbe consentire di cercare un particolare genere di libri oppure mostrare consigli su libri che potrebbero piacere, in base ai libri presi in prestito in precedenza. Quando questo accade, la pagina deve essere aggiornata con il nuovo insieme di libri da visualizzare. Tuttavia, la maggior parte del contenuto della pagina — inclusi elementi come intestazione, barra laterale e piè di pagina — rimane invariata.

Il problema del modello tradizionale in questo caso è che sarebbe necessario recuperare e caricare l'intera pagina, anche quando occorre aggiornare soltanto una sua parte. Questo è inefficiente e può portare a una scarsa esperienza utente.

Perciò, invece del modello tradizionale, molti siti web usano API JavaScript per richiedere dati al server e aggiornare il contenuto della pagina senza ricaricarla. Quindi, quando l'utente cerca un nuovo prodotto, il browser richiede soltanto i dati necessari per aggiornare la pagina — per esempio, il nuovo insieme di libri da visualizzare.

![Uso di fetch per aggiornare le pagine](fetch-update.svg)

L'API principale in questo caso è la [Fetch API](/it/docs/Web/API/Fetch_API). Questa consente al codice JavaScript in esecuzione in una pagina di effettuare una richiesta [HTTP](/it/docs/Web/HTTP) a un server per recuperare risorse specifiche. Quando il server le fornisce, JavaScript può usare i dati per aggiornare la pagina, in genere tramite le [API di manipolazione del DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting). I dati richiesti sono spesso in formato [JSON](/it/docs/Learn_web_development/Core/Scripting/JSON), un formato adatto per trasferire dati strutturati, ma possono anche essere HTML o semplice testo.

Questo è un modello comune per siti basati sui dati come Amazon, YouTube, eBay e così via. Con questo modello:

- Gli aggiornamenti della pagina sono molto più rapidi e non è necessario attendere l'aggiornamento della pagina, quindi il sito sembra più veloce e reattivo.
- Vengono scaricati meno dati a ogni aggiornamento, riducendo la larghezza di banda sprecata. Questo potrebbe non essere un problema rilevante su un computer desktop con una connessione a banda larga, ma è un problema importante sui dispositivi mobili e nei paesi in cui un servizio Internet veloce non è diffuso ovunque.

> [!NOTE]
> Agli inizi, questa tecnica generale era nota come JavaScript e XML {{Glossary("Asynchronous", "asincroni")}} ({{Glossary("AJAX", "AJAX")}}), poiché tendeva a richiedere dati XML. Oggi normalmente non è più così (è più probabile richiedere JSON), ma il risultato rimane lo stesso e il termine "AJAX" viene ancora spesso usato per descrivere questa tecnica.

Per velocizzare ulteriormente le cose, alcuni siti memorizzano anche risorse e dati sul computer dell'utente quando vengono richiesti per la prima volta; ciò significa che nelle visite successive usano le versioni locali invece di scaricare nuove copie ogni volta che la pagina viene caricata inizialmente. Il contenuto viene ricaricato dal server solo quando è stato aggiornato.

## La Fetch API

In questa sezione verranno esaminati alcuni esempi della Fetch API.

Gli esempi seguenti presentano un certo livello di complessità e mostrano come utilizzare la Fetch API in alcuni contesti reali. Se non è mai stato usato fetch in precedenza, potrebbe essere utile iniziare seguendo il tutorial interattivo [First fetch](https://scrimba.com/frontend-path-c0j/~0lu?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>, che fornisce una semplice introduzione guidata.

### Recuperare contenuto testuale

In questo esempio verranno richiesti dati da alcuni file di testo diversi e utilizzati per popolare un'area di contenuto.

Questa serie di file fungerà da database fittizio; in un'applicazione reale, sarebbe più probabile usare un linguaggio lato server come PHP, Python o Node per richiedere i dati da un database. Qui, tuttavia, l'obiettivo è mantenere le cose semplici e concentrarsi sulla parte lato client.

Per iniziare questo esempio, creare una copia locale di [fetch-start.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/fetch-start.html) e dei quattro file di testo — [verse1.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse1.txt), [verse2.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse2.txt), [verse3.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse3.txt) e [verse4.txt](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/verse4.txt) — in una nuova directory sul computer. In questo esempio verrà recuperata una strofa diversa della poesia (che potrebbe essere riconoscibile) quando viene selezionata nel menu a discesa.

Appena all'interno dell'elemento {{htmlelement("script")}}, aggiungere il codice seguente. Questo memorizza riferimenti agli elementi {{htmlelement("select")}} e {{htmlelement("pre")}} e aggiunge un listener all'elemento `<select>`, in modo che quando l'utente seleziona un nuovo valore, il nuovo valore venga passato come parametro alla funzione denominata `updateDisplay()`.

```js
const verseChoose = document.querySelector("select");
const poemDisplay = document.querySelector("pre");

verseChoose.addEventListener("change", () => {
  const verse = verseChoose.value;
  updateDisplay(verse);
});
```

Definiamo la funzione `updateDisplay()`. Innanzitutto, inserire quanto segue sotto il blocco di codice precedente: questa è la struttura vuota della funzione.

```js-nolint
function updateDisplay(verse) {

}
```

La funzione inizierà costruendo un URL relativo che punta al file di testo da caricare, poiché servirà in seguito. Il valore dell'elemento {{htmlelement("select")}} in qualsiasi momento è uguale al testo all'interno dell'elemento {{htmlelement("option")}} selezionato (a meno che non venga specificato un valore diverso in un attributo value), quindi per esempio "Verse 1". Il file di testo della strofa corrispondente è "verse1.txt" e si trova nella stessa directory del file HTML; pertanto è sufficiente il solo nome del file.

Tuttavia, i server web tendono a distinguere tra maiuscole e minuscole e il nome del file non contiene uno spazio. Per convertire "Verse 1" in "verse1.txt" occorre convertire la "V" in minuscolo, rimuovere lo spazio e aggiungere ".txt" alla fine. Questo può essere fatto con {{jsxref("String.replace", "replace()")}}, {{jsxref("String.toLowerCase", "toLowerCase()")}} e un [template literal](/it/docs/Web/JavaScript/Reference/Template_literals). Aggiungere le righe seguenti all'interno della funzione `updateDisplay()`:

```js
verse = verse.replace(" ", "").toLowerCase();
const url = `${verse}.txt`;
```

Infine è possibile usare la Fetch API:

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

Ci sono diversi aspetti da analizzare.

Innanzitutto, il punto di ingresso della Fetch API è una funzione globale chiamata [`fetch()`](/it/docs/Web/API/Window/fetch), che accetta l'URL come parametro (accetta un altro parametro opzionale per impostazioni personalizzate, ma qui non viene usato).

Successivamente, `fetch()` è un'API asincrona che restituisce una {{jsxref("Promise")}}. Se non si sa cosa sia, leggere il modulo su [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS), in particolare la lezione sulle [promise](/it/docs/Learn_web_development/Extensions/Async_JS/Promises), quindi tornare qui. Quell'articolo tratta anche dell'API `fetch()`.

Poiché `fetch()` restituisce una promise, viene passata una funzione al metodo {{jsxref("Promise/then", "then()")}} della promise restituita. Questo metodo verrà chiamato quando la richiesta HTTP avrà ricevuto una risposta dal server. Nell'handler, viene verificato che la richiesta sia riuscita e viene generato un errore in caso contrario. Altrimenti, viene chiamato [`response.text()`](/it/docs/Web/API/Response/text) per ottenere il corpo della risposta come testo.

Si scopre che anche `response.text()` è _asincrono_, pertanto viene restituita la promise che genera e viene passata una funzione al metodo `then()` di questa nuova promise. Questa funzione verrà chiamata quando il testo della risposta sarà pronto e al suo interno verrà aggiornato il blocco `<pre>` con il testo.

Infine, alla fine viene concatenato un handler {{jsxref("Promise/catch", "catch()")}} per intercettare eventuali errori generati da una delle funzioni asincrone chiamate o dai relativi handler.

Un problema dell'esempio nello stato attuale è che non mostrerà alcuna parte della poesia al primo caricamento. Per risolverlo, aggiungere le due righe seguenti alla fine del codice, appena sopra il tag di chiusura `</script>`, per caricare la strofa 1 per impostazione predefinita e assicurarsi che l'elemento {{htmlelement("select")}} mostri sempre il valore corretto:

```js
updateDisplay("Verse 1");
verseChoose.value = "Verse 1";
```

#### Servire l'esempio da un server

I browser moderni non eseguono richieste HTTP se l'esempio viene eseguito direttamente da un file locale. Questo è dovuto a restrizioni di sicurezza (per ulteriori informazioni sulla sicurezza web, leggere [Sicurezza dei siti web](/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Website_security)).

Per aggirare questo problema, è necessario testare l'esempio eseguendolo tramite un server web locale. Per scoprire come fare, vedere [Come si configura un server locale per i test?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

### The can store

In questo esempio è stato creato un sito di esempio chiamato The Can Store: è un supermercato fittizio che vende soltanto prodotti in scatola. È possibile trovare questo [esempio in esecuzione su GitHub](https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/) e [visualizzarne il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/fetching-data/can-store).

![Un sito e-commerce fittizio che mostra opzioni di ricerca nella colonna a sinistra e risultati della ricerca dei prodotti nella colonna a destra.](can-store.png)

Per impostazione predefinita, il sito mostra tutti i prodotti, ma è possibile utilizzare i controlli del modulo nella colonna a sinistra per filtrarli in base alla categoria, al termine di ricerca o a entrambi.

È presente una quantità considerevole di codice complesso che si occupa di filtrare i prodotti per categoria e termini di ricerca, manipolare le stringhe affinché i dati vengano visualizzati correttamente nell'interfaccia utente e così via. Non verrà discusso tutto nell'articolo, ma nel codice sono disponibili commenti dettagliati (vedere [can-script.js](https://github.com/mdn/learning-area/blob/main/javascript/apis/fetching-data/can-store/can-script.js)).

Verrà però spiegato il codice Fetch.

Il primo blocco che usa Fetch si trova all'inizio del codice JavaScript:

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

La funzione `fetch()` restituisce una promise. Se questa viene completata correttamente, la funzione all'interno del primo blocco `.then()` contiene la `response` restituita dalla rete.

All'interno di questa funzione:

- viene verificato che il server non abbia restituito un errore, come [`404 Not Found`](/it/docs/Web/HTTP/Reference/Status/404). In caso contrario, viene generato l'errore.
- viene chiamato [`json()`](/it/docs/Web/API/Response/json) sulla risposta. Questo recupera i dati come [oggetto JSON](/it/docs/Learn_web_development/Core/Scripting/JSON). Viene restituita la promise restituita da `response.json()`.

Successivamente viene passata una funzione al metodo `then()` di quella promise restituita. A questa funzione verrà passato un oggetto contenente i dati della risposta come JSON, che viene passato alla funzione `initialize()`. È `initialize()` ad avviare il processo di visualizzazione di tutti i prodotti nell'interfaccia utente.

Per gestire gli errori, viene concatenato un blocco `.catch()` alla fine della catena. Questo viene eseguito se la promise non riesce per qualche motivo. Al suo interno viene inclusa una funzione a cui viene passato come parametro un oggetto `err`. Questo oggetto `err` può essere usato per segnalare la natura dell'errore verificatosi; in questo caso, ciò avviene con un semplice `console.error()`.

Tuttavia, un sito web completo gestirebbe questo errore in modo più elegante visualizzando un messaggio sullo schermo dell'utente e magari offrendo opzioni per risolvere la situazione, ma qui non serve nulla di più di un semplice `console.error()`.

È possibile testare personalmente il caso di errore:

<!-- cSpell:ignore produc -->

1. Creare una copia locale dei file di esempio.
2. Eseguire il codice tramite un server web, come descritto in precedenza in [Servire l'esempio da un server](#servire_l'esempio_da_un_server).
3. Modificare il percorso del file recuperato in qualcosa come 'produc.json' (assicurandosi che sia scritto in modo errato).
4. Caricare ora il file index nel browser, tramite `localhost:8000`, e controllare la console per sviluppatori del browser. Verrà visualizzato un messaggio simile a "Fetch problem: HTTP error: 404".

Il secondo blocco Fetch si trova all'interno della funzione `fetchBlob()`:

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

Questo funziona in modo molto simile al precedente, tranne per il fatto che invece di usare [`json()`](/it/docs/Web/API/Response/json), viene usato [`blob()`](/it/docs/Web/API/Response/blob). In questo caso si vuole restituire la risposta come file immagine e il formato dei dati usato a questo scopo è [Blob](/it/docs/Web/API/Blob) (il termine è l'abbreviazione di "Binary Large Object" e può essere usato essenzialmente per rappresentare oggetti grandi simili a file, come immagini o file video).

Dopo aver ricevuto correttamente il blob, questo viene passato alla funzione `showProduct()`, che lo visualizza.

## L'API XMLHttpRequest

Talvolta, specialmente nel codice meno recente, è possibile trovare un'altra API chiamata [`XMLHttpRequest`](/it/docs/Web/API/XMLHttpRequest) (spesso abbreviata in "XHR") usata per effettuare richieste HTTP. Questa API è precedente a Fetch ed è stata la prima API ampiamente utilizzata per implementare AJAX. Si consiglia di usare Fetch quando possibile: è un'API più semplice e offre più funzionalità di `XMLHttpRequest`. Non verrà illustrato un esempio che usa `XMLHttpRequest`, ma verrà mostrato come apparirebbe la versione `XMLHttpRequest` della prima richiesta del can store:

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

Le fasi sono cinque:

1. Creare un nuovo oggetto `XMLHttpRequest`.
2. Chiamare il relativo metodo [`open()`](/it/docs/Web/API/XMLHttpRequest/open) per inizializzarlo.
3. Aggiungere un event listener al relativo evento [`load`](/it/docs/Web/API/XMLHttpRequestEventTarget/load_event), che viene attivato quando la risposta è stata completata correttamente. Nel listener viene chiamato `initialize()` con i dati.
4. Aggiungere un event listener al relativo evento [`error`](/it/docs/Web/API/XMLHttpRequestEventTarget/error_event), che viene attivato quando la richiesta incontra un errore.
5. Inviare la richiesta.

È inoltre necessario racchiudere tutto nel blocco [try...catch](/it/docs/Web/JavaScript/Reference/Statements/try...catch), per gestire eventuali errori generati da `open()` o `send()`.

Si spera che la Fetch API risulti un miglioramento rispetto a questo approccio. In particolare, si noti come sia necessario gestire gli errori in due punti diversi.

## Riepilogo

Questo articolo mostra come iniziare a lavorare con Fetch per recuperare dati dal server.

## Vedere anche

In questo articolo sono stati tuttavia trattati molti argomenti diversi, dei quali è stata appena scalfita la superficie. Per molti più dettagli su questi argomenti, provare i seguenti articoli:

- [Usare Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch)
- [Promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Lavorare con dati JSON](/it/docs/Learn_web_development/Core/Scripting/JSON)
- [Una panoramica di HTTP](/it/docs/Web/HTTP/Guides/Overview)
- [Programmazione di siti web lato server](/it/docs/Learn_web_development/Extensions/Server-side)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Image_gallery","Learn_web_development/Core/Scripting/JSON", "Learn_web_development/Core/Scripting")}}
