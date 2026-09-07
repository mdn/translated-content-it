---
title: Nozioni di base sugli oggetti JavaScript
short-title: Objects
slug: Learn_web_development/Core/Scripting/Object_basics
l10n:
  sourceCommit: ce12c10364f35c64184dec44be85537b7e10d91f
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Events","Learn_web_development/Core/Scripting/Test_your_skills/Object_basics", "Learn_web_development/Core/Scripting")}}

In questo articolo verrà esaminata la sintassi fondamentale degli oggetti JavaScript e verranno riprese alcune funzionalità JavaScript già viste in precedenza nel corso, ribadendo il fatto che molte delle funzionalità già utilizzate sono oggetti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le nozioni di base di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere che in JavaScript la maggior parte delle cose sono oggetti e che probabilmente sono stati utilizzati oggetti ogni volta che si è usato JavaScript.</li>
          <li>Sintassi di base: letterali oggetto, proprietà e metodi, annidamento di oggetti e array negli oggetti.</li>
          <li>Uso dei costruttori per creare un nuovo oggetto.</li>
          <li>Ambito degli oggetti e <code>this</code>.</li>
          <li>Accesso a proprietà e metodi — sintassi con parentesi quadre e punto.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Nozioni di base sugli oggetti

Un oggetto è una raccolta di dati e/o funzionalità correlati.
Questi consistono generalmente di diverse variabili e funzioni, che vengono chiamate proprietà e metodi quando si trovano all'interno degli oggetti.
Analizziamo un esempio per comprendere il loro aspetto.

Per iniziare, creare una copia locale del file [oojs.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/introduction/oojs.html). Contiene pochissimo: un elemento {{HTMLElement("script")}} in cui scrivere il codice sorgente. Verrà usato come base per esplorare la sintassi di base degli oggetti. Durante il lavoro su questo esempio, tenere aperta e pronta per digitare alcuni comandi la [console JavaScript degli strumenti di sviluppo](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#the_javascript_console).

Come per molte cose in JavaScript, la creazione di un oggetto inizia spesso definendo e inizializzando una variabile. Provare a inserire la seguente riga sotto il codice JavaScript già presente nel file, quindi salvare e aggiornare la pagina:

```js
const person = {};
```

Ora aprire la [console JavaScript](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#the_javascript_console) del browser, inserire `person` e premere <kbd>Enter</kbd>/<kbd>Return</kbd>. Si dovrebbe ottenere un risultato simile a una delle righe seguenti:

```plain
[object Object]
Object { }
{ }
```

Congratulazioni, è stato appena creato il primo oggetto. Fatto! Tuttavia, questo è un oggetto vuoto, quindi non è possibile farci molto. Aggiornare l'oggetto JavaScript nel file affinché abbia questo aspetto:

```js
const person = {
  name: ["Bob", "Smith"],
  age: 32,
  bio: function () {
    console.log(`${this.name[0]} ${this.name[1]} is ${this.age} years old.`);
  },
  introduceSelf: function () {
    console.log(`Hi! I'm ${this.name[0]}.`);
  },
};
```

Dopo aver salvato e aggiornato, provare a inserire alcuni dei seguenti elementi nella console JavaScript degli strumenti di sviluppo del browser:

```js
person.name;
person.name[0];
person.age;
person.bio();
// "Bob Smith is 32 years old."
person.introduceSelf();
// "Hi! I'm Bob."
```

L'oggetto ora contiene dati e funzionalità, a cui è possibile accedere con una sintassi semplice e chiara.

Cosa sta succedendo qui? Un oggetto è composto da più membri, ciascuno dei quali ha un nome, ad esempio `name` e `age` sopra, e un valore, ad esempio `['Bob', 'Smith']` e `32`. Ogni coppia nome/valore deve essere separata da una virgola, mentre nome e valore in ciascun caso sono separati da due punti. La sintassi segue sempre questo schema:

```js
const objectName = {
  member1Name: member1Value,
  member2Name: member2Value,
  member3Name: member3Value,
};
```

Il valore di un membro dell'oggetto può essere praticamente qualsiasi cosa: nel nostro oggetto person sono presenti un numero, un array e due funzioni. I primi due elementi sono elementi di dati e vengono chiamati **proprietà** dell'oggetto. Gli ultimi due elementi sono funzioni che consentono all'oggetto di fare qualcosa con quei dati e vengono chiamati **metodi** dell'oggetto.

Quando i membri dell'oggetto sono funzioni, esiste una sintassi più semplice. Invece di `bio: function ()` è possibile scrivere `bio()`. In questo modo:

```js
const person = {
  name: ["Bob", "Smith"],
  age: 32,
  bio() {
    console.log(`${this.name[0]} ${this.name[1]} is ${this.age} years old.`);
  },
  introduceSelf() {
    console.log(`Hi! I'm ${this.name[0]}.`);
  },
};
```

D'ora in poi verrà usata questa sintassi più breve.

Un oggetto di questo tipo viene chiamato **letterale oggetto**: il contenuto dell'oggetto è stato letteralmente scritto durante la sua creazione. Questo è diverso dagli oggetti istanziati dalle classi, che verranno esaminati più avanti.

È molto comune creare un oggetto tramite un letterale oggetto quando si desidera trasferire in qualche modo una serie di elementi di dati strutturati e correlati, ad esempio inviando una richiesta al server per inserirli in un database. Inviare un singolo oggetto è molto più efficiente che inviare diversi elementi singolarmente ed è più facile da gestire rispetto a un array quando si vogliono identificare i singoli elementi per nome.

## Notazione con punto

In precedenza, è stato effettuato l'accesso alle proprietà e ai metodi dell'oggetto mediante la **notazione con punto**. Il nome dell'oggetto (`person`) agisce come **spazio dei nomi**: deve essere inserito per primo per accedere a qualsiasi elemento all'interno dell'oggetto. Successivamente si scrive un punto, quindi l'elemento a cui si vuole accedere: può essere il nome di una proprietà semplice, un elemento di una proprietà array o una chiamata a uno dei metodi dell'oggetto, ad esempio:

```js
person.age;
person.bio();
```

### Oggetti come proprietà di oggetti

Una proprietà di un oggetto può essere a sua volta un oggetto. Ad esempio, provare a modificare il membro `name` da

```js
const person = {
  name: ["Bob", "Smith"],
};
```

a

```js
const person = {
  name: {
    first: "Bob",
    last: "Smith",
  },
  // …
};
```

Per accedere a questi elementi è sufficiente concatenare il passaggio aggiuntivo alla fine con un altro punto. Provare questi esempi nella console JS:

```js
person.name.first;
person.name.last;
```

In questo caso, sarà inoltre necessario esaminare il codice dei metodi e modificare tutte le occorrenze di

```js
name[0];
name[1];
```

in

```js
name.first;
name.last;
```

Altrimenti, i metodi non funzioneranno più.

## Notazione con parentesi quadre

La notazione con parentesi quadre fornisce un modo alternativo per accedere alle proprietà degli oggetti.
Invece di usare la [notazione con punto](#notazione_con_punto) in questo modo:

```js
person.age;
person.name.first;
```

È possibile invece usare le parentesi quadre:

```js
person["age"];
person["name"]["first"];
```

Questo è molto simile a come si accede agli elementi di un array ed è essenzialmente la stessa cosa: invece di usare un numero di indice per selezionare un elemento, viene usato il nome associato al valore di ciascun membro.
Non sorprende che gli oggetti vengano talvolta chiamati **array associativi**: associano stringhe a valori nello stesso modo in cui gli array associano numeri a valori.

In genere si preferisce la notazione con punto rispetto alla notazione con parentesi quadre perché è più concisa e facile da leggere.
Tuttavia, esistono alcuni casi in cui è necessario usare le parentesi quadre.
Ad esempio, se il nome di una proprietà dell'oggetto è contenuto in una variabile, non è possibile usare la notazione con punto per accedere al valore, ma è possibile accedervi mediante la notazione con parentesi quadre.

Nell'esempio seguente, la funzione `logProperty()` può usare `person[propertyName]` per recuperare il valore della proprietà il cui nome è contenuto in `propertyName`.

```js
const person = {
  name: ["Bob", "Smith"],
  age: 32,
};

function logProperty(propertyName) {
  console.log(person[propertyName]);
}

logProperty("name");
// ["Bob", "Smith"]
logProperty("age");
// 32
```

## Impostare i membri di un oggetto

Finora è stato esaminato solo il recupero, o **lettura**, dei membri di un oggetto: è possibile anche **impostare** (aggiornare) il valore dei membri di un oggetto dichiarando il membro da impostare, usando la notazione con punto o con parentesi quadre, in questo modo:

```js
person.age = 45;
person["name"]["last"] = "Cratchit";
```

Provare a inserire le righe precedenti e poi a recuperare nuovamente i membri per vedere come sono cambiati:

```js
person.age;
person["name"]["last"];
```

L'impostazione dei membri non si limita all'aggiornamento dei valori di proprietà e metodi esistenti: è anche possibile creare membri completamente nuovi. Provare questi esempi nella console JS:

```js
person["eyes"] = "hazel";
person.farewell = function () {
  console.log("Bye everybody!");
};
```

Ora è possibile verificare i nuovi membri:

```js
person["eyes"];
person.farewell();
// "Bye everybody!"
```

Un aspetto utile della notazione con parentesi quadre è che può essere usata per impostare dinamicamente non solo i valori dei membri, ma anche i loro nomi. Si supponga di voler consentire agli utenti di memorizzare tipi di valori personalizzati nei propri dati sulle persone, digitando il nome e il valore del membro in due input di testo. Questi valori potrebbero essere ottenuti in questo modo:

```js
const myDataName = nameInput.value;
const myDataValue = nameValue.value;
```

Il nuovo nome e valore del membro potrebbero quindi essere aggiunti all'oggetto `person` in questo modo:

```js
person[myDataName] = myDataValue;
```

Per verificarlo, provare ad aggiungere le seguenti righe nel codice, subito sotto la parentesi graffa di chiusura dell'oggetto `person`:

```js
const myDataName = "height";
const myDataValue = "1.75m";
person[myDataName] = myDataValue;
```

Ora provare a salvare e aggiornare la pagina, quindi inserire quanto segue nell'input di testo:

```js
person.height;
```

L'aggiunta di una proprietà a un oggetto usando il metodo sopra non è possibile con la notazione con punto, che può accettare solo un nome di membro letterale, non il valore di una variabile che punta a un nome.

## Che cos'è "this"?

Potrebbe essere stato notato qualcosa di leggermente strano nei metodi. Osservare ad esempio questo:

```js
const person = {
  // …
  introduceSelf() {
    console.log(`Hi! I'm ${this.name[0]}.`);
  },
};
```

Probabilmente ci si sta chiedendo che cosa sia "this". La parola chiave `this` si riferisce in genere all'oggetto corrente in cui viene eseguito il codice. Nel contesto di un metodo di un oggetto, `this` si riferisce all'oggetto su cui è stato chiamato il metodo.

Illustriamo il concetto con una coppia semplificata di oggetti person:

```js
const person1 = {
  name: "Chris",
  introduceSelf() {
    console.log(`Hi! I'm ${this.name}.`);
  },
};

const person2 = {
  name: "Deepti",
  introduceSelf() {
    console.log(`Hi! I'm ${this.name}.`);
  },
};
```

In questo caso, `person1.introduceSelf()` restituisce "Hi! I'm Chris."; `person2.introduceSelf()` restituisce "Hi! I'm Deepti." Questo accade perché, quando viene chiamato il metodo, `this` si riferisce all'oggetto sul quale il metodo viene chiamato, consentendo alla stessa definizione del metodo di funzionare per più oggetti.

Questo non è particolarmente utile quando si scrivono letterali oggetto manualmente, poiché usare il nome dell'oggetto (`person1` e `person2`) porta allo stesso identico risultato, ma sarà essenziale quando si inizieranno a usare i **costruttori** per creare più di un oggetto da una singola definizione di oggetto. Questo è l'argomento della sezione successiva.

## Introduzione ai costruttori

L'uso dei letterali oggetto va bene quando è necessario creare un solo oggetto, ma se occorre crearne più di uno, come nella sezione precedente, risultano decisamente inadeguati. Bisogna scrivere lo stesso codice per ogni oggetto creato e, se si vogliono modificare alcune proprietà dell'oggetto, ad esempio aggiungere una proprietà `height`, bisogna ricordarsi di aggiornare ogni oggetto.

Sarebbe utile disporre di un modo per definire la "forma" di un oggetto, ovvero l'insieme di metodi e proprietà che può avere, e quindi creare tutti gli oggetti desiderati aggiornando solo i valori delle proprietà che differiscono.

La prima versione di questo è semplicemente una funzione:

```js
function createPerson(name) {
  const obj = {};
  obj.name = name;
  obj.introduceSelf = function () {
    console.log(`Hi! I'm ${this.name}.`);
  };
  return obj;
}
```

Questa funzione crea e restituisce un nuovo oggetto ogni volta che viene chiamata. L'oggetto avrà due membri:

- una proprietà `name`
- un metodo `introduceSelf()`.

Notare che `createPerson()` accetta un parametro `name` per impostare il valore della proprietà `name`, ma il valore del metodo `introduceSelf()` sarà lo stesso per tutti gli oggetti creati usando questa funzione. Questo è uno schema molto comune per creare oggetti.

Ora è possibile creare tutti gli oggetti desiderati riutilizzando la definizione:

```js
const salva = createPerson("Salva");
salva.introduceSelf();
// "Hi! I'm Salva."

const frankie = createPerson("Frankie");
frankie.introduceSelf();
// "Hi! I'm Frankie."
```

Questo funziona correttamente, ma è piuttosto prolisso: bisogna creare un oggetto vuoto, inizializzarlo e restituirlo. Un modo migliore consiste nell'usare un **costruttore**. Un costruttore è semplicemente una funzione chiamata usando la parola chiave {{jsxref("new")}}. Quando viene chiamato un costruttore, esso:

- crea un nuovo oggetto
- associa `this` al nuovo oggetto, in modo da poter fare riferimento a `this` nel codice del costruttore
- esegue il codice nel costruttore
- restituisce il nuovo oggetto.

Per convenzione, i costruttori iniziano con una lettera maiuscola e prendono il nome dal tipo di oggetto che creano. L'esempio può quindi essere riscritto in questo modo:

```js
function Person(name) {
  this.name = name;
  this.introduceSelf = function () {
    console.log(`Hi! I'm ${this.name}.`);
  };
}
```

Per chiamare `Person()` come costruttore, si usa `new`:

```js
const salva = new Person("Salva");
salva.introduceSelf();
// "Hi! I'm Salva."

const frankie = new Person("Frankie");
frankie.introduceSelf();
// "Hi! I'm Frankie."
```

## Gli oggetti sono stati usati fin dall'inizio

Durante l'analisi di questi esempi, la notazione con punto utilizzata potrebbe essere sembrata molto familiare. Questo perché è stata usata per tutto il corso. Ogni volta che è stato analizzato un esempio che usa un'API del browser integrata o un oggetto JavaScript, sono stati usati oggetti, perché tali funzionalità sono realizzate usando esattamente lo stesso tipo di strutture a oggetti esaminate qui, sebbene più complesse rispetto agli esempi personalizzati di base.

Quindi, quando sono stati usati metodi delle stringhe come:

```js
myString.split(",");
```

È stato usato un metodo disponibile su un oggetto [`String`](/it/docs/Web/JavaScript/Reference/Global_Objects/String). Ogni volta che viene creata una stringa nel codice, tale stringa viene automaticamente creata come istanza di `String` e dispone quindi di diversi metodi e proprietà comuni.

Quando si è effettuato l'accesso al modello a oggetti del documento usando righe come questa:

```js
const myDiv = document.createElement("div");
const myVideo = document.querySelector("video");
```

Sono stati usati metodi disponibili su un oggetto [`Document`](/it/docs/Web/API/Document). Per ogni pagina web caricata viene creata un'istanza di `Document`, chiamata `document`, che rappresenta l'intera struttura, il contenuto e altre funzionalità della pagina, come il suo URL. Anche questo significa che dispone di diversi metodi e proprietà comuni.

Lo stesso vale per praticamente qualsiasi altro oggetto integrato o API usata: [`Array`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array), [`Math`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math) e così via.

Notare che gli oggetti e le API integrati non creano sempre automaticamente istanze di oggetti. Ad esempio, la [Notifications API](/it/docs/Web/API/Notifications_API), che consente ai browser moderni di attivare notifiche di sistema, richiede di istanziare un nuovo oggetto usando il costruttore per ogni notifica da attivare. Provare a inserire quanto segue nella console JavaScript:

```js
const myNotification = new Notification("Hello!");
```

## Riepilogo

A questo punto dovrebbe essere chiaro come lavorare con gli oggetti in JavaScript, inclusa la creazione di semplici oggetti personalizzati. Dovrebbe anche risultare evidente che gli oggetti sono molto utili come strutture per memorizzare dati e funzionalità correlati: se si provasse a tenere traccia di tutte le proprietà e i metodi del nostro oggetto `person` come variabili e funzioni separate, sarebbe inefficiente e frustrante, con il rischio di entrare in conflitto con altre variabili e funzioni che hanno gli stessi nomi. Gli oggetti consentono di mantenere le informazioni al sicuro nel proprio contenitore, lontano da possibili problemi.

Nel prossimo articolo verranno proposti alcuni test per verificare quanto bene sono state comprese e memorizzate tutte queste informazioni.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Events","Learn_web_development/Core/Scripting/Test_your_skills/Object_basics", "Learn_web_development/Core/Scripting")}}
