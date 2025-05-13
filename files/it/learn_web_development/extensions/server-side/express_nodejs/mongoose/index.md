---
title: "Tutorial Express Parte 3: Utilizzare un Database con Mongoose"
short-title: "3: Utilizzo di database con Mongoose"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose
l10n:
  sourceCommit: 759102220c07fb140b3e06971cd5981d8f0f134f
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo articolo introduce brevemente i database, e come utilizzarli con app Node/Express. Viene poi mostrato come possiamo usare [Mongoose](https://mongoosejs.com/) per fornire accesso al database per il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Spiega come vengono dichiarati schema di oggetti e modelli, i principali tipi di campo e la validazione di base. Inoltre, viene brevemente mostrato alcuni dei modi principali in cui è possibile accedere ai dati del modello.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website">Tutorial Express Parte 2: Creare un sito scheletro</a>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Essere in grado di progettare e creare i propri modelli usando Mongoose.</td>
    </tr>
  </tbody>
</table>

## Panoramica

Il personale bibliotecario utilizzerà il sito web della Biblioteca Locale per memorizzare informazioni su libri e prestatori, mentre i membri della biblioteca lo utilizzeranno per sfogliare e cercare libri, verificare se ci sono copie disponibili, e quindi prenotarle o prenderle in prestito. Per memorizzare e recuperare le informazioni in modo efficiente, le memorizzeremo in un _database_.

Le app Express possono utilizzare molti database diversi, e ci sono diversi approcci per eseguire operazioni **C**rea, **R**icerca, **A**ggiorna e **C**ancella (CRUD). Questo tutorial fornisce una breve panoramica di alcune delle opzioni disponibili per poi mostrare in dettaglio i meccanismi particolari selezionati.

### Quali database posso usare?

Le app _Express_ possono utilizzare qualsiasi database supportato da _Node_ (Express stesso non definisce nessun comportamento aggiuntivo/specifico per la gestione dei database). Ci sono [molte opzioni popolari](https://expressjs.com/en/guide/database-integration.html), incluso PostgreSQL, MySQL, Redis, SQLite e MongoDB.

Quando si sceglie un database, dovreste considerare aspetti come il tempo alla produttività/curva di apprendimento, le prestazioni, la facilità di replica/backup, il costo, il supporto della comunità, ecc. Anche se non esiste un "miglior" database, quasi tutte le soluzioni popolari dovrebbero essere più che accettabili per un sito di piccole/medie dimensioni come la nostra Biblioteca Locale.

Per ulteriori informazioni sulle opzioni, vedere [Integrazione del database](https://expressjs.com/en/guide/database-integration.html) (documentazione Express).

### Qual è il modo migliore per interagire con un database?

Ci sono due approcci comuni per interagire con un database:

- Utilizzando il linguaggio di query nativo dei database, come SQL.
- Utilizzando un Object Relational Mapper ("ORM") o Object Document Mapper ("ODM"). Questi rappresentano i dati del sito web come oggetti JavaScript, che sono poi mappati sul database sottostante. Alcuni ORM e ODM sono legati a un database specifico, mentre altri forniscono un backend indipendente dal database.

Le migliori _performance_ possono essere ottenute usando SQL, o qualsiasi linguaggio di query supportato dal database. Gli Object mappers sono spesso più lenti perché utilizzano codice di traduzione per mappare tra oggetti e il formato del database, che potrebbe non utilizzare le query di database più efficienti (questo è particolarmente vero se il mapper supporta backend di database differenti, e deve fare maggiori compromessi su quanto riguarda le caratteristiche del database supportate).

Il vantaggio di usare un ORM/ODM è che i programmatori possono continuare a pensare in termini di oggetti JavaScript piuttosto che di semantica di database — questo è particolarmente vero se avete bisogno di lavorare con database differenti (sullo stesso o diversi siti web). Forniscono inoltre un luogo ovvio per eseguire la validazione dei dati.

> [!NOTE]
> Utilizzare ODM/ORM spesso porta a costi inferiori per sviluppo e manutenzione! A meno che non sia molto familiare con il linguaggio di query nativo o le prestazioni siano fondamentali, dovrebbe considerare seriamente di utilizzare un ODM.

### Quale ORM/ODM dovrei usare?

Ci sono molte soluzioni ODM/ORM disponibili sul sito del gestore pacchetti npm (controllate i tag [odm](https://www.npmjs.com/search?q=keywords:odm) e [orm](https://www.npmjs.com/search?q=keywords:orm) per un sottoinsieme!).

Alcune soluzioni che erano popolari al momento della stesura sono:

- [Mongoose](https://www.npmjs.com/package/mongoose): Mongoose è uno strumento di modellazione di oggetti [MongoDB](https://www.mongodb.com/) progettato per funzionare in un ambiente asincrono.
- [Waterline](https://www.npmjs.com/package/waterline): Un ORM estratto dal framework web [Sails](https://sailsjs.com/) basato su Express. Fornisce un'API uniforme per accedere a numerosi database diversi, inclusi Redis, MySQL, LDAP, MongoDB e Postgres.
- [Bookshelf](https://www.npmjs.com/package/bookshelf): Presenta sia interfacce basate su promesse che tradizionali di callback, fornendo supporto per transazioni, caricamento di relazioni ansiose/annidate ansiose, associazioni polimorfiche e supporto per relazioni uno-a-uno, uno-a-molti e molti-a-molti. Funziona con PostgreSQL, MySQL e SQLite3.
- [Objection](https://www.npmjs.com/package/objection): Rende il più facile possibile utilizzare tutta la potenza di SQL e del motore di database sottostante (supporta SQLite3, Postgres e MySQL).
- [Sequelize](https://www.npmjs.com/package/sequelize) è un ORM basato su promesse per Node.js e io.js. Supporta i dialetti PostgreSQL, MySQL, MariaDB, SQLite, e MSSQL e presenta solido supporto per le transazioni, relazioni, replica di lettura e molto altro.
- [Node ORM2](https://node-orm.readthedocs.io/en/latest/) è un Object Relationship Manager per NodeJS. Supporta MySQL, SQLite e Postgres, aiutando a lavorare con il database utilizzando un approccio orientato agli oggetti.
- [GraphQL](https://graphql.org/): Principalmente un linguaggio di query per API restful, GraphQL è molto popolare e ha funzionalità disponibili per la lettura dei dati dai database.

Come regola generale, dovreste considerare sia le funzionalità fornite sia l'"attività della comunità" (download, contributi, segnalazioni di bug, qualità della documentazione, ecc.) quando selezionate una soluzione. Al momento della stesura, Mongoose è di gran lunga l'ODM più popolare ed è una scelta ragionevole se state utilizzando MongoDB come vostro database.

### Utilizzare Mongoose e MongoDB per il LocalLibrary

Per l'esempio della _Biblioteca Locale_ (e il resto di questo argomento) useremo l'[ODM Mongoose](https://www.npmjs.com/package/mongoose) per accedere ai nostri dati della biblioteca. Mongoose funge da front end per [MongoDB](https://www.mongodb.com/company/what-is-mongodb), un database open source [NoSQL](https://en.wikipedia.org/wiki/NoSQL) che utilizza un modello di dati orientato ai documenti. Una "collezione" di "documenti" in un database MongoDB [è analoga](https://www.mongodb.com/docs/manual/core/databases-and-collections/) a una "tabella" di "righe" in un database relazionale.

Questa combinazione di ODM e database è estremamente popolare nella comunità Node, parzialmente perché il sistema di archiviazione dei documenti e delle query assomiglia molto a JSON, ed è perciò familiare agli sviluppatori JavaScript.

> [!NOTE]
> Non è necessario conoscere MongoDB per usare Mongoose, anche se parti della [documentazione di Mongoose](https://mongoosejs.com/docs/guide.html) _sono_ più facili da utilizzare e capire se si è già familiari con MongoDB.

Il resto di questo tutorial mostra come definire e accedere agli schema e modelli Mongoose per l'esempio del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website).

## Progettare i modelli LocalLibrary

Prima di iniziare a codificare i modelli, vale la pena prendersi qualche minuto per pensare a quali dati abbiamo bisogno di memorizzare e le relazioni tra i diversi oggetti.

Sappiamo che dobbiamo memorizzare informazioni sui libri (titolo, riassunto, autore, genere, ISBN) e che potremmo avere più copie disponibili (con ID globalmente unici, stati di disponibilità, ecc.). Potremmo avere bisogno di memorizzare più informazioni sull'autore oltre al solo nome, e potrebbe esserci più autori con nomi simili o uguali. Vogliamo essere in grado di ordinare le informazioni in base al titolo del libro, autore, genere e categoria.

Quando progettate i vostri modelli, ha senso avere modelli separati per ogni "oggetto" (un gruppo di informazioni correlate). In questo caso, alcune ovvie candidate per questi modelli sono libri, istanze di libri e autori.

Potreste anche voler utilizzare modelli per rappresentare le opzioni di lista di selezione (ad esempio, come una lista a discesa di scelte), piuttosto che codificare rigidamente le scelte nel sito web stesso — questo è raccomandato quando non tutte le opzioni sono conosciute in anticipo o potrebbero cambiare. Un buon esempio è un genere (ad esempio, fantasy, fantascienza, ecc.).

Una volta decisi i nostri modelli e campi, dobbiamo pensare alle relazioni tra di essi.

Con ciò in mente, il diagramma di associazione UML qui sotto mostra i modelli che definiremo in questo caso (come caselle). Come discusso sopra, abbiamo creato modelli per il libro (i dettagli generici del libro), l'istanza del libro (stato delle copie fisiche specifiche del libro disponibili nel sistema) e l'autore. Abbiamo deciso anche di avere un modello per il genere in modo che i valori possano essere creati dinamicamente. Abbiamo deciso di non avere un modello per lo `status` di `BookInstance` — codificheremo i valori accettabili perché non ci aspettiamo che cambino. All'interno di ciascuna casella, potete vedere il nome del modello, i nomi dei campi e i tipi, e anche i metodi e i loro tipi di ritorno.

Il diagramma mostra anche le relazioni tra i modelli, incluse le loro _molteplicità_. Le molteplicità sono i numeri nel diagramma che mostrano i numeri (massimi e minimi) di ciascun modello che possono essere presenti nella relazione. Ad esempio, la linea di collegamento tra le caselle mostra che `Book` e un `Genre` sono correlati. I numeri vicino al modello `Book` mostrano che un `Genre` deve avere zero o più `Book` (quanti ne vuoi), mentre i numeri all'altro capo della linea accanto al `Genre` mostrano che un libro può avere zero o più `Genre` associati.

> [!NOTE]
> Come discusso nel nostro [primer di Mongoose](#primer_su_mongoose) qui sotto, è spesso meglio avere il campo che definisce la relazione tra i documenti/modelli in un solo modello (è ancora possibile trovare la relazione inversa cercando l'`_id` associato nell'altro modello). Qui abbiamo scelto di definire la relazione tra `Book`/`Genre` e `Book`/`Author` nello schema di Book e la relazione tra `Book`/`BookInstance` nello Schema di `BookInstance`. Questa scelta è stata un po' arbitraria — potevamo ugualmente bene avremmo potuto avere il campo nell'altro schema.

![Modello della Libreria Mongoose con cardinalità corretta](library_website_-_mongoose_express.png)

> [!NOTE]
> La sezione successiva fornisce un primer di base che spiega come i modelli sono definiti e utilizzati. Mentre lo leggete, considerate come costruiremo ciascuno dei modelli nel diagramma sopra.

### Le API del database sono asincrone

I metodi del database per creare, trovare, aggiornare o cancellare i record sono asincroni. Questo significa che i metodi restituiscono immediatamente e il codice per gestire il successo o il fallimento del metodo viene eseguito in un momento successivo, quando l'operazione è completata. Un altro codice può essere eseguito mentre il server attende che l'operazione del database si completi, in modo che il server possa rimanere reattivo ad altre richieste.

JavaScript dispone di diversi meccanismi per supportare il comportamento asincrono. Storicamente, JavaScript si basava fortemente sul passaggio di [funzioni di callback](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing) ai metodi asincroni per gestire i casi di successo ed errore. Nel JavaScript moderno, i callback sono stati in gran parte sostituiti dalle [Promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise). Le Promise sono oggetti che vengono (immediatamente) restituiti da un metodo asincrono che rappresentano il suo stato futuro. Quando l'operazione è completa, l'oggetto Promise è "risolto" e risolve un oggetto che rappresenta il risultato dell'operazione o un errore.

Ci sono due modi principali per utilizzare le promesse per eseguire il codice quando una promessa è risolta, e raccomandiamo vivamente di leggere [Come utilizzare le promesse](/it/docs/Learn_web_development/Extensions/Async_JS/Promises) per una panoramica ad alto livello di entrambi gli approcci. In questo tutorial, utilizzeremo principalmente [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) per attendere il completamento della promise all'interno di una [`funzione async`](/it/docs/Web/JavaScript/Reference/Statements/async_function), perché questo porta a un codice asincrono più leggibile e comprensibile.

Il modo in cui funziona questo approccio è che si utilizza la parola chiave `async function` per contrassegnare una funzione come asincrona, e quindi all'interno di quella funzione si applica `await` a qualsiasi metodo che restituisce una promessa. Quando la funzione asincrona è eseguita, la sua operazione è sospesa al primo metodo `await` fino a quando la promessa è risolta. Dal punto di vista del codice circostante, la funzione asincrona ritorna e il codice dopo di essa è in grado di eseguire. Più tardi, quando la promessa è risolta, il metodo `await` all'interno della funzione asincrona restituisce il risultato, o viene generato un errore se la promessa è stata rifiutata. Il codice nella funzione asincrona viene eseguito fino a quando non viene incontrato un altro `await`, a quel punto si sospenderà nuovamente, o fino a quando tutto il codice nella funzione è stato eseguito.

Potete vedere come funziona nell'esempio qui sotto. `myFunction()` è una funzione asincrona chiamata all'interno di un blocco [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch). Quando `myFunction()` è eseguita, l'esecuzione del codice è sospesa in `methodThatReturnsPromise()` fino a quando la promessa è risolta, a quel punto il codice continua in `aFunctionThatReturnsPromise()` e aspetta di nuovo. Il codice nel blocco `catch` viene eseguito se viene generato un errore nella funzione asincrona, e questo accadrà se la promessa restituita da uno dei metodi è stata rifiutata.

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

I metodi asincroni sopra sono eseguiti in sequenza. Se i metodi non dipendono l'uno dall'altro, è possibile eseguirli in parallelo e concludere l'intera operazione più rapidamente. Questo è fatto utilizzando il metodo [`Promise.all()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise/all), che prende come input un iterabile di promesse e restituisce una singola `Promise`. Questa promessa restituita si realizza quando tutte le promesse di ingresso si realizzano, con un array di valori di realizzazione. Si rifiuta quando una delle promesse di input si rifiuta, con questo primo motivo di rifiuto.

Il codice seguente mostra come funziona. Innanzitutto, abbiamo due funzioni che restituiscono le promesse. Attendiamo che entrambe siano completate utilizzando la promessa restituita da `Promise.all()`. Una volta che entrambe sono completate, `await` restituisce e l'array dei risultati viene popolato, la funzione continua quindi al prossimo `await`, e attende fino a quando la promessa restituita da `anotherFunctionThatReturnsPromise()` è risolta. Dovreste chiamare `myFunction()` in un blocco `try...catch` per intercettare eventuali errori.

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

Le promesse con `await`/`async` consentono sia il controllo flessibile che "comprensibile" sull'esecuzione asincrona!

## Primer su Mongoose

Questa sezione fornisce una panoramica su come connettere Mongoose a un database MongoDB, come definire uno schema e un modello, e come eseguire query di base.

> [!NOTE]
> Questo primer è fortemente influenzato dall'[introduzione rapida di Mongoose](https://www.npmjs.com/package/mongoose) su _npm_ e dalla [documentazione ufficiale](https://mongoosejs.com/docs/guide.html).

### Installazione di Mongoose e MongoDB

Mongoose è installato nel vostro progetto (**package.json**) come qualsiasi altra dipendenza — usando npm. Per installarlo, utilizzare il seguente comando all'interno della cartella del vostro progetto:

```bash
npm install mongoose
```

Installare _Mongoose_ aggiunge tutte le sue dipendenze, incluso il driver del database MongoDB, ma non installa MongoDB stesso. Se desiderate installare un server MongoDB, è possibile [scaricare gli installer da qui](https://www.mongodb.com/try/download/community) per vari sistemi operativi e installarlo localmente. È inoltre possibile utilizzare istanze MongoDB basate su cloud.

> [!NOTE]
> Per questo tutorial, utilizzeremo il tier gratuito del servizio cloud _database as a service_ [MongoDB Atlas](https://www.mongodb.com/) per fornire il database. Questo è adatto per lo sviluppo e ha senso per il tutorial perché rende l'"installazione" indipendente dal sistema operativo (il database-as-a-service è anche un approccio che potreste utilizzare per il vostro database in produzione).

### Connessione a MongoDB

_Mongoose_ richiede una connessione a un database MongoDB. Si può `require()` e connettere a un database ospitato localmente con `mongoose.connect()` come mostrato di seguito (per il tutorial ci connetteremo invece a un database ospitato su internet).

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
> Come discusso nella sezione [Le API del database sono asincrone](#le_api_del_database_sono_asincrone), qui attendiamo la promessa restituita dal metodo `connect()` all'interno di una `funzione async`.
> Usiamo il gestore di promise `catch()` per gestire eventuali errori durante il tentativo di connessione, ma potremmo anche aver chiamato `main()` all'interno di un blocco `try...catch`.

È possibile ottenere l'oggetto di `Connection` predefinito con `mongoose.connection`. Se avete bisogno di creare connessioni aggiuntive, potete usare `mongoose.createConnection()`. Questo richiede lo stesso tipo di URI di database (con host, database, porta, opzioni, ecc.) di `connect()` e restituisce un oggetto `Connection`. Si tenga presente che `createConnection()` restituisce immediatamente; se avete bisogno di attendere che la connessione sia stabilita, potete chiamarlo con `asPromise()` per restituire una promessa (`mongoose.createConnection(mongoDB).asPromise()`).

### Definire e creare modelli

I modelli sono _definiti_ utilizzando l'interfaccia `Schema`. Lo Schema consente di definire i campi memorizzati in ciascun documento, insieme ai loro requisiti di validazione e valori predefiniti. Inoltre, è possibile definire metodi di aiuto statici e di istanza per facilitare il lavoro con i propri tipi di dati, nonché proprietà virtuali che è possibile utilizzare come qualsiasi altro campo, ma che non vengono effettivamente memorizzate nel database (ne discuteremo ulteriormente sotto).

Gli schemi sono quindi "compilati" in modelli utilizzando il metodo `mongoose.model()`. Una volta che avete un modello, potete usarlo per trovare, creare, aggiornare e cancellare oggetti del tipo dato.

> [!NOTE]
> Ogni modello è mappato a una _collezione_ di _documenti_ nel database MongoDB. I documenti conterranno i campi/tipi di schema definiti nel modello `Schema`.

#### Definire gli schemi

Il frammento di codice qui sotto mostra come potreste definire un semplice schema. Prima si `require()` mongoose, quindi si utilizza il costruttore Schema per creare un nuova istanza di schema, definendo i vari campi all'interno del parametro oggetto del costruttore.

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

Nel caso sopra, abbiamo solo due campi, una stringa e una data. Nelle sezioni successive, mostreremo alcuni degli altri tipi di campo, la validazione e altri metodi.

#### Creare un modello

I modelli sono creati dagli schemi utilizzando il metodo `mongoose.model()`:

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

Il primo argomento è il nome singolare della collezione che verrà creata per il vostro modello (Mongoose creerà la collezione di database per il modello _SomeModel_ sopra), e il secondo argomento è lo schema che si desidera utilizzare per creare il modello.

> [!NOTE]
> Una volta definiti i vostri modelli di classi, è possibile utilizzarli per creare, aggiornare o cancellare record ed eseguire query per ottenere tutti i record o particolari sottoinsiemi di record. Vi mostreremo come farlo nella sezione [Utilizzare i modelli](#utilizzare_i_modelli) e quando creeremo le nostre viste.

#### Tipi di Schema (campi)

Uno schema può avere un numero arbitrario di campi — ciascuno rappresenta un campo nei documenti memorizzati in _MongoDB_. Un esempio di schema che mostra molti dei tipi di campo comuni e come vengono dichiarati è mostrato sotto.

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

- `ObjectId`: Rappresenta istanze specifiche di un modello nel database. Ad esempio, un libro potrebbe usarlo per rappresentare il suo oggetto autore. Questo conterrà effettivamente l'ID univoco (`_id`) per l'oggetto specificato. Possiamo utilizzare il metodo `populate()` per richiamare le informazioni associate quando necessario.
- [`Mixed`](https://mongoosejs.com/docs/schematypes.html#mixed): Un tipo di schema arbitrario.
- `[]`: Un array di elementi. È possibile eseguire operazioni di array JavaScript su questi modelli (push, pop, unshift, ecc.). Gli esempi sopra mostrano un array di oggetti senza un tipo specificato e un array di oggetti `String`, ma si può avere un array di qualsiasi tipo di oggetto.

Il codice mostra anche entrambi i modi di dichiarare un campo:

- Nome del campo e tipo come coppia chiave-valore (cioè come fatto con i campi `name`, `binary` e `living`).
- Nome del campo seguito da un oggetto che definisce il `type` e tutte le altre _opzioni_ per il campo. Le opzioni includono cose come:

  - valori di default.
  - validatori integrati (ad esempio, valori max/min) e funzioni di validazione personalizzate.
  - Se il campo è obbligatorio
  - Se i campi `String` dovrebbero essere automaticamente impostati in minuscolo, maiuscolo o tagliati (ad esempio, `{ type: String, lowercase: true, trim: true }`)

Per ulteriori informazioni sulle opzioni, vedere [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) (documentazione di Mongoose).

#### Validazione

Mongoose fornisce validatori integrati e personalizzati, nonché validatori sincroni e asincroni. Consente di specificare sia il range accettabile di valori sia il messaggio di errore per il fallimento della validazione in tutti i casi.

I validatori integrati includono:

- Tutti gli [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) hanno il validatore built-in [required](https://mongoosejs.com/docs/api.html#schematype_SchemaType-required). Questo è usato per specificare se il campo deve essere fornito per salvare un documento.
- [Numeri](https://mongoosejs.com/docs/api/schemanumber.html) hanno validatori [min](<https://mongoosejs.com/docs/api/schemanumber.html#SchemaNumber.prototype.min()>) e [max](<https://mongoosejs.com/docs/api/schemanumber.html#SchemaNumber.prototype.max()>).
- [Stringhe](https://mongoosejs.com/docs/api/schemastring.html) hanno:

  - [enum](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.enum()>): specifica l'insieme dei valori consentiti per il campo.
  - [match](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.match()>): specifica un'espressione regolare che la stringa deve corrispondere.
  - [maxLength](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.maxlength()>) e [minLength](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.minlength()>) per la stringa.

L'esempio qui sotto (leggermente modificato dai documenti di Mongoose) mostra come potete specificare alcuni dei tipi di validatori e messaggi di errore:

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

Per informazioni complete sulla validazione dei campi, vedere [Validazione](https://mongoosejs.com/docs/validation.html) (documentazione di Mongoose).

#### Proprietà virtuali

Le proprietà virtuali sono proprietà del documento che potete ottenere e impostare ma che non vengono persistite in MongoDB. I getter sono utili per formattare o combinare campi, mentre i setter sono utili per scomporre un singolo valore in più valori da memorizzare. L'esempio nella documentazione costruisce (e decompone) una proprietà virtuale del nome completo da un campo del nome e del cognome, che è più facile e pulito di costruire un nome completo ogni volta che viene utilizzato in un modello.

> [!NOTE]
> Utilizzeremo una proprietà virtuale nella libreria per definire un URL univoco per ciascun record del modello utilizzando un percorso e il valore `_id` del record.

Per ulteriori informazioni, vedere [Virtuali](https://mongoosejs.com/docs/guide.html#virtuals) (documentazione di Mongoose).

#### Metodi e helper di query

Uno schema può anche avere [metodi di istanza](https://mongoosejs.com/docs/guide.html#methods), [metodi statici](https://mongoosejs.com/docs/guide.html#statics), e [helper di query](https://mongoosejs.com/docs/guide.html#query-helpers). I metodi di istanza e statica sono simili, ma con la differenza ovvia che un metodo di istanza è associato a un particolare record e ha accesso all'oggetto corrente. Gli helper di query consentono di estendere l'API [costruttore di query concatenabili di Mongoose](https://mongoosejs.com/docs/queries.html) (ad esempio, consentendo di aggiungere una query "byName" oltre ai metodi `find()`, `findOne()` e `findById()`).

### Utilizzare i modelli

Una volta creato uno schema, potete utilizzarlo per creare modelli. Il modello rappresenta una collezione di documenti nel database che potete cercare, mentre le istanze del modello rappresentano singoli documenti che potete salvare e recuperare.

Forniamo una breve panoramica qui sotto. Per ulteriori informazioni vedere: [Modelli](https://mongoosejs.com/docs/models.html) (documentazione di Mongoose).

> [!NOTE]
> La creazione, l'aggiornamento, la cancellazione e l'interrogazione dei record sono operazioni asincrone che restituiscono una [promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise).
> Gli esempi seguono mostrano solo l'uso dei metodi rilevanti e `await` (cioè, il codice essenziale per utilizzare i metodi).
> La `funzione async` circostante e il blocco `try...catch` per catturare gli errori sono omessi per chiarezza.
> Per ulteriori informazioni sull'utilizzo di `await/async` vedere [Le API del database sono asincrone](#le_api_del_database_sono_asincrone) sopra.

#### Creare e modificare documenti

Per creare un record, è possibile definire un'istanza del modello e quindi chiamare [`save()`](https://mongoosejs.com/docs/api/model.html#Model.prototype.save) su di esso. Gli esempi seguenti presumono che `SomeModel` sia un modello (con un solo campo `name`) che abbiamo creato dal nostro schema.

```js
// Create an instance of model SomeModel
const awesome_instance = new SomeModel({ name: "awesome" });

// Save the new model instance asynchronously
await awesome_instance.save();
```

È possibile anche utilizzare [`create()`](https://mongoosejs.com/docs/api/model.html#Model.create) per definire l'istanza del modello contemporaneamente al salvataggio. Di seguito creiamo solo uno, ma potete creare più istanze passando un array di oggetti.

```js
await SomeModel.create({ name: "also_awesome" });
```

Ogni modello ha una connessione associata (questa sarà la connessione predefinita quando si utilizza `mongoose.model()`). Create una nuova connessione e chiamate `.model()` su di essa per creare i documenti su un diverso database.

Potete accedere ai campi in questo nuovo record utilizzando la sintassi del punto e modificare i valori. È necessario chiamare `save()` o `update()` per memorizzare i valori modificati nel database.

```js
// Access model field values using dot notation
console.log(awesome_instance.name); // should log 'also_awesome'

// Change record by modifying the fields, then calling save().
awesome_instance.name = "New cool name";
await awesome_instance.save();
```

#### Ricerca di record

Potete cercare i record utilizzando i metodi di query, specificando le condizioni di query come un documento JSON. Il frammento di codice qui sotto mostra come potresti trovare tutti gli atleti in un database che giocano a tennis, restituendo solo i campi per _name_ e _age_ dell'atleta. Qui, specifichiamo solo un campo corrispondente (sport) ma potete aggiungere più criteri, specificare criteri di espressione regolare o rimuovere completamente le condizioni per restituire tutti gli atleti.

```js
const Athlete = mongoose.model("Athlete", yourSchema);

// find all athletes who play tennis, returning the 'name' and 'age' fields
const tennisPlayers = await Athlete.find(
  { sport: "Tennis" },
  "name age",
).exec();
```

> [!NOTE]
> È importante ricordare che non trovare alcun risultato **non è un errore** per una ricerca — ma potrebbe essere un caso di fallimento nel contesto della vostra applicazione.
> Se la vostra applicazione si aspetta che una ricerca trovi un valore, potete controllare il numero di voci restituite nel risultato.

Le API di query, come [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>), restituiscono una variabile di tipo [Query](https://mongoosejs.com/docs/api/query.html). Potete usare un oggetto di query per costruire una query in parti prima di eseguirla con il metodo [`exec()`](https://mongoosejs.com/docs/api/query.html#Query.prototype.exec). `exec()` esegue la query e restituisce una promessa sulla quale potete `await` per ottenere il risultato.

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

Sopra, abbiamo definito le condizioni della query nel metodo [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>). Possiamo anche farlo utilizzando una funzione [`where()`](<https://mongoosejs.com/docs/api/model.html#Model.where()>), e possiamo concatenare tutte le parti della nostra query insieme utilizzando l'operatore punto (.) invece di aggiungerle separatamente. Il frammento di codice qui sotto è lo stesso della nostra query sopra, con una condizione aggiuntiva per l'età.

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

Il metodo [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>) ottiene tutti i record corrispondenti, ma spesso si desidera ottenere solo una corrispondenza. I seguenti metodi interrogano un singolo record:

- [`findById()`](<https://mongoosejs.com/docs/api/model.html#Model.findById()>): Trova il documento con l'`id` specificato (ogni documento ha un `id` univoco).
- [`findOne()`](<https://mongoosejs.com/docs/api/model.html#Model.findOne()>): Trova un singolo documento che corrisponde ai criteri specificati.
- [`findByIdAndDelete()`](<https://mongoosejs.com/docs/api/model.html#Model.findByIdAndDelete()>), [`findByIdAndUpdate()`](<https://mongoosejs.com/docs/api/model.html#Model.findByIdAndUpdate()>), [`findOneAndRemove()`](<https://mongoosejs.com/docs/api/model.html#Model.findOneAndRemove()>), [`findOneAndUpdate()`](<https://mongoosejs.com/docs/api/model.html#Model.findOneAndUpdate()>): Trova un singolo documento per `id` o criteri e, o lo aggiorna, o lo rimuove. Questi sono funzioni di comodità utili per aggiornare e rimuovere record.

> [!NOTE]
> C'è anche un metodo [`countDocuments()`](<https://mongoosejs.com/docs/api/model.html#Model.countDocuments()>) che potete usare per ottenere il numero di elementi che corrispondono alle condizioni. Questo è utile se volete eseguire un conteggio senza effettivamente recuperare i record.

C'è molto di più che potete fare con le query. Per ulteriori informazioni, vedere: [Query](https://mongoosejs.com/docs/queries.html) (documentazione di Mongoose).

#### Lavorare con documenti correlati — popolazione

Potete creare riferimenti da un documento/istanza di modello a un altro utilizzando il campo di schema `ObjectId`, o da un documento a molti utilizzando un array di `ObjectIds`. Il campo memorizza l'identificativo del modello correlato. Se avete bisogno del contenuto effettivo del documento associato, potete utilizzare il metodo [`populate()`](https://mongoosejs.com/docs/populate.html) in una query per sostituire l'id con i dati effettivi.

Ad esempio, il seguente schema definisce autori e storie. Ogni autore può avere più storie, che rappresentiamo come un array di `ObjectId`. Ogni storia può avere un singolo autore. La proprietà `ref` dice allo schema quale modello può essere assegnato a questo campo.

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

Possiamo salvare i nostri riferimenti al documento correlato assegnando il valore `_id`. Di seguito creiamo un autore, poi una storia e assegniamo l'id autore al campo autore della nostra storia.

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
> Un grande vantaggio di questo stile di programmazione è che non dobbiamo complicare il percorso principale del nostro codice con il controllo degli errori.
> Se una qualsiasi delle operazioni `save()` fallisce, la promise rifiuterà e verrà generato un errore.
> Il nostro codice di gestione degli errori si occupa di ciò separatamente (di solito in un blocco `catch()`), quindi l'intento del nostro codice è molto chiaro.

Il nostro documento della storia ha ora un autore referenziato dall'ID del documento dell'autore. Per ottenere le informazioni dell'autore nei risultati della storia utilizziamo [`populate()`](https://mongoosejs.com/docs/api/model.html#Model.populate), come mostrato di seguito.

```js
Story.findOne({ title: "Bob goes sledding" })
  .populate("author") // Replace the author id with actual author information in results
  .exec();
```

> [!NOTE]
> I lettori attenti noteranno che abbiamo aggiunto un autore alla nostra storia, ma non abbiamo fatto nulla per aggiungere la nostra storia all'array `stories` del nostro autore. Come possiamo allora ottenere tutte le storie di un determinato autore? Un modo sarebbe quello di aggiungere la nostra storia all'array di storie, ma questo comporterebbe che dobbiamo mantenere le informazioni che riguardano autori e storie in due posti.
>
> Un modo migliore è quello di ottenere l'`_id` del nostro _autore_, quindi utilizzare `find()` per cercare questo nel campo autore attraverso tutte le storie.
>
> ```js
> Story.find({ author: bob._id }).exec();
> ```

Questo è quasi tutto ciò che c'è da sapere su lavorare con elementi correlati _per questo tutorial_. Per informazioni più dettagliate vedere [Popolazione](https://mongoosejs.com/docs/populate.html) (documentazione di Mongoose).

### Uno schema/modello per file

Anche se è possibile creare schemi e modelli utilizzando qualsiasi struttura di file che si vuole, consigliamo vivamente di definire il modello schema in un proprio modulo (file), quindi esportare il metodo per creare il modello. Questo è mostrato di seguito:

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

Potete quindi require e utilizzare il modello immediatamente in altri file. Di seguito mostriamo come potresti utilizzarlo per ottenere tutte le istanze del modello.

```js
// Create a SomeModel model just by requiring the module
const SomeModel = require("../models/some-model");

// Use the SomeModel object (model) to find all SomeModel records
const modelInstances = await SomeModel.find().exec();
```

## Configurare il database MongoDB

Ora che comprendiamo meglio cosa può fare Mongoose e come vogliamo progettare i nostri modelli, è tempo di iniziare a lavorare sul sito web del _LocalLibrary_. La prima cosa che vogliamo fare è configurare un database MongoDB che possiamo utilizzare per memorizzare i nostri dati della biblioteca.

Per questo tutorial, useremo il database sandbox ospitato su cloud [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database). Questo livello di database non è considerato adatto per siti web in produzione perché non ha ridondanza, ma è ottimo per lo sviluppo e la prototipazione. Lo utilizziamo qui perché è gratuito e facile da configurare, e perché MongoDB Atlas è un fornitore popolare di _database as a service_ che potreste ragionevolmente scegliere per il vostro database di produzione (altre scelte popolari al momento della scrittura includono [ScaleGrid](https://scalegrid.io/) e [ObjectRocket](https://www.objectrocket.com/)).

> [!NOTE]
> Se preferite, potete configurare un database MongoDB localmente scaricando e installando i [binarie appropriati per il vostro sistema](https://www.mongodb.com/try/download/community-edition/releases). Il resto delle istruzioni in questo articolo sarebbero simili, eccetto per l'URL del database che specifichereste quando vi connettete.
> Nel tutorial [Tutorial Express Parte 7: Deployment a Produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment) ospitiamo sia l'applicazione che il database su [Railway](https://railway.com/), ma avremmo potuto utilizzare anche un database su [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database).

Prima avrai bisogno di [creare un account](https://www.mongodb.com/cloud/atlas/register) con MongoDB Atlas (questo è gratuito, e richiede solo che inseriate dettagli di contatto di base e accettiate i loro termini di servizio).

Dopo aver effettuato l'accesso, sarete portati alla schermata [home](https://cloud.mongodb.com/v2):

1. Cliccate sul pulsante **+ Create** nella sezione _Overview_.

   ![Creare un database su MongoDB Atlas.](mongodb_atlas_-_createdatabase.jpg)

2. Questo aprirà la schermata _Deploy your cluster_. Cliccate sull'opzione modello **M0 FREE**.

   ![Scegliere un'opzione di deployment quando si utilizza MongoDB Atlas.](mongodb_atlas_-_deploy.jpg)

3. Scorrete verso il basso la pagina per vedere le diverse opzioni che potete scegliere.
   ![Scegliere un fornitore di cloud quando si utilizza MongoDB Atlas.](mongodb_atlas_-_createsharedcluster.jpg)

   - Potete cambiare il nome del vostro Cluster sotto il campo _Cluster Name_. Lo manteniamo come `Cluster0` per questo tutorial.
   - Deselezionate la casella _Preload sample dataset_, poiché importeremo i nostri dati di esempio in seguito.
   - Selezionate un qualsiasi provider e regione dalle sezioni _Provider_ e _Region_. Diverse regioni offrono diversi provider.
   - I tag sono opzionali. Non li utilizziamo qui.
   - Cliccate sul pulsante **Create deployment** (la creazione del cluster richiederà alcuni minuti).

4. Questo aprirà la sezione _Security Quickstart_.
   ![Configurare le regole di accesso nella schermata Security Quickstart su MongoDB Atlas.](mongodb_atlas_-_securityquickstart.jpg)

   - Inserite un nome utente e una password che la vostra applicazione utilizzerà per accedere al database (sopra abbiamo creato un login "cooluser").
     Ricordatevi di copiare e memorizzare in modo sicuro le credenziali poiché ne avremo bisogno in seguito.
     Cliccate sul pulsante **Create User**.

     > [!NOTE]
     > Evitate di usare caratteri speciali nella vostra password utente MongoDB poiché mongoose potrebbe non interpretare correttamente la stringa di connessione.

   - Selezionate **Add by current IP address** per consentire l'accesso dal vostro attuale computer.
   - Inserite `0.0.0.0/0` nel campo Indirizzo IP e quindi cliccate sul pulsante **Add Entry**.
     Questo dice a MongoDB che vogliamo consentire l'accesso da ovunque.

     > [!NOTE]
     > È una buona pratica limitare gli indirizzi IP che possono connettersi al vostro database e ad altre risorse. Qui consentiamo una connessione da ovunque perché non sappiamo da dove arriverà la richiesta dopo il deployment.

   - Cliccate sul pulsante **Finish and Close**.

5. Questo aprirà la seguente schermata. Cliccate sul pulsante **Go to Overview**.
   ![Accedere ai Database dopo aver configurato le Regole di Accesso su MongoDB Atlas](mongodb_atlas_-_accessrules.jpg)

6. Verrete riportati alla schermata _Overview_. Cliccate sulla sezione _Database_ nel menu _Deployment_ a sinistra. Cliccate sul pulsante **Browse Collections**.
   ![Configurare una collezione su MongoDB Atlas.](mongodb_atlas_-_createcollection.jpg)

7. Questo aprirà la sezione _Collections_. Cliccate sul pulsante **Add My Own Data**.
   ![Creare un database su MongoDB Atlas.](mongodb_atlas_-_adddata.jpg)

8. Questo aprirà la schermata _Create Database_.

   ![Dettagli durante la creazione del database su MongoDB Atlas.](mongodb_atlas_-_databasedetails.jpg)

   - Inserite il nome del nuovo database come `local_library`.
   - Inserite il nome della collezione come `Collection0`.
   - Cliccate sul pulsante **Create** per creare il database.

9. Sarete riportati alla schermata _Collections_ con il vostro database creato.
   ![Conferma della creazione del database su MongoDB Atlas.](mongodb_atlas_-_databasecreated.jpg)

   - Cliccate sulla scheda _Overview_ per tornare alla panoramica del cluster.

10. Dalla schermata `Overview` di Cluster0 cliccare sul pulsante **Connect**.

    ![Configurare la connessione dopo aver configurato un cluster in MongoDB Atlas.](mongodb_atlas_-_connectbutton.jpg)

11. Questo aprirà la schermata _Connect to Cluster0_.

    ![Scegliere Short SRV connection quando si configura una connessione su MongoDB Atlas.](mongodb_atlas_-_connectforshortsrv.jpg)

    - Selezionate il vostro utente del database.
    - Selezionate la categoria _Drivers_, quindi il _Driver_ **Node.js** e _Versione_ come mostrato.
    - **NON** installate il driver come suggerito.
    - Cliccate sull'icona **Copy** per copiare la stringa di connessione.
    - Incollate questa nel vostro editor di testo locale.
    - Sostituite il segnaposto `<password>` nella stringa di connessione con la password del vostro utente.
    - Inserite il nome del database "local_library" nel percorso prima delle opzioni (`...mongodb.net/local_library?retryWrites...`)
    - Salvate il file contenente questa stringa in un luogo sicuro.

Avete ora creato il database e disponete di un URL (con nome utente e password) che può essere utilizzato per accedervi. Questo sarà qualcosa come: `mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority&appName=Cluster0`

## Installare Mongoose

Aprite un prompt dei comandi e navigate nella directory in cui avete creato il vostro [sito scheletro della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website). Inserite il seguente comando per installare Mongoose (e le sue dipendenze) e aggiungerlo al vostro file **package.json**, a meno che non l'abbiate già fatto durante la lettura del [Primer su Mongoose](#installazione_di_mongoose_e_mongodb) sopra.

```bash
npm install mongoose
```

## Connettersi a MongoDB

Aprite **/app.js** (nella radice del vostro progetto) e copiate il seguente testo sotto dove dichiarate l'oggetto _Express application_ (dopo la riga `const app = express();`). Sostituite la stringa dell'URL del database ('_insert_your_database_url_here_') con l'URL del rappresentante del proprio database (cioè, utilizzando le informazioni ottenute da _MongoDB Atlas_).

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

Come discusso nel [primer di Mongoose](#connessione_a_mongodb) sopra, questo codice crea la connessione predefinita al database e riporta alla console eventuali errori.

Si noti che codificare le credenziali del database nel codice sorgente come mostrato sopra non è raccomandato. Lo facciamo qui perché mostra il codice di connessione principale e perché durante lo sviluppo non c'è un rischio significativo che divulgare questi dettagli esponga o danneggi informazioni sensibili. Vi mostreremo come farlo in modo più sicuro quando [effettueremo il deployment in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment#database_configuration)!

## Definire lo Schema di LocalLibrary

Definiremo un modulo separato per ciascun modello, come [discusso sopra](#one_schemamodel_per_file). Iniziate creando una cartella per i nostri modelli nella radice del progetto (**/models**) e poi create file separati per ciascuno dei modelli:

```plain
/express-locallibrary-tutorial  # the project root
  /models
    author.js
    book.js
    bookinstance.js
    genre.js
```

### Modello Author

Copiate il codice dello schema `Author` qui sotto e incollatelo nel vostro file **./models/author.js**. Lo schema definisce un autore come avente `String` SchemaTypes per nome e cognome (richiesti, con un massimo di 100 caratteri) e campi `Date` per le date di nascita e morte.

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

Abbiamo dichiarato anche una [proprietà virtuale](#proprietà_virtuali) per il AuthorSchema chiamata "url" che restituisce l'URL assoluto necessario per ottenere un'istanza particolare del modello — useremo questa proprietà nei nostri template ogni volta che abbiamo bisogno di un link a un autore particolare.

> [!NOTE]
> Dichiarare i nostri URL come una proprietà virtuale nello schema è una buona idea perché allora l'URL di un elemento deve solo essere cambiato in un unico posto.
> A questo punto, un link che utilizza questo URL non funzionerebbe, poiché non abbiamo ancora alcun codice per la gestione delle rotte per singole istanze di modello.
> Le configureremo in un articolo successivo!

Alla fine del modulo, esportiamo il modello.

### Modello Book

Copiate il codice dello schema `Book` qui sotto e incollatelo nel vostro file **./models/book.js**. La maggior parte di questo è simile al modello autore — abbiamo dichiarato uno schema con un numero di campi stringa e una proprietà virtuale per ottenere l'URL di record di libro specifici, e abbiamo esportato il modello.

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

Infine, copiate il codice dello schema `BookInstance` qui sotto e incollatelo nel vostro file **./models/bookinstance.js**. Il `BookInstance` rappresenta una copia specifica di un libro che qualcuno potrebbe prendere in prestito e include informazioni sul fatto che la copia sia disponibile, su quale data è prevista la restituzione e dettagli di "imprint" (o versione).

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

- `enum`: Questo ci consente di impostare i valori consentiti di una stringa. In questo caso, lo utilizziamo per specificare lo stato di disponibilità dei nostri libri (usare un enum significa che possiamo prevenire errori di ortografia e valori arbitrari per il nostro status).
- `default`: Usare default per impostare lo stato predefinito delle istanze di libro appena create su "Maintenance" e la data predefinita `due_back` su `now` (notate come potete chiamare la funzione Date quando impostate la data!).

Tutto il resto dovrebbe essere familiare dal nostro precedente schema.

### Modello Genre - sfida

Aprite il vostro file **./models/genre.js** e create uno schema per memorizzare i generi (categoria del libro, ad esempio se è fiction o non-fiction, romance o storia militare, ecc.).

La definizione sarà molto simile agli altri modelli:

- Il modello dovrebbe avere uno `String` SchemaType chiamato `name` per descrivere il genere.
- Questo nome dovrebbe essere obbligatorio e avere tra 3 e 100 caratteri.
- Dichiarate una [proprietà virtuale](#proprietà_virtuali) per l'URL del genere, chiamata `url`.
- Esportate il modello.

## Test — creare alcuni elementi

Questo è tutto. Ora abbiamo impostato tutti i modelli per il sito!

Per testare i modelli (e creare alcuni libri di esempio e altri elementi che possiamo utilizzare nei nostri prossimi articoli) eseguiremo ora uno script _indipendente_ per creare elementi di ciascun tipo:

1. Scaricate (o altrimenti create) il file [populatedb.js](https://raw.githubusercontent.com/mdn/express-locallibrary-tutorial/main/populatedb.js) all'interno della vostra cartella _express-locallibrary-tutorial_ (allo stesso livello di `package.json`).

   > [!NOTE]
   > Il codice in `populatedb.js` potrebbe essere utile per imparare JavaScript, ma non è necessario comprenderlo per questo tutorial.

2. Eseguite lo script utilizzando node nel vostro prompt dei comandi, passando l'URL del vostro database _MongoDB_ (lo stesso che avete sostituito al segnaposto _insert_your_database_url_here_ all'interno di `app.js` in precedenza):

   ```bash
   node populatedb <your MongoDB url>
   ```

   > [!NOTE]
   > Su Windows dovete incapsulare l'URL del database all'interno di doppi apici (").
   > Su altri sistemi operativi potrebbe essere necessario un singolo (') apice.

3. Lo script dovrebbe eseguirsi fino al completamento, visualizzando gli elementi creati nel terminale.

> [!NOTE]
> Andate al vostro database su MongoDB Atlas (nella scheda _Collections_).
> Dovreste ora essere in grado di approfondire le singole collezioni di Books, Authors, Genres e BookInstances, e controllare i singoli documenti.

## Riepilogo

In questo articolo, abbiamo appreso qualcosa sui database e sugli ORM su Node/Express, e molto su come definire schemi e modelli Mongoose. Abbiamo utilizzato queste informazioni per progettare e implementare i modelli di `Book`, `BookInstance`, `Author` e `Genre` per il sito web _LocalLibrary_.

Infine, abbiamo testato i nostri modelli creando un certo numero di istanze (utilizzando uno script autonomo). Nel prossimo articolo vedremo come creare alcune pagine per visualizzare questi oggetti.

## Vedi anche

- [Integrazione del database](https://expressjs.com/en/guide/database-integration.html) (documentazione Express)
- [Sito web di Mongoose](https://mongoosejs.com/) (documentazione Mongoose)
- [Guida di Mongoose](https://mongoosejs.com/docs/guide.html) (documentazione Mongoose)
- [Validazione](https://mongoosejs.com/docs/validation.html) (documentazione Mongoose)
- [Tipi di Schema](https://mongoosejs.com/docs/schematypes.html) (documentazione Mongoose)
- [Modelli](https://mongoosejs.com/docs/models.html) (documentazione Mongoose)
- [Query](https://mongoosejs.com/docs/queries.html) (documentazione Mongoose)
- [Popolazione](https://mongoosejs.com/docs/populate.html) (documentazione Mongoose)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
