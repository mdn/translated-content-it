---
title: Fondamenti delle media query
short-title: Media query
slug: Learn_web_development/Core/CSS_layout/Media_queries
l10n:
  sourceCommit: 4c58f4735f986a91bee1b77e336143630df727a2
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Responsive_Design", "Learn_web_development/Core/CSS_layout/Test_your_skills/Responsive_design", "Learn_web_development/Core/CSS_layout")}}

Le **CSS Media Query** consentono di applicare CSS solo quando l'ambiente del browser e del dispositivo corrisponde a una regola specificata, ad esempio "la viewport è più larga di 480 pixel". Le media query sono una parte fondamentale del [responsive web design](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design), poiché consentono di creare layout diversi in base alle dimensioni della viewport, ma possono essere utilizzate anche per rilevare altre caratteristiche dell'ambiente in cui viene eseguito il sito, ad esempio se l'utente utilizza un touchscreen anziché un mouse.

In questa lezione verrà prima illustrata la sintassi utilizzata nelle media query, quindi verranno usate in esempi che mostrano come rendere responsivo un design di base.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare contenuti con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Fondamenti dello stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti dello stile del testo e dei font</a>,
        familiarità con i <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>La sintassi delle media query.</li>
          <li>I tipi comuni di media query.</li>
          <li>L'uso delle media query <code>width</code> e <code>height</code> per creare layout responsivi.</li>
          <li>La scelta dei breakpoint.</li>
          <li>L'uso delle media query per implementare un design mobile-first.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Concetti di base delle media query

La sintassi più semplice di una media query è la seguente:

```css
@media media-type and (media-feature-rule) {
  /* CSS rules go here */
}
```

È composta da:

- Un tipo di media, che indica al browser per quale tipo di supporto è destinato questo codice (stampa o schermo).
- Un'espressione media, ovvero una regola o un test che deve essere superato affinché venga applicato il CSS contenuto.
- Un insieme di regole CSS che verranno applicate se il test viene superato e il tipo di media è corretto.

### Tipi di media

I possibili tipi di media specificabili sono:

- `all`
- `print`
- `screen`

La seguente media query imposterà il body a 12pt solo se la pagina viene stampata. Non verrà applicata quando la pagina è caricata in un browser.

```css
@media print {
  body {
    font-size: 12pt;
  }
}
```

> [!NOTE]
> Il tipo di media qui è diverso dal cosiddetto {{Glossary("MIME_type", "tipo MIME")}}.
> Nella specifica Media Queries Level 3 erano definiti diversi altri tipi di media; questi sono stati deprecati e dovrebbero essere evitati.
> I tipi di media sono facoltativi; se non viene indicato un tipo di media nella media query, questa sarà per impostazione predefinita destinata a tutti i tipi di media.

### Regole delle media feature

Dopo aver specificato il tipo, è possibile indirizzare una media feature con una regola.
Gli esempi seguenti mostrano come utilizzare diverse media query.
Per modificare la `width` dello schermo, modificare le dimensioni del browser o ruotare il dispositivo portatile.

> [!NOTE]
> In alternativa, è possibile usare le funzionalità di ridimensionamento responsivo degli strumenti di sviluppo del browser (come [Responsive Design Mode](https://firefox-source-docs.mozilla.org/devtools-user/responsive_design_mode/) di Firefox) per simulare diverse larghezze dei dispositivi.

#### Larghezza e altezza

La feature che viene rilevata più spesso per creare design responsivi, e che dispone di un ampio supporto nei browser, è la larghezza della viewport. È possibile applicare CSS se la viewport è superiore o inferiore a una determinata larghezza, oppure ha una larghezza esatta, usando la media feature `width` e anteponendo `min-` o `max-` secondo necessità.

Queste feature vengono utilizzate per creare layout che rispondono a dimensioni dello schermo diverse. Ad esempio, per impostare il colore del testo del body su rosso se la viewport è esattamente di 600 pixel, si utilizzerebbe la seguente media query.

```css live-sample___width
@media screen and (width: 600px) {
  body {
    color: red;
  }
}
```

```html live-sample___width
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("width")}}

Provare a regolare la larghezza della finestra del browser per trovare il punto preciso in cui la demo precedente è larga esattamente `600px`, così che il testo diventi rosso.

Le media feature `width` e `height` possono essere utilizzate come intervalli e, pertanto, possono avere il prefisso `min-` o `max-` per indicare che il valore fornito è rispettivamente un minimo o un massimo. Ad esempio, per rendere il colore blu se la viewport è di 600 pixel o più stretta, usare `max-width`:

```css live-sample___max-width
@media screen and (max-width: 600px) {
  body {
    color: blue;
  }
}
```

```html hidden live-sample___max-width
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("max-width")}}

Provare a restringere la finestra finché il testo precedente non diventa blu.

In pratica, usare valori minimi o massimi è molto più utile per il design responsivo, quindi raramente si vedranno `width` o `height` utilizzati da soli.

Esistono molte altre media feature che è possibile verificare, sebbene alcune delle feature più recenti introdotte nei livelli 4 e 5 della specifica media queries abbiano un supporto limitato nei browser. Ogni feature è documentata su MDN insieme alle informazioni sul supporto dei browser; un elenco completo è disponibile in [Usare le media query: Sintassi](/it/docs/Web/CSS/Guides/Media_queries/Using#syntax).

#### Orientamento

Una media feature con buon supporto è `orientation`, che consente di verificare l'orientamento verticale o orizzontale. Per modificare il colore del testo del body se il dispositivo è in orientamento orizzontale, usare la seguente media query.

```css live-sample___orientation
@media (orientation: landscape) {
  body {
    color: rebeccapurple;
  }
}
```

```html hidden live-sample___orientation
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("orientation")}}

L'esempio precedente è piuttosto difficile da testare nella pagina; per vederlo in azione, si consiglia di copiare il codice precedente in un file HTML locale e aprirlo in una scheda separata.

Una visualizzazione desktop standard ha un orientamento orizzontale e un design che funziona bene in questo orientamento potrebbe non funzionare altrettanto bene se visualizzato su un telefono o tablet in modalità verticale. Verificare l'orientamento può aiutare a creare un layout ottimizzato per i dispositivi in modalità verticale.

#### Uso dei dispositivi di puntamento

Nella specifica Level 4 è stata introdotta la media feature `hover`. Questa feature consente di verificare se l'utente è in grado di passare il puntatore sopra un elemento, il che significa essenzialmente che utilizza un qualche tipo di dispositivo di puntamento; la navigazione tramite touchscreen e tastiera non dispone di hover.

```css live-sample___hover-example
@media screen and (hover: hover) {
  body:hover {
    color: white;
    background: black;
  }
}
```

```html hidden live-sample___hover-example
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("hover-example")}}

L'esempio precedente cambia in testo bianco su sfondo nero al passaggio del puntatore, ma solo sui dispositivi in cui l'hover è possibile. Se si sa che l'utente non può usare l'hover, alcune funzionalità interattive potrebbero essere visualizzate per impostazione predefinita. Per gli utenti che possono usare l'hover, si potrebbe scegliere di renderle disponibili quando il puntatore passa sopra un link.

Sempre nel Level 4 è presente la media feature `pointer`. Questa accetta tre possibili valori: `none`, `fine` e `coarse`. Un puntatore `fine` è qualcosa come un mouse o un trackpad. Consente all'utente di selezionare con precisione una piccola area. Un puntatore `coarse` è un dito su un touchscreen. Il valore `none` significa che l'utente non dispone di un dispositivo di puntamento; potrebbe navigare soltanto con la tastiera o con comandi vocali.

L'uso di `pointer` può aiutare a progettare interfacce migliori che rispondono al tipo di interazione dell'utente con uno schermo. Ad esempio, si potrebbero creare aree cliccabili più grandi se si sa che l'utente interagisce con il dispositivo tramite touchscreen.

### Uso della sintassi per intervalli

Un caso comune consiste nel verificare se la larghezza della viewport è compresa tra due valori:

```css
@media (min-width: 30em) and (max-width: 50em) {
  /* … */
}
```

Per migliorare la leggibilità, è possibile usare la sintassi degli intervalli:

```css
@media (30em <= width <= 50em) {
  /* … */
}
```

In questo caso, quindi, gli stili vengono applicati quando la larghezza della viewport è compresa tra `30em` e `50em`.

## Media query più complesse

Con tutte le diverse media query possibili, potrebbe essere necessario combinarle oppure creare elenchi di query, di cui una qualsiasi potrebbe corrispondere.

Come in precedenza, provare a testare gli esempi di questa sezione regolando la larghezza del browser.

### Logica "and" nelle media query

Per combinare media feature è possibile usare `and`, in modo molto simile a come `and` è stato usato sopra per combinare un tipo di media e una feature. Ad esempio, potrebbe essere necessario verificare `width` e `orientation`. Il testo del body sarà blu solo se la viewport è larga almeno 600 pixel e il dispositivo è in modalità orizzontale.

```css live-sample___and
@media screen and (width >= 600px) and (orientation: landscape) {
  body {
    color: blue;
  }
}
```

```html hidden live-sample___and
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("and")}}

### Logica "or" nelle media query

Se si dispone di un insieme di query, di cui una qualsiasi potrebbe corrispondere, è possibile separare queste query con virgole. Nell'esempio seguente il testo sarà blu se la viewport è larga almeno 600 pixel OPPURE il dispositivo è in orientamento orizzontale. Se una di queste condizioni è vera, la query corrisponde.

```css live-sample___or
@media screen and (width >= 600px), screen and (orientation: landscape) {
  body {
    color: blue;
  }
}
```

```html hidden live-sample___or
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("or")}}

### Logica "not" nelle media query

È possibile negare un'intera media query usando l'operatore `not`. Questo inverte il significato dell'intera media query. Pertanto, nell'esempio successivo il testo sarà blu solo se la viewport _non_ è larga almeno 600 pixel.

```css live-sample___not
@media not (width >= 600px) {
  body {
    color: blue;
  }
}
```

```html hidden live-sample___not
<p>
  One November night in the year 1782, so the story runs, two brothers sat over
  their winter fire in the little French town of Annonay, watching the grey
  smoke-wreaths from the hearth curl up the wide chimney. Their names were
  Stephen and Joseph Montgolfier, they were papermakers by trade, and were noted
  as possessing thoughtful minds and a deep interest in all scientific knowledge
  and new discovery.
</p>
```

{{EmbedLiveSample("not")}}

È inoltre possibile usare `not` per negare espressioni specifiche.

```css
@media (not (width < 600px)) and (not (width > 1000px)) {
  body {
    color: blue;
  }
}
```

Questo applicherà gli stili se la larghezza della viewport è compresa tra 600 e 1000 pixel. È equivalente a `(600px <= width <= 1000px)`.

## Come scegliere i breakpoint

Agli inizi del design responsivo, molti designer cercavano di indirizzare dimensioni dello schermo molto specifiche. Venivano pubblicati elenchi delle dimensioni degli schermi di telefoni e tablet popolari, in modo da poter creare design che corrispondessero esattamente a quelle viewport.

Oggi esistono troppi dispositivi, con un'enorme varietà di dimensioni, perché questo sia praticabile. Ciò significa che, anziché indirizzare dimensioni specifiche per tutti i design, un approccio migliore consiste nel modificare il design alla dimensione in cui il contenuto inizia a rompersi in qualche modo. Forse le lunghezze delle righe diventano eccessive, oppure una barra laterale viene compressa e risulta difficile da leggere. Questo è il punto in cui utilizzare una media query per modificare il design rendendolo più adatto allo spazio disponibile. Questo approccio fa sì che le dimensioni esatte del dispositivo utilizzato non siano importanti: ogni intervallo viene gestito. I punti in cui viene introdotta una media query sono chiamati **breakpoint**.

La [Responsive Design Mode](https://firefox-source-docs.mozilla.org/devtools-user/responsive_design_mode/index.html) nei Firefox DevTools è molto utile per determinare dove collocare questi breakpoint. È possibile ridurre e aumentare facilmente la viewport per vedere dove il contenuto trarrebbe beneficio dall'aggiunta di una media query e dall'adeguamento del design.

![Uno screenshot di un layout in visualizzazione mobile nei Firefox DevTools.](rwd-mode.png)

## Design responsivo mobile-first

In generale, è possibile adottare due approcci al design responsivo. Si può iniziare dalla visualizzazione desktop o più ampia e poi aggiungere breakpoint per riorganizzare gli elementi man mano che la viewport diventa più piccola, oppure si può iniziare dalla visualizzazione più piccola e aggiungere layout man mano che la viewport diventa più grande. Questo secondo approccio viene descritto come design responsivo **mobile-first** e spesso è il migliore da seguire.

La visualizzazione per i dispositivi più piccoli è spesso una semplice colonna singola di contenuti, proprio come appare nel flusso normale. Ciò significa che probabilmente non sarà necessario realizzare molto layout per i dispositivi piccoli: ordinando bene il sorgente si otterrà un layout leggibile per impostazione predefinita.

## Creare un proprio design mobile-first

Ora è il momento di mettere in pratica quanto appreso; in questa sezione del tutorial verrà creato un design responsivo mobile-first di base. In un sito di produzione è probabile che ci siano più elementi da modificare nelle media query, ma l'approccio sarà esattamente lo stesso.

### Per iniziare

Il punto di partenza è un documento HTML a cui è applicato del CSS per aggiungere colori di sfondo alle varie parti del layout.

Prima di tutto, copiare il codice HTML dal blocco seguente in un editor di testo, salvarlo come file HTML sul computer e aprirlo nel browser:

```html live-sample___walkthrough
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width" />
  <title>Media Queries: a simple mobile first design, step 1</title>
  <style>
    /* Add styles here */
  </style>
</head>
<div class="wrapper">
  <header>
    <nav>
      <ul>
        <li><a href="">About</a></li>
        <li><a href="">Contact</a></li>
        <li><a href="">Meet the team</a></li>
        <li><a href="">Blog</a></li>
      </ul>
    </nav>
  </header>
  <main>
    <article>
      <div class="content">
        <h1>Veggies!</h1>
        <p>
          Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh
          onion daikon amaranth tatsoi tomatillo melon azuki bean garlic.
        </p>

        <p>
          Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot
          courgette tatsoi pea sprouts fava bean collard greens dandelion okra
          wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.
        </p>

        <p>
          Turnip greens yarrow ricebean rutabaga endive cauliflower sea lettuce
          kohlrabi amaranth water spinach avocado daikon napa cabbage asparagus
          winter purslane kale. Celery potato scallion desert raisin horseradish
          spinach carrot soko. Lotus root water spinach fennel kombu maize
          bamboo shoot green bean swiss chard seakale pumpkin onion chickpea
          gram corn pea. Brussels sprout coriander water chestnut gourd swiss
          chard wakame kohlrabi beetroot carrot watercress. Corn amaranth
          salsify bunya nuts nori azuki bean chickweed potato bell pepper
          artichoke.
        </p>

        <p>
          Nori grape silver beet broccoli kombu beet greens fava bean potato
          quandong celery. Bunya nuts black-eyed pea prairie turnip leek lentil
          turnip greens parsnip. Sea lettuce lettuce water chestnut eggplant
          winter purslane fennel azuki bean earthnut pea sierra leone bologi
          leek soko chicory celtuce parsley jícama salsify.
        </p>
      </div>
      <aside class="related">
        <p>
          All these veggies are brought to you by the
          <a href="https://veggieipsum.com/">Veggie Ipsum generator</a>.
        </p>
      </aside>
    </article>
    <aside class="sidebar">
      <h2>External vegetable-based links</h2>
      <ul>
        <li>
          <a
            href="https://www.thekitchn.com/how-to-cook-broccoli-5-ways-167323">
            How to cook broccoli
          </a>
        </li>
        <li>
          <a href="https://www.bbcgoodfood.com/glossary/swiss-chard">
            Swiss Chard
          </a>
        </li>
        <li>
          <a
            href="https://www.bbcgoodfood.com/recipes/collection/christmas-parsnip">
            Christmas Parsnip Recipes
          </a>
        </li>
      </ul>
    </aside>
  </main>

  <footer>
    <p>&copy; 2024</p>
  </footer>
</div>
```

Il sorgente del documento è ordinato in modo da rendere il contenuto leggibile. Questo è un importante primo passo, che garantisce che il contenuto sia comprensibile anche se letto ad alta voce da uno screen reader.

Gli stili iniziali per l'esempio sono i seguenti; copiarli nel file HTML all'interno dei tag `<style></style>`, sostituendo il commento `/* Add styles here */`.

```css live-sample___walkthrough
* {
  box-sizing: border-box;
}

body {
  width: 90%;
  margin: 2em auto;
  font:
    1em/1.3 "Helvetica",
    "Arial",
    sans-serif;
}

a:link,
a:visited {
  color: #333333;
}

nav ul,
aside ul {
  list-style: none;
  padding: 0;
}

nav a:link,
nav a:visited {
  background-color: rgb(207 232 220 / 20%);
  border: 2px solid rgb(79 185 227);
  text-decoration: none;
  display: block;
  padding: 10px;
  color: #333333;
  font-weight: bold;
}

nav a:hover {
  background-color: rgb(207 232 220 / 70%);
}

.related {
  background-color: rgb(79 185 227 / 30%);
  border: 1px solid rgb(79 185 227);
  padding: 10px;
}

.sidebar {
  background-color: rgb(207 232 220 / 50%);
  padding: 10px;
}

article {
  margin-bottom: 1em;
}
```

Se si visualizza il layout in Responsive Design Mode nei DevTools, oppure si restringe la finestra del browser fino a una larghezza simile a quella di un dispositivo mobile, si noterà che funziona piuttosto bene come semplice visualizzazione mobile del sito.

{{EmbedLiveSample("walkthrough", "", "600px")}}

### Creare un layout a due colonne per larghezze medie

Allargare la finestra finché non risulta evidente che le righe stanno diventando piuttosto lunghe; a questo punto c'è spazio per visualizzare la navigazione in una riga orizzontale. Qui verrà aggiunta la prima media query. Verranno usate le unità `em`, poiché se l'utente ha aumentato la dimensione del testo, il breakpoint si verificherà a una lunghezza di riga simile ma con una viewport più ampia rispetto a un utente con una dimensione del testo minore.

Aggiungere quanto segue alla fine del CSS:

```css
@media screen and (width >= 40em) {
  article {
    display: grid;
    grid-template-columns: 3fr 1fr;
    column-gap: 20px;
  }

  nav ul {
    display: flex;
  }

  nav li {
    flex: 1;
  }
}
```

Questo CSS fornisce un layout a due colonne all'interno di `<article>`, per il contenuto dell'articolo e le informazioni correlate nell'elemento `<aside>`. È stato inoltre utilizzato flexbox per disporre la navigazione in una riga.

### Aggiungere una terza colonna per schermi più ampi

Continuare ad aumentare la larghezza finché non sembra che ci sia spazio sufficiente perché anche la barra laterale formi una nuova colonna. All'interno di una media query, l'elemento `<main>` verrà trasformato in una griglia a due colonne. Sarà quindi necessario rimuovere il {{cssxref("margin-bottom")}} dall'articolo affinché le due barre laterali si allineino tra loro e aggiungere un {{cssxref("border")}} alla parte superiore del footer. In genere, questi piccoli ritocchi sono il tipo di intervento che viene eseguito per rendere gradevole il design a ogni breakpoint.

Aggiungere quanto segue alla fine del CSS:

```css
@media screen and (width >= 70em) {
  main {
    display: grid;
    grid-template-columns: 3fr 1fr;
    column-gap: 20px;
  }

  article {
    margin-bottom: 0;
  }

  footer {
    border-top: 1px solid #cccccc;
    margin-top: 2em;
  }
}
```

L'esempio è terminato. Osservando il risultato a diverse larghezze, è possibile vedere come il design risponda e funzioni come una, due o tre colonne, a seconda della larghezza disponibile. Questo è un esempio di base di design responsivo mobile-first.

### Meta viewport

Osservando il sorgente HTML dell'esempio precedente, si vedrà il seguente elemento incluso nell'head del documento:

```html
<meta name="viewport" content="width=device-width" />
```

Questo è il meta tag [`viewport`](/it/docs/Web/HTML/Reference/Elements/meta/name/viewport): esiste come modo per controllare il rendering dei contenuti da parte dei browser mobili, assicurando che rispettino le media query. Quello precedente indica ai browser mobili: "non eseguire il rendering del contenuto con una viewport di 980 pixel, ma usare invece la larghezza reale del dispositivo". Le media query verranno quindi attivate come previsto.

Per ulteriori informazioni sul motivo per cui è necessario, vedere la sezione [Il meta tag viewport](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design#the_viewport_meta_tag) nell'articolo precedente.

## È davvero necessaria una media query?

Flexbox e CSS Grid offrono modi per creare componenti flessibili e persino responsivi senza bisogno di una media query: vale sempre la pena considerare se ne serva davvero una. Ad esempio, potrebbe essere necessario un insieme di schede larghe almeno 200 pixel, inserendone quante più possibile da 200 pixel nella colonna del contenuto principale, indipendentemente dalla sua larghezza.

Questo può essere ottenuto con CSS Grid, senza utilizzare alcuna media query:

```html live-sample___grid
<ul class="grid">
  <li>
    <h2>Card 1</h2>
    <p>…</p>
  </li>
  <li>
    <h2>Card 2</h2>
    <p>…</p>
  </li>
  <li>
    <h2>Card 3</h2>
    <p>…</p>
  </li>
  <li>
    <h2>Card 4</h2>
    <p>…</p>
  </li>
  <li>
    <h2>Card 5</h2>
    <p>…</p>
  </li>
</ul>
```

```css live-sample___grid
body {
  font: 1.2em / 1.5 sans-serif;
}
.grid {
  list-style: none;
  margin: 0;
  padding: 0;
  display: grid;
  gap: 20px;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
}

.grid li {
  border: 1px solid #666666;
  padding: 10px;
}
```

{{EmbedLiveSample("grid", "", "350px")}}

Provare ad allargare e restringere la finestra del browser per vedere cambiare il numero di tracce di colonna.

L'aspetto interessante di questo metodo è che la griglia non considera la larghezza della viewport, ma la larghezza disponibile per questo componente. Potrebbe sembrare strano concludere una sezione sulle media query suggerendo che potrebbe non esserne necessaria alcuna. Tuttavia, nella pratica, un buon uso dei moderni metodi di layout, integrati con le media query, darà i risultati migliori.

## Riepilogo

In questa lezione sono state illustrate le media query e il loro uso pratico per creare un design responsivo mobile-first.

È possibile usare il punto di partenza creato per provare altre media query. Ad esempio, si potrebbe modificare la dimensione della navigazione se viene rilevato che il visitatore ha un puntatore `coarse`, usando la media feature `pointer`.

È anche possibile sperimentare aggiungendo componenti diversi e verificando se l'aggiunta di una media query, oppure l'uso di un metodo di layout come flexbox o grid, sia il modo più appropriato per rendere responsivi i componenti. Molto spesso non esiste un modo giusto o sbagliato: è opportuno sperimentare e vedere cosa funziona meglio per il design e i contenuti.

Bene, il modulo è quasi terminato. Nel [prossimo articolo](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Responsive_design), verranno proposti alcuni test per verificare quanto bene sono state comprese e assimilate tutte le informazioni sul responsive web design e sulle media query fornite nei due articoli precedenti.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Responsive_Design", "Learn_web_development/Core/CSS_layout/Test_your_skills/Responsive_design", "Learn_web_development/Core/CSS_layout")}}
