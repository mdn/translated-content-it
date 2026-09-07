---
title: Lavorare con JSON
short-title: JSON
slug: Learn_web_development/Core/Scripting/JSON
l10n:
  sourceCommit: 65692fd4d256d5647749b7c7005dcf53d425a533
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Network_requests","Learn_web_development/Core/Scripting/Test_your_skills/JSON", "Learn_web_development/Core/Scripting")}}

JavaScript Object Notation (JSON) è un formato standard basato su testo per rappresentare dati strutturati, basato sulla sintassi degli oggetti JavaScript. Viene comunemente usato per trasmettere dati nelle applicazioni web (ad esempio, per inviare dati dal server al client affinché possano essere visualizzati in una pagina web, o viceversa). Lo si incontrerà molto spesso; in questo articolo viene fornito tutto ciò che serve per lavorare con JSON usando JavaScript, incluso l'analisi di JSON per poter accedere ai dati al suo interno e la creazione di JSON.

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
          <li>Che cos'è JSON: un formato di dati molto comune basato sulla sintassi degli oggetti JavaScript.</li>
          <li>Che JSON può contenere anche array.</li>
          <li>Recuperare JSON come oggetto JavaScript usando i meccanismi disponibili nelle Web API (ad esempio, <code>Response.json()</code> nella Fetch API).</li>
          <li>Accedere ai valori nei dati JSON usando la sintassi con parentesi quadre e punto.</li>
          <li>Convertire tra oggetti e testo usando <code>JSON.parse()</code> e <code>JSON.stringify()</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## No, davvero, che cos'è JSON?

{{Glossary("JSON", "JSON")}} è un formato di dati basato su testo che segue la sintassi degli oggetti JavaScript.
Rappresenta dati strutturati come una stringa, utile quando si desidera trasmettere dati attraverso una rete.
Sebbene assomigli molto alla sintassi letterale degli oggetti JavaScript, può essere usato indipendentemente da JavaScript. Molti ambienti di programmazione offrono la possibilità di leggere (analizzare) e generare JSON.
In JavaScript, i metodi per analizzare e generare JSON sono forniti dall'oggetto [`JSON`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON).

> [!NOTE]
> La conversione di una stringa in un oggetto nativo è chiamata _deserializzazione_, mentre la conversione di un oggetto nativo in una stringa affinché possa essere trasmessa attraverso la rete è chiamata _serializzazione_.

Una stringa JSON può essere archiviata in un proprio file, che è essenzialmente un file di testo con estensione `.json` e {{Glossary("MIME_type", "tipo MIME")}} `application/json`.

### Struttura JSON

Come descritto sopra, JSON è una stringa il cui formato assomiglia molto al formato letterale degli oggetti JavaScript.
Quella seguente è una stringa JSON valida che rappresenta un oggetto.
Si noti che è anche un letterale di oggetto JavaScript valido, ma con alcune ulteriori [restrizioni sintattiche](#restrizioni_sintattiche_json).

<!-- cSpell:ignore tonne -->

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

Se si carica questo JSON nel programma JavaScript come stringa, è possibile analizzarlo in un normale oggetto e quindi accedere ai dati al suo interno usando la stessa notazione con punto/parentesi quadre esaminata nell'articolo sulle [basi degli oggetti JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics).
Ad esempio:

```js
superHeroes.homeTown;
superHeroes.members[1].powers[2];
```

1. Innanzitutto, c'è il nome della variabile: `superHeroes`.
2. Al suo interno, si desidera accedere alla proprietà `members`, quindi si usa `.members`.
3. `members` contiene un array popolato da oggetti. Si desidera accedere al secondo oggetto all'interno dell'array, quindi si usa `[1]`.
4. All'interno di questo oggetto, si desidera accedere alla proprietà `powers`, quindi si usa `.powers`.
5. All'interno della proprietà `powers` si trova un array contenente i superpoteri dell'eroe selezionato. Si desidera il terzo, quindi si usa `[2]`.

L'aspetto fondamentale è che non c'è davvero nulla di speciale nel lavorare con JSON: dopo averlo analizzato in un oggetto JavaScript, lo si usa proprio come un oggetto dichiarato usando la stessa sintassi letterale degli oggetti.

> [!NOTE]
> Il JSON mostrato sopra è stato reso disponibile all'interno di una variabile nell'esempio [JSONTest.html](https://mdn.github.io/learning-area/javascript/oojs/json/JSONTest.html) (vedere il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/JSONTest.html)).
> Provare a caricarlo e quindi ad accedere ai dati all'interno della variabile tramite la console JavaScript del browser.

### Array come JSON

In precedenza è stato menzionato che il testo JSON assomiglia essenzialmente a un oggetto JavaScript all'interno di una stringa.
È inoltre possibile convertire array da e verso JSON. L'esempio seguente è JSON perfettamente valido:

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

È necessario accedere agli elementi dell'array (nella sua versione analizzata) iniziando con un indice dell'array, ad esempio `superHeroes[0].powers[0]`.

JSON può contenere anche un singolo valore primitivo. Ad esempio, `29`, `"Dan Jukes"` e `true` sono tutti JSON validi.

### Restrizioni sintattiche JSON

Come menzionato in precedenza, qualsiasi JSON è un letterale JavaScript valido (oggetto, array, numero e così via). Il contrario non è però vero: non tutti i letterali di oggetti JavaScript sono JSON validi.

- JSON può contenere solo tipi di dati _serializzabili_. Questo significa:
  - Per i valori primitivi, JSON può contenere letterali stringa, letterali numerici, `true`, `false` e `null`. In particolare, non può contenere `undefined`, `NaN` o `Infinity`.
  - Per i valori non primitivi, JSON può contenere letterali di oggetto e array, ma non funzioni né altri tipi di oggetto, come `Date`, `Set` e `Map`. Gli oggetti e gli array all'interno di JSON devono inoltre contenere tipi di dati JSON validi.
- Le stringhe devono essere racchiuse tra virgolette doppie, non tra virgolette singole.
- I numeri devono essere scritti in notazione decimale.
- Ogni proprietà di un oggetto deve essere nella forma `"key": value`. I nomi delle proprietà devono essere letterali stringa racchiusi tra virgolette doppie. La sintassi JavaScript speciale, come i metodi, non è consentita perché i metodi sono funzioni e le funzioni non sono tipi di dati JSON validi.
- Gli oggetti e gli array non possono contenere [virgole finali](/it/docs/Web/JavaScript/Reference/Trailing_commas).
- I commenti non sono consentiti in JSON.

Anche una singola virgola o due punti posizionati in modo errato possono rendere non valido un file JSON e causarne il fallimento.
Occorre prestare attenzione a convalidare tutti i dati che si tenta di usare, anche se è meno probabile che JSON generato da computer includa errori, purché il programma generatore funzioni correttamente.
È possibile convalidare JSON usando un'applicazione come [JSONLint](https://jsonlint.com/) o [JSON-validate](https://www.json-validate.com/)

> [!NOTE]
> Dopo aver letto questa sezione, si potrebbe voler integrare l'apprendimento con il tutorial interattivo [ripasso di JSON](https://scrimba.com/frontend-path-c0j/~0lt?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>, che offre indicazioni utili sulla sintassi JSON di base e su come visualizzare i dati delle richieste JSON negli strumenti di sviluppo del browser.

## Analisi di un esempio JSON

Vediamo quindi un esempio per mostrare come usare dati formattati in JSON in un sito web.

### Per iniziare

Per cominciare, creare copie locali dei file [heroes.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes.html) e [style.css](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/style.css).
Il secondo contiene del semplice CSS per applicare lo stile alla pagina, mentre il primo contiene un HTML del `body` molto semplice, oltre a un elemento {{HTMLElement("script")}} che conterrà il codice JavaScript scritto in questo esercizio:

```html-nolint
<header>
...
</header>

<section>
...
</section>

<script>
// JavaScript goes here
</script>
```

I dati JSON sono disponibili su GitHub, all'indirizzo <https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json>.

Verrà caricato il JSON nello script e verranno usate alcune ingegnose manipolazioni del DOM per visualizzarlo, in questo modo:

![Immagine di un documento intitolato "Super hero squad" (con un carattere elaborato) e sottotitolato "Hometown: Metro City // Formed: 2016". Sotto l'intestazione, tre colonne sono intitolate rispettivamente "Molecule Man", "Madame Uppercut" e "Eternal Flame". Ogni colonna elenca il nome dell'identità segreta dell'eroe, l'età e i superpoteri.](json-superheroes.png)

### Funzione di livello superiore

La funzione di livello superiore è simile a questa:

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

Per ottenere il JSON, viene usata un'API chiamata [Fetch](/it/docs/Web/API/Fetch_API).
Questa API consente di effettuare richieste di rete per recuperare risorse da un server tramite JavaScript, come immagini, testo, JSON e perfino frammenti HTML. Ciò permette di aggiornare piccole sezioni di contenuto senza dover ricaricare l'intera pagina.

Nella funzione, le prime quattro righe usano la Fetch API per recuperare il JSON dal server:

- viene dichiarata la variabile `requestURL` per memorizzare l'URL di GitHub
- l'URL viene usato per inizializzare un nuovo oggetto [`Request`](/it/docs/Web/API/Request)
- viene effettuata la richiesta di rete usando la funzione [`fetch()`](/it/docs/Web/API/Window/fetch), che restituisce un oggetto [`Response`](/it/docs/Web/API/Response)
- la risposta viene recuperata come JSON usando la funzione [`json()`](/it/docs/Web/API/Response/json) dell'oggetto `Response`.

> [!NOTE]
> L'API `fetch()` è **asincrona**. È possibile approfondire le funzioni asincrone nel modulo [JavaScript asincrono](/it/docs/Learn_web_development/Extensions/Async_JS), ma per il momento basta dire che occorre aggiungere la parola chiave {{jsxref("Statements/async_function", "async")}} prima del nome della funzione che usa l'API fetch, e aggiungere la parola chiave {{jsxref("Operators/await", "await")}} prima delle chiamate a qualsiasi funzione asincrona.

Dopo tutto questo, la variabile `superHeroes` conterrà l'oggetto JavaScript basato sul JSON. L'oggetto viene quindi passato a due chiamate di funzione: la prima riempie `<header>` con i dati corretti, mentre la seconda crea una scheda informativa per ogni eroe della squadra e la inserisce in `<section>`.

### Popolare l'intestazione

Ora che i dati JSON sono stati recuperati e convertiti in un oggetto JavaScript, è possibile usarli scrivendo le due funzioni a cui si è fatto riferimento sopra. Prima di tutto, aggiungere la seguente definizione di funzione sotto il codice precedente:

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

Qui viene prima creato un elemento {{HTMLElement("Heading_Elements", "h1")}} con [`createElement()`](/it/docs/Web/API/Document/createElement), viene impostato il relativo [`textContent`](/it/docs/Web/API/Node/textContent) affinché sia uguale alla proprietà `squadName` dell'oggetto, quindi l'elemento viene aggiunto all'intestazione usando [`appendChild()`](/it/docs/Web/API/Node/appendChild). Viene poi eseguita un'operazione molto simile con un paragrafo: viene creato, ne viene impostato il contenuto testuale e viene aggiunto all'intestazione. L'unica differenza è che il testo è impostato come un [template literal](/it/docs/Web/JavaScript/Reference/Template_literals) contenente sia la proprietà `homeTown` sia la proprietà `formed` dell'oggetto.

### Creare le schede informative degli eroi

Successivamente, aggiungere la seguente funzione alla fine del codice, che crea e visualizza le schede dei supereroi:

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

Per iniziare, la proprietà `members` dell'oggetto JavaScript viene memorizzata in una nuova variabile. Questo array contiene più oggetti con le informazioni per ciascun eroe.

Successivamente, viene usato un [ciclo `for...of`](/it/docs/Learn_web_development/Core/Scripting/Loops#the_for...of_loop) per iterare attraverso ogni oggetto nell'array. Per ciascuno:

1. Vengono creati diversi nuovi elementi: un `<article>`, un `<h2>`, tre `<p>` e un `<ul>`.
2. Il contenuto di `<h2>` viene impostato sul valore `name` dell'eroe corrente.
3. I tre paragrafi vengono riempiti con `secretIdentity`, `age` e una riga con il testo "Superpoteri:" per introdurre le informazioni nella lista.
4. La proprietà `powers` viene memorizzata in un'altra nuova costante chiamata `superPowers`, che contiene un array con l'elenco dei superpoteri dell'eroe corrente.
5. Viene usato un altro ciclo `for...of` per scorrere i superpoteri dell'eroe corrente: per ciascuno viene creato un elemento `<li>`, viene inserito il superpotere al suo interno e quindi `listItem` viene inserito nell'elemento `<ul>` (`myList`) usando `appendChild()`.
6. Infine, `<h2>`, `<p>` e `<ul>` vengono aggiunti all'interno di `<article>` (`myArticle`), quindi `<article>` viene aggiunto all'interno di `<section>`. L'ordine in cui gli elementi vengono aggiunti è importante, poiché è l'ordine in cui verranno visualizzati nell'HTML.

> [!NOTE]
> In caso di difficoltà nel far funzionare l'esempio, provare a fare riferimento al codice sorgente di [heroes-finished.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes-finished.html) (è disponibile anche [in esecuzione](https://mdn.github.io/learning-area/javascript/oojs/json/heroes-finished.html)).

> [!NOTE]
> In caso di difficoltà nel seguire la notazione con punto/parentesi quadre usata per accedere all'oggetto JavaScript, può essere utile aprire il file [superheroes.json](https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json) in un'altra scheda o nell'editor di testo e farvi riferimento mentre si osserva il JavaScript.
> Per ulteriori informazioni sulla notazione con punto e parentesi quadre, consultare anche l'articolo sulle [basi degli oggetti JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics).

### Chiamare la funzione di livello superiore

Infine, è necessario chiamare la funzione di livello superiore `populate()`:

```js
populate();
```

## Conversione tra oggetti e testo

L'esempio precedente era semplice per quanto riguarda l'accesso all'oggetto JavaScript, perché la risposta di rete è stata convertita direttamente in un oggetto JavaScript usando `response.json()`.

Ma non sempre si è così fortunati: talvolta si riceve una stringa JSON non elaborata ed è necessario convertirla manualmente in un oggetto. E quando si desidera inviare un oggetto JavaScript attraverso la rete, occorre convertirlo in JSON, ovvero una stringa, prima di inviarlo. Fortunatamente, questi due problemi sono così comuni nello sviluppo web che nei browser è disponibile un oggetto [JSON](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON) integrato, che contiene i seguenti due metodi:

- [`parse()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse): accetta una stringa JSON come parametro e restituisce il corrispondente oggetto JavaScript.
- [`stringify()`](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify): accetta un oggetto come parametro e restituisce la stringa JSON equivalente.

È possibile vedere il primo in azione nell'esempio [heroes-finished-json-parse.html](https://mdn.github.io/learning-area/javascript/oojs/json/heroes-finished-json-parse.html) (vedere il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/oojs/json/heroes-finished-json-parse.html)): esegue esattamente la stessa operazione dell'esempio costruito in precedenza, tranne che:

- la risposta viene recuperata come testo anziché come JSON, chiamando il metodo [`text()`](/it/docs/Web/API/Response/text) della risposta
- viene quindi usato `parse()` per convertire il testo in un oggetto JavaScript.

Il frammento di codice principale è il seguente:

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

Come si può intuire, `stringify()` funziona al contrario. Provare a inserire le seguenti righe nella console JavaScript del browser, una alla volta, per vederlo in azione:

```js
let myObj = { name: "Chris", age: 38 };
myObj;
let myString = JSON.stringify(myObj);
myString;
```

Qui viene creato un oggetto JavaScript, viene controllato cosa contiene, viene convertito in una stringa JSON usando `stringify()` — salvando il valore restituito in una nuova variabile — e quindi viene controllato di nuovo.

## Riepilogo

In questa lezione è stato introdotto l'uso di JSON nei programmi, incluso come creare e analizzare JSON e come accedere ai dati contenuti al suo interno. Nel prossimo articolo verranno proposti alcuni test per verificare quanto bene siano state comprese e memorizzate tutte queste informazioni.

## Vedere anche

- [Riferimento JSON](/it/docs/Web/JavaScript/Reference/Global_Objects/JSON)
- [Panoramica della Fetch API](/it/docs/Web/API/Fetch_API)
- [Usare Fetch](/it/docs/Web/API/Fetch_API/Using_Fetch)
- [Metodi di richiesta HTTP](/it/docs/Web/HTTP/Reference/Methods)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Network_requests","Learn_web_development/Core/Scripting/Test_your_skills/JSON", "Learn_web_development/Core/Scripting")}}
