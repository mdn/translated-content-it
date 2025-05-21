---
title: Event bubbling
slug: Learn_web_development/Core/Scripting/Event_bubbling
l10n:
  sourceCommit: e68530dbce2b661c8860e9c6a1c70b1caca5a199
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Events","Learn_web_development/Core/Scripting/Image_gallery", "Learn_web_development/Core/Scripting")}}

Abbiamo visto che una pagina web è composta da _elementi_ — intestazioni, paragrafi di testo, immagini, pulsanti, e così via — e che è possibile ascoltare gli eventi che accadono a questi elementi. Ad esempio, è possibile aggiungere un listener a un pulsante, che verrà eseguito quando l'utente clicca sul pulsante.

Abbiamo anche visto che questi elementi possono essere _annidati_ uno dentro l'altro: ad esempio, un {{htmlelement("button")}} potrebbe essere posizionato all'interno di un elemento {{htmlelement("div")}}. In questo caso, chiameremmo l'elemento `<div>` un elemento _genitore_ e il `<button>` un elemento _figlio_.

In questo capitolo esamineremo il **bubbling degli eventi** — ciò che accade quando si aggiunge un listener di eventi a un elemento genitore e l'utente clicca sull'elemento figlio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Delegazione degli eventi, ottenuta tramite il bubbling o la cattura degli eventi.</li>
          <li>Interruzione della delegazione degli eventi con <code>stopPropagation()</code>.</li>
          <li>Accesso ai target degli eventi dall'oggetto evento.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Introduzione al bubbling degli eventi

Definiamo il bubbling degli eventi tramite un esempio.

### Impostare un listener su un elemento genitore

Consideriamo una pagina web come questa:

```html
<div id="container">
  <button>Click me!</button>
</div>
<pre id="output"></pre>
```

Qui il pulsante si trova all'interno di un altro elemento, un elemento {{HTMLElement("div")}}. Diciamo che l'elemento `<div>` è il **genitore** dell'elemento che contiene. Cosa succede se aggiungiamo un gestore di eventi di click al genitore e poi clicchiamo sul pulsante?

```js
const output = document.querySelector("#output");
function handleClick(e) {
  output.textContent += `You clicked on a ${e.currentTarget.tagName} element\n`;
}

const container = document.querySelector("#container");
container.addEventListener("click", handleClick);
```

{{ EmbedLiveSample('Setting a listener on a parent element', '100%', 200, "", "") }}

Vedrai che il genitore scatena un evento di click quando l'utente clicca sul pulsante:

```plain
You clicked on a DIV element
```

Questo ha senso: il pulsante è dentro il `<div>`, quindi quando clicchi il pulsante stai anche implicitamente cliccando l'elemento che lo contiene.

### Esempio di bubbling

Cosa succede se aggiungiamo dei listener di eventi sia al pulsante _che_ al genitore?

```html
<body>
  <div id="container">
    <button>Click me!</button>
  </div>
  <pre id="output"></pre>
</body>
```

Proviamo ad aggiungere gestori di eventi di click al pulsante, al suo genitore (l'elemento `<div>`) e all'elemento {{HTMLElement("body")}} che li contiene entrambi:

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

Vedrai che tutti e tre gli elementi scatenano un evento di click quando l'utente clicca sul pulsante:

```plain
You clicked on a BUTTON element
You clicked on a DIV element
You clicked on a BODY element
```

In questo caso:

- il click sul pulsante viene scatenato per primo.
- seguito dal click sul suo genitore (l'elemento `<div>`).
- seguito dal click sul genitore dell'elemento `<div>` (l'elemento `<body>`).

Descriviamo questo fenomeno dicendo che l'evento **risale** dall'elemento più interno che è stato cliccato.

Questo comportamento può essere utile ma può anche causare problemi inattesi. Nelle sezioni successive vedremo un problema che provoca, e troveremo la soluzione.

### Esempio di lettore video

In questo esempio la nostra pagina contiene un video, che inizialmente è nascosto, e un pulsante etichettato "Mostra video". Vogliamo la seguente interazione:

- Quando l'utente clicca il pulsante "Mostra video", mostrare la casella che contiene il video, ma senza avviare la riproduzione del video.
- Quando l'utente clicca sul video, avviare la riproduzione del video.
- Quando l'utente clicca in qualsiasi punto della casella fuori dal video, nascondere la casella.

L'HTML si presenta così:

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
- un elemento `<div>` che inizialmente ha un attributo `class="hidden"`.
- un elemento `<video>` annidato all'interno dell'elemento `<div>`.

Stiamo utilizzando CSS per nascondere gli elementi con la classe `"hidden"` impostata.

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

Il JavaScript si presenta così:

```js
const btn = document.querySelector("button");
const box = document.querySelector("div");
const video = document.querySelector("video");

btn.addEventListener("click", () => box.classList.remove("hidden"));
video.addEventListener("click", () => video.play());
box.addEventListener("click", () => box.classList.add("hidden"));
```

Questo aggiunge tre listener di eventi di `'click'`:

- uno sul `<button>`, che mostra il `<div>` che contiene il `<video>`.
- uno sul `<video>`, che inizia a riprodurre il video.
- uno sul `<div>`, che nasconde il video.

Vediamo come funziona:

{{ EmbedLiveSample('Video_player_example', '100%', 500) }}

Dovresti vedere che quando clicchi il pulsante, la casella e il video che contiene vengono mostrati. Ma poi, quando clicchi sul video, il video inizia a essere riprodotto, ma la casella viene nascosta di nuovo!

Il video è all'interno del `<div>` — ne fa parte — quindi cliccare sul video fa eseguire _entrambi_ i gestori di eventi, causando questo comportamento.

### Risolvere il problema con `stopPropagation()`

Come abbiamo visto nella sezione precedente, il bubbling degli eventi può talvolta creare problemi, ma esiste un modo per prevenirlo.
L'oggetto [`Event`](/it/docs/Web/API/Event) ha una funzione disponibile chiamata [`stopPropagation()`](/it/docs/Web/API/Event/stopPropagation) che, quando viene chiamata all'interno di un gestore di eventi, impedisce all'evento di risalire ad altri elementi.

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

Tutto ciò che facciamo qui è chiamare `stopPropagation()` sull'oggetto evento nel gestore per l'evento di `'click'` dell'elemento `<video>`. Questo impedirà che l'evento risalga alla casella. Ora prova a cliccare sul pulsante e poi sul video:

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

Una forma alternativa di propagazione degli eventi è la _cattura degli eventi_. È simile al bubbling degli eventi, ma l'ordine è invertito: anziché l'evento che si attiva prima sull'elemento più interno, e poi su elementi sempre meno annidati, l'evento si attiva prima sull'elemento _meno annidato_ e poi su elementi sempre più annidati, fino a raggiungere il target.

La cattura degli eventi è disabilitata di default. Per abilitarla è necessario passare l'opzione `capture` in `addEventListener()`.

Questo esempio è simile all'[esempio di bubbling](#esempio_di_bubbling) che abbiamo visto in precedenza, tranne per il fatto che abbiamo utilizzato l'opzione `capture`:

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

In questo caso, l'ordine dei messaggi è invertito: il gestore dell'evento `<body>` si attiva per primo, seguito dal gestore dell'evento `<div>`, e poi dal gestore dell'evento `<button>`:

```plain
You clicked on a BODY element
You clicked on a DIV element
You clicked on a BUTTON element
```

Perché preoccuparsi sia di cattura che di bubbling? Nei vecchi tempi, quando i browser erano meno compatibili tra loro, Netscape usava solo la cattura degli eventi, mentre Internet Explorer usava solo il bubbling degli eventi. Quando il W3C ha deciso di provare a standardizzare il comportamento e raggiungere un consenso, hanno finito per adottare questo sistema che includeva entrambi, che è quello implementato dai browser moderni.

Di default, quasi tutti i gestori di eventi sono registrati nella fase di bubbling, e questo ha più senso la maggior parte delle volte.

## Delegazione degli eventi

Nell'ultima sezione abbiamo esaminato un problema causato dal bubbling degli eventi e come risolverlo. Il bubbling degli eventi non è solo fastidioso, però: può essere molto utile. In particolare, abilita la **delegazione degli eventi**. In questa pratica, quando vogliamo che del codice venga eseguito quando l'utente interagisce con uno qualsiasi di un gran numero di elementi figli, impostiamo il listener dell'evento sul loro genitore e lasciamo che gli eventi che accadono su di essi risalgano al loro genitore piuttosto che dover impostare il listener dell'evento su ogni figlio singolarmente.

Torniamo al nostro primo esempio, in cui impostavamo il colore di sfondo dell'intera pagina quando l'utente cliccava un pulsante. Supponiamo invece che la pagina sia divisa in 16 riquadri, e vogliamo impostare ciascun riquadro a un colore casuale quando l'utente clicca su quel riquadro.

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

Ora, in JavaScript, potremmo aggiungere un gestore di eventi di click per ogni riquadro. Ma una opzione molto più semplice ed efficiente è quella di impostare il gestore di eventi di click sul genitore e fare affidamento sul bubbling degli eventi per garantire che il gestore venga eseguito quando l'utente clicca su un riquadro:

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

L'output è il seguente (prova a cliccare su di esso):

{{ EmbedLiveSample('Event delegation', '100%', 430, "", "") }}

> [!NOTE]
> In questo esempio, stiamo usando `event.target` per ottenere l'elemento che era il target dell'evento (cioè l'elemento più interno). Se volessimo accedere all'elemento che ha gestito questo evento (in questo caso il contenitore) potremmo usare `event.currentTarget`.

> [!NOTE]
> Vedi [useful-eventtarget.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/useful-eventtarget.html) per il codice sorgente completo; vedi anche [running live](https://mdn.github.io/learning-area/javascript/building-blocks/events/useful-eventtarget.html) qui.

## `target` e `currentTarget`

Esaminando attentamente gli esempi che abbiamo introdotto in questa pagina, vedrai che stiamo utilizzando due diverse proprietà dell'oggetto evento per accedere all'elemento che è stato cliccato. In [Impostare un listener su un elemento genitore](#impostare_un_listener_su_un_elemento_genitore) stiamo usando [`event.currentTarget`](/it/docs/Web/API/Event/currentTarget). Tuttavia, in [Delegazione degli eventi](#delegazione_degli_eventi), stiamo usando [`event.target`](/it/docs/Web/API/Event/target).

La differenza è che `target` fa riferimento all'elemento su cui l'evento è stato inizialmente scatenato, mentre `currentTarget` fa riferimento all'elemento a cui questo gestore di eventi è stato associato.

Mentre `target` rimane lo stesso mentre un evento risale, `currentTarget` sarà diverso per i gestori di eventi associati a elementi diversi nella gerarchia.

Possiamo vedere questo se modifichiamo leggermente l'[esempio di bubbling](#esempio_di_bubbling) sopra. Stiamo usando lo stesso HTML di prima:

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

Nota che quando clicchi il pulsante, `target` è l'elemento button ogni volta, sia che il gestore dell'evento sia associato al pulsante stesso, al `<div>`, o al `<body>`. Tuttavia, `currentTarget` identifica l'elemento il cui gestore di eventi stiamo attualmente eseguendo:

{{embedlivesample("target and currentTarget")}}

La proprietà `target` è comunemente usata nella delegazione degli eventi, come nel nostro esempio di [Delegazione degli eventi](#delegazione_degli_eventi) visto sopra.

## Testa le tue abilità!

Sei giunto alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Per verificare di aver trattenuto queste informazioni prima di andare avanti — vedi [Testa le tue abilità: Eventi](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Events).

## Sommario

Ora dovresti sapere tutto ciò che devi sapere sugli eventi web in questa fase iniziale.
Come detto, gli eventi non fanno realmente parte del core di JavaScript — sono definiti nelle API Web del browser.

Inoltre, è importante capire che i diversi contesti in cui JavaScript è utilizzato hanno modelli di eventi diversi — dalle API Web ad altre aree come le WebExtension del browser e Node.js (JavaScript lato server).
Non ci aspettiamo che tu comprenda tutte queste aree ora, ma certamente aiuta comprendere le basi degli eventi mentre prosegui nel tuo apprendimento dello sviluppo web.

Successivamente, troverai una sfida che testerà la tua comprensione degli ultimi argomenti.

## Vedi anche

- [domevents.dev](https://domevents.dev/)
  - : Un'app di apprendimento interattivo utile che consente di esplorare il comportamento del sistema di eventi DOM.
- [Riferimento agli eventi](/it/docs/Web/Events)
  - : Il principale riferimento agli eventi MDN.
- [Ordine degli eventi](https://www.quirksmode.org/js/events_order.html)
  - : Una discussione dettagliata su cattura e bubbling di Peter-Paul Koch.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Events","Learn_web_development/Core/Scripting/Image_gallery", "Learn_web_development/Core/Scripting")}}
