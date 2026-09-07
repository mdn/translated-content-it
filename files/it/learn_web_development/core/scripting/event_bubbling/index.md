---
title: Propagazione degli eventi
slug: Learn_web_development/Core/Scripting/Event_bubbling
l10n:
  sourceCommit: a73e5b9e881645835a254c4b3d07c48230010d29
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Events","Learn_web_development/Core/Scripting/Test_your_skills/Events", "Learn_web_development/Core/Scripting")}}

Abbiamo visto che una pagina web è composta da _elementi_ — titoli, paragrafi di testo, immagini, pulsanti e così via — e che è possibile ascoltare gli eventi che si verificano su questi elementi. Per esempio, si potrebbe aggiungere un listener a un pulsante, che verrà eseguito quando l'utente fa clic sul pulsante.

Abbiamo anche visto che questi elementi possono essere _annidati_ gli uni dentro gli altri: per esempio, un {{htmlelement("button")}} potrebbe essere inserito all'interno di un elemento {{htmlelement("div")}}. In questo caso, l'elemento `<div>` viene chiamato elemento _genitore_, mentre `<button>` elemento _figlio_.

In questo capitolo verrà esaminata la **propagazione degli eventi** (_event bubbling_): ciò che accade quando si aggiunge un event listener a un elemento genitore e l'utente fa clic sull'elemento figlio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Delega degli eventi, ottenuta tramite la propagazione degli eventi o la cattura degli eventi.</li>
          <li>Interruzione della delega degli eventi con <code>stopPropagation()</code>.</li>
          <li>Accesso ai target degli eventi dall'oggetto evento.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Introduzione alla propagazione degli eventi

Introduciamo e definiamo la propagazione degli eventi attraverso un esempio.

### Impostare un listener su un elemento genitore

Si consideri una pagina web come questa:

```html
<div id="container">
  <button>Click me!</button>
</div>
<pre id="output"></pre>
```

Qui il pulsante è all'interno di un altro elemento, un elemento {{HTMLElement("div")}}. Si dice che l'elemento `<div>` sia il **genitore** dell'elemento che contiene. Cosa accade se si aggiunge un gestore dell'evento click al genitore e poi si fa clic sul pulsante?

```js
const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

const container = document.querySelector("#container");
container.addEventListener("click", handleClick);
```

{{ EmbedLiveSample('Setting a listener on a parent element', '100%', 200, "", "") }}

Si vedrà che il genitore attiva un evento click quando l'utente fa clic sul pulsante:

```plain
You clicked on a DIV element
```

Questo è logico: il pulsante è all'interno del `<div>`, quindi fare clic sul pulsante implica anche fare clic sull'elemento che lo contiene.

### Esempio di propagazione

Cosa accade se si aggiungono event listener sia al pulsante _sia_ al genitore?

```html
<body>
  <div id="container">
    <button>Click me!</button>
  </div>
  <pre id="output"></pre>
</body>
```

Proviamo ad aggiungere gestori dell'evento click al pulsante, al suo genitore (il `<div>`) e all'elemento {{HTMLElement("body")}} che contiene entrambi:

```js
const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

const container = document.querySelector("#container");
const button = document.querySelector("button");

document.body.addEventListener("click", handleClick);
container.addEventListener("click", handleClick);
button.addEventListener("click", handleClick);
```

{{ EmbedLiveSample('Bubbling example', '100%', 200, "", "") }}

Si vedrà che tutti e tre gli elementi attivano un evento click quando l'utente fa clic sul pulsante:

```plain
You clicked on a BUTTON element
You clicked on a DIV element
You clicked on a BODY element
```

In questo caso:

- il click sul pulsante si attiva per primo;
- seguito dal click sul suo genitore (l'elemento `<div>`);
- seguito dal click sul genitore dell'elemento `<div>` (l'elemento `<body>`).

Questo comportamento viene descritto dicendo che l'evento **risale** dall'elemento più interno su cui è stato fatto clic.

Questo comportamento può essere utile, ma può anche causare problemi imprevisti. Nelle sezioni successive verrà analizzato un problema che provoca e trovata la soluzione.

### Esempio di lettore video

In questo esempio la pagina contiene un video, inizialmente nascosto, e un pulsante con l'etichetta "Display video". Si desidera la seguente interazione:

- Quando l'utente fa clic sul pulsante "Display video", mostrare il riquadro contenente il video, ma senza avviare ancora la riproduzione del video.
- Quando l'utente fa clic sul video, avviare la riproduzione del video.
- Quando l'utente fa clic in qualsiasi punto del riquadro al di fuori del video, nascondere il riquadro.

L'HTML è il seguente:

```html
<button>Display video</button>

<div class="hidden">
  <video>
    <source src="/shared-assets/videos/flower.webm" type="video/webm" />
    <p>
      Your browser doesn't support HTML video. Here is a
      <a href="rabbit320.mp4">link to the video</a> instead.
    </p>
  </video>
</div>
```

Include:

- un elemento `<button>`;
- un elemento `<div>` che inizialmente ha un attributo `class="hidden"`;
- un elemento `<video>` annidato all'interno dell'elemento `<div>`.

Viene utilizzato CSS per nascondere gli elementi con la classe `"hidden"` impostata.

```css hidden
div {
  width: 100%;
  height: 100%;
  background-color: #eeeeee;
}

.hidden {
  display: none;
}

div video {
  padding: 40px;
  display: block;
  width: 400px;
  margin: 40px auto;
}
```

Il JavaScript è il seguente:

```js
const btn = document.querySelector("button");
const box = document.querySelector("div");
const video = document.querySelector("video");

btn.addEventListener("click", () => box.classList.remove("hidden"));
video.addEventListener("click", () => video.play());
box.addEventListener("click", () => box.classList.add("hidden"));
```

Questo aggiunge tre event listener per `'click'`:

- uno sul `<button>`, che mostra il `<div>` contenente il `<video>`;
- uno sul `<video>`, che avvia la riproduzione del video;
- uno sul `<div>`, che nasconde il video.

Vediamo come funziona:

{{ EmbedLiveSample('Video_player_example', '100%', 500) }}

Facendo clic sul pulsante, dovrebbero essere visualizzati il riquadro e il video che contiene. Tuttavia, quando si fa clic sul video, il video inizia a essere riprodotto, ma il riquadro viene nuovamente nascosto.

Il video è all'interno del `<div>` — ne fa parte — quindi fare clic sul video esegue _entrambi_ i gestori di evento, causando questo comportamento.

### Risolvere il problema con `stopPropagation()`

Come visto nella sezione precedente, la propagazione degli eventi può talvolta creare problemi, ma esiste un modo per evitarlo.
L'oggetto [`Event`](/it/docs/Web/API/Event) dispone di una funzione chiamata [`stopPropagation()`](/it/docs/Web/API/Event/stopPropagation) che, quando chiamata all'interno di un gestore di evento, impedisce all'evento di propagarsi agli altri elementi.

È possibile risolvere il problema corrente modificando il JavaScript in questo modo:

```js
const btn = document.querySelector("button");
const box = document.querySelector("div");
const video = document.querySelector("video");

btn.addEventListener("click", () => box.classList.remove("hidden"));

video.addEventListener("click", (event) => {
  event.stopPropagation();
  video.play();
});

box.addEventListener("click", () => box.classList.add("hidden"));
```

Qui viene semplicemente chiamato `stopPropagation()` sull'oggetto evento nel gestore per l'evento `'click'` dell'elemento `<video>`. Questo impedirà all'evento di risalire fino al riquadro. Ora provare a fare clic sul pulsante e poi sul video:

{{EmbedLiveSample("Fixing the problem with stopPropagation()", '100%', 500)}}

```html hidden
<button>Display video</button>

<div class="hidden">
  <video>
    <source src="/shared-assets/videos/flower.webm" type="video/webm" />
    <p>
      Your browser doesn't support HTML video. Here is a
      <a href="rabbit320.mp4">link to the video</a> instead.
    </p>
  </video>
</div>
```

```css hidden
div {
  width: 100%;
  height: 100%;
  background-color: #eeeeee;
}

.hidden {
  display: none;
}

div video {
  padding: 40px;
  display: block;
  width: 400px;
  margin: 40px auto;
}
```

## Cattura degli eventi

Una forma alternativa di propagazione degli eventi è la _cattura degli eventi_ (_event capture_). È simile alla propagazione degli eventi, ma l'ordine è invertito: invece di attivarsi prima sull'elemento più interno target e poi su elementi via via meno annidati, l'evento si attiva prima sull'elemento _meno annidato_ e poi su elementi via via più annidati, fino al raggiungimento del target.

La cattura degli eventi è disabilitata per impostazione predefinita. Per abilitarla, è necessario passare l'opzione `capture` in `addEventListener()`.

Questo esempio è identico all'[esempio di propagazione](#esempio_di_propagazione) visto in precedenza, tranne per il fatto che è stata usata l'opzione `capture`:

```html
<body>
  <div id="container">
    <button>Click me!</button>
  </div>
  <pre id="output"></pre>
</body>
```

```js
const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

const container = document.querySelector("#container");
const button = document.querySelector("button");

document.body.addEventListener("click", handleClick, { capture: true });
container.addEventListener("click", handleClick, { capture: true });
button.addEventListener("click", handleClick);
```

{{ EmbedLiveSample('Event capture', '100%', 200, "", "") }}

In questo caso, l'ordine dei messaggi è invertito: il gestore di evento del `<body>` si attiva per primo, seguito dal gestore di evento del `<div>` e infine dal gestore di evento del `<button>`:

```plain
You clicked on a BODY element
You clicked on a DIV element
You clicked on a BUTTON element
```

Perché usare sia la cattura sia la propagazione? Ai vecchi tempi, quando i browser erano molto meno compatibili tra loro rispetto a oggi, Netscape usava solo la cattura degli eventi, mentre Internet Explorer usava solo la propagazione degli eventi. Quando il W3C decise di tentare di standardizzare il comportamento e raggiungere un consenso, adottò questo sistema che includeva entrambi, implementato dai browser moderni.

Per impostazione predefinita, quasi tutti i gestori di evento vengono registrati nella fase di propagazione, e nella maggior parte dei casi questo approccio è più sensato.

## Delega degli eventi

Nell'ultima sezione è stato esaminato un problema causato dalla propagazione degli eventi e come risolverlo. Tuttavia, la propagazione degli eventi non è solo fastidiosa: può essere molto utile. In particolare, consente la **delega degli eventi**. In questa pratica, quando si desidera eseguire del codice quando l'utente interagisce con uno qualsiasi di un gran numero di elementi figli, si imposta l'event listener sul loro genitore e si fa risalire al genitore gli eventi che si verificano su di essi, invece di dover impostare l'event listener su ogni figlio singolarmente.

Torniamo al nostro [primo esempio](/it/docs/Learn_web_development/Core/Scripting/Events#an_example_handling_a_click_event),
in cui veniva impostato il colore di sfondo dell'intera pagina quando l'utente faceva clic su un pulsante. Si supponga invece che la pagina sia divisa in 16 riquadri e che si desideri impostare ogni riquadro su un colore casuale quando l'utente fa clic su quel riquadro.

Ecco l'HTML:

```html
<div id="container">
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
  <div class="tile"></div>
</div>
```

È presente un po' di CSS per impostare le dimensioni e la posizione dei riquadri:

```css
#container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 100px;
}
```

Ora, in JavaScript, si potrebbe aggiungere un gestore dell'evento click per ogni riquadro. Tuttavia, un'opzione molto più semplice ed efficiente consiste nell'impostare il gestore dell'evento click sul genitore e fare affidamento sulla propagazione degli eventi per assicurarsi che il gestore venga eseguito quando l'utente fa clic su un riquadro:

```js
function random(number) {
  return Math.floor(Math.random() * number);
}

function bgChange() {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  return rndCol;
}

const container = document.querySelector("#container");

container.addEventListener("click", (event) => {
  event.target.style.backgroundColor = bgChange();
});
```

L'output è il seguente (provare a fare clic in vari punti):

{{ EmbedLiveSample('Event delegation', '100%', 430, "", "") }}

> [!NOTE]
> In questo esempio viene usato `event.target` per ottenere l'elemento che era il target dell'evento, ovvero l'elemento più interno. Per accedere all'elemento che ha gestito questo evento, in questo caso il contenitore, si potrebbe usare `event.currentTarget`.

> [!NOTE]
> Consultare [useful-eventtarget.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/useful-eventtarget.html) per il codice sorgente completo; è inoltre possibile vederlo [in esecuzione qui](https://mdn.github.io/learning-area/javascript/building-blocks/events/useful-eventtarget.html).

## `target` e `currentTarget`

Osservando attentamente gli esempi presentati in questa pagina, si noterà che vengono usate due proprietà diverse dell'oggetto evento per accedere all'elemento su cui è stato fatto clic. In [Impostare un listener su un elemento genitore](#impostare_un_listener_su_un_elemento_genitore) viene usato [`event.currentTarget`](/it/docs/Web/API/Event/currentTarget). Tuttavia, in [Delega degli eventi](#delega_degli_eventi) viene usato [`event.target`](/it/docs/Web/API/Event/target).

La differenza è che `target` fa riferimento all'elemento su cui l'evento è stato inizialmente attivato, mentre `currentTarget` fa riferimento all'elemento a cui è stato collegato questo gestore di evento.

Mentre `target` rimane invariato quando un evento risale, `currentTarget` sarà diverso per i gestori di evento collegati a elementi diversi nella gerarchia.

Questo può essere osservato adattando leggermente l'[esempio di propagazione](#esempio_di_propagazione) precedente. Viene usato lo stesso HTML di prima:

```html
<body>
  <div id="container">
    <button>Click me!</button>
  </div>
  <pre id="output"></pre>
</body>
```

Il JavaScript è quasi identico, tranne per il fatto che vengono registrati sia `target` sia `currentTarget`:

```js
const output = document.querySelector("#output");
function handleClick(e) {
  const logTarget = `Target: ${e.target.tagName}`;
  const logCurrentTarget = `Current target: ${e.currentTarget.tagName}`;
  output.textContent += `${logTarget}, ${logCurrentTarget}\n`;
}

const container = document.querySelector("#container");
const button = document.querySelector("button");

document.body.addEventListener("click", handleClick);
container.addEventListener("click", handleClick);
button.addEventListener("click", handleClick);
```

Si noti che quando si fa clic sul pulsante, `target` è sempre l'elemento pulsante, indipendentemente dal fatto che il gestore di evento sia collegato al pulsante stesso, al `<div>` o al `<body>`. Tuttavia, `currentTarget` identifica l'elemento il cui gestore di evento è attualmente in esecuzione:

{{embedlivesample("target and currentTarget")}}

La proprietà `target` è comunemente usata nella delega degli eventi, come visto nell'esempio di [Delega degli eventi](#delega_degli_eventi) precedente.

## Riepilogo

A questo punto si dovrebbe sapere tutto il necessario sugli eventi web in questa fase iniziale. Come accennato, gli eventi non fanno realmente parte del linguaggio JavaScript principale: sono definiti nelle Web API del browser.

Nel prossimo articolo verranno forniti alcuni test che possono essere usati per verificare quanto bene siano state comprese e memorizzate tutte le informazioni presentate sugli eventi.

## Vedere anche

- [domevents.dev](https://domevents.dev/)
  - : Un'utile applicazione interattiva che consente di apprendere il comportamento del sistema DOM Event attraverso l'esplorazione.
- [Eventi DOM](/it/docs/Web/API/Document_Object_Model/Events)
  - : Una guida completa per comprendere e gestire gli eventi.
- [Ordine degli eventi](https://www.quirksmode.org/js/events_order.html)
  - : Una discussione estremamente dettagliata della cattura e della propagazione di Peter-Paul Koch.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Events","Learn_web_development/Core/Scripting/Test_your_skills/Events", "Learn_web_development/Core/Scripting")}}
