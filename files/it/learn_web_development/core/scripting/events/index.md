---
title: Introduzione agli eventi
short-title: Events
slug: Learn_web_development/Core/Scripting/Events
l10n:
  sourceCommit: 2b4a2ad5d9ba084a9eaa2f9204102655e7b575c4
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Functions","Learn_web_development/Core/Scripting/Event_bubbling", "Learn_web_development/Core/Scripting")}}

Gli eventi sono cose che accadono nel sistema che si sta programmando, di cui il sistema informa in modo che il codice possa reagire.
Ad esempio, se l'utente fa clic su un pulsante in una pagina web, potrebbe essere necessario reagire a quell'azione visualizzando una casella informativa.
In questo articolo vengono discussi alcuni concetti importanti relativi agli eventi e vengono illustrate le basi del loro funzionamento nei browser.

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
          <li>Cosa sono gli eventi: un segnale emesso dal browser quando accade qualcosa di significativo, a cui lo sviluppatore può reagire eseguendo del codice.</li>
          <li>Impostare event handler usando <code>addEventListener()</code> (e <code>removeEventListener()</code>) e le proprietà degli event handler.</li>
          <li>Attributi inline degli event handler e perché non dovrebbero essere usati.</li>
          <li>Oggetti evento.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è un evento?

Gli eventi sono cose che accadono nel sistema che si sta programmando: il sistema produce (o "emette") un qualche tipo di segnale quando si verifica un evento e fornisce un meccanismo tramite cui un'azione può essere eseguita automaticamente (ovvero, del codice viene eseguito) quando l'evento si verifica.
Gli eventi vengono emessi all'interno della finestra del browser e tendono a essere associati a un elemento specifico presente in essa. Potrebbe trattarsi di un singolo elemento, di un insieme di elementi, del documento HTML caricato nella scheda corrente o dell'intera finestra del browser.
Esistono molti tipi diversi di eventi che possono verificarsi.

Ad esempio:

- L'utente seleziona un determinato elemento, fa clic su di esso o vi passa sopra il cursore.
- L'utente preme un tasto sulla tastiera.
- L'utente ridimensiona o chiude la finestra del browser.
- Una pagina web termina il caricamento.
- Viene inviato un modulo.
- Un video viene riprodotto, messo in pausa o termina.
- Si verifica un errore.

Da questo elenco (e dando un'occhiata all'[indice degli eventi](/it/docs/Web/API/Document_Object_Model/Events#event_index)) si può capire che esistono **moltissimi** eventi che possono essere emessi.

Per reagire a un evento, si associa a esso un **event listener**. Si tratta di una funzionalità del codice che rimane in ascolto dell'emissione dell'evento. Quando l'evento viene emesso, viene chiamata una funzione **event handler** (a cui l'event listener fa riferimento o che è contenuta al suo interno) per reagire all'emissione dell'evento. Quando un blocco di codice viene configurato per essere eseguito in risposta a un evento, si dice che viene **registrato un event handler**.

### Un esempio: gestire un evento di clic

Nell'esempio seguente, nella pagina è presente un solo {{htmlelement("button")}}:

```html
<button>Change color</button>
```

```css hidden
button {
  margin: 10px;
}
```

È quindi presente del JavaScript. Questo verrà esaminato più nel dettaglio nella sezione successiva, ma per ora è sufficiente dire che aggiunge un event listener all'evento `"click"` del pulsante e che la funzione handler contenuta reagisce all'evento impostando lo sfondo della pagina su un colore casuale:

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

L'output dell'esempio è il seguente. Provare a fare clic sul pulsante:

{{ EmbedLiveSample('An example: handling a click event', '100%', 200, "", "") }}

## Usare addEventListener()

Come visto nell'ultimo esempio, gli oggetti che possono emettere eventi dispongono di un metodo [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener), che è il meccanismo consigliato per aggiungere event listener.

Esaminiamo più da vicino il codice dell'ultimo esempio:

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

L'elemento HTML {{HTMLElement("button")}} emette un evento `click` quando l'utente vi fa clic. Su di esso viene chiamato il metodo `addEventListener()` per aggiungere un event listener; questo accetta due parametri:

- la stringa `"click"`, per indicare che si desidera ascoltare l'evento `click`. I pulsanti possono emettere molti altri eventi, come [`"mouseover"`](/it/docs/Web/API/Element/mouseover_event) quando l'utente sposta il mouse sopra il pulsante, oppure [`"keydown"`](/it/docs/Web/API/Element/keydown_event) quando l'utente preme un tasto e il pulsante ha il focus.
- una funzione da chiamare quando si verifica l'evento. In questo caso, la funzione anonima definita genera un colore RGB casuale e imposta la {{cssxref("background-color")}} del [`<body>`](/it/docs/Web/HTML/Reference/Elements/body) della pagina su quel colore.

Si potrebbe anche creare una funzione nominata separata e farvi riferimento nel secondo parametro di `addEventListener()`, in questo modo:

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

Esistono molti eventi diversi che possono essere emessi da un elemento pulsante. Facciamo qualche esperimento.

Innanzitutto, creare una copia locale di [random-color-addeventlistener.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/random-color-addeventlistener.html) e aprirla nel browser.
Si tratta solo di una copia del semplice esempio del colore casuale già usato. Ora provare a sostituire `click`, a turno, con i seguenti valori diversi e osservare i risultati nell'esempio:

- [`focus`](/it/docs/Web/API/Element/focus_event) e [`blur`](/it/docs/Web/API/Element/blur_event): il colore cambia quando il pulsante riceve e perde il focus; provare a premere Tab per portare il focus sul pulsante e premere di nuovo Tab per spostare il focus dal pulsante.
  Questi eventi vengono spesso usati per visualizzare informazioni sulla compilazione dei campi di un modulo quando ricevono il focus, oppure per mostrare un messaggio di errore se un campo del modulo viene compilato con un valore non corretto.
- [`dblclick`](/it/docs/Web/API/Element/dblclick_event): il colore cambia solo quando si fa doppio clic sul pulsante.
- [`mouseover`](/it/docs/Web/API/Element/mouseover_event) e [`mouseout`](/it/docs/Web/API/Element/mouseout_event): il colore cambia quando il puntatore del mouse passa sopra il pulsante o quando il puntatore esce dal pulsante, rispettivamente.

Alcuni eventi, come `click`, sono disponibili su quasi tutti gli elementi. Altri sono più specifici e utili solo in determinate situazioni: ad esempio, l'evento [`play`](/it/docs/Web/API/HTMLMediaElement/play_event) è disponibile solo sugli elementi che dispongono di funzionalità di riproduzione, come {{htmlelement("video")}}.

### Rimuovere i listener

Se è stato aggiunto un event listener usando `addEventListener()`, è possibile rimuoverlo se necessario. Il modo più comune per farlo è usare il metodo [`removeEventListener()`](/it/docs/Web/API/EventTarget/removeEventListener). Ad esempio, la riga seguente rimuoverebbe l'event handler `click` visto in precedenza:

```js
btn.removeEventListener("click", changeBackground);
```

Per programmi semplici e di piccole dimensioni non è necessario ripulire i vecchi event handler non usati, ma per programmi più grandi e complessi questo può migliorare l'efficienza.
Inoltre, la possibilità di rimuovere gli event handler consente di far eseguire allo stesso pulsante azioni diverse in circostanze diverse: è sufficiente aggiungere o rimuovere gli handler.

### Aggiungere più listener per un singolo evento

Effettuando più di una chiamata a [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) e fornendo handler diversi, è possibile avere più funzioni handler eseguite in risposta a un singolo evento:

```js
myElement.addEventListener("click", functionA);
myElement.addEventListener("click", functionB);
```

Entrambe le funzioni verrebbero ora eseguite quando si fa clic sull'elemento.

## Altri meccanismi per gli event listener

Si consiglia di usare `addEventListener()` per registrare gli event handler. È il metodo più potente e quello che si adatta meglio ai programmi più complessi. Tuttavia, esistono altri due modi per registrare gli event handler che potrebbero essere incontrati: le _proprietà degli event handler_ e gli _event handler inline_.

### Proprietà degli event handler

Gli oggetti (come i pulsanti) che possono emettere eventi dispongono solitamente anche di proprietà il cui nome è formato da `on` seguito dal nome di un evento. Ad esempio, gli elementi dispongono di una proprietà `onclick`.
Questa viene chiamata **proprietà dell'event handler**. Per ascoltare l'evento, è possibile assegnare la funzione handler alla proprietà.

Ad esempio, l'esempio del colore casuale potrebbe essere riscritto in questo modo:

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

È anche possibile impostare la proprietà handler su una funzione nominata:

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

Le proprietà degli event handler presentano svantaggi rispetto a `addEventListener()`. Uno dei più significativi è che non è possibile [aggiungere più di un listener per un singolo evento](#aggiungere_più_listener_per_un_singolo_evento). Il seguente schema non funziona, perché ogni tentativo successivo di impostare il valore della proprietà sovrascrive quelli precedenti:

```js
element.onclick = function1;
element.onclick = function2;
```

### Event handler inline — non usarli

Nel codice potrebbe anche comparire uno schema come questo:

```html example-bad
<button onclick="bgChange()">Press me</button>
```

```js
function bgChange() {
  const rndCol = `rgb(${random(255)} ${random(255)} ${random(255)})`;
  document.body.style.backgroundColor = rndCol;
}
```

Il primo metodo per registrare gli event handler comparso sul Web prevedeva [_attributi HTML degli event handler_](/it/docs/Web/HTML/Reference/Attributes#event_handler_attributes) (o _event handler inline_) come quello mostrato sopra: il valore dell'attributo contiene il codice JavaScript da eseguire quando si verifica l'evento.
L'esempio precedente invoca una funzione definita all'interno di un elemento {{htmlelement("script")}} nella stessa pagina, ma è anche possibile inserire JavaScript direttamente nell'attributo, ad esempio:

```html example-bad
<button onclick="alert('Hello, this is my old-fashioned event handler!');">
  Press me
</button>
```

È possibile trovare equivalenti negli attributi HTML per molte proprietà degli event handler; tuttavia, non dovrebbero essere usati, poiché sono considerati una cattiva pratica.
Potrebbe sembrare semplice usare un attributo event handler per qualcosa di molto rapido, ma diventano presto difficili da gestire e inefficienti.

Per cominciare, non è una buona idea mescolare HTML e JavaScript, perché il codice diventa difficile da leggere. Mantenere separato il JavaScript è una buona pratica e, se si trova in un file separato, può essere applicato a più documenti HTML.

Anche in un singolo file, gli event handler inline non sono una buona idea.
Un pulsante va bene, ma cosa succederebbe con 100 pulsanti? Sarebbe necessario aggiungere 100 attributi al file e la manutenzione diventerebbe presto un incubo.
Con JavaScript, è possibile aggiungere facilmente una funzione event handler a tutti i pulsanti della pagina, indipendentemente dal loro numero, usando qualcosa come questo:

```js
const buttons = document.querySelectorAll("button");

for (const button of buttons) {
  button.addEventListener("click", bgChange);
}
```

Infine, molte configurazioni comuni dei server non consentono JavaScript inline come misura di sicurezza.

**Non dovrebbero mai essere usati gli attributi HTML degli event handler**: sono obsoleti e il loro utilizzo è una cattiva pratica.

## Oggetti evento

A volte, all'interno di una funzione event handler, è presente un parametro specificato con un nome come `event`, `evt` o `e`.
Questo è chiamato **oggetto evento** e viene passato automaticamente agli event handler per fornire funzionalità e informazioni aggiuntive.
Ad esempio, riscriviamo l'esempio del colore casuale per includere un oggetto evento:

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
> Il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/random-color-eventobject.html) di questo esempio è disponibile su GitHub (è anche possibile [vederlo in esecuzione](https://mdn.github.io/learning-area/javascript/building-blocks/events/random-color-eventobject.html)).

Qui si può vedere che viene incluso un oggetto evento, **e**, nella funzione e che nella funzione viene impostato uno stile di colore di sfondo su `e.target`, ovvero il pulsante stesso.
La proprietà `target` dell'oggetto evento è sempre un riferimento all'elemento su cui si è verificato l'evento.
Quindi, in questo esempio, viene impostato un colore di sfondo casuale sul pulsante, non sulla pagina.

> [!NOTE]
> È possibile usare qualsiasi nome per l'oggetto evento: basta scegliere un nome a cui sia possibile fare riferimento all'interno della funzione event handler.
> `e`, `evt` ed `event` sono comunemente usati dagli sviluppatori perché sono brevi e facili da ricordare.
> È sempre bene essere coerenti, con sé stessi e, se possibile, con gli altri.

### Proprietà aggiuntive degli oggetti evento

La maggior parte degli oggetti evento dispone di un insieme standard di proprietà e metodi disponibili; per un elenco completo, vedere il riferimento dell'oggetto [`Event`](/it/docs/Web/API/Event).

Alcuni oggetti evento aggiungono proprietà extra pertinenti a quel particolare tipo di evento. Ad esempio, l'evento [`keydown`](/it/docs/Web/API/Element/keydown_event) viene emesso quando l'utente preme un tasto. Il suo oggetto evento è un [`KeyboardEvent`](/it/docs/Web/API/KeyboardEvent), ovvero un oggetto `Event` specializzato con una proprietà `key` che indica quale tasto è stato premuto:

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

Provare a digitare nella casella di testo e osservare l'output:

{{EmbedLiveSample("Extra_properties_of_event_objects", 100, 100)}}

## Impedire il comportamento predefinito

A volte si incontrerà una situazione in cui è necessario impedire a un evento di eseguire il suo comportamento predefinito.
L'esempio più comune è quello di un modulo web, ad esempio un modulo di registrazione personalizzato.
Quando vengono compilati i dati e si fa clic sul pulsante di invio, il comportamento naturale consiste nell'inviare i dati a una pagina specificata sul server per l'elaborazione e nel reindirizzare il browser a una sorta di pagina con un "messaggio di successo" (o alla stessa pagina, se non ne viene specificata un'altra).

Il problema si presenta quando l'utente non ha inviato correttamente i dati: come sviluppatore, è necessario impedire l'invio al server e fornire un messaggio di errore che indichi cosa non va e cosa occorre fare per correggerlo.
Alcuni browser supportano funzionalità di convalida automatica dei dati dei moduli, ma poiché molti non le supportano, si consiglia di non fare affidamento su di esse e di implementare controlli di convalida propri.
Vediamo un esempio.

Innanzitutto, un semplice modulo HTML che richiede l'inserimento di nome e cognome:

```html
<form action="#">
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

Ora un po' di JavaScript: qui viene implementato un controllo di base all'interno di un handler per l'evento [`submit`](/it/docs/Web/API/HTMLFormElement/submit_event) (l'evento di invio viene emesso su un modulo quando viene inviato) che verifica se i campi di testo sono vuoti.
Se lo sono, viene chiamata la funzione [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) sull'oggetto evento, che interrompe l'invio del modulo, quindi viene visualizzato un messaggio di errore nel paragrafo sotto il modulo per indicare all'utente cosa non va:

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

Ovviamente, si tratta di una convalida del modulo piuttosto debole: non impedirebbe all'utente di convalidare il modulo inserendo, ad esempio, spazi o numeri nei campi, ma è adeguata per scopi dimostrativi.

È possibile vedere l'esempio completo [in esecuzione](https://mdn.github.io/learning-area/javascript/building-blocks/events/preventdefault-validation.html): provarlo direttamente lì. Per il codice sorgente completo, vedere [preventdefault-validation.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/events/preventdefault-validation.html).

## Non si tratta solo di pagine web

Gli eventi non sono esclusivi di JavaScript: la maggior parte dei linguaggi di programmazione dispone di qualche tipo di modello di eventi e il funzionamento del modello spesso differisce da quello di JavaScript.
In effetti, il modello di eventi in JavaScript per le pagine web differisce dal modello di eventi per JavaScript usato in altri ambienti.

Ad esempio, [Node.js](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs) è un runtime JavaScript molto diffuso che consente agli sviluppatori di usare JavaScript per creare applicazioni di rete e lato server.
Il [modello di eventi di Node.js](https://nodejs.org/api/events.html) si basa su listener che ascoltano gli eventi ed emitter che emettono eventi periodicamente: non sembra molto diverso, ma il codice è piuttosto differente e usa funzioni come `on()` per registrare un event listener e `once()` per registrare un event listener che viene annullato dopo essere stato eseguito una volta.
La documentazione dell'[evento HTTP connect di Node.js](https://nodejs.org/api/http.html#event-connect) fornisce un buon esempio.

È inoltre possibile usare JavaScript per creare add-on multipiattaforma per browser, ovvero miglioramenti delle funzionalità del browser, usando una tecnologia chiamata [WebExtensions](/it/docs/Mozilla/Add-ons/WebExtensions).
Il modello di eventi è simile al modello degli eventi web, ma leggermente diverso: le proprietà degli event listener sono scritte in {{Glossary("camel_case", "camel case")}} (come `onMessage` anziché `onmessage`) e devono essere combinate con la funzione `addListener`.
Vedere la pagina [`runtime.onMessage`](/it/docs/Mozilla/Add-ons/WebExtensions/API/runtime/onMessage#examples) per un esempio.

In questa fase dell'apprendimento non è necessario comprendere nulla di questi altri ambienti; l'obiettivo è semplicemente chiarire che gli eventi possono differire nei diversi ambienti di programmazione.

## Riepilogo

In questo capitolo sono stati appresi cosa sono gli eventi, come ascoltarli e come reagire a essi.

Come già visto, gli elementi di una pagina web possono essere annidati all'interno di altri elementi. Ad esempio, nell'esempio [Impedire il comportamento predefinito](#impedire_il_comportamento_predefinito), sono presenti alcune caselle di testo poste all'interno di elementi {{htmlelement("div")}}, che a loro volta sono inseriti in un elemento {{htmlelement("form")}}. Cosa accade quando viene associato un event listener `click` all'elemento `<form>` e l'utente fa clic all'interno di una delle caselle di testo? La funzione event handler associata viene comunque emessa tramite un processo chiamato _event bubbling_, trattato nella lezione successiva.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Functions","Learn_web_development/Core/Scripting/Event_bubbling", "Learn_web_development/Core/Scripting")}}
