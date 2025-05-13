---
title: Propagazione degli eventi
slug: Learn_web_development/Core/Scripting/Event_bubbling
l10n:
  sourceCommit: e68530dbce2b661c8860e9c6a1c70b1caca5a199
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Events","Learn_web_development/Core/Scripting/Image_gallery", "Learn_web_development/Core/Scripting")}}

Abbiamo visto che una pagina web è composta da _elementi_ — intestazioni, paragrafi di testo, immagini, pulsanti e così via — e che è possibile ascoltare eventi che accadono a questi elementi. Ad esempio, si potrebbe aggiungere un ascoltatore a un pulsante, e verrà eseguito quando l'utente clicca sul pulsante.

Abbiamo anche visto che questi elementi possono essere _annidati_ l'uno dentro l'altro: per esempio, un elemento {{htmlelement("button")}} potrebbe essere posizionato all'interno di un elemento {{htmlelement("div")}}. In questo caso chiameremmo l'elemento `<div>` un elemento _genitore_, e il `<button>` un elemento _figlio_.

In questo capitolo esamineremo il concetto di **propagazione degli eventi** — ovvero ciò che accade quando si aggiunge un ascoltatore di eventi a un elemento genitore e l'utente clicca sull'elemento figlio.

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
          <li>Delegazione degli eventi, realizzata mediante la propagazione degli eventi o l'evento di cattura.</li>
          <li>Interrompere la delegazione degli eventi con <code>stopPropagation()</code>.</li>
          <li>Accesso agli obiettivi degli eventi dall'oggetto evento.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Introduzione alla propagazione degli eventi

Introduciamo e definiamo la propagazione degli eventi attraverso un esempio.

### Impostare un ascoltatore su un elemento genitore

Consideriamo una pagina web come questa:

```html
<div id="container">
  <button>Click me!</button>
</div>
<pre id="output"></pre>
```

Qui il pulsante è all'interno di un altro elemento, un elemento {{HTMLElement("div")}}. Diciamo che l'elemento `<div>` qui è il **genitore** dell'elemento che contiene. Cosa succede se aggiungiamo un gestore di eventi click al genitore, quindi clicchiamo sul pulsante?

```js
const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

const container = document.querySelector("#container");
container.addEventListener("click", handleClick);
```

{{ EmbedLiveSample('Setting a listener on a parent element', '100%', 200, "", "") }}

Vedrà che il genitore attiva un evento di click quando l'utente clicca sul pulsante:

```plain
You clicked on a DIV element
```

Questo ha senso: il pulsante è all'interno del `<div>`, quindi quando si clicca sul pulsante si clicca anche implicitamente sull'elemento in cui si trova.

### Esempio di propagazione

Cosa succede se aggiungiamo ascoltatori di eventi sia al pulsante che al genitore?

```html
<body>
  <div id="container">
    <button>Click me!</button>
  </div>
  <pre id="output"></pre>
</body>
```

Proviamo ad aggiungere gestori di eventi click al pulsante, al suo genitore (l'elemento `<div>`) e all'elemento {{HTMLElement("body")}} che li contiene entrambi:

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

Vedrà che tutti e tre gli elementi attivano un evento di click quando l'utente clicca sul pulsante:

```plain
You clicked on a BUTTON element
You clicked on a DIV element
You clicked on a BODY element
```

In questo caso:

- il click sul pulsante viene attivato per primo.
- seguito dal click sul suo genitore (l'elemento `<div>`).
- seguito dal click sul genitore dell'elemento `<div>` (l'elemento `<body>`).

Descriviamo questo dicendo che l'evento **si propaga** dall'elemento più interno che è stato cliccato.

Questo comportamento può essere utile e può anche causare problemi inaspettati. Nelle sezioni successive, vedremo un problema che causa e troveremo la soluzione.

### Esempio di lettore video

In questo esempio la nostra pagina contiene un video, che è nascosto inizialmente, e un pulsante etichettato "Mostra video". Vogliamo che l'interazione sia la seguente:

- Quando l'utente clicca sul pulsante "Mostra video", mostra la casella che contiene il video, ma non avvia ancora la riproduzione del video.
- Quando l'utente clicca sul video, inizia la riproduzione del video.
- Quando l'utente clicca ovunque nella casella fuori dal video, nascondi la casella.

L'HTML appare così:

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

- un elemento `<button>`.
- un elemento `<div>` che ha inizialmente un attributo `class="hidden"`.
- un elemento `<video>` annidato all'interno dell'elemento `<div>`.

Stiamo usando CSS per nascondere gli elementi con la classe `"hidden"` impostata.

```css hidden
div {
  width: 100%;
  height: 100%;
  background-color: #eee;
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

Il JavaScript appare così:

```js
const btn = document.querySelector("button");
const box = document.querySelector("div");
const video = document.querySelector("video");

btn.addEventListener("click", () => box.classList.remove("hidden"));
video.addEventListener("click", () => video.play());
box.addEventListener("click", () => box.classList.add("hidden"));
```

Questo aggiunge tre ascoltatori di eventi `'click'`:

- uno sul `<button>`, che mostra il `<div>` che contiene il `<video>`.
- uno sul `<video>`, che inizia la riproduzione del video.
- uno sul `<div>`, che nasconde il video.

Vediamo come funziona:

{{ EmbedLiveSample('Video_player_example', '100%', 500) }}

Dovrebbe vedere che quando clicca sul pulsante, la casella e il video che contiene vengono mostrati. Ma poi quando clicca sul video, il video inizia a riprodursi, ma la casella viene nuovamente nascosta!

Il video è all'interno del `<div>` — ne fa parte — quindi cliccando sul video si eseguono entrambi i gestori di eventi, causando questo comportamento.

### Risolvere il problema con `stopPropagation()`

Come abbiamo visto nella sezione precedente, la propagazione degli eventi può a volte creare problemi, ma c'è un modo per evitarlo.
L'oggetto [`Event`](/it/docs/Web/API/Event) ha una funzione disponibile su di esso chiamata [`stopPropagation()`](/it/docs/Web/API/Event/stopPropagation) che, quando chiamata all'interno di un gestore di eventi, impedisce all'evento di propagarsi ad altri elementi.

Possiamo risolvere il nostro problema attuale modificando il JavaScript in questo modo:

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

Tutto ciò che stiamo facendo qui è chiamare `stopPropagation()` sull'oggetto evento nel gestore dell'evento `'click'` dell'elemento `<video>`. Questo impedirà a quell'evento di propagarsi fino alla casella. Ora provi a cliccare sul pulsante e poi sul video:

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
  background-color: #eee;
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

Una forma alternativa di propagazione degli eventi è la _cattura degli eventi_. Questo è simile alla propagazione degli eventi ma l'ordine è invertito: invece che l'evento si attivi prima sull'elemento più interno mirato e poi su elementi successivamente meno annidati, l'evento si attiva prima sull'elemento meno annidato e poi su elementi successivamente più annidati, fino a raggiungere il bersaglio.

La cattura degli eventi è disabilitata di default. Per abilitarla è necessario passare l'opzione `capture` in `addEventListener()`.

Questo esempio è proprio come l'[esempio di propagazione](#esempio_di_propagazione) che abbiamo visto in precedenza, tranne per il fatto che abbiamo usato l'opzione `capture`:

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

In questo caso, l'ordine dei messaggi è invertito: il gestore di eventi del `<body>` si attiva per primo, seguito dal gestore di eventi del `<div>`, seguito dal gestore di eventi del `<button>`:

```plain
You clicked on a BODY element
You clicked on a DIV element
You clicked on a BUTTON element
```

Perché preoccuparsi sia della cattura che della propagazione? Nei tempi andati, quando i browser erano molto meno compatibili tra loro rispetto a oggi, Netscape utilizzava solo la cattura degli eventi e Internet Explorer utilizzava solo la propagazione degli eventi. Quando il W3C ha deciso di provare a standardizzare il comportamento e raggiungere un consenso, sono finiti con questo sistema che includeva entrambi, il quale è ciò che i browser moderni implementano.

Di default quasi tutti i gestori di eventi sono registrati nella fase di propagazione, e questo ha più senso la maggior parte delle volte.

## Delegazione degli eventi

Nell'ultima sezione abbiamo esaminato un problema causato dalla propagazione degli eventi e come risolverlo. Tuttavia la propagazione degli eventi non è solo fastidiosa, può essere molto utile. In particolare, abilita la **delegazione degli eventi**. In questa pratica, quando vogliamo che un codice venga eseguito quando l'utente interagisce con uno di un gran numero di elementi figli, impostiamo l'ascoltatore di eventi sul loro genitore e facciamo in modo che gli eventi che accadono loro propaghino fino al loro genitore piuttosto che dover impostare il listener su ogni figlio individualmente.

Torniamo al nostro primo esempio, dove abbiamo impostato il colore di sfondo dell'intera pagina quando l'utente cliccava un pulsante. Supponiamo invece che la pagina sia divisa in 16 riquadri e vogliamo impostare ogni riquadro su un colore casuale quando l'utente clicca su quel riquadro.

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

Abbiamo un po' di CSS, per impostare la dimensione e la posizione dei riquadri:

```css
#container {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: 100px;
}
```

Ora in JavaScript, potremmo aggiungere un gestore di eventi click per ogni riquadro. Ma un'opzione molto più semplice ed efficiente è quella di impostare il gestore di eventi click sul genitore e fare affidamento sulla propagazione degli eventi per garantire che il gestore venga eseguito quando l'utente clicca su un riquadro:

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

L'output è il seguente (provi a cliccare in giro):

{{ EmbedLiveSample('Event delegation', '100%', 430, "", "") }}

> [!NOTE]
> In questo esempio, stiamo usando `event.target` per ottenere l'elemento che è stato il bersaglio dell'evento (ossia l'elemento più interno). Se volessimo accedere all'elemento che ha gestito questo evento (in questo caso il contenitore) potremmo usare `event.currentTarget`.

> [!NOTE]
> Veda [useful-eventtarget.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/useful-eventtarget.html) per il codice sorgente completo; veda anche [eseguito dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/events/useful-eventtarget.html) qui.

## `target` e `currentTarget`

Se osserva attentamente gli esempi che abbiamo introdotto in questa pagina, vedrà che stiamo usando due proprietà diverse dell'oggetto evento per accedere all'elemento che è stato cliccato. In [Impostare un ascoltatore su un elemento genitore](#impostare_un_ascoltatore_su_un_elemento_genitore) stiamo usando [`event.currentTarget`](/it/docs/Web/API/Event/currentTarget). Tuttavia, in [Delegazione degli eventi](#delegazione_degli_eventi), stiamo usando [`event.target`](/it/docs/Web/API/Event/target).

La differenza è che `target` si riferisce all'elemento su cui l'evento è stato inizialmente attivato, mentre `currentTarget` si riferisce all'elemento a cui questo gestore di eventi è stato attaccato.

Mentre `target` rimane lo stesso mentre un evento si propaga, `currentTarget` sarà diverso per i gestori di eventi che sono attaccati a elementi diversi nella gerarchia.

Possiamo vedere questo se adattiamo leggermente l'[esempio di propagazione](#esempio_di_propagazione) sopra. Stiamo usando lo stesso HTML di prima:

```html
<body>
  <div id="container">
    <button>Click me!</button>
  </div>
  <pre id="output"></pre>
</body>
```

Il JavaScript è quasi lo stesso, tranne per il fatto che stiamo registrando sia `target` che `currentTarget`:

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

Si noti che quando si clicca sul pulsante, `target` è l'elemento button ogni volta, che l'ascoltatore dell'evento sia attaccato al pulsante stesso, al `<div>`, o al `<body>`. Tuttavia `currentTarget` identifica l'elemento il cui ascoltatore di eventi stiamo attualmente eseguendo:

{{embedlivesample("target and currentTarget")}}

La proprietà `target` è comunemente usata nella delegazione degli eventi, come nel nostro esempio di [Delegazione degli eventi](#delegazione_degli_eventi) sopra.

## Test delle sue competenze!

Ha raggiunto la fine di questo articolo, ma ricorda le informazioni più importanti? Per verificare di aver mantenuto queste informazioni prima di andare avanti — vedere [Test your skills: Events](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Events).

## Riepilogo

Ora dovrebbe conoscere tutto ciò che le serve sapere sugli eventi web in questa fase iniziale.
Come accennato, gli eventi non fanno davvero parte del nucleo di JavaScript — sono definiti nelle API Web dei browser.

Inoltre, è importante capire che i diversi contesti in cui viene utilizzato JavaScript hanno modelli di eventi diversi — dalle API Web ad altre aree come le estensioni Web del browser e Node.js (JavaScript lato server).
Non ci aspettiamo che capisca tutte queste aree ora, ma certamente aiuta capire le basi degli eventi mentre avanza nell'apprendimento dello sviluppo web.

Di seguito, troverà una sfida che metterà alla prova la sua comprensione degli ultimi argomenti trattati.

## Vedere anche

- [domevents.dev](https://domevents.dev/)
  - : Un'app interattiva utile che consente di apprendere il comportamento del sistema di eventi DOM tramite l'esplorazione.
- [Riferimento degli eventi](/it/docs/Web/Events)
  - : Il principale riferimento agli eventi su MDN.
- [Ordine degli eventi](https://www.quirksmode.org/js/events_order.html)
  - : Una discussione dettagliata sulla cattura e propagazione degli eventi di Peter-Paul Koch.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Events","Learn_web_development/Core/Scripting/Image_gallery", "Learn_web_development/Core/Scripting")}}
