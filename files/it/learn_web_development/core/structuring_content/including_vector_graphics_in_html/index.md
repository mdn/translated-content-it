---
title: Includere grafica vettoriale in HTML
short-title: Grafica vettoriale
slug: Learn_web_development/Core/Structuring_content/Including_vector_graphics_in_HTML
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

La grafica vettoriale è molto utile in molte circostanze: ha dimensioni di file ridotte e altamente scalabili, quindi non si sgranano quando ingrandite o ingrandite a una dimensione grande. In questo articolo ti mostreremo come includerne una nella tua pagina web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Dovresti conoscere il
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">le basi dell'HTML</a>
        e come
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_images">inserire un'immagine nel tuo documento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare come incorporare un'immagine SVG (vettoriale) in una pagina web.</td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Questo articolo non ha l'intenzione di insegnarti SVG; solo cosa è e come aggiungerlo alle pagine web.

## Cosa sono le grafica vettoriale?

Sul web, lavorerai con due tipi di immagini: **immagini raster** e **immagini vettoriali**:

- **Immagini raster** sono definite usando una griglia di pixel — un file di immagine raster contiene informazioni che mostrano esattamente dove ogni pixel deve essere piazzato e di che colore deve essere. I formati raster web popolari includono Bitmap (`.bmp`), PNG (`.png`), JPEG (`.jpg`), e GIF (`.gif`).
- **Immagini vettoriali** sono definite usando algoritmi — un file di immagine vettoriale contiene definizioni di forme e percorsi che il computer può usare per capire come l'immagine dovrebbe apparire quando viene resa sullo schermo. Il formato {{Glossary("SVG", "SVG")}} ci permette di creare grafica vettoriale potente da usare sul Web.

Per darti un'idea della differenza tra le due, diamo un'occhiata a un esempio. Puoi trovare questo esempio live nel nostro repository GitHub come [vector-versus-raster.html](https://mdn.github.io/learning-area/html/multimedia-and-embedding/adding-vector-graphics-to-the-web/vector-versus-raster.html) — mostra due immagini apparentemente identiche affiancate, di una stella rossa con un'ombra nera. La differenza è che quella a sinistra è un PNG, e quella a destra è un'immagine SVG.

La differenza diventa evidente quando ingrandisci la pagina — l'immagine PNG diventa pixelata man mano che ingrandisci perché contiene informazioni su dove dovrebbe essere ogni pixel (e di che colore). Quando viene ingrandita, ogni pixel viene aumentato di dimensione per riempire più pixel sullo schermo, quindi l'immagine inizia a sembrare a blocchi. L'immagine vettoriale invece continua a sembrare nitida e chiara, perché non importa quale dimensione abbia, gli algoritmi vengono usati per calcolare le forme dell'immagine, con i valori che vengono scalati man mano che diventa più grande.

![Due immagini di stelle](raster-vector-default-size.png)

![Due immagini di stelle ingrandite, una nitida e l'altra sfocata](raster-vector-zoomed.png)

> [!NOTE]
> Le immagini sopra sono in realtà tutte PNG — con la stella a sinistra in ciascun caso che rappresenta un'immagine raster, e la stella a destra che rappresenta un'immagine vettoriale. Ancora una volta, vai alla demo [vector-versus-raster.html](https://mdn.github.io/learning-area/html/multimedia-and-embedding/adding-vector-graphics-to-the-web/vector-versus-raster.html) per un esempio reale!

Inoltre, i file di immagine vettoriale sono molto più leggeri dei loro equivalenti raster, perché devono contenere solo una manciata di algoritmi, piuttosto che informazioni su ogni singolo pixel dell'immagine.

## Cos'è SVG?

[SVG](/it/docs/Web/SVG) è un linguaggio basato su {{Glossary("XML", "XML")}} per descrivere immagini vettoriali. È fondamentalmente un markup, come HTML, tranne che ci sono molti elementi diversi per definire le forme che vuoi appaiano nella tua immagine e gli effetti che vuoi applicare a quelle forme. SVG serve a marcare la grafica, non il contenuto. SVG definisce elementi per creare forme di base, come {{svgelement("circle")}} e {{svgelement("rect")}}, così come elementi per creare forme più complesse, come {{svgelement("path")}} e {{svgelement("polygon")}}. Funzionalità SVG più avanzate includono {{svgelement("feColorMatrix")}} (trasformare i colori usando una matrice di trasformazione), {{svgelement("animate")}} (animare parti della tua grafica vettoriale), e {{svgelement("mask")}} (applicare una maschera sopra la tua immagine).

Come esempio di base, il seguente codice crea un cerchio e un rettangolo:

```html
<svg
  version="1.1"
  baseProfile="full"
  width="300"
  height="200"
  xmlns="http://www.w3.org/2000/svg">
  <rect width="100%" height="100%" fill="black" />
  <circle cx="150" cy="100" r="90" fill="blue" />
</svg>
```

Questo crea il seguente output:

{{ EmbedLiveSample('What_is_SVG', 300, 240, "", "") }}

Dall'esempio sopra, potresti avere l'impressione che SVG sia facile da scrivere a mano. Sì, puoi scrivere a mano SVG semplici con un editor di testo, ma per un'immagine complessa questo diventa rapidamente molto difficile. Per creare immagini SVG, la maggior parte delle persone usa un editor di grafica vettoriale come [Inkscape](https://inkscape.org/) o [Illustrator](https://en.wikipedia.org/wiki/Adobe_Illustrator). Questi pacchetti ti permettono di creare una varietà di illustrazioni usando vari strumenti grafici e creare approssimazioni di foto (ad esempio, la funzione di Tracciamento Bitmap di Inkscape).

SVG ha alcuni vantaggi aggiuntivi rispetto a quelli descritti finora:

- Il testo nelle immagini vettoriali rimane accessibile (il che beneficia anche il tuo {{Glossary("SEO", "SEO")}}).
- Gli SVG si prestano bene allo stile/script, perché ogni componente dell'immagine è un elemento che può essere stilizzato via CSS o scritturato via JavaScript.

Quindi perché qualcuno dovrebbe voler usare la grafica raster invece di SVG? Bene, SVG ha alcuni svantaggi:

- SVG può complicarsi molto rapidamente, il che significa che le dimensioni dei file possono crescere; SVG complessi possono anche richiedere un tempo significativo di elaborazione nel browser.
- SVG può essere più difficile da creare rispetto alle immagini raster, a seconda del tipo di immagine che stai cercando di creare.

Le grafiche raster sono probabilmente migliori per immagini di precisione complesse come le foto, per i motivi sopra descritti.

> [!NOTE]
> In Inkscape, salva i tuoi file come SVG Semplice per risparmiare spazio. Inoltre, fai riferimento a questo [articolo che descrive come preparare SVG per il Web](http://tavmjong.free.fr/INKSCAPE/MANUAL/html/Web-Inkscape.html).

## Aggiungere SVG alle tue pagine

In questa sezione esploreremo i diversi modi in cui puoi aggiungere grafica vettoriale SVG alle tue pagine web.

### Il modo rapido: elemento `img`

Per incorporare un SVG tramite un elemento {{htmlelement("img")}}, devi solo riferirlo nell'attributo src come ti aspetteresti. Avrai bisogno di un attributo `height` o `width` (o entrambi se il tuo SVG non ha un {{Glossary("aspect_ratio", "rapporto d'aspetto")}} intrinseco). Se non l'hai già fatto, leggi [Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images).

```html
<img
  src="equilateral.svg"
  alt="triangle with all three sides equal"
  height="87"
  width="100" />
```

#### Pro

- Sintassi dell'immagine rapida e familiare con equivalente testuale incorporato disponibile nell'attributo `alt`.
- Puoi trasformare facilmente l'immagine in un collegamento ipertestuale nidificando l'`<img>` all'interno di un elemento {{htmlelement("a")}}.
- Il file SVG può essere memorizzato nella cache dal browser, risultando in tempi di caricamento più veloci per qualsiasi pagina che utilizza l'immagine successivamente caricata.

#### Contro

- Non puoi manipolare l'immagine con JavaScript.
- Se vuoi controllare il contenuto SVG con CSS, devi includere stili CSS inline nel tuo codice SVG. (I fogli di stile esterni invocati dal file SVG non hanno effetto.)
- Non puoi restilizzare l'immagine con pseudoclassi CSS (come `:focus`).

### Risoluzione dei problemi e supporto cross-browser

Per i browser che non supportano SVG (IE 8 e versioni precedenti, Android 2.3 e versioni precedenti), potresti fare riferimento a un PNG o JPG dal tuo attributo `src` e usare un attributo [`srcset`](/it/docs/Web/HTML/Reference/Elements/img#srcset) (che solo i browser recenti riconoscono) per fare riferimento all'SVG. In questo modo, solo i browser che lo supportano caricheranno l'SVG — i browser più vecchi caricheranno invece il PNG:

```html
<img
  src="equilateral.png"
  alt="triangle with equal sides"
  srcset="equilateral.svg" />
```

Puoi anche usare SVG come immagini di sfondo CSS, come mostrato sotto. Nel codice qui sotto, i browser più vecchi si fermeranno al PNG che conoscono, mentre i browser più recenti caricheranno l'SVG:

```css
background: url("fallback.png") no-repeat center;
background-image: url("image.svg");
background-size: contain;
```

Come il metodo `<img>` descritto sopra, inserire SVG usando immagini di sfondo CSS significa che l'SVG non può essere manipolato con JavaScript ed è soggetto alle stesse limitazioni CSS.

Se i tuoi SVG non compaiono affatto, potrebbe essere perché il tuo server non è configurato correttamente. Se questo è il problema, questo [articolo ti indirizzerà nella giusta direzione](/it/docs/Web/SVG/Tutorials/SVG_from_scratch/Getting_started#a_word_on_web_servers_for_.svgz_files).

### Come includere il codice SVG all'interno del tuo HTML

Puoi anche aprire il file SVG in un editor di testo, copiare il codice SVG e incollarlo nel tuo documento HTML — questo viene talvolta chiamato inserire il tuo **SVG inline**, o **inlining SVG**. Assicurati che il tuo snippet di codice SVG inizi con un tag di apertura `<svg>` e termini con un tag di chiusura `</svg>`. Ecco un esempio molto semplice di cosa potresti incollare nel tuo documento:

```html
<svg width="300" height="200">
  <rect width="100%" height="100%" fill="green" />
</svg>
```

#### Pro

- Mettere il tuo SVG inline salva una richiesta HTTP e quindi può ridurre un po' i tempi di caricamento.
- Puoi assegnare classi e id agli elementi SVG e stilizzarli con CSS, sia all'interno dell'SVG sia ovunque tu metta le regole di stile CSS per il tuo documento HTML. In effetti, puoi usare qualsiasi [attributo di presentazione SVG](/it/docs/Web/SVG/Reference/Attribute#presentation_attributes) come una proprietà CSS.
- Inlining SVG è l'unico approccio che ti permette di usare interazioni CSS (come `:focus`) e animazioni CSS sulla tua immagine SVG (anche nel tuo foglio di stile regolare).
- Puoi trasformare il markup SVG in un collegamento ipertestuale avvolgendolo in un elemento {{htmlelement("a")}}.

#### Contro

- Questo metodo è adatto solo se stai usando l'SVG in un solo posto. La duplicazione rende la manutenzione onerosa.
- Il codice SVG extra aumenta la dimensione del tuo file HTML.
- Il browser non può memorizzare nella cache l'SVG inline come farebbe con le risorse di immagini regolari, quindi le pagine che includono l'immagine non si caricheranno più velocemente dopo che la prima pagina contenente l'immagine è stata caricata.
- Puoi includere fallback in un elemento {{svgelement("foreignObject")}}, ma i browser che supportano SVG scaricano comunque tutte le immagini di fallback. Devi valutare se il sovraccarico extra è davvero utile, solo per supportare i browser obsoleti.

### Come incorporare un SVG con un `iframe`

Puoi aprire immagini SVG nel tuo browser proprio come pagine web. Quindi incorporare un documento SVG con un `<iframe>` viene fatto proprio come abbiamo studiato in [Da \<object> a \<iframe> — tecnologie di incorporazione generali](/it/docs/Learn_web_development/Core/Structuring_content/General_embedding_technologies).

Ecco una rapida recensione:

```html
<iframe src="triangle.svg" width="500" height="500" sandbox>
  <img src="triangle.png" alt="Triangle with three unequal sides" />
</iframe>
```

Questo non è sicuramente il metodo migliore da scegliere:

#### Contro

- Gli `iframe` hanno un meccanismo di fallback, come puoi vedere, ma i browser mostrano il fallback solo se non hanno supporto per `iframe` del tutto.
- Inoltre, a meno che l'SVG e la tua pagina web attuale abbiano la stessa {{Glossary("origin", "origin")}}, non puoi usare JavaScript sulla tua pagina principale per manipolare l'SVG.

## Apprendimento Attivo: Giocare con SVG

In questa sezione di apprendimento attivo ti invitiamo a divertirti giocando con alcuni SVG. Nella sezione _Input_ qui sotto vedrai che ti abbiamo già fornito alcuni esempi per iniziare. Puoi anche andare al [Riferimento Elementi SVG](/it/docs/Web/SVG/Reference/Element), scoprire più dettagli sugli altri strumenti che puoi usare in SVG, e provarli. Questa sezione è tutta incentrata sulla pratica delle tue abilità di ricerca e sul divertimento.

Se rimani bloccato e non riesci a far funzionare il tuo codice, puoi sempre reimpostarlo usando il pulsante _Reset_.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 50px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="input" style="width: 95%;min-height: 200px;">
  <svg width="100%" height="100%">
    <rect width="100%" height="100%" fill="red" />
    <circle cx="100%" cy="100%" r="150" fill="blue" stroke="black" />
    <polygon points="120,0 240,225 0,225" fill="green"/>
    <text x="50" y="100" font-family="Verdana" font-size="55"
          fill="white" stroke="black" stroke-width="2">
            Hello!
    </text>
  </svg>
</textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" disabled />
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
let code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  output.innerHTML = textarea.value;
}

reset.addEventListener("click", function () {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = htmlSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", function () {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

const htmlSolution = "";
let solutionEntry = htmlSolution;

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = function (e) {
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

{{ EmbedLiveSample('Active_Learning_Playing_with_SVG', 700, 540) }}

## Sommario

Questo articolo ti ha fornito un rapido tour di cosa sono le grafiche vettoriali e SVG, perché sono utili da conoscere, e come includere SVG all'interno delle tue pagine web. Non è mai stato inteso come una guida completa per apprendere SVG, solo un punto di riferimento per sapere cosa è l'SVG se lo incontri nelle tue esplorazioni sul Web. Quindi non preoccuparti se non ti senti ancora un esperto di SVG. Abbiamo incluso alcuni link qui sotto che potrebbero aiutarti se desideri approfondire come funziona.

## Vedi anche

- [Tutorial SVG](/it/docs/Web/SVG/Tutorials/SVG_from_scratch/Getting_started) su MDN
- [Tutorial di Sara Soueidan su immagini SVG responsive](https://tympanus.net/codrops/2014/08/19/making-svgs-responsive-with-css/)
- [Benefici di accessibilità di SVG](https://www.w3.org/TR/SVG-access/)
- [SVG Properties and CSS](https://css-tricks.com/svg-properties-and-css/)
- [How to scale SVGs](https://css-tricks.com/scale-svg/) (non è semplice come la grafica raster!)
