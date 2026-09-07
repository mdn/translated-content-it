---
title: "Metti alla prova le tue competenze: eventi"
short-title: "Test: eventi"
slug: Learn_web_development/Core/Scripting/Test_your_skills/Events
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Event_bubbling","Learn_web_development/Core/Scripting/Object_basics", "Learn_web_development/Core/Scripting")}}

Lo scopo di questo test di competenze è aiutare a valutare se è stato compreso l'articolo [Introduzione agli eventi](/it/docs/Learn_web_development/Core/Scripting/Events).

> [!NOTE]
> Per ottenere aiuto, leggere la guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Manipolazione del DOM: considerata utile

Alcune delle domande seguenti richiedono di scrivere codice di manipolazione del {{Glossary("DOM", "DOM")}} per completarle, ad esempio creando nuovi elementi HTML, impostando i relativi contenuti testuali affinché siano uguali a specifici valori stringa e annidandoli all'interno di elementi esistenti nella pagina, il tutto tramite JavaScript.

Non abbiamo ancora insegnato esplicitamente questo argomento nel corso, ma sono stati mostrati alcuni esempi che lo utilizzano e vorremmo che venisse svolta una ricerca sulle API DOM necessarie per rispondere con successo alle domande. Un buon punto di partenza è il tutorial [Introduzione allo scripting DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

## Eventi 1

La prima attività relativa agli eventi coinvolge un {{htmlelement("button")}} che, quando viene selezionato, aggiorna la propria etichetta di testo. L'HTML non deve essere modificato, solo il JavaScript.

Per completare l'attività, creare un event listener che faccia cambiare il testo all'interno del pulsante (`btn`) quando viene selezionato e che lo riporti allo stato precedente quando viene selezionato di nuovo.

Il punto di partenza dell'attività è il seguente:

{{ EmbedLiveSample("events-1", "100%", 80) }}

Ecco il codice sottostante per questo punto di partenza:

```css hidden live-sample___events-1 live-sample___events-1-finish
p {
  color: purple;
  margin: 0.5em 0;
}

* {
  box-sizing: border-box;
}

button {
  display: block;
  margin: 20px 0 20px 20px;
}

canvas {
  border: 1px solid black;
}
```

```html hidden live-sample___events-1 live-sample___events-1-finish
<button class="off">Machine is off</button>
```

```js live-sample___events-1
const btn = document.querySelector("button");

// Add your code here
```

L'esempio aggiornato dovrebbe comportarsi in questo modo (provare a premere il pulsante):

{{ EmbedLiveSample("events-1-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile al seguente:

```js live-sample___events-1-finish
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  if (btn.className === "on") {
    btn.textContent = "Machine is off";
    btn.className = "off";
  } else {
    btn.textContent = "Machine is on";
    btn.className = "on";
  }
});
```

</details>

## Eventi 2

Ora verranno esaminati gli eventi della tastiera.

Per completare questa attività, creare un event listener che sposti il cerchio nell'area canvas fornita quando vengono premuti i tasti WASD sulla tastiera. Il cerchio viene disegnato con la funzione `drawCircle()`, che accetta i seguenti parametri come input:

- `x` — la coordinata x del cerchio.
- `y` — la coordinata y del cerchio.
- `size` — il raggio del cerchio.

> [!WARNING]
> Durante il test del codice, sarà necessario mettere il focus sul canvas prima di provare i comandi della tastiera (ad esempio, facendo clic su di esso o raggiungendolo con il tasto Tab). Altrimenti non funzioneranno.

Il punto di partenza dell'attività è il seguente:

{{ EmbedLiveSample("events-2", "100%", 350) }}

Ecco il codice sottostante per questo punto di partenza:

```html hidden live-sample___events-2 live-sample___events-2-finish
<canvas width="480" height="320" tabindex="0"> </canvas>
```

```css hidden live-sample___events-2 live-sample___events-2-finish
* {
  box-sizing: border-box;
}

canvas {
  border: 1px solid black;
}
```

```js live-sample___events-2
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

function drawCircle(x, y, size) {
  ctx.fillStyle = "white";
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  ctx.beginPath();
  ctx.fillStyle = "black";
  ctx.arc(x, y, size, 0, 2 * Math.PI);
  ctx.fill();
}

let x = 50;
let y = 50;
const size = 30;

drawCircle(x, y, size);
// Don't edit the code above here!

// Add your code here
```

L'esempio aggiornato dovrebbe comportarsi in questo modo (farvi clic sopra e poi provare i controlli della tastiera):

{{ EmbedLiveSample("events-2-finish", "100%", 350) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile al seguente:

```js
// ...
// Don't edit the code above here!

window.addEventListener("keydown", (e) => {
  switch (e.key) {
    case "a":
      x -= 5;
      break;
    case "d":
      x += 5;
      break;
    case "w":
      y -= 5;
      break;
    case "s":
      y += 5;
      break;
  }

  drawCircle(x, y, size);
});
```

```js hidden live-sample___events-2-finish
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

function drawCircle(x, y, size) {
  ctx.fillStyle = "white";
  ctx.fillRect(0, 0, canvas.width, canvas.height);

  ctx.beginPath();
  ctx.fillStyle = "black";
  ctx.arc(x, y, size, 0, 2 * Math.PI);
  ctx.fill();
}

let x = 50;
let y = 50;
const size = 30;

drawCircle(x, y, size);

window.addEventListener("keydown", (e) => {
  switch (e.key) {
    case "a":
      x -= 5;
      break;
    case "d":
      x += 5;
      break;
    case "w":
      y -= 5;
      break;
    case "s":
      y += 5;
      break;
  }

  drawCircle(x, y, size);
});
```

</details>

## Eventi 3

La successiva attività relativa agli eventi verifica la conoscenza dell'event bubbling. È necessario impostare un event listener sull'elemento padre dei `<button>` (`<div class="button-bar"> … </div>`) che, quando viene invocato facendo clic su uno qualsiasi dei pulsanti, imposti lo sfondo di `button-bar` al colore contenuto nell'attributo `data-color` del pulsante.

La soluzione deve essere realizzata senza iterare su tutti i pulsanti e assegnare a ciascuno il proprio event listener.

Il punto di partenza dell'attività è il seguente:

{{ EmbedLiveSample("events-3", "100%", 80) }}

Ecco il codice sottostante per questo punto di partenza:

```html hidden live-sample___events-3 live-sample___events-3-finish
<div class="button-bar">
  <button data-color="red">Red</button>
  <button data-color="yellow">Yellow</button>
  <button data-color="green">Green</button>
  <button data-color="purple">Purple</button>
</div>
```

```css hidden live-sample___events-3 live-sample___events-3-finish
* {
  box-sizing: border-box;
}

html,
body,
.button-bar {
  height: 100%;
}

.button-bar {
  display: flex;
  align-items: center;
  justify-content: space-around;
}

button {
  padding: 5px 10px;
}
```

```js live-sample___events-3
const buttonBar = document.querySelector(".button-bar");

// Add your code here
```

L'esempio aggiornato dovrebbe comportarsi in questo modo (provare a fare clic sui pulsanti):

{{ EmbedLiveSample("events-3-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile al seguente:

```js live-sample___events-3-finish
const buttonBar = document.querySelector(".button-bar");

function setColor(e) {
  buttonBar.style.backgroundColor = e.target.getAttribute("data-color");
}

buttonBar.addEventListener("click", setColor);
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Event_bubbling","Learn_web_development/Core/Scripting/Object_basics", "Learn_web_development/Core/Scripting")}}
