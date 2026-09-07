---
title: "Metti alla prova le tue competenze: le condizionali"
short-title: "Test: le condizionali"
slug: Learn_web_development/Core/Scripting/Test_your_skills/Conditionals
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Conditionals", "Learn_web_development/Core/Scripting/Loops", "Learn_web_development/Core/Scripting")}}

Lo scopo di questo test delle competenze è aiutare a valutare se è stato compreso l'articolo [Prendere decisioni nel codice — le condizionali](/it/docs/Learn_web_development/Core/Scripting/Conditionals).

> [!NOTE]
> Per ricevere aiuto, leggere la guida sull'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Condizionali 1

In questa attività vengono fornite due variabili:

- `season` — contiene una stringa che indica qual è la stagione corrente.
- `response` — inizia non inizializzata, ma viene usata in seguito per memorizzare una risposta che verrà stampata nel pannello di output.

Per completare l'attività:

1. Creare una condizionale che controlli se `season` contiene la stringa "summer" e, in caso affermativo, assegni a `response` una stringa che fornisca all'utente un messaggio appropriato sulla stagione. In caso contrario, deve assegnare a `response` una stringa generica che informi l'utente che non si conosce quale sia la stagione.
2. Aggiungere un'altra condizionale che controlli se `season` contiene la stringa "winter" e assegni nuovamente una stringa appropriata a `response`.

<!-- Codice condiviso tra gli esempi -->

```html hidden live-sample___conditionals-1 live-sample___conditionals-2 live-sample___conditionals-3 live-sample___conditionals-1-finish live-sample___conditionals-2-finish live-sample___conditionals-3-finish
<section></section>
```

```css hidden live-sample___conditionals-1 live-sample___conditionals-2 live-sample___conditionals-3 live-sample___conditionals-1-finish live-sample___conditionals-2-finish live-sample___conditionals-3-finish
* {
  box-sizing: border-box;
}

p {
  color: purple;
  margin: 0.5em 0;
}
```

<!-- Codice specifico dell'esempio -->

Il punto di partenza dell'attività è il seguente (non viene ancora visualizzato nulla):

{{ EmbedLiveSample("conditionals-1", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___conditionals-1
let season = "summer";
let response;

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = response;
section.appendChild(para1);
```

Lo stato iniziale dell'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("conditionals-1-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile al seguente:

```js
let season = "summer";
let response;

if (season === "summer") {
  response = "It's probably nice and warm where you are; enjoy the sun!";
} else if (season === "winter") {
  response = "I hope you are not too cold. Put some warm clothes on!";
} else {
  response =
    "I don't know what the season is where you are. Hope you are well.";
}

// Don't edit the code below here!
// ...
```

```js hidden live-sample___conditionals-1-finish
let season = "summer";
let response;

if (season === "summer") {
  response = "It's probably nice and warm where you are; enjoy the sun!";
} else if (season === "winter") {
  response = "I hope you are not too cold. Put some warm clothes on!";
} else {
  response =
    "I don't know what the season is where you are. Hope you are well.";
}

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = response;
section.appendChild(para1);
```

</details>

## Condizionali 2

Per questa attività vengono fornite tre variabili:

- `machineActive`: contiene un indicatore che segnala se la segreteria telefonica è attivata o meno (`true`/`false`).
- `score`: contiene il punteggio in un gioco immaginario. Questo punteggio viene inserito nella segreteria telefonica, che fornisce una risposta per indicare quanto è stata buona la prestazione.
- `response`: inizia non inizializzata, ma viene usata in seguito per memorizzare una risposta che verrà stampata nel pannello di output.

Per completare l'attività:

1. Creare una struttura `if...else` che controlli se la macchina è attivata e inserisca un messaggio nella variabile `response` se non lo è, comunicando all'utente di attivarla.
2. All'interno del primo `if...else`, annidare un altro `if...else` che inserisca messaggi appropriati nella variabile `response` a seconda del valore di `score`, se la macchina è attivata. I diversi test condizionali (e le risposte risultanti) sono i seguenti:
   - Punteggio inferiore a 0 o superiore a 100 — "This is not possible, an error has occurred."
   - Punteggio da 0 a 19 — "That was a terrible score — total fail!"
   - Punteggio da 20 a 39 — "You know some things, but it's a pretty bad score. Needs improvement."
   - Punteggio da 40 a 69 — "You did a passable job, not bad!"
   - Punteggio da 70 a 89 — "That's a great score, you really know your stuff."
   - Punteggio da 90 a 100 — "What an amazing score! Did you cheat? Are you for real?"

Dopo aver inserito il codice, provare a modificare `machineActive` in `true` e `score` con alcuni valori diversi per verificare che funzioni.
Tenere presente che, nell'ambito di questo esercizio, la stringa `Your score is __` rimarrà sullo schermo indipendentemente dal valore della variabile `machineActive`.

Il punto di partenza dell'attività è il seguente:

{{ EmbedLiveSample("conditionals-2", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___conditionals-2
let response;
let score = 75;
let machineActive = false;

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
const para2 = document.createElement("p");
para1.textContent = `Your score is ${score}`;
para2.textContent = response;
section.appendChild(para1);
section.appendChild(para2);
```

Lo stato iniziale dell'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("conditionals-2-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile al seguente:

```js
let response;
let score = 75;
let machineActive = false;

if (machineActive) {
  if (score < 0 || score > 100) {
    response = "This is not possible, an error has occurred.";
  } else if (score >= 0 && score < 20) {
    response = "That was a terrible score — total fail!";
  } else if (score >= 20 && score < 40) {
    response =
      "You know some things, but it's a pretty bad score. Needs improvement.";
  } else if (score >= 40 && score < 70) {
    response = "You did a passable job, not bad!";
  } else if (score >= 70 && score < 90) {
    response = "That's a great score, you really know your stuff.";
  } else if (score >= 90 && score <= 100) {
    response = "What an amazing score! Did you cheat? Are you for real?";
  }
} else {
  response = "The machine is turned off. Turn it on to process your score.";
}

// Don't edit the code below here!
// ...
```

```js hidden live-sample___conditionals-2-finish
let response;
let score = 75;
let machineActive = false;

if (machineActive) {
  if (score < 0 || score > 100) {
    response = "This is not possible, an error has occurred.";
  } else if (score >= 0 && score < 20) {
    response = "That was a terrible score — total fail!";
  } else if (score >= 20 && score < 40) {
    response =
      "You know some things, but it's a pretty bad score. Needs improvement.";
  } else if (score >= 40 && score < 70) {
    response = "You did a passable job, not bad!";
  } else if (score >= 70 && score < 90) {
    response = "That's a great score, you really know your stuff.";
  } else if (score >= 90 && score <= 100) {
    response = "What an amazing score! Did you cheat? Are you for real?";
  }
} else {
  response = "The machine is turned off. Turn it on to process your score.";
}

const section = document.querySelector("section");
const para1 = document.createElement("p");
const para2 = document.createElement("p");
para1.textContent = `Your score is ${score}`;
para2.textContent = response;
section.appendChild(para1);
section.appendChild(para2);
```

</details>

## Condizionali 3

Per l'attività finale vengono fornite quattro variabili:

- `machineActive`: contiene un indicatore che segnala se la macchina di accesso è attivata o meno (`true`/`false`).
- `pwd`: contiene la password di accesso dell'utente.
- `machineResult`: inizia non inizializzata, ma viene usata in seguito per memorizzare una risposta che verrà stampata nel pannello di output e che informa l'utente se la macchina è attivata.
- `pwdResult`: inizia non inizializzata, ma viene usata in seguito per memorizzare una risposta che verrà stampata nel pannello di output e che informa l'utente se il tentativo di accesso ha avuto successo.

Per completare l'attività:

1. Creare una struttura `if...else` che controlli se la macchina è attivata e inserisca un messaggio nella variabile `machineResult` per informare l'utente se è attivata o disattivata.
2. Se la macchina è attivata, deve essere eseguita anche una seconda condizionale che controlli se `pwd` è uguale a `cheese`. In caso affermativo, deve assegnare a `pwdResult` una stringa che informi l'utente che ha effettuato l'accesso con successo. In caso contrario, deve assegnare a `pwdResult` una stringa diversa che informi l'utente che il tentativo di accesso non ha avuto successo. L'operazione deve essere eseguita in un'unica riga, usando qualcosa che non sia una struttura `if...else`.

Il punto di partenza dell'attività è il seguente (non viene ancora visualizzato nulla):

{{ EmbedLiveSample("conditionals-3", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___conditionals-3
let machineActive = true;
let pwd = "cheese";

let machineResult;
let pwdResult;

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
const para2 = document.createElement("p");
para1.textContent = machineResult;
para2.textContent = pwdResult;
section.appendChild(para1);
section.appendChild(para2);
```

L'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("conditionals-3-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile al seguente:

```js
let machineActive = true;
let pwd = "cheese";

let machineResult;
let pwdResult;

if (machineActive) {
  machineResult = "Machine is active. Trying login.";
  pwdResult =
    pwd === "cheese"
      ? "Login successful."
      : "Password incorrect; login failed.";
} else {
  machineResult = "Machine is inactive. Activate and try logging in again.";
}

// Don't edit the code below here!
// ...
```

```js hidden live-sample___conditionals-3-finish
let machineActive = true;
let pwd = "cheese";

let machineResult;
let pwdResult;

if (machineActive) {
  machineResult = "Machine is active. Trying login.";
  pwdResult =
    pwd === "cheese"
      ? "Login successful."
      : "Password incorrect; login failed.";
} else {
  machineResult = "Machine is inactive. Activate and try logging in again.";
}

const section = document.querySelector("section");
const para1 = document.createElement("p");
const para2 = document.createElement("p");
para1.textContent = machineResult;
para2.textContent = pwdResult;
section.appendChild(para1);
section.appendChild(para2);
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Conditionals", "Learn_web_development/Core/Scripting/Loops", "Learn_web_development/Core/Scripting")}}
