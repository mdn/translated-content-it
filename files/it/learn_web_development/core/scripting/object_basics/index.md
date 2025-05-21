---
title: Nozioni di base sugli oggetti JavaScript
short-title: Objects
slug: Learn_web_development/Core/Scripting/Object_basics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Image_gallery","Learn_web_development/Core/Scripting/DOM_scripting", "Learn_web_development/Core/Scripting")}}

In questo articolo esamineremo la sintassi fondamentale degli oggetti JavaScript, e rivedremo alcune caratteristiche di JavaScript che abbiamo già visto in precedenza nel corso, ribadendo il fatto che molte delle funzionalità con cui hai già lavorato sono oggetti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Capire che in JavaScript la maggior parte delle cose sono oggetti, e probabilmente hai usato oggetti ogni volta che hai toccato JavaScript.</li>
          <li>Sintassi di base: Oggetti letterali, proprietà e metodi, nidificazione di oggetti e array negli oggetti.</li>
          <li>Utilizzare i costruttori per creare un nuovo oggetto.</li>
          <li>Scope dell'oggetto, e <code>this</code>.</li>
          <li>Accesso a proprietà e metodi — sintassi a parentesi e punto.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Nozioni di base sugli oggetti

Un oggetto è una collezione di dati e/o funzionalità correlati.
Questi solitamente consistono in diverse variabili e funzioni (che sono chiamate proprietà e metodi quando sono all'interno di oggetti).
Lavoriamo attraverso un esempio per capire come appaiono.

Per iniziare, fai una copia locale del nostro file [oojs.html](https://github.com/mdn/learning-area/blob/main/javascript/oojs/introduction/oojs.html). Questo contiene molto poco — un elemento {{HTMLElement("script")}} per scrivere il nostro codice sorgente. Lo useremo come base per esplorare la sintassi degli oggetti di base. Mentre lavori con questo esempio, dovresti avere aperta e pronta a ricevere comandi la tua [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#the_javascript_console).

Come avviene in molte cose in JavaScript, la creazione di un oggetto inizia spesso con la definizione e l'inizializzazione di una variabile. Prova a inserire la riga seguente sotto il codice JavaScript già presente nel tuo file, poi salva e aggiorna:

```js
const person = {};
```

Ora apri la [console JavaScript](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#the_javascript_console) del tuo browser, inserisci `person` e premi <kbd>Enter</kbd>/<kbd>Return</kbd>. Dovresti ottenere un risultato simile a una delle righe seguenti:

```plain
[object Object]
Object { }
{ }
```

Congratulazioni, hai appena creato il tuo primo oggetto. Lavoro fatto! Ma questo è un oggetto vuoto, quindi non possiamo fare molto con esso. Aggiorniamo il nostro oggetto JavaScript nel nostro file per assomigliare a questo:

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

Dopo aver salvato e aggiornato, prova a inserire alcuni dei seguenti comandi nella console JavaScript sugli strumenti per sviluppatori del tuo browser:

```js
person.name;
person.name[0];
person.age;
person.bio();
// "Bob Smith is 32 years old."
person.introduceSelf();
// "Hi! I'm Bob."
```

Ora hai alcuni dati e funzionalità all'interno del tuo oggetto e sei in grado di accedervi con una sintassi semplice e chiara!

Quindi cosa sta succedendo qui? Beh, un oggetto è composto da più membri, ciascuno dei quali ha un nome (ad es., `name` e `age` sopra), e un valore (ad es., `['Bob', 'Smith']` e `32`). Ogni coppia nome/valore deve essere separata da una virgola, e il nome e il valore in ogni caso sono separati da un due punti. La sintassi segue sempre questo schema:

```js
const objectName = {
  member1Name: member1Value,
  member2Name: member2Value,
  member3Name: member3Value,
};
```

Il valore di un membro dell'oggetto può essere praticamente qualsiasi cosa — nel nostro oggetto person abbiamo un numero, un array, e due funzioni. I primi due elementi sono elementi di dati, e sono indicati come **proprietà** dell'oggetto. Gli ultimi due elementi sono funzioni che permettono all'oggetto di fare qualcosa con quei dati, e sono indicati come **metodi** dell'oggetto.

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

Da ora in poi utilizzeremo questa sintassi abbreviata.

Un oggetto come questo è indicato come **oggetto letterale** — abbiamo scritto letteralmente il contenuto dell'oggetto nel momento della sua creazione. Ciò è diverso rispetto agli oggetti istanziati da classi, che vedremo più avanti.

È molto comune creare un oggetto usando un oggetto letterale quando si vuole trasferire una serie di dati strutturati e correlati in qualche modo, per esempio inviando una richiesta al server per essere inseriti in un database. Inviare un singolo oggetto è molto più efficiente che inviare diversi elementi individualmente, ed è più facile lavorare con esso rispetto a un array, quando vuoi identificare elementi individuali per nome.

## Notazione a punto

Sopra, hai avuto accesso alle proprietà e ai metodi dell'oggetto usando la **notazione a punto**. Il nome dell'oggetto (person) agisce da **namespace** — deve essere inserito per primo per accedere a qualsiasi cosa all'interno dell'oggetto. Successivamente si scrive un punto, quindi l'elemento a cui si desidera accedere — questo può essere il nome di una semplice proprietà, un elemento di un array di proprietà, o una chiamata a uno dei metodi dell'oggetto, ad esempio:

```js
person.age;
person.bio();
```

### Oggetti come proprietà di oggetto

Una proprietà di un oggetto può essere a sua volta un oggetto. Ad esempio, prova a modificare il membro `name` da

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

Per accedere a questi elementi basta concatenare il passaggio aggiuntivo alla fine con un altro punto. Prova questi nella console JS:

```js
person.name.first;
person.name.last;
```

Se fai questo, dovrai anche scorrere il codice dei tuoi metodi e cambiare qualsiasi istanza di

```js
name[0];
name[1];
```

in

```js
name.first;
name.last;
```

Altrimenti, i tuoi metodi non funzioneranno più.

## Notazione a parentesi

La notazione a parentesi fornisce un'alternativa per accedere alle proprietà degli oggetti.
Invece di usare la [notazione a punto](#notazione_a_punto) in questo modo:

```js
person.age;
person.name.first;
```

Puoi invece usare le parentesi quadre:

```js
person["age"];
person["name"]["first"];
```

Questo sembra molto simile al modo in cui si accede agli elementi in un array, ed è fondamentalmente la stessa cosa — invece di utilizzare un numero di indice per selezionare un elemento, stai usando il nome associato al valore di ciascun membro.
Non è sorprendente che gli oggetti siano talvolta chiamati **array associativi** — mappano stringhe ai valori nello stesso modo in cui gli array mappano numeri ai valori.

La notazione a punto è generalmente preferita alla notazione a parentesi perché è più concisa e facile da leggere.
Tuttavia ci sono alcuni casi in cui è necessario utilizzare le parentesi quadre.
Ad esempio, se un nome di proprietà di un oggetto è memorizzato in una variabile, non puoi usare la notazione a punto per accedere al valore, ma puoi accedere al valore utilizzando la notazione a parentesi.

Nell'esempio seguente, la funzione `logProperty()` può utilizzare `person[propertyName]` per recuperare il valore della proprietà nominata in `propertyName`.

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

## Impostazione dei membri dell'oggetto

Finora abbiamo solo esaminato il recupero (o **ottenimento**) dei membri dell'oggetto — puoi anche **impostare** (aggiornare) il valore dei membri dell'oggetto dichiarando il membro che vuoi impostare (usando la notazione a punto o a parentesi), come questo:

```js
person.age = 45;
person["name"]["last"] = "Cratchit";
```

Prova a inserire le righe sopra, e poi ottenere di nuovo i membri per vedere come sono cambiati, in questo modo:

```js
person.age;
person["name"]["last"];
```

L'impostazione dei membri non si limita ad aggiornare i valori delle proprietà e dei metodi esistenti; puoi anche creare membri completamente nuovi. Prova questi nella console JS:

```js
person["eyes"] = "hazel";
person.farewell = function () {
  console.log("Bye everybody!");
};
```

Ora puoi testare i tuoi nuovi membri:

```js
person["eyes"];
person.farewell();
// "Bye everybody!"
```

Un aspetto utile della notazione a parentesi è che può essere utilizzata non solo per impostare i valori dei membri dinamicamente, ma anche per i nomi dei membri. Supponiamo di voler permettere agli utenti di memorizzare tipi di valori personalizzati nei dati delle persone, digitando il nome del membro e il valore in due input di testo. Potremmo ottenere quei valori in questo modo:

```js
const myDataName = nameInput.value;
const myDataValue = nameValue.value;
```

Potremmo quindi aggiungere questo nuovo nome di membro e valore all'oggetto `person` in questo modo:

```js
person[myDataName] = myDataValue;
```

Per testarlo, prova a inserire le seguenti righe nel tuo codice, appena sotto la parentesi graffa di chiusura dell'oggetto `person`:

```js
const myDataName = "height";
const myDataValue = "1.75m";
person[myDataName] = myDataValue;
```

Ora prova a salvare e aggiornare, e inserisci il seguente nei tuoi input di testo:

```js
person.height;
```

Aggiungere una proprietà a un oggetto usando il metodo sopra non è possibile con la notazione a punto, che può accettare solo un nome di membro letterale, non un valore di variabile puntato a un nome.

## Che cos'è "this"?

Potresti aver notato qualcosa di leggermente strano nei nostri metodi. Guarda questo, per esempio:

```js
const person = {
  // …
  introduceSelf() {
    console.log(`Hi! I'm ${this.name[0]}.`);
  },
};
```

Probabilmente ti stai chiedendo cosa sia "this". La parola chiave `this` normalmente si riferisce all'oggetto corrente in cui il codice viene eseguito. Nel contesto di un metodo di oggetto, `this` si riferisce all'oggetto su cui il metodo è stato chiamato.

Illustriamo cosa intendiamo con una coppia semplificata di oggetti person:

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

In questo caso, `person1.introduceSelf()` restituisce "Hi! I'm Chris."; `person2.introduceSelf()` restituisce "Hi! I'm Deepti." Ciò accade perché quando il metodo viene chiamato, `this` si riferisce all'oggetto su cui il metodo è chiamato, il che permette alla stessa definizione del metodo di funzionare per più oggetti.

Questo non è molto utile quando si stanno scrivendo oggetti letterali a mano, poiché usare il nome dell'oggetto (`person1` e `person2`) porta esattamente allo stesso risultato, ma sarà essenziale quando inizieremo a usare i **costruttori** per creare più di un oggetto a partire da una singola definizione di oggetto, ed è l'argomento della sezione successiva.

## Introduzione ai costruttori

Usare oggetti letterali va bene quando è necessario creare un solo oggetto, ma se devi creare più di uno, come nella sezione precedente, risultano seriamente inadeguati. Dobbiamo scrivere lo stesso codice per ogni oggetto che creiamo, e se vogliamo cambiare alcune proprietà dell'oggetto - come aggiungere una proprietà `height` - allora dobbiamo ricordarci di aggiornare ogni oggetto.

Vorremmo un modo per definire la "forma" di un oggetto — l'insieme di metodi e le proprietà che può avere — e poi creare quanti oggetti desideriamo, aggiornando solo i valori per le proprietà che sono diverse.

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

Questa funzione crea e restituisce un nuovo oggetto ogni volta che la chiamiamo. L'oggetto avrà due membri:

- una proprietà `name`
- un metodo `introduceSelf()`.

Nota che `createPerson()` prende un parametro `name` per impostare il valore della proprietà `name`, ma il valore del metodo `introduceSelf()` sarà lo stesso per tutti gli oggetti creati usando questa funzione. Questo è un pattern molto comune per creare oggetti.

Ora possiamo creare quanti oggetti vogliamo, riutilizzando la definizione:

```js
const salva = createPerson("Salva");
salva.introduceSelf();
// "Hi! I'm Salva."

const frankie = createPerson("Frankie");
frankie.introduceSelf();
// "Hi! I'm Frankie."
```

Questo funziona bene ma è un po' prolisso: dobbiamo creare un oggetto vuoto, inizializzarlo e restituirlo. Un modo migliore è usare un **costruttore**. Un costruttore è semplicemente una funzione chiamata usando la parola chiave {{jsxref("operators/new", "new")}}. Quando chiami un costruttore, farà:

- creare un nuovo oggetto
- legare `this` al nuovo oggetto, quindi puoi riferirti a `this` nel tuo codice del costruttore
- eseguire il codice nel costruttore
- restituire il nuovo oggetto.

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

## Hai sempre utilizzato oggetti

Mentre passavi attraverso questi esempi, probabilmente hai pensato che la notazione a punto che stavi usando fosse molto familiare. È perché l'hai usata nel corso! Ogni volta che abbiamo lavorato attraverso un esempio che utilizza un'API del browser integrata o un oggetto JavaScript, abbiamo usato oggetti, perché tali funzionalità sono costruite usando lo stesso tipo di strutture di oggetti che abbiamo esaminato qui, sebbene più complesse rispetto ai nostri semplici esempi personalizzati.

Quindi quando usavi metodi di stringa come:

```js
myString.split(",");
```

Stavi usando un metodo disponibile su un oggetto [`String`](/it/docs/Web/JavaScript/Reference/Global_Objects/String). Ogni volta che crei una stringa nel tuo codice, quella stringa viene automaticamente creata come un'istanza di `String`, e di conseguenza ha diversi metodi e proprietà comuni disponibili su di essa.

Quando accedevi al document object model usando righe come questa:

```js
const myDiv = document.createElement("div");
const myVideo = document.querySelector("video");
```

Stavi usando metodi disponibili su un oggetto [`Document`](/it/docs/Web/API/Document). Per ogni pagina web caricata, viene creata un'istanza di `Document`, chiamata `document`, che rappresenta l'intera struttura della pagina, il contenuto e altre caratteristiche come il suo URL. Ancora una volta, questo significa che ha diversi metodi e proprietà comuni disponibili su di esso.

Lo stesso vale per praticamente qualsiasi altro oggetto integrato o API che hai utilizzato — [`Array`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array), [`Math`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math), e così via.

Nota che gli oggetti e le API incorporati non creano sempre automaticamente istanze di oggetti. Ad esempio, l'[API delle Notifiche](/it/docs/Web/API/Notifications_API) — che consente ai browser moderni di inviare notifiche di sistema — richiede di istanziare una nuova istanza di oggetto utilizzando il costruttore per ogni notifica che vuoi inviare. Prova a inserire quanto segue nella console del tuo JavaScript:

```js
const myNotification = new Notification("Hello!");
```

## Metti alla prova le tue competenze!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare che hai memorizzato queste informazioni prima di andare avanti — vedi [Metti alla prova le tue competenze: Nozioni di base sugli oggetti](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Object_basics).

## Riepilogo

Dovresti ora avere una buona idea di come lavorare con gli oggetti in JavaScript — inclusa la creazione dei tuoi semplici oggetti. Dovresti anche apprezzare che gli oggetti sono molto utili come strutture per memorizzare dati e funzionalità correlati — se provi a tenere traccia di tutte le proprietà e dei metodi nel nostro oggetto `person` come variabili e funzioni separate, sarebbe inefficiente e frustrante, e rischieremmo di entrare in conflitto con altre variabili e funzioni che hanno lo stesso nome. Gli oggetti ci permettono di mantenere le informazioni al sicuro nella loro confezione, fuori dai problemi.

Nel prossimo articolo esamineremo il **DOM scripting**, che sblocca una grande quantità di funzionalità fondamentali delle API del browser.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Image_gallery","Learn_web_development/Core/Scripting/DOM_scripting", "Learn_web_development/Core/Scripting")}}
