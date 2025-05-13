---
title: Enfasi e importanza
slug: Learn_web_development/Core/Structuring_content/Emphasis_and_importance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content")}}

L'articolo precedente ha esaminato perché la semantica è importante in HTML, concentrandosi su intestazioni e paragrafi. Questo articolo prosegue sulla stessa linea, esaminando gli elementi HTML che applicano enfasi e importanza al testo (in parallelo con il testo in corsivo e grassetto nei media stampati).

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
          <li>Il significato di enfasi e importanza, e gli elementi di base che li applicano in HTML, come <code>&lt;em&gt;</code> e <code>&lt;strong&gt;</code>.</li>
          <li>Identificare il markup di presentazione che non dovrebbe più essere utilizzato (ad es., <code>&lt;big&gt;</code> e <code>&lt;font&gt;</code>); è deprecato.</li>
          <li>Identificare il markup di presentazione che è stato riproposto per avere un nuovo significato semantico (ad es., <code>&lt;i&gt;</code> e <code>&lt;b&gt;</code>).</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono enfasi e importanza?

Nel linguaggio umano, spesso enfatizziamo certe parole per alterare il significato di una frase, e vogliamo spesso segnare certe parole come importanti o diverse in qualche modo. HTML fornisce vari elementi semantici che ci permettono di marcare il contenuto testuale con tali effetti, e in questa sezione vedremo alcuni dei più comuni.

### Enfasi

Quando vogliamo aggiungere enfasi nel linguaggio parlato, _sottolineamo_ certe parole, alterando sottilmente il significato di ciò che stiamo dicendo. Allo stesso modo, nel linguaggio scritto tendiamo a sottolineare le parole mettendole in corsivo. Ad esempio, le seguenti due frasi hanno significati diversi.

> Sono contento che non sia in ritardo.
>
> Sono _contento_ che non sia _in ritardo_.

La prima frase sembra genuinamente sollevata che la persona non sia in ritardo. Al contrario, la seconda, con entrambi i termini "contento" e "in ritardo" in corsivo, sembra sarcastica o passivo-aggressiva, esprimendo fastidio per il fatto che la persona sia arrivata un po' in ritardo.

In HTML usiamo l'elemento {{htmlelement("em")}} (enfasi) per marcare tali istanze. Oltre a rendere il documento più interessante da leggere, queste sono riconosciute dai lettori di schermo, che possono essere configurati per leggerle con un tono di voce diverso. I browser stilizzano questo testo come corsivo per impostazione predefinita, ma non dovrebbe utilizzare questo tag solo per ottenere una stilizzazione in corsivo. Per fare ciò, si utilizzerebbe un elemento {{htmlelement("span")}} e un po' di CSS, o forse un elemento {{htmlelement("i")}} (vedere sotto).

```html
<p>I am <em>glad</em> you weren't <em>late</em>.</p>
```

### Forte importanza

Per enfatizzare parole importanti, tendiamo a sottolinearle nel linguaggio parlato e a metterle in **grassetto** nel linguaggio scritto. Ad esempio:

> Questo liquido è **altamente tossico**.
>
> Conto su di lei. **Non** arrivi in ritardo!

In HTML usiamo l'elemento {{htmlelement("strong")}} (forte importanza) per marcare tali istanze. Oltre a rendere il documento più utile, anche queste vengono riconosciute dai lettori di schermo, che possono essere configurati per leggerle con un tono di voce diverso. I browser stilizzano questo testo come grassetto per impostazione predefinita, ma non dovrebbe utilizzare questo tag solo per ottenere una stilizzazione in grassetto. Per fare ciò, si utilizzerebbe un elemento {{htmlelement("span")}} e un po' di CSS, o forse un elemento {{htmlelement("b")}} (vedere sotto).

```html
<p>This liquid is <strong>highly toxic</strong>.</p>

<p>I am counting on you. <strong>Do not</strong> be late!</p>
```

È possibile annidare forte importanza ed enfasi una dentro l'altra, se lo si desidera:

```html-nolint
<p>This liquid is <strong>highly toxic</strong> — if you drink it, <strong>you may <em>die</em></strong>.</p>
```

{{EmbedLiveSample('Strong importance')}}

## Apprendimento attivo: Diamoci importanza

In questa sezione di apprendimento attivo, abbiamo fornito un esempio modificabile. All'interno, vorremmo che provasse ad aggiungere enfasi e forte importanza alle parole che pensa ne abbiano bisogno, solo per esercitarsi un po'.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="input" style="min-height: 200px; width: 95%">
<h1>Important notice</h1>
<p>On Sunday January 9th 2010, a gang of goths were
  spotted stealing several garden gnomes from a
  shopping center in downtown Milwaukee. They were
  all wearing green jumpsuits and silly hats, and
  seemed to be having a whale of a time. If anyone
   has any information about this incident, please
    contact the police now.</p>
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
  "<h1>Important notice</h1>\n<p>On <strong>Sunday January 9th 2010</strong>, a gang of <em>goths</em> were spotted stealing <strong><em>several</em> garden gnomes</strong> from a shopping center in downtown <strong>Milwaukee</strong>. They were all wearing <em>green jumpsuits</em> and <em>silly hats</em>, and seemed to be having a whale of a time. If anyone has <strong>any</strong> information about this incident, please contact the police <strong>now</strong>.</p>";
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

{{ EmbedLiveSample('Active_learning_Lets_be_important', 700, 520, "", "") }}

## Corsivo, grassetto, sottolineatura…

Gli elementi discussi finora hanno chiari significati semantici associati. La situazione con {{htmlelement("b")}}, {{htmlelement("i")}}, e {{htmlelement("u")}} è un po' più complicata. Sono stati creati affinché le persone potessero scrivere testo in grassetto, corsivo o sottolineato in un'epoca in cui il CSS era ancora poco supportato o non supportato affatto. Elementi come questi, che influenzano solo la presentazione e non la semantica, sono noti come **elementi di presentazione** e non dovrebbero più essere usati, poiché, come abbiamo visto prima, la semantica è così importante per l'accessibilità, la SEO, ecc.

HTML5 ha ridefinito `<b>`, `<i>`, e `<u>` con nuovi, in qualche modo confusi, ruoli semantici.

Ecco la migliore regola che può ricordare: È appropriato utilizzare `<b>`, `<i>`, `<u>` per trasmettere un significato tradizionalmente trasmesso con grassetto, corsivo o sottolineatura solo quando non c'è un elemento più adatto; e in genere c'è. Considerare se `<strong>`, `<em>`, `<mark>`, o `<span>` potrebbero essere più appropriati.

Mantenga sempre una mentalità orientata all'accessibilità. Il concetto di corsivo non è molto utile per le persone che utilizzano lettori di schermo, o per chi usa un sistema di scrittura diverso dall'alfabeto latino.

- {{HTMLElement('i')}} è utilizzato per trasmettere un significato tradizionalmente trasmesso dal corsivo: parole straniere, designazione tassonomica, termini tecnici, un pensiero…
- {{HTMLElement('b')}} è utilizzato per trasmettere un significato tradizionalmente trasmesso dal grassetto: parole chiave, nomi di prodotti, frase principale…
- {{HTMLElement('u')}} è utilizzato per trasmettere un significato tradizionalmente trasmesso dalla sottolineatura: nome proprio, errore di ortografia…

> [!NOTE]
> Le persone associano fortemente la sottolineatura ai collegamenti ipertestuali. Pertanto, sul web, è meglio sottolineare solo i collegamenti. Usare l'elemento `<u>` quando è semanticamente appropriato, ma considerare di utilizzare il CSS per cambiare la sottolineatura predefinita in qualcosa di più appropriato sul web. L'esempio qui sotto illustra come può essere fatto.

<!-- cSpell:ignore spel -->

```html
<!-- scientific names -->
<p>
  The Ruby-throated Hummingbird (<i>Archilochus colubris</i>) is the most common
  hummingbird in Eastern North America.
</p>

<!-- foreign words -->
<p>
  The menu was a sea of exotic words like <i lang="uk-latn">vatrushka</i>,
  <i lang="id">nasi goreng</i> and <i lang="fr">soupe à l'oignon</i>.
</p>

<!-- a known misspelling -->
<p>Someday I'll learn how to <u class="spelling-error">spel</u> better.</p>

<!-- term being defined when used in a definition -->
<dl>
  <dt>Semantic HTML</dt>
  <dd>
    Use the elements based on their <b>semantic</b> meaning, not their
    appearance.
  </dd>
</dl>
```

{{EmbedLiveSample('Italic, bold, underline…','100%','270')}}

## Riepilogo

Abbiamo finito di esaminare enfasi e importanza per il momento. Passiamo a vedere come rappresentiamo le liste in HTML.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Headings_and_paragraphs", "Learn_web_development/Core/Structuring_content/Lists", "Learn_web_development/Core/Structuring_content")}}
