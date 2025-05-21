---
title: Intestazioni e paragrafi
slug: Learn_web_development/Core/Structuring_content/Headings_and_paragraphs
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content/Emphasis_and_importance", "Learn_web_development/Core/Structuring_content")}}

Uno dei compiti principali di HTML è dare struttura al testo in modo che un browser possa visualizzare un documento HTML come il suo sviluppatore intende. Questo articolo spiega come {{Glossary("HTML", "HTML")}} possa essere utilizzato per fornire una struttura fondamentale della pagina definendo intestazioni e paragrafi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base di HTML</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Come creare una buona struttura del documento con intestazioni e contenuto sotto queste intestazioni.</li>
          <li>Usare HTML semantico piuttosto che HTML presentazionale e perché questo è importante.</li>
          <li>La necessità di usare i livelli di intestazione in modo logico, cioè senza saltare livelli o usarli arbitrariamente perché si vuole raggiungere una certa dimensione del carattere (questa è un'attività per CSS).</li>
          <li>Benefici per la SEO: ad esempio, le parole chiave sono valorizzate nelle intestazioni.</li>
          <li>Benefici per l'accessibilità: le tecnologie assistive (TA) come i lettori di schermo usano le intestazioni (e altri punti di riferimento) come segnaposti per navigare nel contenuto. I documenti HTML sono molto difficili da utilizzare per gli utenti TA senza intestazioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Intestazioni e paragrafi

La maggior parte del testo strutturato è composto da intestazioni e paragrafi, che si tratti di leggere una storia, un giornale, un libro di testo universitario, una rivista, ecc.

![Esempio di una copertina di giornale, che mostra l'uso di un'intestazione di livello superiore, sottotitoli e paragrafi.](newspaper_small.jpg)

Il contenuto strutturato rende l'esperienza di lettura più facile e piacevole.

In HTML, ogni paragrafo deve essere racchiuso in un elemento {{htmlelement("p")}}, in questo modo:

```html
<p>I am a paragraph, oh yes I am.</p>
```

Ogni intestazione deve essere racchiusa in un elemento di intestazione:

```html
<h1>I am the title of the story.</h1>
```

Ci sono sei elementi di intestazione: {{htmlelement("Heading_Elements", "h1")}}, {{htmlelement("Heading_Elements", "h2")}}, {{htmlelement("Heading_Elements", "h3")}}, {{htmlelement("Heading_Elements", "h4")}}, {{htmlelement("Heading_Elements", "h5")}}, e {{htmlelement("Heading_Elements", "h6")}}. Ogni elemento rappresenta un diverso livello di contenuto nel documento; `<h1>` rappresenta l'intestazione principale, `<h2>` rappresenta i sottotitoli, `<h3>` rappresenta i sottosottotitoli, e così via.

## Implementare la gerarchia strutturale

Ad esempio, in questa storia, l'elemento `<h1>` rappresenta il titolo della storia, gli elementi `<h2>` rappresentano il titolo di ciascun capitolo e gli elementi `<h3>` rappresentano le sottosezioni di ciascun capitolo:

```html
<h1>The Crushing Bore</h1>

<p>By Chris Mills</p>

<h2>Chapter 1: The dark night</h2>

<p>
  It was a dark night. Somewhere, an owl hooted. The rain lashed down on the…
</p>

<h2>Chapter 2: The eternal silence</h2>

<p>Our protagonist could not so much as a whisper out of the shadowy figure…</p>

<h3>The specter speaks</h3>

<p>
  Several more hours had passed, when all of a sudden the specter sat bolt
  upright and exclaimed, "Please have mercy on my soul!"
</p>
```

Sta a te decidere cosa rappresentano gli elementi coinvolti, purché la gerarchia abbia senso. Devi solo tenere a mente alcune buone pratiche mentre crei tali strutture:

- Preferibilmente, dovresti usare un solo `<h1>` per pagina—questo è il livello di intestazione superiore e tutte le altre si trovano sotto di esso nella gerarchia.
- Assicurati di utilizzare le intestazioni nell'ordine corretto nella gerarchia. Non usare elementi `<h3>` per rappresentare sottotitoli, seguiti da elementi `<h2>` per rappresentare sottosottotitoli—non ha senso e porterà a risultati strani.
- Dei sei livelli di intestazione disponibili, dovresti mirare a usare non più di tre per pagina, a meno che non lo ritenga necessario. Documenti con molti livelli (ad esempio, una gerarchia di intestazione profonda) diventano ingombranti e difficili da navigare. In tali occasioni, è consigliabile distribuire il contenuto su più pagine, se possibile.

## Perché abbiamo bisogno della struttura?

Per rispondere a questa domanda, diamo un'occhiata a [text-start.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/html-text-formatting/text-start.html)—il punto di partenza del nostro esempio corrente per questo articolo (una bella ricetta di hummus). Dovresti salvare una copia di questo file sul tuo computer locale, poiché ti servirà per gli esercizi nelle lezioni successive. Il corpo di questo documento attualmente contiene più pezzi di contenuto. Non sono contrassegnati in alcun modo, ma sono separati da interruzioni di linea (Invio/Return premuto per andare alla riga successiva).

Tuttavia, quando apri il documento nel tuo browser, vedrai che il testo appare come un grande blocco!

![Una pagina web che mostra un muro di testo non formattato, perché non ci sono elementi sulla pagina per strutturarlo.](screen_shot_2017-03-29_at_09.20.35.png)

Questo perché non ci sono elementi per dare struttura al contenuto, quindi il browser non sa cosa è un'intestazione e cosa è un paragrafo. Inoltre:

- Gli utenti che guardano una pagina web tendono a scansionare rapidamente per trovare contenuti rilevanti, spesso leggendo solo le intestazioni, per cominciare. (Di solito [spendiamo pochissimo tempo su una pagina web](https://www.nngroup.com/articles/how-long-do-users-stay-on-web-pages/).) Se non riescono a vedere nulla di utile entro pochi secondi, probabilmente si frustreranno e andranno altrove.
- I motori di ricerca che indicizzano la tua pagina considerano il contenuto delle intestazioni come parole chiave importanti per influenzare il posizionamento della pagina nei risultati di ricerca. Senza intestazioni, la tua pagina avrà una performance scarsa in termini di {{Glossary("SEO", "SEO")}} (Search Engine Optimization, Ottimizzazione per i Motori di Ricerca).
- Le persone con gravi problemi visivi spesso non leggono le pagine web; le ascoltano invece. Ciò avviene con un software chiamato [text-to-speech](https://en.wikipedia.org/wiki/Screen_reader). Questo software fornisce modi per accedere rapidamente al testo dato. Tra le varie tecniche utilizzate, forniscono una panoramica del documento leggendo le intestazioni, consentendo ai loro utenti di trovare rapidamente le informazioni di cui hanno bisogno. Se le intestazioni non sono disponibili, saranno costretti ad ascoltare l'intero documento letto ad alta voce.
- Per stilare il contenuto con {{Glossary("CSS", "CSS")}}, o per fargli fare cose interessanti con {{Glossary("JavaScript", "JavaScript")}}, devi avere elementi che racchiudono il contenuto rilevante, in modo che CSS/JavaScript possa bersagliarlo efficacemente.

Pertanto, dobbiamo fornire al nostro contenuto una marcatura strutturale.

## Apprendimento attivo: Dare struttura al nostro contenuto

Passiamo subito a un esempio pratico. Nell'esempio qui sotto, aggiungi elementi al testo non elaborato nel campo _Input_ in modo che appaia come un'intestazione e due paragrafi nel campo _Output_.

Se commetti un errore, puoi sempre resettarlo usando il pulsante _Reset_. Se ti blocchi, premi il pulsante _Mostra soluzione_ per vedere la risposta.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="input" style="min-height: 100px; width: 95%">
My short story I am a statistician and my name is Trish.

My legs are made of cardboard and I am married to a fish.
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

const htmlSolution = `<h1>My short story</h1>
<p>
  I am a statistician and my name is Trish.
</p>
<p>
  My legs are made of cardboard and I am married to a fish.
</p>`;

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

// Stop tab key tabbing out of textarea and
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
textarea.onkeyup = function () {
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

{{ EmbedLiveSample('Active_learning_Giving_our_content_structure', 700, 400, "", "") }}

## Perché abbiamo bisogno della semantica?

La semantica è necessaria ovunque intorno a noi—ci affidiamo all'esperienza precedente per dirci quale sia la funzione di un oggetto di uso quotidiano; quando vediamo qualcosa, sappiamo quale sarà la sua funzione. Così, ad esempio, ci aspettiamo che una luce rossa del semaforo significhi "stop", e una luce verde significhi "vai." Le cose possono diventare complicate molto rapidamente se viene applicata una semantica errata. (Ci sono paesi che usano il rosso per significare "vai"? Speriamo di no.)

In modo simile, dobbiamo assicurarci di utilizzare gli elementi corretti, dando al nostro contenuto il significato, la funzione o l'aspetto corretti. In questo contesto, l'elemento {{htmlelement("Heading_Elements", "h1")}} è anche un elemento semantico, che assegna al testo che racchiude il ruolo (o significato) di "un'intestazione di livello superiore sulla tua pagina."

```html
<h1>This is a top level heading</h1>
```

Per default, il browser gli assegnerà una grande dimensione del carattere per farlo sembrare un'intestazione (anche se potresti stilizzarlo per farlo sembrare qualsiasi cosa tu voglia usando CSS). Più importante, il suo valore semantico verrà utilizzato in diversi modi, ad esempio dai motori di ricerca e dai lettori di schermo (come menzionato sopra).

D'altra parte, potresti far _apparire_ qualsiasi elemento come un'intestazione di livello superiore. Considera quanto segue:

```html
<span style="font-size: 32px; margin: 21px 0; display: block;">
  Is this a top level heading?
</span>
```

Questo è un elemento {{htmlelement("span")}}. Non ha semantica. Lo usi per racchiudere il contenuto quando vuoi applicargli CSS (o fare qualcosa con JavaScript) senza dargli alcun significato aggiuntivo. (Scoprirai di più su questi più avanti nel corso.) Abbiamo applicato del CSS per farlo sembrare un'intestazione di livello superiore, ma poiché non ha valore semantico, non otterrà nessuno dei benefici extra descritti sopra. È una buona idea utilizzare l'elemento HTML pertinente per il compito.

## Sommario

Questo conclude il nostro studio sulle intestazioni e sui paragrafi HTML. Successivamente, guarderemo altri aspetti di HTML semantico: dare enfasi alle parole.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content/Emphasis_and_importance", "Learn_web_development/Core/Structuring_content")}}
