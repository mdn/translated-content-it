---
title: "Tutorial di Express - Parte 3: utilizzo di un database (con Mongoose)"
short-title: "3: Utilizzo di database con Mongoose"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose
l10n:
  sourceCommit: 3d04bc6079a6b9d051c72c465a6e0421f20f603c
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo articolo introduce brevemente i database e come utilizzarli con le app Node/Express. Mostra poi come utilizzare [Mongoose](https://mongoosejs.com/) per fornire l'accesso al database per il sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Spiega come dichiarare schemi e modelli di oggetti, i principali tipi di campo e la validazione di base. Mostra inoltre brevemente alcuni dei principali modi per accedere ai dati dei modelli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website">Tutorial di Express - Parte 2: creazione dello scheletro di un sito web</a>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Essere in grado di progettare e creare i propri modelli utilizzando Mongoose.</td>
    </tr>
  </tbody>
</table>

## Panoramica

Il personale della biblioteca utilizzerà il sito web Local Library per memorizzare informazioni su libri e mutuatari, mentre i membri della biblioteca lo utilizzeranno per sfogliare e cercare libri, verificare se sono disponibili copie e quindi prenotarle o prenderle in prestito. Per memorizzare e recuperare le informazioni in modo efficiente, le conserveremo in un _database_.

Le app Express possono utilizzare molti database diversi ed esistono vari approcci per eseguire operazioni di **C**reazione, **L**ettura, **A**ggiornamento ed **E**liminazione (CRUD). Questo tutorial offre una breve panoramica di alcune delle opzioni disponibili e poi illustra in dettaglio i meccanismi specifici selezionati.

### Quali database si possono utilizzare?

Le app _Express_ possono usare qualsiasi database supportato da _Node_ (_Express_ stesso non definisce comportamenti o requisiti aggiuntivi specifici per la gestione dei database). Sono disponibili [molte opzioni popolari](https://expressjs.com/en/guide/database-integration/), tra cui PostgreSQL, MySQL, Redis, SQLite e MongoDB.

Nella scelta di un database, occorre considerare aspetti quali tempo necessario per essere produttivi/curva di apprendimento, prestazioni, facilità di replica/backup, costo, supporto della comunità e così via. Sebbene non esista un singolo database "migliore", quasi tutte le soluzioni popolari dovrebbero essere più che adeguate per un sito di dimensioni piccole o medie come Local Library.

Per maggiori informazioni sulle opzioni, vedere [Integrazione del database](https://expressjs.com/en/guide/database-integration/) (documentazione di Express).

### Qual è il modo migliore per interagire con un database?

Esistono due approcci comuni per interagire con un database:

- Utilizzare il linguaggio di query nativo del database, come SQL.
- Utilizzare un Object Relational Mapper ("ORM") o un Object Document Mapper ("ODM"). Questi rappresentano i dati del sito web come oggetti JavaScript, che vengono poi mappati nel database sottostante. Alcuni ORM e ODM sono legati a un database specifico, mentre altri forniscono un backend indipendente dal database.

Le prestazioni migliori in assoluto si possono ottenere utilizzando SQL, oppure qualunque linguaggio di query sia supportato dal database. I mapper di oggetti sono spesso più lenti perché utilizzano codice di traduzione per mappare gli oggetti nel formato del database, che potrebbe non usare le query del database più efficienti; questo è particolarmente vero se il mapper supporta backend di database diversi e deve quindi adottare compromessi maggiori riguardo alle funzionalità del database supportate.

Il vantaggio dell'uso di un ORM/ODM è che i programmatori possono continuare a ragionare in termini di oggetti JavaScript anziché di semantica del database; ciò è particolarmente vero se è necessario lavorare con database diversi, nello stesso sito web o in siti differenti. Forniscono inoltre un luogo evidente in cui eseguire la validazione dei dati.

> [!NOTE]
> L'uso di ODM/ORM comporta spesso costi inferiori per lo sviluppo e la manutenzione. A meno che non si conosca molto bene il linguaggio di query nativo o che le prestazioni siano di primaria importanza, è fortemente consigliato considerare l'uso di un ODM.

### Quale ORM/ODM usare?

Sono disponibili molte soluzioni ODM/ORM nel gestore di pacchetti npm; consultare i tag [odm](https://www.npmjs.com/search?q=keywords:odm) e [orm](https://www.npmjs.com/search?q=keywords:orm) per un sottoinsieme.

Alcune soluzioni popolari al momento della stesura sono:

- [Mongoose](https://www.npmjs.com/package/mongoose): Mongoose è uno strumento di modellazione degli oggetti per [MongoDB](https://www.mongodb.com/), progettato per funzionare in un ambiente asincrono.
- [Waterline](https://www.npmjs.com/package/waterline): un ORM estratto dal framework web [Sails](https://sailsjs.com/) basato su Express. Fornisce un'API uniforme per accedere a numerosi database differenti, inclusi Redis, MySQL, LDAP, MongoDB e Postgres.
- [Bookshelf](https://www.npmjs.com/package/bookshelf): offre sia interfacce basate su promise sia interfacce tradizionali basate su callback, fornendo supporto per transazioni, caricamento eager/nested-eager delle relazioni, associazioni polimorfiche e supporto per relazioni uno-a-uno, uno-a-molti e molti-a-molti. Funziona con PostgreSQL, MySQL e SQLite3.
- [Objection](https://www.npmjs.com/package/objection): rende il più semplice possibile usare tutta la potenza di SQL e del motore di database sottostante; supporta SQLite3, Postgres e MySQL.
- [Sequelize](https://www.npmjs.com/package/sequelize): è un ORM basato su promise per Node.js e io.js. Supporta i dialetti PostgreSQL, MySQL, MariaDB, SQLite e MSSQL e offre un solido supporto per transazioni, relazioni, replica in lettura e altro.
- [Node ORM2](https://node-orm.readthedocs.io/en/latest/): è un Object Relationship Manager per Node.js. Supporta MySQL, SQLite e Postgres, aiutando a lavorare con il database attraverso un approccio orientato agli oggetti.
- [GraphQL](https://graphql.org/): principalmente un linguaggio di query per API RESTful, GraphQL è molto popolare e offre funzionalità per leggere dati dai database.

Come regola generale, nella scelta di una soluzione è opportuno considerare sia le funzionalità offerte sia l'"attività della comunità" (download, contributi, segnalazioni di bug, qualità della documentazione e così via). Al momento della stesura Mongoose è di gran lunga l'ODM più popolare ed è una scelta ragionevole se si usa MongoDB come database.

### Utilizzare Mongoose e MongoDB per LocalLibrary

Per l'esempio _Local Library_ e per il resto di questo argomento verrà utilizzato l'[ODM Mongoose](https://www.npmjs.com/package/mongoose) per accedere ai dati della biblioteca. Mongoose agisce come front-end per [MongoDB](https://www.mongodb.com/company/what-is-mongodb), un database [NoSQL](https://en.wikipedia.org/wiki/NoSQL) open source che utilizza un modello di dati orientato ai documenti. Una "collection" di "documents" in un database MongoDB [è analoga a](https://www.mongodb.com/docs/manual/core/databases-and-collections/) una "table" di "rows" in un database relazionale.

Questa combinazione di ODM e database è estremamente popolare nella comunità Node, in parte perché il sistema di memorizzazione e query dei documenti somiglia molto a JSON ed è quindi familiare agli sviluppatori JavaScript.

> [!NOTE]
> Non è necessario conoscere MongoDB per utilizzare Mongoose, anche se alcune parti della [documentazione di Mongoose](https://mongoosejs.com/docs/guide.html) sono più semplici da usare e comprendere se si conosce già MongoDB.

Il resto di questo tutorial mostra come definire e accedere agli schemi e ai modelli Mongoose per l'esempio del [sito web LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website).

## Progettare i modelli di LocalLibrary

Prima di iniziare a scrivere il codice dei modelli, vale la pena dedicare alcuni minuti a riflettere sui dati da memorizzare e sulle relazioni tra i vari oggetti.

È necessario memorizzare informazioni sui libri, come titolo, riassunto, autore, genere e ISBN, e potrebbe essere necessario avere più copie disponibili, con ID globalmente univoci, stati di disponibilità e così via. Potrebbe essere necessario memorizzare più informazioni sull'autore oltre al semplice nome, e potrebbero esistere più autori con nomi identici o simili. Si desidera poter ordinare le informazioni in base al titolo del libro, all'autore, al genere e alla categoria.

Nella progettazione dei modelli è sensato avere modelli separati per ogni "oggetto", ovvero un gruppo di informazioni correlate. In questo caso, candidati evidenti per tali modelli sono libri, istanze dei libri e autori.

Potrebbe inoltre essere utile usare modelli per rappresentare le opzioni delle liste di selezione, ad esempio un elenco a discesa di scelte, anziché codificare rigidamente le scelte nel sito web stesso. Questo è consigliato quando tutte le opzioni non sono note in anticipo o possono cambiare. Un buon esempio è un genere, come fantasy, fantascienza e così via.

Dopo aver deciso i modelli e i campi, occorre riflettere sulle relazioni tra essi.

Tenendo presente questo, il diagramma delle associazioni UML seguente mostra i modelli che verranno definiti in questo caso, rappresentati come riquadri. Come illustrato sopra, sono stati creati modelli per il libro, ovvero i dettagli generici del libro, l'istanza del libro, ovvero lo stato delle copie fisiche specifiche del libro disponibili nel sistema, e l'autore. Si è inoltre deciso di avere un modello per il genere, in modo che i valori possano essere creati dinamicamente. Si è deciso di non avere un modello per `BookInstance:status`: i valori accettabili saranno codificati rigidamente perché non è previsto che cambino. All'interno di ogni riquadro sono visibili il nome del modello, i nomi e i tipi dei campi, nonché i metodi e i rispettivi tipi restituiti.

Il diagramma mostra anche le relazioni tra i modelli, comprese le rispettive _molteplicità_. Le molteplicità sono i numeri nel diagramma che indicano il numero, massimo e minimo, di ciascun modello che può essere presente nella relazione. Ad esempio, la linea di collegamento tra i riquadri mostra che `Book` e `Genre` sono correlati. I numeri vicini al modello `Book` mostrano che un `Genre` può avere zero o più `Book`, tanti quanti se ne desiderano, mentre i numeri all'altra estremità della linea accanto a `Genre` mostrano che un libro può avere zero o più `Genre` associati.

> [!NOTE]
> Come illustrato nel [primer su Mongoose](#primer_su_mongoose) seguente, spesso è meglio che il campo che definisce la relazione tra documenti/modelli sia presente in un solo modello; è comunque possibile trovare la relazione inversa cercando l'`_id` associato nell'altro modello. Di seguito è stato scelto di definire la relazione tra `Book`/`Genre` e `Book`/`Author` nello schema Book, e la relazione tra `Book`/`BookInstance` nello schema `BookInstance`. Questa scelta è in parte arbitraria: il campo avrebbe potuto essere presente anche nell'altro schema.

![Modello della biblioteca Mongoose con cardinalità corretta](library_website_-_mongoose_express.png)

> [!NOTE]
> La sezione successiva fornisce un primer di base che spiega come vengono definiti e usati i modelli. Durante la lettura, considerare come verrà costruito ciascuno dei modelli nel diagramma precedente.

### Le API del database sono asincrone

I metodi del database per creare, trovare, aggiornare o eliminare record sono asincroni.
Ciò significa che i metodi restituiscono immediatamente e il codice che gestisce il successo o l'errore del metodo viene eseguito in un momento successivo, quando l'operazione è completata.
Mentre il server attende il completamento dell'operazione del database, può essere eseguito altro codice, quindi il server può rimanere reattivo ad altre richieste.

JavaScript dispone di vari meccanismi per supportare il comportamento asincrono.
Storicamente JavaScript si è affidato in larga misura al passaggio di [funzioni callback](/it/docs/Learn_web_development/Extensions/Async_JS/Introducing) ai metodi asincroni per gestire i casi di successo ed errore.
Nel JavaScript moderno, le callback sono state in gran parte sostituite dalle [Promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise).
Le promise sono oggetti restituiti immediatamente da un metodo asincrono che ne rappresentano lo stato futuro.
Quando l'operazione termina, l'oggetto promise viene "risolto" e restituisce un oggetto che rappresenta il risultato dell'operazione o un errore.

Esistono due modi principali per utilizzare le promise al fine di eseguire codice quando una promise viene risolta ed è fortemente consigliato leggere [Come utilizzare le promise](/it/docs/Learn_web_development/Extensions/Async_JS/Promises) per una panoramica generale di entrambi gli approcci.
In questo tutorial verrà utilizzato principalmente [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) per attendere il completamento di una promise all'interno di una [`async function`](/it/docs/Web/JavaScript/Reference/Statements/async_function), perché questo produce codice asincrono più leggibile e comprensibile.

Questo approccio funziona usando la parola chiave `async function` per contrassegnare una funzione come asincrona, quindi applicando `await` all'interno della funzione a ogni metodo che restituisce una promise.
Quando la funzione asincrona viene eseguita, la sua operazione viene sospesa al primo metodo `await` finché la promise non viene risolta.
Dal punto di vista del codice circostante, la funzione asincrona restituisce quindi il controllo e il codice successivo può essere eseguito.
In seguito, quando la promise viene risolta, il metodo `await` all'interno della funzione asincrona restituisce il risultato oppure viene generato un errore se la promise è stata rifiutata.
Il codice nella funzione asincrona viene quindi eseguito fino a incontrare un altro `await`, momento in cui verrà nuovamente sospeso, oppure fino all'esecuzione di tutto il codice della funzione.

È possibile vedere il funzionamento nell'esempio seguente.
`myFunction()` è una funzione asincrona che contiene un blocco [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch) attorno alle espressioni `await`.
Quando viene eseguita `myFunction()`, l'esecuzione del codice viene sospesa in corrispondenza di `methodThatReturnsPromise()` finché la promise non viene risolta; il codice prosegue quindi fino a `functionThatReturnsPromise()` e attende nuovamente.
Il codice nel blocco `catch` viene eseguito se viene generato un errore nella funzione asincrona, e ciò avviene se la promise restituita da uno dei due metodi viene rifiutata.

```js
async function myFunction() {
  try {
    // …
    await someObject.methodThatReturnsPromise();
    // …
    await functionThatReturnsPromise();
    // …
  } catch (e) {
    // error handling code
  }
}

myFunction();
```

Una funzione asincrona restituisce una promise che viene rifiutata se l'errore non viene intercettato all'interno della funzione.
Per intercettare tale errore nel codice chiamante, utilizzare il metodo [`catch()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) della promise restituita oppure applicare `await` alla chiamata della funzione all'interno di un blocco `try...catch`.
Chiamare la funzione all'interno di un blocco `try...catch` senza `await` non intercetta il rifiuto, poiché è `await` a convertire la promise rifiutata restituita in un errore generato.

I metodi asincroni precedenti vengono eseguiti in sequenza.
Se i metodi non dipendono l'uno dall'altro, è possibile eseguirli in parallelo e completare l'intera operazione più rapidamente.
Questo si esegue usando il metodo [`Promise.all()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise/all), che accetta come input un iterabile di promise e restituisce una singola `Promise`.
La promise restituita viene soddisfatta quando tutte le promise di input vengono soddisfatte, con un array dei valori di soddisfacimento.
Viene rifiutata quando una qualsiasi delle promise di input viene rifiutata, con il motivo del primo rifiuto.

Il codice seguente mostra come funziona.
Per prima cosa, sono presenti due funzioni che restituiscono promise.
Si applica `await` a entrambe affinché siano completate usando la promise restituita da `Promise.all()`.
Dopo che entrambe sono completate, `await` restituisce il controllo e l'array dei risultati viene popolato; la funzione prosegue quindi fino al successivo `await` e attende che la promise restituita da `anotherFunctionThatReturnsPromise()` sia risolta.
Sarebbe necessario associare un gestore `catch()` alla promise restituita da `myFunction()` per intercettare eventuali errori.

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

Le promise con `await`/`async` consentono un controllo dell'esecuzione asincrona flessibile e comprensibile.

## Primer su Mongoose

Questa sezione fornisce una panoramica su come connettere Mongoose a un database MongoDB, definire uno schema e un modello, ed eseguire query di base.

> [!NOTE]
> Questo primer è fortemente ispirato alla [guida rapida di Mongoose](https://www.npmjs.com/package/mongoose) su _npm_ e alla [documentazione ufficiale](https://mongoosejs.com/docs/guide.html).

### Installare Mongoose e MongoDB

Mongoose viene installato nel progetto, in **package.json**, come qualsiasi altra dipendenza, utilizzando npm.
Per installarlo, utilizzare il comando seguente all'interno della cartella del progetto:

```bash
npm install mongoose
```

L'installazione di _Mongoose_ aggiunge tutte le sue dipendenze, incluso il driver del database MongoDB, ma non installa MongoDB stesso. Per installare un server MongoDB, è possibile [scaricare gli installer da qui](https://www.mongodb.com/try/download/community) per vari sistemi operativi e installarlo localmente. È inoltre possibile utilizzare istanze MongoDB basate sul cloud.

> [!NOTE]
> Per questo tutorial verrà utilizzato il livello gratuito del _database as a service_ basato sul cloud [MongoDB Atlas](https://www.mongodb.com/) per fornire il database. Questo è adatto allo sviluppo e ha senso per il tutorial perché rende l'"installazione" indipendente dal sistema operativo; il database-as-a-service è anche uno degli approcci utilizzabili per il database di produzione.

### Connessione a MongoDB

_Mongoose_ richiede una connessione a un database MongoDB.
È possibile usare `require()` e connettersi a un database ospitato localmente con `mongoose.connect()`, come mostrato di seguito; per il tutorial ci si connetterà invece a un database ospitato su Internet.

```js
// Import the mongoose module
const mongoose = require("mongoose");

// Define the database URL to connect to.
const mongoDB = "mongodb://127.0.0.1/my_database";

// Wait for database to connect, logging an error if there is a problem
main().catch((err) => console.log(err));
async function main() {
  await mongoose.connect(mongoDB);
}
```

> [!NOTE]
> Come illustrato nella sezione [Le API del database sono asincrone](#le_api_del_database_sono_asincrone), qui si applica `await` alla promise restituita dal metodo `connect()` all'interno di una funzione `async`.
> Si utilizza il gestore `catch()` della promise per gestire eventuali errori durante il tentativo di connessione, ma sarebbe stato possibile usare anche `await main()` all'interno di un blocco `try...catch` in un'altra funzione `async`.

È possibile ottenere l'oggetto `Connection` predefinito con `mongoose.connection`.
Se è necessario creare connessioni aggiuntive, è possibile usare `mongoose.createConnection()`.
Questo accetta la stessa forma di URI del database, con host, database, porta, opzioni e così via, di `connect()` e restituisce un oggetto `Connection`.
Si noti che `createConnection()` restituisce immediatamente; se è necessario attendere che la connessione venga stabilita, è possibile chiamarlo con `asPromise()` per restituire una promise: `mongoose.createConnection(mongoDB).asPromise()`.

### Definizione e creazione dei modelli

I modelli vengono _definiti_ usando l'interfaccia `Schema`. Lo schema consente di definire i campi memorizzati in ciascun documento insieme ai relativi requisiti di validazione e valori predefiniti. Inoltre, è possibile definire metodi helper statici e di istanza per semplificare il lavoro con i tipi di dati, nonché proprietà virtuali utilizzabili come qualsiasi altro campo ma che non sono effettivamente memorizzate nel database; l'argomento verrà approfondito poco più avanti.

Gli schemi vengono poi "compilati" in modelli usando il metodo `mongoose.model()`. Una volta ottenuto un modello, è possibile utilizzarlo per trovare, creare, aggiornare ed eliminare oggetti del tipo specificato.

> [!NOTE]
> Ogni modello viene mappato a una _collection_ di _documents_ nel database MongoDB. I documenti conterranno i campi/tipi di schema definiti nel modello `Schema`.

#### Definizione degli schemi

Il frammento di codice seguente mostra come definire un semplice schema. Innanzitutto si esegue `require()` su mongoose, quindi si utilizza il costruttore Schema per creare una nuova istanza di schema, definendo i vari campi all'interno del parametro oggetto del costruttore.

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

Nel caso precedente sono presenti solo due campi, una stringa e una data. Nelle sezioni successive verranno illustrati alcuni degli altri tipi di campo, la validazione e altri metodi.

#### Creazione di un modello

I modelli vengono creati dagli schemi usando il metodo `mongoose.model()`:

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

Il primo argomento è il nome singolare della collection che verrà creata per il modello; Mongoose creerà la collection del database per il modello _SomeModel_ precedente. Il secondo argomento è lo schema da utilizzare per creare il modello.

> [!NOTE]
> Dopo aver definito le classi di modello, è possibile utilizzarle per creare, aggiornare o eliminare record ed eseguire query per ottenere tutti i record o particolari sottoinsiemi di record. Questo verrà mostrato nella sezione [Utilizzo dei modelli](#utilizzo_dei_modelli) e durante la creazione delle viste.

#### Tipi di schema (campi)

Uno schema può avere un numero arbitrario di campi: ciascuno rappresenta un campo nei documenti memorizzati in _MongoDB_.
Di seguito è mostrato un esempio di schema che presenta molti dei tipi di campo comuni e il modo in cui vengono dichiarati.

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

La maggior parte degli [SchemaTypes](https://mongoosejs.com/docs/schematypes.html), ovvero i descrittori dopo "type:" o dopo i nomi dei campi, sono autoesplicativi. Le eccezioni sono:

- `ObjectId`: rappresenta istanze specifiche di un modello nel database. Ad esempio, un libro potrebbe utilizzarlo per rappresentare l'oggetto autore. Questo conterrà effettivamente l'ID univoco, `_id`, dell'oggetto specificato. È possibile utilizzare il metodo `populate()` per recuperare le informazioni associate quando necessario.
- [`Mixed`](https://mongoosejs.com/docs/schematypes.html#mixed): un tipo di schema arbitrario.
- `[]`: un array di elementi. È possibile eseguire operazioni sugli array JavaScript su questi modelli, come push, pop e unshift. Gli esempi precedenti mostrano un array di oggetti senza un tipo specificato e un array di oggetti `String`, ma è possibile avere un array di qualsiasi tipo di oggetto.

Il codice mostra inoltre entrambi i modi per dichiarare un campo:

- _Nome_ e _tipo_ del campo come coppia chiave-valore, come avviene per i campi `name`, `binary` e `living`.
- _Nome_ del campo seguito da un oggetto che definisce il `type` e qualsiasi altra _opzione_ del campo. Le opzioni includono elementi quali:
  - valori predefiniti;
  - validatori incorporati, come valori max/min, e funzioni di validazione personalizzate;
  - se il campo è obbligatorio;
  - se i campi `String` devono essere convertiti automaticamente in minuscolo, maiuscolo o sottoposti a trim, ad esempio `{ type: String, lowercase: true, trim: true }`.

Per maggiori informazioni sulle opzioni, vedere [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) (documentazione di Mongoose).

#### Validazione

Mongoose fornisce validatori incorporati e personalizzati, oltre a validatori sincroni e asincroni. Consente di specificare sia l'intervallo accettabile di valori sia il messaggio di errore in caso di fallimento della validazione.

I validatori incorporati includono:

- Tutti gli [SchemaTypes](https://mongoosejs.com/docs/schematypes.html) dispongono del validatore incorporato [required](https://mongoosejs.com/docs/api.html#schematype_SchemaType-required). Viene usato per specificare se il campo deve essere fornito per poter salvare un documento.
- I [numeri](https://mongoosejs.com/docs/api/schemanumber.html) hanno i validatori [min](<https://mongoosejs.com/docs/api/schemanumber.html#SchemaNumber.prototype.min()>) e [max](<https://mongoosejs.com/docs/api/schemanumber.html#SchemaNumber.prototype.max()>).
- Le [stringhe](https://mongoosejs.com/docs/api/schemastring.html) hanno:
  - [enum](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.enum()>): specifica l'insieme di valori consentiti per il campo.
  - [match](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.match()>): specifica un'espressione regolare a cui la stringa deve corrispondere.
  - [maxLength](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.maxlength()>) e [minLength](<https://mongoosejs.com/docs/api/schemastring.html#SchemaString.prototype.minlength()>) per la stringa.

L'esempio seguente, leggermente modificato dalla documentazione di Mongoose, mostra come specificare alcuni tipi di validatore e messaggi di errore:

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

Per informazioni complete sulla validazione dei campi, vedere [Validation](https://mongoosejs.com/docs/validation.html) (documentazione di Mongoose).

#### Proprietà virtuali

Le proprietà virtuali sono proprietà di documenti che è possibile ottenere e impostare, ma che non vengono persistite in MongoDB. I getter sono utili per formattare o combinare campi, mentre i setter sono utili per scomporre un singolo valore in più valori da memorizzare. L'esempio nella documentazione costruisce e scompone una proprietà virtuale del nome completo a partire da un campo nome e un campo cognome, operazione più semplice e pulita rispetto alla costruzione di un nome completo ogni volta che viene utilizzato in un template.

> [!NOTE]
> Nella biblioteca verrà utilizzata una proprietà virtuale per definire un URL univoco per ogni record del modello, usando un percorso e il valore `_id` del record.

Per maggiori informazioni, vedere [Virtuals](https://mongoosejs.com/docs/guide.html#virtuals) (documentazione di Mongoose).

#### Metodi e helper per le query

Uno schema può anche avere [metodi di istanza](https://mongoosejs.com/docs/guide.html#methods), [metodi statici](https://mongoosejs.com/docs/guide.html#statics) e [helper per le query](https://mongoosejs.com/docs/guide.html#query-helpers). I metodi di istanza e statici sono simili, ma con l'ovvia differenza che un metodo di istanza è associato a un record particolare e ha accesso all'oggetto corrente. Gli helper per le query consentono di estendere l'[API del query builder concatenabile](https://mongoosejs.com/docs/queries.html) di mongoose, ad esempio consentendo di aggiungere una query "byName" oltre ai metodi `find()`, `findOne()` e `findById()`.

### Utilizzo dei modelli

Dopo aver creato uno schema, è possibile utilizzarlo per creare modelli. Il modello rappresenta una collection di documenti nel database su cui è possibile effettuare ricerche, mentre le istanze del modello rappresentano documenti individuali che è possibile salvare e recuperare.

Di seguito viene fornita una breve panoramica. Per maggiori informazioni, vedere [Models](https://mongoosejs.com/docs/models.html) (documentazione di Mongoose).

> [!NOTE]
> La creazione, l'aggiornamento, l'eliminazione e l'interrogazione dei record sono operazioni asincrone che restituiscono una [promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise).
> Gli esempi seguenti mostrano solo l'uso dei metodi rilevanti e di `await`, ovvero il codice essenziale per usare i metodi.
> La funzione `async function` e il blocco `try...catch` circostanti per intercettare gli errori sono omessi per maggiore chiarezza.
> Per maggiori informazioni sull'uso di `await/async`, vedere [Le API del database sono asincrone](#le_api_del_database_sono_asincrone).

#### Creazione e modifica di documenti

Per creare un record, è possibile definire un'istanza del modello e poi chiamare [`save()`](https://mongoosejs.com/docs/api/model.html#Model.prototype.save) su di essa.
Gli esempi seguenti presuppongono che `SomeModel` sia un modello, con un singolo campo `name`, creato dallo schema.

```js
// Create an instance of model SomeModel
const awesome_instance = new SomeModel({ name: "awesome" });

// Save the new model instance asynchronously
await awesome_instance.save();
```

È inoltre possibile utilizzare [`create()`](https://mongoosejs.com/docs/api/model.html#Model.create) per definire l'istanza del modello contemporaneamente al suo salvataggio.
Di seguito ne viene creata una sola, ma è possibile creare più istanze passando un array di oggetti.

```js
await SomeModel.create({ name: "also_awesome" });
```

Ogni modello dispone di una connessione associata, che sarà la connessione predefinita quando si utilizza `mongoose.model()`. È possibile creare una nuova connessione e chiamare `.model()` su di essa per creare i documenti in un database diverso.

È possibile accedere ai campi di questo nuovo record usando la sintassi con punto e modificare i valori. È necessario chiamare `save()` o `update()` per memorizzare nuovamente nel database i valori modificati.

```js
// Access model field values using dot notation
console.log(awesome_instance.name); // should log 'also_awesome'

// Change record by modifying the fields, then calling save().
awesome_instance.name = "New cool name";
await awesome_instance.save();
```

#### Ricerca di record

È possibile cercare record usando metodi di query e specificando le condizioni della query come documento JSON. Il frammento di codice seguente mostra come trovare tutti gli atleti in un database che giocano a tennis, restituendo solo i campi _name_ e _age_ dell'atleta. Qui viene specificato un solo campo corrispondente, sport, ma è possibile aggiungere altri criteri, specificare criteri con espressioni regolari o rimuovere del tutto le condizioni per restituire tutti gli atleti.

```js
const Athlete = mongoose.model("Athlete", yourSchema);

// find all athletes who play tennis, returning the 'name' and 'age' fields
const tennisPlayers = await Athlete.find(
  { sport: "Tennis" },
  "name age",
).exec();
```

> [!NOTE]
> È importante ricordare che non trovare risultati **non è un errore** per una ricerca, ma potrebbe rappresentare un caso di fallimento nel contesto dell'applicazione.
> Se l'applicazione prevede che una ricerca trovi un valore, è possibile verificare il numero di voci restituite nel risultato.

Le API di query, come [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>), restituiscono una variabile di tipo [Query](https://mongoosejs.com/docs/api/query.html).
È possibile usare un oggetto query per costruire una query in parti prima di eseguirla con il metodo [`exec()`](https://mongoosejs.com/docs/api/query.html#Query.prototype.exec).
`exec()` esegue la query e restituisce una promise a cui è possibile applicare `await` per ottenere il risultato.

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

In precedenza sono state definite le condizioni della query nel metodo [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>). È possibile farlo anche usando una funzione [`where()`](<https://mongoosejs.com/docs/api/model.html#Model.where()>), concatenando tutte le parti della query con l'operatore punto (`.`) anziché aggiungerle separatamente.
Il frammento di codice seguente è uguale alla query precedente, con una condizione aggiuntiva per l'età.

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

Il metodo [`find()`](<https://mongoosejs.com/docs/api/model.html#Model.find()>) ottiene tutti i record corrispondenti, ma spesso si desidera ottenere solo una corrispondenza. I seguenti metodi eseguono query per un singolo record:

- [`findById()`](<https://mongoosejs.com/docs/api/model.html#Model.findById()>): trova il documento con l'`id` specificato; ogni documento ha un `id` univoco.
- [`findOne()`](<https://mongoosejs.com/docs/api/model.html#Model.findOne()>): trova un singolo documento che corrisponde ai criteri specificati.
- [`findByIdAndDelete()`](<https://mongoosejs.com/docs/api/model.html#Model.findByIdAndDelete()>), [`findByIdAndUpdate()`](<https://mongoosejs.com/docs/api/model.html#Model.findByIdAndUpdate()>), [`findOneAndRemove()`](<https://mongoosejs.com/docs/api/model.html#Model.findOneAndRemove()>), [`findOneAndUpdate()`](<https://mongoosejs.com/docs/api/model.html#Model.findOneAndUpdate()>): trovano un singolo documento tramite `id` o criteri e lo aggiornano oppure lo rimuovono. Sono funzioni di utilità pratiche per aggiornare e rimuovere record.

> [!NOTE]
> Esiste anche un metodo [`countDocuments()`](<https://mongoosejs.com/docs/api/model.html#Model.countDocuments()>) utilizzabile per ottenere il numero di elementi che corrispondono alle condizioni. È utile se si desidera eseguire un conteggio senza recuperare effettivamente i record.

Con le query è possibile fare molto altro. Per maggiori informazioni, vedere [Queries](https://mongoosejs.com/docs/queries.html) (documentazione di Mongoose).

#### Lavorare con documenti correlati — popolamento

È possibile creare riferimenti da un'istanza di documento/modello a un'altra usando il campo schema `ObjectId`, oppure da un documento a molti usando un array di `ObjectId`. Il campo memorizza l'ID del modello correlato. Se è necessario il contenuto effettivo del documento associato, è possibile utilizzare il metodo [`populate()`](https://mongoosejs.com/docs/populate.html) in una query per sostituire l'ID con i dati effettivi.

Ad esempio, lo schema seguente definisce autori e racconti.
Ogni autore può avere più racconti, rappresentati come un array di `ObjectId`.
Ogni racconto può avere un singolo autore.
La proprietà `ref` comunica allo schema quale modello può essere assegnato a questo campo.

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

È possibile salvare i riferimenti al documento correlato assegnando il valore `_id`.
Di seguito viene creato un autore, poi un racconto, e l'ID dell'autore viene assegnato al campo autore del racconto.

```js
const bob = new Author({ name: "Bob Smith" });

await bob.save();

// Bob now exists, so let's create a story
const story = new Story({
  title: "Bob goes sledding",
  author: bob._id, // assign the _id from our author Bob. This ID is created by default!
});

await story.save();
```

> [!NOTE]
> Un grande vantaggio di questo stile di programmazione è che non è necessario complicare il percorso principale del codice con controlli degli errori.
> Se una qualsiasi delle operazioni `save()` fallisce, la promise viene rifiutata e viene generato un errore.
> Il codice di gestione degli errori se ne occupa separatamente, di solito in un blocco `catch()`, quindi l'intento del codice è molto chiaro.

Il documento del racconto ora ha un autore a cui fa riferimento tramite l'ID del documento autore. Per ottenere le informazioni dell'autore nei risultati del racconto, viene utilizzato [`populate()`](https://mongoosejs.com/docs/api/model.html#Model.populate), come mostrato di seguito.

```js
Story.findOne({ title: "Bob goes sledding" })
  .populate("author") // Replace the author id with actual author information in results
  .exec();
```

> [!NOTE]
> I lettori più attenti avranno notato che è stato aggiunto un autore al racconto, ma non è stato fatto nulla per aggiungere il racconto all'array `stories` dell'autore. Come si possono quindi ottenere tutti i racconti di un particolare autore? Un modo sarebbe aggiungere il racconto all'array stories, ma ciò comporterebbe due luoghi in cui mantenere le informazioni che collegano autori e racconti.
>
> Un approccio migliore consiste nell'ottenere l'`_id` dell'_autore_ e poi usare `find()` per cercarlo nel campo autore di tutti i racconti.
>
> ```js
> Story.find({ author: bob._id }).exec();
> ```

Questo è quasi tutto ciò che serve sapere per lavorare con elementi correlati _in questo tutorial_. Per informazioni più dettagliate, vedere [Population](https://mongoosejs.com/docs/populate.html) (documentazione di Mongoose).

### Uno schema/modello per file

Sebbene sia possibile creare schemi e modelli usando qualunque struttura di file, è fortemente consigliato definire ogni schema del modello nel proprio modulo, ovvero file, e poi esportare il metodo per creare il modello.
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

È quindi possibile usare `require` e utilizzare immediatamente il modello in altri file. Di seguito viene mostrato come usarlo per ottenere tutte le istanze del modello.

```js
// Create a SomeModel model just by requiring the module
const SomeModel = require("../models/some-model");

// Use the SomeModel object (model) to find all SomeModel records
const modelInstances = await SomeModel.find().exec();
```

## Configurare il database MongoDB

Ora che sono state illustrate alcune funzionalità di Mongoose e il modo in cui progettare i modelli, è il momento di iniziare a lavorare sul sito web _LocalLibrary_. La prima cosa da fare è configurare un database MongoDB utilizzabile per memorizzare i dati della biblioteca.

Per questo tutorial verrà utilizzato il database sandbox ospitato nel cloud [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database). Questo livello di database non è considerato adatto ai siti web in produzione perché non dispone di ridondanza, ma è ottimo per sviluppo e prototipazione. Viene utilizzato qui perché è gratuito e facile da configurare, e perché MongoDB Atlas è un fornitore popolare di _database as a service_ che potrebbe ragionevolmente essere scelto per il database di produzione. Altre scelte popolari al momento della stesura includono [ScaleGrid](https://scalegrid.io/) e [Rackspace](https://www.rackspace.com/data/rackspace-dbaas).

> [!NOTE]
> Se si preferisce, è possibile configurare un database MongoDB localmente scaricando e installando i [binari appropriati per il sistema](https://www.mongodb.com/try/download/community-edition/releases). Il resto delle istruzioni in questo articolo sarebbe simile, a eccezione dell'URL del database da specificare durante la connessione.
> Nel tutorial [Tutorial di Express - Parte 7: distribuzione in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment), sia l'applicazione sia il database vengono ospitati su [Railway](https://railway.com/), ma sarebbe stato ugualmente possibile utilizzare un database su [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database).

Per prima cosa sarà necessario [creare un account](https://www.mongodb.com/cloud/atlas/register) con MongoDB Atlas. È gratuito e richiede solo l'inserimento di dati di contatto di base e l'accettazione dei termini di servizio.

Dopo aver effettuato l'accesso, verrà visualizzata la schermata [home](https://cloud.mongodb.com/v2):

1. Fare clic sul pulsante **+ Create** nella sezione _Overview_.

   ![Creare un database su MongoDB Atlas.](mongodb_atlas_-_createdatabase.jpg)

2. Verrà aperta la schermata _Deploy your cluster_.
   Fare clic sul modello dell'opzione **M0 FREE**.

   ![Scegliere un'opzione di distribuzione quando si usa MongoDB Atlas.](mongodb_atlas_-_deploy.jpg)

3. Scorrere la pagina per visualizzare le diverse opzioni disponibili.
   ![Scegliere un provider cloud quando si usa MongoDB Atlas.](mongodb_atlas_-_createsharedcluster.jpg)
   - È possibile cambiare il nome del Cluster in _Cluster Name_.
     In questo tutorial verrà mantenuto `Cluster0`.
   - Deselezionare la casella di controllo _Preload sample dataset_, perché i dati di esempio verranno importati in seguito.
   - Selezionare qualsiasi provider e regione dalle sezioni _Provider_ e _Region_. Regioni diverse offrono provider diversi.
   - I tag sono facoltativi. Non verranno utilizzati qui.
   - Fare clic sul pulsante **Create deployment**. La creazione del cluster richiederà alcuni minuti.

4. Verrà aperta la sezione _Security Quickstart_.
   ![Configurare le regole di accesso nella schermata Security Quickstart di MongoDB Atlas.](mongodb_atlas_-_securityquickstart.jpg)
   - Inserire un nome utente e una password che l'applicazione utilizzerà per accedere al database. Nell'esempio precedente è stato creato un nuovo accesso denominato "cooluser".
     Ricordare di copiare e memorizzare le credenziali in modo sicuro, poiché saranno necessarie in seguito.
     Fare clic sul pulsante **Create User**.

     > [!NOTE]
     > Evitare di usare caratteri speciali nella password dell'utente MongoDB, poiché mongoose potrebbe non analizzare correttamente la stringa di connessione.

   - Selezionare **Add by current IP address** per consentire l'accesso dal computer corrente.
   - Inserire `0.0.0.0/0` nel campo IP Address, quindi fare clic sul pulsante **Add Entry**.
     Questo comunica a MongoDB che si desidera consentire l'accesso da qualsiasi posizione.

     > [!NOTE]
     > È una buona pratica limitare gli indirizzi IP che possono connettersi al database e ad altre risorse. Qui viene consentita una connessione da qualunque posizione perché non si sa da dove arriverà la richiesta dopo la distribuzione.

   - Fare clic sul pulsante **Finish and Close**.

5. Verrà aperta la schermata seguente. Fare clic sul pulsante **Go to Overview**.
   ![Andare a Databases dopo aver configurato le regole di accesso in MongoDB Atlas](mongodb_atlas_-_accessrules.jpg)

6. Verrà visualizzata nuovamente la schermata _Overview_. Fare clic sulla sezione _Database_ nel menu _Deployment_ a sinistra. Fare clic sul pulsante **Browse Collections**.
   ![Configurare una collection su MongoDB Atlas.](mongodb_atlas_-_createcollection.jpg)

7. Verrà aperta la sezione _Collections_. Fare clic sul pulsante **Add My Own Data**.
   ![Creare un database su MongoDB Atlas.](mongodb_atlas_-_adddata.jpg)

8. Verrà aperta la schermata _Create Database_.

   ![Dettagli durante la creazione di un database in MongoDB Atlas.](mongodb_atlas_-_databasedetails.jpg)
   - Inserire `local_library` come nome del nuovo database.
   - Inserire `Collection0` come nome della collection.
   - Fare clic sul pulsante **Create** per creare il database.

9. Verrà visualizzata nuovamente la schermata _Collections_ con il database creato.
   ![Conferma della creazione del database in MongoDB Atlas.](mongodb_atlas_-_databasecreated.jpg)
   - Fare clic sulla scheda _Overview_ per tornare alla panoramica del cluster.

10. Dalla schermata _Overview_ di Cluster0, fare clic sul pulsante **Connect**.

    ![Configurare la connessione dopo aver creato un cluster in MongoDB Atlas.](mongodb_atlas_-_connectbutton.jpg)

11. Verrà aperta la schermata _Connect to Cluster0_.

    ![Scegliere la connessione Short SRV durante la configurazione di una connessione in MongoDB Atlas.](mongodb_atlas_-_connectforshortsrv.jpg)
    - Selezionare l'utente del database.
    - Selezionare la categoria _Drivers_, quindi il _Driver_ **Node.js** e la _Version_ come mostrato.
    - **NON** installare il driver come suggerito.
    - Fare clic sull'icona **Copy** per copiare la stringa di connessione.
    - Incollarla nell'editor di testo locale.
    - Sostituire il segnaposto `<password>` nella stringa di connessione con la password dell'utente.
    - Inserire il nome del database "local_library" nel percorso prima delle opzioni (`...mongodb.net/local_library?retryWrites...`).
    - Salvare il file contenente questa stringa in un luogo sicuro.

A questo punto il database è stato creato ed è disponibile un URL, con nome utente e password, utilizzabile per accedervi.
Avrà un aspetto simile a: `mongodb+srv://your_user_name:your_password@cluster0.cojoign.mongodb.net/local_library?retryWrites=true&w=majority&appName=Cluster0`

## Installare Mongoose

Aprire un prompt dei comandi e passare alla directory in cui è stato creato lo [scheletro del sito web Local Library](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website).
Inserire il comando seguente per installare Mongoose, e le relative dipendenze, e aggiungerlo al file **package.json**, a meno che non sia già stato fatto durante la lettura del [primer su Mongoose](#installare_mongoose_e_mongodb).

```bash
npm install mongoose
```

## Connettersi a MongoDB

Aprire **bin/www**, dalla radice del progetto, e copiare il testo seguente sotto il punto in cui è impostata la porta, dopo la riga `app.set("port", port);`.
Sostituire la stringa dell'URL del database (`insert_your_database_url_here`) con l'URL della posizione che rappresenta il database, ovvero usando le informazioni di _MongoDB Atlas_.

```js
// Set up mongoose connection
const mongoose = require("mongoose");

const mongoDB = "insert_your_database_url_here";

connectMongoose()
  .then(startServer)
  .catch((err) => {
    console.error("Failed to connect to MongoDB:", err);
    process.exit(1);
  });

async function connectMongoose() {
  await mongoose.connect(mongoDB);

  // Add connection error handlers
  mongoose.connection.on("error", (err) => {
    console.error("MongoDB connection error:", err);
  });

  mongoose.connection.on("disconnected", () => {
    console.warn("MongoDB disconnected");
  });
}
```

Come illustrato nel [primer su Mongoose](#connessione_a_mongodb), questo codice crea la connessione predefinita al database e segnala eventuali errori alla console.
Chiama inoltre una funzione `startServer()` una volta che la connessione ha esito positivo, che verrà creata successivamente.

Il file **bin/www** generato crea il server HTTP e inizia immediatamente l'ascolto, indipendentemente dal fatto che la connessione al database riesca.
Individuare questo codice, più avanti nello stesso file:

```js
/**
 * Create HTTP server.
 */

var server = http.createServer(app);

/**
 * Listen on provided port, on all network interfaces.
 */

server.listen(port);
server.on("error", onError);
server.on("listening", onListening);
```

Sostituirlo con il seguente, che sposta la creazione e l'ascolto del server in una funzione `startServer()`, affinché venga eseguita solo dopo la risoluzione di `connectMongoose()`:

```js
/**
 * Create HTTP server and listen on provided port, on all network
 * interfaces, once the MongoDB connection is established.
 */

var server;

function startServer() {
  server = http.createServer(app);

  server.listen(port);
  server.on("error", onError);
  server.on("listening", onListening);
}
```

> [!NOTE]
> Il codice di connessione al database avrebbe potuto essere inserito nel codice **app.js**.
> Collocarlo nel punto di ingresso dell'applicazione separa l'applicazione e il database, rendendo più semplice usare un database differente per eseguire il codice di test.

Si noti che non è consigliato codificare rigidamente le credenziali del database nel codice sorgente, come mostrato sopra.
Questo viene fatto qui perché mostra il codice di connessione principale e perché durante lo sviluppo non vi è un rischio significativo che la divulgazione di questi dettagli esponga o danneggi informazioni sensibili.
Verrà mostrato come fare in modo più sicuro durante la [distribuzione in produzione](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment#database_configuration).

## Definizione dello schema LocalLibrary

Verrà definito un modulo separato per ogni modello, come [illustrato sopra](#one_schemamodel_per_file).
Iniziare creando una cartella per i modelli nella radice del progetto, **/models**, quindi creare file separati per ciascuno dei modelli:

```plain
/express-locallibrary-tutorial  # the project root
  /models
    author.js
    book.js
    bookinstance.js
    genre.js
```

### Modello Author

Copiare il codice dello schema `Author` mostrato di seguito e incollarlo nel file **./models/author.js**.
Lo schema definisce un autore con SchemaTypes `String` per nome e cognome, obbligatori e con un massimo di 100 caratteri, e campi `Date` per le date di nascita e morte.

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

È stata inoltre dichiarata una [proprietà virtuale](#proprietà_virtuali) per AuthorSchema denominata "url", che restituisce l'URL assoluto necessario per ottenere una particolare istanza del modello. La proprietà verrà utilizzata nei template ogni volta che sarà necessario ottenere un collegamento a un particolare autore.

> [!NOTE]
> Dichiarare gli URL come proprietà virtuale nello schema è una buona idea perché l'URL di un elemento deve essere modificato in un solo punto.
> A questo punto, un collegamento che utilizza questo URL non funzionerebbe perché non esiste ancora codice di gestione delle route per singole istanze del modello.
> Questo verrà configurato in un articolo successivo.

Alla fine del modulo viene esportato il modello.

### Modello Book

Copiare il codice dello schema `Book` mostrato di seguito e incollarlo nel file **./models/book.js**.
La maggior parte di questo è simile al modello autore: è stato dichiarato uno schema con vari campi stringa e una proprietà virtuale per ottenere l'URL di record specifici dei libri, ed è stato esportato il modello.

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

La differenza principale è che sono stati creati due riferimenti ad altri modelli:

- author è un riferimento a un singolo oggetto del modello `Author` ed è obbligatorio.
- genre è un riferimento a un array di oggetti del modello `Genre`. Questo oggetto non è ancora stato dichiarato.

### Modello BookInstance

Infine, copiare il codice dello schema `BookInstance` mostrato di seguito e incollarlo nel file **./models/bookinstance.js**.
`BookInstance` rappresenta una copia specifica di un libro che qualcuno potrebbe prendere in prestito e include informazioni sulla disponibilità della copia, sulla data prevista per la restituzione e sui dettagli dell'"imprint", ovvero edizione o versione.

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

Qui vengono mostrate nuove opzioni di campo:

- `enum`: consente di impostare i valori consentiti di una stringa. In questo caso viene utilizzato per specificare lo stato di disponibilità dei libri. L'uso di un enum consente di evitare errori di ortografia e valori arbitrari per lo stato.
- `default`: viene usato per impostare lo stato predefinito delle istanze di libri appena create su "Maintenance" e la data `due_back` predefinita su `now`. Si noti come sia possibile chiamare la funzione Date quando si imposta la data.

Tutto il resto dovrebbe essere già noto dagli schemi precedenti.

### Modello Genre - sfida

Aprire il file **./models/genre.js** e creare uno schema per memorizzare i generi, ovvero la categoria del libro, ad esempio se è narrativa o saggistica, romantico o di storia militare e così via.

La definizione sarà molto simile agli altri modelli:

- Il modello deve avere uno SchemaType `String` chiamato `name` per descrivere il genere.
- Questo nome deve essere obbligatorio e avere da 3 a 100 caratteri.
- Dichiarare una [proprietà virtuale](#proprietà_virtuali) per l'URL del genere, denominata `url`.
- Esportare il modello.

## Test: creazione di alcuni elementi

Questo è tutto. Ora sono configurati tutti i modelli del sito.

Per testare i modelli, e per creare alcuni libri ed elementi di esempio utilizzabili nei prossimi articoli, verrà ora eseguito uno script _indipendente_ per creare elementi di ogni tipo:

1. Scaricare, o creare in altro modo, il file [populatedb.js](https://raw.githubusercontent.com/mdn/express-locallibrary-tutorial/main/populatedb.js) nella directory _express-locallibrary-tutorial_, allo stesso livello di `package.json`.

   > [!NOTE]
   > Il codice in `populatedb.js` può essere utile per apprendere JavaScript, ma comprenderlo non è necessario per questo tutorial.

2. Eseguire lo script utilizzando node nel prompt dei comandi, passando l'URL del database _MongoDB_, lo stesso con cui è stato sostituito il segnaposto `insert_your_database_url_here` in `app.js` in precedenza:

   ```bash
   node populatedb <your MongoDB url>
   ```

   > [!NOTE]
   > Su Windows è necessario racchiudere l'URL del database tra virgolette doppie (`"`).
   > Su altri sistemi operativi potrebbero essere necessarie virgolette singole (`'`).

3. Lo script dovrebbe essere eseguito fino al completamento, visualizzando gli elementi nel terminale durante la creazione.

> [!NOTE]
> Andare al database in MongoDB Atlas, nella scheda _Collections_.
> Ora dovrebbe essere possibile esplorare singole collection di Books, Authors, Genres e BookInstances e controllare singoli documenti.

## Riepilogo

In questo articolo sono stati approfonditi i database e gli ORM in Node/Express, nonché il modo in cui vengono definiti gli schemi e i modelli Mongoose. Queste informazioni sono poi state utilizzate per progettare e implementare i modelli `Book`, `BookInstance`, `Author` e `Genre` per il sito web _LocalLibrary_.

Infine, i modelli sono stati testati creando diverse istanze tramite uno script autonomo. Nel prossimo articolo verrà illustrata la creazione di alcune pagine per visualizzare questi oggetti.

## Vedere anche

- [Integrazione del database](https://expressjs.com/en/guide/database-integration/) (documentazione di Express)
- [Sito web di Mongoose](https://mongoosejs.com/) (documentazione di Mongoose)
- [Guida di Mongoose](https://mongoosejs.com/docs/guide.html) (documentazione di Mongoose)
- [Validation](https://mongoosejs.com/docs/validation.html) (documentazione di Mongoose)
- [Schema Types](https://mongoosejs.com/docs/schematypes.html) (documentazione di Mongoose)
- [Models](https://mongoosejs.com/docs/models.html) (documentazione di Mongoose)
- [Queries](https://mongoosejs.com/docs/queries.html) (documentazione di Mongoose)
- [Population](https://mongoosejs.com/docs/populate.html) (documentazione di Mongoose)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
