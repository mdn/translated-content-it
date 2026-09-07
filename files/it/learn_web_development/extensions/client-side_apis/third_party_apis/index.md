---
title: API di terze parti
slug: Learn_web_development/Extensions/Client-side_APIs/Third_party_APIs
l10n:
  sourceCommit: f4c14731a1a157fc8d8f7357ac4d74d14a7d7fb5
---

{{PreviousMenu("Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}

Le API affrontate finora sono integrate nel browser, ma non tutte le API lo sono. Molti grandi siti web e servizi come Google Maps, Twitter, Facebook, PayPal, ecc. forniscono API che consentono agli sviluppatori di utilizzare i loro dati (ad esempio, visualizzare il proprio flusso Twitter sul proprio blog) o servizi (ad esempio, usare il login di Facebook per far accedere gli utenti). Questo articolo esamina la differenza tra le API del browser e le API di terze parti e mostra alcuni usi tipici di queste ultime.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>, in particolare con <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">i concetti di base sugli oggetti JavaScript</a> e la trattazione delle API fondamentali, come <a href="/it/docs/Learn_web_development/Core/Scripting/DOM_scripting">lo scripting del DOM</a> e <a href="/it/docs/Learn_web_development/Core/Scripting/Network_requests">le richieste di rete</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I concetti alla base delle API di terze parti e i pattern associati, come le chiavi API.</li>
          <li>Utilizzare un'API di mappe di terze parti.</li>
          <li>Utilizzare un'API RESTful.</li>
          <li>Utilizzare le API YouTube di Google.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono le API di terze parti?

Le API di terze parti sono API fornite da soggetti terzi — generalmente aziende come Facebook, Twitter o Google — per consentire di accedere alla loro funzionalità tramite JavaScript e utilizzarla sul proprio sito. Uno degli esempi più evidenti è l'uso delle API di mappe per visualizzare mappe personalizzate nelle proprie pagine.

Osserviamo un [semplice esempio dell'API Mapquest](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/mapquest) e usiamolo per illustrare in che modo le API di terze parti differiscono dalle API del browser.

### Si trovano su server di terze parti

Le API del browser sono integrate nel browser: è possibile accedervi immediatamente da JavaScript. Ad esempio, alla Web Audio API che [abbiamo visto nell'articolo introduttivo](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#how_do_apis_work) si accede usando l'oggetto nativo [`AudioContext`](/it/docs/Web/API/AudioContext). Per esempio:

```js
const audioCtx = new AudioContext();
// …
const audioElement = document.querySelector("audio");
// …
const audioSource = audioCtx.createMediaElementSource(audioElement);
// etc.
```

Le API di terze parti, invece, si trovano su server di terze parti. Per accedervi da JavaScript, è prima necessario connettersi alla funzionalità dell'API e renderla disponibile nella propria pagina. Questo comporta in genere il collegamento a una libreria JavaScript disponibile sul server tramite un elemento {{htmlelement("script")}}, come si vede nel nostro esempio Mapquest:

```html
<script
  src="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.js"
  defer></script>
<link
  rel="stylesheet"
  href="https://api.mqcdn.com/sdk/mapquest-js/v1.3.2/mapquest.css" />
```

È quindi possibile iniziare a usare gli oggetti disponibili in quella libreria. Per esempio:

```js
const map = L.mapquest.map("map", {
  center: [53.480759, -2.242631],
  layers: L.mapquest.tileLayer("map"),
  zoom: 12,
});
```

Qui viene creata una variabile in cui memorizzare le informazioni della mappa, quindi viene creata una nuova mappa usando il metodo `mapquest.map()`, che accetta come parametri l'ID di un elemento {{htmlelement("div")}} in cui visualizzare la mappa (`'map'`) e un oggetto di opzioni contenente i dettagli della mappa specifica da visualizzare. In questo caso vengono specificate le coordinate del centro della mappa, un layer della mappa di tipo `map` da mostrare (creato usando il metodo `mapquest.tileLayer()`) e il livello di zoom predefinito.

Queste sono tutte le informazioni necessarie all'API Mapquest per tracciare una semplice mappa. Il server a cui ci si connette gestisce tutte le operazioni complesse, come la visualizzazione dei riquadri di mappa corretti per l'area mostrata, ecc.

> [!NOTE]
> Alcune API gestiscono l'accesso alle proprie funzionalità in modo leggermente diverso, richiedendo allo sviluppatore di effettuare una richiesta HTTP a un pattern URL specifico per recuperare i dati. Queste sono chiamate [API RESTful — un esempio verrà mostrato più avanti](#a_restful_api_%e2%80%94_nytimes).

### Generalmente richiedono chiavi API

La sicurezza delle API del browser tende a essere gestita tramite richieste di autorizzazione, come [discusso nel primo articolo](/it/docs/Learn_web_development/Extensions/Client-side_APIs/Introduction#they_have_additional_security_mechanisms_where_appropriate). Il loro scopo è fare in modo che l'utente sappia cosa accade nei siti web visitati e abbia meno probabilità di diventare vittima di chi usa un'API in modo malevolo.

Le API di terze parti hanno un sistema di autorizzazioni leggermente diverso: tendono a usare chiavi per sviluppatori per consentire agli sviluppatori di accedere alla funzionalità dell'API, più per proteggere il fornitore dell'API che l'utente.

Nell'esempio dell'API Mapquest si troverà una riga simile alla seguente:

```js
L.mapquest.key = "YOUR-API-KEY-HERE";
```

Questa riga specifica una chiave API o chiave per sviluppatori da usare nell'applicazione: lo sviluppatore dell'applicazione deve richiedere una chiave e quindi includerla nel proprio codice per ottenere l'accesso alla funzionalità dell'API. Nel nostro esempio è stato fornito soltanto un segnaposto.

> [!NOTE]
> Quando vengono creati esempi personali, occorre usare la propria chiave API al posto di qualsiasi segnaposto.

Altre API potrebbero richiedere l'inclusione della chiave in un modo leggermente diverso, ma il pattern è relativamente simile per la maggior parte di esse.

Richiedere una chiave consente al provider dell'API di ritenere gli utenti dell'API responsabili delle proprie azioni. Quando lo sviluppatore si registra per ottenere una chiave, diventa noto al provider dell'API, che può intervenire se inizia a fare qualcosa di malevolo con l'API, come tracciare la posizione delle persone o tentare di inviare all'API un numero elevato di richieste per impedirne il funzionamento. L'azione più semplice sarebbe semplicemente revocare i privilegi API.

## Estendere l'esempio Mapquest

Aggiungiamo altre funzionalità all'esempio Mapquest per mostrare come usare alcune altre caratteristiche dell'API.

1. Per iniziare questa sezione, creare una copia del [file iniziale Mapquest](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/mapquest/start/index.html), in una nuova directory. Se è già stato clonato il [repository degli esempi](https://github.com/mdn/learning-area), si dispone già di una copia di questo file, reperibile nella directory _javascript/apis/third-party-apis/mapquest/start_.
2. Successivamente, è necessario andare al [sito per sviluppatori Mapquest](https://developer.mapquest.com/), creare un account e quindi creare una chiave per sviluppatori da usare con l'esempio. Al momento della stesura, sul sito era chiamata "consumer key" e il processo di creazione della chiave chiedeva anche un "callback URL" facoltativo. Non è necessario inserire un URL qui: basta lasciarlo vuoto.
3. Aprire il file iniziale e sostituire il segnaposto della chiave API con la propria chiave.

### Modificare il tipo di mappa

Esistono diversi tipi di mappa visualizzabili con l'API Mapquest. Per farlo, individuare la riga seguente:

```js-nolint
layers: L.mapquest.tileLayer("map"),
```

Provare a cambiare `'map'` in `'hybrid'` per visualizzare una mappa in stile ibrido. Provare anche altri valori. La [pagina di riferimento di `tileLayer`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-tile-layer/) mostra le diverse opzioni disponibili, oltre a molte altre informazioni.

### Aggiungere controlli diversi

La mappa dispone di diversi controlli; per impostazione predefinita mostra soltanto un controllo dello zoom. È possibile ampliare i controlli disponibili usando il metodo `map.addControl()`; aggiungere questo al codice:

```js
map.addControl(L.mapquest.control());
```

Il [`metodo mapquest.control()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-control/) crea semplicemente un set di controlli completo e, per impostazione predefinita, viene posizionato nell'angolo in alto a destra. È possibile regolare la posizione specificando come parametro per il controllo un oggetto di opzioni contenente una proprietà `position`, il cui valore è una stringa che specifica una posizione per il controllo. Provare, ad esempio, questo:

```js
map.addControl(L.mapquest.control({ position: "bottomright" }));
```

Sono disponibili altri tipi di controllo, ad esempio [`mapquest.searchControl()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-search-control/) e [`mapquest.satelliteControl()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-satellite-control/), e alcuni sono piuttosto complessi e potenti. Sperimentare per vedere cosa è possibile ottenere.

### Aggiungere un marcatore personalizzato

Aggiungere un marcatore (icona) in un determinato punto della mappa è semplice: basta usare il metodo [`L.marker()`](https://leafletjs.com/reference.html#marker), che sembra essere documentato nella documentazione correlata di Leaflet.js. Aggiungere il codice seguente all'esempio, sempre all'interno di `window.onload`:

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

Come si può vedere, nella forma più semplice questo accetta due parametri: un array contenente le coordinate in cui visualizzare il marcatore e un oggetto di opzioni contenente una proprietà `icon` che definisce l'icona da visualizzare in quel punto.

L'icona viene definita usando un metodo [`mapquest.icons.marker()`](https://developer.mapquest.com/documentation/mapquest-js/v1.3/l-mapquest-icons/), che, come si può vedere, contiene informazioni quali il colore e la dimensione del marcatore.

Alla fine della prima chiamata di metodo viene concatenato `.bindPopup('This is Manchester!')`, che definisce il contenuto da visualizzare quando si fa clic sul marcatore.

Infine, viene concatenato `.addTo(map)` alla fine della catena per aggiungere effettivamente il marcatore alla mappa.

Sperimentare con le altre opzioni mostrate nella documentazione per vedere cosa è possibile ottenere. Mapquest fornisce funzionalità piuttosto avanzate, come indicazioni stradali, ricerca, ecc.

> [!NOTE]
> Se si riscontrano problemi nel far funzionare l'esempio, confrontare il codice con la nostra [versione completata](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/mapquest/finished/script.js).

## Un'API RESTful — NYTimes

Vediamo ora un altro esempio di API: la [New York Times API](https://developer.nytimes.com/). Questa API consente di recuperare informazioni sugli articoli di cronaca del New York Times e visualizzarle sul proprio sito. Questo tipo di API è noto come **API RESTful**: invece di ottenere dati usando le funzionalità di una libreria JavaScript come fatto con Mapquest, si ottengono dati effettuando richieste HTTP a URL specifici, con dati come termini di ricerca e altre proprietà codificati nell'URL, spesso come parametri URL. Questo è un pattern comune che si incontrerà con le API.

Di seguito viene proposto un esercizio che mostra come usare l'API NYTimes e fornisce anche una serie di passaggi più generali da seguire come approccio al lavoro con nuove API.

### Trovare la documentazione

Quando si desidera usare un'API di terze parti, è essenziale scoprire dove si trova la documentazione, così da poter capire quali funzionalità possiede l'API, come usarle, ecc. La documentazione dell'API New York Times si trova all'indirizzo <https://developer.nytimes.com/>.

### Ottenere una chiave per sviluppatori

La maggior parte delle API richiede l'uso di un qualche tipo di chiave per sviluppatori, per ragioni di sicurezza e responsabilità. Per registrarsi e ottenere una chiave API NYTimes, seguire le istruzioni disponibili su <https://developer.nytimes.com/get-started>.

1. Richiediamo una chiave per l'Article Search API: creare una nuova app selezionando questa come API da usare, compilare nome e descrizione, attivare l'interruttore sotto "Article Search API", quindi fare clic su "Create".
2. Ottenere la chiave API dalla pagina risultante.
3. Ora, per avviare l'esempio, creare una copia di tutti i file nella directory [nytimes/start](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/nytimes/start). Se è già stato clonato il [repository degli esempi](https://github.com/mdn/learning-area), si dispone già di una copia di questi file, reperibile nella directory _javascript/apis/third-party-apis/nytimes/start_. Inizialmente il file `script.js` contiene diverse variabili necessarie per l'impostazione dell'esempio; di seguito verranno aggiunte le funzionalità richieste.

L'app consentirà infine di digitare un termine di ricerca e date di inizio e fine facoltative, che verranno poi usati per interrogare l'Article Search API e visualizzare i risultati della ricerca.

![Uno screenshot di una query di ricerca di esempio e dei risultati di ricerca recuperati dalla New York Article Search API.](nytimes-example.png)

### Collegare l'API all'app

Per prima cosa, è necessario stabilire una connessione tra l'API e l'app. Nel caso di questa API, è necessario includere la chiave API come parametro [get](/it/docs/Web/HTTP/Reference/Methods/GET) ogni volta che si richiedono dati al servizio all'URL corretto.

1. Individuare la riga seguente:

   ```js
   const key = "INSERT-YOUR-API-KEY-HERE";
   ```

   Sostituire la chiave API esistente con la chiave API effettiva ottenuta nella sezione precedente.

2. Aggiungere la riga seguente al JavaScript, sotto il commento `// Event listeners to control the functionality`. Questa esegue una funzione chiamata `submitSearch()` quando il modulo viene inviato, cioè quando si preme il pulsante.

   ```js
   searchForm.addEventListener("submit", submitSearch);
   ```

3. Ora aggiungere le definizioni delle funzioni `submitSearch()` e `fetchResults()`, sotto la riga precedente:

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

`submitSearch()` reimposta inizialmente il numero di pagina a 0, quindi chiama `fetchResults()`. Questa chiama anzitutto [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento, per impedire l'effettivo invio del modulo, che interromperebbe l'esempio. Successivamente, viene usata una manipolazione di stringhe per assemblare l'URL completo a cui verrà effettuata la richiesta. Si inizia assemblando le parti ritenute obbligatorie per questa demo:

- L'URL di base, ricavato dalla variabile `baseURL`.
- La chiave API, che deve essere specificata nel parametro URL `api-key`, con valore ricavato dalla variabile `key`.
- Il numero di pagina, che deve essere specificato nel parametro URL `page`, con valore ricavato dalla variabile `pageNumber`.
- Il termine di ricerca, che deve essere specificato nel parametro URL `q`, con valore ricavato dal valore dell'{{htmlelement("input")}} di testo `searchTerm`.
- Il tipo di documento per cui restituire i risultati, come specificato in un'espressione passata tramite il parametro URL `fq`. In questo caso, si desidera restituire articoli.

Successivamente, vengono usate un paio di istruzioni [`if ()`](/it/docs/Web/JavaScript/Reference/Statements/if...else) per verificare se gli elementi `startDate` e `endDate` hanno valori compilati. In tal caso, i relativi valori vengono aggiunti all'URL, specificati rispettivamente nei parametri URL `begin_date` e `end_date`.

Pertanto, un URL completo risulterebbe simile a questo:

```url
https://api.nytimes.com/svc/search/v2/articlesearch.json?api-key=YOUR-API-KEY-HERE&page=0&q=cats&fq=document_type:("article")&begin_date=20170301&end_date=20170312
```

> [!NOTE]
> Maggiori dettagli sui parametri URL che possono essere inclusi sono disponibili nella [documentazione per sviluppatori NYTimes](https://developer.nytimes.com/).

> [!NOTE]
> L'esempio dispone di una convalida rudimentale dei dati del modulo: il campo del termine di ricerca deve essere compilato prima che il modulo possa essere inviato, ottenuto tramite l'attributo `required`, e i campi data hanno attributi `pattern` specificati, il che significa che non verranno inviati se i loro valori non sono composti da 8 cifre (`pattern="[0-9]{8}"`). Consultare [Convalida dei dati dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Form_validation) per maggiori dettagli sul loro funzionamento.

### Richiedere dati dall'API

Ora che l'URL è stato costruito, effettuiamo una richiesta. Verrà usata la [Fetch API](/it/docs/Web/API/Fetch_API/Using_Fetch).

Aggiungere il seguente blocco di codice all'interno della funzione `fetchResults()`, immediatamente sopra la parentesi graffa di chiusura:

```js
// Use fetch() to make the request to the API
fetch(url)
  .then((response) => response.json())
  .then((json) => displayResults(json))
  .catch((error) => console.error(`Error fetching data: ${error.message}`));
```

Qui viene eseguita la richiesta passando la variabile `url` a [`fetch()`](/it/docs/Web/API/Window/fetch), viene convertito il corpo della risposta in JSON usando la funzione [`json()`](/it/docs/Web/API/Response/json), quindi il JSON risultante viene passato alla funzione `displayResults()` affinché i dati possano essere visualizzati nell'interfaccia utente. Vengono inoltre intercettati e registrati eventuali errori generati.

### Visualizzare i dati

Vediamo ora come visualizzare i dati. Aggiungere la funzione seguente sotto la funzione `fetchResults()`.

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
      const para = document.createElement("p");
      const keywordPara = document.createElement("p");
      keywordPara.classList.add("keywords");

      console.log(current);

      link.href = current.web_url;
      link.textContent = current.headline.main;
      para.textContent = current.snippet;
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
      article.appendChild(para);
      article.appendChild(keywordPara);
      section.appendChild(article);
    }
  }
}
```

Qui è presente molto codice; esaminiamolo passo dopo passo:

- Il ciclo [`while`](/it/docs/Web/JavaScript/Reference/Statements/while) è un pattern comune usato per eliminare tutto il contenuto di un elemento DOM, in questo caso l'elemento {{htmlelement("section")}}. Si continua a verificare se `<section>` ha un primo figlio e, se lo ha, si rimuove il primo figlio. Il ciclo termina quando `<section>` non ha più figli.
- Successivamente, la variabile `articles` viene impostata uguale a `json.response.docs`: questo è l'array che contiene tutti gli oggetti che rappresentano gli articoli restituiti dalla ricerca. Questa operazione viene eseguita unicamente per rendere il codice seguente un po' più semplice.
- Il primo blocco [`if ()`](/it/docs/Web/JavaScript/Reference/Statements/if...else) verifica se vengono restituiti 10 articoli, poiché l'API restituisce fino a 10 articoli alla volta. In tal caso, viene visualizzato l'elemento {{htmlelement("nav")}} che contiene i pulsanti di paginazione _Previous 10_/_Next 10_. Se vengono restituiti meno di 10 articoli, saranno tutti contenuti in una pagina, quindi non è necessario mostrare i pulsanti di paginazione. La funzionalità di paginazione verrà collegata nella sezione successiva.
- Il blocco `if ()` successivo verifica se non vengono restituiti articoli. In tal caso, non si tenta di visualizzarne alcuno: viene creato un elemento {{htmlelement("p")}} contenente il testo "No results returned." e viene inserito nel `<section>`.
- Se vengono restituiti alcuni articoli, vengono innanzitutto creati tutti gli elementi da usare per visualizzare ogni articolo di cronaca, viene inserito il contenuto corretto in ciascuno di essi e poi vengono inseriti nel DOM nelle posizioni appropriate. Per capire quali proprietà degli oggetti articolo contenessero i dati corretti da mostrare, è stata consultata la documentazione dell'Article Search API, vedere [NYTimes APIs](https://developer.nytimes.com/apis). La maggior parte di queste operazioni è piuttosto evidente, ma alcune meritano di essere evidenziate:
  - È stato usato un ciclo [`for...of`](/it/docs/Web/JavaScript/Reference/Statements/for...of) per scorrere tutte le parole chiave associate a ogni articolo e inserire ciascuna all'interno del proprio {{htmlelement("span")}}, all'interno di un `<p>`. Questo è stato fatto per semplificare lo stile di ciascuna parola chiave.
  - È stato usato un blocco `if ()` (`if (current.multimedia.length > 0) { }`) per verificare se ogni articolo ha immagini associate, poiché alcuni articoli non ne hanno. Viene visualizzata soltanto la prima immagine se esiste; altrimenti verrebbe generato un errore.

### Collegare i pulsanti di paginazione

Per far funzionare i pulsanti di paginazione, verrà incrementato o decrementato il valore della variabile `pageNumber`, quindi verrà eseguita nuovamente la richiesta fetch con il nuovo valore incluso nel parametro URL della pagina. Questo funziona perché l'API NYTimes restituisce soltanto 10 risultati alla volta: se sono disponibili più di 10 risultati, restituirà i primi 10 (0-9) se il parametro URL `page` è impostato a 0, o non è incluso affatto, poiché 0 è il valore predefinito; i successivi 10 (10-19) se è impostato a 1, e così via.

Ciò consente di scrivere una semplice funzione di paginazione.

1. Sotto la chiamata [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) esistente, aggiungere queste due nuove chiamate, che fanno sì che le funzioni `nextPage()` e `previousPage()` vengano invocate quando si fa clic sui pulsanti pertinenti:

   ```js
   nextBtn.addEventListener("click", nextPage);
   previousBtn.addEventListener("click", previousPage);
   ```

2. Sotto l'aggiunta precedente, definiamo le due funzioni: aggiungere ora questo codice:

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

   La prima funzione incrementa la variabile `pageNumber`, quindi esegue nuovamente la funzione `fetchResults()` per visualizzare i risultati della pagina successiva.

   La seconda funzione opera in modo quasi identico al contrario, ma è necessario anche verificare che `pageNumber` non sia già zero prima di decrementarlo: se la richiesta fetch viene eseguita con un parametro URL `page` negativo, potrebbe causare errori. Se `pageNumber` è già 0, viene eseguito un [`return`](/it/docs/Web/JavaScript/Reference/Statements/return) dalla funzione: se ci si trova già alla prima pagina, non è necessario caricare nuovamente gli stessi risultati.

> [!NOTE]
> Il [codice di esempio dell'API NYTimes completato è disponibile su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/apis/third-party-apis/nytimes/finished/index.html); è anche possibile [vederlo in esecuzione qui](https://mdn.github.io/learning-area/javascript/apis/third-party-apis/nytimes/finished/).

## Esempio YouTube

È stato creato anche un altro esempio da studiare e da cui apprendere: vedere il nostro [esempio di ricerca di video YouTube](https://mdn.github.io/learning-area/javascript/apis/third-party-apis/youtube/). Questo usa due API correlate:

- La [YouTube Data API](https://developers.google.com/youtube/v3/docs/) per cercare video YouTube e restituire risultati.
- La [YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference) per visualizzare gli esempi di video restituiti all'interno di player video IFrame, così da poterli guardare.

Questo esempio è interessante perché mostra due API di terze parti correlate usate insieme per creare un'app. La prima è un'API RESTful, mentre la seconda funziona più come Mapquest, con metodi specifici dell'API, ecc. Vale tuttavia la pena notare che entrambe le API richiedono l'applicazione di una libreria JavaScript alla pagina. L'API RESTful dispone di funzioni per gestire l'esecuzione delle richieste HTTP e la restituzione dei risultati.

![Uno screenshot di una ricerca di esempio di video YouTube che usa due API correlate. Il lato sinistro dell'immagine mostra una query di ricerca di esempio che usa la YouTube Data API. Il lato destro dell'immagine visualizza i risultati della ricerca usando la YouTube Iframe Player API.](youtube-example.png)

Non verrà detto molto altro su questo esempio nell'articolo: il [codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/apis/third-party-apis/youtube) contiene commenti dettagliati che spiegano come funziona.

Per eseguirlo, sarà necessario:

- Leggere la documentazione [YouTube Data API Overview](https://developers.google.com/youtube/v3/getting-started).
- Assicurarsi di visitare la [pagina delle API abilitate](https://console.cloud.google.com/apis/enabled) e, nell'elenco delle API, verificare che lo stato della YouTube Data API v3 sia ON.
- Ottenere una chiave API da [Google Cloud](https://cloud.google.com/).
- Individuare la stringa `ENTER-API-KEY-HERE` nel codice sorgente e sostituirla con la propria chiave API.
- Eseguire l'esempio tramite un server web. Non funzionerà se eseguito direttamente nel browser, cioè tramite un URL `file://`.

## Riepilogo

Questo articolo ha fornito un'utile introduzione all'uso delle API di terze parti per aggiungere funzionalità ai siti web.

{{PreviousMenu("Learn_web_development/Extensions/Client-side_APIs/Client-side_storage", "Learn_web_development/Extensions/Client-side_APIs")}}
