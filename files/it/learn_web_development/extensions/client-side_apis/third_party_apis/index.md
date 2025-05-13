---
title: API di terze parti
slug: Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}

Le API che abbiamo trattato finora sono integrate nel browser, ma non tutte le API lo sono. Molti grandi siti web e servizi come Google Maps, Twitter, Facebook, PayPal, ecc. forniscono API che consentono agli sviluppatori di utilizzare i loro dati (ad esempio, visualizzare il flusso di Twitter sul proprio blog) o servizi (ad esempio, utilizzare il login di Facebook per accedere ai propri utenti). Questo articolo esamina la differenza tra le API del browser e le API di terze parti e mostra alcuni usi tipici delle seconde.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, soprattutto <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">i concetti base sugli oggetti JavaScript</a> e la copertura delle API di base come <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">Richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti dietro le API di terze parti e i modelli associati, come le chiavi API.</li>
          <li>Utilizzare un'API di mappe di terze parti.</li>
          <li>Utilizzare un'API RESTful.</li>
          <li>Utilizzare le API di YouTube di Google.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono le API di terze parti?

Le API di terze parti sono API fornite da terzi — generalmente aziende come Facebook, Twitter o Google — che consentono di accedere alla loro funzionalità tramite JavaScript e utilizzarla sul proprio sito. Uno degli esempi più evidenti è l'utilizzo delle API di mappatura per visualizzare mappe personalizzate sulle proprie pagine.

Esaminiamo un [esempio semplice di API di Mapquest](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/mapquest) e usiamolo per illustrare in che modo le API di terze parti differiscono dalle API del browser.

### Si trovano su server di terzi

Le API del browser sono integrate nel browser — può accedervi da JavaScript immediatamente. Ad esempio, l'API Web Audio che [abbiamo visto nell'articolo introduttivo](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#how_do_apis_work) è accessibile utilizzando l'oggetto nativo [`AudioContext`](/it/docs/Web/API/AudioContext). Ad esempio:

```js
const audioCtx = new AudioContext();
// …
const audioElement = document.querySelector("audio");
// …
const audioSource = audioCtx.createMediaElementSource(audioElement);
// etc.
```

Le API di terze parti, al contrario, si trovano su server di terzi. Per accedervi da JavaScript è necessario prima connettersi alla funzionalità dell'API e renderla disponibile sulla propria pagina. Questo comporta generalmente il collegamento a una libreria JavaScript disponibile sul server tramite un elemento {{htmlelement("script")}}, come visto nel nostro esempio di Mapquest:

```html
<script
  src="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.js"
  defer></script>
<link
  rel="stylesheet"
  href="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.css" />
```

Può poi iniziare a utilizzare gli oggetti disponibili in quella libreria. Ad esempio:

```js
const map = L.mapquest.map("map", {
  center: [53.480759, -2.242631],
  layers: L.mapquest.tileLayer("map"),
  zoom: 12,
});
```

Qui stiamo creando una variabile per memorizzare le informazioni sulla mappa, quindi creiamo una nuova mappa utilizzando il metodo `mapquest.map()`, che prende come parametri l'ID di un elemento {{htmlelement("div")}} in cui si desidera visualizzare la mappa ('map'), e un oggetto opzioni contenente i dettagli della mappa particolare che si desidera visualizzare. In questo caso specifichiamo le coordinate del centro della mappa, un layer di mappa di tipo `map` da mostrare (creato utilizzando il metodo `mapquest.tileLayer()`), e il livello di zoom predefinito.

Queste sono tutte le informazioni che l'API di Mapquest necessita per tracciare una mappa semplice. Il server a cui si sta connettendo gestisce tutte le cose complicate, come mostrare le tessere di mappa corrette per l'area visualizzata, ecc.

> [!NOTE]
> Alcune API gestiscono l'accesso alla loro funzionalità in modo leggermente diverso, richiedendo al sviluppatore di effettuare una richiesta HTTP con un pattern di URL specifico per recuperare dati. Queste sono chiamate [API RESTful — mostreremo un esempio più avanti](#a_restful_api_%e2%80%94_nytimes).

### Richiedono solitamente chiavi API

La sicurezza per le API del browser tende a essere gestita da richieste di autorizzazione, come [discusso nel nostro primo articolo](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#they_have_additional_security_mechanisms_where_appropriate). Lo scopo di questi è che l'utente sappia cosa sta succedendo nei siti web che visita ed è meno probabile che cada vittima di qualcuno che utilizza un'API in modo malevolo.

Le API di terze parti hanno un sistema di autorizzazioni leggermente diverso — tendono a utilizzare chiavi sviluppatori per consentire agli sviluppatori l'accesso alla funzionalità dell'API, che è più per proteggere il fornitore dell'API che l'utente.

Troverà una linea simile alla seguente nell'esempio API di Mapquest:

```js
L.mapquest.key = "YOUR-API-KEY-HERE";
```

Questa linea specifica una chiave API o sviluppatore da utilizzare nella propria applicazione — lo sviluppatore dell'applicazione deve fare richiesta per ottenere una chiave e quindi includerla nel loro codice per essere autorizzato ad accedere alla funzionalità dell'API. Nel nostro esempio abbiamo fornito solo un segnaposto.

> [!NOTE]
> Quando crea i propri esempi, userà la sua chiave API al posto di qualsiasi segnaposto.

Altre API possono richiedere di includere la chiave in modo leggermente diverso, ma il modello è relativamente simile per la maggior parte di essi.

Richiedere una chiave consente al fornitore dell'API di tenere gli utenti dell'API responsabili delle loro azioni. Quando lo sviluppatore si è registrato per una chiave, allora è conosciuto dal fornitore dell'API e possono essere intraprese azioni se inizia a fare qualcosa di malevolo con l'API (come tracciare la posizione delle persone o cercare di spammare l'API con un carico di richieste per far smettere di funzionare, ad esempio). L'azione più semplice sarebbe semplicemente revocare i loro privilegi API.

## Estendere l'esempio di Mapquest

Aggiungiamo alcune funzionalità all'esempio di Mapquest per dimostrare come utilizzare alcune altre caratteristiche dell'API.

1. Per iniziare questa sezione, faccia una copia del [file iniziale di mapquest](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/mapquest/start/index.html), in una nuova directory. Se ha già clonato il [repository degli esempi](https://github.com/mdn/learning-area), avrà già una copia di questo file, che può trovare nella directory _javascript/apis/third-party-apis/mapquest/start_.
2. Successivamente, deve andare al [sito degli sviluppatori di Mapquest](https://developer.mapquest.com/), creare un account e quindi creare una chiave sviluppatore da utilizzare con il suo esempio. (Al momento della scrittura, veniva chiamato "consumer key" sul sito e il processo di creazione della chiave richiedeva anche un opzionale "callback URL". Non è necessario inserire un URL qui: basta lasciarlo vuoto.)
3. Apra il suo file iniziale e sostituisca il segnaposto della chiave API con la sua chiave.

### Cambiare il tipo di mappa

Ci sono diversi tipi di mappa che possono essere mostrati con l'API di Mapquest. Per farlo, trovi la seguente riga:

```js
layers: L.mapquest.tileLayer("map");
```

Provi a cambiare `'map'` a `'hybrid'` per mostrare una mappa in stile ibrido. Provi anche altri valori. La [pagina di riferimento `tileLayer`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-tile-layer/) mostra le diverse opzioni disponibili, oltre a molte altre informazioni.

### Aggiungere diversi controlli

La mappa ha una serie di controlli disponibili; per impostazione predefinita mostra solo un controllo zoom. Può espandere i controlli disponibili utilizzando il metodo `map.addControl()`; aggiunga questo al suo codice:

```js
map.addControl(L.mapquest.control());
```

Il metodo [`mapquest.control()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-control/) crea solo un set di controlli completo, e viene posizionato nell'angolo in alto a destra per impostazione predefinita. Può regolare la posizione specificando un oggetto opzioni come parametro per il controllo contenente una proprietà `position`, il cui valore è una stringa che specifica una posizione per il controllo. Provi questo, per esempio:

```js
map.addControl(L.mapquest.control({ position: "bottomright" }));
```

Ci sono altri tipi di controllo disponibili, ad esempio [`mapquest.searchControl()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-search-control/) e [`mapquest.satelliteControl()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-satellite-control/), e alcuni sono abbastanza complessi e potenti. Si diverta a sperimentare per vedere cosa riesce a creare.

### Aggiungere un marcatore personalizzato

Aggiungere un marcatore (icona) in un certo punto della mappa è facile — basta utilizzare il metodo [`L.marker()`](https://leafletjs.com/reference.html#marker) (che sembra essere documentato nei relativi documenti di Leaflet.js). Aggiunga il seguente codice al suo esempio, ancora una volta all'interno di `window.onload`:

```js
L.marker([53.480759, -2.242631], {
  icon: L.mapquest.icons.marker({
    primaryColor: "#22407F",
    secondaryColor: "#3B5998",
    shadow: true,
    size: "md",
    symbol: "A",
  }),
})
  .bindPopup("This is Manchester!")
  .addTo(map);
```

Come può vedere, questo al suo livello più semplice prende due parametri, un array contenente le coordinate in cui visualizzare il marcatore, e un oggetto opzioni contenente una proprietà `icon` che definisce l'icona da visualizzare in quel punto.

L'icona è definita utilizzando un metodo [`mapquest.icons.marker()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-icons/), che come può vedere contiene informazioni come il colore e la dimensione del marcatore.

Alla fine della prima chiamata al metodo collega `.bindPopup('This is Manchester!')`, che definisce il contenuto da visualizzare quando il marcatore viene cliccato.

Infine, collega `.addTo(map)` alla fine della catena per aggiungere effettivamente il marcatore alla mappa.

Si diverta con le altre opzioni mostrate nella documentazione per vedere cosa riesce a creare! Mapquest fornisce alcune funzionalità avanzate, come indicazioni, ricerca, ecc.

> [!NOTE]
> Se ha difficoltà a far funzionare l'esempio, controlli il suo codice con la nostra [versione finale](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/mapquest/finished/script.js).

## Un'API RESTful — NYTimes

Ora esaminiamo un altro esempio di API — l'[API del New York Times](https://developer.nytimes.com/). Questa API consente di recuperare le informazioni sulle notizie del New York Times e visualizzarle sul sito. Questo tipo di API è conosciuto come **API RESTful** — invece di ottenere dati utilizzando le funzionalità di una libreria JavaScript come abbiamo fatto con Mapquest, otteniamo dati effettuando richieste HTTP a URL specifici, con dati come termini di ricerca e altre proprietà codificate nell'URL (spesso come parametri URL). Questo è un modello comune che incontrerà con le API.

Di seguito la guideremo attraverso un esercizio per mostrarle come utilizzare l'API del NYTimes, che fornisce anche un insieme più generale di passaggi da seguire che può utilizzare come approccio per lavorare con nuove API.

### Trovare la documentazione

Quando desidera utilizzare un'API di terze parti, è essenziale sapere dove si trova la documentazione, in modo da poter scoprire quali caratteristiche ha l'API, come utilizzarle, ecc. La documentazione dell'API del New York Times si trova all'indirizzo <https://developer.nytimes.com/>.

### Ottenere una chiave sviluppatore

La maggior parte delle API richiede di utilizzare un tipo di chiave sviluppatore, per ragioni di sicurezza e responsabilità. Per iscriversi per ottenere una chiave API del NYTimes, segua le istruzioni all'indirizzo <https://developer.nytimes.com/get-started>.

1. Richieda una chiave per l'API di Ricerca Articoli — crei una nuova app, selezionando questa come API che desidera utilizzare (compili un nome e una descrizione, attivi l'interruttore sotto l'"API di Ricerca Articoli" e quindi clicchi su "Create").
2. Ottenga la chiave API dalla pagina risultante.
3. Ora, per avviare l'esempio, faccia una copia di tutti i file nella directory [nytimes/start](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/nytimes/start). Se ha già clonato il [repository degli esempi](https://github.com/mdn/learning-area), avrà già una copia di questi file, che può trovare nella directory _javascript/apis/third-party-apis/nytimes/start_. Inizialmente il file `script.js` contiene una serie di variabili necessarie per l'impostazione dell'esempio; di seguito riempiremo la funzionalità richiesta.

L'app consentirà di digitare un termine di ricerca e date di inizio e fine opzionali, che utilizzerà per interrogare l'API di Ricerca Articoli e visualizzare i risultati della ricerca.

![Una schermata di una query di ricerca campione e risultati di ricerca come recuperati dall'API di Ricerca Articoli del New York Times.](nytimes-example.png)

### Collegare l'API alla sua app

Innanzitutto, sarà necessario stabilire una connessione tra l'API e la sua app. Nel caso di questa API, deve includere la chiave API come parametro [get](/it/docs/Web/HTTP/Reference/Methods/GET) ogni volta che richiede dati dal servizio all'URL corretto.

1. Trovi la seguente riga:

   ```js
   const key = "INSERT-YOUR-API-KEY-HERE";
   ```

   Sostituisca la chiave API esistente con la chiave API effettiva ottenuta nella sezione precedente.

2. Aggiunga la seguente riga al suo JavaScript, sotto il commento `// Event listeners to control the functionality`. Questo esegue una funzione chiamata `submitSearch()` quando il modulo viene inviato (il pulsante viene premuto).

   ```js
   searchForm.addEventListener("submit", submitSearch);
   ```

3. Ora aggiunga le definizioni delle funzioni `submitSearch()` e `fetchResults()`, sotto la riga precedente:

   ```js
   function submitSearch(e) {
     pageNumber = 0;
     fetchResults(e);
   }

   function fetchResults(e) {
     // Use preventDefault() to stop the form submitting
     e.preventDefault();

     // Assemble the full URL
     let url = `${baseURL}?api-key=${key}&page=${pageNumber}&q=${searchTerm.value}&fq=document_type:("article")`;

     if (startDate.value !== "") {
       url = `${url}&begin_date=${startDate.value}`;
     }

     if (endDate.value !== "") {
       url = `${url}&end_date=${endDate.value}`;
     }
   }
   ```

`submitSearch()` imposta il numero della pagina su 0 all'inizio e poi chiama `fetchResults()`. Questa prima chiama [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento, per impedire il vero invio del modulo (che interromperebbe l'esempio). Successivamente, usiamo alcune manipolazioni di stringhe per assemblare l'URL completo su cui effettueremo la richiesta. Iniziamo assemblando le parti che riteniamo obbligatorie per questa demo:

- L'URL di base (preso dalla variabile `baseURL`).
- La chiave API, che deve essere specificata nel parametro URL `api-key` (il valore è preso dalla variabile `key`).
- Il numero di pagina, che deve essere specificato nel parametro URL `page` (il valore è preso dalla variabile `pageNumber`).
- Il termine di ricerca, che deve essere specificato nel parametro URL `q` (il valore è preso dal valore dell'{{htmlelement("input")}} di testo `searchTerm`).
- Il tipo di documento per cui restituire i risultati, come specificato in un'espressione passata tramite il parametro URL `fq`. In questo caso, vogliamo restituire articoli.

Successivamente, usiamo un paio di istruzioni [`if ()`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per controllare se gli elementi `startDate` e `endDate` sono stati compilati. Se lo sono, aggiungiamo i loro valori all'URL, specificato rispettivamente nei parametri URL `begin_date` e `end_date`.

Quindi, un URL completo finirebbe per sembrare qualcosa di simile a questo:

```url
https://api.nytimes.com/svc/search/v2/articlesearch.json?api-key=YOUR-API-KEY-HERE&page=0&q=cats&fq=document_type:("article")&begin_date=20170301&end_date=20170312
```

> [!NOTE]
> Può trovare più dettagli su quali parametri URL possono essere inclusi presso i [documenti per sviluppatori NYTimes](https://developer.nytimes.com/).

> [!NOTE]
> L'esempio ha una convalida rudimentale dei dati del modulo — il campo del termine di ricerca deve essere compilato prima che il modulo possa essere inviato (ottenuto utilizzando l'attributo `required`), e i campi data hanno attributi `pattern` specificati, il che significa che non invieranno a meno che i loro valori non consistano di 8 numeri (`pattern="[0-9]{8}"`). Veda [Form data validation](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) per maggiori dettagli su come funzionano.

### Richiedere dati dall'API

Ora che abbiamo costruito il nostro URL, effettuiamo una richiesta a esso. Lo faremo utilizzando l'[API Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch).

Aggiunga il seguente blocco di codice all'interno della funzione `fetchResults()`, appena sopra la graffa di chiusura:

```js
// Use fetch() to make the request to the API
fetch(url)
  .then((response) => response.json())
  .then((json) => displayResults(json))
  .catch((error) => console.error(`Error fetching data: ${error.message}`));
```

Qui eseguiamo la richiesta passando la nostra variabile `url` a [`fetch()`](/it/docs/Web/API/Window/fetch), convertiamo il corpo della risposta in JSON utilizzando la funzione [`json()`](/it/docs/Web/API/Response/json), quindi passiamo il JSON risultante alla funzione `displayResults()` in modo che i dati possano essere visualizzati nella nostra UI. Rileviamo anche e registriamo eventuali errori che potrebbero essere generati.

### Visualizzare i dati

Va bene, vediamo come visualizzeremo i dati. Aggiunga la seguente funzione sotto la funzione `fetchResults()`.

```js
function displayResults(json) {
  while (section.firstChild) {
    section.removeChild(section.firstChild);
  }

  const articles = json.response.docs;

  nav.style.display = articles.length === 10 ? "block" : "none";

  if (articles.length === 0) {
    const para = document.createElement("p");
    para.textContent = "No results returned.";
    section.appendChild(para);
  } else {
    for (const current of articles) {
      const article = document.createElement("article");
      const heading = document.createElement("h2");
      const link = document.createElement("a");
      const img = document.createElement("img");
      const para1 = document.createElement("p");
      const keywordPara = document.createElement("p");
      keywordPara.classList.add("keywords");

      console.log(current);

      link.href = current.web_url;
      link.textContent = current.headline.main;
      para1.textContent = current.snippet;
      keywordPara.textContent = "Keywords: ";
      for (const keyword of current.keywords) {
        const span = document.createElement("span");
        span.textContent = `${keyword.value} `;
        keywordPara.appendChild(span);
      }

      if (current.multimedia.length > 0) {
        img.src = `http://www.nytimes.com/${current.multimedia[0].url}`;
        img.alt = current.headline.main;
      }

      article.appendChild(heading);
      heading.appendChild(link);
      article.appendChild(img);
      article.appendChild(para1);
      article.appendChild(keywordPara);
      section.appendChild(article);
    }
  }
}
```

C'è un sacco di codice qui; spieghiamolo passo dopo passo:

- Il ciclo [`while`](/it/docs/Web/JavaScript/Reference/Statements/while) è un modello comune utilizzato per eliminare tutti i contenuti di un elemento DOM, in questo caso, l'elemento {{htmlelement("section")}}. Continuiamo a controllare se `<section>` ha un primo figlio, e se lo ha, rimuoviamo il primo figlio. Il ciclo termina quando `<section>` non ha più figli.
- Successivamente, impostiamo la variabile `articles` per essere uguale a `json.response.docs` — questo è l'array che contiene tutti gli oggetti che rappresentano gli articoli restituiti dalla ricerca. Questo è fatto puramente per rendere il codice seguente un po' più semplice.
- Il primo blocco [`if ()`](/it/docs/Web/JavaScript/Reference/Statements/if...else) controlla se vengono restituiti 10 articoli (l'API restituisce fino a 10 articoli alla volta). Se sì, mostriamo la {{htmlelement("nav")}} che contiene i pulsanti di paginazione _Precedenti 10_/_Successivi 10_. Se vengono restituiti meno di 10 articoli, si adatteranno tutti in una pagina, quindi non abbiamo bisogno di mostrare i pulsanti di paginazione. Cableremo la funzionalità di paginazione nella prossima sezione.
- Il blocco `if ()` successivo controlla se non vengono restituiti articoli. Se sì, non proviamo a visualizzare nessuno — creiamo un {{htmlelement("p")}} contenente il testo "No results returned." e lo inseriamo nel `<section>`.
- Se alcuni articoli vengono restituiti, iniziamo innanzitutto a creare tutti gli elementi che vogliamo utilizzare per visualizzare ogni storia di notizie, inserire i contenuti giusti in ciascuno di essi e poi inserirli nel DOM nei luoghi appropriati. Per capire quali proprietà negli oggetti articolo contenessero i dati giusti da mostrare, abbiamo consultato il riferimento dell'API di Ricerca Articoli (vedi [NYTimes APIs](https://developer.nytimes.com/apis)). La maggior parte di queste operazioni sono abbastanza evidenti, ma alcune meritano di essere evidenziate:

  - Abbiamo utilizzato un ciclo [`for...of`](/it/docs/Web/JavaScript/Reference/Statements/for...of) per attraversare tutte le parole chiave associate a ciascun articolo e inserire ciascuna nel proprio {{htmlelement("span")}}, all'interno di un `<p>`. Questo è stato fatto per facilitare la stilizzazione di ciascuna.
  - Abbiamo usato un blocco `if ()` (`if (current.multimedia.length > 0) { }`) per controllare se ciascun articolo ha immagini associate, poiché alcune storie no. Visualizziamo la prima immagine solo se esiste; altrimenti, verrebbe generato un errore.

### Cablaggio dei pulsanti di paginazione

Per far funzionare i pulsanti di paginazione, incrementeremo (o decrementeremo) il valore della variabile `pageNumber`, e poi rieseguiremo la richiesta di fetch con il nuovo valore incluso nel parametro URL della pagina. Questo funziona perché l'API di NYTimes restituisce solo 10 risultati alla volta — se sono disponibili più di 10 risultati, restituirà i primi 10 (0-9) se il parametro URL `page` è impostato a 0 (o non incluso — 0 è il valore predefinito), i successivi 10 (10-19) se è impostato a 1, e così via.

Questo ci consente di scrivere una funzione di paginazione semplificata.

1. Sotto l'attuale chiamata [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener), aggiunga questi due nuovi, che fanno sì che vengano invocate le funzioni `nextPage()` e `previousPage()` quando vengono cliccati i relativi pulsanti:

   ```js
   nextBtn.addEventListener("click", nextPage);
   previousBtn.addEventListener("click", previousPage);
   ```

2. Sotto la sua aggiunta precedente, definiamo le due funzioni — aggiunga questo codice ora:

   ```js
   function nextPage(e) {
     pageNumber++;
     fetchResults(e);
   }

   function previousPage(e) {
     if (pageNumber > 0) {
       pageNumber--;
     } else {
       return;
     }
     fetchResults(e);
   }
   ```

   La prima funzione incrementa la variabile `pageNumber`, quindi esegue di nuovo la funzione `fetchResults()` per visualizzare i risultati della pagina successiva.

   La seconda funzione funziona quasi esattamente allo stesso modo al contrario, ma dobbiamo anche prendere la misura extra di controllare che `pageNumber` non sia già zero prima di decrementarlo — se la richiesta di recupero viene eseguita con un parametro URL `page` negativo, potrebbe causare errori. Se `pageNumber` è già 0, usiamo [`return`](/it/docs/Web/JavaScript/Reference/Statements/return) per uscire dalla funzione — se siamo già alla prima pagina, non abbiamo bisogno di caricare nuovamente gli stessi risultati.

> [!NOTE]
> Può trovare il nostro [codice di esempio finale dell'API NYTimes su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/nytimes/finished/index.html) (anche [guardarlo in esecuzione dal vivo qui](https://mdn.github.io/learning-area/javascript/apis/third-party-apis/nytimes/finished/)).

## Esempio YouTube

Abbiamo anche creato un altro esempio per lei da studiare e imparare — veda il nostro [esempio di ricerca video su YouTube](https://mdn.github.io/learning-area/javascript/apis/third-party-apis/youtube/). Questo utilizza due API correlate:

- L'[API dei dati di YouTube](https://developers.google.com/youtube/v3/docs/) per cercare video di YouTube e restituire risultati.
- L'[API YouTube IFrame Player](https://developers.google.com/youtube/iframe_api_reference) per visualizzare i video restituiti all'interno di lettori video IFrame in modo da poterli guardare.

Questo esempio è interessante perché mostra due API di terze parti correlate utilizzate insieme per costruire un'app. La prima è un'API RESTful, mentre la seconda funziona più come Mapquest (con metodi specifici per l'API, ecc.). Vale la pena notare tuttavia che entrambe le API richiedono che una libreria JavaScript sia applicata alla pagina. L'API RESTful ha funzioni disponibili per gestire la creazione delle richieste HTTP e il ritorno dei risultati.

![Una schermata di una ricerca video su YouTube campione utilizzando due API correlate. Il lato sinistro dell'immagine ha una query di ricerca campione utilizzando l'API dei dati di YouTube. Il lato destro dell'immagine visualizza i risultati della ricerca utilizzando l'API YouTube Iframe Player.](youtube-example.png)

Non diremo molto di più su questo esempio nell'articolo — [il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/youtube) ha commenti dettagliati inseriti al suo interno per spiegare come funziona.

Per farlo funzionare, occorre:

- Leggere la [Panoramica dell'API dei dati di YouTube](https://developers.google.com/youtube/v3/getting-started).
- Assicurarsi di visitare la [pagina delle API abilitate](https://console.cloud.google.com/apis/enabled), e nell'elenco delle API, assicurarsi che lo stato sia su ON per l'API dei dati di YouTube v3.
- Ottenere una chiave API da [Google Cloud](https://cloud.google.com/).
- Trovare la stringa `ENTER-API-KEY-HERE` nel codice sorgente e sostituirla con la sua chiave API.
- Eseguire l'esempio tramite un server web. Non funzionerà se lo esegue direttamente nel browser (cioè, tramite un URL `file://`).

## Sommario

Questo articolo le ha fornito un'introduzione utile all'utilizzo delle API di terze parti per aggiungere funzionalità ai suoi siti web.

{{PreviousMenu("Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}
