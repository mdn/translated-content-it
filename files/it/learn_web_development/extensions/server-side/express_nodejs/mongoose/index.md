---
title: "Tutorial Express Parte 3: Utilizzo di un Database (con Mongoose)"
short-title: "3: Utilizzo dei database con Mongoose"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose
l10n:
  sourceCommit: 759102220c07fb140b3e06971cd5981d8f0f134f
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo articolo introduce brevemente i database e come utilizzarli con le app Node/Express. Successivamente mostra come possiamo utilizzare [Mongoose](https://mongoosejs.com/) per fornire l'accesso al database per il sito web della [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Spiega come vengono dichiarati gli schemi e i modelli degli oggetti, i principali tipi di campo e la validazione di base. Inoltre, mostra brevemente alcuni dei principali metodi in cui si può accedere ai dati del modello.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website">Tutorial Express Parte 2: Creazione di un sito web scheletro</a>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Essere in grado di progettare e creare i propri modelli utilizzando Mongoose.</td>
    </tr>
  </tbody>
</table>

## Panoramica

Il personale della biblioteca utilizzerà il sito web Local Library per memorizzare informazioni su libri e prestatori, mentre i membri della biblioteca lo utilizzeranno per sfogliare e cercare i libri, scoprire se ci sono copie disponibili e quindi prenotarle o prenderle in prestito. Per memorizzare e recuperare le informazioni in modo efficiente, le archivieremo in un _database_.

Le app Express possono utilizzare molti database diversi e ci sono diversi approcci che si possono utilizzare per eseguire operazioni di **C**reazione, **L**ettura, **A**ggiornamento e **E**liminazione (CRUD). Questo tutorial fornisce una breve panoramica di alcune delle opzioni disponibili e poi mostra nel dettaglio i meccanismi particolari selezionati.

### Quali database posso usare?

Le app _Express_ possono utilizzare qualsiasi database supportato da _Node_ (Express stesso non definisce alcun comportamento/requisito aggiuntivo specifico per la gestione del database). Ci sono [molte opzioni popolari](https://expressjs.com/en/guide/database-integration.html), tra cui PostgreSQL, MySQL, Redis, SQLite e MongoDB.

Quando si sceglie un database, si dovrebbero considerare aspetti come il tempo di produttività/la curva di apprendimento, le prestazioni, la facilità di replica/backup, il costo, il supporto della comunità, ecc. Sebbene non esista un "miglior" database unico, quasi tutte le soluzioni popolari dovrebbero essere più che accettabili per un sito di piccole-medie dimensioni come la nostra Local Library.

Per ulteriori informazioni sulle opzioni, vedi [Integrazione del database](https://expressjs.com/en/guide/database-integration.html) (documentazione Express).

### Qual è il modo migliore per interagire con un database?

Ci sono due approcci comuni per interagire con un database:

- Utilizzare il linguaggio nativo di query del database, come SQL.
- Utilizzare un Object Relational Mapper ("ORM") o Object Document Mapper ("ODM"). Questi rappresentano i dati del sito web come oggetti JavaScript, che vengono poi mappati al database sottostante. Alcuni ORM e ODM sono legati a un database specifico, mentre altri forniscono un backend agnostico rispetto al database.

Le migliori _prestazioni_ possono essere ottenute utilizzando SQL, o qualsiasi linguaggio di query supportato dal database. I mapper di oggetti sono spesso più lenti perché usano codice di traduzione per mappare tra oggetti e il formato del database, che potrebbe non utilizzare le query di database più efficienti (questo è particolarmente vero se il mapper supporta diversi backend di database e deve fare maggiori compromessi in termini di funzionalità di database supportate).

Il vantaggio di usare un ORM/ODM è che i programmatori possono continuare a pensare in termini di oggetti JavaScript piuttosto che di semantica del database - questo è particolarmente vero se si ha bisogno di lavorare con diversi database (sullo stesso sito o su siti diversi). Offrono anche un luogo ovvio dove eseguire la validazione dei dati.

> [!NOTE]
> L'uso di ODM/ORM porta spesso a costi inferiori per lo sviluppo e la manutenzione! A meno che non si abbia una profonda familiarità con il linguaggio di query nativo o che le prestazioni siano fondamentali, si dovrebbe considerare fortemente l'uso di un ODM.

### Quale ORM/ODM dovrei usare?

Ci sono molte soluzioni ODM/ORM disponibili sul sito del gestore pacchetti npm (consulta i tag [odm](https://www.npmjs.com/search?q=keywords:odm) e [orm](https://www.npmjs.com/search?q=keywords:orm) per un sottoinsieme!).

Alcune soluzioni che erano popolari al momento della scrittura sono:

- [Mongoose](https://www.npmjs.com/package/mongoose): Mongoose è un tool di modellazione di oggetti per [MongoDB](https://www.mongodb.com/), progettato per lavorare in un ambiente asincrono.
- [Waterline](https://www.npmjs.com/package/waterline): Un ORM estratto dal framework web basato su Express [Sails](https://sailsjs.com/). Fornisce un'API uniforme per accedere a numerosi database diversi, inclusi Redis, MySQL, LDAP, MongoDB e Postgres.
- [Bookshelf](https://www.npmjs.com/package/bookshelf): Presenta sia interfacce basate su promesse che tradizionali interfacce callback, fornendo supporto alle transazioni, caricamento di relazioni con/o senza nidificazione, associazioni polimorfiche e supporto per relazioni one-to-one, one-to-many, e many-to-many. Funziona con PostgreSQL, MySQL e SQLite3.
- [Objection](https://www.npmjs.com/package/objection): Rende il più semplice possibile utilizzare tutta la potenza di SQL e il motore di database sottostante (supporta SQLite3, Postgres e MySQL).
- [Sequelize](https://www.npmjs.com/package/sequelize) è un ORM basato su promesse per Node.js e io.js. Supporta i dialetti PostgreSQL, MySQL, MariaDB, SQLite e MSSQL e offre solidi supporti per le transazioni, le relazioni, la replica della lettura e altro ancora.
- [Node ORM2](https://node-orm.readthedocs.io/en/latest/) è un Object Relationship Manager per NodeJS. Supporta MySQL, SQLite e Postgres, aiutando a lavorare con il database utilizzando un approccio orientato agli oggetti.
- [GraphQL](https://graphql.org/): Principalmente un linguaggio di query per API RESTful, GraphQL è molto popolare e presenta funzionalità disponibili per la lettura dei dati dai database.

Come regola generale, si dovrebbero considerare sia le funzionalità fornite che l'"attività della comunità" (download, contributi, segnalazioni di bug, qualità della documentazione, ecc.) quando si sceglie una soluzione. Al momento della scrittura, Mongoose è di gran lunga l'ODM più popolare, ed è una scelta ragionevole se si utilizza MongoDB per il database.

### Utilizzo di Mongoose e MongoDB per LocalLibrary

Per l'esempio della _Local Library_ (e il resto di questo argomento) utilizzeremo l'[ODM Mongoose](https://www.npmjs.com/package/mongoose) per accedere ai dati della nostra biblioteca. Mongoose funge da front end per [MongoDB](https://www.mongodb.com/company/what-is-mongodb), un database open source [NoSQL](https://it.wikipedia.org/wiki/NoSQL) che utilizza un modello di dati orientato ai documenti. Una "collezione" di "documenti" in un database MongoDB [è analoga a](https://www.mongodb.com/docs/manual/core/databases-and-collections/) una "tabella" di "righe" in un database relazionale.

Questa combinazione di ODM e database è estremamente popolare nella comunità Node, in parte perché il sistema di memorizzazione e interrogazione dei documenti è molto simile a JSON, ed è quindi familiare agli sviluppatori JavaScript.

> [!NOTE]
> Non è necessario conoscere MongoDB per utilizzare Mongoose, sebbene parti della [documentazione di Mongoose](https://mongoosejs.com/docs/guide.html) _siano_ più facili da utilizzare e comprendere se si ha già familiarità con MongoDB.

Il resto di questo tutorial mostra come definire e accedere agli schemi e ai modelli Mongoose per l'esempio di sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website).

## Progettazione dei modelli di LocalLibrary

Prima di iniziare a programmare i modelli, vale la pena prendersi qualche minuto per pensare a quali dati dobbiamo memorizzare e alle relazioni tra i diversi oggetti.

Sappiamo di dover memorizzare informazioni sui libri (titolo, riepilogo, autore, genere, ISBN) e che potremmo avere più copie disponibili (con identificatori univoci globali, stati di disponibilità, ecc.). Potremmo aver bisogno di memorizzare ulteriori informazioni sull'autore oltre al loro nome, e potrebbe esistere più di un autore con il medesimo o simile nome. Vogliamo essere in grado di ordinare le informazioni basate sul titolo del libro, autore, genere e categoria.

Quando si progettano i modelli, ha senso avere modelli separati per ogni "oggetto" (un gruppo di informazioni correlate). In questo caso alcuni candidati ovvi per questi modelli sono i libri, le istanze dei libri e gli autori.

Potresti anche voler usare modelli per rappresentare le opzioni di un elenco di selezione (es. come una lista a discesa di scelte), piuttosto che codificare le scelte nel sito stesso — questo è raccomandato quando tutte le opzioni non sono conosciute in anticipo o possono cambiare. Un buon esempio è un genere (es. fantasy, fantascienza, ecc.).

Una volta decisi i modelli e i campi, dobbiamo pensare alle relazioni tra di essi.

Tenendo presente ciò, il diagramma di associazione UML qui sotto mostra i modelli che definiremo in questo caso (come scatole). Come discusso sopra, abbiamo creato modelli per il libro (i dettagli generici del libro), l'istanza del libro (lo stato delle copie fisiche specifiche del libro disponibili nel sistema) e l'autore. Abbiamo anche deciso di avere un modello per il genere in modo che i valori possano essere creati dinamicamente. Abbiamo deciso di non avere un modello per `BookInstance:status` — codificheremo i valori accettabili perché non ci aspettiamo che questi cambino. All'interno di ciascuna delle caselle, puoi vedere il nome del modello, i nomi dei campi e i tipi, e anche i metodi e i loro tipi di ritorno.

Il diagramma mostra anche le relazioni tra i modelli, incluse le loro _multiplicità_. Le multiplicità sono i numeri nel diagramma che mostrano i numeri (massimo e minimo) di ciascun modello che può essere presente nella relazione. Ad esempio, la linea di collegamento tra le caselle mostra che `Book` e `Genre` sono correlati. I numeri vicino al modello `Book` mostrano che un `Genre` deve avere zero o più `Book`s (quanti se ne desideri), mentre i numeri dall'altra parte della linea accanto al `Genre` mostrano che un libro può avere zero o più `Genre` associati.

> [!NOTE]
> Come discusso nella nostra [introduzione a Mongoose](#introduzione_a_mongoose) qui sotto, è spesso meglio avere il campo che definisce la relazione tra i documenti/modelli in un solo modello (puoi comunque trovare la relazione inversa cercando l'`_id` associato nell'altro modello). Qui sotto abbiamo scelto di definire la relazione tra `Book`/`Genre` e `Book`/`Author` nello schema del Book, e la relazione tra `Book`/`BookInstance` nello Schema del `BookInstance`. Questa scelta è stata piuttosto arbitraria — avremmo potuto altrettanto bene avere il campo nell'altro schema.

![Modello di libreria Mongoose con cardinalità corretta](library_website_-_mongoose_express.png)

> [!NOTE]
> La sezione successiva fornisce una breve introduzione su come i modelli sono definiti e usati. Mentre leggi, considera come costruiremo ciascuno dei modelli nel diagramma sopra.

### Le API dei database sono asincrone

I metodi dei database per creare, trovare, aggiornare o eliminare record sono asincroni. Ciò significa che i metodi restituiscono immediatamente, e il codice per gestire il successo o il fallimento del metodo viene eseguito successivamente quando l'operazione completa. Altri codici possono essere eseguiti mentre il server sta aspettando il completamento dell'operazione del database, quindi il server può rimanere reattivo alle altre richieste.

JavaScript ha diversi meccanismi per supportare il comportamento asincrono. Storicamente, JavaScript si è basato pesantemente sul passaggio di [funzioni di callback](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing) ai metodi asincroni per gestire i casi di successo ed errore. Nel JavaScript moderno, i callback sono stati in gran parte sostituiti dalle [Promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise). Le Promise sono oggetti che vengono (immediatamente) restituiti da un metodo asincrono e rappresentano il suo stato futuro. Quando l'operazione completa, l'oggetto Promise si "risolve", e restituisce un oggetto che rappresenta il risultato dell'operazione o un errore.

Ci sono due modi principali per utilizzare le promise per eseguire il codice quando una promise si risolve, e ti raccomandiamo vivamente di leggere [Come usare le promise](/it/docs/Learn_web_development/Extensions/Async_JS/Promises) per una panoramica ad alto livello di entrambi gli approcci. In questo tutorial, utilizzeremo principalmente [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) per attendere il completamento della promise all'interno di una [`async function`](/it/docs/Web/JavaScript/Reference/Statements/async_function), perché questo porta a codice asincrono più leggibile e comprensibile.

Il modo in cui funziona questo approccio è che si usa la keyword `async function` per contrassegnare una funzione come asincrona, e poi si applica `await` a qualsiasi metodo che restituisce una promise all'interno di quella funzione. Quando la funzione asincrona viene eseguita, la sua operazione viene messa in pausa al primo metodo `await` finché la promise si risolve. Dal punto di vista del codice circostante la funzione asincrona ritorna e il codice successivo è in grado di eseguire. Più tardi, quando la promise si risolve, il metodo `await` all'interno della funzione asincrona restituisce con il risultato, o viene sollevato un errore se la promise è stata respinta. Il codice nella funzione asincrona esegue quindi fino a quando non incontra un altro `await`, a quel punto si ferma di nuovo, o fino a quando tutto il codice nella funzione è stato eseguito.

Puoi vedere come funziona questo nell'esempio qui sotto. `myFunction()` è una funzione asincrona che viene chiamata all'interno di un blocco [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch). Quando `myFunction()` viene eseguita, l'esecuzione del codice viene messa in pausa su `methodThatReturnsPromise()` finché la promise si risolve, a quel punto il codice continua su `aFunctionThatReturnsPromise()` e attende di nuovo. Il codice nel blocco `catch` viene eseguito se un errore viene sollevato nella funzione asincrona, e questo accadrà se la promise restituita da uno dei metodi viene respinta.

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

I metodi asincroni sopra vengono eseguiti in sequenza. Se i metodi non dipendono l'uno dall'altro, puoi eseguirli in parallelo e completare l'intera operazione più rapidamente. Questo si fa utilizzando il metodo [`Promise.all()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise/all), che prende come input un iterabile di promise e restituisce una singola `Promise`. Questa promise restituita si completa quando tutte le promise dell'input si completano, con un array dei valori di adempimento. Viene respinta quando una delle promise dell'input viene respinta, con questo primo motivo di respinta.

Il codice qui sotto mostra come funziona. Prima, abbiamo due funzioni che restituiscono promise. Attendiamo il completamento di entrambe utilizzando la promise restituita da `Promise.all()`. Una volta completate entrambe, `await` restituisce e l'array dei risultati viene popolato, la funzione continua poi al prossimo `await`, e attende finché la promise restituita da `anotherFunctionThatReturnsPromise()` non si risolve. Chiameresti `myFunction()` in un blocco `try...catch` per catturare eventuali errori.

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

Le promise con `await`/`async` consentono un controllo sia flessibile che "comprensibile" sull'esecuzione asincrona!

## Introduzione a Mongoose

Questa sezione fornisce una panoramica su come collegare Mongoose a un database MongoDB, come definire uno schema e un modello, e come eseguire query di base.

> [!NOTE]
> Questa introduzione è fortemente influenzata dalla [versione rapida di Mongoose](https://www.npmjs.com/package/mongoose) su _npm_ e dalla [documentazione ufficiale](https://mongoosejs.com/docs/guide.html).

### Installazione di Mongoose e MongoDB

Mongoose viene installato nel tuo progetto (**package.json**) come qualsiasi altra dipendenza — utilizzando npm. Per installarlo, usa il seguente comando all'interno della tua cartella del progetto:

```bash
npm install mongoose
```

L'installazione di _Mongoose_ aggiunge tutte le sue dipendenze, incluso il driver del database MongoDB, ma non installa il database MongoDB stesso. Se vuoi installare un server MongoDB, puoi [scaricare i programmi di installazione da qui](https://www.mongodb.com/try/download/community) per vari sistemi operativi e installarlo localmente. Puoi anche utilizzare istanze MongoDB basate su cloud.

> [!NOTE]
> Per questo tutorial, utilizzeremo il cloud-based _database as a service_ [MongoDB Atlas](https://www.mongodb.com/) tier gratuito per fornire il database. Questo è adatto allo sviluppo e ha senso per il tutorial perché rende l'"installazione" indipendente dal sistema operativo (database-as-a-service è anche un approccio che potresti usare per il tuo database di produzione).

### Collegamento a MongoDB

_Mongoose_ richiede una connessione a un database MongoDB. Puoi `require()` e connetterti a un database ospitato localmente con `mongoose.connect()` come mostrato di seguito (per il tutorial ci connetteremo invece a un database ospitato su Internet).

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
> Come discusso nella sezione [Le API dei database sono asincrone](#le_api_dei_database_sono_asincrone), qui attendiamo la promise restituita dal metodo `connect()` all'interno di una funzione `async`.
> Usiamo il gestore `catch()` della promise per gestire eventuali errori durante il tentativo di connessione, ma potremmo anche aver chiamato `main()` all'interno di un blocco `try...catch`.

Puoi ottenere l'oggetto `Connection` predefinito con `mongoose.connection`. Se hai bisogno di creare connessioni aggiuntive, puoi usare `mongoose.createConnection()`. Questo prende la stessa forma di URI del database (con host, database, porta, opzioni, ecc.) di `connect()` e restituisce un oggetto `Connection`. Nota che `createConnection()` restituisce immediatamente; se hai bisogno di attendere che la connessione venga stabilita puoi chiamarlo con `asPromise()` per restituire una promise (`mongoose.createConnection(mongoDB).asPromise()`).

### Definizione e creazione dei modelli

I modelli sono _definiti_ utilizzando l'interfaccia `Schema`. Lo Schema consente di definire i campi memorizzati in ciascun documento insieme ai loro requisiti di validazione e valori predefiniti. Inoltre, puoi definire metodi helper statici e di istanza per facilitare il lavoro con i tuoi tipi di dati, e anche proprietà virtuali che puoi utilizzare come qualsiasi altro campo, ma che non vengono effettivamente memorizzate nel database (lo discuteremo un po' più avanti).

Gli schemi vengono quindi "compilati" in modelli utilizzando il metodo `mongoose.model()`. Una volta che hai un modello, puoi usarlo per trovare, creare, aggiornare e eliminare oggetti del tipo specificato.

> [!NOTE]
> Ciascun modello mappa a una _collezione_ di _documenti_ nel database MongoDB. I documenti conterranno i campi/i tipi di schema definiti nel modello `Schema`.

#### Definizione degli schemi

Il frammento di codice qui sotto mostra come potresti definire uno schema semplice. Prima `require()` mongoose, poi utilizza il costruttore di Schema per creare una nuova istanza di schema, definendo i vari campi al suo interno nell'oggetto parametro del costruttore.

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

Nel caso sopra abbiamo solo due campi, una stringa e una data. Nelle sezioni successive, mostreremo alcuni dei tipi di campo, la validazione e altri metodi.

#### Creazione di un modello

I modelli vengono creati dagli schemi utilizzando il metodo `mongoose.model()`:

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

Il primo argomento è il nome singolare della collezione che verrà creata per il tuo modello (Mongoose creerà la collezione del database per il modello _SomeModel_ sopra), e il secondo argomento è lo schema che vuoi usare nella creazione del modello.

> [!NOTE]
> Una volta che hai definito le tue classi modello, puoi usarle per creare, aggiornare o eliminare record e eseguire query per ottenere tutti i record o particolari sottoinsiemi di record. Ti mostreremo come farlo nella sezione [Utilizzo dei modelli](#utilizzo_dei_modelli) e quando creiamo le nostre viste.

#### Tipi di schema (campi)

Uno schema può avere un numero arbitrario di campi — ciascuno rappresenta un campo nei documenti memorizzati in _MongoDB_. Un esempio di schema che mostra molti dei tipi di campo comuni e come vengono dichiarati è mostrato di seguito.

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

- `ObjectId`: Rappresenta istanze specifiche di un modello nel database. Ad esempio, un libro potrebbe utilizzare questo per rappresentare il suo oggetto autore. Questo conterrà effettivamente l'ID univoco (`_id`) per l'oggetto specificato. Possiamo usare il metodo `populate()` per ottenere le informazioni associate quando necessario.
- [`Mixed`](https://mongoosejs.com/docs/schematypes.html#mixed): Un tipo di schema arbitrario.
- `[]`: Un array di elementi. Puoi eseguire operazioni sull'array JavaScript su questi modelli (push, pop, unshift, ecc.). Gli esempi sopra mostrano un array di oggetti senza un tipo specificato e un array di oggetti `String`, ma puoi avere un array di qualsiasi tipo di oggetto.

Il codice mostra anche entrambi i modi di dichiarare un campo:

- Nome del _campo_ e _tipo_ come coppia chiave-valore (cioè, come fatto con i campi `name`, `binary` e `living`).
- Nome del _campo_ seguito da un oggetto che definisce il `type` e qualsiasi altra _opzione_ per il campo. Le opzioni includono cose come:

  - valori predefiniti.
  - validatori integrati (es. valori min/max) e funzioni di validazione personalizzate.
  - Se il campo è obbligatorio
  - Se i campi `String` devono essere automaticamente impostati in minuscolo, maiuscolo o tagliato (es. `{ type: String, lowercase: true, trim: true }`)

Per ulteriori informazioni sulle opzioni, consulta [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) (documentazione di Mongoose).

#### Validazione

Mongoose fornisce validatori integrati e personalizzati, e validatori sincroni e asincroni. Ti permette di specificare sia l'intervallo accettabile di valori che il messaggio di errore per il fallimento della validazione in tutti i casi.

I validatori integrati includono:

- Tutti i [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) hanno il validator integrato [required](https://mongoosejs.com/docs/api.html#schematype_SchemaType-required). Questo viene utilizzato per specificare se il campo deve essere fornito per poter salvare un documento.
- I [numeri](https://mongoosejs.com/docs/api/schemanumber.html) hanno i validator [min](<https://mongoosejs.com/docs/api/schemanumber.html#SchemaNumber.prototype.min()>) e [max](<https://mongoosejs.com/docs/api/schemanumber.html#SchemaNumber.prototype.max()>).
- Le [Stringhe](https://mongoosejs.com/docs/api/schemastring.html) hanno:

  - [enum](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.enum()>): specifica il set di valori consentiti per il campo.
  - [match](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.match()>): specifica un'espressione regolare che la stringa deve corrispondere.
  - [maxLength](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.maxlength()>) e [minLength](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.minlength()>) per la stringa.

L'esempio qui sotto (leggermente modificato dai documenti di Mongoose) mostra come si possono specificare alcuni dei tipi di validatori e messaggi di errore:

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

Per informazioni complete sulla validazione dei campi, consulta [Validation](https://mongoosejs.com/docs/validation.html) (documentazione di Mongoose).

#### Proprietà virtuali

Le proprietà virtuali sono proprietà del documento che puoi ottenere e impostare ma che non vengono memorizzate in MongoDB. I getter sono utili per formattare o combinare campi, mentre i setter sono utili per scomporre un singolo valore in più valori per la memorizzazione. L'esempio nei documenti costruisce (e decompone) una proprietà virtuale del nome completo da un campo nome e cognome, che è più facile e pulito che costruire un nome completo ogni volta che viene utilizzato in un template.

> [!NOTE]
> Utilizzeremo una proprietà virtuale nella libreria per definire un URL univoco per ciascun record modello utilizzando un percorso e il valore `_id` del record.

Per ulteriori informazioni, consulta [Virtuals](https://mongoosejs.com/docs/guide.html#virtuals) (documentazione di Mongoose).

#### Metodi e helper di query

Uno schema può anche avere [metodi di istanza](https://mongoosejs.com/docs/guide.html#methods), [metodi statici](https://mongoosejs.com/docs/guide.html#statics), e [helper di query](https://mongoosejs.com/docs/guide.html#query-helpers). I metodi di istanza e statici sono simili, ma con la differenza ovvia che un metodo di istanza è associato a un particolare record e ha accesso all'oggetto corrente. Gli helper di query consentono di estendere l'[API dei costruttori di query concatenabili](https://mongoosejs.com/docs/queries.html) di Mongoose (ad esempio, consentendo di aggiungere una query "byName" oltre ai metodi `find()`, `findOne()` e `findById()`).

### Utilizzo dei modelli

Una volta creato uno schema, puoi usarlo per creare modelli. Il modello rappresenta una collezione di documenti nel database che puoi cercare, mentre le istanze del modello rappresentano singoli documenti che puoi salvare e recuperare.

Forniamo una breve panoramica qui sotto. Per ulteriori informazioni, consulta: [Models](https://mongoosejs.com/docs/models.html) (documentazione di Mongoose).

> [!NOTE]
> La creazione, l'aggiornamento, l'eliminazione e l'interrogazione dei record sono operazioni asincrone che restituiscono una [promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise).
> Gli esempi qui sotto mostrano solo l'uso dei metodi pertinenti e `await` (cioè, il codice essenziale per utilizzare i metodi).
> La funzione `async` circostante e il blocco `try...catch` per catturare errori sono omessi per chiarezza.
> Per ulteriori informazioni sull'uso di `await/async`, consulta [Le API dei database sono asincrone](#le_api_dei_database_sono_asincrone) sopra.

#### Creazione e modifica dei documenti

Per creare un record puoi definire un'istanza del modello e poi chiamare [`save()`](https://mongoosejs.com/docs/api/model.html#Model.prototype.save) su di essa.
Gli esempi di seguito presuppongono che `SomeModel` sia un modello (con un singolo campo `name`) che abbiamo creato dal nostro schema.

```js
// Create an instance of model SomeModel
const awesome_instance = new SomeModel({ name: "awesome" });

// Save the new model instance asynchronously
await awesome_instance.save();
```

Puoi anche usare [`create()`](https://mongoosejs.com/docs/api/model.html#Model.create) per definire l'istanza del modello nello stesso momento in cui la salvi.
Sotto ne creiamo solo uno, ma puoi creare più istanze passando un array di oggetti.

```js
await SomeModel.create({ name: "also_awesome" });
```

Ogni modello ha una connessione associata (questa sarà la connessione predefinita quando usi `mongoose.model()`). Crei una nuova connessione e chiami `.model()` su di essa per creare i documenti su un database diverso.

Puoi accedere ai campi in questo nuovo record utilizzando la sintassi del punto, e cambiare i valori. Devi chiamare `save()` o `update()` per memorizzare i valori modificati nel database.

```js
// Access model field values using dot notation
console.log(awesome_instance.name); // should log 'also_awesome'

// Change record by modifying the fields, then calling save().
awesome_instance.name = "New cool name";
await awesome_instance.save();
```

#### Ricerca dei record

Puoi cercare i record utilizzando i metodi di query, specificando le condizioni di query come documento JSON. Il frammento di codice qui sotto mostra come potresti trovare tutti gli atleti in un database che giocano a tennis, restituendo solo i campi per nome dell'atleta e età. Qui specifichiamo solo un campo corrispondente (sport) ma puoi aggiungere più criteri, specificare criteri di espressioni regolari o rimuovere del tutto le condizioni per restituire tutti gli atleti.

```js
const Athlete = mongoose.model("Athlete", yourSchema);

// find all athletes who play tennis, returning the 'name' and 'age' fields
const tennisPlayers = await Athlete.find(
  { sport: "Tennis" },
  "name age",
).exec();
```

> [!NOTE]
> È importante ricordare che non trovare nessun risultato **non è un errore** per una ricerca — ma potrebbe essere un caso di errore nel contesto della tua applicazione.
> Se la tua applicazione si aspetta che una ricerca trovi un valore puoi controllare il numero di voci restituite nel risultato.

Le API di query, come [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>), restituiscono una variabile di tipo [Query](https://mongoosejs.com/docs/api/query.html).
Puoi utilizzare un oggetto query per costruire una query in parti prima di eseguirla con il metodo [`exec()`](https://mongoosejs.com/docs/api/query.html#Query.prototype.exec).
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

Sopra abbiamo definito le condizioni di query nel metodo [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>). Possiamo anche farlo usando una funzione [`where()`](<https://mongoosejs.com/docs/api/model.html#Model.where()>), e possiamo concatenare tutte le parti della nostra query insieme utilizzando l'operatore del punto (.) anziché aggiungerle separatamente.
Il frammento di codice qui sotto è lo stesso della nostra query sopra, con una condizione aggiuntiva per l'età.

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

Il metodo [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>) ottiene tutti i record corrispondenti, ma spesso vuoi ottenere solo una corrispondenza. I seguenti metodi eseguono una query per un singolo record:

- [`findById()`](<https://mongoosejs.com/docs/api/model.html#Model.findById()>): Trova il documento con l'`id` specificato (ogni documento ha un `id` univoco).
- [`findOne()`](<https://mongoosejs.com/docs/api/model.html#Model.findOne()>): Trova un singolo documento che corrisponde ai criteri specificati.
- [`findByIdAndDelete()`](<https://mongoosejs.com/docs/api/model.html#Model.findByIdAndDelete()>), [`findByIdAndUpdate()`](<https://mongoosejs.com/docs/api/model.html#Model.findByIdAndUpdate()>), [`findOneAndRemove()`](<https://mongoosejs.com/docs/api/model.html#Model.findOneAndRemove()>), [`findOneAndUpdate()`](<https://mongoosejs.com/docs/api/model.html#Model.findOneAndUpdate()>): Trova un singolo documento per `id` o criteri e lo aggiorna o lo rimuove. Queste sono funzioni di convenienza utili per aggiornare e rimuovere i record.

> [!NOTE]
> Esiste anche un metodo [`countDocuments()`](<https://mongoosejs.com/docs/api/model.html#Model.countDocuments()>) che puoi usare per ottenere il numero di elementi che corrispondono alle condizioni. Questo è utile se vuoi eseguire un conteggio senza effettivamente recuperare i record.

C'è molto di più che puoi fare con le query. Per ulteriori informazioni, consulta: [Queries](https://mongoosejs.com/docs/queries.html) (documentazione di Mongoose).

#### Lavorare con documenti correlati — population

Puoi creare riferimenti da un'istanza di documento/modello a un'altra utilizzando il campo `ObjectId` dello schema, o da un documento a molti usando un array di `ObjectId`. Il campo memorizza l'id del modello correlato. Se hai bisogno del contenuto effettivo del documento associato, puoi usare il metodo [`populate()`](https://mongoosejs.com/docs/populate.html) in una query per sostituire l'id con i dati effettivi.

Ad esempio, il seguente schema definisce autori e storie. Ogni autore può avere più storie, che rappresentiamo come un array di `ObjectId`. Ogni storia può avere un solo autore. La proprietà `ref` dice allo schema quale modello può essere assegnato a questo campo.

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

Possiamo salvare i nostri riferimenti al documento correlato assegnando il valore `_id`. Sotto creiamo un autore, poi una storia, e assegniamo l'id dell'autore al campo autore della nostra storia.

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
> Uno dei grandi vantaggi di questo stile di programmazione è che non dobbiamo complicare il percorso principale del nostro codice con i controlli sugli errori.
> Se una delle operazioni `save()` fallisce, la promise verrà respinta e verrà sollevato un errore.
> Il nostro codice di gestione degli errori si occupa di ciò separatamente (di solito in un blocco `catch()`), quindi l'intento del nostro codice è molto chiaro.

Il nostro documento della storia ha ora un autore referenziato dall'ID del documento autore. Per ottenere le informazioni dell'autore nei risultati della storia usiamo [`populate()`](https://mongoosejs.com/docs/api/model.html#Model.populate), come mostrato di seguito.

```js
Story.findOne({ title: "Bob goes sledding" })
  .populate("author") // Replace the author id with actual author information in results
  .exec();
```

> [!NOTE]
> I lettori attenti noteranno che abbiamo aggiunto un autore alla nostra storia, ma non abbiamo fatto nulla per aggiungere la nostra storia all'array `stories` del nostro autore. Come possiamo quindi ottenere tutte le storie di un particolare autore? Un modo sarebbe quello di aggiungere la nostra storia all'array `stories`, ma questo risulterebbe nel dover mantenere due luoghi in cui le informazioni relative a autori e storie devono essere mantenute.
>
> Un modo migliore è ottenere l'`_id` del nostro _autore_, quindi usare `find()` per cercarlo nel campo autore in tutte le storie.
>
> ```js
> Story.find({ author: bob._id }).exec();
> ```

Questo è quasi tutto ciò che devi sapere su come lavorare con elementi correlati _per questo tutorial_. Per informazioni più dettagliate, consulta [Population](https://mongoosejs.com/docs/populate.html) (documentazione di Mongoose).

### Uno schema/modello per file

Sebbene tu possa creare schemi e modelli utilizzando qualsiasi struttura di file desideri, ti consigliamo vivamente di definire ciascun modello di schema nel proprio modulo (file), quindi esportare il metodo per creare il modello. Questo è mostrato di seguito:

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

Puoi poi richiedere e usare il modello immediatamente in altri file. Di seguito mostriamo come potresti usarlo per ottenere tutte le istanze del modello.

```js
// Create a SomeModel model just by requiring the module
const SomeModel = require("../models/some-model");

// Use the SomeModel object (model) to find all SomeModel records
const modelInstances = await SomeModel.find().exec();
```

## Configurazione del database MongoDB

Ora che comprendiamo qualcosa di ciò che Mongoose può fare e di come vogliamo progettare i nostri modelli, è il momento di iniziare a lavorare sul sito web _LocalLibrary_. La primissima cosa che vogliamo fare è configurare un database MongoDB che possiamo utilizzare per memorizzare i dati della nostra biblioteca.

Per questo tutorial, utilizzeremo il database ospitato nel cloud [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database). Questo tier del database non è considerato adatto per siti web di produzione perché non ha ridondanza, ma è eccellente per lo sviluppo e la prototipazione. Lo stiamo utilizzando qui perché è gratuito e facile da configurare e perché MongoDB Atlas è un popolare fornitore di _database come servizio_ che potresti ragionevolmente scegliere per il tuo database di produzione (altre scelte popolari al momento della scrittura includono [ScaleGrid](https://scalegrid.io/) e [ObjectRocket](https://www.objectrocket.com/)).

> [!NOTE]
> Se preferisci, puoi configurare un database MongoDB localmente scaricando e installando i [binaries appropriati per il tuo sistema](https://www.mongodb.com/try/download/community-edition/releases). Il resto delle istruzioni in questo articolo sarebbe simile, tranne che per l'URL del database che specificheresti quando ti connetti.
> Nel tutorial [Express Tutorial Parte 7: Distribuzione in Produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment) ospitiamo sia l'applicazione che il database su [Railway](https://railway.com/), ma avremmo potuto altrettanto bene utilizzare un database su [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database).

Avrai prima bisogno di [creare un account](https://www.mongodb.com/cloud/atlas/register) con MongoDB Atlas (questo è gratuito e richiede solo di inserire dettagli di contatto base e accettare i loro termini di servizio).

Dopo aver effettuato l'accesso, verrai portato alla schermata [home](https://cloud.mongodb.com/v2):

1. Clicca sul pulsante **+ Create** nella sezione _Overview_.

   ![Crea un database su MongoDB Atlas.](mongodb_atlas_-_createdatabase.jpg)

2. Questo aprirà la schermata _Deploy your cluster_. Clicca sull'opzione modello **M0 FREE**.

   ![Scegli un'opzione di distribuzione quando usi MongoDB Atlas.](mongodb_atlas_-_deploy.jpg)

3. Scorri la pagina per vedere le varie opzioni che puoi scegliere.
   ![Scegli un provider cloud quando usi MongoDB Atlas.](mongodb_atlas_-_createsharedcluster.jpg)

   - Puoi cambiare il nome del tuo Cluster in _Cluster Name_. Lo lasciamo come `Cluster0` per questo tutorial.
   - Deseleziona la casella _Preload sample dataset_, poiché importeremo i nostri dati di esempio più avanti.
   - Seleziona qualsiasi provider e regione dalle sezioni _Provider_ e _Region_. Regioni diverse offrono provider diversi.
   - I tag sono opzionali. Non li utilizzeremo qui.
   - Clicca sul pulsante **Create deployment** (la creazione del cluster richiederà alcuni minuti).

4. Questo aprirà la sezione _Security Quickstart_.
   ![Impostare le regole di accesso sulla schermata Security Quickstart su MongoDB Atlas.](mongodb_atlas_-_securityquickstart.jpg)

   - Immetti un nome utente e una password che la tua applicazione utilizzerà per accedere al database (sopra abbiamo creato un nuovo login "cooluser").
     Ricorda di copiare e memorizzare le credenziali in modo sicuro poiché ne avremo bisogno più avanti.
     Clicca sul pulsante **Create User**.

     > [!NOTE]
     > Evita di usare caratteri speciali nella password dell'utente MongoDB poiché mongoose potrebbe non interpretare correttamente la stringa di connessione.

   - Seleziona **Add by current IP address** per consentire l'accesso dal tuo computer corrente
   - Immetti `0.0.0.0/0` nel campo Indirizzo IP e quindi clicca sul pulsante **Add Entry**.
     Questo dice a MongoDB che vogliamo consentire l'accesso da qualsiasi luogo.

     > [!NOTE]
     > È buona pratica limitare gli indirizzi IP che possono connettersi al tuo database e ad altre risorse. Qui consentiamo una connessione da qualsiasi luogo poiché non sappiamo da dove arriverà la richiesta dopo la distribuzione.

   - Clicca sul pulsante **Finish and Close**.

5. Questo aprirà la seguente schermata. Clicca sul pulsante **Go to Overview**.
   ![Vai a Database dopo aver configurato le Regole di Accesso su MongoDB Atlas](mongodb_atlas_-_accessrules.jpg)

6. Verrai riportato alla schermata _Overview_. Clicca sulla sezione _Database_ sotto il menu _Deployment_ a sinistra. Clicca sul pulsante **Browse Collections**.
   ![Imposta una collezione su MongoDB Atlas.](mongodb_atlas_-_createcollection.jpg)

7. Questo aprirà la sezione _Collections_. Clicca sul pulsante **Add My Own Data**.
   ![Crea un database su MongoDB Atlas.](mongodb_atlas_-_adddata.jpg)

8. Questo aprirà la schermata _Create Database_.

   ![Dettagli durante la creazione del database su MongoDB Atlas.](mongodb_atlas_-_databasedetails.jpg)

   - Immetti il nome per il nuovo database come `local_library`.
   - Immetti il nome della collezione come `Collection0`.
   - Clicca sul pulsante **Create** per creare il database.

9. Verrai riportato alla schermata _Collections_ con il tuo database creato.
   ![Conferma della creazione del database su MongoDB Atlas.](mongodb_atlas_-_databasecreated.jpg)

   - Clicca sulla scheda _Overview_ per tornare alla panoramica del cluster.

10. Dalla schermata _Overview_ del Cluster0, clicca sul pulsante **Connect**.

    ![Configura la connessione dopo aver impostato un cluster su MongoDB Atlas.](mongodb_atlas_-_connectbutton.jpg)

11. Questo aprirà la schermata _Connect to Cluster0_.

    ![Scegli la connessione Short SRV quando imposti una connessione su MongoDB Atlas.](mongodb_atlas_-_connectforshortsrv.jpg)

    - Seleziona il tuo utente del database.
    - Seleziona la categoria _Drivers_, quindi il _Driver_ **Node.js** e la _Versione_ come mostrato.
    - **NON** installare il driver come suggerito.
    - Clicca sull'icona **Copy** per copiare la stringa di connessione.
    - Incolla questo nel tuo editor di testo locale.
    - Sostituisci il placeholder `<password>` nella stringa di connessione con la password del tuo utente.
    - Inserisci il nome del database "local_library" nel percorso prima delle opzioni (`...mongodb.net/local_library?retryWrites...`)
    - Salva il file contenente questa stringa in un posto sicuro.

Hai ora creato il database e hai un URL (con nome utente e password) che può essere utilizzato per accedervi. Questo sarà qualcosa come: `mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority&appName=Cluster0`

## Installare Mongoose

Apri un prompt dei comandi e naviga nella directory in cui hai creato il tuo [sito web Local Library scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website). Inserisci il seguente comando per installare Mongoose (e le sue dipendenze) e aggiungerlo al tuo file **package.json**, a meno che tu non l'abbia già fatto leggendo l'[Introduzione a Mongoose](#installazione_di_mongoose_e_mongodb) sopra.

```bash
npm install mongoose
```

## Connettersi a MongoDB

Apri **/app.js** (nella radice del tuo progetto) e copia il seguente testo sotto dove dichiari l'_oggetto applicazione Express_ (dopo la riga `const app = express();`). Sostituisci la stringa URL del database ('_insert_your_database_url_here_') con l'URL della posizione che rappresenta il tuo database (cioè, utilizzando le informazioni da _MongoDB Atlas_).

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

Come discusso nell'[Introduzione a Mongoose](#collegamento_a_mongodb) sopra, questo codice crea la connessione predefinita al database e segnala eventuali errori alla console.

Nota che non è consigliato inserire credenziali del database in codice sorgente come mostrato sopra. Lo facciamo qui perché mostra il codice di connessione core, e perché durante lo sviluppo non c'è un rischio significativo che la divulgazione di questi dettagli esponga o corrompa informazioni sensibili. Ti mostreremo come farlo in modo più sicuro quando [distribuiremo in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment#database_configuration)!

## Definizione dello Schema LocalLibrary

Definiremo un modulo separato per ciascun modello, come [discusso sopra](#one_schemamodel_per_file). Inizia creando una cartella per i nostri modelli nella radice del progetto (**/models**) e poi crea file separati per ciascuno dei modelli:

```plain
/express-locallibrary-tutorial  # the project root
  /models
    author.js
    book.js
    bookinstance.js
    genre.js
```

### Modello Author

Copia il codice dello schema `Author` mostrato sotto e incollalo nel tuo file **./models/author.js**. Lo schema definisce un autore come avente `String` SchemaTypes per il nome e il cognome (necessari, con un massimo di 100 caratteri), e campi `Date` per le date di nascita e morte.

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

Abbiamo anche dichiarato un [virtual](#proprietà_virtuali) per l'AuthorSchema chiamato "url" che restituisce l'URL assoluto richiesto per ottenere una particolare istanza del modello — useremo la proprietà nei nostri template ogni volta che avremo bisogno di ottenere un link a un autore particolare.

> [!NOTE]
> Dichiarare i nostri URL come virtual nel schema è una buona idea perché l'URL per un elemento deve essere modificato solo in un punto.
> A questo punto, un link che utilizza questo URL non funzionerebbe, perché non abbiamo ancora alcun codice di gestione dei percorsi per le istanze di modello individuali.
> Li imposteremo in un articolo successivo!

Alla fine del modulo, esportiamo il modello.

### Modello Book

Copia il codice dello schema `Book` mostrato sotto e incollalo nel tuo file **./models/book.js**. La maggior parte di questo è simile al modello autore — abbiamo dichiarato uno schema con una serie di campi stringa e un virtual per ottenere l'URL dei record di libri specifici, ed esportiamo il modello.

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

- author è un riferimento a un singolo oggetto modello `Author`, ed è obbligatorio.
- genre è un riferimento a un array di oggetti modello `Genre`. Non abbiamo ancora dichiarato questo oggetto!

### Modello BookInstance

Infine, copia il codice dello schema `BookInstance` mostrato sotto e incollalo nel tuo file **./models/bookinstance.js**. Il `BookInstance` rappresenta una copia specifica di un libro che qualcuno potrebbe prendere in prestito e include informazioni su se la copia è disponibile, in quale data è prevista la restituzione e dettagli "imprint" (o versione).

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

Le nuove cose che mostriamo qui sono le opzioni dei campi:

- `enum`: Questo ci consente di impostare i valori consentiti di una stringa. In questo caso, lo usiamo per specificare lo stato di disponibilità dei nostri libri (usare un enum ci permette di prevenire errori di ortografia e valori arbitrari per il nostro stato).
- `default`: Usiamo il valore predefinito per impostare lo stato predefinito per le nuove istanze di libro su "Maintenance" e la data `due_back` predefinita su `now` (nota come puoi chiamare la funzione Date quando imposti la data!).

Tutto il resto dovrebbe essere familiare dal nostro schema precedente.

### Modello Genre - sfida

Apri il tuo file **./models/genre.js** e crea uno schema per memorizzare i generi (la categoria del libro, es., se è narrativa o saggistica, romanzo o storia militare, ecc.).

La definizione sarà molto simile agli altri modelli:

- Il modello dovrebbe avere un tipo `String` SchemaType chiamato `name` per descrivere il genere.
- Questo nome dovrebbe essere obbligatorio e avere tra 3 e 100 caratteri.
- Dichiara un [virtual](#proprietà_virtuali) per l'URL del genere, chiamato `url`.
- Esporta il modello.

## Test — crea alcuni elementi

Ecco fatto. Ora abbiamo tutti i modelli per il sito configurati!

Per testare i modelli (e per creare alcuni libri di esempio e altri elementi che possiamo utilizzare nei nostri prossimi articoli) eseguiremo ora uno script _indipendente_ per creare elementi di ciascun tipo:

1. Scarica (o altrimenti crea) il file [populatedb.js](https://raw.githubusercontent.com/mdn/express-locallibrary-tutorial/main/populatedb.js) all'interno della tua directory _express-locallibrary-tutorial_ (allo stesso livello di `package.json`).

   > [!NOTE]
   > Il codice in `populatedb.js` può essere utile per apprendere JavaScript, ma non è necessario comprenderlo per questo tutorial.

2. Esegui lo script usando node nel tuo prompt dei comandi, passando l'URL del tuo database _MongoDB_ (lo stesso che hai sostituito al placeholder _insert_your_database_url_here_, all'interno di `app.js` in precedenza):

   ```bash
   node populatedb <your MongoDB url>
   ```

   > [!NOTE]
   > Su Windows devi avvolgere l'URL del database all'interno di doppie virgolette (").
   > Su altri sistemi operativi potresti aver bisogno di singole virgolette (').

3. Lo script dovrebbe eseguire fino alla completa, visualizzando gli elementi man mano che li crea nel terminale.

> [!NOTE]
> Vai al tuo database su MongoDB Atlas (nella scheda _Collections_).
> Dovresti ora essere in grado di esplorare le singole collezioni di Books, Authors, Genres e BookInstances, e controllare i singoli documenti.

## Riepilogo

In questo articolo, abbiamo imparato un po' sui database e gli ORM su Node/Express, e tanto su come sono definiti gli schema e i modelli Mongoose. Abbiamo poi utilizzato queste informazioni per progettare e implementare i modelli `Book`, `BookInstance`, `Author` e `Genre` per il sito web _LocalLibrary_.

Infine, abbiamo testato i nostri modelli creando un certo numero di istanze (usando uno script stand-alone). Nel prossimo articolo esamineremo come creare alcune pagine per visualizzare questi oggetti.

## Vedi anche

- [Integrazione del database](https://expressjs.com/en/guide/database-integration.html) (documentazione Express)
- [Sito web di Mongoose](https://mongoosejs.com/) (documentazione Mongoose)
- [Guida a Mongoose](https://mongoosejs.com/docs/guide.html) (documentazione Mongoose)
- [Validazione](https://mongoosejs.com/docs/validation.html) (documentazione Mongoose)
- [Tipi di Schema](https://mongoosejs.com/docs/schematypes.html) (documentazione Mongoose)
- [Modelli](https://mongoosejs.com/docs/models.html) (documentazione Mongoose)
- [Query](https://mongoosejs.com/docs/queries.html) (documentazione Mongoose)
- [Population](https://mongoosejs.com/docs/populate.html) (documentazione Mongoose)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
