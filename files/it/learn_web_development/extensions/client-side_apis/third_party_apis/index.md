---
title: API di terze parti
slug: Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenu("Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}

Le API che abbiamo coperto finora sono integrate nel browser, ma non tutte le API lo sono. Molti grandi siti web e servizi come Google Maps, Twitter, Facebook, PayPal, ecc. forniscono API che permettono agli sviluppatori di utilizzare i loro dati (ad esempio, visualizzare il tuo stream Twitter sul tuo blog) o servizi (ad esempio, utilizzare l'accesso Facebook per effettuare il login dei tuoi utenti). Questo articolo esamina la differenza tra le API dei browser e le API di terze parti e mostra alcuni usi tipici di queste ultime.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare le <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">basi degli oggetti JavaScript</a> e la copertura delle API di base come <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">DOM scripting</a> e <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti dietro le API di terze parti e i pattern associati come le chiavi API.</li>
          <li>Utilizzo di un'API di mappe di terze parti.</li>
          <li>Utilizzo di un'API RESTful.</li>
          <li>Utilizzo delle API di YouTube di Google.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono le API di terze parti?

Le API di terze parti sono API fornite da terze parti — generalmente aziende come Facebook, Twitter o Google — per consentire di accedere alla loro funzionalità tramite JavaScript e usarla sul tuo sito. Uno degli esempi più ovvi è l'uso delle API di mappatura per visualizzare mappe personalizzate sulle tue pagine.

Diamo un'occhiata a un [semplice esempio di API Mapquest](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/mapquest) e usiamolo per illustrare come le API di terze parti si differenziano dalle API dei browser.

### Si trovano su server di terze parti

Le API del browser sono integrate nel browser — è possibile accedervi da JavaScript immediatamente. Ad esempio, l'API Web Audio che abbiamo [visto nell'articolo introduttivo](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#how_do_apis_work) è accessibile usando l'oggetto nativo [`AudioContext`](/it/docs/Web/API/AudioContext). Ad esempio:

```js
const audioCtx = new AudioContext();
// …
const audioElement = document.querySelector("audio");
// …
const audioSource = audioCtx.createMediaElementSource(audioElement);
// etc.
```

Le API di terze parti, invece, si trovano su server di terze parti. Per accedervi da JavaScript è necessario prima connettersi alla funzionalità API e renderla disponibile sulla tua pagina. Questo tipicamente comporta il primo collegamento a una libreria JavaScript disponibile sul server tramite un elemento {{htmlelement("script")}}, come visto nel nostro esempio Mapquest:

```html
<script
  src="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.js"
  defer></script>
<link
  rel="stylesheet"
  href="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.css" />
```

Puoi quindi iniziare a utilizzare gli oggetti disponibili in quella libreria. Ad esempio:

```js
const map = L.mapquest.map("map", {
  center: [53.480759, -2.242631],
  layers: L.mapquest.tileLayer("map"),
  zoom: 12,
});
```

Qui stiamo creando una variabile per memorizzare le informazioni della mappa, quindi creando una nuova mappa utilizzando il metodo `mapquest.map()`, che prende come parametri l'ID di un elemento {{htmlelement("div")}} in cui vuoi visualizzare la mappa ('map'), e un oggetto opzioni contenente i dettagli della mappa particolare che vuoi visualizzare. In questo caso specifichiamo le coordinate del centro della mappa, uno strato di mappa di tipo `map` da mostrare (creato usando il metodo `mapquest.tileLayer()`), e il livello di zoom predefinito.

Queste sono tutte le informazioni di cui l'API Mapquest ha bisogno per tracciare una semplice mappa. Il server a cui ti stai collegando si occupa di tutte le cose complicate, come visualizzare i riquadri della mappa corretti per l'area mostrata, ecc.

> [!NOTE]
> Alcune API gestiscono l'accesso alla loro funzionalità in modo leggermente diverso, richiedendo allo sviluppatore di fare una richiesta HTTP a un modello di URL specifico per recuperare i dati. Queste sono chiamate [API RESTful — mostreremo un esempio più avanti](#a_restful_api_%e2%80%94_nytimes).

### Richiedono solitamente chiavi API

La sicurezza per le API del browser tende a essere gestita attraverso richieste di autorizzazione, come [discusso nel nostro primo articolo](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#they_have_additional_security_mechanisms_where_appropriate). Lo scopo di queste è far sapere all'utente cosa sta succedendo nei siti web che visita ed è meno probabile che cada vittima di qualcuno che utilizza un'API in modo malevolo.

Le API di terze parti hanno un sistema di permessi leggermente diverso — tendono a utilizzare chiavi sviluppatore per consentire agli sviluppatori l'accesso alla funzionalità API, il che è più per proteggere il fornitore dell'API che l'utente.

Troverai una riga simile alla seguente nell'esempio API Mapquest:

```js
L.mapquest.key = "YOUR-API-KEY-HERE";
```

Questa riga specifica una chiave API o sviluppatore da utilizzare nella tua applicazione — lo sviluppatore dell'applicazione deve fare richiesta per ottenere una chiave e quindi includerla nel loro codice per essere autorizzato ad accedere alla funzionalità dell'API. Nel nostro esempio, abbiamo fornito solo un placeholder.

> [!NOTE]
> Quando crei i tuoi esempi, userai la tua chiave API al posto di qualsiasi placeholder.

Altre API potrebbero richiedere di includere la chiave in un modo leggermente diverso, ma il pattern è relativamente simile per la maggior parte di esse.

Richiedere una chiave consente al fornitore dell'API di ritenere responsabili gli utenti dell'API per le loro azioni. Quando lo sviluppatore si è registrato per una chiave, è quindi noto al fornitore dell'API, e possono essere intraprese azioni se iniziano a fare qualsiasi cosa malevola con l'API (come tracciare la posizione delle persone o provare a spammare l'API con un sacco di richieste per fermarla). L'azione più semplice sarebbe semplicemente revocare i loro privilegi di API.

## Estendere l'esempio Mapquest

Aggiungiamo un po' più di funzionalità all'esempio Mapquest per mostrare come utilizzare alcune altre funzionalità dell'API.

1. Per iniziare questa sezione, fai una copia del [file starter di Mapquest](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/mapquest/start/index.html), in una nuova directory. Se hai già clonato il [repository degli esempi](https://github.com/mdn/learning-area), avrai già una copia di questo file, che puoi trovare nella directory _javascript/apis/third-party-apis/mapquest/start_.
2. Successivamente, devi andare al [sito sviluppatori di Mapquest](https://developer.mapquest.com/), creare un account e quindi creare una chiave sviluppatore da utilizzare con il tuo esempio. (Al momento della scrittura, veniva chiamata "chiave consumer" sul sito, e il processo di creazione della chiave chiedeva anche un "URL di callback" opzionale. Non è necessario inserire un URL qui: lascialo semplicemente vuoto.)
3. Apri il tuo file iniziale e sostituisci il placeholder della chiave API con la tua chiave.

### Cambiare il tipo di mappa

Ci sono diversi tipi di mappa che possono essere mostrati con l'API Mapquest. Per fare ciò, trova la seguente riga:

```js-nolint
layers: L.mapquest.tileLayer("map"),
```

Prova a cambiare `'map'` in `'hybrid'` per mostrare una mappa in stile ibrido. Prova anche altri valori. La [pagina di riferimento `tileLayer`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-tile-layer/) mostra le diverse opzioni disponibili, oltre a molte più informazioni.

### Aggiungere controlli diversi

La mappa ha una serie di controlli diversi disponibili; per impostazione predefinita, mostra solo un controllo di zoom. Puoi espandere i controlli disponibili usando il metodo `map.addControl()`; aggiungi questo al tuo codice:

```js
map.addControl(L.mapquest.control());
```

Il metodo [`mapquest.control()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-control/) crea un semplice set di controlli completo e viene posizionato nell'angolo in alto a destra per impostazione predefinita. Puoi regolare la posizione specificando un oggetto opzioni come parametro per il controllo contenente una proprietà `position`, il cui valore è una stringa che specifica una posizione per il controllo. Prova questo, ad esempio:

```js
map.addControl(L.mapquest.control({ position: "bottomright" }));
```

Ci sono altri tipi di controllo disponibili, come [`mapquest.searchControl()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-search-control/) e [`mapquest.satelliteControl()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-satellite-control/), e alcune sono abbastanza complesse e potenti. Divertiti a esplorare e vedere cosa puoi creare.

### Aggiungere un marcatore personalizzato

Aggiungere un marcatore (icona) in un certo punto sulla mappa è semplice: basta usare il metodo [`L.marker()`](https://leafletjs.com/reference.html#marker) (che sembra essere documentato nei documenti correlati a Leaflet.js). Aggiungi il seguente codice al tuo esempio, di nuovo all'interno di `window.onload`:

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

Come puoi vedere, questo nel modo più semplice prende due parametri, un array contenente le coordinate in cui visualizzare il marcatore, e un oggetto opzioni contenente una proprietà `icon` che definisce l'icona da visualizzare a quel punto.

L'icona è definita utilizzando un metodo [`mapquest.icons.marker()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-icons/), che come puoi vedere contiene informazioni come colore e dimensione del marcatore.

Alla fine della chiamata del primo metodo concatenato `.bindPopup('This is Manchester!')`, che definisce il contenuto da visualizzare quando il marcatore viene cliccato.

Infine, concatenamo `.addTo(map)` alla fine della catena per aggiungere effettivamente il marcatore alla mappa.

Divertiti a esplorare le altre opzioni mostrate nella documentazione e vedi cosa puoi creare! Mapquest fornisce alcune funzionalità piuttosto avanzate, come indicazioni stradali, ricerca, ecc.

> [!NOTE]
> Se hai difficoltà a far funzionare l'esempio, controlla il tuo codice con la nostra [versione finale](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/mapquest/finished/script.js).

## Un'API RESTful — NYTimes

Ora diamo un'occhiata a un altro esempio di API — l'[API del New York Times](https://developer.nytimes.com/). Questa API ti consente di recuperare le informazioni delle notizie del New York Times e visualizzarle sul tuo sito. Questo tipo di API è noto come **API RESTful** — invece di ottenere i dati utilizzando le funzionalità di una libreria JavaScript come abbiamo fatto con Mapquest, otteniamo i dati facendo richieste HTTP a URL specifici, con dati come termini di ricerca e altre proprietà codificati nell'URL (spesso come parametri URL). Questo è un pattern comune che incontrerai con le API.

Di seguito ti guideremo attraverso un esercizio per mostrarti come utilizzare l'API NYTimes, che fornisce anche una serie più generale di passaggi da seguire che puoi usare come approccio per lavorare con nuove API.

### Trova la documentazione

Quando vuoi utilizzare un'API di terze parti, è essenziale scoprire dove si trova la documentazione, in modo da poter scoprire quali funzionalità ha l'API, come usarle, ecc. La documentazione dell'API New York Times è disponibile su <https://developer.nytimes.com/>.

### Ottieni una chiave sviluppatore

La maggior parte delle API richiede di utilizzare un tipo di chiave sviluppatore, per motivi di sicurezza e responsabilità. Per registrarti per una chiave API NYTimes, segui le istruzioni su <https://developer.nytimes.com/get-started>.

1. Richiediamo una chiave per l'API di Ricerca Articoli — crea una nuova app, selezionando questa come API che vuoi usare (compila un nome e una descrizione, attiva l'interruttore sotto "Article Search API" nella posizione ON, e quindi fai clic su "Create").
2. Ottieni la chiave API dalla pagina risultante.
3. Ora, per iniziare l'esempio, fai una copia di tutti i file nella directory [nytimes/start](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/nytimes/start). Se hai già clonato il [repository degli esempi](https://github.com/mdn/learning-area), avrai già una copia di questi file, che si trovano nella directory _javascript/apis/third-party-apis/nytimes/start_. Inizialmente, il file `script.js` contiene una serie di variabili necessarie per l'impostazione dell'esempio; di seguito inseriremo la funzionalità necessaria.

L'app consentirà alla fine di digitare un termine di ricerca e date di inizio e fine opzionali, che verranno quindi usati per interrogare l'API di Ricerca Articoli e visualizzare i risultati della ricerca.

![Uno screenshot di un esempio di query di ricerca e risultati di ricerca recuperati dall'API di Ricerca Articoli del New York Times.](nytimes-example.png)

### Collegare l'API alla tua app

Per prima cosa, dovrai stabilire una connessione tra l'API e la tua app. Nel caso di questa API, è necessario includere la chiave API come un parametro [get](/it/docs/Web/HTTP/Reference/Methods/GET) ogni volta che richiedi dati dal servizio all'URL corretto.

1. Trova la seguente riga:

   ```js
   const key = "INSERT-YOUR-API-KEY-HERE";
   ```

   Sostituisci la chiave API esistente con la chiave API effettiva che hai ottenuto nella sezione precedente.

2. Aggiungi la seguente riga al tuo JavaScript, sotto il commento `// Event listeners to control the functionality`. Questo esegue una funzione chiamata `submitSearch()` quando il modulo viene inviato (il pulsante viene premuto).

   ```js
   searchForm.addEventListener("submit", submitSearch);
   ```

3. Ora aggiungi le definizioni delle funzioni `submitSearch()` e `fetchResults()`, sotto la riga precedente:

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

`submitSearch()` imposta il numero di pagina di nuovo a 0 per iniziare, quindi chiama `fetchResults()`. Questo prima chiama [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento, per evitare che il form venga effettivamente inviato (il che interromperebbe l'esempio). Successivamente, utilizziamo alcune manipolazioni delle stringhe per assemblare l'URL completo a cui effettueremo la richiesta. Iniziamo assemblando le parti che riteniamo obbligatorie per questa demo:

- L'URL di base (preso dalla variabile `baseURL`).
- La chiave API, che deve essere specificata nel parametro URL `api-key` (il valore è preso dalla variabile `key`).
- Il numero di pagina, che deve essere specificato nel parametro URL `page` (il valore è preso dalla variabile `pageNumber`).
- Il termine di ricerca, che deve essere specificato nel parametro URL `q` (il valore è preso dal valore del testo {{htmlelement("input")}} `searchTerm`).
- Il tipo di documento per restituire i risultati, come specificato in un'espressione passata tramite il parametro URL `fq`. In questo caso, vogliamo restituire articoli.

Successivamente, utilizziamo un paio di istruzioni [`if ()`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per verificare se gli elementi `startDate` e `endDate` contengono valori. Se lo sono, aggiungiamo i loro valori all'URL, specificati nei parametri URL `begin_date` e `end_date` rispettivamente.

Quindi, un URL completo finirebbe per apparire qualcosa del genere:

```url
https://api.nytimes.com/svc/search/v2/articlesearch.json?api-key=YOUR-API-KEY-HERE&page=0&q=cats&fq=document_type:("article")&begin_date=20170301&end_date=20170312
```

> [!NOTE]
> Puoi trovare ulteriori dettagli su quali parametri URL possono essere inclusi presso i [documenti sviluppatori NYTimes](https://developer.nytimes.com/).

> [!NOTE]
> L'esempio ha una convalida rudimentale dei dati del form — il campo del termine di ricerca deve essere compilato prima che il modulo possa essere inviato (ottenuto usando l'attributo `required`), e i campi della data hanno attributi `pattern` specificati, che significa che non verranno inviati a meno che i loro valori non consistano di 8 numeri (`pattern="[0-9]{8}"`). Vedi [Convalida dei dati del form](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) per ulteriori dettagli su come questi funzionano.

### Richiedere i dati dall'API

Ora che abbiamo costruito il nostro URL, facciamo una richiesta ad esso. Lo faremo usando l'[API Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch).

Aggiungi il seguente blocco di codice all'interno della funzione `fetchResults()`, appena sopra la parentesi graffa di chiusura:

```js
// Use fetch() to make the request to the API
fetch(url)
  .then((response) => response.json())
  .then((json) => displayResults(json))
  .catch((error) => console.error(`Error fetching data: ${error.message}`));
```

Qui eseguiamo la richiesta passando la nostra variabile `url` a [`fetch()`](/it/docs/Web/API/Window/fetch), convertiamo il body della risposta in JSON usando la funzione [`json()`](/it/docs/Web/API/Response/json), quindi passiamo il JSON risultante alla funzione `displayResults()` in modo che i dati possano essere visualizzati nella nostra interfaccia utente. Catturiamo e registriamo anche eventuali errori che potrebbero essere generati.

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

C'è molto codice qui; spieghiamolo passo passo:

- Il ciclo [`while`](/it/docs/Web/JavaScript/Reference/Statements/while) è un pattern comune usato per eliminare tutti i contenuti di un elemento DOM, in questo caso, l'elemento {{htmlelement("section")}}. Controlliamo continuamente se `<section>` ha un primo figlio e, se lo ha, rimuoviamo il primo figlio. Il ciclo termina quando `<section>` non ha più figli.
- Successivamente, impostiamo la variabile `articles` per essere uguale a `json.response.docs` — questo è l'array che contiene tutti gli oggetti che rappresentano gli articoli restituiti dalla ricerca. Questo viene fatto puramente per semplificare il codice seguente.
- Il primo blocco [`if ()`](/it/docs/Web/JavaScript/Reference/Statements/if...else) verifica se vengono restituiti 10 articoli (l'API restituisce fino a 10 articoli alla volta.) Se è così, visualizziamo il {{htmlelement("nav")}} che contiene i pulsanti di paginazione _Previous 10_/_Next 10_. Se vengono restituiti meno di 10 articoli, si adattano tutti su una pagina, quindi non abbiamo bisogno di mostrare i pulsanti di paginazione. Collegheremo la funzionalità di paginazione nella sezione successiva.
- Il successivo blocco `if ()` verifica se non viene restituito alcun articolo. In tal caso, non proviamo a visualizzare nulla — creiamo un {{htmlelement("p")}} con il testo "No results returned." e lo inseriamo nel `<section>`.
- Se vengono restituiti alcuni articoli, per prima cosa creiamo tutti gli elementi che vogliamo usare per visualizzare ogni articolo di notizie, inseriamo i contenuti giusti in ciascuno di essi, e quindi li inseriamo nel DOM nei posti appropriati. Per determinare quali proprietà negli oggetti articolo contenevano i dati giusti da mostrare, abbiamo consultato il riferimento API di Ricerca Articoli (vedi [NYTimes APIs](https://developer.nytimes.com/apis)). La maggior parte di queste operazioni sono abbastanza ovvie, ma alcune meritano di essere menzionate:

  - Abbiamo utilizzato un ciclo [`for...of`](/it/docs/Web/JavaScript/Reference/Statements/for...of) per passare attraverso tutte le parole chiave associate a ciascun articolo, e inserire ciascuna in un proprio {{htmlelement("span")}}, all'interno di un `<p>`. Questo è stato fatto per rendere facile lo stile di ciascuna.
  - Abbiamo utilizzato un blocco `if ()` (`if (current.multimedia.length > 0) { }`) per verificare se ogni articolo ha immagini associate, poiché alcune storie non ne hanno. Visualizziamo solo la prima immagine se esiste; altrimenti, si verificherebbe un errore.

### Collegare i pulsanti di paginazione

Per far funzionare i pulsanti di paginazione, incrementeremo (o decrementeremo) il valore della variabile `pageNumber` e quindi eseguiremo di nuovo la richiesta fetch con il nuovo valore incluso nel parametro URL del pagina. Questo funziona perché l'API NYTimes restituisce solo 10 risultati alla volta — se sono disponibili più di 10 risultati, restituirà i primi 10 (da 0 a 9) se il parametro URL `page` è impostato su 0 (o non incluso affatto — 0 è il valore predefinito), i successivi 10 (da 10 a 19) se è impostato su 1, e così via.

Questo ci consente di scrivere una funzione di paginazione semplice.

1. Sotto la chiamata esistente a [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener), aggiungi queste due nuove, che causano l'invocazione delle funzioni `nextPage()` e `previousPage()` quando vengono cliccati i relativi pulsanti:

   ```js
   nextBtn.addEventListener("click", nextPage);
   previousBtn.addEventListener("click", previousPage);
   ```

2. Sotto la tua aggiunta precedente, definiamo le due funzioni — aggiungi ora questo codice:

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

   La prima funzione incrementa la variabile `pageNumber`, quindi esegue la funzione `fetchResults()` di nuovo per visualizzare i risultati della pagina successiva.

   La seconda funzione funziona quasi allo stesso modo in inverso, ma dobbiamo anche fare il passo extra di controllare che `pageNumber` non sia già zero prima di decrementarlo — se la richiesta fetch viene eseguita con un parametro URL `page` negativo, potrebbe causare errori. Se `pageNumber` è già 0, facciamo [`return`](/it/docs/Web/JavaScript/Reference/Statements/return) fuori dalla funzione — se siamo già alla prima pagina, non abbiamo bisogno di caricare di nuovo gli stessi risultati.

> [!NOTE]
> Puoi trovare il nostro [codice esempio finale dell'API NYTimes su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/nytimes/finished/index.html) (vedi anche [in esecuzione dal vivo qui](https://mdn.github.io/learning-area/javascript/apis/third-party-apis/nytimes/finished/)).

## Esempio di YouTube

Abbiamo anche creato un altro esempio da studiare e da cui imparare — vedi il nostro [esempio di ricerca video di YouTube](https://mdn.github.io/learning-area/javascript/apis/third-party-apis/youtube/). Questo utilizza due API correlate:

- L'[API di YouTube Data](https://developers.google.com/youtube/v3/docs/) per cercare video su YouTube e restituire risultati.
- L'[API YouTube IFrame Player](https://developers.google.com/youtube/iframe_api_reference) per visualizzare gli esempi di video restituiti all'interno di player video IFrame in modo che tu possa guardarli.

Questo esempio è interessante perché mostra due API di terze parti correlate che vengono utilizzate insieme per costruire un'app. La prima è un'API RESTful, mentre la seconda funziona più come Mapquest (con metodi specifici dell'API, ecc.). Vale la pena notare tuttavia che entrambe le API richiedono che una libreria JavaScript venga applicata alla pagina. L'API RESTful ha funzioni disponibili per gestire le richieste HTTP e restituire i risultati.

![Uno screenshot di un esempio di ricerca video di YouTube utilizzando due API correlate. Sul lato sinistro dell'immagine c'è un esempio di query di ricerca utilizzando l'API di YouTube Data. Sul lato destro dell'immagine vengono visualizzati i risultati della ricerca utilizzando l'API YouTube Iframe Player.](youtube-example.png)

Non diremo molto di più su questo esempio nell'articolo — [il codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/youtube) ha commenti dettagliati inseriti al suo interno per spiegare come funziona.

Per farlo funzionare, dovrai:

- Leggere la documentazione [YouTube Data API Overview](https://developers.google.com/youtube/v3/getting-started).
- Assicurarti di visitare la [pagina API abilitate](https://console.cloud.google.com/apis/enabled), e nell'elenco delle API, assicurati che lo stato sia ON per l'API YouTube Data v3.
- Ottenere una chiave API da [Google Cloud](https://cloud.google.com/).
- Trovare la stringa `ENTER-API-KEY-HERE` nel codice sorgente e sostituirla con la tua chiave API.
- Eseguire l'esempio tramite un server web. Non funzionerà se lo esegui direttamente nel browser (ovvero, tramite un URL `file://`).

## Riepilogo

Questo articolo ti ha fornito un'introduzione utile all'uso delle API di terze parti per aggiungere funzionalità ai tuoi siti web.

{{PreviousMenu("Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}
