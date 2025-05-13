---
title: Inclusione di grafica vettoriale in HTML
short-title: Grafica vettoriale
slug: Learn_web_development/Core/Structuring_content/Including_vector_graphics_in_HTML
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

La grafica vettoriale è molto utile in molte circostanze: ha dimensioni di file ridotte ed è altamente scalabile, quindi non si pixellizza quando si effettua lo zoom o si ingrandisce a una dimensione elevata. In questo articolo le mostreremo come includerne una nella sua pagina web.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Dovrebbe conoscere le
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">basi dell'HTML</a>
        e come
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_images" >inserire un'immagine nel suo documento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Imparare a incorporare un'immagine SVG (vettoriale) in una pagina web.</td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> Questo articolo non ha l'intenzione di insegnarle SVG; solo cosa sia e come aggiungerlo alle pagine web.

## Cosa sono le grafiche vettoriali?

Sul web, lavorerà con due tipi di immagini: **immagini raster** e **immagini vettoriali**:

- **Immagini raster** sono definite usando una griglia di pixel — un file di immagine raster contiene informazioni che mostrano esattamente dove ogni pixel deve essere posizionato e di che colore deve essere. I formati raster web popolari includono Bitmap (`.bmp`), PNG (`.png`), JPEG (`.jpg`), e GIF (`.gif`).
- **Immagini vettoriali** sono definite utilizzando algoritmi — un file di immagine vettoriale contiene definizioni di forme e percorsi che il computer può utilizzare per determinare come l'immagine dovrebbe apparire quando viene renderizzata sullo schermo. Il formato {{Glossary("SVG", "SVG")}} ci consente di creare potenti grafiche vettoriali per l'uso sul Web.

Per darle un'idea della differenza tra i due, diamo un'occhiata ad un esempio. Può trovare questo esempio live sul nostro repository GitHub come [vector-versus-raster.html](https://mdn.github.io/learning-area/html/multimedia-and-embedding/adding-vector-graphics-to-the-web/vector-versus-raster.html) — mostra due immagini apparentemente identiche affiancate, di una stella rossa con un'ombra nera. La differenza è che quella a sinistra è un PNG, e quella a destra è un'immagine SVG.

La differenza diventa evidente quando si ingrandisce la pagina: l'immagine PNG diventa pixellata quando si esegue lo zoom perché contiene informazioni su dove ogni pixel dovrebbe essere (e di che colore). Quando viene ingrandita, ogni pixel viene aumentato di dimensione per riempire più pixel sullo schermo, quindi l'immagine inizia ad apparire blocchettata. L'immagine vettoriale continua invece a sembrare bella e nitida, perché, indipendentemente dalla dimensione, gli algoritmi vengono utilizzati per calcolare le forme nell'immagine, con i valori che vengono scalati man mano che si ingrandisce.

![Due immagini di stelle](raster-vector-default-size.png)

![Due immagini di stelle ingrandite, una nitida e l'altra sfocata](raster-vector-zoomed.png)

> [!NOTE]
> Le immagini sopra in realtà sono tutti PNG — con la stella a sinistra in ciascun caso che rappresenta un'immagine raster e la stella a destra che rappresenta un'immagine vettoriale. Ancora una volta, vada alla demo [vector-versus-raster.html](https://mdn.github.io/learning-area/html/multimedia-and-embedding/adding-vector-graphics-to-the-web/vector-versus-raster.html) per un esempio reale!

Inoltre, i file di immagini vettoriali sono molto più leggeri rispetto ai loro equivalenti raster, perché devono contenere solo un insieme di algoritmi, piuttosto che informazioni su ogni singolo pixel nell'immagine.

## Cos'è SVG?

[SVG](/it/docs/Web/SVG) è un linguaggio basato su {{Glossary("XML", "XML")}} per descrivere immagini vettoriali. È sostanzialmente markup, come HTML, tranne per il fatto che ci sono molti elementi diversi per definire le forme che intende far apparire nella sua immagine e gli effetti che intende applicare a quelle forme. SVG serve per il markup della grafica, non dei contenuti. SVG definisce elementi per creare forme base, come {{svgelement("circle")}} e {{svgelement("rect")}}, oltre ad elementi per creare forme più complesse, come {{svgelement("path")}} e {{svgelement("polygon")}}. Le funzionalità SVG più avanzate includono {{svgelement("feColorMatrix")}} (trasformare i colori usando una matrice di trasformazione), {{svgelement("animate")}} (animare parti della sua grafica vettoriale), e {{svgelement("mask")}} (applicare una maschera sull'immagine).

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

Dall'esempio sopra, potrebbe impressionarsi che SVG sia facile da codificare a mano. Sì, può codificare manualmente semplici SVG in un editor di testo, ma per un'immagine complessa questo diventa rapidamente molto difficile. Per creare immagini SVG, la maggior parte delle persone usa un editor di grafica vettoriale come [Inkscape](https://inkscape.org/) o [Illustrator](https://en.wikipedia.org/wiki/Adobe_Illustrator). Questi pacchetti le consentono di creare una varietà di illustrazioni utilizzando diversi strumenti grafici, e creare approssimazioni di foto (ad esempio la funzione Traccia Bitmap di Inkscape).

SVG ha alcuni vantaggi aggiuntivi oltre a quelli già descritti:

- Il testo nelle immagini vettoriali rimane accessibile (il che avvantaggia anche la sua {{Glossary("SEO", "SEO")}}).
- Gli SVG si prestano bene allo stile/scripting, poiché ogni componente dell'immagine è un elemento che può essere stilizzato tramite CSS o programmato via JavaScript.

Quindi perché qualcuno dovrebbe voler usare grafiche raster rispetto a SVG? Beh, SVG ha alcuni svantaggi:

- SVG può diventare complicato molto rapidamente, il che significa che le dimensioni dei file possono crescere; SVG complessi possono anche richiedere un tempo di elaborazione significativo nel browser.
- SVG può essere più difficile da creare rispetto alle immagini raster, a seconda del tipo di immagine che sta cercando di creare.

Le grafiche raster sono probabilmente migliori per immagini complesse di precisione come le foto, per i motivi descritti sopra.

> [!NOTE]
> In Inkscape, salvi i suoi file come Plain SVG per risparmiare spazio. Inoltre, faccia riferimento a questo [articolo che descrive come preparare SVG per il Web](http://tavmjong.free.fr/INKSCAPE/MANUAL/html/Web-Inkscape.html).

## Aggiungere SVG alle sue pagine

In questa sezione, esamineremo i diversi modi in cui può aggiungere grafiche vettoriali SVG alle sue pagine web.

### Il modo rapido: elemento `img`

Per incorporare un SVG tramite un elemento {{htmlelement("img")}}, deve semplicemente farvi riferimento nell'attributo src, come ci si aspetterebbe. Avrà bisogno di un attributo `height` o `width` (o entrambi se il suo SVG non ha un {{Glossary("aspect_ratio", "rapporto d'aspetto")}} intrinseco). Se non l'ha già fatto, legga [immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images).

```html
<img
  src="equilateral.svg"
  alt="triangle with all three sides equal"
  height="87"
  width="100" />
```

#### Pro

- Sintassi familiare per le immagini con equivalente di testo incorporato disponibile nell'attributo `alt`.
- Può trasformare facilmente l'immagine in un collegamento ipertestuale annidando il `<img>` all'interno di un elemento {{htmlelement("a")}}.
- Il file SVG può essere memorizzato nella cache dal browser, risultando in tempi di caricamento più rapidi per qualsiasi pagina che utilizza l'immagine caricata in futuro.

#### Contro

- Non può manipolare l'immagine con JavaScript.
- Se desidera controllare il contenuto SVG con CSS, deve includere stili CSS inline nel suo codice SVG. (I fogli di stile esterni richiamati dal file SVG non hanno effetto.)
- Non può ritoccare l'immagine con pseudoclassi CSS (come `:focus`).

### Risoluzione dei problemi e supporto cross-browser

Per i browser che non supportano SVG (IE 8 e versioni precedenti, Android 2.3 e versioni precedenti), potrebbe fare riferimento a un PNG o JPG dal suo attributo `src` e utilizzare un attributo [`srcset`](/it/docs/Web/HTML/Reference/Elements/img#srcset) (che solo i browser recenti riconoscono) per fare riferimento all'SVG. In questo caso, solo i browser supportati caricheranno l'SVG — i browser meno recenti caricheranno invece il PNG:

```html
<img
  src="equilateral.png"
  alt="triangle with equal sides"
  srcset="equilateral.svg" />
```

Può anche usare gli SVG come immagini di sfondo CSS, come mostrato di seguito. Nel codice seguente, i browser meno recenti si attengono al PNG che comprendono, mentre i browser più recenti caricheranno l'SVG:

```css
background: url("fallback.png") no-repeat center;
background-image: url("image.svg");
background-size: contain;
```

Come il metodo `<img>` descritto sopra, inserire SVG usando immagini di sfondo CSS significa che l'SVG non può essere manipolato con JavaScript e è soggetto alle stesse limitazioni CSS.

Se i suoi SVG non vengono visualizzati affatto, potrebbe essere perché il suo server non è configurato correttamente. Se questo è il problema, questo [articolo la indirizzerà nella giusta direzione](/it/docs/Web/SVG/Tutorials/SVG_from_scratch/Getting_started#a_word_on_web_servers_for_.svgz_files).

### Come includere il codice SVG all'interno del suo HTML

Può anche aprire il file SVG in un editor di testo, copiare il codice SVG e incollarlo nel suo documento HTML — questo a volte è chiamato mettere il suo **SVG inline**, o **inlining SVG**. Si assicuri che il suo frammento di codice SVG inizi con un tag iniziale `<svg>` e termini con un tag finale `</svg>`. Ecco un esempio molto semplice di cosa potrebbe incollare nel suo documento:

```html
<svg width="300" height="200">
  <rect width="100%" height="100%" fill="green" />
</svg>
```

#### Pro

- Mettere il suo SVG inline risparmia una richiesta HTTP e può ridurre un po' il tempo di caricamento.
- Può assegnare `classi` e `id` agli elementi SVG e stilarli con CSS, sia all'interno dell'SVG che ovunque metta le regole di stile CSS per il suo documento HTML. In effetti, può utilizzare qualsiasi [attributo di presentazione SVG](/it/docs/Web/SVG/Reference/Attribute#presentation_attributes) come proprietà CSS.
- Inlining SVG è l'unico approccio che le consente di utilizzare interazioni CSS (come `:focus`) e animazioni CSS sulla sua immagine SVG (anche nel suo foglio di stile normale).
- Può trasformare il markup SVG in un collegamento ipertestuale avvolgendolo in un elemento {{htmlelement("a")}}.

#### Contro

- Questo metodo è adatto solo se sta utilizzando l'SVG in un solo posto. La duplicazione rende la manutenzione delle risorse intensiva.
- Il codice SVG extra aumenta la dimensione del suo file HTML.
- Il browser non può memorizzare nella cache l'SVG inline come farebbe con le risorse di immagini regolari, quindi le pagine che includono l'immagine non si caricheranno più velocemente dopo che la prima pagina contenente l'immagine è stata caricata.
- Può includere un fallback in un elemento {{svgelement("foreignObject")}}, ma i browser che supportano SVG scaricano ancora eventuali immagini di fallback. Deve valutare se l'overhead extra è veramente utile, solo per supportare browser obsoleti.

### Come incorporare un SVG con un `iframe`

Può aprire immagini SVG nel suo browser proprio come le pagine web. Quindi incorporare un documento SVG con un `<iframe>` viene fatto proprio come abbiamo studiato in [Da \<object> a \<iframe> — tecnologie di incorporamento generali](/it/docs/Learn_web_development/Core/Structuring_content/General_embedding_technologies).

Ecco una rapida revisione:

```html
<iframe src="triangle.svg" width="500" height="500" sandbox>
  <img src="triangle.png" alt="Triangle with three unequal sides" />
</iframe>
```

Questo non è decisamente il miglior metodo da scegliere:

#### Contro

- Gli `iframe` hanno un meccanismo di fallback, come può vedere, ma i browser visualizzano il fallback solo se non supportano affatto gli `iframe`.
- Inoltre, a meno che l'SVG e la sua pagina web corrente abbiano la stessa {{Glossary("origin", "origine")}}, non può usare JavaScript sulla sua pagina web principale per manipolare l'SVG.

## Apprendimento attivo: Giocare con SVG

In questa sezione di apprendimento attivo, vogliamo che provi a giocare con qualche SVG per divertimento. Nella sezione _Input_ qui sotto vedrà che le abbiamo già fornito alcuni esempi per iniziare. Può anche andare al [Riferimento Elemento SVG](/it/docs/Web/SVG/Reference/Element), scoprire maggiori dettagli su altri giocattoli che può usare in SVG e provare anche quelli. Questa sezione è tutta dedicata a praticare le sue abilità di ricerca e a divertirsi.

Se si blocca e non riesce a far funzionare il suo codice, può sempre resettarlo usando il pulsante _Reset_.

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

## Riepilogo

Questo articolo le ha fornito un rapido tour di cosa siano le grafiche vettoriali e SVG, perché siano utili da conoscere e come includere SVG all'interno delle sue pagine web. Non è mai stato inteso come una guida completa per imparare SVG, ma piuttosto come un'indicazione affinché sappia cosa sia SVG se lo incontra nei suoi viaggi sul Web. Quindi non si preoccupi se non si sente ancora un esperto di SVG. Abbiamo incluso alcuni link qui sotto che potrebbero aiutarla se desidera andare a scoprire di più su come funziona.

## Vedere anche

- [Tutorial SVG](/it/docs/Web/SVG/Tutorials/SVG_from_scratch/Getting_started) su MDN
- [Tutorial di Sara Soueidan su immagini SVG reattive](https://tympanus.net/codrops/2014/08/19/making-svgs-responsive-with-css/)
- [Benefici di accessibilità di SVG](https://www.w3.org/TR/SVG-access/)
- [Proprietà SVG e CSS](https://css-tricks.com/svg-properties-and-css/)
- [Come scalare gli SVG](https://css-tricks.com/scale-svg/) (non è semplice come con grafiche raster!)
