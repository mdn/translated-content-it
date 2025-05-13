---
title: Sintassi base di HTML
slug: Learn_web_development/Core/Structuring_content/Basic_HTML_syntax
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{NextMenu("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content")}}

In questo articolo, affrontiamo le basi assolute di HTML. Per iniziare, questo articolo definisce elementi, attributi e tutti gli altri termini importanti che potrebbe aver sentito. Spiega anche dove si inseriscono in HTML. Imparerà come sono strutturati gli elementi HTML, come è strutturata una tipica pagina HTML e altre caratteristiche base importanti della lingua. Durante il percorso, ci sarà l'opportunità di giocare anche con HTML!

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software">Software di base installato</a> e conoscenza di base di <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files">lavorare con i file</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Anatomia di un elemento HTML — elemento, tag di apertura, contenuto, tag di chiusura, attributi.</li>
          <li>Il corpo HTML e il suo scopo come contenitore per il contenuto della pagina.</li>
          <li>Cosa sono gli <a href="/it/docs/Glossary/Void_element">elementi void</a> (noti anche come elementi vuoti) e come differiscono dagli altri elementi.</li>
          <li>La necessità di un doctype nella parte superiore dei documenti HTML. Il suo scopo originario e il fatto che ora sia in qualche modo un artefatto storico.</li>
          <li>Comprendere che HTML deve essere nidificato correttamente.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è HTML?

{{Glossary("HTML", "HTML")}} (HyperText Markup Language) è un _linguaggio di markup_ che dice ai browser web come strutturare le pagine web che visita. Può essere complesso o semplice quanto desidera il sviluppatore web. HTML consiste in una serie di {{Glossary("Element", "elementi")}}, che usa per racchiudere, avvolgere o _markup_ diverse parti del contenuto per farlo apparire o agire in un certo modo. I {{Glossary("Tag", "tag")}} di chiusura possono trasformare il contenuto in un hyperlink per connettersi a un'altra pagina, rendere in corsivo le parole, e così via. Ad esempio, consideri la seguente riga di testo:

```plain
My cat is very grumpy
```

Se volessimo che il testo stesse da solo, potremmo specificare che è un paragrafo racchiudendolo in un elemento paragrafo ({{htmlelement("p")}}):

```html
<p>My cat is very grumpy</p>
```

HTML vive all'interno di file di testo chiamati **documenti HTML**, o semplicemente **documenti**, con un'estensione di file `.html`. Dove in precedenza abbiamo parlato di pagine web, un documento HTML contiene il contenuto della pagina web e ne specifica la struttura.

Il file HTML più comune che incontrerà è `index.html`, che di solito viene utilizzato per contenere il contenuto della pagina principale di un sito web. È anche comune vedere sottocartelle con il proprio `index.html`, quindi un sito web può avere più file index in posizioni diverse.

> [!NOTE]
> I tag in HTML non sono sensibili alle maiuscole. Questo significa che possono essere scritti in maiuscolo o minuscolo. Ad esempio, un tag {{htmlelement("title")}} potrebbe essere scritto come `<title>`, `<TITLE>`, `<Title>`, `<TiTlE>`, ecc., e funzionerà. Tuttavia, è una buona pratica scrivere tutti i tag in minuscolo per coerenza e leggibilità.

## Anatomia di un elemento HTML

Esploriamo ulteriormente il nostro elemento paragrafo dalla sezione precedente:

![Un esempio di snippet di codice che dimostra la struttura di un elemento HTML.<p> Il mio gatto è molto scontroso </p>.](grumpy-cat-small.png)

L'anatomia del nostro elemento è:

- **Il tag di apertura:** Questo consiste nel nome dell'elemento (in questo esempio, _p_ per paragrafo), racchiuso tra parentesi angolari di apertura e chiusura. Questo tag di apertura segna dove l'elemento inizia o comincia ad avere effetto. In questo esempio, precede l'inizio del testo del paragrafo.
- **Il contenuto:** Questo è il contenuto dell'elemento. In questo esempio, è il testo del paragrafo.
- **Il tag di chiusura:** Questo è lo stesso del tag di apertura, eccetto che include una barra prima del nome dell'elemento. Questo segna dove l'elemento finisce. Non includere un tag di chiusura è un errore comune per i principianti che può produrre risultati strani.

L'elemento è il tag di apertura, seguito dal contenuto, seguito dal tag di chiusura.

> [!NOTE]
> Vai al nostro partner di apprendimento Scrimba per una spiegazione interattiva dei [tag HTML](https://scrimba.com/learn-html-and-css-c0p/~02?via=mdn) <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> attraverso un esercizio interattivo.

### Apprendimento attivo: creare il suo primo elemento HTML

Modifichi la linea qui sotto nell'area "Codice modificabile" racchiudendola con i tag `<em>` e `</em>.` Per _aprire l'elemento_, metta il tag di apertura `<em>` all'inizio della linea. Per _chiudere l'elemento_, metta il tag di chiusura `</em>` alla fine della linea. Fare ciò dovrebbe dare alla linea una formattazione di testo in corsivo! Veda i suoi cambiamenti aggiornarsi dal vivo nell'area _Output_.

Se commette un errore, può cancellare il suo lavoro usando il pulsante _Reset_. Se rimane veramente bloccato, prema il pulsante _Mostra soluzione_ per vedere la risposta.

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

### Nidificazione degli elementi

Gli elementi possono essere inseriti all'interno di altri elementi. Questo è chiamato _nidificazione_. Se volessimo affermare che il nostro gatto è **molto** scontroso, potremmo racchiudere la parola _molto_ in un elemento {{htmlelement("strong")}}, il quale significa che la parola avrà una formattazione di testo più forte:

```html
<p>My cat is <strong>very</strong> grumpy.</p>
```

Esiste un modo giusto e uno sbagliato di fare nidificazione. Nell'esempio sopra, abbiamo aperto l'elemento `p` prima, poi aperto l'elemento `strong`. Per una nidificazione corretta, dovremmo chiudere prima l'elemento `strong`, prima di chiudere il `p`.

Il seguente è un esempio del modo _sbagliato_ di fare nidificazione:

```html-nolint example-bad
<p>My cat is <strong>very grumpy.</p></strong>
```

I **tag devono aprire e chiudere in modo che siano dentro o fuori uno dall'altro**. Con il tipo di sovrapposizione nell'esempio sopra, il browser deve indovinare la sua intenzione. Questo tipo di deduzione può portare a risultati inaspettati.

### Elementi void

Non tutti gli elementi seguono il modello di un tag di apertura, contenuto e un tag di chiusura. Alcuni elementi consistono in un singolo tag, che viene tipicamente utilizzato per inserire/incorporare qualcosa nel documento. Tali elementi sono chiamati {{Glossary("void_element", "elementi void")}}. Ad esempio, l'elemento {{htmlelement("img")}} incorpora un file immagine su una pagina:

```html
<img
  src="https://raw.githubusercontent.com/mdn/beginner-html-site/gh-pages/images/firefox-icon.png"
  alt="Firefox icon" />
```

Questo produrrebbe il seguente output:

{{ EmbedLiveSample('Void_elements', 700, 300, "", "") }}

> [!NOTE]
> In HTML, non c'è la necessità di aggiungere un `/` alla fine di un tag di un elemento void, per esempio: `<img src="images/cat.jpg" alt="cat" />`. Tuttavia, è anche una sintassi valida e può farlo quando vuole che il suo HTML sia un XML valido.

## Attributi

Gli elementi possono anche avere attributi. Gli attributi appaiono così:

![tag di paragrafo con l'attributo 'class="editor-note"' evidenziato](grumpy-cat-attribute-small.png)

Gli attributi contengono informazioni extra sull'elemento che non appariranno nel contenuto. In questo esempio, l'attributo **`class`** è un nome identificativo usato per targhettare l'elemento con informazioni di stile.

Un attributo dovrebbe avere:

- Uno spazio tra esso e il nome dell'elemento. (Per un elemento con più di un attributo, gli attributi dovrebbero essere separati da spazi anch'essi.)
- Il nome dell'attributo, seguito da un segno di uguale.
- Un valore dell'attributo, racchiuso da virgolette di apertura e chiusura.

### Apprendimento attivo: aggiungere attributi a un elemento

L'elemento `<img>` può prendere un numero di attributi, inclusi:

- `src`
  - : L'attributo `src` è un attributo **richiesto** che specifica la posizione dell'immagine. Per esempio: `src="https://raw.githubusercontent.com/mdn/beginner-html-site/gh-pages/images/firefox-icon.png"`.
- `alt`
  - : L'attributo `alt` specifica una descrizione testuale dell'immagine. Per esempio: `alt="The Firefox icon"`.
- `width`
  - : L'attributo `width` specifica la larghezza dell'immagine con l'unità essendo pixel. Per esempio: `width="300"`.
- `height`
  - : L'attributo `height` specifica l'altezza dell'immagine con l'unità essendo pixel. Per esempio: `height="300"`.

Modifichi la linea qui sotto nell'area _Input_ per trasformarla in un'immagine.

1. Trovi la sua immagine preferita online, faccia clic con il tasto destro su di essa e premi _Copia link/indirizzo immagine_.
2. Torni nell'area sottostante, aggiunga l'attributo `src` e lo riempi con il link ottenuto nel passaggio 1.
3. Imposti l'attributo `alt`.
4. Aggiunga gli attributi `width` e `height`.

Potrà vedere i suoi cambiamenti dal vivo nell'area _Output_.

Se commette un errore, può sempre resettarla usando il pulsante _Reset_. Se rimane veramente bloccato, prema il pulsante _Mostra soluzione_ per vedere la risposta.

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

A volte vedrà attributi scritti senza valori. Questo è interamente accettabile. Questi sono chiamati {{Glossary("Boolean/HTML", "attributi booleani")}}. Quando un attributo booleano è scritto senza un valore, o con qualsiasi valore, anche come `"false"`, l'attributo booleano è sempre impostato su true. Altrimenti, se l'attributo non è scritto in un tag HTML, l'attributo è impostato su false. La specifica richiede che il valore dell'attributo sia sia la stringa vuota (compreso quando l'attributo non ha specificata nessun valore esplicitamente) sia lo stesso del nome dell'attributo, ma altri valori funzionano allo stesso modo. Per esempio, consideri l'attributo [`disabled`](/it/docs/Web/HTML/Reference/Elements/input#disabled), che può assegnare agli elementi di input del modulo. (Usa questo per _disabilitare_ gli elementi di input del modulo in modo che l'utente non possa inserire dati. Gli elementi disabilitati tipicamente hanno un'aspetto grigio.) Per esempio:

```html
<input type="text" disabled="disabled" />
```

Come abbreviazione, è accettabile scrivere questo come segue:

```html
<!-- using the disabled attribute prevents the end user from entering text into the input box -->
<input type="text" disabled />

<!-- text input is allowed, as it doesn't contain the disabled attribute -->
<input type="text" />
```

Per riferimento, l'esempio sopra include anche un elemento di input del modulo non disabilitato. L'HTML dell'esempio sopra produce questo risultato:

{{ EmbedLiveSample('Boolean_attributes', 700, 100, "", "") }}

### Omettere le virgolette intorno ai valori degli attributi

Se osserva il codice di molti altri siti, potrebbe imbattersi in una serie di stili di markup strani, inclusi valori di attributo senza virgolette. Questo è permesso in alcune circostanze, ma può anche rompere il suo markup in altre circostanze. L'elemento nello snippet di codice sottostante, `<a>`, è chiamato un'àncora. Le àncore racchiudono il testo e lo trasformano in collegamenti. L'attributo `href` specifica l'indirizzo web a cui punta il collegamento. Può scrivere questa versione di base qui sotto con _solo_ l'attributo `href`, come questo:

```html
<a href=https://www.mozilla.org/>favorite website</a>
```

Le àncore possono anche avere un attributo `title`, una descrizione della pagina collegata. Tuttavia, non appena aggiungiamo il `title` nello stesso modo dell'attributo `href` ci sono problemi:

```html-nolint example-bad
<a href=https://www.mozilla.org/ title=The Mozilla homepage>favorite website</a>
```

Così com'è scritto sopra, il browser interpreta male il markup, scambiando l'attributo `title` per tre attributi: un attributo title con il valore `The`, e due attributi Boolean `Mozilla` e `homepage`. Ovviamente, questo non è intenzionale! Può causare errori o comportamenti inaspettati, come può vedere nel live esempio sottostante. Provi a passare il cursore sopra il collegamento per visualizzare il testo del title!

{{ EmbedLiveSample('Omitting_quotes_around_attribute_values', 700, 100, "", "") }}

Includa sempre le virgolette dell'attributo. Evita tali problemi e porta a un codice più leggibile.

### Virgolette singole o doppie?

In questo articolo noterà anche che gli attributi sono racchiusi tra virgolette doppie. Tuttavia, potrebbe vedere virgolette singole in alcuni codici HTML. Si tratta di una questione di stile. Si senta libero di scegliere quella che preferisce. Entrambe queste righe sono equivalenti:

```html-nolint
<a href='https://www.example.com'>A link to my example.</a>

<a href="https://www.example.com">A link to my example.</a>
```

Assicurati di non mescolare virgolette singole e doppie. Questo esempio (qui sotto) mostra un tipo di mescolanza di virgolette che andrà male:

```html-nolint example-bad
<a href="https://www.example.com'>A link to my example.</a>
```

Tuttavia, se usa un tipo di virgolette, può includere l'altro tipo di virgolette _dentro_ i suoi valori di attributo:

```html
<a href="https://www.example.com" title="Isn't this fun?">
  A link to my example.
</a>
```

Per usare i segni di virgolette dentro altri segni di virgolette dello stesso tipo (virgolette singole o virgolette doppie), usi {{Glossary("character_reference", "riferimenti di carattere")}}.
Per esempio, questo romperà:

```html-nolint example-bad
<a href="https://www.example.com" title="An "interesting" reference">A link to my example.</a>
```

Invece, deve fare così:

```html-nolint
<a href="https://www.example.com" title="An &quot;interesting&quot; reference">A link to my example.</a>
```

## Anatomia di un documento HTML

Gli elementi HTML individuali non sono molto utili da soli. Successivamente, esaminiamo come gli elementi individuali si combinano per formare un'intera pagina HTML:

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

1. `<!doctype html>`: Il doctype. Quando l'HTML era giovane (1991-1992), i doctype erano pensati per agire come collegamenti a un insieme di regole che la pagina HTML doveva seguire per essere considerata un buon HTML. I doctype erano soliti apparire in questo modo:

   ```html
   <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
   ```

   Più recentemente, il doctype è un artefatto storico che deve essere incluso affinché tutto il resto funzioni correttamente. `<!doctype html>` è la stringa di caratteri più breve che conta come un doctype valido. Questo è tutto ciò che deve sapere!

2. `<html></html>`: L'elemento {{htmlelement("html")}}. Questo elemento racchiude tutto il contenuto sulla pagina. È talvolta noto come l'elemento radice.
3. `<head></head>`: L'elemento {{htmlelement("head")}}. Questo elemento agisce come contenitore per tutto ciò che vuole includere nella pagina HTML, **che non sia il contenuto** che la pagina mostrerà ai visitatori. Questo include parole chiave e una descrizione della pagina che appare nei risultati di ricerca, CSS per stilizzare il contenuto, dichiarazioni di set di caratteri, e altro ancora. Imparerà di più su questo nel prossimo articolo della serie.
4. `<meta charset="utf-8">`: L'elemento {{htmlelement("meta")}}. Questo elemento rappresenta i metadati che non possono essere rappresentati da altri elementi meta-correlati HTML, come {{htmlelement("base")}}, {{htmlelement("link")}}, {{htmlelement("script")}}, {{htmlelement("style")}} o {{htmlelement("title")}}. L'attributo [`charset`](/it/docs/Web/HTML/Reference/Elements/meta#charset) specifica la codifica del carattere per il suo documento come UTF-8, che include la maggior parte dei caratteri dalla stragrande maggioranza delle lingue scritte umane. Con questa impostazione, la pagina può ora gestire qualsiasi contenuto testuale che potrebbe contenere. Non c'è motivo di non impostarlo, e può aiutare ad evitare alcuni problemi in seguito.
5. `<title></title>`: L'elemento {{htmlelement("title")}}. Questo imposta il titolo della pagina, che è il titolo che appare nella scheda del browser in cui la pagina è caricata. Il titolo della pagina è anche usato per descrivere la pagina quando viene aggiunta ai segnalibri.
6. `<body></body>`: L'elemento {{htmlelement("body")}}. Questo contiene _tutto_ il contenuto che appare sulla pagina, compresi testi, immagini, video, giochi, tracce audio riproducibili, o qualsiasi altra cosa.

### Apprendimento attivo: Aggiungere alcune caratteristiche a un documento HTML

Se vuole sperimentare a scrivere alcuni HTML sul suo computer locale, può:

1. Copiare l'esempio di pagina HTML elencato sopra.
2. Creare un nuovo file nel suo editor di testo.
3. Incollare il codice nel nuovo file di testo.
4. Salvare il file come `index.html`.

> [!NOTE]
> Può anche trovare questo modello HTML di base nel [GitHub repo dell'area di apprendimento MDN](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/getting-started/index.html).

Può ora aprire questo file in un browser web per vedere come appare il codice renderizzato. Modifichi il codice e aggiorni il browser per vedere quale è il risultato. Inizialmente, la pagina appare così:

![Una semplice pagina HTML che dice This is my page](template-screenshot.png)

In questo esercizio, può modificare il codice localmente sul suo computer, come descritto in precedenza, o può modificarlo nella finestra del campione qui sotto (la finestra del campione modificabile rappresenta solo i contenuti dell'elemento {{htmlelement("body")}}, in questo caso). Affini le sue abilità implementando i seguenti compiti:

- Subito sotto il tag di apertura dell'elemento {{htmlelement("body")}}, aggiunga un titolo principale per il documento. Questo dovrebbe essere racchiuso all'interno di un tag di apertura `<h1>` e un tag di chiusura `</h1>`.
- Modifichi il contenuto del paragrafo per includere un testo su un argomento che trova interessante.
- Faccia risaltare le parole importanti in grassetto racchiudendole all'interno di un tag di apertura `<strong>` e un tag di chiusura `</strong>`.
- Aggiunga un collegamento al suo paragrafo, come [spiegato in precedenza nell'articolo](#active_learning_adding_attributes_to_an_element).
- Aggiunga un'immagine al suo documento. La posizioni sotto il paragrafo, come [spiegato in precedenza nell'articolo](#elementi_void). Guadagni punti bonus se riesce a collegare a un'immagine diversa (sia localmente sul suo computer che da qualche altra parte sul web).

Se commette un errore, può sempre resettarla usando il pulsante _Reset_. Se rimane veramente bloccato, prema il pulsante _Mostra soluzione_ per vedere la risposta.

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

### Spazio bianco in HTML

Negli esempi sopra, potrebbe aver notato che molto spazio bianco è incluso nel codice. Questo è opzionale. Questi due snippet di codice sono equivalenti:

```html-nolint
<p id="noWhitespace">Dogs are silly.</p>

<p id="whitespace">Dogs
    are
        silly.</p>
```

Non importa quanto spazio bianco usa all'interno del contenuto degli elementi HTML (che possono includere uno o più spazi, ma anche interruzioni di linea), il parser HTML riduce ogni sequenza di spazio bianco a un solo spazio durante il rendering del codice. Allora perché usare tanto spazio bianco? La risposta è leggibilità.

Può essere più facile capire cosa sta succedendo nel suo codice se lo ha formattato bene. Nel nostro HTML, abbiamo ogni elemento nidificato rientrato di due spazi in più rispetto a quello in cui è inserito. Sta a lei scegliere lo stile di formattazione (quanti spazi per ogni livello di rientro, per esempio), ma dovrebbe considerare di formattarlo.

Diamo un'occhiata a come il browser renderizza i due paragrafi sopra con e senza spazio bianco:

{{ EmbedLiveSample('Whitespace_in_HTML', 700, 100) }}

> [!NOTE]
> Accedere all'[innerHTML](/it/docs/Web/API/Element/innerHTML) degli elementi da JavaScript manterrà intatto tutto il spazio bianco.
> Questo potrebbe restituire risultati inaspettati se lo spazio bianco viene ritagliato dal browser.

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

## Riferimenti di carattere: includere caratteri speciali in HTML

In HTML, i caratteri `<`, `>`, `"`, `'`, e `&` sono caratteri speciali. Fanno parte della sintassi HTML stessa. Quindi come includere uno di questi caratteri speciali nel suo testo? Ad esempio, se vuole usare un simbolo di ampersand o di minoranza, e non vuole che venga interpretato come codice.

Fa ciò con {{Glossary("character_reference", "riferimenti di carattere")}}. Questi sono codici speciali che rappresentano caratteri, da utilizzare in queste esatte circostanze. Ogni riferimento di carattere inizia con un carattere ampersand (&) e termina con un punto e virgola (;).

| Carattere letterale | Equivalente riferimento di carattere |
| ------------------- | ----------------------------------- |
| <                   | `&lt;`                              |
| >                   | `&gt;`                              |
| "                   | `&quot;`                            |
| '                   | `&apos;`                            |
| &                   | `&amp;`                             |

L'equivalente del riferimento di carattere potrebbe essere facilmente memorizzato perché il testo che usa può essere visto come minore di per `&lt;`, citazione per `&quot;` e simili per altri. Per sapere di più sui riferimenti agli enti, veda [List of XML and HTML character entity references](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references) (Wikipedia).

Nell'esempio sottostante, ci sono due paragrafi:

```html-nolint
<p>In HTML, you define a paragraph using the <p> element.</p>

<p>In HTML, you define a paragraph using the &lt;p&gt; element.</p>
```

Nell'output live qui sotto, può vedere che il primo paragrafo è andato storto. Il browser interpreta la seconda istanza di `<p>` come l'inizio di un nuovo paragrafo. Il secondo paragrafo appare corretto perché ha le parentesi angolari con riferimenti di carattere.

{{ EmbedLiveSample('Entity_references_Including_special_characters_in_HTML', 700, 200, "", "") }}

> [!NOTE]
> Non è necessario utilizzare riferimenti agli enti per qualsiasi altro simbolo, poiché i browser moderni gestiranno bene i simboli reali purché la [codifica dei caratteri del suo HTML sia impostata su UTF-8](/it/docs/Learn_web_development/Core/Structuring_content/Webpage_metadata#specifying_your_documents_character_encoding).

## Commenti in HTML

HTML ha un meccanismo per scrivere commenti nel codice. I browser ignorano i commenti, rendendoli effettivamente invisibili all'utente. Lo scopo dei commenti è consentirle di includere note nel codice per spiegare la sua logica o la sua codifica. Questo è molto utile se ritorna su una base di codice dopo essere stato lontano abbastanza a lungo da non ricordarlo completamente. Allo stesso modo, i commenti sono inestimabili mentre persone diverse apportano modifiche e aggiornamenti.

Per scrivere un commento HTML, lo racchiuda nei marcatori speciali `<!--` e `-->`. Per esempio:

```html
<p>I'm not inside a comment</p>

<!-- <p>I am!</p> -->
```

Come può vedere qui sotto, solo il primo paragrafo viene visualizzato nell'output live.

{{ EmbedLiveSample('HTML_comments', 700, 100, "", "") }}

## Riepilogo

È arrivato alla fine dell'articolo! Speriamo che abbia apprezzato il suo tour sulle basi di HTML.

A questo punto, dovrebbe comprendere come appare HTML e come funziona a livello base. Dovrebbe anche essere in grado di scrivere pochi elementi e attributi. I successivi articoli di questo modulo approfondiranno alcuni degli argomenti introdotti qui, oltre a presentare altri concetti della lingua.

- Mentre inizia a imparare di più su HTML, consideri di imparare le basi di CSS (Cascading Style Sheets). [CSS](/it/docs/Learn_web_development/Core/Styling_basics) è il linguaggio utilizzato per stilizzare le pagine web, ad esempio cambiando i caratteri o i colori o alterando il layout della pagina. HTML e CSS lavorano bene insieme, come scoprirà presto.

## Vedere anche

- [Applicare colore agli elementi HTML usando CSS](/it/docs/Web/CSS/CSS_colors/Applying_color)

{{NextMenu("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content")}}
