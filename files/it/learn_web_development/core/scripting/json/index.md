---
title: Lavorare con JSON
short-title: JSON
slug: Learn_web_development/Core/Scripting/JSON
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Network_requests","Learn_web_development/Core/Scripting/Debugging_JavaScript", "Learn_web_development/Core/Scripting")}}

JavaScript Object Notation (JSON) è un formato standard basato su testo per rappresentare i dati strutturati basandosi sulla sintassi degli oggetti JavaScript. È comunemente usato per trasmettere dati nelle applicazioni web (ad es., inviare alcuni dati dal server al client affinché possano essere visualizzati su una pagina web, o viceversa). Ti imbatterai in JSON piuttosto spesso, quindi in questo articolo ti forniamo tutto ciò di cui hai bisogno per lavorare con JSON usando JavaScript, inclusa l'analisi di JSON per accedere ai dati che contiene, e la creazione di JSON.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è JSON — un formato dati molto comunemente usato basato sulla sintassi degli oggetti JavaScript.</li>
          <li>Che JSON può contenere anche array.</li>
          <li>Recuperare JSON come un oggetto JavaScript usando i meccanismi disponibili nelle Web API (ad esempio, <code>Response.json()</code> nella Fetch API).</li>
          <li>Accedere ai valori all'interno dei dati JSON usando la notazione con parentesi quadre e quella con punto.</li>
          <li>Convertire tra oggetti e testo usando <code>JSON.parse()</code> e <code>JSON.stringify()</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## No, davvero, cos'è JSON?

{{Glossary("JSON", "JSON")}} è un formato di dati basato su testo che segue la sintassi degli oggetti JavaScript.
Rappresenta i dati strutturati come una stringa, il che è utile quando si desidera trasmettere dati attraverso una rete.
Anche se somiglia molto alla sintassi del literale oggetto JavaScript, può essere usato indipendentemente da JavaScript. Molti ambienti di programmazione dispongono della capacità di leggere (parsing) e generare JSON.
In JavaScript, i metodi per l'analisi e la generazione di JSON sono forniti dall'oggetto [`JSON`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON).

> [!NOTE]
> Convertire una stringa in un oggetto nativo è chiamato _deserializzazione_, mentre convertire un oggetto nativo in una stringa, in modo che possa essere trasmesso sulla rete, è chiamato _serializzazione_.

Una stringa JSON può essere conservata in un file proprio, che è essenzialmente solo un file di testo con un'estensione `.json`, e un {{Glossary("MIME_type", "tipo MIME")}} di `application/json`.

### Struttura di JSON

Come descritto sopra, JSON è una stringa il cui formato somiglia molto al formato del literale oggetto JavaScript.
Quello che segue è una stringa JSON valida che rappresenta un oggetto.
Nota come sia anche un `literale oggetto JavaScript` valido — solo con alcune ulteriori [restrizioni di sintassi](#restrizioni_di_sintassi_di_json).

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

Se carichi questo JSON nel tuo programma JavaScript come una stringa, puoi analizzarlo in un normale oggetto e poi accedere ai dati al suo interno usando la stessa notazione punto/parentesi quadre di cui abbiamo parlato nell'articolo [JavaScript object basics](/it/docs/Learn_web_development/Core/Scripting/Object_basics). Ad esempio:

```js
superHeroes.homeTown;
superHeroes.members[1].powers[2];
```

1. Prima, abbiamo il nome della variabile — `superHeroes`.
2. All'interno di essa, vogliamo accedere alla proprietà `members`, quindi usiamo `.members`.
3. `members` contiene un array popolato da oggetti. Vogliamo accedere al secondo oggetto all'interno dell'array, quindi usiamo `[1]`.
4. All'interno di questo oggetto, vogliamo accedere alla proprietà `powers`, quindi usiamo `.powers`.
5. All'interno della proprietà `powers` c'è un array contenente i superpoteri dell'eroe selezionato. Vogliamo il terzo, quindi usiamo `[2]`.

Il punto chiave da prendere è che non c'è davvero nulla di speciale nel lavorare con JSON; dopo averlo analizzato in un oggetto JavaScript, ci lavori proprio come faresti con un oggetto dichiarato usando la stessa sintassi di literale oggetto.

> [!NOTE]
> Abbiamo reso disponibile il JSON visto sopra all'interno di una variabile nel nostro esempio [JSONTest.html](https://mdn.github.io/learning-area/javascript/oojs/json/JSONTest.html) (vedi il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/JSONTest.html)).
> Prova a caricarlo e poi accedi ai dati all'interno della variabile tramite la console JavaScript del tuo browser.

### Array come JSON

Sopra abbiamo menzionato che il testo JSON assomiglia essenzialmente a un oggetto JavaScript all'interno di una stringa.
Possiamo anche convertire gli array da/a JSON. L'esempio qui sotto è un JSON perfettamente valido:

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

Devi accedere agli elementi dell'array (nella sua versione analizzata) partendo con un indice di array, ad esempio `superHeroes[0].powers[0]`.

Il JSON può anche contenere un solo tipo primitivo. Ad esempio, `29`, `"Dan Jukes"`, o `true` sono tutti JSON validi.

### Restrizioni di sintassi di JSON

Come menzionato prima, qualsiasi JSON è un literale JavaScript valido (oggetto, array, numero, ecc.). Il contrario non è vero, però — non tutti i literali oggetto JavaScript sono JSON validi.

- JSON può contenere solo tipi di dati _serializzabili_. Questo significa:
  - Per i primitivi, JSON può contenere literali di stringhe, numeri, `true`, `false`, e `null`. Degno di nota è il fatto che non può contenere `undefined`, `NaN`, o `Infinity`.
  - Per i non primitivi, JSON può contenere literali oggetto e array, ma non funzioni o altri tipi di oggetto, come `Date`, `Set`, e `Map`. Gli oggetti e gli array all'interno di JSON devono contenere ulteriormente tipi di dati JSON validi.
- Le stringhe devono essere racchiuse tra doppi apici, non singoli apici.
- I numeri devono essere scritti in notazione decimale.
- Ogni proprietà di un oggetto deve essere nella forma di `"key": value`. I nomi delle proprietà devono essere literali stringa racchiusi tra doppi apici. La speciale sintassi JavaScript, come i metodi, non è ammessa perché i metodi sono funzioni, e le funzioni non sono tipi di dati JSON validi.
- Gli oggetti e gli array non possono contenere [virgole finali](/it/docs/Web/JavaScript/Reference/Trailing_commas).
- I commenti non sono ammessi in JSON.

Anche una sola virgola o due punti fuori posto possono rendere un file JSON non valido e farlo fallire.
Dovresti essere cauto nel convalidare qualsiasi dato che stai cercando di usare (anche se il JSON generato al computer è meno probabile che includa errori, a condizione che il programma generatore stia funzionando correttamente).
Puoi convalidare JSON usando un'applicazione come [JSONLint](https://jsonlint.com/) o [JSON-validate](https://www.json-validate.com/)

> [!NOTE]
> Ora che hai letto questa sezione, potresti anche voler integrare il tuo apprendimento con il [riesame di JSON](https://scrimba.com/frontend-path-c0j/~0lt?via=mdn) di Scrimba <sup>[_partner di apprendimento di MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>, un tutorial interattivo che fornisce alcune utili indicazioni sulla sintassi di base di JSON e su come visualizzare i dati delle richieste JSON all'interno degli strumenti per sviluppatori del browser.

## Apprendimento attivo: Lavorare attraverso un esempio JSON

Quindi, lavoriamo attraverso un esempio per mostrare come potremmo utilizzare alcuni dati formattati JSON su un sito web.

### Iniziare

Per cominciare, fai copie locali dei nostri file [heroes.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes.html) e [style.css](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/style.css).
Quest'ultimo contiene alcuni semplici CSS per stilizzare la nostra pagina, mentre il primo contiene un HTML del corpo molto semplice, oltre a un elemento {{HTMLElement("script")}} per contenere il codice JavaScript che scriveremo in questo esercizio:

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

Abbiamo reso disponibili i nostri dati JSON sul nostro GitHub, all'URL <https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json>.

Caricheremo il JSON nel nostro script e utilizzeremo alcune manipolazioni del DOM per visualizzarlo, come segue:

![Immagine di un documento intitolato "Super hero squad" (in un elegante font) e sottotitolato "Hometown: Metro City // Formed: 2016". Tre colonne sotto l'intestazione sono intitolate "Molecule Man", "Madame Uppercut", e "Eternal Flame", rispettivamente. Ogni colonna elenca il nome dell'identità segreta dell'eroe, l'età e i superpoteri.](json-superheroes.png)

### Funzione di livello superiore

La funzione di livello superiore appare come segue:

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

Per ottenere il JSON, utilizziamo un'API chiamata [Fetch](/it/docs/Web/API/Fetch_API).
Questa API ci permette di effettuare richieste di rete per recuperare risorse da un server tramite JavaScript (ad esempio, immagini, testo, JSON, persino frammenti HTML), il che significa che possiamo aggiornare piccole sezioni di contenuto senza dover ricaricare l'intera pagina.

Nella nostra funzione, le prime quattro righe utilizzano la Fetch API per recuperare il JSON dal server:

- dichiariamo la variabile `requestURL` per memorizzare l'URL di GitHub
- utilizziamo l'URL per inizializzare un nuovo oggetto [`Request`](/it/docs/Web/API/Request).
- effettuiamo la richiesta di rete utilizzando la funzione [`fetch()`](/it/docs/Web/API/Window/fetch), e questo restituisce un oggetto [`Response`](/it/docs/Web/API/Response)
- recuperiamo la risposta come JSON utilizzando la funzione [`json()`](/it/docs/Web/API/Response/json) dell'oggetto `Response`.

> [!NOTE]
> L'API `fetch()` è **asincrona**. Puoi imparare in dettaglio sulle funzioni asincrone nel nostro [modulo JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS), ma per ora, diremo semplicemente che dobbiamo aggiungere la parola chiave {{jsxref("Statements/async_function", "async")}} prima del nome della funzione che utilizza l'API fetch, e aggiungere la parola chiave {{jsxref("Operators/await", "await")}} prima delle chiamate a qualsiasi funzione asincrona.

Dopo tutto ciò, la variabile `superHeroes` conterrà l'oggetto JavaScript basato sul JSON. Passiamo poi quell'oggetto a due chiamate di funzione — la prima riempie il `<header>` con i dati corretti, mentre la seconda crea una scheda informativa per ogni eroe del team, e la inserisce nel `<section>`.

### Popolare l'intestazione

Ora che abbiamo recuperato i dati JSON e li abbiamo convertiti in un oggetto JavaScript, facciamone uso scrivendo le due funzioni a cui abbiamo fatto riferimento sopra. Prima di tutto, aggiungi la seguente definizione di funzione sotto il codice precedente:

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

Qui creiamo prima un elemento {{HTMLElement("Heading_Elements", "h1")}} con [`createElement()`](/it/docs/Web/API/Document/createElement), impostiamo il suo [`textContent`](/it/docs/Web/API/Node/textContent) per essere uguale alla proprietà `squadName` dell'oggetto, poi lo appende all'intestazione usando [`appendChild()`](/it/docs/Web/API/Node/appendChild). Quindi facciamo un'operazione molto simile con un paragrafo: crearlo, impostare il suo testo e appenderlo all'intestazione. L'unica differenza è che il suo testo è impostato su un [template literal](/it/docs/Web/JavaScript/Reference/Template_literals) contenente sia la proprietà `homeTown` che `formed` dell'oggetto.

### Creare le schede informative degli eroi

Successivamente, aggiungi la seguente funzione alla fine del codice, che crea e visualizza le schede dei supereroi:

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

Per cominciare, memorizziamo la proprietà `members` dell'oggetto JavaScript in una nuova variabile. Questo array contiene più oggetti che contengono le informazioni su ciascun eroe.

Successivamente, usiamo un [ciclo for...of](/it/docs/Learn_web_development/Core/Scripting/Loops#the_for...of_loop) per scorrere ogni oggetto nell'array. Per ciascuno di essi, noi:

1. Creiamo diversi nuovi elementi: un `<article>`, un `<h2>`, tre `<p>`, e un `<ul>`.
2. Impostiamo il `<h2>` per contenere il `name` dell'eroe corrente.
3. Riempiamo i tre paragrafi con il loro `secretIdentity`, `age`, e una riga che dice "Superpowers:" per introdurre le informazioni nella lista.
4. Memorizziamo la proprietà `powers` in un'altra nuova costante chiamata `superPowers` — questa contiene un array che elenca i superpoteri dell'eroe corrente.
5. Usiamo un altro ciclo `for...of` per scorrere i superpoteri dell'attuale eroe — per ciascuno creiamo un elemento `<li>`, mettiamo il superpotere al suo interno, poi mettiamo il `listItem` all'interno dell'elemento `<ul>` (`myList`) usando `appendChild()`.
6. L'ultima cosa che facciamo è appendere `<h2>`, `<p>`, e `<ul>` all'interno di `<article>` (`myArticle`), poi appendere `<article>` all'interno di `<section>`. L'ordine in cui le cose sono appese è importante, poiché questo è l'ordine in cui verranno visualizzate all'interno dell'HTML.

> [!NOTE]
> Se hai difficoltà a far funzionare l'esempio, prova a fare riferimento al nostro [heroes-finished.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes-finished.html) codice sorgente (vedi anche l'[esecuzione live](https://mdn.github.io/learning-area/javascript/oojs/json/heroes-finished.html)).

> [!NOTE]
> Se hai difficoltà a seguire la notazione punto/parentesi quadra che stiamo usando per accedere all'oggetto JavaScript, può essere utile avere il file [superheroes.json](https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json) aperto in un'altra scheda o nel tuo editor di testo e fare riferimento ad esso mentre guardi il nostro JavaScript.
> Dovresti anche fare riferimento al nostro articolo [JavaScript object basics](/it/docs/Learn_web_development/Core/Scripting/Object_basics) per ulteriori informazioni sulla notazione punto e parentesi quadra.

### Chiamare la funzione di livello superiore

Infine, dobbiamo chiamare la nostra funzione `populate()` di livello superiore:

```js
populate();
```

## Convertire tra oggetti e testo

L'esempio sopra era semplice in termini di accesso all'oggetto JavaScript, perché abbiamo convertito la risposta di rete direttamente in un oggetto JavaScript usando `response.json()`.

Ma a volte non siamo così fortunati — a volte riceviamo una stringa JSON grezza, e dobbiamo convertirla in un oggetto noi stessi. E quando vogliamo inviare un oggetto JavaScript attraverso la rete, dobbiamo convertirlo in JSON (una stringa) prima di inviarlo. Fortunatamente, questi due problemi sono così comuni nello sviluppo web che un oggetto [JSON](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON) è disponibile nei browser, che contiene i seguenti due metodi:

- [`parse()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse): Accetta una stringa JSON come parametro e restituisce il corrispondente oggetto JavaScript.
- [`stringify()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify): Accetta un oggetto come parametro e restituisce la stringa JSON equivalente.

Puoi vedere il primo in azione nel nostro esempio [heroes-finished-json-parse.html](https://mdn.github.io/learning-area/javascript/oojs/json/heroes-finished-json-parse.html) (vedi il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes-finished-json-parse.html)) — questo fa esattamente la stessa cosa dell'esempio che abbiamo costruito sopra, eccetto che:

- recuperiamo la risposta come testo invece che come JSON, chiamando il metodo [`text()`](/it/docs/Web/API/Response/text) della risposta
- poi usiamo `parse()` per convertire il testo in un oggetto JavaScript.

Il frammento di codice chiave è qui:

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

Come puoi immaginare, `stringify()` funziona nel modo opposto. Prova a inserire le seguenti righe nella console JavaScript del tuo browser una per una per vederla in azione:

```js
let myObj = { name: "Chris", age: 38 };
myObj;
let myString = JSON.stringify(myObj);
myString;
```

Qui creiamo un oggetto JavaScript, controlliamo cosa contiene, lo convertiamo in una stringa JSON usando `stringify()` — salvando il valore di ritorno in una nuova variabile — poi lo controlliamo di nuovo.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare di aver assimilato queste informazioni prima di andare avanti — vedi [Test your skills: JSON](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/JSON).

## Riepilogo

In questa lezione, ti abbiamo introdotto l'uso di JSON nei tuoi programmi, inclusa la creazione e l'analisi di JSON e l'accesso ai dati bloccati all'interno di esso. Nel prossimo articolo, esamineremo le tecniche pratiche per il debug di JavaScript e la gestione degli errori.

## Vedi anche

- [Riferimento JSON](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [Panoramica della Fetch API](/it/docs/Web/API/Fetch_API)
- [Utilizzo della Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch)
- [Metodi di richiesta HTTP](/it/docs/Web/HTTP/Reference/Methods)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Network_requests","Learn_web_development/Core/Scripting/Debugging_JavaScript", "Learn_web_development/Core/Scripting")}}
