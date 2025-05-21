---
title: Introduzione agli eventi
short-title: Events
slug: Learn_web_development/Core/Scripting/Events
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Return_values","Learn_web_development/Core/Scripting/Event_bubbling", "Learn_web_development/Core/Scripting")}}

Gli eventi sono cose che accadono nel sistema che stai programmando, di cui il sistema ti informa affinché il tuo codice possa reagire a essi. Ad esempio, se un utente clicca un pulsante su una pagina web, potresti voler reagire a quell'azione mostrando una finestra informativa. In questo articolo, discuteremo alcuni concetti importanti sugli eventi e analizzeremo i fondamenti di come funzionano nei browser.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sono gli eventi — un segnale inviato dal browser quando accade qualcosa di significativo, al quale il programmatore può rispondere con del codice.</li>
          <li>Configurare gestori di eventi usando <code>addEventListener()</code> (e <code>removeEventListener()</code>) e proprietà dei gestori di eventi.</li>
          <li>Attributi dei gestori di eventi inline, e perché non dovresti usarli.</li>
          <li>Oggetti evento.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è un evento?

Gli eventi sono cose che accadono nel sistema che stai programmando — il sistema produce (o "lancia") un segnale di qualche tipo quando si verifica un evento, e fornisce un meccanismo tramite il quale può essere intrapresa un'azione automaticamente (cioè, eseguire del codice) quando l'evento si verifica. Gli eventi vengono attivati all'interno della finestra del browser e tendono ad essere associati a un oggetto specifico che si trova nel browser. Questo potrebbe essere un singolo elemento, un insieme di elementi, il documento HTML caricato nella scheda corrente o l'intera finestra del browser. Ci sono molti tipi diversi di eventi che possono verificarsi.

Ad esempio:

- L'utente seleziona, clicca o passa il cursore su un determinato elemento.
- L'utente preme un tasto sulla tastiera.
- L'utente ridimensiona o chiude la finestra del browser.
- Una pagina web termina il caricamento.
- Un modulo viene inviato.
- Un video viene riprodotto, messo in pausa o finisce.
- Si verifica un errore.

Puoi intuire da questo (e dare un'occhiata al [riferimento eventi](/it/docs/Web/Events) di MDN) che ci sono **molti** eventi che possono essere lanciati.

Per reagire a un evento, si attacca un **gestore di eventi** ad esso. Questo è un blocco di codice (solitamente una funzione JavaScript che il programmatore crea) che viene eseguito quando l'evento viene lanciato. Quando un tale blocco di codice è definito per essere eseguito in risposta a un evento, diciamo che stiamo **registrando un gestore di eventi**. Nota: I gestori di eventi vengono talvolta chiamati **ascoltatori di eventi** — per i nostri scopi, sono praticamente intercambiabili, anche se a rigor di termini, lavorano insieme. L'ascoltatore ascolta l'evento che si sta verificando e il gestore è il codice che viene eseguito in risposta al suo verificarsi.

> [!NOTE]
> Gli eventi web non sono parte del linguaggio JavaScript di base — sono definiti come parte delle API integrate nel browser.

### Un esempio: gestione di un evento di click

Nel seguente esempio, abbiamo un singolo {{htmlelement("button")}} nella pagina:

```html
<button>Change color</button>
```

```css hidden
button {
  margin: 10px;
}
```

Poi abbiamo un po' di JavaScript. Lo esamineremo più dettagliatamente nella prossima sezione, ma per ora possiamo solo dire: aggiunge un gestore di eventi all'evento `"click"` del pulsante, e il gestore reagisce all'evento impostando lo sfondo della pagina su un colore casuale:

```js
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.addEventListener("click", () => {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
});
```

L'output dell'esempio è il seguente. Prova a cliccare il pulsante:

{{ EmbedLiveSample('An example: handling a click event', '100%', 200, "", "") }}

## Utilizzando addEventListener()

Come abbiamo visto nell'ultimo esempio, gli oggetti che possono lanciare eventi hanno un metodo [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener), e questo è il meccanismo raccomandato per aggiungere gestori di eventi.

Analizziamo più da vicino il codice dell'ultimo esempio:

```js
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.addEventListener("click", () => {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
});
```

L'elemento {{HTMLElement("button")}} HTML lancerà un evento quando l'utente clicca sul pulsante. Quindi definisce una funzione `addEventListener()`, che stiamo chiamando qui. Stiamo passando due parametri:

- la stringa `"click"`, per indicare che vogliamo ascoltare l'evento di click. I pulsanti possono lanciare molti altri eventi, come [`"mouseover"`](/it/docs/Web/API/Element/mouseover_event) quando l'utente passa il mouse sopra il pulsante, o [`"keydown"`](/it/docs/Web/API/Element/keydown_event) quando l'utente preme un tasto e il pulsante è concentrato.
- una funzione da chiamare quando l'evento si verifica. Nel nostro caso, la funzione genera un colore RGB casuale e imposta il [`background-color`](/it/docs/Web/CSS/background-color) della pagina [`<body>`](/it/docs/Web/HTML/Reference/Elements/body) su quel colore.

Va bene rendere la funzione del gestore una funzione nominata separata, come questa:

```js
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

function changeBackground() {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
}

btn.addEventListener("click", changeBackground);
```

### Ascoltare altri eventi

Ci sono molti eventi diversi che possono essere lanciati da un elemento pulsante. Facciamo un esperimento.

Innanzitutto, effettua una copia locale di [random-color-addeventlistener.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/random-color-addeventlistener.html), e aprila nel tuo browser. È semplicemente una copia dell'esempio semplice con il colore casuale che abbiamo già usato. Ora prova a cambiare `click` con i seguenti valori diversi a turno e osserva i risultati nell'esempio:

- [`focus`](/it/docs/Web/API/Element/focus_event) e [`blur`](/it/docs/Web/API/Element/blur_event) — Il colore cambia quando il pulsante è concentrato e non concentrato; prova a premere il tab per mettere a fuoco il pulsante e premi di nuovo il tab per togliere il fuoco dal pulsante. Questi eventi spesso vengono utilizzati per visualizzare informazioni sul riempimento dei campi del modulo quando sono concentrati, o per visualizzare un messaggio di errore se un campo del modulo è stato riempito con un valore errato.
- [`dblclick`](/it/docs/Web/API/Element/dblclick_event) — Il colore cambia solo quando il pulsante viene cliccato due volte.
- [`mouseover`](/it/docs/Web/API/Element/mouseover_event) e [`mouseout`](/it/docs/Web/API/Element/mouseout_event) — Il colore cambia quando il puntatore del mouse passa sopra il pulsante, o quando il puntatore si sposta fuori dal pulsante, rispettivamente.

Alcuni eventi, come `click`, sono disponibili su quasi tutti gli elementi. Altri sono più specifici e utili solo in determinate situazioni: ad esempio, l'evento [`play`](/it/docs/Web/API/HTMLMediaElement/play_event) è disponibile solo su alcuni elementi, come {{htmlelement("video")}}.

### Rimozione degli ascoltatori

Se hai aggiunto un gestore di eventi usando `addEventListener()`, puoi rimuoverlo nuovamente utilizzando il metodo [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener). Ad esempio, in questo modo si rimuoverebbe il gestore di eventi `changeBackground()`:

```js
btn.removeEventListener("click", changeBackground);
```

I gestori di eventi possono anche essere rimossi passando un [`AbortSignal`](/it/docs/Web/API/AbortSignal) a [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) e successivamente chiamando [`abort()`](/it/docs/Web/API/AbortController/abort) sul controller che possiede l'`AbortSignal`. Ad esempio, aggiungere un gestore di eventi che possiamo rimuovere con un `AbortSignal`:

```js-nolint
const controller = new AbortController();

btn.addEventListener("click",
  () => {
    const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
    document.body.style.backgroundColor = rndCol;
  },
  { signal: controller.signal } // pass an AbortSignal to this handler
);
```

Quindi, il gestore di eventi creato dal codice sopra può essere rimosso in questo modo:

```js
controller.abort(); // removes any/all event handlers associated with this controller
```

Per programmi semplici e piccoli, non è necessario pulire vecchi gestori di eventi non utilizzati, ma per programmi più grandi e complessi, può migliorare l'efficienza. Inoltre, la capacità di rimuovere i gestori di eventi ti consente di avere lo stesso pulsante che esegue azioni diverse in circostanze diverse: tutto ciò che devi fare è aggiungere o rimuovere i gestori.

### Aggiungere più ascoltatori per un singolo evento

Facendo più di una chiamata a [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener), fornendo gestori diversi, puoi avere più gestori per un singolo evento:

```js
myElement.addEventListener("click", functionA);
myElement.addEventListener("click", functionB);
```

Entrambe le funzioni ora verrebbero eseguite quando l'elemento viene cliccato.

## Altri meccanismi di ascolto degli eventi

Raccomandiamo di utilizzare `addEventListener()` per registrare gestori di eventi. È il metodo più potente e si adatta meglio a programmi più complessi. Tuttavia, ci sono altri due modi per registrare gestori di eventi che potresti vedere: _proprietà dei gestori di eventi_ e _gestori di eventi inline_.

### Proprietà dei gestori di eventi

Gli oggetti (come i pulsanti) che possono lanciare eventi solitamente hanno anche proprietà il cui nome è `on` seguito dal nome dell'evento. Ad esempio, gli elementi hanno una proprietà `onclick`. Questa è chiamata _proprietà del gestore di eventi_. Per ascoltare l'evento, puoi assegnare la funzione gestore alla proprietà.

Ad esempio, potremmo riscrivere l'esempio del colore casuale in questo modo:

```js
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

btn.onclick = () => {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
};
```

Puoi anche impostare la proprietà del gestore su una funzione nominata:

```js
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

function bgChange() {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
}

btn.onclick = bgChange;
```

Con le proprietà dei gestori di eventi, non puoi aggiungere più di un gestore per un singolo evento. Ad esempio, puoi chiamare `addEventListener('click', handler)` su un elemento più volte, specificando funzioni diverse nel secondo argomento:

```js
element.addEventListener("click", function1);
element.addEventListener("click", function2);
```

Questo è impossibile con le proprietà dei gestori di eventi perché qualsiasi tentativo successivo di impostare la proprietà sovrascriverà quelli precedenti:

```js
element.onclick = function1;
element.onclick = function2;
```

### Gestori di eventi inline — non usarli

Potresti vedere anche un modello simile a questo nel tuo codice:

```html
<button onclick="bgChange()">Press me</button>
```

```js
function bgChange() {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
}
```

Il metodo più antico di registrazione dei gestori di eventi trovato sul Web coinvolge [_attributi di gestori di eventi HTML_](/it/docs/Web/HTML/Reference/Attributes#event_handler_attributes) (o _gestori di eventi inline_) come quello mostrato sopra — il valore dell'attributo è letteralmente il codice JavaScript che vuoi eseguire quando si verifica l'evento. L'esempio sopra invoca una funzione definita all'interno di un elemento {{htmlelement("script")}} nella stessa pagina, ma potresti anche inserire JavaScript direttamente all'interno dell'attributo, ad esempio:

```html
<button onclick="alert('Hello, this is my old-fashioned event handler!');">
  Press me
</button>
```

Puoi trovare equivalenti degli attributi HTML per molte delle proprietà dei gestori di eventi; tuttavia, non dovresti usarli — sono considerati una cattiva pratica. Potrebbe sembrare facile utilizzare un attributo del gestore di eventi se stai facendo qualcosa di veramente veloce, ma diventano rapidamente ingestibili e inefficaci.

In primo luogo, non è una buona idea mescolare il tuo HTML e il tuo JavaScript, poiché diventa difficile da leggere. Mantenere il tuo JavaScript separato è una buona pratica, e se si trova in un file separato puoi applicarlo a più documenti HTML.

Anche in un singolo file, i gestori di eventi inline non sono una buona idea. Un pulsante va bene, ma se ne avessi 100? Dovresti aggiungere 100 attributi al file; si trasformerebbe rapidamente in un incubo per la manutenzione. Con JavaScript, puoi facilmente aggiungere una funzione gestore di eventi a tutti i pulsanti nella pagina, indipendentemente da quanti siano, usando qualcosa come questo:

```js
const buttons = document.querySelectorAll("button");

for (const button of buttons) {
  button.addEventListener("click", bgChange);
}
```

Infine, molte configurazioni server comuni disabiliteranno JavaScript inline, come misura di sicurezza.

**Non dovresti mai usare gli attributi dei gestori di eventi HTML** — sono obsoleti e utilizzarli è una cattiva pratica.

## Oggetti evento

A volte, all'interno di una funzione di gestore eventi, vedrai un parametro specificato con un nome come `event`, `evt` o `e`. Questo è chiamato **oggetto evento**, e viene passato automaticamente ai gestori di eventi per fornire funzionalità extra e informazioni. Ad esempio, riscriviamo nuovamente il nostro esempio di colore casuale:

```js
const btn = document.querySelector("button");

function random(number) {
  return Math.floor(Math.random() * (number + 1));
}

function bgChange(e) {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  e.target.style.backgroundColor = rndCol;
  console.log(e);
}

btn.addEventListener("click", bgChange);
```

> [!NOTE]
> Puoi trovare il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/random-color-eventobject.html) per questo esempio su GitHub (vedi anche un [esempio in esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/events/random-color-eventobject.html)).

Qui puoi vedere che stiamo includendo un oggetto evento, **e**, nella funzione, e nella funzione impostiamo uno stile di sfondo colore su `e.target` — che è il pulsante stesso. La proprietà `target` dell'oggetto evento è sempre un riferimento all'elemento su cui si è verificato l'evento. Quindi, in questo esempio, stiamo impostando un colore di sfondo casuale sul pulsante, non sulla pagina.

> [!NOTE]
> Puoi usare qualsiasi nome ti piaccia per l'oggetto evento — devi solo scegliere un nome che puoi poi usare per riferirti ad esso all'interno della funzione gestore eventi.
> `e`/`evt`/`event` è più comunemente usato dagli sviluppatori perché sono corti e facili da ricordare.
> È sempre bene essere coerenti — con te stesso e, se possibile, con gli altri.

### Proprietà extra degli oggetti evento

La maggior parte degli oggetti evento ha un insieme standard di proprietà e metodi disponibili sull'oggetto evento; vedi la documentazione dell'oggetto [`Event`](/it/docs/Web/API/Event) per un elenco completo.

Alcuni oggetti evento aggiungono proprietà extra che sono rilevanti per quel tipo particolare di evento. Ad esempio, l'evento [`keydown`](/it/docs/Web/API/Element/keydown_event) viene lanciato quando l'utente preme un tasto. Il suo oggetto evento è un [`KeyboardEvent`](/it/docs/Web/API/KeyboardEvent), che è un oggetto `Event` specializzato con una proprietà `key` che ti dice quale tasto è stato premuto:

```html
<input id="textBox" type="text" />
<div id="output"></div>
```

```js
const textBox = document.querySelector("#textBox");
const output = document.querySelector("#output");
textBox.addEventListener("keydown", (event) => {
  output.textContent = `You pressed "${event.key}".`;
});
```

```css hidden
div {
  margin: 0.5rem 0;
}
```

Prova a digitare nella casella di testo e guarda l'output:

{{EmbedLiveSample("Extra_properties_of_event_objects", 100, 100)}}

## Prevenire il comportamento predefinito

A volte, ti capiterà una situazione in cui vuoi impedire a un evento di fare quello che farebbe in modo predefinito. L'esempio più comune è quello di un modulo web, ad esempio un modulo di registrazione personalizzato. Quando inserisci i dettagli e clicchi sul pulsante di invio, il comportamento naturale è quello che i dati vengano inviati a una pagina specifica sul server per l'elaborazione, e il browser venga reindirizzato a una pagina di "messaggio di successo" di qualche tipo (o la stessa pagina, se non ne viene specificata un'altra).

Il problema sorge quando l'utente non ha inviato correttamente i dati — come sviluppatore, vuoi impedire l'invio al server e fornire un messaggio di errore che indichi cosa c'è di sbagliato e cosa deve essere fatto per risolvere il problema. Alcuni browser supportano funzionalità di convalida dei dati del modulo automatiche, ma poiché molti non lo fanno, si consiglia di non fare affidamento su queste e implementare i propri controlli di convalida. Vediamo un esempio.

Innanzitutto, un semplice modulo HTML che richiede di inserire il proprio nome e cognome:

```html
<form>
  <div>
    <label for="fname">First name: </label>
    <input id="fname" type="text" />
  </div>
  <div>
    <label for="lname">Last name: </label>
    <input id="lname" type="text" />
  </div>
  <div>
    <input id="submit" type="submit" />
  </div>
</form>
<p></p>
```

```css hidden
div {
  margin-bottom: 10px;
}
```

Ora un po' di JavaScript — qui implementiamo un controllo molto semplice all'interno di un gestore per l'evento [`submit`](/it/docs/Web/API/HTMLFormElement/submit_event) (l'evento submit viene lanciato su un modulo quando viene inviato) che verifica se i campi di testo sono vuoti. Se lo sono, chiamiamo la funzione [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento — che ferma l'invio del modulo — e poi visualizziamo un messaggio di errore nel paragrafo sotto il nostro modulo per dire all'utente cosa c'è che non va:

```js
const form = document.querySelector("form");
const fname = document.getElementById("fname");
const lname = document.getElementById("lname");
const para = document.querySelector("p");

form.addEventListener("submit", (e) => {
  if (fname.value === "" || lname.value === "") {
    e.preventDefault();
    para.textContent = "You need to fill in both names!";
  }
});
```

Ovviamente, questa è una validazione del modulo piuttosto debole — non fermerebbe l'utente dal convalidare il modulo con spazi o numeri inseriti nei campi, ad esempio — ma va bene per scopi di esempio. L'output è il seguente:

{{ EmbedLiveSample('Preventing_default_behavior', '100%', 180, "", "") }}

> [!NOTE]
> Per il codice sorgente completo, vedi [preventdefault-validation.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/preventdefault-validation.html) (vedi anche un [esempio in esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/events/preventdefault-validation.html) qui).

## Non solo pagine web

Gli eventi non sono unici per JavaScript — la maggior parte dei linguaggi di programmazione ha un modello di eventi di qualche tipo, e il modo in cui il modello funziona spesso differisce dal modo di JavaScript. In realtà, il modello di eventi in JavaScript per le pagine web differisce dal modello di eventi per JavaScript come viene utilizzato in altri ambienti.

Ad esempio, [Node.js](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs) è un ambiente JavaScript molto popolare che consente agli sviluppatori di utilizzare JavaScript per costruire applicazioni di rete e lato server. Il [modello di eventi Node.js](https://nodejs.org/api/events.html) si basa sugli ascoltatori per ascoltare gli eventi e sugli emettitori per emettere eventi periodicamente — non sembra molto diverso, ma il codice è abbastanza diverso, facendo uso di funzioni come `on()` per registrare un ascoltatore di eventi, e `once()` per registrare un ascoltatore di eventi che si deregistra dopo essere stato eseguito una volta. I documenti sugli eventi di connessione [HTTP](https://nodejs.org/api/http.html#event-connect) forniscono un buon esempio.

Puoi anche usare JavaScript per costruire componenti aggiuntivi multipiattaforma — miglioramenti della funzionalità del browser — usando una tecnologia chiamata [WebExtensions](/it/docs/Mozilla/Add-ons/WebExtensions). Il modello di eventi è simile al modello di eventi web, ma un po' diverso — le proprietà degli ascoltatori di eventi sono scritte in {{Glossary("camel_case", "camel case")}} (come `onMessage` anziché `onmessage`), e devono essere combinate con la funzione `addListener`. Vedi la pagina [`runtime.onMessage`](/it/docs/Mozilla/Add-ons/WebExtensions/API/runtime/onMessage#examples) per un esempio.

Non è necessario comprendere nulla su altri ambienti a questo punto del tuo apprendimento; volevamo solo chiarire che gli eventi possono differire in diversi ambienti di programmazione.

## Riepilogo

In questo capitolo abbiamo imparato cosa sono gli eventi, come ascoltarli e come rispondere ad essi.

Avrai già visto che gli elementi in una pagina web possono essere nidificati all'interno di altri elementi. Ad esempio, nell'esempio [Prevenire il comportamento predefinito](#prevenire_il_comportamento_predefinito), abbiamo alcune caselle di testo, posizionate all'interno di elementi {{htmlelement("div")}}, a loro volta poste all'interno di un elemento {{htmlelement("form")}}. Cosa succede quando un ascoltatore di eventi del click viene collegato all'elemento `<form>`, e l'utente clicca all'interno di una delle caselle di testo? La funzione gestore associata viene ancora eseguita tramite un processo chiamato _event bubbling_, che viene trattato nella prossima lezione.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Return_values","Learn_web_development/Core/Scripting/Event_bubbling", "Learn_web_development/Core/Scripting")}}
