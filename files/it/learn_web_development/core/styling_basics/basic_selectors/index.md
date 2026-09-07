---
title: Selettori CSS di base
short-title: Selettori di base
slug: Learn_web_development/Core/Styling_basics/Basic_selectors
l10n:
  sourceCommit: 28f5f3b9b463fa842fa686ccc73c9e1d9b06282b
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics")}}

È già stato illustrato come, in {{Glossary("CSS", "CSS")}}, i selettori vengano usati per individuare gli elementi {{Glossary("HTML", "HTML")}} delle pagine web a cui si desidera applicare stili. È disponibile un'ampia varietà di selettori CSS, che consente una precisione dettagliata nella selezione degli elementi da stilizzare; nei prossimi articoli verranno esaminati in profondità i diversi tipi. In questo articolo verranno riepilogati alcuni concetti fondamentali sui selettori, inclusi i selettori di tipo, classe e ID di base, nonché gli elenchi di selettori. Verrà inoltre introdotto il selettore universale.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Fondamenti di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >).
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I tipi di selettore di base — tipo di elemento, classe, ID.</li>
          <li>Comprendere che gli ID sono univoci per documento — è necessario usare un ID per selezionare uno specifico elemento.</li>
          <li>Comprendere che ogni elemento può avere più classi, utilizzabili per sovrapporre gli stili secondo necessità.</li>
          <li>Elenchi di selettori.</li>
          <li>Selettore universale.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è un selettore?

Un selettore CSS è la prima parte di una regola CSS. È un modello di elementi e altri termini che indica al browser a quali elementi HTML devono essere applicati i valori delle proprietà CSS contenuti nella regola. L'elemento o gli elementi selezionati dal selettore vengono definiti il _soggetto del selettore_.

![Codice con `h1` evidenziato.](selector.png)

Negli articoli precedenti sono stati presentati vari selettori ed è stato illustrato che esistono selettori che individuano il documento in modi diversi; ad esempio, selezionando un elemento come `h1` oppure una classe come `.special`. Si inizierà riepilogando i principali già visti.

## Selettori di tipo

Un **selettore di tipo** viene talvolta chiamato _selettore del nome del tag_ o _selettore di elemento_, poiché seleziona un tag/elemento HTML nel documento. Nell'esempio seguente sono stati usati i selettori `span`, `em` e `strong`.

Provare a modificare l'esempio seguente (fare clic su **"Play"** per aprirlo in MDN Playground) aggiungendo una regola CSS che selezioni l'elemento `<h1>` e ne cambi il colore in blu:

```html live-sample___type
<h1>Type selectors</h1>
<p>
  Veggies es bonus vobis, proinde vos postulo essum magis
  <span>kohlrabi welsh onion</span> daikon amaranth tatsoi tomatillo melon azuki
  bean garlic.
</p>

<p>
  Gumbo beet greens corn soko <strong>endive</strong> gumbo gourd. Parsley
  shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra
  wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.
</p>

<p>
  Turnip greens yarrow ricebean rutabaga <em>endive cauliflower</em> sea lettuce
  kohlrabi amaranth water spinach avocado daikon napa cabbage asparagus winter
  purslane kale. Celery potato scallion desert raisin horseradish spinach
</p>
```

```css live-sample___type
body {
  font-family: sans-serif;
}

span {
  background-color: yellow;
}

strong {
  color: rebeccapurple;
}

em {
  color: rebeccapurple;
}
```

{{EmbedLiveSample("type", "", "280px")}}

## Selettori di classe

Il selettore di classe, sensibile alle maiuscole e minuscole, inizia con il carattere punto (`.`). Seleziona tutto ciò nel documento a cui è applicata quella classe. Nell'esempio interattivo seguente, è stata creata una classe denominata `highlight` e applicata in diversi punti del documento. Tutti gli elementi con quella classe vengono evidenziati.

```html live-sample___class
<h1 class="highlight">Class selectors</h1>
<p>
  Veggies es bonus vobis, proinde vos postulo essum magis
  <span class="highlight">kohlrabi welsh onion</span> daikon amaranth tatsoi
  tomatillo melon azuki bean garlic.
</p>

<p class="highlight">
  Gumbo beet greens corn soko <strong>endive</strong> gumbo gourd. Parsley
  shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra
  wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.
</p>
```

```css live-sample___class
body {
  font-family: sans-serif;
}

.highlight {
  background-color: yellow;
}
```

{{EmbedLiveSample("class", "", "220px")}}

### Sperimentare con i selettori di classe

Provare a modificare l'esempio precedente (usando MDN Playground) per apportare le seguenti modifiche:

1. Modificare l'HTML per cambiare il contenuto a cui vengono applicati gli stili `.highlight`. Si potrebbero, ad esempio, aggiungere elementi `<span>` per racchiudere parti diverse del contenuto esistente e applicare loro la classe `highlight`, rimuovere alcune classi `highlight` esistenti oppure aggiungere nuovo contenuto a cui applicare la classe `highlight`.
2. Modificare il CSS per cambiare le dichiarazioni all'interno della regola `.highlight`, aggiungendone di nuove se lo si desidera, e osservare come questo influenzi lo stile di tutti gli elementi a cui è applicata la classe `highlight`.
3. Creare una nuova regola di classe nel CSS con dichiarazioni diverse al suo interno, ad esempio con un selettore `.highlight2`, quindi provare ad applicarla ad alcuni elementi HTML.

### Individuare le classi su elementi specifici

È possibile creare un selettore che individui elementi specifici a cui è applicata una classe. Nel prossimo esempio, un `<span>` con una classe `highlight` verrà evidenziato in modo diverso rispetto a un'intestazione `<h1>` con una classe `highlight`. Questo avviene usando il selettore di tipo per l'elemento che si desidera individuare, con la classe aggiunta tramite un punto, senza spazi bianchi tra i due.

```html live-sample___class-type
<h1 class="highlight">Class selectors</h1>
<p>
  Veggies es bonus vobis, proinde vos postulo essum magis
  <span class="highlight">kohlrabi welsh onion</span> daikon amaranth tatsoi
  tomatillo melon azuki bean garlic.
</p>

<p class="highlight">
  Gumbo beet greens corn soko <strong>endive</strong> gumbo gourd. Parsley
  shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra
  wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.
</p>
```

```css live-sample___class-type
body {
  font-family: sans-serif;
}

span.highlight {
  background-color: yellow;
}

h1.highlight {
  background-color: pink;
}
```

{{EmbedLiveSample("class-type", "", "200px")}}

Questo approccio riduce l'ambito di una regola. La regola verrà applicata soltanto a quella particolare combinazione di elemento e classe. Sarebbe necessario aggiungere un altro selettore se si volesse applicare la regola ad altri elementi.

### Individuare un elemento se ha più di una classe applicata

È possibile applicare più classi a un elemento e individuarle singolarmente, oppure selezionare l'elemento soltanto quando sono presenti tutte le classi nel selettore. Questo può essere utile quando si creano componenti combinabili in modi diversi nel sito.

Nell'esempio seguente è presente un `<div>` che contiene una nota. Il bordo grigio viene applicato quando il riquadro ha una classe `notebox`. Se possiede anche una classe `warning` o `danger`, viene modificato il valore di {{cssxref("border-color")}}.

È possibile indicare al browser che l'elemento deve corrispondere soltanto se ha due classi applicate concatenandole senza spazi bianchi tra di esse. Si noterà che all'ultimo `<div>` non viene applicato alcuno stile, poiché ha soltanto la classe `danger`. Per ricevere degli stili, necessita anche della classe `notebox`.

```html live-sample___class-many
<div class="notebox">This is an informational note.</div>

<div class="notebox warning">This note shows a warning.</div>

<div class="notebox danger">This note shows danger!</div>

<div class="danger">
  This won't get styled — it also needs to have the notebox class
</div>
```

```css live-sample___class-many
body {
  font-family: sans-serif;
}

.notebox {
  border: 4px solid #666666;
  padding: 0.5em;
  margin: 0.5em;
}

.notebox.warning {
  border-color: orange;
  font-weight: bold;
}

.notebox.danger {
  border-color: red;
  font-weight: bold;
}
```

{{EmbedLiveSample("class-many", "", "200px")}}

## Selettori ID

Il selettore ID, sensibile alle maiuscole e minuscole, inizia con un `#` anziché con un punto, ma viene usato nello stesso modo di un selettore di classe. La differenza è che un ID può essere usato una sola volta per pagina e gli elementi possono avere un solo valore `id`. Un selettore ID individua un elemento con uno specifico `id`; è possibile anteporre all'ID un selettore di tipo per individuare l'elemento soltanto se corrispondono sia l'elemento sia l'ID. Entrambi questi usi sono visibili nell'esempio seguente:

```html live-sample___id
<h1 id="heading">ID selector</h1>
<p>
  Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
  daikon amaranth tatsoi tomatillo melon azuki bean garlic.
</p>

<p id="one">
  Gumbo beet greens corn soko <strong>endive</strong> gumbo gourd. Parsley
  shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra
  wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.
</p>
```

```css live-sample___id
body {
  font-family: sans-serif;
}

#one {
  background-color: yellow;
}

h1#heading {
  color: rebeccapurple;
}
```

{{EmbedLiveSample("id", "", "200px")}}

> [!WARNING]
> Usare lo stesso ID più volte in un documento può sembrare funzionare a fini di styling, ma non bisogna farlo. Produce codice non valido e causerà comportamenti insoliti in molti contesti.

### Sperimentare con i selettori ID

Provare a modificare l'esempio precedente per apportare le seguenti modifiche:

1. Modificare l'HTML per applicare gli stili `#one` al primo paragrafo anziché al secondo.
2. Modificare il CSS per cambiare le dichiarazioni all'interno dei selettori ID e osservare come questo modifichi l'aspetto dell'HTML.

## Elenchi di selettori

Se si desidera applicare lo stesso CSS a più elementi, è possibile combinare singoli selettori in un _elenco di selettori_. La regola viene quindi applicata a tutti i singoli selettori. Ad esempio, se si ha lo stesso CSS per un selettore `h1` e uno `.special`, si potrebbero scrivere due regole separate.

```css
h1 {
  color: blue;
}

.special {
  color: blue;
}
```

È anche possibile combinarle in un elenco di selettori aggiungendo una virgola tra di esse.

```css-nolint
h1, .special {
  color: blue;
}
```

Lo spazio bianco è valido prima o dopo la virgola. I selettori potrebbero inoltre risultare più leggibili se ciascuno è su una nuova riga.

```css
h1,
.special {
  color: blue;
}
```

### Sperimentare con gli elenchi di selettori

Nell'esempio seguente, provare a combinare i due selettori che hanno dichiarazioni identiche. La visualizzazione dovrebbe rimanere invariata.

```html live-sample___selector-list
<h1>Type selectors</h1>
<p>
  Veggies es bonus vobis, proinde vos postulo essum magis
  <span>kohlrabi welsh onion</span> daikon amaranth tatsoi tomatillo melon azuki
  bean garlic.
</p>

<p>
  Gumbo beet greens corn soko <strong>endive</strong> gumbo gourd. Parsley
  shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra
  wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.
</p>

<p>
  Turnip greens yarrow ricebean rutabaga <em>endive cauliflower</em> sea lettuce
  kohlrabi amaranth water spinach avocado daikon napa cabbage asparagus winter
  purslane kale. Celery potato scallion desert raisin horseradish spinach
</p>
```

```css live-sample___selector-list
body {
  font-family: sans-serif;
}
span {
  background-color: yellow;
}

strong {
  color: rebeccapurple;
}

em {
  color: rebeccapurple;
}
```

{{EmbedLiveSample("selector-list", "", "280px")}}

### Selettori non validi negli elenchi di selettori

Quando i selettori vengono raggruppati in questo modo, se uno qualsiasi di essi è sintatticamente non valido, l'intera regola verrà ignorata.

Nell'esempio seguente, la regola con il selettore di classe non valido verrà ignorata, mentre `h1` continuerà a ricevere lo stile.

```css-nolint
h1 {
  color: blue;
}

..special {
  color: blue;
}
```

Se combinati, tuttavia, né `h1` né la classe riceveranno uno stile, poiché l'intera regola viene considerata non valida.

```css-nolint
h1, ..special {
  color: blue;
}
```

## Il selettore universale

Il selettore universale è indicato da un asterisco (`*`). Seleziona tutto nel documento. Se `*` è concatenato usando un [combinatore discendente](/it/docs/Web/CSS/Reference/Selectors/Descendant_combinator), seleziona tutto all'interno di quell'elemento antenato. Ad esempio, `p *` seleziona tutti gli elementi annidati all'interno dell'elemento `<p>`.

Nell'esempio seguente, viene usato il selettore universale per rimuovere i margini da tutti gli elementi. Invece dello stile predefinito del browser, che distanzia intestazioni e paragrafi tramite margini, tutti gli elementi risultano ravvicinati.

```html live-sample___universal
<h1>Universal selector</h1>
<p>
  Veggies es bonus vobis, proinde vos postulo essum magis
  <span>kohlrabi welsh onion</span> daikon amaranth tatsoi tomatillo melon azuki
  bean garlic.
</p>

<p>
  Gumbo beet greens corn soko <strong>endive</strong> gumbo gourd. Parsley
  shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra
  wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini.
</p>
```

```css live-sample___universal
body {
  font-family: sans-serif;
}

* {
  margin: 0;
}
```

{{EmbedLiveSample("universal")}}

Questo tipo di comportamento può talvolta essere osservato nei "fogli di stile di reset", che eliminano tutto lo stile del browser. Poiché il selettore universale apporta modifiche globali, viene usato in situazioni molto specifiche, come quella descritta di seguito.

### Usare il selettore universale per rendere i selettori più leggibili

Un uso del selettore universale consiste nel rendere i selettori più leggibili e intuitivi. Ad esempio, se si desidera selezionare tutti gli elementi discendenti di un elemento `<article>` che sono il primo figlio del rispettivo genitore, inclusi i figli diretti, si potrebbe usare la pseudo-classe {{cssxref(":first-child")}}. Questo argomento verrà approfondito in [pseudo-classi e pseudo-elementi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements):

```css
article :first-child {
  font-weight: bold;
}
```

Tuttavia, questo selettore potrebbe essere confuso con `article:first-child`, che seleziona qualsiasi elemento `<article>` che sia il primo figlio di un altro elemento.

Per evitare questa confusione, è possibile aggiungere il selettore universale alla pseudo-classe `:first-child`, rendendo più evidente cosa faccia il selettore. Esso seleziona _qualsiasi_ elemento che sia il primo figlio di un elemento `<article>`, oppure il primo figlio di qualsiasi elemento discendente di `<article>`:

```css
article *:first-child {
  font-weight: bold;
}
```

Entrambi sono equivalenti, ma alcune persone ritengono che la seconda opzione sia più facile da leggere.

> [!NOTE]
> È improbabile che questa tecnica venga usata spesso nei siti web pubblicati. Ad esempio, su MDN non viene usata molto. Tuttavia, è comunque opportuno considerare di usarla nel proprio codice se risulta più facile da comprendere.

## Riepilogo

In questo articolo sono stati riepilogati i selettori CSS, che consentono di individuare specifici elementi HTML, esaminando i selettori di tipo, classe e ID con un po' più di approfondimento rispetto a quanto fatto in precedenza. Nel prossimo articolo verranno approfonditi i selettori di attributo.

> [!NOTE]
> Per un elenco completo dei selettori, consultare il nostro [riferimento ai selettori CSS](/it/docs/Web/CSS/Guides/Selectors).

## Vedere anche

- [Classi CSS](https://scrimba.com/the-frontend-developer-career-path-c0j/~01d?via=mdn), Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Una lezione interattiva che fornisce alcune indicazioni sulle classi CSS.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics")}}
