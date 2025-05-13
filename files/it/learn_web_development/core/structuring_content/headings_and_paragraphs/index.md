---
title: Intestazioni e paragrafi
slug: Learn_web_development/Core/Structuring_content/Headings_and_paragraphs
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content/Emphasis_and_importance", "Learn_web_development/Core/Structuring_content")}}

Uno dei compiti principali di HTML è dare struttura al testo in modo che un browser possa visualizzare un documento HTML come intende il suo sviluppatore. Questo articolo spiega come {{Glossary("HTML", "HTML")}} possa essere utilizzato per fornire una struttura fondamentale alla pagina definendo intestazioni e paragrafi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Come creare una buona struttura del documento con intestazioni e contenuto sotto quelle intestazioni.</li>
          <li>Utilizzo di HTML semantico piuttosto che HTML di presentazione, e perché questo è importante.</li>
          <li>La necessità di utilizzare i livelli di intestazione in modo logico, ad esempio senza saltare livelli o usarli arbitrariamente perché si vuole ottenere una certa dimensione del carattere (questo è un compito per il CSS).</li>
          <li>Benefici per SEO: ad esempio, le parole chiave vengono potenziate nelle intestazioni.</li>
          <li>Benefici per l'accessibilità: la tecnologia assistiva (AT) come i lettori di schermo utilizza intestazioni (e altri punti di riferimento) come indicazioni per navigare nel contenuto. I documenti HTML sono molto difficili da usare per gli utenti AT senza intestazioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Intestazioni e paragrafi

La maggior parte del testo strutturato consiste di intestazioni e paragrafi, sia che lei stia leggendo una storia, un giornale, un libro di testo universitario, una rivista, ecc.

![Un esempio di prima pagina di un giornale, che mostra l'uso di un'intestazione di livello superiore, sottotitoli e paragrafi.](newspaper_small.jpg)

Il contenuto strutturato rende l'esperienza di lettura più facile e più piacevole.

In HTML, ogni paragrafo deve essere racchiuso in un elemento {{htmlelement("p")}}, come segue:

```html
<p>I am a paragraph, oh yes I am.</p>
```

Ogni intestazione deve essere racchiusa in un elemento di intestazione:

```html
<h1>I am the title of the story.</h1>
```

Ci sono sei elementi di intestazione: {{htmlelement("Heading_Elements", "h1")}}, {{htmlelement("Heading_Elements", "h2")}}, {{htmlelement("Heading_Elements", "h3")}}, {{htmlelement("Heading_Elements", "h4")}}, {{htmlelement("Heading_Elements", "h5")}}, e {{htmlelement("Heading_Elements", "h6")}}. Ogni elemento rappresenta un diverso livello di contenuto nel documento; `<h1>` rappresenta l'intestazione principale, `<h2>` rappresenta i sottotitoli, `<h3>` rappresenta i sotto-sottotitoli, e così via.

## Implementazione della gerarchia strutturale

Ad esempio, in questa storia, l'elemento `<h1>` rappresenta il titolo della storia, gli elementi `<h2>` rappresentano il titolo di ogni capitolo e gli elementi `<h3>` rappresentano le sottosezioni di ogni capitolo:

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

Sta a lei decidere cosa rappresentano gli elementi coinvolti, finché la gerarchia ha senso. Deve solo tenere a mente alcune buone pratiche mentre crea tali strutture:

- Preferibilmente, dovrebbe usare un singolo `<h1>` per pagina: questa è l'intestazione di livello superiore, e tutte le altre si trovano sotto di essa nella gerarchia.
- Assicurarsi di utilizzare le intestazioni nell'ordine corretto nella gerarchia. Non usare elementi `<h3>` per rappresentare sottotitoli, seguiti da elementi `<h2>` per rappresentare sotto-sottotitoli: non ha senso e porterà a risultati strani.
- Dei sei livelli di intestazioni disponibili, si dovrebbe puntare a usarne non più di tre per pagina, a meno che non si ritenga necessario. Documenti con molti livelli (ad esempio, una gerarchia di intestazioni profonda) diventano ingombranti e difficili da navigare. In tali occasioni, è consigliabile distribuire il contenuto su più pagine se possibile.

## Perché abbiamo bisogno di struttura?

Per rispondere a questa domanda, diamo un'occhiata a [text-start.html](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/html-text-formatting/text-start.html)—il punto di partenza del nostro esempio in corso per questo articolo (una bella ricetta di hummus). Dovrebbe salvare una copia di questo file sul suo computer locale, poiché le servirà per gli esercizi nelle lezioni successive. Il corpo di questo documento attualmente contiene più pezzi di contenuto. Non sono marcati in alcun modo, ma sono separati da interruzioni di linea (Enter/Return premuto per passare alla riga successiva).

Tuttavia, quando apre il documento nel suo browser, vedrà che il testo appare come un grande blocco!

![Una pagina web che mostra un muro di testo non formattato, perché non ci sono elementi sulla pagina per strutturarlo.](screen_shot_2017-03-29_at_09.20.35.png)

Questo perché non ci sono elementi per dare struttura al contenuto, quindi il browser non sa cosa è un'intestazione e cosa è un paragrafo. Inoltre:

- Gli utenti che guardano una pagina web tendono a scansionare velocemente per trovare contenuti pertinenti, spesso leggono solo le intestazioni, per cominciare. (Di solito [passiamo un tempo molto breve su una pagina web](https://www.nngroup.com/articles/how-long-do-users-stay-on-web-pages/).) Se non vedono nulla di utile entro pochi secondi, probabilmente si frustreranno e andranno altrove.
- I motori di ricerca che indicizzano la sua pagina considerano il contenuto delle intestazioni come parole chiave importanti per influenzare il ranking della pagina tra i risultati di ricerca. Senza intestazioni, la sua pagina avrà una scarsa prestazione in termini di {{Glossary("SEO", "SEO")}} (Ottimizzazione per i motori di ricerca).
- Le persone con gravi problemi di vista spesso non leggono le pagine web; le ascoltano invece. Questo viene fatto con un software chiamato [screen reader](https://en.wikipedia.org/wiki/Screen_reader). Questo software fornisce modi per ottenere un accesso rapido a un dato contenuto di testo. Tra le varie tecniche utilizzate, forniscono una panoramica del documento leggendo le intestazioni, consentendo ai loro utenti di trovare rapidamente le informazioni necessarie. Se le intestazioni non sono disponibili, saranno costrette ad ascoltare l'intero documento letto ad alta voce.
- Per stilare il contenuto con {{Glossary("CSS", "CSS")}}, o farlo fare cose interessanti con {{Glossary("JavaScript", "JavaScript")}}, è necessario avere elementi che racchiudano il contenuto pertinente, in modo che il CSS/JavaScript possa mirarlo efficacemente.

Pertanto, abbiamo bisogno di dare al nostro contenuto un markup strutturale.

## Apprendimento attivo: Dare struttura al nostro contenuto

Iniziamo subito con un esempio dal vivo. Nell'esempio qui sotto, aggiunga elementi al testo grezzo nel campo _Input_ in modo che appaia come un'intestazione e due paragrafi nel campo _Output_.

Se commette un errore, può sempre reimpostarlo usando il pulsante _Reset_. Se si blocca, preme il pulsante _Show solution_ per vedere la risposta.

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

## Perché abbiamo bisogno di semantica?

La semantica è utilizzata ovunque intorno a noi—ci affidiamo all'esperienza passata per sapere quale sia la funzione di un oggetto quotidiano; quando vediamo qualcosa, sappiamo quale sarà la sua funzione. Ad esempio, ci aspettiamo che un semaforo rosso significhi "ferma" e un semaforo verde significhi "vai". Le cose possono diventare rapidamente difficili se vengono applicate le semantiche sbagliate. (Qualche paese usa il rosso per significare "vai"? Speriamo di no.)

In modo simile, dobbiamo assicurarci di usare gli elementi corretti, dando il giusto significato, funzione o aspetto al nostro contenuto. In questo contesto, l'elemento {{htmlelement("Heading_Elements", "h1")}} è anche un elemento semantico, che attribuisce al testo intorno a esso il ruolo (o significato) di "un'intestazione di livello superiore sulla sua pagina."

```html
<h1>This is a top level heading</h1>
```

Per impostazione predefinita, il browser gli assegnerà una grande dimensione del carattere per farlo sembrare un'intestazione (anche se potrebbe stilizzarlo per farlo sembrare qualsiasi cosa desidera utilizzando CSS). Più importante, il suo valore semantico sarà utilizzato in molti modi, ad esempio dai motori di ricerca e dai lettori di schermo (come menzionato sopra).

D'altra parte, potrebbe far sembrare qualsiasi elemento _come_ un'intestazione di livello superiore. Consideri il seguente:

```html
<span style="font-size: 32px; margin: 21px 0; display: block;">
  Is this a top level heading?
</span>
```

Questo è un elemento {{htmlelement("span")}}. Non ha semantica. Lei lo usa per racchiudere il contenuto quando vuole applicare CSS ad esso (o farci qualcosa con JavaScript) senza dargli alcun significato extra. (Imparerà di più su questi più avanti nel corso.) Abbiamo applicato del CSS ad esso per farlo sembrare un'intestazione di livello superiore, ma poiché non ha alcun valore semantico, non otterrà nessuno dei benefici extra descritti sopra. È una buona idea utilizzare l'elemento HTML pertinente per il lavoro.

## Riepilogo

Questo conclude il nostro studio sulle intestazioni e i paragrafi in HTML. Prossimamente, vedremo altri aspetti di HTML semantico: dare enfasi alle parole.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Webpage_metadata", "Learn_web_development/Core/Structuring_content/Emphasis_and_importance", "Learn_web_development/Core/Structuring_content")}}
