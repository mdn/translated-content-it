---
title: Come fare per implementare un'API basata su promise
short-title: Implementazione di API basate su promise
slug: Learn_web_development/Extensions/Async_JS/Implementing_a_promise-based_API
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Promises", "Learn_web_development/Extensions/Async_JS/Introducing_workers", "Learn_web_development/Extensions/Async_JS")}}

Nell'ultimo articolo abbiamo discusso come usare le API che restituiscono promise. In questo articolo guarderemo l'altro lato — come _implementare_ API che restituiscono promise. Questo è un compito molto meno comune rispetto all'utilizzo di API basate su promise, ma è comunque utile conoscerlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
         Una solida comprensione dei <a href="/it/docs/Learn_web_development/Core/Scripting">fondamenti di JavaScript</a> e dei concetti asincroni, come coperto nelle lezioni precedenti di questo modulo.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>Comprendere come implementare API basate su promise.</td>
    </tr>
  </tbody>
</table>

In generale, quando si implementa un'API basata su promise, si incapsulerà un'operazione asincrona, che potrebbe utilizzare eventi, semplici callback, o un modello di passaggio di messaggi. Si organizzerà un oggetto `Promise` per gestire correttamente il successo o il fallimento di quell'operazione.

## Implementazione di un'API alarm()

In questo esempio implementeremo un'API d'allarme basata su promise, chiamata `alarm()`. Prenderà come argomenti il nome della persona da svegliare e un ritardo in millisecondi da attendere prima di svegliare la persona. Dopo il ritardo, la funzione invierà un messaggio "Svegliati!", incluso il nome della persona che dobbiamo svegliare.

### Incapsulare setTimeout()

Utilizzeremo l'API [`setTimeout()`](/it/docs/Web/API/Window/setTimeout) per implementare la nostra funzione `alarm()`. L'API `setTimeout()` prende come argomenti una funzione di callback e un ritardo, espresso in millisecondi. Quando viene chiamato `setTimeout()`, avvia un timer impostato sul ritardo dato, e quando il tempo scade, chiama la funzione data.

Nell'esempio seguente, chiamiamo `setTimeout()` con una funzione di callback e un ritardo di 1000 millisecondi:

```html
<button id="set-alarm">Set alarm</button>
<div id="output"></div>
```

```css hidden
div {
  margin: 0.5rem 0;
}
```

```js
const output = document.querySelector("#output");
const button = document.querySelector("#set-alarm");

function setAlarm() {
  setTimeout(() => {
    output.textContent = "Wake up!";
  }, 1000);
}

button.addEventListener("click", setAlarm);
```

{{EmbedLiveSample("Incapsulare setTimeout()", 600, 100)}}

### Il costruttore Promise()

La nostra funzione `alarm()` restituirà un `Promise` che viene risolta quando il timer scade. Passerà un messaggio "Svegliati!" al gestore `then()`, e rifiuterà la promise se il chiamante fornisce un valore di ritardo negativo.

Il componente chiave qui è il costruttore {{jsxref("Promise/Promise", "Promise()")}}. Il costruttore `Promise()` prende come argomento una singola funzione. Chiameremo questa funzione l'`executor`. Quando si crea una nuova promise si fornisce l'implementazione dell'executor.

Questa funzione executor stessa prende due argomenti, che sono anch'essi funzioni, e che sono convenzionalmente chiamati `resolve` e `reject`. Nell'implementazione dell'executor, si chiama la funzione asincrona sottostante. Se la funzione asincrona ha successo, si chiama `resolve`, e se fallisce, si chiama `reject`. Se la funzione executor genera un errore, `reject` viene chiamato automaticamente. Puoi passare un singolo parametro di qualsiasi tipo a `resolve` e `reject`.

Quindi possiamo implementare `alarm()` in questo modo:

```js
function alarm(person, delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) {
      reject(new Error("Alarm delay must not be negative"));
      return;
    }
    setTimeout(() => {
      resolve(`Wake up, ${person}!`);
    }, delay);
  });
}
```

Questa funzione crea e restituisce una nuova `Promise`. All'interno dell'executor per la promise, noi:

- verifichiamo che `delay` non sia negativo, e chiamiamo `reject`, passando un errore personalizzato se lo è.

- chiamiamo `setTimeout()`, passando un callback e `delay`. Il callback verrà chiamato quando il timer scade, e nel callback chiamiamo `resolve`, passando il nostro messaggio "Svegliati!".

## Uso dell'API alarm()

Questa parte dovrebbe essere abbastanza familiare dall'ultimo articolo. Possiamo chiamare `alarm()`, e sulla promise restituita chiamare `then()` e `catch()` per impostare i gestori per la risoluzione e il rifiuto della promise.

```html hidden
<div>
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" size="4" value="Matilda" />
</div>

<div>
  <label for="delay">Delay:</label>
  <input type="text" id="delay" name="delay" size="4" value="1000" />
</div>

<button id="set-alarm">Set alarm</button>
<div id="output"></div>
```

```css hidden
button {
  display: block;
}

div,
button {
  margin: 0.5rem 0;
}
```

```js
const name = document.querySelector("#name");
const delay = document.querySelector("#delay");
const button = document.querySelector("#set-alarm");
const output = document.querySelector("#output");

function alarm(person, delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) {
      reject(new Error("Alarm delay must not be negative"));
      return;
    }
    setTimeout(() => {
      resolve(`Wake up, ${person}!`);
    }, delay);
  });
}

button.addEventListener("click", () => {
  alarm(name.value, delay.value)
    .then((message) => (output.textContent = message))
    .catch((error) => (output.textContent = `Couldn't set alarm: ${error}`));
});
```

{{EmbedLiveSample("Uso dell'API alarm()", 600, 160)}}

Prova a impostare valori diversi per "Nome" e "Delay". Prova a impostare un valore negativo per "Delay".

## Uso di async e await con l'API alarm()

Poiché `alarm()` restituisce un `Promise`, possiamo fare con essa tutto ciò che potremmo fare con qualsiasi altra promise: concatenamento delle promise, `Promise.all()`, e `async` / `await`:

```html hidden
<div>
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" size="4" value="Matilda" />
</div>

<div>
  <label for="delay">Delay:</label>
  <input type="text" id="delay" name="delay" size="4" value="1000" />
</div>

<button id="set-alarm">Set alarm</button>
<div id="output"></div>
```

```css hidden
button {
  display: block;
}

div,
button {
  margin: 0.5rem 0;
}
```

```js
const name = document.querySelector("#name");
const delay = document.querySelector("#delay");
const button = document.querySelector("#set-alarm");
const output = document.querySelector("#output");

function alarm(person, delay) {
  return new Promise((resolve, reject) => {
    if (delay < 0) {
      reject(new Error("Alarm delay must not be negative"));
      return;
    }
    setTimeout(() => {
      resolve(`Wake up, ${person}!`);
    }, delay);
  });
}

button.addEventListener("click", async () => {
  try {
    const message = await alarm(name.value, delay.value);
    output.textContent = message;
  } catch (error) {
    output.textContent = `Couldn't set alarm: ${error}`;
  }
});
```

{{EmbedLiveSample("Uso di async e await con l'API alarm()", 600, 160)}}

## Vedi anche

- [Costruttore `Promise()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise/Promise)
- [Uso delle promise](/it/docs/Web/JavaScript/Guide/Using_promises)

{{PreviousMenuNext("Learn_web_development/Extensions/Async_JS/Promises", "Learn_web_development/Extensions/Async_JS/Introducing_workers", "Learn_web_development/Extensions/Async_JS")}}
