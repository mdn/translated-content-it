---
title: Prototipi degli oggetti
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Object_prototypes
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}

I prototipi sono il meccanismo attraverso il quale gli oggetti JavaScript ereditano caratteristiche gli uni dagli altri. In questo articolo, spieghiamo cosa è un prototipo, come funzionano le catene di prototipi e come può essere impostato un prototipo per un oggetto.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi di JavaScript
        (in particolare
        <a href="/it/docs/Learn_web_development/Core/Scripting/Object_basics">le basi degli oggetti</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>La catena dei prototipi di JavaScript.</li>
          <li>Il concetto di proprietà shadowing (ombreggianti).</li>
          <li>Impostare i prototipi.</li>
          <li>I concetti di prototipi ed ereditarietà.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## La catena dei prototipi

Nella console del browser, prova a creare un oggetto letterale:

```js
const myObject = {
  city: "Madrid",
  greet() {
    console.log(`Greetings from ${this.city}`);
  },
};

myObject.greet(); // Greetings from Madrid
```

Questo è un oggetto con una proprietà di dati, `city`, e un metodo, `greet()`. Se scrivi il nome dell'oggetto _seguito da un punto_ nella console, come `myObject.`, la console farà apparire un elenco di tutte le proprietà disponibili per questo oggetto. Vedrai che oltre a `city` e `greet`, ci sono molte altre proprietà!

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

Prova ad accedere a una di esse:

```js
myObject.toString(); // "[object Object]"
```

Funziona (anche se non è ovvio cosa faccia `toString()`).

Cosa sono queste proprietà aggiuntive, e da dove provengono?

Ogni oggetto in JavaScript ha una proprietà incorporata, chiamata **prototipo**. Il prototipo è esso stesso un oggetto, quindi avrà il suo prototipo, creando quella che viene chiamata una **catena di prototipi**. La catena termina quando raggiungiamo un prototipo che ha `null` come suo prototipo.

> [!NOTE]
> La proprietà di un oggetto che punta al suo prototipo **non** è chiamata `prototype`. Il suo nome non è standard, ma in pratica tutti i browser utilizzano [`__proto__`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/proto). Il modo standard per accedere al prototipo di un oggetto è il metodo {{jsxref("Object/getPrototypeOf", "Object.getPrototypeOf()")}}.

Quando provi ad accedere a una proprietà di un oggetto: se la proprietà non può essere trovata nell'oggetto stesso, il prototipo viene cercato per la proprietà. Se la proprietà ancora non può essere trovata, allora il prototipo del prototipo viene cercato, e così via fino a quando la proprietà non viene trovata, o la fine della catena viene raggiunta, nel qual caso viene restituito `undefined`.

Quindi, quando chiamiamo `myObject.toString()`, il browser:

- cerca `toString` in `myObject`
- non riesce a trovarlo lì, quindi lo cerca nel prototipo di `myObject`
- lo trova lì e lo chiama.

Qual è il prototipo di `myObject`? Per scoprirlo, possiamo usare la funzione `Object.getPrototypeOf()`:

```js
Object.getPrototypeOf(myObject); // Object { }
```

Questo è un oggetto chiamato `Object.prototype`, ed è il prototipo più basilare, che tutti gli oggetti hanno per impostazione predefinita. Il prototipo di `Object.prototype` è `null`, quindi è alla fine della catena dei prototipi:

![Catena dei prototipi per myObject](myobject-prototype-chain.svg)

Il prototipo di un oggetto non è sempre `Object.prototype`. Prova questo:

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

Questo codice crea un oggetto `Date`, quindi cammina su per la catena dei prototipi, registrando i prototipi. Ci mostra che il prototipo di `myDate` è un oggetto `Date.prototype`, e il prototipo di _quello_ è `Object.prototype`.

![Catena dei prototipi per myDate](mydate-prototype-chain.svg)

Infatti, quando chiami metodi familiari, come `myDate2.getTime()`, stai chiamando un metodo definito su `Date.prototype`.

## Proprietà shadowing

Cosa succede se definisci una proprietà in un oggetto, quando una proprietà con lo stesso nome è definita nel prototipo dell'oggetto? Vediamo:

```js
const myDate = new Date(1995, 11, 17);

console.log(myDate.getTime()); // 819129600000

myDate.getTime = function () {
  console.log("something else!");
};

myDate.getTime(); // 'something else!'
```

Questo dovrebbe essere prevedibile, data la descrizione della catena dei prototipi. Quando chiamiamo `getTime()` il browser cerca prima in `myDate` una proprietà con quel nome, e controlla il prototipo solo se `myDate` non la definisce. Quindi quando aggiungiamo `getTime()` a `myDate`, viene chiamata la versione in `myDate`.

Questo si chiama "ombreggiare" la proprietà.

## Impostare un prototipo

Esistono vari modi per impostare il prototipo di un oggetto in JavaScript, e qui ne descriveremo due: `Object.create()` e i costruttori.

### Usare Object.create

Il metodo `Object.create()` crea un nuovo oggetto e ti permette di specificare un oggetto che verrà utilizzato come prototipo del nuovo oggetto.

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

Qui creiamo un oggetto `personPrototype`, che ha un metodo `greet()`. Usiamo poi `Object.create()` per creare un nuovo oggetto con `personPrototype` come suo prototipo. Ora possiamo chiamare `greet()` sul nuovo oggetto, e il prototipo fornisce la sua implementazione.

### Usare un costruttore

In JavaScript, tutte le funzioni hanno una proprietà chiamata `prototype`. Quando chiami una funzione come costruttore, questa proprietà viene impostata come prototipo del nuovo oggetto costruito (per convenzione, nella proprietà chiamata `__proto__`).

Quindi se impostiamo il `prototype` di un costruttore, possiamo assicurare che tutti gli oggetti creati con quel costruttore vengano assegnati a quel prototipo:

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

Mettiamo quindi i metodi definiti in `personPrototype` nella proprietà `prototype` della funzione `Person` usando [`Object.assign`](/it/docs/Web/JavaScript/Reference/Global_Objects/Object/assign).

Dopo questo codice, gli oggetti creati usando `Person()` avranno `Person.prototype` come loro prototipo, che contiene automaticamente il metodo `greet`.

```js
const reuben = new Person("Reuben");
reuben.greet(); // hello, my name is Reuben!
```

Questo spiega anche perché abbiamo detto precedentemente che il prototipo di `myDate` è chiamato `Date.prototype`: è la proprietà `prototype` del costruttore `Date`.

### Proprietà proprie

Gli oggetti che creiamo utilizzando il costruttore `Person` sopra hanno due proprietà:

- una proprietà `name`, che è impostata nel costruttore, quindi appare direttamente sugli oggetti `Person`
- un metodo `greet()`, che è impostato nel prototipo.

È comune vedere questo schema, in cui i metodi sono definiti sul prototipo, ma le proprietà di dati sono definite nel costruttore. Questo perché di solito i metodi sono gli stessi per ogni oggetto che creiamo, mentre spesso vogliamo che ogni oggetto abbia il proprio valore per le sue proprietà di dati (proprio come qui dove ogni persona ha un nome diverso).

Le proprietà che sono definite direttamente nell'oggetto, come `name` qui, sono chiamate **proprietà proprie**, e puoi controllare se una proprietà è una proprietà propria usando il metodo statico {{jsxref("Object/hasOwn", "Object.hasOwn()")}}:

```js
const irma = new Person("Irma");

console.log(Object.hasOwn(irma, "name")); // true
console.log(Object.hasOwn(irma, "greet")); // false
```

> [!NOTE]
> Puoi anche usare il metodo non statico {{jsxref("Object/hasOwnProperty", "Object.hasOwnProperty()")}} qui, ma raccomandiamo di usare `Object.hasOwn()` se possibile.

## Prototipi ed ereditarietà

I prototipi sono una caratteristica potente e molto flessibile di JavaScript, rendendo possibile il riutilizzo del codice e la combinazione di oggetti.

In particolare, supportano una versione di **ereditarietà**. L'ereditarietà è una caratteristica dei linguaggi di programmazione orientati agli oggetti che consente ai programmatori di esprimere l'idea che alcuni oggetti in un sistema sono versioni più specializzate di altri oggetti.

Per esempio, se stiamo modellando una scuola, potremmo avere _professori_ e _studenti_: sono entrambi _persone_, quindi hanno alcune caratteristiche in comune (per esempio, hanno entrambi nomi), ma ciascuno potrebbe aggiungere caratteristiche extra (per esempio, i professori hanno una materia che insegnano), o potrebbe implementare la stessa caratteristica in modi diversi. In un sistema OOP potremmo dire che professori e studenti entrambi **ereditano da** persone.

Puoi capire come in JavaScript, se gli oggetti `Professor` e `Student` possono avere prototipi `Person`, allora possono ereditare le proprietà comuni, mentre aggiungono e ridefiniscono quelle proprietà che devono differire.

Nel prossimo articolo discuteremo dell'ereditarietà insieme alle altre caratteristiche principali dei linguaggi di programmazione orientati agli oggetti e vedremo come JavaScript le supporta.

## Sommario

Questo articolo ha trattato i prototipi degli oggetti JavaScript, inclusi come le catene di oggetti prototipo consentono agli oggetti di ereditare caratteristiche gli uni dagli altri, la proprietà del prototipo e come può essere utilizzata per aggiungere metodi ai costruttori e altri argomenti correlati.

Nel prossimo articolo esamineremo i concetti alla base della programmazione orientata agli oggetti.

{{NextMenu("Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming", "Learn_web_development/Extensions/Advanced_JavaScript_objects")}}
