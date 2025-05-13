---
title: Lavorare con JSON
short-title: JSON
slug: Learn_web_development/Core/Scripting/JSON
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Network_requests","Learn_web_development/Core/Scripting/Debugging_JavaScript", "Learn_web_development/Core/Scripting")}}

JavaScript Object Notation (JSON) è un formato standard basato su testo per rappresentare dati strutturati basato sulla sintassi degli oggetti JavaScript. È comunemente usato per trasmettere dati nelle applicazioni web (ad esempio, inviare alcuni dati dal server al client affinché possano essere visualizzati su una pagina web, o viceversa). Si incontrerà spesso, quindi in questo articolo le forniamo tutte le informazioni necessarie per lavorare con JSON usando JavaScript, compresa l'analisi di JSON per accedere ai dati in esso contenuti e la creazione di JSON.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è JSON — un formato di dati usato molto comunemente, basato sulla sintassi degli oggetti JavaScript.</li>
          <li>Che JSON può anche contenere array.</li>
          <li>Recuperare JSON come un oggetto JavaScript utilizzando i meccanismi disponibili nelle API Web (ad esempio, <code>Response.json()</code> nell'API Fetch).</li>
          <li>Accedere ai valori all'interno dei dati JSON utilizzando la sintassi con parentesi quadre e dot notation.</li>
          <li>Conversione tra oggetti e testo usando <code>JSON.parse()</code> e <code>JSON.stringify()</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## No, davvero, cos'è JSON?

{{Glossary("JSON", "JSON")}} è un formato di dati basato su testo che segue la sintassi degli oggetti JavaScript. Rappresenta dati strutturati come una stringa, il che è utile quando si desidera trasmettere dati attraverso una rete. Anche se somiglia molto alla sintassi letterale degli oggetti di JavaScript, può essere usato indipendentemente da JavaScript. Molti ambienti di programmazione includono la capacità di leggere (analizzare) e generare JSON. In JavaScript, i metodi per analizzare e generare JSON sono forniti dall'oggetto [`JSON`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON).

> [!NOTE]
> Convertire una stringa in un oggetto nativo si chiama _deserializzazione_, mentre convertire un oggetto nativo in una stringa per poterlo trasmettere attraverso la rete si chiama _serializzazione_.

Una stringa JSON può essere memorizzata in un proprio file, che fondamentalmente è solo un file di testo con un'estensione `.json`, e un tipo {{Glossary("MIME_type", "MIME")}} di `application/json`.

### Struttura di JSON

Come descritto sopra, JSON è una stringa il cui formato somiglia molto al formato letterale degli oggetti di JavaScript. La seguente è una stringa JSON valida che rappresenta un oggetto. Nota come è anche un oggetto letterale di JavaScript valido — solo con alcune [restrizioni sintattiche](#restrizioni_sintattiche_di_json).

```json
{
  "squadName": "Super hero squad",
  "homeTown": "Metro City",
  "formed": 2016,
  "secretBase": "Super tower",
  "active": true,
  "members": [
    {
      "name": "Molecule Man",
      "age": 29,
      "secretIdentity": "Dan Jukes",
      "powers": ["Radiation resistance", "Turning tiny", "Radiation blast"]
    },
    {
      "name": "Madame Uppercut",
      "age": 39,
      "secretIdentity": "Jane Wilson",
      "powers": [
        "Million tonne punch",
        "Damage resistance",
        "Superhuman reflexes"
      ]
    },
    {
      "name": "Eternal Flame",
      "age": 1000000,
      "secretIdentity": "Unknown",
      "powers": [
        "Immortality",
        "Heat Immunity",
        "Inferno",
        "Teleportation",
        "Interdimensional travel"
      ]
    }
  ]
}
```

Se carica questo JSON nel suo programma JavaScript come una stringa, può analizzarlo in un oggetto normale e accedere ai dati al suo interno utilizzando la stessa notazione dot/bracket che abbiamo studiato nell'articolo [JavaScript object basics](/it/docs/Learn_web_development/Core/Scripting/Object_basics). Per esempio:

```js
superHeroes.homeTown;
superHeroes.members[1].powers[2];
```

1. Per prima cosa, abbiamo il nome della variabile — `superHeroes`.
2. All'interno di essa, vogliamo accedere alla proprietà `members`, quindi usiamo `.members`.
3. `members` contiene un array popolato da oggetti. Vogliamo accedere al secondo oggetto all'interno dell'array, quindi usiamo `[1]`.
4. All'interno di questo oggetto, vogliamo accedere alla proprietà `powers`, quindi usiamo `.powers`.
5. All'interno della proprietà `powers` c'è un array che contiene i superpoteri dell'eroe selezionato. Vogliamo il terzo, quindi usiamo `[2]`.

La lezione principale è che non c'è davvero nulla di speciale nel lavorare con JSON; dopo averlo analizzato in un oggetto JavaScript, si lavora con esso proprio come si farebbe con un oggetto dichiarato usando la stessa sintassi letterale.

> [!NOTE]
> Abbiamo reso il JSON visto sopra disponibile all'interno di una variabile nel nostro esempio [JSONTest.html](https://mdn.github.io/learning-area/javascript/oojs/json/JSONTest.html) (vede il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/JSONTest.html)). Provi a caricarlo e poi accedere ai dati all'interno della variabile tramite la console JavaScript del suo browser.

### Array come JSON

Sopra abbiamo menzionato che il testo JSON assomiglia fondamentalmente a un oggetto JavaScript all'interno di una stringa. Possiamo anche convertire array in/from JSON. Il seguente esempio è un JSON perfettamente valido:

```json
[
  {
    "name": "Molecule Man",
    "age": 29,
    "secretIdentity": "Dan Jukes",
    "powers": ["Radiation resistance", "Turning tiny", "Radiation blast"]
  },
  {
    "name": "Madame Uppercut",
    "age": 39,
    "secretIdentity": "Jane Wilson",
    "powers": [
      "Million tonne punch",
      "Damage resistance",
      "Superhuman reflexes"
    ]
  }
]
```

Deve accedere agli elementi dell'array (nella sua versione analizzata) iniziando con un indice di array, per esempio `superHeroes[0].powers[0]`.

Il JSON può anche contenere un singolo elemento primitivo. Per esempio, `29`, `"Dan Jukes"`, o `true` sono tutti JSON validi.

### Restrizioni sintattiche di JSON

Come accennato in precedenza, qualsiasi JSON è un letterale di oggetto JavaScript valido (oggetto, array, numero, ecc.). Il contrario non è vero, però: non tutti i letterali di oggetto JavaScript sono validi JSON.

- JSON può contenere solo tipi di dati _serializzabili_. Questo significa:
  - Per i primitivi, JSON può contenere stringhe letterali, numeri letterali, `true`, `false`, e `null`. È importante notare che non può contenere `undefined`, `NaN`, o `Infinity`.
  - Per i non primitivi, JSON può contenere oggetti letterali e array, ma non funzioni o altri tipi di oggetto, come `Date`, `Set`, e `Map`. Gli oggetti e gli array all'interno di JSON devono contenere ulteriori tipi di dati JSON validi.
- Le stringhe devono essere racchiuse tra virgolette doppie, non singole.
- I numeri devono essere scritti in notazione decimale.
- Ogni proprietà di un oggetto deve essere nella forma di `"key": value`. I nomi delle proprietà devono essere stringhe letterali racchiuse tra virgolette doppie. La sintassi speciale di JavaScript, come i metodi, non è consentita perché i metodi sono funzioni e le funzioni non sono tipi di dati JSON validi.
- Oggetti e array non possono contenere [virgole finali](/it/docs/Web/JavaScript/Reference/Trailing_commas).
- I commenti non sono consentiti in JSON.

Anche una sola virgola o due punti fuori posto può rendere un file JSON non valido e causarne il fallimento. Si dovrà essere attenti a validare qualsiasi dato che si tenta di utilizzare (anche se i JSON generati dal computer sono meno inclini a errori, fintanto che il programma generatore funziona correttamente). Può validare JSON utilizzando un'applicazione come [JSONLint](https://jsonlint.com/) o [JSON-validate](https://www.json-validate.com/)

## Apprendimento attivo: Lavorare con un esempio JSON

Quindi, lavoriamo su un esempio per mostrare come potremmo utilizzare alcuni dati formattati JSON su un sito web.

### Iniziare

Per iniziare, effettui copie locali dei nostri file [heroes.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes.html) e [style.css](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/style.css). Il secondo contiene alcuni semplici stili CSS per la nostra pagina, mentre il primo contiene del codice HTML per il corpo molto semplice, più un elemento {{HTMLElement("script")}} per contenere il codice JavaScript che scriveremo in questo esercizio:

```html-nolint
<header>
...
</header>

<section>
...
</section>

<script>
...
</script>
```

Abbiamo reso i nostri dati JSON disponibili sul nostro GitHub, a <https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json>.

Caricheremo il JSON nel nostro script e useremo alcune manipolazioni DOM ingegnose per visualizzarlo, in questo modo:

![Immagine di un documento intitolato "Super hero squad" (in un carattere elegante) e sottotitolato "Hometown: Metro City // Formed: 2016". Tre colonne sotto il titolo sono intitolate rispettivamente "Molecule Man", "Madame Uppercut" e "Eternal Flame". Ogni colonna elenca il nome segreto dell'eroe, l'età e i superpoteri.](json-superheroes.png)

### Funzione di livello superiore

La funzione di livello superiore sembra così:

```js
async function populate() {
  const requestURL =
    "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";
  const request = new Request(requestURL);

  const response = await fetch(request);
  const superHeroes = await response.json();

  populateHeader(superHeroes);
  populateHeroes(superHeroes);
}
```

Per ottenere il JSON, usiamo un'API chiamata [Fetch](/it/docs/Web/API/Fetch_API). Questa API ci permette di fare richieste di rete per recuperare risorse da un server tramite JavaScript (ad esempio, immagini, testo, JSON, persino frammenti di HTML), il che significa che possiamo aggiornare piccole sezioni di contenuto senza dover ricaricare l'intera pagina.

Nella nostra funzione, le prime quattro righe utilizzano l'API Fetch per ottenere il JSON dal server:

- dichiariamo la variabile `requestURL` per memorizzare l'URL di GitHub
- utilizziamo l'URL per inizializzare un nuovo oggetto [`Request`](/it/docs/Web/API/Request).
- facciamo la richiesta di rete usando la funzione [`fetch()`](/it/docs/Web/API/Window/fetch), e questo restituisce un oggetto [`Response`](/it/docs/Web/API/Response)
- recuperiamo la risposta come JSON utilizzando la funzione [`json()`](/it/docs/Web/API/Response/json) dell'oggetto `Response`.

> [!NOTE]
> L'API `fetch()` è **asincrona**. Può apprendere i dettagli delle funzioni asincrone nel nostro modulo [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS), ma per ora, diremo soltanto che dobbiamo aggiungere la parola chiave {{jsxref("Statements/async_function", "async")}} prima del nome della funzione che utilizza l'API fetch e aggiungere la parola chiave {{jsxref("Operators/await", "await")}} prima delle chiamate a qualsiasi funzione asincrona.

Dopo tutto ciò, la variabile `superHeroes` conterrà l'oggetto JavaScript basato sul JSON. Passiamo poi quell'oggetto a due chiamate di funzione — la prima riempie il `<header>` con i dati corretti, mentre la seconda crea una scheda informativa per ciascun eroe nel gruppo e la inserisce nel `<section>`.

### Riempire l'intestazione

Ora che abbiamo recuperato i dati JSON e li abbiamo convertiti in un oggetto JavaScript, facciamo uso di essi scrivendo le due funzioni a cui abbiamo fatto riferimento sopra. Prima di tutto, aggiunga la seguente definizione di funzione sotto il codice precedente:

```js
function populateHeader(obj) {
  const header = document.querySelector("header");
  const myH1 = document.createElement("h1");
  myH1.textContent = obj.squadName;
  header.appendChild(myH1);

  const myPara = document.createElement("p");
  myPara.textContent = `Hometown: ${obj.homeTown} // Formed: ${obj.formed}`;
  header.appendChild(myPara);
}
```

Qui creiamo prima un elemento {{HTMLElement("Heading_Elements", "h1")}} con [`createElement()`](/it/docs/Web/API/Document/createElement), impostiamo il suo [`textContent`](/it/docs/Web/API/Node/textContent) per essere uguale alla proprietà `squadName` dell'oggetto, quindi lo aggiungiamo all'intestazione usando [`appendChild()`](/it/docs/Web/API/Node/appendChild). Poi facciamo un'operazione molto simile con un paragrafo: lo creiamo, impostiamo il suo contenuto testuale e lo aggiungiamo all'intestazione. L'unica differenza è che il suo testo è impostato su un [template literal](/it/docs/Web/JavaScript/Reference/Template_literals) contenente sia le proprietà `homeTown` che `formed` dell'oggetto.

### Creare le schede informative degli eroi

Successivamente, aggiunga la seguente funzione alla fine del codice, che crea e visualizza le schede degli eroi:

```js
function populateHeroes(obj) {
  const section = document.querySelector("section");
  const heroes = obj.members;

  for (const hero of heroes) {
    const myArticle = document.createElement("article");
    const myH2 = document.createElement("h2");
    const myPara1 = document.createElement("p");
    const myPara2 = document.createElement("p");
    const myPara3 = document.createElement("p");
    const myList = document.createElement("ul");

    myH2.textContent = hero.name;
    myPara1.textContent = `Secret identity: ${hero.secretIdentity}`;
    myPara2.textContent = `Age: ${hero.age}`;
    myPara3.textContent = "Superpowers:";

    const superPowers = hero.powers;
    for (const power of superPowers) {
      const listItem = document.createElement("li");
      listItem.textContent = power;
      myList.appendChild(listItem);
    }

    myArticle.appendChild(myH2);
    myArticle.appendChild(myPara1);
    myArticle.appendChild(myPara2);
    myArticle.appendChild(myPara3);
    myArticle.appendChild(myList);

    section.appendChild(myArticle);
  }
}
```

Per iniziare, conserviamo la proprietà `members` dell'oggetto JavaScript in una nuova variabile. Questo array contiene più oggetti che contengono le informazioni per ciascun eroe.

Successivamente, usiamo un [ciclo for...of](/it/docs/Learn_web_development/Core/Scripting/Loops#the_for...of_loop) per scorrere ogni oggetto dell'array. Per ciascuno di essi, noi:

1. Creiamo diversi nuovi elementi: un `<article>`, un `<h2>`, tre `<p>` e un `<ul>`.
2. Impostiamo il `<h2>` per contenere il `name` dell'eroe corrente.
3. Riempiamo i tre paragrafi con il loro `secretIdentity`, `age`, e una linea che dice "Superpowers:" per introdurre le informazioni nella lista.
4. Conserviamo la proprietà `powers` in un'altra nuova costante chiamata `superPowers` — questo contiene un array che elenca i superpoteri dell'eroe corrente.
5. Usiamo un altro ciclo `for...of` per scorrere i superpoteri dell'eroe corrente — per ciascuno creiamo un elemento `<li>`, mettiamo il superpotere all'interno, quindi inseriamo l'elemento `listItem` nel `<ul>` (`myList`) usando `appendChild()`.
6. L'ultima cosa che facciamo è appendere il `<h2>`, i `<p>` e il `<ul>` dentro l'`<article>` (`myArticle`), poi appendere l'`<article>` dentro il `<section>`. L'ordine in cui le cose sono aggiunte è importante, poiché questo è l'ordine in cui saranno visualizzate all'interno dell'HTML.

> [!NOTE]
> Se sta avendo difficoltà a far funzionare l'esempio, provi a fare riferimento al nostro [heroes-finished.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes-finished.html) codice sorgente (vede anche il [live](https://mdn.github.io/learning-area/javascript/oojs/json/heroes-finished.html)).

> [!NOTE]
> Se sta avendo difficoltà a seguire la notazione dot/bracket che stiamo usando per accedere all'oggetto JavaScript, può aiutare ad avere il file [superheroes.json](https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json) aperto in un'altra scheda o nell'editor di testo, e fare riferimento a esso mentre guarda il nostro JavaScript.
> Dovrebbe anche far riferimento al nostro articolo [JavaScript object basics](/it/docs/Learn_web_development/Core/Scripting/Object_basics) per ulteriori informazioni sulla notazione dot e bracket.

### Chiamare la funzione di livello superiore

Infine, dobbiamo chiamare la nostra funzione di livello superiore `populate()`:

```js
populate();
```

## Conversione tra oggetti e testo

L'esempio sopra era semplice in termini di accesso all'oggetto JavaScript, perché abbiamo convertito la risposta di rete direttamente in un oggetto JavaScript usando `response.json()`.

Ma a volte non siamo così fortunati — a volte riceviamo una stringa JSON grezza e dobbiamo convertirla in un oggetto noi stessi. E quando vogliamo inviare un oggetto JavaScript attraverso la rete, dobbiamo convertirlo in JSON (una stringa) prima di inviarlo. Fortunatamente, questi due problemi sono così comuni nello sviluppo web che un oggetto [JSON](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON) incorporato è disponibile nei browser, che contiene i seguenti due metodi:

- [`parse()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse): Accetta una stringa JSON come parametro e restituisce il corrispondente oggetto JavaScript.
- [`stringify()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify): Accetta un oggetto come parametro e restituisce la corrispondente stringa JSON.

Può vedere il primo in azione nel nostro esempio [heroes-finished-json-parse.html](https://mdn.github.io/learning-area/javascript/oojs/json/heroes-finished-json-parse.html) (vede il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes-finished-json-parse.html)) — questo fa esattamente la stessa cosa dell'esempio che abbiamo costruito in precedenza, tranne per il fatto che:

- recuperiamo la risposta come testo piuttosto che JSON, chiamando il metodo [`text()`](/it/docs/Web/API/Response/text) della risposta
- poi usiamo `parse()` per convertire il testo in un oggetto JavaScript.

Lo snippet di codice chiave è qui:

```js
async function populate() {
  const requestURL =
    "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";
  const request = new Request(requestURL);

  const response = await fetch(request);
  const superHeroesText = await response.text();

  const superHeroes = JSON.parse(superHeroesText);
  populateHeader(superHeroes);
  populateHeroes(superHeroes);
}
```

Come potrà immaginare, `stringify()` funziona nel modo opposto. Provi a inserire le seguenti righe nella console JavaScript del suo browser una per una per vederlo in azione:

```js
let myObj = { name: "Chris", age: 38 };
myObj;
let myString = JSON.stringify(myObj);
myString;
```

Qui stiamo creando un oggetto JavaScript, poi controlliamo cosa contiene, poi lo convertiamo in una stringa JSON usando `stringify()` — salvando il valore di ritorno in una nuova variabile — poi controlliamo di nuovo.

## Verifichi le sue competenze!

È arrivato alla fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare di aver assimilato queste informazioni prima di passare oltre — veda [Test your skills: JSON](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/JSON).

## Sommario

In questa lezione, le abbiamo introdotto l'uso di JSON nei suoi programmi, compreso come creare e analizzare JSON e come accedere ai dati bloccati al suo interno. Nel prossimo articolo, esamineremo tecniche pratiche per fare il debug di JavaScript e gestire gli errori.

## Veda anche

- [Riferimento JSON](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [Panoramica dell'API Fetch](/it/docs/Web/API/Fetch_API)
- [Usare Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch)
- [Metodi di richiesta HTTP](/it/docs/Web/HTTP/Reference/Methods)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Network_requests","Learn_web_development/Core/Scripting/Debugging_JavaScript", "Learn_web_development/Core/Scripting")}}
