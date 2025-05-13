---
title: Introduzione agli eventi
short-title: Events
slug: Learn_web_development/Core/Scripting/Events
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Return_values","Learn_web_development/Core/Scripting/Event_bubbling", "Learn_web_development/Core/Scripting")}}

Gli eventi sono eventi che si verificano nel sistema che si sta programmando e di cui il sistema informa in modo che il codice possa reagire ad essi. Ad esempio, se l'utente clicca su un pulsante in una pagina web, potrebbe voler reagire a quell'azione visualizzando una finestra informativa. In questo articolo, discuteremo alcuni concetti importanti relativi agli eventi e guarderemo ai fondamenti di come funzionano nei browser.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">nozioni fondamentali di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sono gli eventi — un segnale emesso dal browser quando si verifica qualcosa di significativo, che consente al programmatore di eseguire del codice in risposta.</li>
          <li>Impostare i gestori di eventi utilizzando <code>addEventListener()</code> (e <code>removeEventListener()</code>) e le proprietà del gestore di eventi.</li>
          <li>Attributi dei gestori di eventi inline, e perché non dovrebbero essere utilizzati.</li>
          <li>Oggetti evento.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è un evento?

Gli eventi sono eventi che si verificano nel sistema che si sta programmando — il sistema produce (o "emette") un segnale di qualche tipo quando si verifica un evento e fornisce un meccanismo tramite cui può essere automaticamente intrapresa un'azione (cioè eseguire del codice) quando si verifica l'evento. Gli eventi vengono emessi all'interno della finestra del browser e tendono ad essere associati a un elemento specifico che risiede in essa. Questo potrebbe essere un singolo elemento, un insieme di elementi, il documento HTML caricato nella scheda corrente o l'intera finestra del browser. Ci sono molti diversi tipi di eventi che possono verificarsi.

Per esempio:

- L'utente seleziona, clicca o passa il cursore su un determinato elemento.
- L'utente preme un tasto sulla tastiera.
- L'utente ridimensiona o chiude la finestra del browser.
- Una pagina web termina il caricamento.
- Un modulo viene inviato.
- Un video viene riprodotto, messo in pausa, o terminato.
- Si verifica un errore.

Da questo (e dando un'occhiata al [riferimento eventi](https://developer.mozilla.org/it/docs/Web/Events) su MDN), si può dedurre che ci sono **molti** eventi che possono essere emessi.

Per reagire a un evento, si collega ad esso un **gestore di eventi**. Questo è un blocco di codice (solitamente una funzione JavaScript che si crea come programmatore) che viene eseguito quando l'evento viene emesso. Quando tale blocco di codice è definito per essere eseguito in risposta a un evento, diciamo che stiamo **registrando un gestore di eventi**. Nota: I gestori di eventi sono talvolta chiamati **listener di eventi** — sono praticamente intercambiabili per i nostri scopi, sebbene, in senso stretto, lavorino insieme. Il listener ascolta il verificarsi dell'evento, e il gestore è il codice che viene eseguito in risposta al suo verificarsi.

> [!NOTE]
> Gli eventi web non fanno parte del linguaggio principale JavaScript — sono definiti come parte delle API integrate nel browser.

### Un esempio: gestire un evento click

Nel seguente esempio, abbiamo un singolo {{htmlelement("button")}} nella pagina:

```html
<button>Change color</button>
```

```css hidden
button {
  margin: 10px;
}
```

Poi abbiamo un po' di JavaScript. Esamineremo questo in dettaglio nella prossima sezione, ma per ora possiamo solo dire: aggiunge un gestore di eventi all'evento `"click"` del pulsante, e il gestore reagisce all'evento impostando lo sfondo della pagina a un colore casuale:

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

L'esempio di output è il seguente. Provi a fare clic sul pulsante:

{{ EmbedLiveSample('An example: handling a click event', '100%', 200, "", "") }}

## Utilizzare addEventListener()

Come abbiamo visto nell'ultimo esempio, gli oggetti che possono emettere eventi hanno un metodo [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener), e questo è il meccanismo raccomandato per aggiungere gestori di eventi.

Diamo un'occhiata più da vicino al codice dell'ultimo esempio:

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

L'elemento HTML {{HTMLElement("button")}} emetterà un evento quando l'utente clicca sul pulsante. Quindi definisce una funzione `addEventListener()`, che stiamo chiamando qui. Passiamo due parametri:

- la stringa `"click"`, per indicare che vogliamo ascoltare l'evento click. I pulsanti possono emettere molti altri eventi, come [`"mouseover"`](/it/docs/Web/API/Element/mouseover_event) quando l'utente muove il mouse sopra il pulsante, o [`"keydown"`](/it/docs/Web/API/Element/keydown_event) quando l'utente preme un tasto e il pulsante è focalizzato.
- una funzione da chiamare quando si verifica l'evento. Nel nostro caso, la funzione genera un colore RGB casuale e imposta il [`background-color`](/it/docs/Web/CSS/background-color) della pagina [`<body>`](/it/docs/Web/HTML/Reference/Elements/body) a quel colore.

È possibile rendere la funzione del gestore una funzione nominata separata, in questo modo:

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

### Ascoltare eventi diversi

Ci sono molti diversi eventi che possono essere emessi da un elemento button. Facciamo un esperimento.

Prima, faccia una copia locale di [random-color-addeventlistener.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/random-color-addeventlistener.html), e la apra nel suo browser. È solo una copia del semplice esempio di colore casuale con cui abbiamo già giocato. Ora provi a cambiare `click` con i seguenti valori diversi a turno e osservi i risultati nell'esempio:

- [`focus`](/it/docs/Web/API/Element/focus_event) e [`blur`](/it/docs/Web/API/Element/blur_event) — Il colore cambia quando il pulsante è focalizzato e non focalizzato; provi a premere il tasto tab per concentrarsi sul pulsante e a premere di nuovo il tab per spostare il focus dal pulsante. Questi eventi vengono spesso utilizzati per visualizzare informazioni sull'inserimento di campi del modulo quando sono focalizzati, o per visualizzare un messaggio di errore se un campo del modulo è riempito con un valore errato.
- [`dblclick`](/it/docs/Web/API/Element/dblclick_event) — Il colore cambia solo quando il pulsante viene cliccato due volte velocemente.
- [`mouseover`](/it/docs/Web/API/Element/mouseover_event) e [`mouseout`](/it/docs/Web/API/Element/mouseout_event) — Il colore cambia quando il puntatore del mouse passa sopra il pulsante, o quando il puntatore si sposta fuori dal pulsante, rispettivamente.

Alcuni eventi, come `click`, sono disponibili su quasi ogni elemento. Altri sono più specifici e utili solo in certe situazioni: per esempio, l'evento [`play`](/it/docs/Web/API/HTMLMediaElement/play_event) è disponibile solo su alcuni elementi, come il {{htmlelement("video")}}.

### Rimozione dei listener

Se ha aggiunto un gestore di eventi utilizzando `addEventListener()`, può rimuoverlo di nuovo utilizzando il metodo [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener). Ad esempio, questo rimuoverebbe il gestore `changeBackground()`:

```js
btn.removeEventListener("click", changeBackground);
```

I gestori di eventi possono anche essere rimossi passando un [`AbortSignal`](/it/docs/Web/API/AbortSignal) a [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) e poi chiamare successivamente [`abort()`](/it/docs/Web/API/AbortController/abort) sul controller che possiede l'`AbortSignal`. Ad esempio, per aggiungere un gestore di eventi che possiamo rimuovere con un `AbortSignal`:

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

Quindi, il gestore degli eventi creato dal codice sopra può essere rimosso in questo modo:

```js
controller.abort(); // removes any/all event handlers associated with this controller
```

Per programmi semplici e piccoli, non è necessario ripulire vecchi e inutilizzati gestori di eventi, ma per programmi più grandi e complessi, può migliorare l'efficienza. Inoltre, la capacità di rimuovere i gestori di eventi le permette di avere lo stesso pulsante che esegue azioni diverse in circostanze diverse: tutto ciò che deve fare è aggiungere o rimuovere i gestori.

### Aggiungere più listener per un singolo evento

Effettuando più di una chiamata a [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener), fornendo diversi gestori, può avere più gestori per un singolo evento:

```js
myElement.addEventListener("click", functionA);
myElement.addEventListener("click", functionB);
```

Ora entrambe le funzioni verrebbero eseguite quando l'elemento viene cliccato.

## Altri meccanismi per i listener di eventi

Le consigliamo di usare `addEventListener()` per registrare i gestori di eventi. È il metodo più potente e si adatta meglio a programmi più complessi. Tuttavia, esistono altri due modi di registrare i gestori di eventi che potrebbe vedere: _proprietà dei gestori di eventi_ e _gestori di eventi inline_.

### Proprietà dei gestori di eventi

Gli oggetti (come i pulsanti) che possono emettere eventi hanno solitamente anche proprietà il cui nome è `on` seguito dal nome dell'evento. Ad esempio, gli elementi hanno una proprietà `onclick`. Questa è chiamata _proprietà del gestore di eventi_. Per ascoltare l'evento, può assegnare la funzione del gestore alla proprietà.

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

Può anche impostare la proprietà del gestore a una funzione nominata:

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

Con le proprietà del gestore di eventi, non può aggiungere più di un gestore per un singolo evento. Ad esempio, può chiamare `addEventListener('click', handler)` su un elemento più volte, con funzioni diverse specificate nel secondo argomento:

```js
element.addEventListener("click", function1);
element.addEventListener("click", function2);
```

Questo è impossibile con le proprietà dei gestori di eventi perché qualsiasi tentativo successivo di impostare la proprietà sovrascriverà quelli precedenti:

```js
element.onclick = function1;
element.onclick = function2;
```

### Gestori di eventi inline — non usi questi

Potrebbe anche vedere un pattern come questo nel suo codice:

```html
<button onclick="bgChange()">Press me</button>
```

```js
function bgChange() {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
}
```

Il primo metodo di registrazione dei gestori di eventi trovato sul Web prevedeva [_attributi HTML dei gestori di eventi_](/it/docs/Web/HTML/Reference/Attributes#event_handler_attributes) (o _gestori di eventi inline_) come quello mostrato sopra — il valore dell'attributo è letteralmente il codice JavaScript che si desidera eseguire quando si verifica l'evento. L'esempio sopra invoca una funzione definita all'interno di un elemento {{htmlelement("script")}} sulla stessa pagina, ma potrebbero anche inserire JavaScript direttamente all'interno dell'attributo, ad esempio:

```html
<button onclick="alert('Hello, this is my old-fashioned event handler!');">
  Press me
</button>
```

Può trovare equivalenti attributi HTML per molte delle proprietà dei gestori di eventi; tuttavia, non dovrebbe usare questi — sono considerati una cattiva pratica. Potrebbe sembrare facile usare un attributo del gestore di eventi se sta facendo qualcosa di veramente veloce, ma diventano rapidamente ingovernabili e inefficaci.

Per cominciare, non è una buona idea mescolare HTML e JavaScript, poiché diventa difficile da leggere. Mantenere separati i propri JavaScript è una buona pratica e, se sono in un file separato, li si può applicare a più documenti HTML.

Anche in un singolo file, i gestori di eventi inline non sono una buona idea. Un pulsante va bene, ma che cosa succede se avesse 100 pulsanti? Dovrebbe aggiungere 100 attributi al file; diventerebbe rapidamente un incubo di manutenzione. Con JavaScript, potrebbe facilmente aggiungere una funzione di gestore di eventi a tutti i pulsanti della pagina, indipendentemente da quanti ce ne siano, utilizzando qualcosa di simile:

```js
const buttons = document.querySelectorAll("button");

for (const button of buttons) {
  button.addEventListener("click", bgChange);
}
```

Infine, molte configurazioni di server comuni disabiliteranno JavaScript inline, come misura di sicurezza.

**Non dovrebbe mai usare gli attributi degli handler di eventi HTML** — sono obsoleti e usarli è una cattiva pratica.

## Oggetti evento

A volte, all'interno di una funzione gestore di eventi, vedrà un parametro specificato con un nome come `event`, `evt`, o `e`. Questo è chiamato **oggetto evento**, e viene automaticamente passato ai gestori di eventi per fornire funzionalità e informazioni extra. Ad esempio, riscriviamo di nuovo leggermente il nostro esempio di colore casuale:

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
> Può trovare il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/random-color-eventobject.html) di questo esempio su GitHub (vedilo anche [in esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/events/random-color-eventobject.html)).

Qui può vedere che stiamo includendo un oggetto evento, **e**, nella funzione, e nella funzione che imposta uno stile di colore di sfondo su `e.target` — che è il pulsante stesso. La proprietà `target` dell'oggetto evento è sempre un riferimento all'elemento su cui si è verificato l'evento. Quindi, in questo esempio, stiamo impostando un colore di sfondo casuale sul pulsante, non sulla pagina.

> [!NOTE]
> Può usare qualsiasi nome le piaccia per l'oggetto evento — deve solo scegliere un nome che può poi usare per riferirsi ad esso all'interno della funzione gestore di eventi. `e`/`evt`/`event` è più comunemente usato dagli sviluppatori perché sono corti e facili da ricordare. È sempre bene essere coerenti — con se stessi e con gli altri, se possibile.

### Proprietà aggiuntive degli oggetti evento

La maggior parte degli oggetti evento ha un insieme standard di proprietà e metodi disponibili sull'oggetto evento; consulti il riferimento all'oggetto [`Event`](/it/docs/Web/API/Event) per un elenco completo.

Alcuni oggetti evento aggiungono proprietà extra che sono pertinenti a quel particolare tipo di evento. Ad esempio, l'evento [`keydown`](/it/docs/Web/API/Element/keydown_event) viene emesso quando l'utente preme un tasto. Il suo oggetto evento è un [`KeyboardEvent`](/it/docs/Web/API/KeyboardEvent), che è un oggetto specializzato `Event` con una proprietà `key` che le dice quale tasto è stato premuto:

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

Provi a digitare nella casella di testo e veda l'output:

{{EmbedLiveSample("Extra_properties_of_event_objects", 100, 100)}}

## Prevenire il comportamento predefinito

A volte, si troverà in una situazione in cui vuole impedire a un evento di fare quello che fa di default. L'esempio più comune è quello di un modulo web, ad esempio, un modulo di registrazione personalizzato. Quando compila i dettagli e clicca sul pulsante di invio, il comportamento naturale è che i dati vengano inviati a una pagina specificata sul server per l'elaborazione, e il browser venga reindirizzato a una pagina di "messaggio di successo" di qualche tipo (o alla stessa pagina, se un'altra non è specificata).

Il problema si presenta quando l'utente non ha inviato i dati correttamente — come sviluppatore, vuole impedire l'invio al server e dare un messaggio di errore che dice cosa non va e cosa bisogna fare per mettere le cose a posto. Alcuni browser supportano funzionalità di convalida automatica dei dati del modulo, ma poiché molti non lo fanno, è consigliato non fare affidamento su queste e implementare i propri controlli di validazione. Vediamo un esempio.

Prima, un modulo HTML semplice che richiede di inserire il proprio nome e cognome:

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

Ora un po' di JavaScript — qui implementiamo un controllo molto semplice all'interno di un gestore per l'evento [`submit`](/it/docs/Web/API/HTMLFormElement/submit_event) (l'evento submit viene emesso su un modulo quando viene inviato) che testa se i campi di testo sono vuoti. Se lo sono, chiamiamo la funzione [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento — che interrompe l'invio del modulo — e poi visualizziamo un messaggio di errore nel paragrafo sotto al nostro modulo per dire all'utente cosa non va:

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

Ovviamente, questa è una validazione del modulo piuttosto debole — non impedirebbe all'utente di convalidare il modulo con spazi o numeri inseriti nei campi, ad esempio — ma va bene per scopi di esempio. L'output è il seguente:

{{ EmbedLiveSample('Preventing_default_behavior', '100%', 180, "", "") }}

> [!NOTE]
> Per il codice sorgente completo, vedi [preventdefault-validation.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/preventdefault-validation.html) (vedilo anche [in esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/events/preventdefault-validation.html) qui).

## Non si tratta solo di pagine web

Gli eventi non sono unici per JavaScript — la maggior parte dei linguaggi di programmazione ha qualche tipo di modello di eventi, e il modo in cui il modello funziona spesso differisce dal modo in cui funziona JavaScript. In effetti, il modello di eventi in JavaScript per le pagine web differisce dal modello di eventi per JavaScript come viene usato in altri ambienti.

Per esempio, [Node.js](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs) è un runtime JavaScript molto popolare che consente agli sviluppatori di usare JavaScript per costruire applicazioni di rete e lato server. Il [modello di eventi Node.js](https://nodejs.org/api/events.html) si basa su listener per ascoltare gli eventi ed emettitori per emettere eventi periodicamente — non sembra molto diverso, ma il codice è abbastanza diverso, utilizzando funzioni come `on()` per registrare un listener di eventi e `once()` per registrare un listener di eventi che si deregistra dopo essere stato eseguito una volta. I [documenti dello HTTP connect event](https://nodejs.org/api/http.html#event-connect) forniscono un buon esempio.

Può anche utilizzare JavaScript per costruire add-on cross-browser — miglioramenti della funzionalità del browser — utilizzando una tecnologia chiamata [WebExtensions](/it/docs/Mozilla/Add-ons/WebExtensions). Il modello di eventi è simile al modello degli eventi web, ma un po' diverso — le proprietà dei listener di eventi sono scritte in {{Glossary("camel_case", "camel case")}} (come `onMessage` piuttosto che `onmessage`), e devono essere combinate con la funzione `addListener`. Veda la pagina [`runtime.onMessage`](/it/docs/Mozilla/Add-ons/WebExtensions/API/runtime/onMessage#examples) per un esempio.

Non ha bisogno di comprendere nulla riguardo ad altri ambienti a questo stadio del suo apprendimento; volevamo solo chiarire che gli eventi possono differire nei diversi ambienti di programmazione.

## Sommario

In questo capitolo abbiamo appreso cosa sono gli eventi, come ascoltarli e come rispondere ad essi.

Avrà visto ormai che gli elementi in una pagina web possono essere nidificati all'interno di altri elementi. Ad esempio, nell'esempio [Prevenire il comportamento predefinito](#prevenire_il_comportamento_predefinito), abbiamo alcune caselle di testo, collocate all'interno di elementi {{htmlelement("div")}}, che a loro volta sono collocati all'interno di un elemento {{htmlelement("form")}}. Che cosa succede quando un listener di eventi click è collegato all'elemento `<form>`, e l'utente clicca all'interno di una delle caselle di testo? La funzione del gestore di eventi associato viene comunque eseguita tramite un processo chiamato _event bubbling_, che verrà trattato nella prossima lezione.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Return_values","Learn_web_development/Core/Scripting/Event_bubbling", "Learn_web_development/Core/Scripting")}}
