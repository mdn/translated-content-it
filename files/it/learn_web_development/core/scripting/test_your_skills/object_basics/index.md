---
title: "Metti alla prova le tue abilità: Nozioni di base sugli oggetti"
short-title: "Test: Oggetti"
slug: Learn_web_development/Core/Scripting/Test_your_skills/Object_basics
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Object_basics","Learn_web_development/Core/Scripting/DOM_scripting", "Learn_web_development/Core/Scripting")}}

L'obiettivo di questo test di abilità è aiutare a valutare se sono stati compresi i concetti dell'articolo sulle [nozioni di base sugli oggetti JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics).

> [!NOTE]
> Per ottenere aiuto, leggere la guida all'uso [Metti alla prova le tue abilità](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Nozioni di base sugli oggetti 1

In questa attività viene fornito un oggetto letterale e viene richiesto di svolgere alcune operazioni su di esso.

Per completare l'attività:

1. Memorizzare il valore della proprietà `name` nella variabile `catName`, usando la notazione tra parentesi quadre.
2. Eseguire il metodo `greeting()` usando la notazione con il punto (registrerà il saluto nella console).
3. Aggiornare il valore della proprietà `color` in `black`.

<!-- Codice condiviso tra gli esempi -->

```html hidden live-sample___objects-1 live-sample___objects-2 live-sample___objects-3 live-sample___objects-4 live-sample___objects-1-finish live-sample___objects-2-finish live-sample___objects-4-finish
<section></section>
```

```css hidden live-sample___objects-1 live-sample___objects-2 live-sample___objects-3 live-sample___objects-4 live-sample___objects-1-finish live-sample___objects-2-finish live-sample___objects-4-finish
* {
  box-sizing: border-box;
}

p {
  color: purple;
  margin: 0.5em 0;
}
```

<!-- Codice specifico dell'esempio -->

Il punto di partenza dell'attività è il seguente (non viene ancora mostrato nulla):

{{ EmbedLiveSample("objects-1", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___objects-1
const cat = {
  name: "Bertie",
  breed: "Cymric",
  color: "white",
  greeting: function () {
    console.log("Meow!");
  },
};

// Don't edit the code above here!

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
let para1 = document.createElement("p");
let para2 = document.createElement("p");
para1.textContent = `The cat's name is ${catName}.`;
para2.textContent = `The cat's color is ${cat.color}.`;
section.appendChild(para1);
section.appendChild(para2);
```

L'output aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample("objects-1-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile a questo:

```js
// ...
// Don't edit the code above here!

const catName = cat["name"];
cat.greeting();
cat.color = "black";

// Don't edit the code below here!
// ...
```

```js hidden live-sample___objects-1-finish
const cat = {
  name: "Bertie",
  breed: "Cymric",
  color: "white",
  greeting: function () {
    console.log("Meow!");
  },
};

const catName = cat["name"];
cat.greeting();
cat.color = "black";

const section = document.querySelector("section");
let para1 = document.createElement("p");
let para2 = document.createElement("p");
para1.textContent = `The cat's name is ${catName}.`;
para2.textContent = `The cat's color is ${cat.color}.`;
section.appendChild(para1);
section.appendChild(para2);
```

</details>

## Nozioni di base sugli oggetti 2

Nella prossima attività, viene richiesto di provare a creare un oggetto letterale per rappresentare una delle band preferite.

Per completare l'attività:

1. Creare un oggetto letterale chiamato `band`, che contenga le seguenti proprietà:
   - `name`: una stringa che rappresenta il nome della band.
   - `nationality`: una stringa che rappresenta il paese di provenienza della band.
   - `genre`: il tipo di musica suonato dalla band.
   - `members`: un numero che rappresenta il numero di componenti della band.
   - `formed`: un numero che rappresenta l'anno di formazione della band.
   - `split`: un numero che rappresenta l'anno di scioglimento della band, oppure `false` se è ancora attiva.
   - `albums`: un array che rappresenta gli album pubblicati dalla band. Ogni elemento dell'array deve essere un oggetto contenente i seguenti membri:
     - `name`: una stringa che rappresenta il nome dell'album.
     - `released`: un numero che rappresenta l'anno di pubblicazione dell'album.
       > [!NOTE]
       > Includere almeno due album nell'array `albums`.
2. Scrivere una stringa nella variabile `bandInfo`, che conterrà una breve biografia con il nome, la nazionalità, gli anni di attività e lo stile della band, nonché il titolo e la data di pubblicazione del primo album.

Il punto di partenza dell'attività è il seguente (non viene ancora mostrato nulla):

{{ EmbedLiveSample("objects-2", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___objects-2
let bandInfo;

// Don't edit the code above here!

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
let para1 = document.createElement("p");
para1.textContent = bandInfo;
section.appendChild(para1);
```

L'output aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample("objects-2-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile a questo:

```js
// ...
// Don't edit the code above here!

const band = {
  name: "Black Sabbath",
  nationality: "British",
  genre: "heavy metal",
  members: 4,
  formed: 1968,
  split: 2025,
  albums: [
    {
      name: "Black Sabbath",
      released: 1970,
    },
    {
      name: "Paranoid",
      released: 1970,
    },
    {
      name: "Master of Reality",
      released: 1971,
    },
    {
      name: "Vol. 4",
      released: 1972,
    },
  ],
};

bandInfo = `The ${band.nationality} ${band.genre} band ${band.name} were active between ${band.formed} and ${band.split}. Their first album, ${band.albums[0].name}, was released in ${band.albums[0].released}.`;

// Don't edit the code below here!
// ...
```

```js hidden live-sample___objects-2-finish
let bandInfo;

const band = {
  name: "Black Sabbath",
  nationality: "British",
  genre: "heavy metal",
  members: 4,
  formed: 1968,
  split: 2025,
  albums: [
    {
      name: "Black Sabbath",
      released: 1970,
    },
    {
      name: "Paranoid",
      released: 1970,
    },
    {
      name: "Master of Reality",
      released: 1971,
    },
    {
      name: "Vol. 4",
      released: 1972,
    },
  ],
};

bandInfo = `The ${band.nationality} ${band.genre} band ${band.name} were active between ${band.formed} and ${band.split}. Their first album, ${band.albums[0].name}, was released in ${band.albums[0].released}.`;

const section = document.querySelector("section");
let para1 = document.createElement("p");
para1.textContent = bandInfo;
section.appendChild(para1);
```

</details>

## Nozioni di base sugli oggetti 3

In questa attività, viene richiesto di tornare all'oggetto letterale `cat` della sezione Nozioni di base sugli oggetti 1.

Per completare l'attività:

1. Riscrivere il metodo `greeting()` affinché registri `"Hello, said Bertie the Cymric."` nella console del browser, in modo che funzioni però con _qualsiasi_ oggetto `cat` con la stessa struttura, indipendentemente dal suo nome o dalla sua razza.
2. Scrivere un oggetto chiamato `cat2`, che abbia la stessa struttura e lo stesso metodo `greeting()`, ma `name`, `breed` e `color` diversi.
3. Chiamare entrambi i metodi `greeting()` per verificare che registrino nella console saluti appropriati.

Il punto di partenza dell'attività è il seguente (non viene mostrato nulla):

{{ EmbedLiveSample("objects-3", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___objects-3
const cat = {
  name: "Bertie",
  breed: "Cymric",
  color: "white",
  greeting: function () {
    console.log("Meow!");
  },
};

// Don't edit the code above here!

// Add your code here
```

Non viene fornito il contenuto completato per questa attività, poiché non stampa nulla nel DOM. Tutto l'output viene registrato nella console.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile a questo:

```js
// ...
// Don't edit the code above here!

const cat2 = {
  name: "Elfie",
  breed: "Aphrodite Giant",
  color: "ginger",
  greeting: function () {
    console.log(`Hello, said ${this.name} the ${this.breed}.`);
  },
};

cat.greeting();
cat2.greeting();
```

```js hidden
const cat = {
  name: "Bertie",
  breed: "Cymric",
  color: "white",
  greeting: function () {
    console.log("Meow!");
  },
};

const cat2 = {
  name: "Elfie",
  breed: "Aphrodite Giant",
  color: "ginger",
  greeting: function () {
    console.log(`Hello, said ${this.name} the ${this.breed}.`);
  },
};

cat.greeting();
cat2.greeting();
```

</details>

## Nozioni di base sugli oggetti 4

Nel codice scritto per l'attività 3, il metodo `greeting()` e le proprietà vengono definiti due volte, una per ciascun gatto. Questo non è ideale: in particolare, viola un principio della programmazione chiamato [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself), ovvero "Don't Repeat Yourself" ("non ripeterti").

In questa attività viene richiesto di migliorare il codice affinché i membri dell'oggetto vengano definiti una sola volta.

Per completare l'attività:

1. Creare una classe JavaScript che definisca istanze di gatto.
2. Usare la classe insieme alla parola chiave `new` per creare le istanze `cat` e `cat2`.

Il punto di partenza dell'attività è il seguente (non viene mostrato nulla):

{{ EmbedLiveSample("objects-4", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___objects-4
const cat = {
  name: "Bertie",
  breed: "Cymric",
  color: "white",
  greeting: function () {
    console.log(`Hello, said ${this.name} the ${this.breed}.`);
  },
};

const cat2 = {
  name: "Elfie",
  breed: "Aphrodite Giant",
  color: "ginger",
  greeting: function () {
    console.log(`Hello, said ${this.name} the ${this.breed}.`);
  },
};

// Don't edit the code below here!

cat.greeting();
cat2.greeting();
```

Non viene fornito il contenuto completato per questa attività, poiché non stampa nulla nel DOM. Tutto l'output viene registrato nella console.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile a questo:

```js
class Cat {
  constructor(name, breed, color) {
    this.name = name;
    this.breed = breed;
    this.color = color;
  }
  greeting() {
    console.log(`Hello, said ${this.name} the ${this.breed}.`);
  }
}

const cat = new Cat("Bertie", "Cymric", "white");
const cat2 = new Cat("Elfie", "Aphrodite Giant", "ginger");

// Don't edit the code below here!
// ...
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Object_basics","Learn_web_development/Core/Scripting/DOM_scripting", "Learn_web_development/Core/Scripting")}}
