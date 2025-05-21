---
title: Lavorare con JSON
short-title: JSON
slug: Learn_web_development/Core/Scripting/JSON
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Network_requests","Learn_web_development/Core/Scripting/Debugging_JavaScript", "Learn_web_development/Core/Scripting")}}

JavaScript Object Notation (JSON) è un formato standard basato su testo per rappresentare dati strutturati basato sulla sintassi degli oggetti JavaScript. È comunemente utilizzato per la trasmissione di dati nelle applicazioni web (ad esempio, inviando alcuni dati dal server al client, in modo che possano essere visualizzati su una pagina web, o viceversa). Lo incontrerai spesso, quindi in questo articolo ti forniamo tutto ciò che ti serve per lavorare con JSON usando JavaScript, inclusa la lettura di JSON per accedere ai dati al suo interno e la creazione di JSON.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Che cos'è JSON — un formato di dati molto comunemente usato basato sulla sintassi degli oggetti di JavaScript.</li>
          <li>Che JSON può anche contenere array.</li>
          <li>Recuperare JSON come oggetto JavaScript utilizzando i meccanismi disponibili nelle Web API (ad esempio, <code>Response.json()</code> nell'Fetch API).</li>
          <li>Accesso ai valori all'interno di dati JSON usando la sintassi con parentesi quadre e punto.</li>
          <li>Conversione tra oggetti e testo usando <code>JSON.parse()</code> e <code>JSON.stringify()</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## No, davvero, cos'è JSON?

{{Glossary("JSON", "JSON")}} è un formato di dati basato su testo che segue la sintassi degli oggetti JavaScript.
Rappresenta i dati strutturati come una stringa, il che è utile quando vuoi trasmettere dati attraverso una rete.
Anche se somiglia molto alla sintassi letterale degli oggetti JavaScript, può essere utilizzato in modo indipendente da JavaScript. Molti ambienti di programmazione hanno la capacità di leggere (leggere sintatticamente) e generare JSON. In JavaScript, i metodi per leggere sintatticamente e generare JSON sono forniti dall'oggetto [`JSON`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON).

> [!NOTE]
> Convertire una stringa in un oggetto nativo si chiama _deserializzazione_, mentre convertire un oggetto nativo in una stringa in modo che possa essere trasmesso attraverso la rete si chiama _serializzazione_.

Una stringa JSON può essere memorizzata nel proprio file, che è fondamentalmente solo un file di testo con un'estensione `.json` e un {{Glossary("MIME_type", "MIME type")}} di `application/json`.

### Struttura JSON

Come descritto sopra, JSON è una stringa il cui formato assomiglia molto al formato letterale degli oggetti JavaScript.
Ciò che segue è una stringa JSON valida che rappresenta un oggetto.
Nota come sia anche un oggetto letterale JavaScript valido — solo con alcune [restrizioni sintattiche](#restrizioni_sintattiche_del_json).

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

Se carichi questa JSON nel tuo programma JavaScript come una stringa, puoi leggerla sintatticamente in un oggetto normale e quindi accedere ai dati al suo interno usando la stessa notazione con punto/parantesi che abbiamo visto nell'articolo di base sugli [oggetti JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics). Ad esempio:

```js
superHeroes.homeTown;
superHeroes.members[1].powers[2];
```

1. Prima, abbiamo il nome della variabile — `superHeroes`.
2. All'interno di ciò, vogliamo accedere alla proprietà `members`, quindi usiamo `.members`.
3. `members` contiene un array popolato da oggetti. Vogliamo accedere al secondo oggetto all'interno dell'array, quindi usiamo `[1]`.
4. All'interno di questo oggetto, vogliamo accedere alla proprietà `powers`, quindi usiamo `.powers`.
5. All'interno della proprietà `powers` c'è un array che contiene i superpoteri dell'eroe selezionato. Vogliamo il terzo, quindi usiamo `[2]`.

Il punto chiave è che non c'è davvero nulla di speciale nel lavorare con JSON; dopo averlo letto sintatticamente in un oggetto JavaScript, ci lavori proprio come faresti con un oggetto dichiarato utilizzando la stessa sintassi letterale degli oggetti.

> [!NOTE]
> Abbiamo reso disponibile il JSON visto sopra all'interno di una variabile nel nostro esempio di [JSONTest.html](https://mdn.github.io/learning-area/javascript/oojs/json/JSONTest.html) (vedi il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/JSONTest.html)).
> Prova a caricarlo e quindi ad accedere ai dati all'interno della variabile tramite la console JavaScript del tuo browser.

### Array come JSON

Sopra abbiamo menzionato che il testo JSON assomiglia fondamentalmente a un oggetto JavaScript all'interno di una stringa.
Possiamo anche convertire array in/dalla JSON. L'esempio qui sotto è un JSON perfettamente valido:

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

Devi accedere agli elementi dell'array (nella sua versione letta sintatticamente) iniziando con un indice dell'array, ad esempio `superHeroes[0].powers[0]`.

Il JSON può anche contenere un singolo valore primitivo. Ad esempio, `29`, `"Dan Jukes"`, o `true` sono tutti JSON validi.

### Restrizioni sintattiche del JSON

Come accennato in precedenza, qualsiasi JSON è una letterale JavaScript valida (oggetto, array, numero, ecc.). Il contrario non è vero, però: non tutti i letterali di oggetti JavaScript sono JSON validi.

- JSON può contenere solo tipi di dati _serializzabili_. Questo significa:
  - Per i primitivi, JSON può contenere stringhe letterali, numeri letterali, `true`, `false` e `null`. Notabilmente, non può contenere `undefined`, `NaN`, o `Infinity`.
  - Per i non-primativi, JSON può contenere letterali di oggetti e array, ma non funzioni o altri tipi di oggetto, come `Date`, `Set` e `Map`. Gli oggetti e gli array all'interno del JSON devono contenere ulteriormente tipi di dati JSON validi.
- Le stringhe devono essere racchiuse tra virgolette doppie, non tra virgolette singole.
- I numeri devono essere scritti in notazione decimale.
- Ogni proprietà di un oggetto deve essere nella forma `"chiave": valore`. I nomi delle proprietà devono essere stringhe letterali racchiuse tra virgolette doppie. La sintassi speciale di JavaScript, come i metodi, non è permessa perché i metodi sono funzioni, e le funzioni non sono tipi di dati JSON validi.
- Gli oggetti e gli array non possono contenere [virgole finali](/it/docs/Web/JavaScript/Reference/Trailing_commas).
- I commenti non sono ammessi nel JSON.

Anche un singolo apostrofo o due punti fuori posto possono rendere un file JSON non valido e causare il suo fallimento.
Dovresti essere attento a validare qualsiasi dato stai tentando di usare (anche se i JSON generati dal computer sono meno probabili di includere errori, finché il programma generatore sta funzionando correttamente).
Puoi validare il JSON usando un'applicazione come [JSONLint](https://jsonlint.com/) o [JSON-validate](https://www.json-validate.com/)

## Apprendimento attivo: lavorare attraverso un esempio JSON

Quindi, vediamo un esempio per mostrare come potremmo utilizzare alcuni dati in formato JSON su un sito web.

### Per cominciare

Per iniziare, fai copie locali dei nostri file [heroes.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes.html) e [style.css](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/style.css).
Quest'ultima contiene un po' di CSS semplice per stilizzare la nostra pagina, mentre la prima contiene del corpo HTML molto semplice, oltre a un elemento {{HTMLElement("script")}} per contenere il codice JavaScript che scriveremo in questo esercizio:

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

Abbiamo reso disponibile i nostri dati JSON su GitHub, a <https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json>.

Caricheremo il JSON nel nostro script e utilizzeremo alcune operazioni DOM avanzate per visualizzarlo, come questo:

![Immagine di un documento intitolato "Super hero squad" (in un font fantasioso) e sottotitolato "Hometown: Metro City // Formed: 2016". Tre colonne sotto l'intestazione sono intitolate "Molecule Man", "Madame Uppercut" e "Eternal Flame", rispettivamente. Ogni colonna elenca il nome dell'identità segreta dell'eroe, l'età e i superpoteri.](json-superheroes.png)

### Funzione di livello superiore

La funzione di livello superiore appare così:

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

Per ottenere il JSON, usiamo un'API chiamata [Fetch](/it/docs/Web/API/Fetch_API).
Questa API ci permette di fare richieste di rete per recuperare risorse da un server tramite JavaScript (ad esempio, immagini, testo, JSON, persino frammenti HTML), il che significa che possiamo aggiornare piccole sezioni di contenuto senza dover ricaricare l'intera pagina.

Nella nostra funzione, le prime quattro righe usano l'API Fetch per ottenere il JSON dal server:

- dichiariamo la variabile `requestURL` per memorizzare l'URL di GitHub
- usiamo l'URL per inizializzare un nuovo oggetto [`Request`](/it/docs/Web/API/Request).
- facciamo la richiesta di rete usando la funzione [`fetch()`](/it/docs/Web/API/Window/fetch), e questo restituisce un oggetto [`Response`](/it/docs/Web/API/Response)
- recuperiamo la risposta come JSON usando la funzione [`json()`](/it/docs/Web/API/Response/json) dell'oggetto `Response`.

> [!NOTE]
> L'API `fetch()` è **asincrona**. Puoi apprendere dettagli sulle funzioni asincrone nel nostro modulo [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS), ma per ora diremo solo che dobbiamo aggiungere la parola chiave {{jsxref("Statements/async_function", "async")}} prima del nome della funzione che usa l'API fetch, e aggiungere la parola chiave {{jsxref("Operators/await", "await")}} prima delle chiamate a qualsiasi funzione asincrona.

Dopo tutto ciò, la variabile `superHeroes` conterrà l'oggetto JavaScript basato sul JSON. Quindi passiamo quell'oggetto a due chiamate di funzione — la prima riempie l'`<header>` con i dati corretti, mentre la seconda crea una scheda informativa per ogni eroe della squadra e la inserisce nel `<section>`.

### Popolando l'intestazione

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

Qui creiamo prima un elemento {{HTMLElement("Heading_Elements", "h1")}} con [`createElement()`](/it/docs/Web/API/Document/createElement), ne impostiamo il [`textContent`](/it/docs/Web/API/Node/textContent) per uguale alla proprietà `squadName` dell'oggetto, quindi lo aggiungiamo all'intestazione usando [`appendChild()`](/it/docs/Web/API/Node/appendChild). Facciamo quindi una operazione molto simile con un paragrafo: crearlo, impostare il suo contenuto di testo e aggiungerlo all'intestazione. L'unica differenza è che il suo testo è impostato come un [template literal](/it/docs/Web/JavaScript/Reference/Template_literals) contenente sia le proprietà `homeTown` che `formed` dell'oggetto.

### Creare le schede informative degli eroi

Successivamente, aggiungi la seguente funzione in fondo al codice, che crea e visualizza le carte dei supereroi:

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

Per iniziare, memorizziamo la proprietà `members` dell'oggetto JavaScript in una nuova variabile. Questo array contiene più oggetti che contengono le informazioni per ciascun eroe.

Successivamente, usiamo un [ciclo for...of](/it/docs/Learn_web_development/Core/Scripting/Loops#the_for...of_loop) per iterare attraverso ciascun oggetto nell'array. Per ciascuno di essi, noi:

1. Creiamo diversi nuovi elementi: un `<article>`, un `<h2>`, tre `<p>` e un `<ul>`.
2. Impostiamo l`<h2>` per contenere il `name` dell'eroe attuale.
3. Riempiamo i tre paragrafi con i loro `secretIdentity`, `age`, e una riga che dice "Superpoteri:" per introdurre le informazioni nella lista.
4. Memorizziamo la proprietà `powers` in un'altra nuova costante chiamata `superPowers` — questa contiene un array che elenca i superpoteri dell'eroe attuale.
5. Usiamo un altro ciclo `for...of` per iterare attraverso i superpoteri dell'eroe attuale — per ciascuno creiamo un elemento `<li>`, mettiamo il superpotere all'interno, quindi mettiamo l'`listItem` all'interno dell'elemento `<ul>` (`myList`) usando `appendChild()`.
6. Alla fine, appendiamo l'`<h2>`, i `<p>`, e l'`<ul>` all'interno dell'`<article>` (`myArticle`), poi appendiamo l`<article>` all'interno del `<section>`. L'ordine in cui le cose sono appendate è importante, dato che questo è l'ordine in cui verranno visualizzate all'interno dell'HTML.

> [!NOTE]
> Se stai avendo difficoltà a far funzionare l'esempio, prova a fare riferimento al nostro codice sorgente di [heroes-finished.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes-finished.html) (lo puoi vedere [in esecuzione live](https://mdn.github.io/learning-area/javascript/oojs/json/heroes-finished.html) anche.)

> [!NOTE]
> Se hai difficoltà a seguire la notazione dot/bracket che stiamo usando per accedere all'oggetto JavaScript, può aiutare avere il file [superheroes.json](https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json) aperto in un'altra scheda o nel tuo editor di testo, e fare riferimento a esso mentre osservi la nostra JavaScript. 
> Dovresti anche riferirti all'articolo [basi degli oggetti JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics) per ulteriori informazioni sulla notazione con dot e bracket.

### Chiamare la funzione di livello superiore

Infine, dobbiamo chiamare la nostra funzione `populate()` di livello superiore:

```js
populate();
```

## Conversione tra oggetti e testo

L'esempio sopra era semplice in termini di accesso all'oggetto JavaScript, perché abbiamo convertito la risposta di rete direttamente in un oggetto JavaScript usando `response.json()`.

Ma a volte non siamo così fortunati — a volte riceviamo una stringa JSON grezza, e dobbiamo convertirla noi stessi in un oggetto. E quando vogliamo inviare un oggetto JavaScript attraverso la rete, dobbiamo convertirlo in JSON (una stringa) prima di inviarlo. Fortunatamente, questi due problemi sono così comuni nello sviluppo web che un oggetto [JSON](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON) integrato è disponibile nei browser, il quale contiene i seguenti due metodi:

- [`parse()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse): Accetta una stringa JSON come parametro, e restituisce l'oggetto JavaScript corrispondente.
- [`stringify()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify): Accetta un oggetto come parametro, e restituisce la stringa JSON equivalente.

Puoi vederlo in azione nel nostro esempio [heroes-finished-json-parse.html](https://mdn.github.io/learning-area/javascript/oojs/json/heroes-finished-json-parse.html) (vedi il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes-finished-json-parse.html)) — fa esattamente la stessa cosa dell'esempio che abbiamo costruito in precedenza, tranne per il fatto che:

- recuperiamo la risposta come testo anziché JSON, chiamando il metodo [`text()`](/it/docs/Web/API/Response/text) della risposta
- usiamo poi `parse()` per convertire il testo in un oggetto JavaScript.

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

Come puoi immaginare, `stringify()` funziona nel modo opposto. Prova a inserire le seguenti righe nella console JavaScript del tuo browser una alla volta per vederlo in azione:

```js
let myObj = { name: "Chris", age: 38 };
myObj;
let myString = JSON.stringify(myObj);
myString;
```

Qui stiamo creando un oggetto JavaScript, poi controlliamo cosa contiene, quindi lo convertiamo in una stringa JSON usando `stringify()` — salvando il valore di ritorno in una nuova variabile — e poi lo controlliamo di nuovo.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver conservato queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: JSON](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/JSON).

## Sommario

In questa lezione, ti abbiamo introdotto all'uso di JSON nei tuoi programmi, includendo come creare e leggere sintatticamente JSON, e come accedere ai dati bloccati al suo interno. Nel prossimo articolo, esamineremo tecniche pratiche per il debug di JavaScript e la gestione degli errori.

## Vedi anche

- [Riferimento JSON](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [Panoramica dell'API Fetch](/it/docs/Web/API/Fetch_API)
- [Usare Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch)
- [Metodi di richiesta HTTP](/it/docs/Web/HTTP/Reference/Methods)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Network_requests","Learn_web_development/Core/Scripting/Debugging_JavaScript", "Learn_web_development/Core/Scripting")}}
