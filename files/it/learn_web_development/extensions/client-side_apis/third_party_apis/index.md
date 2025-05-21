---
title: API di terze parti
slug: Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}

Le API che abbiamo trattato finora sono integrate nel browser, ma non tutte le API lo sono. Molti grandi siti web e servizi come Google Maps, Twitter, Facebook, PayPal, ecc. forniscono API che consentono agli sviluppatori di utilizzare i loro dati (ad esempio, visualizzare il tuo stream di Twitter sul tuo blog) o servizi (ad esempio, utilizzare il login di Facebook per accedere ai tuoi utenti). Questo articolo esamina la differenza tra le API del browser e le API di terze parti e mostra alcuni usi tipici di queste ultime.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare le basi degli <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">oggetti JavaScript</a> e la copertura delle API core come il <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e le <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti dietro le API di terze parti e i modelli associati come le chiavi API.</li>
          <li>Usare un'API di mappe di terze parti.</li>
          <li>Usare un'API RESTful.</li>
          <li>Usare le API di YouTube di Google.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono le API di terze parti?

Le API di terze parti sono API fornite da terze parti — generalmente aziende come Facebook, Twitter o Google — per permetterti di accedere alla loro funzionalità tramite JavaScript e usarla sul tuo sito. Uno degli esempi più evidenti è l'uso delle API di mappatura per visualizzare mappe personalizzate sulle tue pagine.

Osserviamo un [semplice esempio di API Mapquest](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/mapquest), e utilizziamolo per illustrare come le API di terze parti differiscono dalle API del browser.

### Si trovano su server di terze parti

Le API del browser sono integrate nel browser — puoi accedervi immediatamente da JavaScript. Per esempio, l'API Web Audio che [abbiamo visto nell'articolo introduttivo](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#how_do_apis_work) è accessibile usando l'oggetto nativo [`AudioContext`](/it/docs/Web/API/AudioContext). Per esempio:

```js
const audioCtx = new AudioContext();
// …
const audioElement = document.querySelector("audio");
// …
const audioSource = audioCtx.createMediaElementSource(audioElement);
// etc.
```

Le API di terze parti, invece, si trovano su server di terze parti. Per accedervi da JavaScript, devi prima connetterti alla funzionalità dell'API e renderla disponibile sulla tua pagina. Tipicamente, questo comporta prima di tutto il collegamento a una libreria JavaScript disponibile sul server tramite un elemento {{htmlelement("script")}}, come visto nel nostro esempio Mapquest:

```html
<script
  src="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.js"
  defer></script>
<link
  rel="stylesheet"
  href="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.css" />
```

Puoi quindi iniziare a utilizzare gli oggetti disponibili in quella libreria. Per esempio:

```js
const map = L.mapquest.map("map", {
  center: [53.480759, -2.242631],
  layers: L.mapquest.tileLayer("map"),
  zoom: 12,
});
```

Qui stiamo creando una variabile per memorizzare le informazioni della mappa, quindi creiamo una nuova mappa usando il metodo `mapquest.map()`, che prende come parametri l'ID di un elemento {{htmlelement("div")}} in cui vuoi visualizzare la mappa ('map'), e un oggetto opzioni contenente i dettagli della mappa specifica che vogliamo visualizzare. In questo caso, specifichiamo le coordinate del centro della mappa, uno strato di mappa di tipo `map` da mostrare (creato usando il metodo `mapquest.tileLayer()`), e il livello di zoom predefinito.

Queste sono tutte le informazioni che l'API Mapquest necessita per tracciare una mappa semplice. Il server a cui ti stai connettendo gestisce tutte le cose complicate, come mostrare i riquadri della mappa corretti per l'area visualizzata, ecc.

> [!NOTE]
> Alcune API gestiscono l'accesso alla loro funzionalità in modo leggermente diverso, richiedendo allo sviluppatore di effettuare una richiesta HTTP a un modello di URL specifico per recuperare i dati. Queste sono chiamate [API RESTful — mostreremo un esempio più avanti](#a_restful_api_%e2%80%94_nytimes).

### Di solito richiedono chiavi API

La sicurezza per le API del browser tende a essere gestita da richieste di autorizzazione, come [discusso nel nostro primo articolo](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#they_have_additional_security_mechanisms_where_appropriate). Lo scopo è che l'utente sappia cosa sta succedendo nei siti che visita ed è meno probabile che cada vittima di qualcuno che usa un'API in modo dannoso.

Le API di terze parti hanno un sistema di autorizzazioni leggermente diverso — tendono a usare chiavi per sviluppatori per consentire agli sviluppatori di accedere alla funzionalità dell'API, il che è più per proteggere il fornitore dell'API che l'utente.

Troverai una linea simile alla seguente nell'esempio dell'API Mapquest:

```js
L.mapquest.key = "YOUR-API-KEY-HERE";
```

Questa riga specifica una chiave per sviluppatori o API da usare nella tua applicazione — lo sviluppatore dell'applicazione deve richiedere una chiave, e poi includerla nel proprio codice per essere autorizzato ad accedere alla funzionalità dell'API. Nel nostro esempio, abbiamo semplicemente fornito un segnaposto.

> [!NOTE]
> Quando crei i tuoi esempi, utilizzerai la tua chiave API al posto di un qualsiasi segnaposto.

Altre API possono richiedere che tu includa la chiave in modo leggermente diverso, ma il modello è relativamente simile per la maggior parte di esse.

Richiedere una chiave permette al fornitore dell'API di ritenere responsabili gli utenti dell'API per le loro azioni. Quando lo sviluppatore si è registrato per una chiave, è noto al fornitore dell'API, e si possono intraprendere azioni se iniziano a fare qualcosa di dannoso con l'API (come tracciare la posizione delle persone o cercare di inviare all'API un'enorme quantità di richieste per bloccarla, per esempio). L'azione più semplice sarebbe semplicemente revocare i privilegi dell'API.

## Estendere l'esempio Mapquest

Aggiungiamo un po' più di funzionalità all'esempio Mapquest per mostrare come utilizzare alcune altre caratteristiche dell'API.

1. Per iniziare questa sezione, fai una copia del [file di partenza di mapquest](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/mapquest/start/index.html), in una nuova directory. Se hai già clonato il [repository degli esempi](https://github.com/mdn/learning-area), avrai già una copia di questo file, che puoi trovare nella directory _javascript/apis/third-party-apis/mapquest/start_.
2. Poi, devi andare sul [sito degli sviluppatori di Mapquest](https://developer.mapquest.com/), creare un account e quindi creare una chiave per sviluppatori da usare con il tuo esempio. (Al momento della scrittura, veniva chiamata "consumer key" sul sito, e il processo di creazione della chiave chiedeva anche un "callback URL" opzionale. Non è necessario inserire un URL qui: basta lasciarlo vuoto.)
3. Apri il tuo file iniziale e sostituisci il segnaposto della chiave API con la tua chiave.

### Cambiare il tipo di mappa

Ci sono diversi tipi di mappa che possono essere mostrati con l'API Mapquest. Per farlo, trova la seguente linea:

```js
layers: L.mapquest.tileLayer("map");
```

Prova a cambiare `'map'` in `'hybrid'` per mostrare una mappa in stile ibrido. Prova anche altri valori. La [pagina di riferimento `tileLayer`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-tile-layer/) mostra le diverse opzioni disponibili, oltre a molte più informazioni.

### Aggiungere diversi controlli

La mappa ha una serie di controlli disponibili; per impostazione predefinita mostra solo un controllo di zoom. Puoi espandere i controlli disponibili usando il metodo `map.addControl()`; aggiungi questo al tuo codice:

```js
map.addControl(L.mapquest.control());
```

Il [`mapquest.control()` method](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-control/) crea semplicemente un set di controlli completo e di default viene posizionato nell'angolo in alto a destra. Puoi regolare la posizione specificando un oggetto di opzioni come parametro per il controllo contenente una proprietà `position`, il cui valore è una stringa che specifica una posizione per il controllo. Prova questo, per esempio:

```js
map.addControl(L.mapquest.control({ position: "bottomright" }));
```

Ci sono altri tipi di controlli disponibili, per esempio [`mapquest.searchControl()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-search-control/) e [`mapquest.satelliteControl()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-satellite-control/), e alcuni sono piuttosto complessi e potenti. Prova a smanettare e vedi cosa riesci a inventare.

### Aggiungere un marcatore personalizzato

Aggiungere un marcatore (icona) in un punto specifico sulla mappa è facile — basta usare il metodo [`L.marker()`](https://leafletjs.com/reference.html#marker) (che sembra essere documentato nei documenti correlati di Leaflet.js). Aggiungi il seguente codice al tuo esempio, di nuovo all'interno di `window.onload`:

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

Come puoi vedere, nella sua forma più semplice accetta due parametri, un array contenente le coordinate a cui visualizzare il marcatore, e un oggetto opzioni contenente una proprietà `icon` che definisce l'icona da visualizzare in quel punto.

L'icona è definita usando un metodo [`mapquest.icons.marker()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-icons/), che come puoi vedere contiene informazioni come colore e dimensione del marcatore.

Alla fine della prima chiamata del metodo concatenamo `.bindPopup('This is Manchester!')`, che definisce il contenuto da visualizzare quando il marcatore viene cliccato.

Infine, concatenamo `.addTo(map)` alla fine della catena per effettivamente aggiungere il marcatore alla mappa.

Prova a smanettare con le altre opzioni mostrate nella documentazione e vedi cosa riesci a inventare! Mapquest offre alcune funzionalità piuttosto avanzate, come indicazioni stradali, ricerche, ecc.

> [!NOTE]
> Se hai difficoltà a far funzionare l'esempio, verifica il tuo codice con la nostra [versione finale](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/mapquest/finished/script.js).

## Un'API RESTful — NYTimes

Ora osserviamo un altro esempio di API — l'[API del New York Times](https://developer.nytimes.com/). Questa API ti permette di recuperare le informazioni sulle notizie del New York Times e di visualizzarle sul tuo sito. Questo tipo di API è conosciuto come API **RESTful** — invece di ottenere dati usando le funzionalità di una libreria JavaScript come abbiamo fatto con Mapquest, otteniamo dati effettuando richieste HTTP a URL specifici, con dati come termini di ricerca e altre proprietà codificate nell'URL (spesso come parametri URL). Questo è un modello comune che incontrerai con le API.

Di seguito ti guideremo attraverso un esercizio per mostrarti come utilizzare l'API di NYTimes, che fornisce anche un insieme di passaggi più generico da seguire che puoi usare come approccio per lavorare con nuove API.

### Trova la documentazione

Quando vuoi utilizzare un'API di terze parti, è essenziale scoprire dove si trova la documentazione, in modo da scoprire quali funzionalità ha l'API, come le usi, ecc. La documentazione dell'API del New York Times si trova a <https://developer.nytimes.com/>.

### Ottieni una chiave per sviluppatori

La maggior parte delle API richiede di usare qualche tipo di chiave per sviluppatori, per motivi di sicurezza e responsabilità. Per registrarti per una chiave API di NYTimes, segui le istruzioni su <https://developer.nytimes.com/get-started>.

1. Richiediamo una chiave per l'API Article Search — crea una nuova app, selezionando questa come API che desideri utilizzare (inserisci un nome e una descrizione, attiva l'interruttore sotto "Article Search API" e quindi clicca su "Crea").
2. Ottieni la chiave API dalla pagina risultante.
3. Ora, per iniziare l'esempio, fai una copia di tutti i file nella directory [nytimes/start](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/nytimes/start). Se hai già clonato il [repository degli esempi](https://github.com/mdn/learning-area), avrai già una copia di questi file, che puoi trovare nella directory _javascript/apis/third-party-apis/nytimes/start_. Inizialmente il file `script.js` contiene un certo numero di variabili necessarie per l'impostazione dell'esempio; di seguito riempiremo la funzionalità richiesta.

L'app finirà per permetterti di digitare un termine di ricerca e date di inizio e fine opzionali, che userà poi per interrogare l'API Article Search e visualizzare i risultati della ricerca.

![Uno screenshot di una query di ricerca di prova e dei risultati di ricerca come recuperati dall'API di ricerca articoli del New York.](nytimes-example.png)

### Collegare l'API alla tua app

Prima di tutto, dovrai creare una connessione tra l'API e la tua app. Nel caso di questa API, è necessario includere la chiave API come un parametro [get](/it/docs/Web/HTTP/Reference/Methods/GET) ogni volta che richiedi dati dal servizio all'URL corretto.

1. Trova la seguente linea:

   ```js
   const key = "INSERT-YOUR-API-KEY-HERE";
   ```

   Sostituisci la chiave API esistente con la chiave API reale che hai ottenuto nella sezione precedente.

2. Aggiungi la seguente linea al tuo JavaScript, sotto il commento `// Event listeners to control the functionality`. Questo esegue una funzione chiamata `submitSearch()` quando il modulo viene inviato (il pulsante viene premuto).

   ```js
   searchForm.addEventListener("submit", submitSearch);
   ```

3. Ora aggiungi le definizioni delle funzioni `submitSearch()` e `fetchResults()`, sotto la linea precedente:

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

`submitSearch()` imposta il numero di pagina su 0 per cominciare, quindi chiama `fetchResults()`. Questo prima chiama [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento, per fermare l'invio effettivo del modulo (che romperebbe l'esempio). Successivamente, usiamo una manipolazione di stringa per assemblare l'URL completo a cui effettueremo la richiesta. Iniziamo assemblando le parti che riteniamo obbligatorie per questa demo:

- L'URL di base (preso dalla variabile `baseURL`).
- La chiave API, che deve essere specificata nel parametro URL `api-key` (il valore è preso dalla variabile `key`).
- Il numero di pagina, che deve essere specificato nel parametro URL `page` (il valore è preso dalla variabile `pageNumber`).
- Il termine di ricerca, che deve essere specificato nel parametro URL `q` (il valore è preso dal valore del testo {{htmlelement("input")}} `searchTerm`).
- Il tipo di documento per cui restituire i risultati, come specificato in un'espressione passata tramite il parametro URL `fq`. In questo caso, vogliamo restituire articoli.

Successivamente, usiamo un paio di blocchi [`if ()`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per controllare se gli elementi `startDate` e `endDate` hanno valori riempiti su di loro. Se lo fanno, aggiungiamo i loro valori all'URL, specificato nei parametri URL `begin_date` ed `end_date` rispettivamente.

Quindi, un URL completo finirebbe per sembrare qualcosa del genere:

```url
https://api.nytimes.com/svc/search/v2/articlesearch.json?api-key=YOUR-API-KEY-HERE&page=0&q=cats&fq=document_type:("article")&begin_date=20170301&end_date=20170312
```

> [!NOTE]
> Puoi trovare maggiori dettagli sui parametri URL che possono essere inclusi nella [documentazione per sviluppatori di NYTimes](https://developer.nytimes.com/).

> [!NOTE]
> L'esempio ha una convalida rudimentale dei dati del modulo — il campo del termine di ricerca deve essere compilato prima che il modulo possa essere inviato (ottenuto usando l'attributo `required`), e i campi di data hanno attributi `pattern` specificati, il che significa che non invieranno a meno che i loro valori non consistano in 8 numeri (`pattern="[0-9]{8}"`). Vedi la [validazione dei dati del modulo](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) per maggiori dettagli su come funzionano.

### Richiedere dati dall'API

Ora che abbiamo costruito il nostro URL, facciamo una richiesta ad esso. Faremo questo utilizzando l'[API Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch).

Aggiungi il seguente blocco di codice all'interno della funzione `fetchResults()`, appena sopra la parentesi graffa di chiusura:

```js
// Use fetch() to make the request to the API
fetch(url)
  .then((response) => response.json())
  .then((json) => displayResults(json))
  .catch((error) => console.error(`Error fetching data: ${error.message}`));
```

Qui eseguiamo la richiesta passando la nostra variabile `url` a [`fetch()`](/it/docs/Web/API/Window/fetch), convertiamo il corpo della risposta in JSON utilizzando la funzione [`json()`](/it/docs/Web/API/Response/json), quindi passiamo il JSON risultante alla funzione `displayResults()` in modo che i dati possano essere visualizzati nella nostra interfaccia utente. Catturiamo e registriamo anche eventuali errori che potrebbero essere generati.

### Visualizzare i dati

OK, vediamo come visualizzeremo i dati. Aggiungi la seguente funzione sotto la tua funzione `fetchResults()`.

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

- Il ciclo [`while`](/it/docs/Web/JavaScript/Reference/Statements/while) è un pattern comune usato per cancellare tutti i contenuti di un elemento DOM, in questo caso, l'elemento {{htmlelement("section")}}. Continuiamo a controllare se la `<section>` ha un primo figlio, e se lo ha, rimuoviamo il primo figlio. Il ciclo termina quando `<section>` non ha più figli.
- Successivamente, impostiamo la variabile `articles` per essere uguale a `json.response.docs` — questo è l'array che contiene tutti gli oggetti che rappresentano gli articoli restituiti dalla ricerca. Ciò viene fatto puramente per rendere il codice successivo un po' più semplice.
- Il primo blocco [`if ()`](/it/docs/Web/JavaScript/Reference/Statements/if...else) controlla se vengono restituiti 10 articoli (l'API restituisce fino a 10 articoli alla volta). Se è così, visualizziamo il {{htmlelement("nav")}} che contiene i pulsanti di paginazione _Previous 10_/_Next 10_. Se vengono restituiti meno di 10 articoli, potranno essere visualizzati tutti in una sola pagina, quindi non abbiamo bisogno di mostrare i pulsanti di paginazione. Nel prossimo passaggio, collegheremo la funzionalità di paginazione.
- Il successivo blocco `if ()` controlla se non vengono restituiti articoli. Se è così, non cerchiamo di visualizzarne nessuno — creiamo un {{htmlelement("p")}} contenente il testo "Nessun risultato trovato." e lo inseriamo nella `<section>`.
- Se vengono restituiti alcuni articoli, iniziamo creando tutti gli elementi che vogliamo usare per visualizzare ciascuna notizia, inserendo i contenuti giusti in ciascuno di essi, e poi li inseriamo nel DOM nei punti appropriati. Per capire quali proprietà negli oggetti articolo contenevano i dati giusti da mostrare, abbiamo consultato il riferimento dell'API Article Search (vedi [NYTimes APIs](https://developer.nytimes.com/apis)). La maggior parte di queste operazioni sono piuttosto ovvie, ma alcune meritano di essere chiamate:

  - Abbiamo usato un ciclo [`for...of`](/it/docs/Web/JavaScript/Reference/Statements/for...of) per scorrere tutte le keyword associate a ciascun articolo e inserirne ognuna all'interno del proprio {{htmlelement("span")}}, all'interno di un `<p>`. Questo è stato fatto per rendere facile stilizzare ciascuna di esse.
  - Abbiamo usato un blocco `if ()` (`if (current.multimedia.length > 0) { }`) per controllare se ogni articolo ha immagini associate, poiché alcune storie non le hanno. Mostriamo solo la prima immagine se esiste; in caso contrario, verrebbe generato un errore.

### Collegamento dei pulsanti di paginazione

Per far funzionare i pulsanti di paginazione, incrementeremo (o decrementeremo) il valore della variabile `pageNumber` e poi rierogheremo la richiesta fetch con il nuovo valore incluso nel parametro URL della pagina. Questo funziona perché l'API di NYTimes restituisce solo 10 risultati alla volta — se ci sono più di 10 risultati disponibili, restituirà i primi 10 (0-9) se il parametro URL `page` è impostato su 0 (o non incluso affatto — 0 è il valore predefinito), i successivi 10 (10-19) se è impostato su 1, e così via.

Ciò ci consente di scrivere una funzione di paginazione semplificata.

1. Sotto la chiamata esistente [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener), aggiungi questi due nuovi, che fanno sì che le funzioni `nextPage()` e `previousPage()` vengano invocate quando i pulsanti pertinenti vengono cliccati:

   ```js
   nextBtn.addEventListener("click", nextPage);
   previousBtn.addEventListener("click", previousPage);
   ```

2. Sotto il tuo precedente aggiunta, definiamo le due funzioni — aggiungi questo codice ora:

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

   La seconda funzione funziona quasi allo stesso modo al contrario, ma dobbiamo anche intraprendere il passo aggiuntivo di controllare che `pageNumber` non sia già zero prima di decrementarlo — se la richiesta fetch viene eseguita con un parametro URL `page` negativo, potrebbe causare errori. Se `pageNumber` è già 0, usciamo dalla funzione con il comando [`return`](/it/docs/Web/JavaScript/Reference/Statements/return) — se siamo già alla prima pagina, non abbiamo bisogno di caricare nuovamente gli stessi risultati.

> [!NOTE]
> Puoi trovare il nostro [codice di esempio finale dell'API di NYTimes su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/nytimes/finished/index.html) (anche [vedilo funzionare in diretta qui](https://mdn.github.io/learning-area/javascript/apis/third-party-apis/nytimes/finished/)).

## Esempio di YouTube

Abbiamo anche costruito un altro esempio per te da studiare e da cui imparare — vedi il nostro [esempio di ricerca video su YouTube](https://mdn.github.io/learning-area/javascript/apis/third-party-apis/youtube/). Questo utilizza due API correlate:

- L'[API YouTube Data](https://developers.google.com/youtube/v3/docs/) per cercare video su YouTube e restituire risultati.
- L'[API YouTube IFrame Player](https://developers.google.com/youtube/iframe_api_reference) per visualizzare gli esempi di video restituiti all'interno di lettori video IFrame in modo che tu possa guardarli.

Questo esempio è interessante perché mostra due API di terze parti correlate utilizzate insieme per costruire un'app. La prima è un'API RESTful, mentre la seconda funziona più come Mapquest (con metodi specifici dell'API, ecc.). Vale la pena notare comunque che entrambe le API richiedono che una libreria JavaScript sia applicata alla pagina. L'API RESTful ha funzioni disponibili per gestire l'effettuazione delle richieste HTTP e la restituzione dei risultati.

![Uno screenshot di una ricerca video di prova su YouTube usando due API correlate. La parte sinistra dell'immagine ha una query di ricerca di prova usando l'API YouTube Data. La parte destra dell'immagine visualizza i risultati della ricerca usando l'API YouTube Iframe Player.](youtube-example.png)

Non diremo molto di più su questo esempio nell'articolo — nel [codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/youtube) sono stati inseriti commenti dettagliati per spiegare come funziona.

Per farlo funzionare, dovrai:

- Leggere la [documentazione sull'introduzione all'API YouTube Data](https://developers.google.com/youtube/v3/getting-started).
- Assicurarti di visitare la [pagina API abilitate](https://console.cloud.google.com/apis/enabled), e nell'elenco delle API, assicurarti che lo stato sia ATTIVO per YouTube Data API v3.
- Ottenere una chiave API da [Google Cloud](https://cloud.google.com/).
- Trova la stringa `ENTER-API-KEY-HERE` nel codice sorgente e sostituiscila con la tua chiave API.
- Esegui l'esempio tramite un server web. Non funzionerà se lo esegui direttamente nel browser (cioè, tramite un URL `file://`).

## Sommario

Questo articolo ti ha fornito un'introduzione utile all'utilizzo delle API di terze parti per aggiungere funzionalità ai tuoi siti web.

{{PreviousMenu("Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}
