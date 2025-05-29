---
title: Sintassi HTML di base
slug: Learn_web_development/Core/Structuring_content/Basic_HTML_syntax
l10n:
  sourceCommit: cb25e0acbd9f0af27c4a99965cb962230d49a35d
---

{{NextMenu("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content")}}

In questo articolo, trattiamo le basi assolute dell'HTML. Per iniziare, questo articolo definisce elementi, attributi e tutti gli altri termini importanti che potresti aver sentito. Spiega anche dove si inseriscono nell'HTML. Imparerai come sono strutturati gli elementi HTML, come è strutturata una tipica pagina HTML e altre caratteristiche fondamentali del linguaggio. Durante il percorso, ci sarà anche l'opportunità di sperimentare con l'HTML!

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software">Software di base installato</a> e conoscenza di base di <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files">come lavorare con i file</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Anatomia di un elemento HTML: elemento, tag di apertura, contenuto, tag di chiusura, attributi.</li>
          <li>Il corpo HTML e il suo scopo come contenitore per il contenuto della pagina.</li>
          <li>Cosa sono gli <a href="/it/docs/Glossary/Void_element">elementi void</a> (noti anche come elementi vuoti) e come differiscono dagli altri elementi.</li>
          <li>La necessità di un doctype in cima ai documenti HTML, il suo scopo originario e il fatto che ora è un artefatto storico.</li>
          <li>Comprendere che l'HTML deve essere annidato correttamente.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è l'HTML?

{{Glossary("HTML", "HTML")}} (HyperText Markup Language) è un _linguaggio di markup_ che indica ai browser web come strutturare le pagine web che visiti. Può essere complicato o semplice quanto lo sviluppatore desidera. L'HTML è composto da una serie di {{Glossary("Element", "elementi")}}, che utilizzi per racchiudere, avvolgere o _marcare_ diverse parti del contenuto per farle apparire o agire in un certo modo. I {{Glossary("Tag", "tag")}} di chiusura possono trasformare il contenuto in un collegamento ipertestuale per connettersi a un'altra pagina, rendere le parole in corsivo, e così via. Ad esempio, considera la seguente riga di testo:

```plain
My cat is very grumpy
```

Se volessimo che il testo si distinguesse da solo, potremmo specificare che è un paragrafo racchiudendolo in un elemento paragrafo ({{htmlelement("p")}}):

```html
<p>My cat is very grumpy</p>
```

L'HTML risiede all'interno di file di testo chiamati **documenti HTML**, o semplicemente **documenti**, con un'estensione di file `.html`. Precedentemente abbiamo parlato di pagine web: un documento HTML contiene il contenuto della pagina web e ne specifica la struttura.

Il file HTML più comune che incontrerai è `index.html`, generalmente utilizzato per contenere il contenuto della home page di un sito web. È anche comune vedere sottocartelle con il proprio `index.html`, così un sito web può avere più file index in posti diversi.

> [!NOTE]
> I tag in HTML non sono case-sensitive. Ciò significa che possono essere scritti in maiuscolo o minuscolo. Ad esempio, un tag {{htmlelement("title")}} potrebbe essere scritto come `<title>`, `<TITLE>`, `<Title>`, `<TiTlE>`, ecc., e funzionerà. Tuttavia, è buona pratica scrivere tutti i tag in minuscolo per coerenza e leggibilità.

## Anatomia di un elemento HTML

Esploriamo ulteriormente il nostro elemento paragrafo dalla sezione precedente:

![Uno snippet di codice di esempio che dimostra la struttura di un elemento html.<p> Il mio gatto è molto scontroso </p>.](grumpy-cat-small.png)

L'anatomia del nostro elemento è:

- **Il tag di apertura:** Questo consiste nel nome dell'elemento (in questo esempio, _p_ per paragrafo), racchiuso tra parentesi angolari. Questo tag di apertura segna dove l'elemento inizia o inizia a produrre effetto. In questo esempio, precede l'inizio del testo del paragrafo.
- **Il contenuto:** Questo è il contenuto dell'elemento. In questo esempio, è il testo del paragrafo.
- **Il tag di chiusura:** Questo è lo stesso del tag di apertura, tranne per il fatto che include una barra obliqua prima del nome dell'elemento. Questo segna dove l'elemento termina. Non includere un tag di chiusura è un errore comune per i principianti che può produrre risultati strani.

L'elemento è costituito dal tag di apertura, seguito dal contenuto, seguito dal tag di chiusura.

> [!NOTE]
> Vai dal nostro partner di apprendimento Scrimba per una [spiegazione interattiva dei tag HTML](https://scrimba.com/learn-html-and-css-c0p/~02?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>.

### Apprendimento attivo: creare il tuo primo elemento HTML

Modifica la riga qui sotto nell'area "Codice modificabile" racchiudendola con i tag `<em>` e `</em>`. Per _aprire l'elemento_, inserisci il tag di apertura `<em>` all'inizio della riga. Per _chiudere l'elemento_, metti il tag di chiusura `</em>` alla fine della riga. Facendo ciò, il testo dovrebbe essere formattato in corsivo! Guarda i tuoi cambiamenti aggiornarsi dal vivo nell'area _Output_.

Se commetti un errore, puoi cancellare il tuo lavoro utilizzando il pulsante _Reset_. Se ti blocchi seriamente, premi il pulsante _Mostra soluzione_ per vedere la risposta.

```html hidden
<h2>Live output</h2>
<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="playable-code" style="min-height: 100px;width: 95%">
  This is my text.
</textarea>

<div class="controls">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css hidden
html {
  font-family: "Open Sans Light", Helvetica, Arial, sans-serif;
}

h2 {
  font-size: 16px;
}

.a11y-label {
  margin: 0;
  text-align: right;
  font-size: 0.7rem;
  width: 98%;
}

body {
  margin: 10px;
  background: #f5f9fa;
}
```

```js hidden
const textarea = document.getElementById("code");
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
const output = document.querySelector(".output");
const code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  output.innerHTML = textarea.value;
}

const htmlSolution = "<em>This is my text.</em>";
let solutionEntry = htmlSolution;

reset.addEventListener("click", () => {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = htmlSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", () => {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead
textarea.onkeydown = (e) => {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
    textarea.blur();
  }
};

function insertAtCaret(text) {
  const scrollPos = textarea.scrollTop;
  let caretPos = textarea.selectionStart;

  const front = textarea.value.substring(0, caretPos);
  const back = textarea.value.substring(
    textarea.selectionEnd,
    textarea.value.length,
  );
  textarea.value = front + text + back;
  caretPos += text.length;
  textarea.selectionStart = caretPos;
  textarea.selectionEnd = caretPos;
  textarea.focus();
  textarea.scrollTop = scrollPos;
}

// Update the saved userCode every time the user updates the text area code

textarea.onkeyup = () => {
  // We only want to save the state when the user code is being shown,
  // not the solution, so that solution is not saved over the user code
  if (solution.value === "Show solution") {
    userEntry = textarea.value;
  } else {
    solutionEntry = textarea.value;
  }

  updateCode();
};
```

{{ EmbedLiveSample('Active_learning_creating_your_first_HTML_element', 700, 400, "", "") }}

### Annidamento degli elementi

Gli elementi possono essere inseriti all'interno di altri elementi. Questo è chiamato _annidamento_. Se volessimo affermare che il nostro gatto è **molto** scontroso, potremmo avvolgere la parola _molto_ in un elemento {{htmlelement("strong")}}, che significa che la parola deve avere una formattazione del testo più forte:

```html
<p>My cat is <strong>very</strong> grumpy.</p>
```

C'è un modo giusto e sbagliato di fare l'annidamento. Nell'esempio sopra, abbiamo aperto l'elemento `p` per primo, poi abbiamo aperto l'elemento `strong`. Per un annidamento corretto, dovremmo chiudere l'elemento `strong` prima, prima di chiudere il `p`.

Il seguente è un esempio del modo _sbagliato_ di fare l'annidamento:

```html-nolint example-bad
<p>My cat is <strong>very grumpy.</p></strong>
```

**I tag devono aprirsi e chiudersi in modo che siano dentro o fuori l'uno dall'altro.** Con il tipo di sovrapposizione nell'esempio sopra, il browser deve indovinare la tua intenzione. Questo tipo di supposizione può portare a risultati inaspettati.

### Elementi void

Non tutti gli elementi seguono il modello di un tag di apertura, contenuto e un tag di chiusura. Alcuni elementi consistono in un singolo tag, che viene tipicamente usato per inserire/incorporare qualcosa nel documento. Tali elementi sono chiamati {{Glossary("void_element", "elementi void")}}. Ad esempio, l'elemento {{htmlelement("img")}} incorpora un file immagine su una pagina:

```html
<img
  src="https://raw.githubusercontent.com/mdn/beginner-html-site/gh-pages/images/firefox-icon.png"
  alt="Firefox icon" />
```

Questo produrrebbe il seguente output:

{{ EmbedLiveSample('Void_elements', 700, 300, "", "") }}

> [!NOTE]
> In HTML, non è necessario aggiungere una barra `/` alla fine del tag di un elemento void, per esempio: `<img src="images/cat.jpg" alt="cat" />`. Tuttavia, è anche una sintassi valida, e puoi farlo quando desideri che il tuo HTML sia un XML valido.

## Attributi

Gli elementi possono anche avere attributi. Gli attributi appaiono così:

![tag paragrafo con attributo 'class="editor-note"' evidenziato](grumpy-cat-attribute-small.png)

Gli attributi contengono informazioni aggiuntive sull'elemento che non appariranno nel contenuto. In questo esempio, l'attributo **`class`** è un nome identificativo utilizzato per indirizzare l'elemento con informazioni di stile.

Un attributo dovrebbe avere:

- Uno spazio tra esso e il nome dell'elemento. (Per un elemento con più di un attributo, gli attributi dovrebbero essere separati da spazi pure.)
- Il nome dell'attributo, seguito da un segno di uguale.
- Un valore di attributo, racchiuso tra virgolette di apertura e chiusura.

### Apprendimento attivo: aggiungere attributi a un elemento

L'elemento `<img>` può prendere un numero di attributi, inclusi:

- `src`
  - : L'attributo `src` è un attributo **obbligatorio** che specifica la posizione dell'immagine. Ad esempio: `src="https://raw.githubusercontent.com/mdn/beginner-html-site/gh-pages/images/firefox-icon.png"`.
- `alt`
  - : L'attributo `alt` specifica una descrizione testuale dell'immagine. Ad esempio: `alt="L'icona di Firefox"`.
- `width`
  - : L'attributo `width` specifica la larghezza dell'immagine con l'unità in pixel. Ad esempio: `width="300"`.
- `height`
  - : L'attributo `height` specifica l'altezza dell'immagine con l'unità in pixel. Ad esempio: `height="300"`.

Modifica la riga qui sotto nell'area _Input_ per trasformarla in un'immagine.

1. Trova la tua immagine preferita online, cliccaci sopra col tasto destro e premi _Copia link/indirizzo immagine_.
2. Torna nell'area sotto, aggiungi l'attributo `src` e riempilo con il link del passaggio 1.
3. Imposta l'attributo `alt`.
4. Aggiungi gli attributi `width` e `height`.

Sarai in grado di vedere i tuoi cambiamenti dal vivo nell'area _Output_.

Se commetti un errore, puoi sempre ripristinarlo utilizzando il pulsante _Reset_. Se ti blocchi seriamente, premi il pulsante _Mostra soluzione_ per vedere la risposta.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="input" style="min-height: 100px;width: 95%">
&lt;img alt="I should be an image" &gt;
</textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css hidden
html {
  font-family: sans-serif;
}

h2 {
  font-size: 16px;
}

.a11y-label {
  margin: 0;
  text-align: right;
  font-size: 0.7rem;
  width: 98%;
}

body {
  margin: 10px;
  background: #f5f9fa;
}
```

```js hidden
const textarea = document.getElementById("code");
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
const output = document.querySelector(".output");
const code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  output.innerHTML = textarea.value;
}

const htmlSolution =
  '<img src="https://raw.githubusercontent.com/mdn/beginner-html-site/gh-pages/images/firefox-icon.png" alt="Firefox icon" width="100" height="100" />';
let solutionEntry = htmlSolution;

reset.addEventListener("click", () => {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = htmlSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", () => {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = (e) => {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
    textarea.blur();
  }
};

function insertAtCaret(text) {
  const scrollPos = textarea.scrollTop;
  let caretPos = textarea.selectionStart;

  const front = textarea.value.substring(0, caretPos);
  const back = textarea.value.substring(
    textarea.selectionEnd,
    textarea.value.length,
  );
  textarea.value = front + text + back;
  caretPos += text.length;
  textarea.selectionStart = caretPos;
  textarea.selectionEnd = caretPos;
  textarea.focus();
  textarea.scrollTop = scrollPos;
}

// Update the saved userCode every time the user updates the text area code

textarea.onkeyup = () => {
  // We only want to save the state when the user code is being shown,
  // not the solution, so that solution is not saved over the user code
  if (solution.value === "Show solution") {
    userEntry = textarea.value;
  } else {
    solutionEntry = textarea.value;
  }

  updateCode();
};
```

{{ EmbedLiveSample('Active_learning_Adding_attributes_to_an_element', 700, 400, "", "") }}

### Attributi booleani

A volte vedrai attributi scritti senza valori. Questo è del tutto accettabile. Questi sono chiamati {{Glossary("Boolean/HTML", "attributi booleani")}}. Quando un attributo booleano è scritto senza valore, o con qualsiasi valore, anche come `"false"`, l'attributo booleano è sempre impostato su true. Altrimenti, se l'attributo non è scritto in un tag HTML, l'attributo è impostato su false. La specifica richiede che il valore dell'attributo sia una stringa vuota (incluso quando l'attributo non ha un valore esplicitamente specificato) o lo stesso nome dell'attributo, ma altri valori funzionano lo stesso. Ad esempio, considera l'attributo [`disabled`](/it/docs/Web/HTML/Reference/Elements/input#disabled), che puoi assegnare agli elementi di input del modulo. (Usi questo per _disattivare_ gli elementi di input del modulo in modo che l'utente non possa fare inserimenti. Gli elementi disabilitati hanno tipicamente un aspetto grigiato.) Ad esempio:

```html
<input type="text" disabled="disabled" />
```

Come scorciatoia, è accettabile scrivere questo come segue:

```html
<!-- using the disabled attribute prevents the end user from entering text into the input box -->
<input type="text" disabled />

<!-- text input is allowed, as it doesn't contain the disabled attribute -->
<input type="text" />
```

Per riferimento, l'esempio sopra include anche un elemento di input del modulo non disabilitato. L'HTML dell'esempio sopra produce questo risultato:

{{ EmbedLiveSample('Boolean_attributes', 700, 100, "", "") }}

### Omettere le virgolette attorno ai valori degli attributi

Se guardi il codice di molti altri siti, potresti incontrare un certo numero di stili di markup strani, inclusi valori degli attributi senza virgolette. Questo è permesso in alcune circostanze, ma può anche rompere il tuo markup in altre circostanze. L'elemento nel seguente snippet di codice, `<a>`, è chiamato ancora. Le ancore racchiudono il testo e lo trasformano in collegamenti. L'attributo `href` specifica l'indirizzo web a cui punta il collegamento. Puoi scrivere questa versione di base sotto con _solo_ l'attributo `href`, come segue:

```html
<a href=https://www.mozilla.org/>favorite website</a>
```

Le ancore possono anche avere un attributo `title`, una descrizione della pagina collegata. Tuttavia, non appena aggiungiamo il `title` nello stesso modo dell'attributo `href` ci sono problemi:

```html-nolint example-bad
<a href=https://www.mozilla.org/ title=The Mozilla homepage>favorite website</a>
```

Come scritto sopra, il browser interpreta erroneamente il markup, scambiando l'attributo `title` per tre attributi: un attributo title con valore `The`, e due attributi booleani, `Mozilla` e `homepage`. Ovviamente, questo non è l'intento! Questo causerà errori o comportamenti inaspettati, come puoi vedere nell'esempio live qui sotto. Prova a passare il mouse sopra il link per vedere il testo del titolo!

{{ EmbedLiveSample('Omitting_quotes_around_attribute_values', 700, 100, "", "") }}

Includi sempre le virgolette degli attributi. In questo modo si evitano tali problemi e si ottiene un codice più leggibile.

### Virgolette singole o doppie?

In questo articolo noterai anche che gli attributi sono racchiusi tra doppie virgolette. Tuttavia, potresti vedere virgolette singole in alcuni codici HTML. Questa è una questione di stile. Puoi scegliere quale preferisci. Entrambe queste righe sono equivalenti:

```html-nolint
<a href='https://www.example.com'>A link to my example.</a>

<a href="https://www.example.com">A link to my example.</a>
```

Assicurati di non mescolare virgolette singole e doppie. Questo esempio (sotto) mostra un tipo di mescolanza di virgolette che andrà storto:

```html-nolint example-bad
<a href="https://www.example.com'>A link to my example.</a>
```

Tuttavia, se utilizzi un tipo di virgolette, puoi includere l'altro tipo di virgolette _dentro_ i valori degli attributi:

```html
<a href="https://www.example.com" title="Isn't this fun?">
  A link to my example.
</a>
```

Per utilizzare i segni di citazione all'interno di altri segni di citazione dello stesso tipo (virgolette singole o doppie), utilizza i {{Glossary("character_reference", "riferimenti ai caratteri")}}.
Ad esempio, questo romperà:

```html-nolint example-bad
<a href="https://www.example.com" title="An "interesting" reference">A link to my example.</a>
```

Invece, devi fare questo:

```html-nolint
<a href="https://www.example.com" title="An &quot;interesting&quot; reference">A link to my example.</a>
```

## Anatomia di un documento HTML

Gli elementi HTML individuali non sono molto utili da soli. Passiamo ora a esaminare come gli elementi individuali si combinano per formare un'intera pagina HTML:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>My test page</title>
  </head>
  <body>
    <p>This is my page</p>
  </body>
</html>
```

Qui abbiamo:

1. `<!doctype html>`: Il doctype. Quando HTML era giovane (1991-1992), i doctypes erano pensati per agire come collegamenti a un insieme di regole che la pagina HTML doveva seguire per essere considerata buon HTML. I doctypes una volta sembravano qualcosa di simile:

   ```html
   <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
   ```

   Più recentemente, il doctype è un artefatto storico che deve essere incluso per far funzionare tutto il resto correttamente. `<!doctype html>` è la stringa più corta di caratteri che conta come un doctype valido. Questo è tutto ciò che devi sapere!

2. `<html></html>`: L'elemento {{htmlelement("html")}}. Questo elemento racchiude tutto il contenuto della pagina. È talvolta noto come elemento radice.
3. `<head></head>`: L'elemento {{htmlelement("head")}}. Questo elemento agisce come contenitore per tutto ciò che vuoi includere nella pagina HTML, **che non è il contenuto** che la pagina mostrerà agli spettatori. Ciò include parole chiave e una descrizione della pagina che apparirebbe nei risultati di ricerca, CSS per stilizzare il contenuto, dichiarazioni del set di caratteri e altro. Imparerai di più su questo nel prossimo articolo della serie.
4. `<meta charset="utf-8">`: L'elemento {{htmlelement("meta")}}. Questo elemento rappresenta i metadati che non possono essere rappresentati da altri elementi correlati ai metadati HTML, come {{htmlelement("base")}}, {{htmlelement("link")}}, {{htmlelement("script")}}, {{htmlelement("style")}} o {{htmlelement("title")}}. L'attributo [`charset`](/it/docs/Web/HTML/Reference/Elements/meta#charset) specifica la codifica dei caratteri per il tuo documento come UTF-8, che include la maggior parte dei caratteri della stragrande maggioranza delle lingue umane scritte. Con questa impostazione, la pagina può ora gestire qualsiasi contenuto testuale che potrebbe contenere. Non c'è motivo di non impostarlo, e può aiutare a evitare alcuni problemi in un secondo momento.
5. `<title></title>`: L'elemento {{htmlelement("title")}}. Questo imposta il titolo della pagina, che è il titolo che appare nella scheda del browser in cui la pagina è caricata. Il titolo della pagina è anche usato per descrivere la pagina quando viene aggiunta ai segnalibri.
6. `<body></body>`: L'elemento {{htmlelement("body")}}. Questo contiene _tutto_ il contenuto che appare sulla pagina, inclusi testi, immagini, video, giochi, tracce audio riproducibili o qualsiasi altra cosa.

### Apprendimento attivo: aggiungere alcune funzionalità a un documento HTML

Se vuoi sperimentare la scrittura di qualche HTML sul tuo computer locale, puoi:

1. Copia l'esempio di pagina HTML elencato sopra.
2. Crea un nuovo file nel tuo editor di testo.
3. Incolla il codice nel nuovo file di testo.
4. Salva il file come `index.html`.

> [!NOTE]
> Puoi anche trovare questo modello HTML di base nel [MDN Learning Area GitHub repo](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html).

Ora puoi aprire questo file in un browser web per vedere come appare il codice renderizzato. Modifica il codice e aggiorna il browser per vedere qual è il risultato. Inizialmente, la pagina appare così:

![Una semplice pagina HTML che dice Questa è la mia pagina](template-screenshot.png)

In questo esercizio, puoi modificare il codice localmente sul tuo computer, come descritto in precedenza, o puoi modificarlo nella finestra di esempio qui sotto (la finestra di esempio modificabile rappresenta solo i contenuti dell'elemento {{htmlelement("body")}}, in questo caso). Affina le tue abilità implementando le seguenti attività:

- Appena sotto il tag di apertura dell'elemento {{htmlelement("body")}}, aggiungi un titolo principale per il documento. Questo dovrebbe essere racchiuso in un tag di apertura `<h1>` e un tag di chiusura `</h1>`.
- Modifica il contenuto del paragrafo per includere un testo su un argomento che trovi interessante.
- Fai risaltare parole importanti in grassetto racchiudendole in un tag di apertura `<strong>` e un tag di chiusura `</strong>`.
- Aggiungi un collegamento al tuo paragrafo, come [spiegato prima nell'articolo](#active_learning_adding_attributes_to_an_element).
- Aggiungi un'immagine al tuo documento. Posizionala sotto il paragrafo, come [spiegato prima nell'articolo](#elementi_void). Guadagna punti bonus se riesci a collegarti a un'immagine diversa (sia localmente sul tuo computer che altrove sul web).

Se commetti un errore, puoi sempre ripristinarlo utilizzando il pulsante _Reset_. Se ti blocchi seriamente, premi il pulsante _Mostra soluzione_ per vedere la risposta.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="input" style="min-height: 100px;width: 95%">
  &lt;p&gt;This is my page&lt;/p&gt;
</textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css hidden
html {
  font-family: sans-serif;
}

h1 {
  color: blue;
}

h2 {
  font-size: 16px;
}

.a11y-label {
  margin: 0;
  text-align: right;
  font-size: 0.7rem;
  width: 98%;
}

img {
  max-width: 100%;
}

body {
  margin: 10px;
  background: #f5f9fa;
}
```

```js hidden
const textarea = document.getElementById("code");
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
const output = document.querySelector(".output");
const code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  output.innerHTML = textarea.value;
}

const htmlSolution =
  '<h1>Some music</h1><p>I really enjoy <strong>playing the drums</strong>. One of my favorite drummers is Neal Peart, who plays in the band <a href="https://en.wikipedia.org/wiki/Rush_%28band%29" title="Rush Wikipedia article">Rush</a>. My favorite Rush album is currently <a href="https://www.deezer.com/album/942295">Moving Pictures</a>.</p> <img src="https://www.cygnus-x1.net/links/rush/images/albums/sectors/sector2-movingpictures-cover-s.jpg" alt="Rush Moving Pictures album cover">';
let solutionEntry = htmlSolution;

reset.addEventListener("click", () => {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = htmlSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", () => {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = (e) => {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
    textarea.blur();
  }
};

function insertAtCaret(text) {
  const scrollPos = textarea.scrollTop;
  let caretPos = textarea.selectionStart;

  const front = textarea.value.substring(0, caretPos);
  const back = textarea.value.substring(
    textarea.selectionEnd,
    textarea.value.length,
  );
  textarea.value = front + text + back;
  caretPos += text.length;
  textarea.selectionStart = caretPos;
  textarea.selectionEnd = caretPos;
  textarea.focus();
  textarea.scrollTop = scrollPos;
}

// Update the saved userCode every time the user updates the text area code
textarea.onkeyup = () => {
  // We only want to save the state when the user code is being shown,
  // not the solution, so that solution is not saved over the user code
  if (solution.value === "Show solution") {
    userEntry = textarea.value;
  } else {
    solutionEntry = textarea.value;
  }

  updateCode();
};
```

{{ EmbedLiveSample('Active_learning_Adding_some_features_to_an_HTML_document', 700, 500) }}

### Spazi bianchi in HTML

Negli esempi sopra, potresti aver notato che molto spazio bianco è incluso nel codice. Questo è opzionale. Questi due frammenti di codice sono equivalenti:

```html-nolint
<p id="noWhitespace">Dogs are silly.</p>

<p id="whitespace">Dogs
    are
        silly.</p>
```

Non importa quanto spazio bianco usi all'interno del contenuto degli elementi HTML (che può includere uno o più caratteri di spazio, ma anche interruzioni di linea), l'analizzatore HTML riduce ogni sequenza di spazio bianco a un singolo spazio durante il rendering del codice. Quindi perché usare così tanto spazio bianco? La risposta è leggibilità.

Può essere più facile capire cosa sta succedendo nel tuo codice se lo hai formattato bene. Nel nostro HTML abbiamo ogni elemento annidato rientrato di due spazi in più rispetto a quello in cui è inserito. Dipende da te scegliere lo stile di formattazione (quanti spazi per ogni livello di rientro, ad esempio), ma dovresti considerare di formattarlo.

Diamo un'occhiata a come il browser rende i due paragrafi sopra con e senza spazio bianco:

{{ EmbedLiveSample('Whitespace_in_HTML', 700, 100) }}

> [!NOTE]
> Accedere al [innerHTML](/it/docs/Web/API/Element/innerHTML) degli elementi da JavaScript manterrà tutto lo spazio bianco intatto.
> Questo può restituire risultati inattesi se lo spazio bianco è ridotto dal browser.

```js
const noWhitespace = document.getElementById("noWhitespace").innerHTML;
console.log(noWhitespace);
// "Dogs are silly."

const whitespace = document.getElementById("whitespace").innerHTML;
console.log(whitespace);
// "Dogs
//    are
//        silly."
```

## Riferimenti ai caratteri: includere caratteri speciali in HTML

In HTML, i caratteri `<`, `>`, `"`, `'`, e `&` sono caratteri speciali. Fanno parte della sintassi HTML stessa. Quindi come includi uno di questi caratteri speciali nel tuo testo? Ad esempio, se vuoi usare un simbolo di ampersand o segno di minore, e non farlo interpretare come codice.

Puoi farlo con i {{Glossary("character_reference", "riferimenti ai caratteri")}}. Questi sono codici speciali che rappresentano i caratteri, da usare in queste esatte circostanze. Ogni riferimento a carattere inizia con una e commerciale (&) e termina con un punto e virgola (;).

| Carattere letterale | Equivalente del riferimento al carattere |
| ------------------- | ---------------------------------------- |
| <                   | `&lt;`                                   |
| >                   | `&gt;`                                   |
| "                   | `&quot;`                                 |
| '                   | `&apos;`                                 |
| &                   | `&amp;`                                  |

L'equivalente del riferimento al carattere potrebbe essere ricordato facilmente perché il testo che utilizza può essere visto come less than per `&lt;`, quotation per `&quot;` e similmente per gli altri. Per trovare ulteriori informazioni sui riferimenti delle entità, consulta [List of XML and HTML character entity references](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references) (Wikipedia).

Nell'esempio qui sotto, ci sono due paragrafi:

```html-nolint
<p>In HTML, you define a paragraph using the <p> element.</p>

<p>In HTML, you define a paragraph using the &lt;p&gt; element.</p>
```

Nell'output dal vivo qui sotto, puoi vedere che il primo paragrafo è andato storto. Il browser interpreta la seconda istanza di `<p>` come l'inizio di un nuovo paragrafo. Il secondo paragrafo sembra a posto perché ha le parentesi angolari con riferimenti ai caratteri.

{{ EmbedLiveSample('Entity_references_Including_special_characters_in_HTML', 700, 200, "", "") }}

> [!NOTE]
> Non è necessario usare riferimenti alle entità per nessun altro simbolo, poiché i browser moderni gestiranno i simboli reali perfettamente fintanto che la [codifica dei caratteri del tuo HTML è impostata su UTF-8](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata#specifying_your_documents_character_encoding).

## Commenti HTML

HTML ha un meccanismo per scrivere commenti nel codice. I browser ignorano i commenti, rendendoli effettivamente invisibili all'utente. Lo scopo dei commenti è permetterti di includere note nel codice per spiegare la tua logica o codifica. Questo è molto utile se si ritorna a un codice base dopo essere stati via abbastanza a lungo da non ricordarlo completamente. Allo stesso modo, i commenti sono inestimabili quando diverse persone effettuano modifiche e aggiornamenti.

Per scrivere un commento HTML, racchiudilo nei marcatori speciali `<!--` e `-->`. Ad esempio:

```html
<p>I'm not inside a comment</p>

<!-- <p>I am!</p> -->
```

Come puoi vedere qui sotto, solo il primo paragrafo è visualizzato nell'output dal vivo.

{{ EmbedLiveSample('HTML_comments', 700, 100, "", "") }}

## Sommario

Sei arrivato alla fine dell'articolo! Speriamo che tu abbia apprezzato il tuo tour delle basi dell'HTML.

A questo punto, dovresti aver capito che aspetto ha l'HTML e come funziona a un livello base. Dovresti anche essere in grado di scrivere alcuni elementi e attributi. Gli articoli successivi di questo modulo approfondiscono alcuni degli argomenti introdotti qui, oltre a presentare altri concetti del linguaggio.

- Mentre inizi a imparare di più sull'HTML, considera di imparare le basi del CSS (Cascading Style Sheets). [CSS](/it/docs/Learn_web_development/Core/Styling_basics) è il linguaggio utilizzato per stilizzare le pagine web, come cambiare i caratteri o i colori o modificare il layout della pagina. HTML e CSS funzionano bene insieme, come scoprirai presto.

## Vedi anche

- [Applicare colore agli elementi HTML usando il CSS](/it/docs/Web/CSS/CSS_colors/Applying_color)

{{NextMenu("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content")}}
