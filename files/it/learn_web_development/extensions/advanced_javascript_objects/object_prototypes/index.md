---
title: Object prototypes
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

I prototipi sono il meccanismo attraverso il quale gli oggetti JavaScript ereditano caratteristiche l'uno dall'altro. In questo articolo, spieghiamo cos'è un prototipo, come funzionano le catene di prototipi e come può essere impostato un prototipo per un oggetto.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (in particolare
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">Basi degli oggetti</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>La catena di prototipi in JavaScript.</li>
          <li>Il concetto di shadowing delle proprietà.</li>
          <li>Impostazione dei prototipi.</li>
          <li>I concetti di prototipi ed ereditarietà.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## La catena di prototipi

Nella console del browser, provi a creare un oggetto letterale:

```js
const myObject = {
  city: "Madrid",
  greet() {
    console.log(`Greetings from ${this.city}`);
  },
};

myObject.greet(); // Greetings from Madrid
```

Questo è un oggetto con una proprietà di dati, `city`, e un metodo, `greet()`. Se digita il nome dell'oggetto _seguito da un punto_ nella console, come `myObject.`, allora la console aprirà un elenco di tutte le proprietà disponibili per questo oggetto. Vedrà che, oltre a `city` e `greet`, ci sono molte altre proprietà!

```plain
__defineGetter__
__defineSetter__
__lookupGetter__
__lookupSetter__
__proto__
city
constructor
greet
hasOwnProperty
isPrototypeOf
propertyIsEnumerable
toLocaleString
toString
valueOf
```

Provi ad accedere a una di esse:

```js
myObject.toString(); // "[object Object]"
```

Funziona (anche se non è ovvio cosa faccia `toString()`).

Cosa sono queste proprietà extra, e da dove provengono?

Ogni oggetto in JavaScript ha una proprietà integrata, che si chiama **prototipo**. Il prototipo è esso stesso un oggetto, quindi il prototipo avrà il suo prototipo, creando quella che viene chiamata una **catena di prototipi**. La catena termina quando raggiungiamo un prototipo che ha `null` come suo prototipo.

> [!NOTE]
> La proprietà di un oggetto che punta al suo prototipo **non** si chiama `prototype`. Il suo nome non è standard, ma nella pratica tutti i browser utilizzano [`__proto__`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/proto). Il modo standard per accedere al prototipo di un oggetto è il metodo {{jsxref("Object/getPrototypeOf", "Object.getPrototypeOf()")}}.

Quando prova ad accedere a una proprietà di un oggetto: se la proprietà non può essere trovata nell'oggetto stesso, il prototipo viene cercato per la proprietà. Se la proprietà ancora non può essere trovata, allora viene cercato il prototipo del prototipo, e così via fino a quando la proprietà viene trovata, o si raggiunge la fine della catena, nel qual caso viene restituito `undefined`.

Quindi, quando chiama `myObject.toString()`, il browser:

- cerca `toString` in `myObject`
- non riesce a trovarlo lì, quindi cerca nell'oggetto prototipo di `myObject` per `toString`
- lo trova lì, e lo chiama.

Qual è il prototipo di `myObject`? Per scoprirlo, possiamo usare la funzione `Object.getPrototypeOf()`:

```js
Object.getPrototypeOf(myObject); // Object { }
```

Questo è un oggetto chiamato `Object.prototype`, ed è il prototipo più basilare, che tutti gli oggetti hanno per impostazione predefinita. Il prototipo di `Object.prototype` è `null`, quindi è alla fine della catena di prototipi:

![Catena di prototipi per myObject](myobject-prototype-chain.svg)

Il prototipo di un oggetto non è sempre `Object.prototype`. Provi questo:

```js
const myDate = new Date();
let object = myDate;

do {
  object = Object.getPrototypeOf(object);
  console.log(object);
} while (object);

// Date.prototype
// Object { }
// null
```

Questo codice crea un oggetto `Date`, poi percorre la catena dei prototipi, registrando i prototipi. Ci mostra che il prototipo di `myDate` è un oggetto `Date.prototype`, e il prototipo di _quello_ è `Object.prototype`.

![Catena di prototipi per myDate](mydate-prototype-chain.svg)

Infatti, quando chiama metodi conosciuti, come `myDate2.getTime()`,
sta chiamando un metodo definito su `Date.prototype`.

## Shadowing delle proprietà

Cosa succede se definisce una proprietà in un oggetto, quando una proprietà con lo stesso nome è definita nel prototipo dell'oggetto? Vediamo:

```js
const myDate = new Date(1995, 11, 17);

console.log(myDate.getTime()); // 819129600000

myDate.getTime = function () {
  console.log("something else!");
};

myDate.getTime(); // 'something else!'
```

Questo dovrebbe essere prevedibile, data la descrizione della catena dei prototipi. Quando chiamiamo `getTime()`, il browser cerca prima una proprietà con quel nome in `myDate`, e controlla il prototipo solo se `myDate` non la definisce. Quindi quando aggiungiamo `getTime()` a `myDate`, viene chiamata la versione in `myDate`.

Questo si chiama "shadowing" della proprietà.

## Impostazione di un prototipo

Esistono vari modi per impostare il prototipo di un oggetto in JavaScript, e qui ne descriveremo due: `Object.create()` e i costruttori.

### Utilizzando Object.create

Il metodo `Object.create()` crea un nuovo oggetto e permette di specificare un oggetto che sarà utilizzato come prototipo del nuovo oggetto.

Ecco un esempio:

```js
const personPrototype = {
  greet() {
    console.log("hello!");
  },
};

const carl = Object.create(personPrototype);
carl.greet(); // hello!
```

Qui creiamo un oggetto `personPrototype`, che ha un metodo `greet()`. Usciamo poi `Object.create()` per creare un nuovo oggetto con `personPrototype` come suo prototipo. Ora possiamo chiamare `greet()` sul nuovo oggetto, e il prototipo fornisce la sua implementazione.

### Usando un costruttore

In JavaScript, tutte le funzioni hanno una proprietà chiamata `prototype`. Quando chiama una funzione come costruttore, questa proprietà è impostata come il prototipo del nuovo oggetto costruito (per convenzione, nella proprietà chiamata `__proto__`).

Quindi, se impostiamo il `prototype` di un costruttore, possiamo garantire che tutti gli oggetti creati con quel costruttore ricevano quel prototipo:

```js
const personPrototype = {
  greet() {
    console.log(`hello, my name is ${this.name}!`);
  },
};

function Person(name) {
  this.name = name;
}

Object.assign(Person.prototype, personPrototype);
// or
// Person.prototype.greet = personPrototype.greet;
```

Qui creiamo:

- un oggetto `personPrototype`, che ha un metodo `greet()`
- una funzione costruttore `Person()` che inizializza il nome della persona da creare.

Poniamo poi i metodi definiti in `personPrototype` sulla proprietà `prototype` della funzione `Person` usando [`Object.assign`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/assign).

Dopo questo codice, gli oggetti creati usando `Person()` otterranno `Person.prototype` come loro prototipo, che contiene automaticamente il metodo `greet`.

```js
const reuben = new Person("Reuben");
reuben.greet(); // hello, my name is Reuben!
```

Questo spiega anche perché abbiamo detto prima che il prototipo di `myDate` si chiama `Date.prototype`: è la proprietà `prototype` del costruttore `Date`.

### Proprietà proprie

Gli oggetti che creiamo usando il costruttore `Person` sopra hanno due proprietà:

- una proprietà `name`, che è impostata nel costruttore, quindi appare direttamente su oggetti `Person`
- un metodo `greet()`, che è impostato nel prototipo.

È comune vedere questo schema, in cui i metodi sono definiti sul prototipo, ma le proprietà di dati sono definite nel costruttore. Questo perché i metodi sono solitamente gli stessi per ogni oggetto che creiamo, mentre spesso vogliamo che ogni oggetto abbia il suo valore per le proprietà di dati (proprio come qui dove ogni persona ha un nome diverso).

Le proprietà che sono definite direttamente nell'oggetto, come `name` qui, sono chiamate proprietà **proprie**, e può verificare se una proprietà è una proprietà propria usando il metodo statico {{jsxref("Object/hasOwn", "Object.hasOwn()")}}:

```js
const irma = new Person("Irma");

console.log(Object.hasOwn(irma, "name")); // true
console.log(Object.hasOwn(irma, "greet")); // false
```

> [!NOTE]
> Può anche utilizzare il metodo non-statico {{jsxref("Object/hasOwnProperty", "Object.hasOwnProperty()")}} qui, ma le consigliamo di usare `Object.hasOwn()` se possibile.

## Prototipi ed ereditarietà

I prototipi sono una caratteristica potente e molto flessibile di JavaScript, rendendo possibile riutilizzare il codice e combinare oggetti.

In particolare, supportano una versione di **ereditarietà**. L'ereditarietà è una caratteristica dei linguaggi di programmazione orientati agli oggetti che consente ai programmatori di esprimere l'idea che alcuni oggetti in un sistema siano versioni più specializzate di altri oggetti.

Ad esempio, se stiamo modellando una scuola, potremmo avere _professori_ e _studenti_: entrambi sono _persone_, quindi hanno alcune caratteristiche in comune (ad esempio, entrambi hanno nomi), ma ciascuno potrebbe aggiungere caratteristiche extra (ad esempio, i professori hanno una materia che insegnano), o potrebbe implementare la stessa caratteristica in modi diversi. In un sistema OPP potremmo dire che professori e studenti entrambi **ereditano da** persone.

Può vedere come in JavaScript, se gli oggetti `Professor` e `Student` possono avere prototipi `Person`, allora possono ereditare le proprietà comuni, mentre aggiungono e ridefiniscono quelle proprietà che devono differire.

Nel prossimo articolo discuteremo l'ereditarietà insieme alle altre caratteristiche principali dei linguaggi di programmazione orientati agli oggetti, e vedremo come JavaScript le supporti.

## Sommario

Questo articolo ha trattato i prototipi degli oggetti JavaScript, inclusi come le catene di prototipi degli oggetti permettano agli oggetti di ereditare caratteristiche l'uno dall'altro, la proprietà di prototipo e come può essere utilizzata per aggiungere metodi ai costruttori, e altri argomenti correlati.

Nel prossimo articolo esamineremo i concetti sottostanti alla programmazione orientata agli oggetti.

{{NextMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
