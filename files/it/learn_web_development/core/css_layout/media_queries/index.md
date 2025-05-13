---
title: Fondamenti delle media query
short-title: Media query
slug: Learn_web_development/Core/CSS_layout/Media_queries
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout/Fundamental_layout_comprehension", "Learn_web_development/Core/CSS_layout")}}

La **CSS Media Query** fornisce un modo per applicare il CSS solo quando l'ambiente del browser e del dispositivo corrisponde a una regola specificata, ad esempio "viewport più ampio di 480 pixel". Le media query sono una parte fondamentale del design web responsivo, poiché consentono di creare layout differenti a seconda delle dimensioni del viewport, ma possono anche essere utilizzate per rilevare altre caratteristiche dell'ambiente in cui il sito è eseguito, ad esempio se l'utente utilizza un touchscreen invece di un mouse.

In questa lezione, apprenderà innanzitutto la sintassi utilizzata nelle media query e successivamente passerà a usarle in esempi che mostrano come un design di base potrebbe diventare responsivo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stile del testo e dei font</a>,
        familiarità con <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">concetti fondamentali del layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>La sintassi delle media query.</li>
          <li>I tipi comuni di media query.</li>
          <li>Usare le media query di <code>width</code> e <code>height</code> per creare layout responsivi.</li>
          <li>Scegliere i breakpoints.</li>
          <li>Utilizzare le media query per implementare un design incentrato prima sui dispositivi mobili.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Nozioni di base sulle media query

La sintassi più semplice delle media query appare come segue:

```css
@media media-type and (media-feature-rule) {
  /* CSS rules go here */
}
```

Consiste in:

- Un tipo di media, che indica al browser per che tipo di media è questo codice (ad es., stampa o schermo).
- Un'espressione media, che è una regola, o un test che deve essere superato affinché il CSS contenuto venga applicato.
- Un insieme di regole CSS che verranno applicate se il test viene superato e il tipo di media è corretto.

### Tipi di media

I tipi di media possibili che può specificare sono:

- `all`
- `print`
- `screen`

La seguente media query imposterà il corpo a 12pt solo se la pagina viene stampata. Non si applicherà quando la pagina viene caricata in un browser.

```css
@media print {
  body {
    font-size: 12pt;
  }
}
```

> [!NOTE]
> Il tipo di media qui è diverso dal cosiddetto {{Glossary("MIME_type", "tipo MIME")}}.
> Nella specifica delle media query di Livello 3 erano definiti un certo numero di altri tipi di media; questi sono stati deprecati e dovrebbero essere evitati.
> I tipi di media sono opzionali; se non si indica un tipo di media nella media query, allora la media query sarà predefinita per tutti i tipi di media.

### Regole di caratteristiche dei media

Dopo aver specificato il tipo, può quindi mirare a una caratteristica del media con una regola.
I seguenti esempi mostrano come utilizzare diverse media query.
Per cambiare la `width` del suo schermo, modifichi la dimensione del suo browser o ruoti il suo dispositivo portatile. In alternativa, può utilizzare le funzionalità di dimensionamento responsive degli strumenti per sviluppatori del browser per simulare diverse larghezze del dispositivo.

#### Larghezza e altezza

La caratteristica che tendiamo più spesso a rilevare per creare design responsivi (e che ha un ampio supporto del browser) è la larghezza del viewport, e possiamo applicare il CSS se il viewport è al di sopra o al di sotto di una certa larghezza — o di una larghezza esatta — utilizzando le caratteristiche del media `min-width`, `max-width` e `width`.

Queste caratteristiche vengono utilizzate per creare layout che rispondono a diverse dimensioni dello schermo. Ad esempio, per impostare il colore del testo del corpo su rosso se il viewport è esattamente di 600 pixel, utilizzerà la seguente media query.

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

Le caratteristiche del media `width` (e `height`) possono essere utilizzate come intervalli, e possono quindi essere prefissate con `min-` o `max-` per indicare che il valore dato è un minimo o un massimo. Ad esempio, per rendere il colore blu se il viewport è uguale o inferiore ai 600 pixel, usi `max-width`:

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

In pratica, l'uso di valori minimi o massimi è molto più utile per il design responsivo quindi raramente vedrà `width` o `height` usati da soli.

Ci sono molte altre caratteristiche dei media che può testare, anche se alcune delle caratteristiche più recenti introdotte nei Livelli 4 e 5 della specifica delle media query hanno un supporto limitato nei browser. Ogni caratteristica è documentata su MDN insieme alle informazioni sul supporto del browser, e può trovare un elenco completo su [Utilizzo delle Media Queries: Sintassi](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries#syntax).

#### Orientamento

Una caratteristica dei media ben supportata è `orientation`, che ci consente di testare la modalità verticale o orizzontale. Per cambiare il colore del testo del corpo se il dispositivo si trova in orientamento orizzontale, utilizzi la seguente media query.

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

Una vista standard del desktop ha un orientamento orizzontale, e un design che funziona bene in questo orientamento potrebbe non funzionare altrettanto bene quando viene visualizzato su un telefono o tablet in modalità verticale. Testare l'orientamento può aiutarla a creare un layout ottimizzato per i dispositivi in modalità verticale.

#### Uso dei dispositivi di puntamento

Come parte della specifica del Livello 4, è stata introdotta la caratteristica del media `hover`. Questa caratteristica significa che può testare se l'utente ha la possibilità di passare il mouse su un elemento, il che significa essenzialmente che sta usando qualche tipo di dispositivo di puntamento; il touchscreen e la navigazione da tastiera non fanno hover.

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

Se sappiamo che l'utente non può eseguire l'hover, potremmo visualizzare alcune caratteristiche interattive per impostazione predefinita. Per gli utenti che possono eseguire l'hover, potremmo scegliere di renderle disponibili quando un link viene passato in hover.

Sempre nel Livello 4 c'è la caratteristica del media `pointer`. Prende tre valori possibili, `none`, `fine` e `coarse`. Un puntatore `fine` è qualcosa come un mouse o un trackpad. Permette all'utente di mirare con precisione a una piccola area. Un puntatore `coarse` è il suo dito su un touchscreen. Il valore `none` significa che l'utente non ha dispositivo di puntamento; forse naviga solo con la tastiera o con comandi vocali.

Usare `pointer` può aiutarla a progettare migliori interfacce che rispondono al tipo di interazione che un utente ha con uno schermo. Ad esempio, potrebbe creare aree di interazione più grandi se sa che l'utente sta interagendo con il dispositivo come un touchscreen.

### Usare la sintassi con intervalli

Un caso comune è verificare se la larghezza del viewport è compresa tra due valori:

```css
@media (min-width: 30em) and (max-width: 50em) {
  /* … */
}
```

Se vuole migliorare la leggibilità di questo, può utilizzare la sintassi "range":

```css
@media (30em <= width <= 50em) {
  /* … */
}
```

Quindi, in questo caso, gli stili vengono applicati quando la larghezza del viewport è compresa tra `30em` e `50em`.

## Media query più complesse

Con tutte le diverse possibili media query, potrebbe volerle combinare o creare elenchi di query — qualsiasi di queste potrebbe essere corrispondente.

### Logica "and" nelle media query

Per combinare le caratteristiche dei media può utilizzare `and` in modo simile a come abbiamo utilizzato `and` sopra per combinare un tipo di media e una caratteristica. Ad esempio, potremmo voler testare per una `min-width` e `orientation`. Il testo del corpo sarà blu solo se il viewport è largo almeno 600 pixel e il dispositivo è in modalità orizzontale.

```css live-sample___and
@media screen and (min-width: 600px) and (orientation: landscape) {
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

Se ha un insieme di query, qualsiasi delle quali potrebbe corrispondere, può separare queste query con una virgola. Nell'esempio sotto, il testo sarà blu se il viewport è largo almeno 600 pixel **o** il dispositivo è in orientamento orizzontale. Se una di queste cose è vera, la query corrisponde.

```css live-sample___or
@media screen and (min-width: 600px), screen and (orientation: landscape) {
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

Può negare un'intera media query utilizzando l'operatore `not`. Questo inverte il significato dell'intera media query. Quindi, in questo prossimo esempio, il testo sarà blu solo se l'orientamento è verticale.

```css live-sample___not
@media not (orientation: landscape) {
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

Può anche usare `not` per negare espressioni specifiche.

```css
@media (not (width < 600px)) and (not (width > 1000px)) {
  body {
    color: blue;
  }
}
```

Questo applicherà gli stili se la larghezza del viewport è compresa tra 600 e 1000 pixel. Questo è equivalente a `(600px <= width <= 1000px)`.

## Come scegliere i breakpoints

Nei primi giorni del design responsivo, molti designer tentavano di prendere di mira dimensioni dello schermo molto specifiche. Venivano pubblicati elenchi delle dimensioni degli schermi di telefoni e tablet popolari affinché i design potessero essere creati per adattarsi perfettamente a quei viewport.

Ora ci sono troppi dispositivi, con una vasta gamma di dimensioni, per rendere ciò fattibile. Ciò significa che invece di puntare a dimensioni specifiche per tutti i design, un approccio migliore è modificare il design nel punto in cui il contenuto inizia a rompersi in qualche modo. Forse le lunghezze delle righe diventano troppo lunghe o una sidebar diventa compressa e difficile da leggere. Questo è il punto in cui vuole utilizzare una media query per modificare il design per uno migliore per lo spazio che ha a disposizione. Questo approccio significa che non importa quali siano le dimensioni esatte del dispositivo utilizzato, ogni intervallo è coperto. I punti in cui viene introdotta una media query sono noti come **breakpoints**.

La [Modalità di design responsiva](https://firefox-source-docs.mozilla.org/devtools-user/responsive_design_mode/index.html) negli strumenti DevTools di Firefox è molto utile per determinare dove questi breakpoints dovrebbero essere inseriti. Può facilmente ridurre o aumentare la larghezza del viewport per vedere dove il contenuto sarebbe migliorato aggiungendo una media query e modificando il design.

![Uno screenshot di un layout in modalità mobile nei DevTools di Firefox.](rwd-mode.png)

## Apprendimento attivo: design responsivo incentrato prima sui dispositivi mobili

In generale, può adottare due approcci per un design responsivo. Può partire dalla vista desktop o più larga e poi aggiungere breakpoint per modificare le cose man mano che il viewport diventa più piccolo, oppure può partire dalla vista più piccola e aggiungere layout man mano che il viewport diventa più grande. Questo secondo approccio è descritto come design responsivo **incentrato prima sui dispositivi mobili** ed è spesso il miglior approccio da seguire.

La vista per i dispositivi più piccoli è spesso una semplice colonna unica di contenuto, proprio come appare nel normale flusso. Ciò significa che probabilmente non avrà bisogno di fare molto layout per i dispositivi piccoli — ordini bene la sua fonte e avrà un layout leggibile per impostazione predefinita.

La guida qui sotto la porterà attraverso questo approccio con un layout molto semplice. In un sito di produzione probabilmente avrà più cose da regolare all'interno delle sue media query, tuttavia l'approccio sarebbe esattamente lo stesso.

### Guida: un layout incentrato prima sui dispositivi mobili

Il nostro punto di partenza è un documento HTML con del CSS applicato per aggiungere colori di sfondo alle varie parti del layout.
Può copiare il codice dai blocchi sotto in un editor di testo, salvarlo come file HTML sul suo computer e aprirlo nel suo browser o fare clic su "Play" per eseguire il rendering e modificare il codice nel Playground di MDN:

```html live-sample___walkthrough
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
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

La fonte del documento è ordinata in modo che il contenuto sia leggibile. Questo è un passo iniziale importante e uno che garantisce che se il contenuto dovesse essere letto da uno screen reader, sarebbe comprensibile.
Ecco alcuni buoni stili iniziali con cui possiamo iniziare:

```css live-sample___walkthrough
* {
  box-sizing: border-box;
}

body {
  width: 90%;
  margin: 2em auto;
  font:
    1em/1.3 Arial,
    Helvetica,
    sans-serif;
}

a:link,
a:visited {
  color: #333;
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
  color: #333;
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

Se osserviamo il layout in Modalità di design responsiva nel DevTools, possiamo vedere che funziona abbastanza bene come una vista mobile semplice del sito.

{{EmbedLiveSample("walkthrough", "", "600px")}}

Da questo punto, inizi a trascinare la vista della Modalità di design responsiva più larga fino a che non vedi che le lunghezze delle righe stanno diventando piuttosto lunghe e abbiamo spazio per visualizzare la navigazione in una linea orizzontale. Questo è il punto in cui aggiungeremo la nostra prima media query. Usiamo em, poiché ciò significherà che se l'utente ha aumentato la dimensione del testo, il breakpoint si verificherà a una lunghezza delle righe simile ma in un viewport più ampio rispetto a un utente con una dimensione del testo più piccola.

Aggiungi il seguente codice CSS:

```css
@media screen and (min-width: 40em) {
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

Questo CSS ci offre un layout a due colonne all'interno dell'articolo, del contenuto dell'articolo e delle informazioni correlate nell'elemento aside. Abbiamo anche usato flexbox per inserire la navigazione in una riga.

Continui ad espandere la larghezza finché non sente che c'è abbastanza spazio per far sì che la barra laterale formi una nuova colonna. All'interno di una media query, faremo dell'elemento principale un layout a due colonne. Dovremo quindi rimuovere il {{cssxref("margin-bottom")}} sull'articolo in modo che le due barre laterali si allineino tra loro, e aggiungeremo un {{cssxref("border")}} alla parte superiore del footer. Tipicamente queste piccole modifiche sono il tipo di cose che farà per far sembrare il design buono in ogni breakpoint.

Aggiungi il seguente codice CSS ai suoi stili:

```css
@media screen and (min-width: 70em) {
  main {
    display: grid;
    grid-template-columns: 3fr 1fr;
    column-gap: 20px;
  }

  article {
    margin-bottom: 0;
  }

  footer {
    border-top: 1px solid #ccc;
    margin-top: 2em;
  }
}
```

Se osserva il risultato a diverse larghezze, potrà vedere come il design risponde e funziona come una colonna unica, due colonne o tre colonne, a seconda della larghezza disponibile. Questo è un esempio base di un design responsivo incentrato prima sui dispositivi mobili.

## Il meta tag viewport

Se osserva il sorgente HTML nell'esempio sopra, vedrà il seguente elemento incluso nell'intestazione del documento:

```html
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

Questo è il [meta tag viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element) — esiste come un modo per controllare come i browser mobili rendono il contenuto. Questo è necessario perché per impostazione predefinita, la maggior parte dei browser mobili mente sulla larghezza del loro viewport. I siti non responsivi comunemente sembrano davvero brutti quando vengono renderizzati in un viewport stretto, quindi i browser mobili di solito rendono il sito con una larghezza del viewport più larga della vera larghezza del dispositivo per impostazione predefinita (di solito 980 pixel), e poi riducono il risultato reso in modo che si adatti al display.

Questo è tutto ben and good, ma significa che i siti responsivi non funzioneranno come previsto. Se la larghezza del viewport viene segnalata come 980 pixel, i layout mobili (ad esempio creati utilizzando una media query di `@media screen and (max-width: 600px) { }`) non verranno renderizzati come previsto.

Per rimediare a questo, includere un meta tag viewport come quello sopra sulla sua pagina dice al browser "non rendere il contenuto con un viewport di 980 pixel — rendilo utilizzando la vera larghezza del dispositivo invece, e impostare un livello di scala iniziale predefinito per una maggiore coerenza." Le media query entreranno quindi in gioco come previsto.

Ci sono un certo numero di altre opzioni che può inserire all'interno dell'attributo `content` del meta tag viewport — veda [Utilizzo del meta tag viewport per controllare il layout sui browser mobili](/it/docs/Web/HTML/Guides/Viewport_meta_element) per ulteriori dettagli.

## Ha davvero bisogno di una media query?

Flexbox, Grid e layout multi-colonna le offrono modi per creare componenti flessibili e persino responsivi senza bisogno di una media query. Vale sempre la pena considerare se questi metodi di layout possono raggiungere ciò che desidera senza aggiungere media query. Ad esempio, potrebbe voler un insieme di card che sono larghe almeno 200 pixel, con quante più di queste 200 pixel che si adattano all'articolo principale. Questo può essere ottenuto con il layout a griglia, senza usare affatto le media query.

Questo potrebbe essere ottenuto utilizzando il seguente:

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
  border: 1px solid #666;
  padding: 10px;
}
```

{{EmbedLiveSample("grid", "", "350px")}}

Renda lo schermo più ampio e più stretto per vedere come cambia il numero di tracce della colonna. La cosa interessante di questo metodo è che la griglia non sta guardando alla larghezza del viewport, ma alla larghezza disponibile per questo componente. Potrebbe sembrare strano concludere una sezione sulle media query con il suggerimento che potrebbe non averne bisogno affatto! Tuttavia, nella pratica troverà che un buon uso dei metodi di layout moderni, migliorati con le media query, produrrà i migliori risultati.

## Metta alla prova le sue capacità

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare un test per verificare di aver trattenuto queste informazioni prima di procedere — veda [Metta alla prova le sue capacità: design web responsivo e media query](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Responsive_design).

## Riepilogo

In questa lezione ha appreso delle media query e ha anche scoperto come usarle in pratica per creare un design responsivo incentrato prima sui dispositivi mobili.

Potrebbe usare il punto di partenza che abbiamo creato per testare altre media query. Ad esempio, forse potrebbe cambiare la dimensione della navigazione se rileva che il visitatore ha un puntatore grossolano, utilizzando la caratteristica del media `pointer`.

Potrebbe anche sperimentare aggiungendo diversi componenti e vedere se l'aggiunta di una media query, o l'utilizzo di un metodo di layout come flexbox o griglia, è il modo più appropriato per rendere i componenti responsivi. Molto spesso non c'è un modo giusto o sbagliato — dovrebbe sperimentare e vedere cosa funziona meglio per il suo design e il suo contenuto.

OK, siamo quasi alla fine di questo modulo. Concludiamo dandole una sfida per testare la sua comprensione.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout/Fundamental_layout_comprehension", "Learn_web_development/Core/CSS_layout")}}
