---
title: Concetti di base sugli oggetti JavaScript
short-title: Objects
slug: Learn_web_development/Core/Scripting/Object_basics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Image_gallery","Learn_web_development/Core/Scripting/DOM_scripting", "Learn_web_development/Core/Scripting")}}

In questo articolo, esamineremo la sintassi fondamentale degli oggetti JavaScript e riprenderemo alcune caratteristiche di JavaScript che abbiamo già visto in precedenza nel corso, ribadendo il fatto che molte delle funzionalità con cui ha già avuto a che fare sono oggetti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi del CSS</a>, familiarità con i concetti di base di JavaScript trattati nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere che in JavaScript la maggior parte delle cose sono oggetti, e probabilmente ha usato oggetti ogni volta che ha toccato JavaScript.</li>
          <li>Sintassi di base: oggetti letterali, proprietà e metodi, annidamento di oggetti e array negli oggetti.</li>
          <li>Utilizzo di costruttori per creare un nuovo oggetto.</li>
          <li>Ambito degli oggetti e <code>this</code>.</li>
          <li>Accesso alle proprietà e ai metodi: sintassi con parentesi quadre e notazione a punto.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Concetti di base sugli oggetti

Un oggetto è una raccolta di dati e/o funzionalità correlati.
Questi consistono generalmente in diverse variabili e funzioni (che vengono chiamate proprietà e metodi quando si trovano all'interno degli oggetti).
Esaminiamo un esempio per capire di cosa si tratta.

Per iniziare, faccia una copia locale del nostro file [oojs.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/introduction/oojs.html). Questo contiene molto poco — un elemento {{HTMLElement("script")}} in cui scrivere il nostro codice sorgente. Lo useremo come base per esplorare la sintassi di base degli oggetti. Durante il lavoro con questo esempio dovrebbe avere aperto i suoi [strumenti di sviluppo, console JavaScript](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#the_javascript_console) e pronta per digitare alcuni comandi.

Come per molte cose in JavaScript, creare un oggetto inizia spesso con la definizione e l'inizializzazione di una variabile. Provi a immettere la seguente riga sotto il codice JavaScript già presente nel suo file, quindi salvi e aggiorni:

```js
const person = {};
```

Ora apra la console JavaScript del suo browser, immetta `person` e premi <kbd>Invio</kbd>/<kbd>Return</kbd>. Dovrebbe ottenere un risultato simile a una delle righe indicate di seguito:

```plain
[object Object]
Object { }
{ }
```

Congratulazioni, ha appena creato il suo primo oggetto. Lavoro fatto! Ma questo è un oggetto vuoto, quindi non possiamo fare molto con esso. Aggiorniamo l'oggetto JavaScript nel nostro file per sembrare così:

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

Dopo aver salvato e aggiornato, provi a immettere alcuni dei seguenti elementi nella console JavaScript del suo browser devtools:

```js
person.name;
person.name[0];
person.age;
person.bio();
// "Bob Smith is 32 years old."
person.introduceSelf();
// "Hi! I'm Bob."
```

Ora ha inserito alcuni dati e funzionalità nel suo oggetto e può ora accedervi con una sintassi semplice e pulita!

Quindi cosa sta succedendo qui? Beh, un oggetto è composto da più membri, ciascuno dei quali ha un nome (ad esempio, `name` e `age` sopra), e un valore (ad esempio, `['Bob', 'Smith']` e `32`). Ogni coppia di nome/valore deve essere separata da una virgola, e il nome e il valore in ogni caso sono separati da due punti. La sintassi segue sempre questo schema:

```js
const objectName = {
  member1Name: member1Value,
  member2Name: member2Value,
  member3Name: member3Value,
};
```

Il valore di un membro dell'oggetto può essere praticamente qualsiasi cosa — nel nostro oggetto `person` abbiamo un numero, un array e due funzioni. I primi due elementi sono elementi dati e vengono riferiti come **proprietà** dell'oggetto. Gli ultimi due elementi sono funzioni che permettono all'oggetto di fare qualcosa con quei dati e vengono definiti **metodi** dell'oggetto.

Quando i membri dell'oggetto sono funzioni, esiste una sintassi più semplice. Invece di `bio: function ()` possiamo scrivere `bio()`. In questo modo:

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

D'ora in poi, useremo questa sintassi più breve.

Un oggetto come questo viene definito **oggetto letterale** — abbiamo letteralmente scritto il contenuto dell'oggetto mentre siamo giunti a crearlo. Questo differisce rispetto agli oggetti istanziati da classi, che vedremo in seguito.

È molto comune creare un oggetto utilizzando un oggetto letterale quando si desidera trasferire una serie di elementi di dati strutturati e correlati in qualche modo, ad esempio inviando una richiesta al server per essere inserita in un database. Inviare un oggetto singolo è molto più efficiente che inviare diversi elementi singolarmente, ed è più semplice da gestire rispetto a un array, quando si desidera identificare singoli elementi per nome.

## Notazione a punto

In precedenza, ha avuto accesso alle proprietà e ai metodi dell'oggetto utilizzando la **notazione a punto**. Il nome dell'oggetto (person) funge da **spazio dei nomi** — deve essere inserito per primo per accedere a qualsiasi cosa all'interno dell'oggetto. Successivamente scriva un punto, poi l'elemento a cui vuole accedere — questo può essere il nome di una proprietà semplice, un elemento di una proprietà array, o una chiamata a uno dei metodi dell'oggetto, per esempio:

```js
person.age;
person.bio();
```

### Oggetti come proprietà di oggetti

Una proprietà di un oggetto può essere essa stessa un oggetto. Ad esempio, provi a cambiare il membro `name` da

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

Per accedere a questi elementi è sufficiente concatenare il passaggio aggiuntivo alla fine con un altro punto. Provi questi nella console JavaScript:

```js
person.name.first;
person.name.last;
```

Se fa questo, avrà anche bisogno di esaminare il codice dei suoi metodi e cambiare qualsiasi istanza di

```js
name[0];
name[1];
```

in

```js
name.first;
name.last;
```

Altrimenti, i suoi metodi non funzioneranno più.

## Notazione a parentesi quadre

La notazione a parentesi quadre offre un modo alternativo per accedere alle proprietà degli oggetti.
Invece di usare [la notazione a punto](#notazione_a_punto) come questa:

```js
person.age;
person.name.first;
```

Può invece usare le parentesi quadre:

```js
person["age"];
person["name"]["first"];
```

Questo assomiglia molto a come si accede agli elementi in un array, ed è fondamentalmente la stessa cosa — invece di usare un numero indice per selezionare un elemento, sta usando il nome associato al valore di ciascun membro.
Non è una meraviglia che gli oggetti siano talvolta chiamati **array associativi** — mappano stringhe su valori nello stesso modo in cui gli array mappano numeri su valori.

La notazione a punto è generalmente preferita alla notazione a parentesi quadre perché è più succinta e facile da leggere.
Tuttavia, ci sono alcuni casi in cui deve usare le parentesi quadre.
Ad esempio, se il nome di una proprietà dell'oggetto è contenuto in una variabile, allora non può usare la notazione a punto per accedere al valore, ma può accedere al valore utilizzando la notazione a parentesi quadre.

Nell'esempio qui sotto, la funzione `logProperty()` può usare `person[propertyName]` per recuperare il valore della proprietà nominata in `propertyName`.

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

## Impostazione dei membri degli oggetti

Finora abbiamo esaminato solo il recupero (o il **getting**) dei membri degli oggetti — può anche **impostare** (aggiornare) il valore dei membri degli oggetti dichiarando il membro che vuole impostare (usando la notazione a punto o a parentesi quadre), in questo modo:

```js
person.age = 45;
person["name"]["last"] = "Cratchit";
```

Provi a immettere le righe precedenti, e quindi a ottenere nuovamente i membri per vedere come sono cambiati, in questo modo:

```js
person.age;
person["name"]["last"];
```

L'impostazione dei membri non si ferma solo all'aggiornamento dei valori delle proprietà e dei metodi esistenti; può anche creare membri completamente nuovi. Provi questi nella console JavaScript:

```js
person["eyes"] = "hazel";
person.farewell = function () {
  console.log("Bye everybody!");
};
```

Può ora testare i suoi nuovi membri:

```js
person["eyes"];
person.farewell();
// "Bye everybody!"
```

Un aspetto utile della notazione a parentesi quadre è che può essere utilizzata per impostare valori dei membri non solo dinamicamente, ma anche i nomi dei membri. Diciamo che vogliamo che gli utenti siano in grado di memorizzare tipi di valore personalizzati nei loro dati delle persone, digitando il nome del membro e il valore in due input di testo. Potremmo ottenere quei valori in questo modo:

```js
const myDataName = nameInput.value;
const myDataValue = nameValue.value;
```

Potremmo quindi aggiungere questo nuovo nome di membro e valore all'oggetto `person` in questo modo:

```js
person[myDataName] = myDataValue;
```

Per testare questo, provi ad aggiungere le seguenti righe nel suo codice, subito sotto la graffa di chiusura dell'oggetto `person`:

```js
const myDataName = "height";
const myDataValue = "1.75m";
person[myDataName] = myDataValue;
```

Ora provi a salvare e aggiornare, e ad immettere quanto segue nel suo input di testo:

```js
person.height;
```

Aggiungere una proprietà a un oggetto usando il metodo sopra non è possibile con la notazione a punto, che può accettare solo un nome di membro letterale, non un valore variabile che punta a un nome.

## Cosa è "this"?

Avrà notato qualcosa di leggermente strano nei nostri metodi. Guardi ad esempio questo:

```js
const person = {
  // …
  introduceSelf() {
    console.log(`Hi! I'm ${this.name[0]}.`);
  },
};
```

Probabilmente si starà chiedendo cosa sia "this". La parola chiave `this` generalmente si riferisce all'oggetto corrente in cui il codice viene eseguito. Nel contesto di un metodo di un oggetto, `this` si riferisce all'oggetto su cui è stato chiamato il metodo.

Illustriamo ciò che intendiamo con una coppia semplificata di oggetti persona:

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

In questo caso, `person1.introduceSelf()` restituisce "Hi! I'm Chris."; `person2.introduceSelf()` restituisce "Hi! I'm Deepti." Questo accade perché quando il metodo viene chiamato, `this` si riferisce all'oggetto su cui è stato chiamato il metodo, il che consente alla stessa definizione di metodo di funzionare per più oggetti.

Questo non è estremamente utile quando scrive oggetti letterali a mano, poiché utilizzare il nome dell'oggetto (`person1` e `person2`) porta esattamente allo stesso risultato, ma sarà essenziale quando inizieremo a usare **costruttori** per creare più di un oggetto da una singola definizione di oggetto, e questo è l'argomento della sezione successiva.

## Introduzione ai costruttori

L'uso di oggetti letterali va bene quando deve creare solo un oggetto, ma se deve crearne più di uno, come nella sezione precedente, non sono adeguati. Dobbiamo scrivere lo stesso codice per ogni oggetto che creiamo e, se vogliamo cambiare alcune proprietà dell'oggetto - come aggiungere una proprietà `height` - allora dobbiamo ricordarci di aggiornare ogni oggetto.

Vorremmo un modo per definire la "forma" di un oggetto — il set di metodi e le proprietà che può avere — e quindi creare quanti oggetti vogliamo, aggiornando solo i valori per le proprietà che sono diverse.

La prima versione di ciò è solo una funzione:

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

Questa funzione crea e restituisce un nuovo oggetto ogni volta che la chiamiamo. L'oggetto avrà due membri:

- una proprietà `name`
- un metodo `introduceSelf()`.

Noti che `createPerson()` prende un parametro `name` per impostare il valore della proprietà `name`, ma il valore del metodo `introduceSelf()` sarà lo stesso per tutti gli oggetti creati utilizzando questa funzione. Questo è un modello molto comune per la creazione di oggetti.

Ora possiamo creare quanti oggetti vogliamo, riutilizzando la definizione:

```js
const salva = createPerson("Salva");
salva.introduceSelf();
// "Hi! I'm Salva."

const frankie = createPerson("Frankie");
frankie.introduceSelf();
// "Hi! I'm Frankie."
```

Questo funziona bene, ma è un po' lungo e complicato: dobbiamo creare un oggetto vuoto, inizializzarlo e restituirlo. Un modo migliore è usare un **costruttore**. Un costruttore è semplicemente una funzione chiamata utilizzando la parola chiave {{jsxref("operators/new", "new")}}. Quando chiama un costruttore, esso:

- crea un nuovo oggetto
- lega `this` al nuovo oggetto, in modo che possa riferirsi a `this` nel codice del costruttore
- esegue il codice nel costruttore
- restituisce il nuovo oggetto.

I costruttori, per convenzione, iniziano con una lettera maiuscola e sono nominati per il tipo di oggetto che creano. Quindi potremmo riscrivere il nostro esempio in questo modo:

```js
function Person(name) {
  this.name = name;
  this.introduceSelf = function () {
    console.log(`Hi! I'm ${this.name}.`);
  };
}
```

Per chiamare `Person()` come costruttore, usiamo `new`:

```js
const salva = new Person("Salva");
salva.introduceSelf();
// "Hi! I'm Salva."

const frankie = new Person("Frankie");
frankie.introduceSelf();
// "Hi! I'm Frankie."
```

## Ha usato oggetti fin dall'inizio

Mentre ha seguito questi esempi, probabilmente ha pensato che la notazione a punto che sta usando sia molto familiare. Questo è perché l'ha usata durante tutto il corso! Ogni volta che abbiamo lavorato su un esempio che utilizza un'API del browser incorporata o un oggetto JavaScript, abbiamo usato oggetti, perché tali funzionalità sono costruite usando esattamente lo stesso tipo di strutture di oggetto che abbiamo esaminato qui, sebbene più complesse rispetto ai nostri esempi di base personalizzati.

Quindi, quando ha usato i metodi della stringa come:

```js
myString.split(",");
```

Ha usato un metodo disponibile su un oggetto [`String`](/it/docs/Web/JavaScript/Reference/Global_Objects/String). Ogni volta che crea una stringa nel suo codice, quella stringa viene automaticamente creata come istanza di `String`, e quindi ha diversi metodi e proprietà comuni disponibili su di essa.

Quando ha avuto accesso al modello di oggetto documento utilizzando linee come questa:

```js
const myDiv = document.createElement("div");
const myVideo = document.querySelector("video");
```

Ha usato metodi disponibili su un oggetto [`Document`](/it/docs/Web/API/Document). Per ogni pagina web caricata, viene creata un'istanza di `Document`, chiamata `document`, che rappresenta l'intera struttura della pagina, il contenuto e altre caratteristiche come l'URL. Ancora una volta, questo significa che ha diversi metodi e proprietà comuni a sua disposizione.

Lo stesso vale per praticamente qualsiasi altro oggetto o API incorporato che ha utilizzato — [`Array`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array), [`Math`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math) e così via.

Si noti che gli oggetti e le API incorporate non creano sempre automaticamente istanze di oggetti. Ad esempio, l'[API delle notifiche](/it/docs/Web/API/Notifications_API) — che consente ai browser moderni di inviare notifiche di sistema — richiede di istanziare una nuova istanza di oggetto utilizzando il costruttore per ogni notifica che desidera inviare. Provi a immettere quanto segue nella console JavaScript:

```js
const myNotification = new Notification("Hello!");
```

## Testi le sue abilità!

Ha raggiunto la fine di questo articolo, ma ricorda le informazioni più importanti? Può trovare ulteriori test per verificare che abbia conservato queste informazioni prima di procedere — veda [Testa le sue abilità: Concetti di base sugli oggetti](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Object_basics).

## Sommario

Ora dovrebbe avere una buona idea di come lavorare con gli oggetti in JavaScript — incluso la creazione dei suoi semplici oggetti. Dovrebbe anche apprezzare che gli oggetti sono molto utili come strutture per memorizzare dati e funzionalità correlate — se cercasse di tenere traccia di tutte le proprietà e i metodi del nostro oggetto `person` come variabili e funzioni separate, sarebbe inefficiente e frustrante e correremmo il rischio di scontrarci con altre variabili e funzioni che hanno gli stessi nomi. Gli oggetti ci permettono di tenere le informazioni al sicuro, racchiuse nel loro pacchetto, fuori da danni.

Nel prossimo articolo esamineremo la **scripting del DOM**, che sblocca una grande quantità di funzionalità fondamentali delle API del browser.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Image_gallery","Learn_web_development/Core/Scripting/DOM_scripting", "Learn_web_development/Core/Scripting")}}
