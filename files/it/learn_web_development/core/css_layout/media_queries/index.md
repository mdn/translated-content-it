---
title: Fondamenti delle media query
short-title: Media query
slug: Learn_web_development/Core/CSS_layout/Media_queries
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout/Fundamental_layout_comprehension", "Learn_web_development/Core/CSS_layout")}}

La **Media Query CSS** offre un metodo per applicare CSS solo quando l'ambiente del browser e del dispositivo corrispondono a una regola specificata, per esempio "viewport è più largo di 480 pixel". Le media query sono una parte fondamentale del responsive web design, poiché consentono di creare layout diversi a seconda delle dimensioni del viewport, ma possono anche essere utilizzate per rilevare altre caratteristiche dell'ambiente in cui funziona il sito, ad esempio se l'utente sta utilizzando un touchscreen piuttosto che un mouse.

In questa lezione, imparerai prima la sintassi utilizzata nelle media query, e poi passerai a usarle in esempi che mostrano come un design di base possa essere reso reattivo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        >,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>,
        <a href="/it/docs/Learn_web_development/Core/Text_styling/Fundamentals">Fondamenti di stile del testo e dei font</a>,
        familiarità con <a href="/it/docs/Learn_web_development/Core/CSS_layout/Introduction">Concetti fondamentali di layout CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>La sintassi delle media query.</li>
          <li>I tipi comuni di media query.</li>
          <li>Utilizzare le media query `width` e `height` per creare layout reattivi.</li>
          <li>Scegliere i punti di interruzione.</li>
          <li>Utilizzare le media query per implementare un design mobile-first.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Nozioni di base sulle media query

La sintassi più semplice di una media query appare così:

```css
@media media-type and (media-feature-rule) {
  /* CSS rules go here */
}
```

Essa consiste di:

- Un tipo di media, che indica al browser per quale tipo di media è questo codice (ad esempio, print o screen).
- Un'espressione media, che è una regola o un test che deve essere superato affinché il CSS contenuto venga applicato.
- Un insieme di regole CSS che verranno applicate se il test supera e il tipo di media è corretto.

### Tipi di media

I tipi di media che puoi specificare sono:

- `all`
- `print`
- `screen`

La seguente media query imposterà il body a 12pt solo se la pagina viene stampata. Non si applicherà quando la pagina viene caricata in un browser.

```css
@media print {
  body {
    font-size: 12pt;
  }
}
```

> [!NOTE]
> Il tipo di media qui è diverso dal cosiddetto {{Glossary("MIME_type", "tipo MIME")}}.
> Nella specifica delle Media Queries di Livello 3 erano definiti un certo numero di altri tipi di media; questi sono stati deprecati e dovrebbero essere evitati.
> I tipi di media sono opzionali; se non si indica un tipo di media nella propria media query, allora la media query predefinita sarà per tutti i tipi di media.

### Regole di funzionalità dei media

Dopo aver specificato il tipo, puoi mirare a una funzionalità media con una regola.
I seguenti esempi mostrano come utilizzare diverse media query.
Per cambiare la `width` del tuo schermo, modifica la dimensione del tuo browser o ruota il tuo dispositivo portatile. In alternativa, puoi utilizzare le caratteristiche di dimensionamento reattivo degli strumenti per sviluppatori del tuo browser per simulare larghezze di dispositivo diverse.

#### Larghezza e altezza

La caratteristica che tendiamo più spesso a rilevare per creare design reattivi (e che ha un ampio supporto da parte dei browser) è la larghezza del viewport, e possiamo applicare CSS se il viewport è sopra o sotto una certa larghezza o una larghezza esatta usando le funzionalità media `min-width`, `max-width`, e `width`.

Queste caratteristiche sono utilizzate per creare layout che rispondono a diverse dimensioni dello schermo. Ad esempio, per impostare il colore del testo del body su rosso se il viewport è esattamente di 600 pixel, useresti la seguente media query.

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

Le funzionalità media `width` (e `height`) possono essere utilizzate come intervalli, e pertanto possono essere prefissate con `min-` o `max-` per indicare che il valore dato è un minimo o un massimo. Ad esempio, per rendere il colore blu se il viewport è di 600 pixel o più stretto, usa `max-width`:

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

In pratica, l'utilizzo di valori minimi o massimi è molto più utile per il design reattivo, quindi raramente vedrai utilizzare `width` o `height` da soli.

Ci sono molte altre funzionalità media che puoi testare, sebbene alcune delle nuove funzionalità introdotte nei Livelli 4 e 5 della specifica delle media query abbiano un supporto limitato nei browser. Ogni caratteristica è documentata su MDN insieme alle informazioni di compatibilità del browser, e puoi trovare un elenco completo in [Utilizzo delle Media Query: Sintassi](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries#syntax).

#### Orientamento

Una caratteristica media ben supportata è `orientation`, che ci permette di testare per la modalità verticale o orizzontale. Per cambiare il colore del testo del body se il dispositivo è in orientamento orizzontale, usa la seguente media query.

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

Una vista standard su desktop ha un orientamento orizzontale, e un design che funziona bene in questo orientamento potrebbe non funzionare altrettanto bene quando viene visualizzato su un telefono o tablet in modalità verticale. Testare l'orientamento può aiutarti a creare un layout ottimizzato per i dispositivi in modalità verticale.

#### Uso dei dispositivi di puntamento

Come parte della specifica di Livello 4, è stata introdotta la funzionalità media `hover`. Questa funzionalità significa che puoi testare se l'utente ha la capacità di passare il cursore su un elemento, il che significa essenzialmente che stanno usando un qualche tipo di dispositivo di puntamento; il touchscreen e la navigazione tramite tastiera non attivano l'hover.

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

Se sappiamo che l'utente non può fare hover, potremmo mostrare alcune caratteristiche interattive per impostazione predefinita. Per gli utenti che possono fare hover, potremmo scegliere di renderle disponibili quando si passa il cursore su un link.

Anche nel Livello 4 c'è la funzionalità media `pointer`. Questa prende tre possibili valori, `none`, `fine`, e `coarse`. Un puntatore `fine` è qualcosa come un mouse o un trackpad. Consente all'utente di mirare con precisione un'area piccola. Un puntatore `coarse` è il tuo dito su un touchscreen. Il valore `none` significa che l'utente non ha un dispositivo di puntamento; forse sta navigando solo con la tastiera o con comandi vocali.

Utilizzare `pointer` può aiutarti a progettare interfacce migliori che rispondano al tipo di interazione che un utente sta avendo con uno schermo. Ad esempio, potresti creare aree di clic più grandi se sai che l'utente sta interagendo con il dispositivo come un touchscreen.

### Utilizzo della sintassi intervallata

Un caso comune è controllare se la larghezza del viewport è compresa tra due valori:

```css
@media (min-width: 30em) and (max-width: 50em) {
  /* … */
}
```

Se vuoi migliorare la leggibilità di questo, puoi usare la sintassi "range":

```css
@media (30em <= width <= 50em) {
  /* … */
}
```

Quindi in questo caso, gli stili vengono applicati quando la larghezza del viewport è compresa tra `30em` e `50em`.

## Media query più complesse

Con tutte le diverse media query possibili, potresti volerle combinare o creare elenchi di query, una delle quali potrebbe corrispondere.

### Logica "and" nelle media query

Per combinare le funzionalità media puoi usare `and` allo stesso modo in cui abbiamo usato `and` sopra per combinare un tipo di media e una caratteristica. Ad esempio, potremmo voler testare per `min-width` e `orientation`. Il testo del body sarà blu solo se il viewport è largo almeno 600 pixel e il dispositivo è in modalità orizzontale.

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

Se hai un set di query, delle quali una qualsiasi potrebbe corrispondere, allora puoi separare queste query con una virgola. Nell'esempio seguente il testo sarà blu se il viewport è largo almeno 600 pixel O il dispositivo è in orientamento orizzontale. Se una di queste condizioni è vera, la query corrisponde.

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

Puoi negare un'intera media query utilizzando l'operatore `not`. Questo inverte il significato dell'intera media query. Pertanto, in questo prossimo esempio il testo sarà blu solo se l'orientamento è verticale.

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

Puoi anche usare `not` per negare espressioni specifiche.

```css
@media (not (width < 600px)) and (not (width > 1000px)) {
  body {
    color: blue;
  }
}
```

Questo applicherà gli stili se la larghezza del viewport è compresa tra 600 e 1000 pixel. Questo è equivalente a `(600px <= width <= 1000px)`.

## Come scegliere i breakpoints

Nei primi giorni del responsive design, molti designer cercavano di mirare a dimensioni dello schermo molto specifiche. Venivano pubblicati elenchi delle dimensioni degli schermi di telefoni e tablet popolari affinché i design potessero essere creati per adattarsi perfettamente a quei viewport.

Adesso ci sono troppi dispositivi, con una vasta gamma di dimensioni, per rendere ciò fattibile. Questo significa che invece di mirare a dimensioni specifiche per tutti i design, un approccio migliore è cambiare il design nel punto in cui il contenuto inizia a rompersi in qualche modo. Forse le lunghezze delle righe diventano troppo lunghe, o una barra laterale boxata si schiaccia ed è difficile da leggere. Questo è il punto in cui vuoi usare una media query per cambiare il design in uno migliore per lo spazio disponibile. Questo approccio significa che non importa quali sono le dimensioni esatte del dispositivo utilizzato, ogni intervallo è soddisfatto. I punti in cui viene introdotta una media query sono noti come **breakpoints**.

Il [Responsive Design Mode](https://firefox-source-docs.mozilla.org/devtools-user/responsive_design_mode/index.html) nei Firefox DevTools è molto utile per determinare dove dovrebbero andare questi breakpoints. Puoi facilmente ridurre e ampliare il viewport per vedere dove il contenuto potrebbe essere migliorato aggiungendo una media query e perfezionando il design.

![Uno screenshot di un layout in una vista mobile nei Firefox DevTools.](rwd-mode.png)

## Apprendimento attivo: design reattivo mobile-first

In generale, puoi adottare due approcci per un design reattivo. Puoi iniziare con la tua vista desktop o la più ampia e quindi aggiungere breakpoints per spostare le cose man mano che il viewport diventa più piccolo, oppure puoi iniziare con la vista più piccola e aggiungere layout man mano che il viewport diventa più grande. Questo secondo approccio è descritto come design reattivo **mobile-first** ed è spesso il miglior approccio da seguire.

La vista per i dispositivi più piccoli è spesso una semplice singola colonna di contenuti, proprio come appare nel flusso normale. Ciò significa che probabilmente non hai bisogno di fare molto layout per i piccoli dispositivi: ordina bene la tua fonte e avrai un layout leggibile per impostazione predefinita.

Il seguente walkthrough ti guida attraverso questo approccio con un layout molto semplice. In un sito di produzione è probabile che tu abbia più cose da aggiustare all'interno delle tue media query, tuttavia l'approccio sarebbe esattamente lo stesso.

### Guida: un layout mobile-first

Il nostro punto di partenza è un documento HTML con qualche CSS applicato per aggiungere colori di sfondo alle varie parti del layout.
Puoi copiare il codice dai blocchi sottostanti in un editor di testo, salvarlo come un file HTML sul tuo computer e aprirlo nel tuo browser o cliccare "Play" per renderizzare e modificare il codice nel Playground di MDN:

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

La fonte del documento è ordinata in modo tale da rendere il contenuto leggibile. Questo è un primo passo importante e uno che garantisce che se il contenuto fosse letto da un lettore di schermo, sarebbe comprensibile.
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

Se visualizzi il layout nel Responsive Design Mode nei DevTools, puoi vedere che funziona abbastanza bene come una semplice vista mobile del sito.

{{EmbedLiveSample("walkthrough", "", "600px")}}

Da questo punto, inizia a trascinare la vista del Responsive Design Mode più larga fino a quando non puoi vedere che le lunghezze delle righe stanno diventando abbastanza lunghe, e abbiamo spazio per la navigazione da visualizzare in una linea orizzontale. Questo è il punto in cui aggiungeremo la nostra prima media query. Useremo ems, poiché ciò significherà che se l'utente ha aumentato la dimensione del testo, il breakpoint avverrà a una lunghezza di riga simile ma con un viewport più ampio rispetto a qualcuno con una dimensione del testo minore.

Aggiungi quanto segue al tuo CSS:

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

Questo CSS ci dà un layout a due colonne all'interno dell'articolo, del contenuto dell'articolo e delle informazioni correlate nell'elemento aside. Abbiamo anche usato flexbox per mettere la navigazione in una riga.

Continuiamo ad espandere la larghezza finché non sentirai che c'è abbastanza spazio per la barra laterale per formare anche una nuova colonna. All'interno di una media query, trasformeremo l'elemento principale in una griglia a due colonne. Poi dobbiamo rimuovere il {{cssxref("margin-bottom")}} sull'articolo affinché le due barre laterali si allineino tra loro, e aggiungeremo un {{cssxref("border")}} alla parte superiore del footer. Tipicamente questi piccoli aggiustamenti sono il genere di cose che farai per far apparire il design bene a ciascun breakpoint.

Aggiungi il seguente CSS ai tuoi stili:

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

Se guardi il risultato a diverse larghezze, puoi vedere come il design risponde e funziona come una colonna singola, due colonne o tre colonne, a seconda della larghezza disponibile. Questo è un esempio basilare di design reattivo mobile-first.

## Il meta tag del viewport

Se guardi la fonte HTML nell'esempio sopra, vedrai che è incluso nel head del documento il seguente elemento:

```html
<meta name="viewport" content="width=device-width,initial-scale=1" />
```

Questo è il [meta tag del viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element) — esiste come un modo per controllare come i browser mobili renderizzano i contenuti. Questo è necessario perché per impostazione predefinita, la maggior parte dei browser mobili mentisce sulla larghezza del viewport. I siti non reattivi comunemente appaiono davvero male quando vengono renderizzati in un viewport stretto, quindi i browser mobili solitamente rendono il sito con una larghezza del viewport più ampia rispetto alla larghezza reale del dispositivo per impostazione predefinita (di solito 980 pixel), e poi riducono il risultato renderizzato in modo che si adatti al display.

Questo è tutto benissimo, ma significa che i siti reattivi non funzioneranno come previsto. Se la larghezza del viewport è indicata come 980 pixel, allora i layout mobili (ad esempio creati usando una media query di `@media screen and (max-width: 600px) { }`) non verranno renderizzati come previsto.

Per ovviare a questo, l'inclusione di un meta tag del viewport come quello sopra sulla tua pagina dice al browser "non renderizzare il contenuto con un viewport di 980 pixel — renderizzalo utilizzando la larghezza reale del dispositivo invece, e imposta un livello di scala iniziale predefinito per una maggiore coerenza." Le media query entreranno quindi in azione come previsto.

Ci sono una serie di altre opzioni che puoi mettere all'interno dell'attributo `content` del meta tag del viewport — vedi [Uso del meta tag del viewport per controllare il layout sui browser mobili](/it/docs/Web/HTML/Guides/Viewport_meta_element) per maggiori dettagli.

## Hai davvero bisogno di una media query?

Flexbox, Grid e layout a più colonne ti offrono modi per creare componenti flessibili e persino reattivi senza la necessità di una media query. Vale sempre la pena considerare se questi metodi di layout possono ottenere ciò che desideri senza aggiungere media query. Ad esempio, potresti volere un set di schede che sia largo almeno 200 pixel, con quanti più di questi 200 pixel che si adattano all'articolo principale. Questo può essere ottenuto con il layout a griglia, senza utilizzare media query.

Questo potrebbe essere ottenuto usando quanto segue:

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

Rendi lo schermo più ampio e stretto per vedere il numero di tracce di colonne cambiare. Il bello di questo metodo è che la griglia non guarda la larghezza del viewport, ma la larghezza che ha disponibile per questo componente. Può sembrare strano concludere una sezione sulle media query suggerendo che potresti non averne affatto bisogno! Tuttavia, in pratica scoprirai che un buon uso dei metodi di layout moderni, arricchito con media query, offrirà i migliori risultati.

## Metti alla prova le tue abilità

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare un test per verificare di aver mantenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Responsive web design e media query](/it/docs/Learn_web_development/Core/CSS_layout/Test_your_skills/Responsive_design).

## Sommario

In questa lezione hai imparato delle media query e anche scoperto come usarle in pratica per creare un design reattivo mobile-first.

Potresti usare il punto di partenza che abbiamo creato per provare più media query. Ad esempio, potresti cambiare la dimensione della navigazione se rilevi che il visitatore ha un puntatore massiccio, utilizzando la funzionalità media `pointer`.

Potresti anche sperimentare con l'aggiunta di diversi componenti e vedere se l'aggiunta di una media query o l'utilizzo di un metodo di layout come flexbox o grid è il modo più appropriato per rendere i componenti reattivi. Molto spesso non c'è un modo giusto o sbagliato: dovresti sperimentare e vedere cosa funziona meglio per il tuo design e contenuto.

Ok, siamo quasi alla fine di questo modulo. Concludiamo dandoti una sfida per testare la tua comprensione.

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Responsive_design", "Learn_web_development/Core/CSS_layout/Fundamental_layout_comprehension", "Learn_web_development/Core/CSS_layout")}}
