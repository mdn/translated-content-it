---
title: "Express Tutorial Parte 3: Utilizzare un Database (con Mongoose)"
short-title: "3: Utilizzare i database con Mongoose"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo articolo introduce brevemente i database e come usarli con le app Node/Express. Segue poi una spiegazione su come possiamo utilizzare [Mongoose](https://mongoosejs.com/) per fornire accesso al database per il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Viene spiegato come sono dichiarati gli schema degli oggetti e i modelli, i principali tipi di campi e la convalida di base. Mostra anche brevemente alcuni dei principali modi in cui è possibile accedere ai dati del modello.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website">Express Tutorial Parte 2: Creare un sito web scheletro</a>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Essere in grado di progettare e creare i propri modelli usando Mongoose.</td>
    </tr>
  </tbody>
</table>

## Panoramica

Il personale della biblioteca utilizzerà il sito web Local Library per memorizzare informazioni su libri e prestatori, mentre i membri della biblioteca lo useranno per sfogliare e cercare libri, scoprire se sono disponibili delle copie e quindi prenotarle o prenderle in prestito. Per memorizzare e recuperare informazioni in modo efficiente, le memorizzeremo in un _database_.

Le app Express possono utilizzare diversi database e ci sono diversi approcci che si possono utilizzare per eseguire operazioni di **C**reazione, **R**icerca, **U**pdate e **D**elete (CRUD). Questo tutorial fornisce una breve panoramica di alcune delle opzioni disponibili e poi prosegue mostrando in dettaglio i meccanismi particolari selezionati.

### Quali database posso utilizzare?

Le app _Express_ possono utilizzare qualsiasi database supportato da _Node_ (Express stesso non definisce alcun comportamento/requisito aggiuntivo specifico per la gestione dei database). Ci sono [molte opzioni popolari](https://expressjs.com/en/guide/database-integration.html), tra cui PostgreSQL, MySQL, Redis, SQLite e MongoDB.

Quando si sceglie un database, bisogna considerare aspetti come il tempo necessario per diventare produttivi/curva di apprendimento, le prestazioni, la facilità di replicazione/backup, i costi, il supporto della comunità, ecc. Sebbene non ci sia un'unica "migliore" opzione di database, quasi tutte le soluzioni popolari dovrebbero essere più che accettabili per un sito di piccole o medie dimensioni come la nostra Local Library.

Per maggiori informazioni sulle opzioni, consulta [Integrazione del database](https://expressjs.com/en/guide/database-integration.html) (documentazione di Express).

### Qual è il modo migliore per interagire con un database?

Ci sono due approcci comuni per interagire con un database:

- Utilizzare il linguaggio di query nativo dei database, come SQL.
- Utilizzare un Object Relational Mapper ("ORM") o Object Document Mapper ("ODM"). Questi rappresentano i dati del sito web come oggetti JavaScript, che vengono poi mappati nel database sottostante. Alcuni ORM e ODM sono legati a un database specifico, mentre altri forniscono un backend agnostico rispetto al database.

Le migliori _prestazioni_ possono essere ottenute utilizzando SQL o qualsiasi linguaggio di query supportato dal database. I mapper degli oggetti sono spesso più lenti perché utilizzano il codice di traduzione per il mapping tra oggetti e il formato del database, che potrebbe non utilizzare le query di database più efficienti (questo è particolarmente vero se il mapper supporta backend diversi per i database e deve fare compromessi più grandi in termini di supporto delle funzionalità del database).

Il vantaggio di utilizzare un ORM/ODM è che i programmatori possono continuare a pensare in termini di oggetti JavaScript piuttosto che in termini di semantica del database — questo è particolarmente vero se è necessario lavorare con database diversi (sullo stesso sito o su siti diversi). Forniscono anche un luogo ovvio per eseguire la convalida dei dati.

> [!NOTE]
> L'uso di ODM/ORMs spesso si traduce in costi di sviluppo e manutenzione inferiori! A meno che non si conosca molto bene il linguaggio di query nativo o le prestazioni siano fondamentali, dovresti considerare fortemente l'utilizzo di un ODM.

### Quale ORM/ODM dovrei usare?

Ci sono molte soluzioni ODM/ORM disponibili sul sito gestore di pacchetti npm (dai un'occhiata ai tag [odm](https://www.npmjs.com/search?q=keywords:odm) e [orm](https://www.npmjs.com/search?q=keywords:orm) per un sottoinsieme!).

Alcune soluzioni che erano popolari al momento della stesura sono:

- [Mongoose](https://www.npmjs.com/package/mongoose): Mongoose è uno strumento di modellazione degli oggetti di [MongoDB](https://www.mongodb.com/) progettato per funzionare in un ambiente asincrono.
- [Waterline](https://www.npmjs.com/package/waterline): Un ORM estratto dal framework web basato su Express [Sails](https://sailsjs.com/). Fornisce un'API uniforme per accedere a numerosi database diversi, tra cui Redis, MySQL, LDAP, MongoDB e Postgres.
- [Bookshelf](https://www.npmjs.com/package/bookshelf): Offre interfacce sia basate su promise che su callback tradizionali, fornendo supporto per le transazioni, caricamento di relazioni eager/nested-eager, associazioni polimorfiche e supporto per relazioni uno a uno, uno a molti e molti a molti. Funziona con PostgreSQL, MySQL e SQLite3.
- [Objection](https://www.npmjs.com/package/objection): Rende il più semplice possibile l'uso del pieno potere di SQL e del motore di database sottostante (supporta SQLite3, Postgres e MySQL).
- [Sequelize](https://www.npmjs.com/package/sequelize) è un ORM basato su promise per Node.js e io.js. Supporta i dialetti PostgreSQL, MySQL, MariaDB, SQLite e MSSQL e offre supporto solido per le transazioni, relazioni, replica di lettura e altro.
- [Node ORM2](https://node-orm.readthedocs.io/en/latest/) è un Object Relationship Manager per NodeJS. Supporta MySQL, SQLite e Postgres, aiutando a lavorare con il database utilizzando un approccio orientato agli oggetti.
- [GraphQL](https://graphql.org/): Principalmente un linguaggio di query per RESTful APIs, GraphQL è molto popolare e ha funzionalità disponibili per leggere dati dai database.

Di regola generale, dovresti considerare sia le funzionalità fornite che l'"attività della comunità" (download, contributi, rapporti di bug, qualità della documentazione, ecc.) quando selezioni una soluzione. Al momento della stesura, Mongoose è di gran lunga l'ODM più popolare ed è una scelta ragionevole se stai usando MongoDB per il tuo database.

### Utilizzare Mongoose e MongoDB per LocalLibrary

Per l'esempio della _Local Library_ (e il resto di questo argomento) useremo l'[ODM Mongoose](https://www.npmjs.com/package/mongoose) per accedere ai nostri dati della biblioteca. Mongoose agisce come front end di [MongoDB](https://www.mongodb.com/company/what-is-mongodb), un database [NoSQL](https://en.wikipedia.org/wiki/NoSQL) open source che utilizza un modello di dati orientato ai documenti. Una "collection" di "documenti" in un database MongoDB [è analoga a](https://www.mongodb.com/docs/manual/core/databases-and-collections/) una "tabella" di "righe" in un database relazionale.

Questa combinazione di ODM e database è estremamente popolare nella comunità Node, parzialmente perché l'archiviazione e il sistema di query dei documenti sembrano molto simili a JSON e sono quindi familiari agli sviluppatori JavaScript.

> [!NOTE]
> Non è necessario conoscere MongoDB per utilizzare Mongoose, sebbene parti della [documentazione di Mongoose](https://mongoosejs.com/docs/guide.html) _siano_ più facili da usare e capire se sei già familiare con MongoDB.

Il resto di questo tutorial mostra come definire e accedere agli schema e ai modelli di Mongoose per l'esempio [sito web LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website).

## Progettare i modelli di LocalLibrary

Prima di iniziare a scrivere i modelli, vale la pena dedicare qualche minuto a pensare a quali dati dovremmo memorizzare e le relazioni tra i diversi oggetti.

Sappiamo che dobbiamo memorizzare informazioni sui libri (titolo, riassunto, autore, genere, ISBN) e che potremmo avere più copie disponibili (con id unici a livello globale, stati di disponibilità, ecc.). Potremmo dover memorizzare più informazioni sull'autore oltre al suo nome, e potrebbero esserci più autori con nomi uguali o simili. Vogliamo essere in grado di ordinare le informazioni in base al titolo del libro, all'autore, al genere e alla categoria.

Quando progetti i tuoi modelli, ha senso avere modelli separati per ogni "oggetto" (un gruppo di informazioni correlate). In questo caso, alcuni candidati ovvi per questi modelli sono libri, istanze di libri e autori.

Potresti anche voler usare modelli per rappresentare opzioni di liste di selezione (ad esempio, come una lista a discesa di scelte), piuttosto che codificare le scelte direttamente nel sito web — questo è consigliato quando tutte le opzioni non sono conosciute in anticipo o possono cambiare. Un buon esempio è un genere (ad es., fantasy, fantascienza, ecc.).

Una volta deciso sui nostri modelli e campi, dobbiamo pensare alle relazioni tra di essi.

Con questo in mente, il diagramma di associazione UML qui sotto mostra i modelli che definiremo in questo caso (come riquadri). Come discusso sopra, abbiamo creato modelli per il libro (i dettagli generici del libro), l'istanza del libro (stato delle copie fisiche specifiche del libro disponibili nel sistema) e l'autore. Abbiamo anche deciso di avere un modello per il genere in modo che i valori possano essere creati dinamicamente. Abbiamo deciso di non avere un modello per lo `stato` di `BookInstance` — codificheremo a mano i valori accettabili perché non ci aspettiamo che cambino. All'interno di ciascun riquadro, puoi vedere il nome del modello, i nomi e i tipi dei campi, e anche i metodi e i loro tipi di ritorno.

Il diagramma mostra anche le relazioni tra i modelli, comprese le loro _multiplicità_. Le multiplicità sono i numeri presenti sul diagramma che mostrano i numeri (massimo e minimo) di ciascun modello che possono essere presenti nella relazione. Ad esempio, la linea di collegamento tra i riquadri mostra che `Book` e un `Genre` sono correlati. I numeri vicino al modello `Book` mostrano che un `Genre` deve avere zero o più `Book` (quanti ne vuoi), mentre i numeri sull'altro lato della linea accanto al `Genre` mostrano che un libro può avere zero o più `Genre` associati.

> [!NOTE]
> Come discusso nel nostro [primer su Mongoose](#primer_su_mongoose) più in basso, è spesso meglio avere il campo che definisce la relazione tra i documenti/model in un solo _modello_ (puoi comunque trovare la relazione inversa cercando l'`_id` associato nell'altro modello). Sotto abbiamo scelto di definire la relazione tra `Book`/`Genre` e `Book`/`Author` nello schema Libri, e la relazione tra `Book`/`BookInstance` nello schema `BookInstance`. Questa scelta era un po' arbitraria — avremmo potuto ugualmente avere il campo nell'altro schema.

![Modello Mongoose Library con cardinalità corretta](library_website_-_mongoose_express.png)

> [!NOTE]
> La prossima sezione fornisce un primer di base che spiega come sono definiti e utilizzati i modelli. Mentre lo leggi, considera come costruiremo ciascuno dei modelli nel diagramma sopra.

### Le API dei database sono asincrone

Metodi di database per creare, trovare, aggiornare o eliminare record sono asincroni.
Questo significa che i metodi ritornano immediatamente, e il codice che gestisce il successo o il fallimento del metodo viene eseguito in un momento successivo quando l'operazione è completata.
Altri codici possono essere eseguiti mentre il server attende il completamento dell'operazione del database, quindi il server può rimanere reattivo ad altre richieste.

JavaScript ha una serie di meccanismi per supportare il comportamento asincrono.
Storicamente, JavaScript si basava pesantemente sul passaggio di [funzioni di callback](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing) a metodi asincroni per gestire i casi di successo e errore.
In JavaScript moderno, i callback sono stati in gran parte sostituiti dalle [Promises](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise).
Le Promises sono oggetti che sono (immediatamente) ritornati da un metodo asincrono che rappresenta il suo stato futuro.
Quando l'operazione è completata, l'oggetto promise è "risolto", e restituisce un oggetto che rappresenta il risultato dell'operazione o un errore.

Ci sono due modi principali per utilizzare le promises per eseguire codice quando una promise è risolta, e consigliamo vivamente di leggere [Come usare le promises](/it/docs/Learn_web_development/Extensions/Async_JS/Promises) per una panoramica ad alto livello di entrambi gli approcci.
In questo tutorial, utilizzeremo principalmente [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) per attendere il completamento delle promise all'interno di una [`funzione async`](/it/docs/Web/JavaScript/Reference/Statements/async_function), perché questo porta a un codice asincrono più leggibile e comprensibile.

Il modo in cui funziona questo approccio è che si utilizza la parola chiave `async function` per contrassegnare una funzione come asincrona, e poi all'interno di tale funzione si applica `await` a qualsiasi metodo che ritorna una promise.
Quando la funzione asincrona viene eseguita, la sua operazione viene messa in pausa al primo metodo `await` fino a quando la promise è risolta.
Dal punto di vista del codice circostante, la funzione asincrona ritorna quindi e il codice successivo è in grado di eseguire.
In seguito, quando la promise è risolta, il metodo `await` all'interno della funzione asincrona ritorna con il risultato, o viene generato un errore se la promise è stata rifiutata.
Il codice nella funzione asincrona viene quindi eseguito fino a quando non viene incontrato un altro `await`, a quel punto si fermerà di nuovo, o fino a quando tutto il codice della funzione è stato eseguito.

Puoi vedere come funziona questo nell'esempio qui sotto.
`myFunction()` è una funzione asincrona che viene chiamata all'interno di un blocco [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch).
Quando `myFunction()` viene eseguita, l'esecuzione del codice viene messa in pausa su `methodThatReturnsPromise()` fino a quando la promise è risolta, a quel punto il codice continua su `aFunctionThatReturnsPromise()` e attende di nuovo.
Il codice nel blocco `catch` viene eseguito se viene generato un errore nella funzione asincrona, e questo accadrà se la promise restituita da uno dei metodi è stata rifiutata.

```js
async function myFunction() {
  // …
  await someObject.methodThatReturnsPromise();
  // …
  await aFunctionThatReturnsPromise();
  // …
}

try {
  // …
  myFunction();
  // …
} catch (e) {
  // error handling code
}
```

I metodi asincroni sopra sono eseguiti in sequenza.
Se i met

odi non dipendono l'uno dall'altro, è possibile eseguirli in parallelo e terminare l'intera operazione più rapidamente.
Questo viene fatto utilizzando il metodo [`Promise.all()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise/all), che prende un iterabile di promises come input e ritorna una singola `Promise`.
Questa promise restituita è soddisfatta quando tutte le promises di input sono soddisfatte, con un array di valori di soddisfazione.
Viene rifiutata quando una qualsiasi delle promises di input è rifiutata, con questa prima ragione di rifiuto.

Il codice sotto mostra come funziona.
Innanzitutto, abbiamo due funzioni che restituiscono promises.
Attendiamo entrambe che completino utilizzando la promise restituita da `Promise.all()`.
Una volta completate entrambe, `await` restituisce e l'array dei risultati viene popolato,
la funzione poi continua al successivo `await`, e attende fino a quando la promise restituita da `anotherFunctionThatReturnsPromise()` è risolta.
Dovresti chiamare la `myFunction()` in un blocco `try...catch` per catturare eventuali errori.

```js
async function myFunction() {
  // …
  const [resultFunction1, resultFunction2] = await Promise.all([
    functionThatReturnsPromise1(),
    functionThatReturnsPromise2(),
  ]);
  // …
  await anotherFunctionThatReturnsPromise(resultFunction1);
}
```

Le Promises con `await`/`async` permettono sia un controllo flessibile che "comprensibile" sull'esecuzione asincrona!

## Primer su Mongoose

Questa sezione fornisce una panoramica su come connettere Mongoose a un database MongoDB, come definire uno schema e un modello, e come effettuare query di base.

> [!NOTE]
> Questo primer è fortemente influenzato dal [quick start di Mongoose](https://www.npmjs.com/package/mongoose) su _npm_ e dalla [documentazione ufficiale](https://mongoosejs.com/docs/guide.html).

### Installare Mongoose e MongoDB

Mongoose è installato nel tuo progetto (**package.json**) come qualsiasi altra dipendenza, utilizzando npm.
Per installarlo, utilizza il seguente comando all'interno della tua cartella di progetto:

```bash
npm install mongoose
```

Installare _Mongoose_ aggiunge tutte le sue dipendenze, incluso il driver del database MongoDB, ma non installa MongoDB stesso. Se vuoi installare un server MongoDB, puoi [scaricare gli installer da qui](https://www.mongodb.com/try/download/community) per vari sistemi operativi e installarlo localmente. Puoi anche utilizzare istanze MongoDB basate su cloud.

> [!NOTE]
> Per questo tutorial, useremo il tier gratuito del servizio cloud [MongoDB Atlas](https://www.mongodb.com/), che fornisce il database. È adatto per lo sviluppo e ha senso per il tutorial perché rende "l'installazione" indipendente dal sistema operativo (il _database as a service_ è anche un approccio che potresti utilizzare per il tuo database in produzione).

### Connessione a MongoDB

_Mongoose_ richiede una connessione a un database MongoDB.
Puoi usare `require()` e connetterti a un database ospitato localmente con `mongoose.connect()` come mostrato qui sotto (per il tutorial ci connetteremo invece a un database ospitato su Internet).

```js
// Import the mongoose module
const mongoose = require("mongoose");

// Set `strictQuery: false` to globally opt into filtering by properties that aren't in the schema
// Included because it removes preparatory warnings for Mongoose 7.
// See: https://mongoosejs.com/docs/migrating_to_6.html#strictquery-is-removed-and-replaced-by-strict
mongoose.set("strictQuery", false);

// Define the database URL to connect to.
const mongoDB = "mongodb://127.0.0.1/my_database";

// Wait for database to connect, logging an error if there is a problem
main().catch((err) => console.log(err));
async function main() {
  await mongoose.connect(mongoDB);
}
```

> [!NOTE]
> Come discusso nella sezione [Le API dei database sono asincrone](#le_api_dei_database_sono_asincrone), qui attendiamo sulla promise restituita dal metodo `connect()` all'interno di una funzione `async`.
> Usiamo il gestore `catch()` della promise per gestire eventuali errori quando si cerca di connettersi, ma potremmo anche chiamare `main()` all'interno di un blocco `try...catch`.

Puoi ottenere l'oggetto `Connection` predefinito con `mongoose.connection`.
Se hai bisogno di creare connessioni aggiuntive, puoi usare `mongoose.createConnection()`.
Questo prende la stessa forma di URI di database (con host, database, porta, opzioni, ecc.) di `connect()` e restituisce un oggetto `Connection`).
Nota che `createConnection()` restituisce immediatamente; se devi attendere che la connessione sia stabilita puoi chiamarla con `asPromise()` per restituire una promise (`mongoose.createConnection(mongoDB).asPromise()`).

### Definire e creare modelli

I modelli vengono _definiti_ utilizzando l'interfaccia `Schema`. Lo Schema ti consente di definire i campi memorizzati in ogni documento insieme ai loro requisiti di convalida e valori predefiniti. Inoltre, puoi definire metodi helper statici e di istanza per rendere più facile lavorare con i tuoi tipi di dati, e anche proprietà virtuali che puoi usare come qualsiasi altro campo, ma che non vengono effettivamente memorizzate nel database (ne discuteremo un po' più avanti).

Gli Schema vengono poi "compilati" in modelli usando il metodo `mongoose.model()`. Una volta che hai un modello puoi usarlo per trovare, creare, aggiornare ed eliminare oggetti del tipo dato.

> [!NOTE]
> Ogni modello mappa una _collection_ di _documenti_ nel database MongoDB. I documenti conterranno i campi/tipi di schema definiti nel modello `Schema`.

#### Definire gli schema

Il frammento di codice qui sotto mostra come potresti definire uno schema semplice. Prima usi `require()` per caricare mongoose, poi utilizzi il costruttore Schema per creare una nuova istanza di schema, definendo i vari campi al suo interno nel parametro oggetto del costruttore.

```js
// Require Mongoose
const mongoose = require("mongoose");

// Define a schema
const Schema = mongoose.Schema;

const SomeModelSchema = new Schema({
  a_string: String,
  a_date: Date,
});
```

Nel caso sopra abbiamo solo due campi, una stringa e una data. Nelle sezioni successive, mostreremo alcuni degli altri tipi di campi, la convalida e altri metodi.

#### Creare un modello

I modelli vengono creati dagli schema usando il metodo `mongoose.model()`:

```js
// Define schema
const Schema = mongoose.Schema;

const SomeModelSchema = new Schema({
  a_string: String,
  a_date: Date,
});

// Compile model from schema
const SomeModel = mongoose.model("SomeModel", SomeModelSchema);
```

Il primo argomento è il nome singolare della collection che verrà creata per il tuo modello (Mongoose creerà la collection del database per il modello _SomeModel_ qui sopra), e il secondo argomento è lo schema che vuoi utilizzare per creare il modello.

> [!NOTE]
> Una volta che hai definito le tue classi modello puoi usarle per creare, aggiornare o eliminare record, ed eseguire query per ottenere tutti i record o particolari sottoinsiemi di record. Ti mostreremo come farlo nella sezione [Utilizzare i modelli](#utilizzare_i_modelli), e quando creeremo le nostre viste.

#### Tipi di schema (campi)

Uno schema può avere un numero arbitrario di campi — ciascuno rappresenta un campo nei documenti memorizzati in _MongoDB_.
Un esempio di schema che mostra molti dei tipi di campi comuni e come vengono dichiarati è mostrato di seguito.

```js
const schema = new Schema({
  name: String,
  binary: Buffer,
  living: Boolean,
  updated: { type: Date, default: Date.now() },
  age: { type: Number, min: 18, max: 65, required: true },
  mixed: Schema.Types.Mixed,
  _someId: Schema.Types.ObjectId,
  array: [],
  ofString: [String], // You can also have an array of each of the other types too.
  nested: { stuff: { type: String, lowercase: true, trim: true } },
});
```

La maggior parte dei [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) (i descrittori dopo "type:" o dopo i nomi dei campi) sono autoesplicativi. Le eccezioni sono:

- `ObjectId`: Rappresenta istanze specifiche di un modello nel database. Per esempio, un libro potrebbe utilizzarlo per rappresentare il suo oggetto autore. Questo conterrà effettivamente l'ID univoco (`_id`) per l'oggetto specificato. Possiamo usare il metodo `populate()` per inserire le informazioni associate quando necessario.
- [`Mixed`](https://mongoosejs.com/docs/schematypes.html#mixed): Un tipo di schema arbitrario.
- `[]`: Un array di elementi. È possibile eseguire operazioni sugli array JavaScript su questi modelli (push, pop, unshift, ecc.). Gli esempi sopra mostrano un array di oggetti senza un tipo specificato e un array di oggetti `String`, ma puoi avere un array di qualsiasi tipo di oggetto.

Il codice mostra anche entrambi i modi di dichiarare un campo:

- Nome e tipo del campo come coppia chiave-valore (cioè, come fatto con i campi `name`, `binary` e `living`).
- Nome del campo seguito da un oggetto che definisce il `type`, e qualsiasi altra opzione per il campo. Le opzioni includono cose come:

  - valori predefiniti.
  - validator incorporati (ad es., valori max/min) e funzioni di convalida personalizzate.
  - Se il campo è obbligatorio.
  - Se i campi `String` devono essere impostati automaticamente in minuscolo, maiuscolo o spazi tagliati (ad esempio, `{ type: String, lowercase: true, trim: true }`).

Per maggiori informazioni sulle opzioni consulta [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) (documentazione di Mongoose).

#### Validazione

Mongoose fornisce validator incorporati e personalizzati, e validator sincroni e asincroni. Permette di specificare sia l'intervallo accettabile di valori che il messaggio di errore per il fallimento della validazione in tutti i casi.

I validator incorporati includono:

- Tutti i [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) hanno il validator [required](https://mongoosejs.com/docs/api.html#schematype_SchemaType-required) incorporato. Questo viene utilizzato per specificare se il campo deve essere fornito per salvare un documento.
- I [Numeri](https://mongoosejs.com/docs/api/schemanumber.html) hanno validator [min](<https://mongoosejs.com/docs/api/schemanumber.html#SchemaNumber.prototype.min()>) e [max](<https://mongoosejs.com/docs/api/schemanumber.html#SchemaNumber.prototype.max()>).
- Le [Stringhe](https://mongoosejs.com/docs/api/schemastring.html) hanno:

  - [enum](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.enum()>): specifica l'insieme dei valori consentiti per il campo.
  - [match](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.match()>): specifica un'espressione regolare che la stringa deve corrispondere.
  - [maxLength](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.maxlength()>) e [minLength](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.minlength()>) per la stringa.

L'esempio sotto (leggermente modificato dai documenti di Mongoose) mostra come puoi specificare alcuni dei tipi di validator e i messaggi di errore:

```js
const breakfastSchema = new Schema({
  eggs: {
    type: Number,
    min: [6, "Too few eggs"],
    max: 12,
    required: [true, "Why no eggs?"],
  },
  drink: {
    type: String,
    enum: ["Coffee", "Tea", "Water"],
  },
});
```

Per informazioni complete sulla convalida dei campi, vedi [Validation](https://mongoosejs.com/docs/validation.html) (documentazione di Mongoose).

#### Proprietà virtuali

Le proprietà virtuali sono proprietà dei documenti che è possibile ottenere e impostare ma che non vengono memorizzate in MongoDB. I getter sono utili per formattare o combinare campi, mentre i setter sono utili per scomporre un singolo valore in più valori per la memorizzazione. L'esempio nella documentazione costruisce (e decompone) una proprietà virtuale del nome completo da un campo di nome e cognome, il che è più semplice e pulito rispetto a costruire un nome completo ogni volta che viene usato in un modello.

> [!NOTE]
> Useremo una proprietà virtuale nella libreria per definire un URL univoco per ogni record del modello utilizzando un percorso e il valore `_id` del record.

Per maggiori informazioni consulta [Virtuals](https://mongoosejs.com/docs/guide.html#virtuals) (documentazione di Mongoose).

#### Metodi e helper di query

Uno schema può avere anche [metodi di istanza](https://mongoosejs.com/docs/guide.html#methods), [metodi statici](https://mongoosejs.com/docs/guide.html#statics) e [helper di query](https://mongoosejs.com/docs/guide.html#query-helpers). I metodi di istanza e statici sono simili, ma con la differenza ovvia che un metodo di istanza è associato a un particolare record e ha accesso all'oggetto corrente. Gli helper di query permettono di estendere l'[API del builder di query a concatenazione](https://mongoosejs.com/docs/queries.html) di mongoose (per esempio, permettendo di aggiungere una query "byName" oltre ai metodi `find()`, `findOne()` e `findById()`).

### Utilizzare i modelli

Una volta creato uno schema, puoi usarlo per creare modelli. Il modello rappresenta una collezione di documenti nel database che puoi cercare, mentre le istanze del modello rappresentano i documenti individuali che puoi salvare e recuperare.

Forniamo una breve panoramica qui sotto. Per maggiori informazioni vedi: [Models](https://mongoosejs.com/docs/models.html) (documentazione di Mongoose).

> [!NOTE]
> La creazione, l'aggiornamento, l'eliminazione e l'esecuzione di query sui record sono operazioni asincrone che restituiscono una [promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise).
> Gli esempi qui sotto mostrano solo l'uso dei metodi rilevanti e `await` (cioè, il codice essenziale per utilizzare i metodi).
> La funzione asincrona e il blocco `try...catch` circostanti per catturare gli errori sono omessi per motivi di chiarezza.
> Per ulteriori informazioni sull'uso di `await/async` vedi [Le API dei database sono asincrone](#le_api_dei_database_sono_asincrone) sopra.

#### Creare e modificare documenti

Per creare un record puoi definire un'istanza del modello e poi chiamare [`save()`](https://mongoosejs.com/docs/api/model.html#Model.prototype.save) su di essa.
Gli esempi qui sotto assumono che `SomeModel` sia un modello (con un singolo campo `name`) che abbiamo creato dal nostro schema.

```js
// Create an instance of model SomeModel
const awesome_instance = new SomeModel({ name: "awesome" });

// Save the new model instance asynchronously
await awesome_instance.save();
```

Puoi anche usare [`create()`](https://mongoosejs.com/docs/api/model.html#Model.create) per definire l'istanza del modello nello stesso momento in cui la salvi.
Di seguito creiamo solo un'istanza, ma puoi crearne più di una passandole in un array di oggetti.

```js
await SomeModel.create({ name: "also_awesome" });
```

Ogni modello ha una connessione associata (questa sarà la connessione predefinita quando usi `mongoose.model()`). Puoi creare una nuova connessione e chiamare `.model()` su di essa per creare i documenti su un database diverso.

Puoi accedere ai campi in questo nuovo record usando la sintassi a punto, e modificare i valori. Devi chiamare `save()` o `update()` per memorizzare i valori modificati nel database.

```js
// Access model field values using dot notation
console.log(awesome_instance.name); // should log 'also_awesome'

// Change record by modifying the fields, then calling save().
awesome_instance.name = "New cool name";
await awesome_instance.save();
```

#### Cercare i record

Puoi cercare i record usando metodi di query, specificando le condizioni di query come un documento JSON. Il frammento di codice qui sotto mostra come potresti trovare tutti gli atleti in un database che giocano a tennis, restituendo solo i campi per nome e età dell'atleta. Qui specifichiamo solo un campo di corrispondenza (sport) ma puoi aggiungere più criteri, specificare criteri di espressione regolare o rimuovere le condizioni del tutto per restituire tutti gli atleti.

```js
const Athlete = mongoose.model("Athlete", yourSchema);

// find all athletes who play tennis, returning the 'name' and 'age' fields
const tennisPlayers = await Athlete.find(
  { sport: "Tennis" },
  "name age",
).exec();
```

> [!NOTE]
> È importante ricordare che non trovare alcun risultato non è **un errore** per una ricerca — ma potrebbe essere un caso di fallimento nel contesto della tua applicazione.
> Se la tua applicazione si aspetta che una ricerca trovi un valore, puoi controllare il numero di voci restituite nel risultato.

Le API di query, come [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>), restituiscono una variabile di tipo [Query](https://mongoosejs.com/docs/api/query.html).
È possibile utilizzare un oggetto di query per creare una query in parti prima di eseguirla con il metodo [`exec()`](https://mongoosejs.com/docs/api/query.html#Query.prototype.exec).
`exec()` esegue la query e restituisce una promise che puoi attendere per il risultato.

```js
// find all athletes that play tennis
const query = Athlete.find({ sport: "Tennis" });

// selecting the 'name' and 'age' fields
query.select("name age");

// limit our results to 5 items
query.limit(5);

// sort by age
query.sort({ age: -1 });

// execute the query at a later time
query.exec();
```

Sopra abbiamo definito le condizioni di query nel metodo [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>). Possiamo anche farlo utilizzando una funzione [`where()`](<https://mongoosejs.com/docs/api/model.html#Model.where()>), e possiamo concatenare tutte le parti della nostra query insieme usando l'operatore punto (.) anziché aggiungerle separatamente.
Il frammento di codice sotto è lo stesso della nostra query sopra, con un ulteriore criterio per l'età.

```js
Athlete.find()
  .where("sport")
  .equals("Tennis")
  .where("age")
  .gt(17)
  .lt(50) // Additional where query
  .limit(5)
  .sort({ age: -1 })
  .select("name age")
  .exec();
```

Il metodo [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>) restituisce tutti i record che corrispondono, ma spesso vuoi solo ottenere una corrispondenza. I seguenti metodi eseguono query per un singolo record:

- [`findById()`](<https://mongoosejs.com/docs/api/model.html#Model.findById()>): Trova il documento con l'`id` specificato (ogni documento ha un `id` univoco).
- [`findOne()`](<https://mongoosejs.com/docs/api/model.html#Model.findOne()>): Trova un singolo documento che corrisponde ai criteri specificati.
- [`findByIdAndDelete()`](<https://mongoosejs.com/docs/api/model.html#Model.findByIdAndDelete()>), [`findByIdAndUpdate()`](<https://mongoosejs.com/docs/api/model.html#Model.findByIdAndUpdate()>), [`findOneAndRemove()`](<https://mongoosejs.com/docs/api/model.html#Model.findOneAndRemove()>), [`findOneAndUpdate()`](<https://mongoosejs.com/docs/api/model.html#Model.findOneAndUpdate()>): Trova un singolo documento per `id` o criteri e lo aggiorna o lo rimuove. Queste sono funzioni di convenienza utili per aggiornare e rimuovere record.

> [!NOTE]
> C'è anche un metodo [`countDocuments()`](<https://mongoosejs.com/docs/api/model.html#Model.countDocuments()>) che puoi usare per ottenere il numero di elementi che corrispondono alle condizioni. Questo è utile se vuoi eseguire un conteggio senza effettivamente recuperare i record.

C'è molto di più che puoi fare con le query. Per ulteriori informazioni vedi: [Queries](https://mongoosejs.com/docs/queries.html) (documentazione di Mongoose).

#### Lavorare con i documenti correlati — popolamento

Puoi creare riferimenti da un'istanza di documento/modello a un'altra utilizzando il campo schema `ObjectId`, o da un documento a molti utilizzando un array di `ObjectId`. Il campo memorizza l'id del modello correlato. Se hai bisogno del contenuto effettivo del documento associato, puoi usare il metodo [`populate()`](https://mongoosejs.com/docs/populate.html) in una query per sostituire l'id con i dati effettivi.

Ad esempio, il seguente schema definisce autori e storie.
Ogni autore può avere più storie, che rappresentiamo come un array di `ObjectId`.
Ogni storia può avere un singolo autore.
La proprietà `ref` indica lo schema quale modello può essere assegnato a questo campo.

```js
const mongoose = require("mongoose");

const Schema = mongoose.Schema;

const authorSchema = new Schema({
  name: String,
  stories: [{ type: Schema.Types.ObjectId, ref: "Story" }],
});

const storySchema = new Schema({
  author: { type: Schema.Types.ObjectId, ref: "Author" },
  title: String,
});

const Story = mongoose.model("Story", storySchema);
const Author = mongoose.model("Author", authorSchema);
```

Possiamo salvare i nostri riferimenti al documento correlato assegnando il valore `_id`.
Di seguito creiamo un autore, quindi una storia, e assegniamo l'id dell'autore al campo autore della nostra storia.

```js
const bob = new Author({ name: "Bob Smith" });

await bob.save();

// Bob now exists, so lets create a story
const story = new Story({
  title: "Bob goes sledding",
  author: bob._id, // assign the _id from our author Bob. This ID is created by default!
});

await story.save();
```

> [!NOTE]
> Uno dei grandi vantaggi di questo stile di programmazione è che non dobbiamo complicare il percorso principale del nostro codice con controlli degli errori.
> Se una delle operazioni `save()` fallisce, la promise verrà rifiutata e verrà generato un errore.
> Il nostro codice di gestione degli errori gestisce questo separatamente (di solito in un blocco `catch()`), quindi l'intento del nostro codice è molto chiaro.

Il nostro documento storia ora ha un autore referenziato dall'id del documento autore. Per ottenere le informazioni sull'autore nei risultati della storia utilizziamo [`populate()`](https://mongoosejs.com/docs/api/model.html#Model.populate), come mostrato di seguito.

```js
Story.findOne({ title: "Bob goes sledding" })
  .populate("author") // Replace the author id with actual author information in results
  .exec();
```

> [!NOTE]
> I lettori attenti avranno notato che abbiamo aggiunto un autore alla nostra storia, ma non abbiamo fatto nulla per aggiungere la nostra storia all'array `stories` dell'autore. Come possiamo fare allora a ottenere tutte le storie di un determinato autore? Un modo sarebbe quello di aggiungere la nostra storia all'array delle storie, ma questo comporterebbe avere due posti in cui le informazioni che collegano autori e storie devono essere mantenute.
>
> Un modo migliore è ottenere l'`_id` del nostro _autore_, quindi usare `find()` per cercare questo nel campo autore di tutte le storie.
>
> ```js
> Story.find({ author: bob._id }).exec();
> ```

Questo è quasi tutto quello che devi sapere sul lavoro con elementi correlati _per questo tutorial_. Per informazioni più dettagliate, consulta [Population](https://mongoosejs.com/docs/populate.html) (documentazione di Mongoose).

### Uno schema/modello per file

Mentre puoi creare schema e modelli utilizzando qualsiasi struttura di file ti piaccia, raccomandiamo vivamente di definire ogni modello di schema in un proprio modulo (file) e quindi esportare il metodo per creare il modello.
Questo è mostrato di seguito:

```js
// File: ./models/some-model.js

// Require Mongoose
const mongoose = require("mongoose");

// Define a schema
const Schema = mongoose.Schema;

const SomeModelSchema = new Schema({
  a_string: String,
  a_date: Date,
});

// Export function to create "SomeModel" model class
module.exports = mongoose.model("SomeModel", SomeModelSchema);
```

Puoi quindi richiedere e utilizzare il modello immediatamente in altri file. Mostriamo di seguito come potresti usarlo per ottenere tutte le istanze del modello.

```js
// Create a SomeModel model just by requiring the module
const SomeModel = require("../models/some-model");

// Use the SomeModel object (model) to find all SomeModel records
const modelInstances = await SomeModel.find().exec();
```

## Configurare il database MongoDB

Ora che abbiamo capito qualcosa su cosa può fare Mongoose e su come vogliamo progettare i nostri modelli, è il momento di iniziare a lavorare sul sito web _LocalLibrary_. La primissima cosa che vogliamo fare è configurare un database MongoDB che possiamo usare per memorizzare i nostri dati della biblioteca.

Per questo tutorial, utilizzeremo il sandbox database ospitato su cloud [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database). Questo tier del database non è considerato adatto per siti web in produzione perché non ha ridondanza, ma è ottimo per lo sviluppo e il prototyping. Lo stiamo usando qui perché è gratuito e facile da configurare, e perché MongoDB Atlas è un venditore popolare di _database as a service_ che potresti ragionevolmente scegliere come tuo database di produzione (altre scelte popolari al momento della stesura includono [ScaleGrid](https://scalegrid.io/) e [ObjectRocket](https://www.objectrocket.com/)).

> [!NOTE]
> Se lo preferisci, puoi configurare un database MongoDB localmente scaricando e installando [i binari appropriati per il tuo sistema](https://www.mongodb.com/try/download/community-edition/releases). Il resto delle istruzioni in questo articolo sarebbe simile, tranne per l'URL del database che specificheresti quando ti connetti.
> Nel tutorial [Express Tutorial Parte 7: Distribuzione in Produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment) ospitiamo sia l'applicazione che il database su [Railway](https://railway.com/), ma potremmo altrettanto bene aver utilizzato un database su [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database).

Per prima cosa, avrai bisogno di [creare un account](https://www.mongodb.com/cloud/atlas/register) con MongoDB Atlas (è gratuito e richiede solo di inserire dettagli di contatto di base e accettare i loro termini di servizio).

Dopo aver effettuato il login, verrai portato alla schermata [home](https://cloud.mongodb.com/v2):

1. Fai clic sul pulsante **+ Crea** nella sezione _Overview_.

   ![Creare un database su MongoDB Atlas.](mongodb_atlas_-_createdatabase.jpg)

2. Questo aprirà la schermata _Deploy your cluster_.
   Fai clic sull'opzione modello **M0 FREE**.

   ![Scegliere un'opzione di distribuzione su MongoDB Atlas.](mongodb_atlas_-_deploy.jpg)

3. Scorri verso il basso nella pagina per vedere le diverse opzioni che puoi scegliere.
   ![Scegliere un fornitore di cloud su MongoDB Atlas.](mongodb_atlas_-_createsharedcluster.jpg)

   - Puoi cambiare il nome del tuo Cluster sotto _Cluster Name_.
     Lo terremo come `Cluster0` per questo tutorial.
   - Deseleziona la casella di controllo _Preload sample dataset_, poiché importeremo i nostri dati di esempio più avanti
   - Seleziona qualsiasi fornitore e regione dalle sezioni _Provider_ e _Region_. Diverse regioni offrono fornitori differenti.
   - I tag sono opzionali. Non li useremo qui.
   - Fai clic sul pulsante **Create deployment** (la creazione del cluster richiederà alcuni minuti).

4. Questo aprirà la sezione _Security Quickstart_.
   ![Impostare le regole di accesso sulla schermata Security Quickstart su MongoDB Atlas.](mongodb_atlas_-_securityquickstart.jpg)

   - Inserisci un nome utente e una password che la tua applicazione utilizzerà per accedere al database (sopra abbiamo creato un nuovo login "cooluser").
     Ricorda di copiare e memorizzare le credenziali in modo sicuro poiché ne avremo bisogno più avanti.
     Fai clic sul pulsante **Crea utente**.

     > [!NOTE]
     > Evita di utilizzare caratteri speciali nella tua password utente MongoDB poiché mongoose potrebbe non analizzare correttamente la stringa di connessione.

   - Seleziona **Aggiungi dall'indirizzo IP corrente** per consentire l'accesso dal tuo computer attuale
   - Inserisci `0.0.0.0/0` nel campo Indirizzo IP e poi fai clic sul pulsante **Aggiungi voce**.
     Questo dice a MongoDB che vogliamo consentire l'accesso da ovunque.

     > [!NOTE]
     > È una buona pratica limitare gli indirizzi IP che possono connettersi al tuo database e ad altre risorse. Qui consenti

amo un collegamento da ovunque perché non sappiamo da dove arriverà la richiesta dopo il deployment.

- Fare clic sul pulsante **Finish and Close**.

5. Verrà aperta la seguente schermata. Fai clic sul pulsante **Vai a Panoramica**.
   ![Vai ai Database dopo aver impostato le regole di accesso su MongoDB Atlas](mongodb_atlas_-_accessrules.jpg)

6. Torni alla schermata _Panoramica_. Fai clic sulla sezione _Database_ nel menu _Deployment_ a sinistra. Fai clic sul pulsante **Sfoglia Collezioni**.
   ![Imposta una raccolta su MongoDB Atlas.](mongodb_atlas_-_createcollection.jpg)

7. Questo aprirà la sezione _Collections_. Fai clic sul pulsante **Aggiungi i miei dati**.
   ![Crea un database su MongoDB Atlas.](mongodb_atlas_-_adddata.jpg)

8. Questo aprirà la schermata _Crea Database_.

   ![Dettagli durante la creazione del database su MongoDB Atlas.](mongodb_atlas_-_databasedetails.jpg)

   - Inserisci il nome del nuovo database come `local_library`.
   - Inserisci il nome della collezione come `Collection0`.
   - Fai clic sul pulsante **Crea** per creare il database.

9. Torni alla schermata _Collections_ con il tuo database creato.
   ![Conferma della creazione del database su MongoDB Atlas.](mongodb_atlas_-_databasecreated.jpg)

   - Fai clic sulla scheda _Panoramica_ per tornare alla panoramica del cluster.

10. Dalla schermata _Panoramica_ di Cluster0 fai clic sul pulsante **Connetti**.

    ![Configura connessione dopo aver impostato un cluster in MongoDB Atlas.](mongodb_atlas_-_connectbutton.jpg)

11. Questo aprirà la schermata _Connetti a Cluster0_.

    ![Scegli la connessione Short SRV durante l'impostazione di una connessione su MongoDB Atlas.](mongodb_atlas_-_connectforshortsrv.jpg)

    - Seleziona il tuo utente del database.
    - Seleziona la categoria _Drivers_, poi il _Driver_ **Node.js** e la _Versione_ come mostrato.
    - **NON** installare il driver come suggerito.
    - Fai clic sull'icona **Copia** per copiare la stringa di connessione.
    - Incollalo nel tuo editor di testo locale.
    - Sostituisci il segnaposto `<password>` nella stringa di connessione con la password del tuo utente.
    - Inserisci il nome del database "local_library" nel percorso prima delle opzioni (`...mongodb.net/local_library?retryWrites...`)
    - Salva il file contenente questa stringa in un posto sicuro.

Hai ora creato il database e hai un URL (con nome utente e password) che può essere utilizzato per accedervi.
Questo sarà simile a: `mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority&appName=Cluster0`

## Installare Mongoose

Apri un prompt dei comandi e vai nella directory in cui hai creato il tuo [sito web scheletro Local Library](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website).
Inserisci il seguente comando per installare Mongoose (e le sue dipendenze) e aggiungerlo al tuo file **package.json**, a meno che non lo abbia già fatto leggendo il [Primer su Mongoose](#installare_mongoose_e_mongodb) sopra.

```bash
npm install mongoose
```

## Connettersi a MongoDB

Apri **/app.js** (nella radice del tuo progetto) e copia il seguente testo sotto dove dichiari l'oggetto _Express application_ (dopo la riga `const app = express();`).
Sostituisci la stringa URL del database ('_inserisci_il_tuo_URL_del_database_quì_') con l'URL del tuo database personale (cioè, utilizzando le informazioni da _MongoDB Atlas_).

```js
// Set up mongoose connection
const mongoose = require("mongoose");

mongoose.set("strictQuery", false);
const mongoDB = "insert_your_database_url_here";

main().catch((err) => console.log(err));
async function main() {
  await mongoose.connect(mongoDB);
}
```

Come discusso nel [primer su Mongoose](#connessione_a_mongodb) sopra, questo codice crea la connessione predefinita al database e segnala eventuali errori nella console.

Nota che non si consiglia di codificare le credenziali del database nel codice sorgente come mostrato sopra.
Lo facciamo qui perché mostra il codice di connessione di base, e perché durante lo sviluppo non ci sono rischi significativi che trapelando questi dettagli si esponga o corrompa informazioni sensibili.
Ti mostreremo come farlo in modo più sicuro quando [distribuiamo in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment#database_configuration)!

## Definire lo Schema di LocalLibrary

Definiremo un modulo separato per ciascun modello, come [discusso sopra](#one_schemamodel_per_file).
Inizia creando una cartella per i nostri modelli nella radice del progetto (**/models**) e poi crea file separati per ciascuno dei modelli:

```plain
/express-locallibrary-tutorial  # the project root
  /models
    author.js
    book.js
    bookinstance.js
    genre.js
```

### Modello Author

Copia il codice dello schema `Author` mostrato sotto e incollalo nel tuo file **./models/author.js**.
Lo schema definisce un autore come avente `String` SchemaTypes per il nome e il cognome (richiesto, con un massimo di 100 caratteri), e campi `Date` per le date di nascita e morte.

```js
const mongoose = require("mongoose");

const Schema = mongoose.Schema;

const AuthorSchema = new Schema({
  first_name: { type: String, required: true, maxLength: 100 },
  family_name: { type: String, required: true, maxLength: 100 },
  date_of_birth: { type: Date },
  date_of_death: { type: Date },
});

// Virtual for author's full name
AuthorSchema.virtual("name").get(function () {
  // To avoid errors in cases where an author does not have either a family name or first name
  // We want to make sure we handle the exception by returning an empty string for that case
  let fullname = "";
  if (this.first_name && this.family_name) {
    fullname = `${this.family_name}, ${this.first_name}`;
  }

  return fullname;
});

// Virtual for author's URL
AuthorSchema.virtual("url").get(function () {
  // We don't use an arrow function as we'll need the this object
  return `/catalog/author/${this._id}`;
});

// Export model
module.exports = mongoose.model("Author", AuthorSchema);
```

Abbiamo anche dichiarato una [virtual](#proprietà_virtuali) per l'AuthorSchema chiamata "url" che restituisce l'URL assoluto necessario per ottenere un'istanza particolare del modello — useremo la proprietà nei nostri modelli ogni volta che avremo bisogno di ottenere un collegamento a un particolare autore.

> [!NOTE]
> Dichiarare i nostri URL come una virtual nello schema è una buona idea perché in questo modo l'URL di un elemento deve essere modificato solo in un posto.
> A questo punto, un collegamento che utilizza questo URL non funzionerebbe, perché non abbiamo ancora alcun codice di trattamento delle rotte per le istanze singole del modello.
> Lo imposteremo in un prossimo articolo!

Alla fine del modulo, esportiamo il modello.

### Modello Book

Copia il codice dello schema `Book` mostrato sotto e incollalo nel tuo file **./models/book.js**.
La maggior parte di questo è simile al modello autore — abbiamo dichiarato uno schema con un numero di campi stringa e un virtual per ottenere l'URL di record di libri specifici, ed esportiamo il modello.

```js
const mongoose = require("mongoose");

const Schema = mongoose.Schema;

const BookSchema = new Schema({
  title: { type: String, required: true },
  author: { type: Schema.Types.ObjectId, ref: "Author", required: true },
  summary: { type: String, required: true },
  isbn: { type: String, required: true },
  genre: [{ type: Schema.Types.ObjectId, ref: "Genre" }],
});

// Virtual for book's URL
BookSchema.virtual("url").get(function () {
  // We don't use an arrow function as we'll need the this object
  return `/catalog/book/${this._id}`;
});

// Export model
module.exports = mongoose.model("Book", BookSchema);
```

La principale differenza qui è che abbiamo creato due riferimenti ad altri modelli:

- author è un riferimento a un singolo oggetto modello `Author`, ed è richiesto.
- genre è un riferimento a un array di oggetti modello `Genre`. Non abbiamo ancora dichiarato questo oggetto!

### Modello BookInstance

Infine, copia il codice dello schema `BookInstance` mostrato sotto e incollalo nel tuo file **./models/bookinstance.js**.
Il `BookInstance` rappresenta una copia specifica di un libro che qualcuno potrebbe prendere in prestito e include informazioni sul fatto che la copia sia disponibile, su quale data è prevista la restituzione, e dettagli sull'"imprint" (o versione).

```js
const mongoose = require("mongoose");

const Schema = mongoose.Schema;

const BookInstanceSchema = new Schema({
  book: { type: Schema.Types.ObjectId, ref: "Book", required: true }, // reference to the associated book
  imprint: { type: String, required: true },
  status: {
    type: String,
    required: true,
    enum: ["Available", "Maintenance", "Loaned", "Reserved"],
    default: "Maintenance",
  },
  due_back: { type: Date, default: Date.now },
});

// Virtual for bookinstance's URL
BookInstanceSchema.virtual("url").get(function () {
  // We don't use an arrow function as we'll need the this object
  return `/catalog/bookinstance/${this._id}`;
});

// Export model
module.exports = mongoose.model("BookInstance", BookInstanceSchema);
```

Le nuove cose che mostriamo qui sono le opzioni del campo:

- `enum`: Ci consente di impostare i valori consentiti di una stringa. In questo caso, lo usiamo per specificare lo stato di disponibilità dei nostri libri (usare un enum significa che possiamo prevenire errori di digitazione e valori arbitrari per il nostro stato).
- `default`: Usiamo default per impostare lo stato predefinito per le nuove istanze di libri su "Maintenance" e la data `due_back` predefinita su `now` (nota come puoi chiamare la funzione Date quando imposti la data!).

Tutto il resto dovrebbe essere familiare dai nostri schemi precedenti.

### Modello Genre - sfida

Apri il tuo file **./models/genre.js** e crea uno schema per memorizzare i generi (la categoria del libro, ad esempio, se è narrativa o non narrativa, romantico o storia militare, ecc.).

La definizione sarà molto simile agli altri modelli:

- Il modello dovrebbe avere un SchemaType `String` chiamato `name` per descrivere il genere.
- Questo nome dovrebbe essere richiesto e avere tra 3 e 100 caratteri.
- Dichiarare una [virtual](#proprietà_virtuali) per l'URL del genere, chiamata `url`.
- Esportare il modello.

## Testare — creare alcuni elementi

Questo è tutto. Ora abbiamo impostato tutti i modelli per il sito!

Per testare i modelli (e creare alcuni libri di esempio e altri articoli che possiamo usare nei nostri prossimi articoli), eseguiremo ora uno _script indipendente_ per creare elementi di ciascun tipo:

1. Scarica (o crea in altro modo) il file [populatedb.js](https://raw.githubusercontent.com/mdn/express-locallibrary-tutorial/main/populatedb.js) nella tua directory _express-locallibrary-tutorial_ (al livello di `package.json`).

   > [!NOTE]
   > Il codice in `populatedb.js` può essere utile per imparare JavaScript, ma comprenderlo non è necessario per questo tutorial.

2. Esegui lo script utilizzando node nel tuo prompt dei comandi, passando l'URL del tuo database _MongoDB_ (lo stesso con cui hai sostituito il segnaposto _inserisci_il_tuo_URL_del_database_quì_, all'interno di `app.js` in precedenza):

   ```bash
   node populatedb <your MongoDB url>
   ```

   > [!NOTE]
   > Su Windows è necessario racchiudere l'URL del database tra doppie virgolette (").
   > Su altri sistemi operativi potresti aver bisogno di virgolette singole (').

3. Lo script dovrebbe eseguire fino al completamento, mostrando gli elementi mentre li crea nel terminale.

> [!NOTE]
> Vai al tuo database su MongoDB Atlas (nella scheda _Collections_).
> Dovresti ora essere in grado di esplorare individuali collezioni di Libri, Autori, Generi e Istanze di Libri, e controllare i singoli documenti.

## Riepilogo

In questo articolo, abbiamo imparato un po' sui database e sugli ORM su Node/Express e molto su come sono definiti gli schema e i modelli di Mongoose. Abbiamo quindi utilizzato queste informazioni per progettare e implementare modelli di `Book`, `BookInstance`, `Author` e `Genre` per il sito web _LocalLibrary_.

Infine, abbiamo testato i nostri modelli creando un certo numero di istanze (utilizzando uno script standalone). Nel prossimo articolo vedremo come creare alcune pagine per visualizzare questi oggetti.

## Vedi anche

- [Integrazione del database](https://expressjs.com/en/guide/database-integration.html) (documentazione di Express)
- [Sito web di Mongoose](https://mongoosejs.com/) (documentazione di Mongoose)
- [Guida di Mongoose](https://mongoosejs.com/docs/guide.html) (documentazione di Mongoose)
- [Validazione](https://mongoosejs.com/docs/validation.html) (documentazione di Mongoose)
- [Tipi di Schema](https://mongoosejs.com/docs/schematypes.html) (documentazione di Mongoose)
- [Modelli](https://mongoosejs.com/docs/models.html) (documentazione di Mongoose)
- [Queries](https://mongoosejs.com/docs/queries.html) (documentazione di Mongoose)
- [Popolamento](https://mongoosejs.com/docs/populate.html) (documentazione di Mongoose)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
