---
title: Funzionalità di testo avanzate
slug: Learn_web_development/Core/Structuring_content/Advanced_text_features
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_documents", "Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content")}}

Ci sono molti altri elementi in HTML per definire la semantica del testo, che non abbiamo trattato nell'articolo [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance). Gli elementi descritti in questo articolo sono meno conosciuti, ma comunque utili da conoscere (e questa non è comunque una lista completa). Qui si imparerà a markupare citazioni, codice informatico e altro testo correlato, apice e pedice, informazioni di contatto, e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >liste</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Citazioni.</li>
          <li>Abbreviazioni e acronimi.</li>
          <li>Indirizzi.</li>
          <li>Orari e date.</li>
          <li>Apice e pedice.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Citazioni

HTML contiene funzionalità disponibili per markupare citazioni; quale elemento usare dipende dal fatto che si stia markupando una citazione a blocco o in linea.

### Citazioni a blocco

Se una sezione di contenuto livello blocco (che sia un paragrafo, più paragrafi, una lista, ecc.) è citata da qualche altra parte, dovrebbe essere avvolta in un elemento {{htmlelement("blockquote")}} per indicarlo, e includere un URL che punti alla fonte della citazione all'interno di un attributo [`cite`](/it/docs/Web/HTML/Reference/Elements/blockquote#cite). Ad esempio, il seguente markup è preso dalla pagina dell'elemento `<blockquote>` di MDN:

```html
<p>
  The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or
  <em>HTML Block Quotation Element</em>) indicates that the enclosed text is an
  extended quotation.
</p>
```

Per trasformare questo in una citazione a blocco, faremmo semplicemente così:

```html
<p>Here is a blockquote:</p>
<blockquote
  cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/blockquote">
  <p>
    The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or
    <em>HTML Block Quotation Element</em>) indicates that the enclosed text is
    an extended quotation.
  </p>
</blockquote>
```

Lo stile predefinito del browser renderà questo come un paragrafo rientrato, come un indicatore che si tratta di una citazione; il paragrafo sopra la citazione serve a dimostrarlo.

{{EmbedLiveSample('Blockquotes', '100%', '200px')}}

### Citazioni in linea

Le citazioni in linea funzionano esattamente allo stesso modo, eccetto che usano l'elemento {{htmlelement("q")}}. Ad esempio, il seguente esempio di markup contiene una citazione dalla pagina `<q>` di MDN:

```html
<p>
  The quote element — <code>&lt;q&gt;</code> — is
  <q
    cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/q">
    intended for short quotations that don't require paragraph breaks.
  </q>
</p>
```

Lo stile predefinito del browser renderà questo come testo normale messo tra virgolette per indicare una citazione, come segue:

{{EmbedLiveSample('Inline_quotations', '100%', '78px')}}

### Citazioni

Il contenuto dell'attributo [`cite`](/it/docs/Web/HTML/Reference/Elements/blockquote#cite) sembra utile, ma sfortunatamente i browser, i lettori di schermo, ecc. non ne fanno molto uso. Non c'è modo di far mostrare il contenuto di `cite` dal browser, senza scrivere la propria soluzione usando JavaScript o CSS. Se si vuole rendere la fonte della citazione disponibile nella pagina, è necessario renderla disponibile nel testo tramite un link o in qualche altro modo appropriato.

C'è un elemento {{htmlelement("cite")}}, ma è pensato per contenere il titolo della risorsa citata, ad esempio, il nome del libro. Non c'è motivo, tuttavia, per cui non si possa collegare il testo all'interno di `<cite>` alla fonte della citazione in qualche modo:

```html-nolint
<p>
  According to the
  <a href="/en-US/docs/Web/HTML/Reference/Elements/blockquote">
    <cite>MDN blockquote page</cite></a>:
</p>

<blockquote
  cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/blockquote">
  <p>
    The <strong>HTML <code>&lt;blockquote&gt;</code> Element</strong> (or
    <em>HTML Block Quotation Element</em>) indicates that the enclosed text is
    an extended quotation.
  </p>
</blockquote>

<p>
  The quote element — <code>&lt;q&gt;</code> — is
  <q cite="https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/q">
    intended for short quotations that don't require paragraph breaks.
  </q>
  — <a href="/en-US/docs/Web/HTML/Reference/Elements/q"><cite>MDN q page</cite></a>.
</p>
```

Le citazioni sono stilizzate con font in corsivo per default.

{{EmbedLiveSample('Citations', '100%', '179px')}}

### Apprendimento attivo: Chi l'ha detto?

È il momento di un altro esempio di apprendimento attivo! In questo esempio vorremmo che lei:

1. Trasformi il paragrafo centrale in una citazione a blocco, che include un attributo `cite`.
2. Trasformi "The Need To Eliminate Negative Self Talk" nel terzo paragrafo in una citazione in linea, e includa un attributo `cite`.
3. Avvolga il titolo di ciascuna fonte in tag `<cite>` e lo trasformi in un link a quella fonte.

Le fonti delle citazioni di cui ha bisogno sono:

- `http://www.brainyquote.com/quotes/authors/c/confucius.html` per la citazione di Confucio
- `http://example.com/affirmationsforpositivethinking` per "The Need To Eliminate Negative Self Talk".

Se commette un errore, può sempre reimpostarlo usando il pulsante _Reset_. Se è veramente bloccato, premi il pulsante _Mostra soluzione_ per vedere la risposta.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="input" style="min-height: 150px; width: 95%">
<p>Hello and welcome to my motivation page. As Confucius' quotes site says:</p>
<p>It does not matter how slowly you go as long as you do not stop.</p>
<p>I also love the concept of positive thinking, and The Need To Eliminate Negative Self Talk (as mentioned in Affirmations for Positive Thinking.)</p>
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
  '<p>Hello and welcome to my motivation page. As <a href="http://www.brainyquote.com/quotes/authors/c/confucius.html"><cite>Confucius\' quotes site</cite></a> says:</p>\n\n<blockquote cite="http://www.brainyquote.com/quotes/authors/c/confucius.html">\n <p>It does not matter how slowly you go as long as you do not stop.</p>\n</blockquote>\n\n<p>I also love the concept of positive thinking, and <q cite="http://example.com/affirmationsforpositivethinking">The Need To Eliminate Negative Self Talk</q> (as mentioned in <a href="http://example.com/affirmationsforpositivethinking"><cite>Affirmations for Positive Thinking</cite></a>.)</p>';
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

{{ EmbedLiveSample('Active_learning_Who_said_that', 700, 450) }}

## Abbreviazioni

Un altro elemento abbastanza comune che si incontra esplorando il Web è {{htmlelement("abbr")}} — questo è usato per avvolgere un'abbreviazione o un acronimo. Quando si include uno dei due, fornisca una piena espansione del termine in testo semplice al primo utilizzo, insieme a `<abbr>` per markupare l'abbreviazione. Questo fornisce un suggerimento agli user agent su come annunciare/visualizzare il contenuto informando tutti gli utenti su cosa significhi l'abbreviazione.

Se fornire l'espansione in aggiunta all'abbreviazione ha poco senso, e l'abbreviazione o l'acronimo è un termine abbastanza abbreviato, può fornire l'espansione completa del termine come valore dell'attributo [`title`](/it/docs/Web/HTML/Reference/Global_attributes/title):

### Esempio di abbreviazione

Guardiamo un esempio.

```html
<p>
  We use <abbr>HTML</abbr>, Hypertext Markup Language, to structure our web
  documents.
</p>

<p>
  I think <abbr title="Reverend">Rev.</abbr> Green did it in the kitchen with
  the chainsaw.
</p>
```

Questi appariranno all'incirca così:

{{EmbedLiveSample('Abbreviation_example', '100%', '150')}}

> [!NOTE]
> Le versioni precedenti di HTML includevano anche il supporto per l'elemento {{htmlelement("acronym")}}, ma è stato rimosso dalla specifica HTML a favore dell'uso di `<abbr>` per rappresentare sia abbreviazioni sia acronimi. `<acronym>` non dovrebbe essere usato.

### Apprendimento attivo: markupare un'abbreviazione

Per questo semplice incarico di apprendimento attivo, vorremmo che lei markupasse un'abbreviazione. Può usare il nostro esempio qui sotto, o sostituirlo con uno suo.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="input" style="min-height: 50px; width: 95%">
<p>NASA, the National Aeronautics and Space Administration, sure does some exciting work.</p>
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
  "<p><abbr>NASA</abbr>, the National Aeronautics and Space Administration, sure does some exciting work.</p>";
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

{{ EmbedLiveSample('Active_learning_marking_up_an_abbreviation', 700, 300) }}

## Markupare i dettagli di contatto

HTML ha un elemento per markupare i dettagli di contatto — {{htmlelement("address")}}. Questo avvolge i suoi dettagli di contatto, per esempio:

```html
<address>Chris Mills, Manchester, The Grim North, UK</address>
```

Potrebbe anche includere markup più complessi e altre forme di informazioni di contatto, per esempio:

```html
<address>
  <p>
    Chris Mills<br />
    Manchester<br />
    The Grim North<br />
    UK
  </p>

  <ul>
    <li>Tel: 01234 567 890</li>
    <li>Email: me@grim-north.co.uk</li>
  </ul>
</address>
```

Noti che qualcosa di simile sarebbe anche ok, se la pagina collegata contenesse le informazioni di contatto:

```html
<address>
  Page written by <a href="../authors/chris-mills/">Chris Mills</a>.
</address>
```

> [!NOTE]
> L'elemento {{htmlelement("address")}} dovrebbe essere usato solo per fornire informazioni di contatto per il documento contenuto con l'elemento {{htmlelement("article")}} o {{htmlelement("body")}} più vicino. Sarebbe corretto usarlo nel piè di pagina di un sito per includere le informazioni di contatto dell'intero sito, o all'interno di un articolo per i dettagli di contatto dell'autore, ma non per markupare un elenco di indirizzi non correlati al contenuto di quella pagina.

## Apice e pedice

Occasionalmente sarà necessario usare apice e pedice quando si markupano elementi come date, formule chimiche e equazioni matematiche in modo che abbiano il significato corretto. Gli elementi {{htmlelement("sup")}} e {{htmlelement("sub")}} gestiscono questo lavoro. Ad esempio:

```html
<p>My birthday is on the 25<sup>th</sup> of May 2001.</p>
<p>
  Caffeine's chemical formula is
  C<sub>8</sub>H<sub>10</sub>N<sub>4</sub>O<sub>2</sub>.
</p>
<p>If x<sup>2</sup> is 9, x must equal 3 or -3.</p>
```

L'output di questo codice appare così:

{{ EmbedLiveSample('Superscript_and_subscript', '100%', 160) }}

## Rappresentare il codice informatico

Ci sono numerosi elementi disponibili per markupare il codice informatico usando HTML:

- {{htmlelement("code")}}: Per markupare pezzi generici di codice informatico.
- {{htmlelement("pre")}}: Per conservare gli spazi bianchi (generalmente blocchi di codice) — se si utilizza l'indentazione o eccessivi spazi bianchi all'interno del testo, i browser li ignoreranno e non li vedrà sulla pagina renderizzata. Se si avvolge il testo nei tag `<pre></pre>`, gli spazi bianchi verranno resi identici a come li vede nel suo editor di testo.
- {{htmlelement("var")}}: Per markupare specificamente i nomi delle variabili.
- {{htmlelement("kbd")}}: Per markupare input (e altri tipi) di tastiera inseriti nel computer.
- {{htmlelement("samp")}}: Per markupare l'output di un programma informatico.

Vediamo esempi di questi elementi e come vengono usati per rappresentare il codice informatico.
Se vuole vedere l'intero file, dia un'occhiata al file [other-semantics.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/advanced-text-formatting/other-semantics.html).
Può scaricare il file e aprirlo nel suo browser per vederlo da solo, ma ecco un frammento del codice:

```html
<pre><code>const para = document.querySelector('p');

para.onclick = function() {
  alert('Owww, stop poking me!');
}</code></pre>

<p>
  You shouldn't use presentational elements like <code>&lt;font&gt;</code> and
  <code>&lt;center&gt;</code>.
</p>

<p>
  In the above JavaScript example, <var>para</var> represents a paragraph
  element.
</p>

<p>Select all the text with <kbd>Ctrl</kbd>/<kbd>Cmd</kbd> + <kbd>A</kbd>.</p>

<pre>$ <kbd>ping mozilla.org</kbd>
<samp>PING mozilla.org (63.245.215.20): 56 data bytes
64 bytes from 63.245.215.20: icmp_seq=0 ttl=40 time=158.233 ms</samp></pre>
```

Il codice sopra apparirà come segue:

{{ EmbedLiveSample('Representing_computer_code','100%',350) }}

## Markupare orari e date

HTML fornisce anche l'elemento {{htmlelement("time")}} per markupare orari e date in un formato leggibile dalla macchina. Ad esempio:

```html
<time datetime="2016-01-20">20 January 2016</time>
```

Perché è utile? Beh, ci sono molti modi diversi in cui gli esseri umani scrivono le date. La data sopra potrebbe essere scritta come:

<!-- markdownlint-disable MD033 -->

- 20 gennaio 2016
- 20° gennaio 2016
- 20 gen 2016
- 20/01/16
- 01/20/16
- Il 20 del prossimo mese
- <span lang="fr">20e Janvier 2016</span>
- <span lang="ja">2016 年 1 月 20 日</span>
- E così via

<!-- markdownlint-enable MD033 -->

Ma queste diverse forme non possono essere facilmente riconosciute dai computer — cosa succederebbe se volesse automaticamente afferrare le date di tutti gli eventi in una pagina e inserirle in un calendario? L'elemento {{htmlelement("time")}} le permette di allegare un orario/data univoca leggibile dalla macchina a questo scopo.

L'esempio di base sopra fornisce solo una semplice data leggibile dalla macchina, ma ci sono molte altre opzioni possibili, ad esempio:

```html
<!-- Standard simple date -->
<time datetime="2016-01-20">20 January 2016</time>
<!-- Just year and month -->
<time datetime="2016-01">January 2016</time>
<!-- Just month and day -->
<time datetime="01-20">20 January</time>
<!-- Just time, hours and minutes -->
<time datetime="19:30">19:30</time>
<!-- You can do seconds and milliseconds too! -->
<time datetime="19:30:01.856">19:30:01.856</time>
<!-- Date and time -->
<time datetime="2016-01-20T19:30">7.30pm, 20 January 2016</time>
<!-- Date and time with timezone offset -->
<time datetime="2016-01-20T19:30+01:00">
  7.30pm, 20 January 2016 is 8.30pm in France
</time>
<!-- Calling out a specific week number -->
<time datetime="2016-W04">The fourth week of 2016</time>
```

## Testi le sue abilità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare che abbia trattenuto queste informazioni prima di andare avanti — veda [Verifichi le sue abilità: Testo HTML avanzato](/it/docs/Learn_web_development/Core/Structuring_content/Test_your_skills/Advanced_HTML_text).

## Riepilogo

Questo segna la fine del nostro studio sulla semantica del testo HTML meno comune. Quello che ha visto durante questo corso non è un elenco esaustivo degli elementi di testo HTML — abbiamo voluto cercare di coprire gli elementi essenziali e alcuni dei più comuni che vedrà in circolazione. Successivamente, esamineremo i collegamenti, una delle funzionalità più importanti del web.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Structuring_documents", "Learn_web_development/Core/Structuring_content/Creating_links", "Learn_web_development/Core/Structuring_content")}}
