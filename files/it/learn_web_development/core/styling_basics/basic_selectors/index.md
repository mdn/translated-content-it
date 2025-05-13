---
title: Selettori CSS di base
short-title: Selettori di base
slug: Learn_web_development/Core/Styling_basics/Basic_selectors
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics")}}

Ha già visto come, in {{Glossary("CSS", "CSS")}}, i selettori sono utilizzati per mirare agli elementi {{Glossary("HTML", "HTML")}} sulle nostre pagine web che vogliamo stilizzare. Esiste una vasta gamma di selettori CSS disponibili, che consentono una precisione raffinata nella selezione degli elementi da stilizzare, e nei prossimi articoli esamineremo in dettaglio i diversi tipi. In questo articolo ricapitoleremo alcuni fondamenti sui selettori, inclusi i selettori di tipo base, classe e ID, e le liste dei selettori. Introdurremo anche il selettore universale.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studi
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base di HTML</a
        >).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>I tipi di selettori di base — tipo di elemento, classe, ID.</li>
          <li>Comprendere che gli ID sono unici per documento — si dovrebbe usare un ID per selezionare un elemento specifico.</li>
          <li>Comprendere che è possibile avere più classi per elemento, e queste possono essere utilizzate per stratificare gli stili come necessario.</li>
          <li>Liste di selettori.</li>
          <li>Selettore universale.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è un selettore?

Un selettore CSS è la prima parte di una regola CSS. È uno schema di elementi e altri termini che indicano al browser quali elementi HTML devono essere selezionati affinché i valori delle proprietà CSS all'interno della regola vengano applicati ad essi. L'elemento o gli elementi che vengono selezionati dal selettore sono definiti _oggetto del selettore_.

![Alcuni codici con l'h1 evidenziato.](selector.png)

In articoli precedenti potrebbe aver incontrato alcuni diversi selettori e appreso che esistono selettori che mirano al documento in modi diversi — ad esempio selezionando un elemento come `h1`, o una classe come `.special`. Iniziamo ricapitolando i principali che ha già visto.

## Selettori di tipo

Un **selettore di tipo** è a volte chiamato anche _selettore per nome di tag_ o _selettore di elemento_ perché seleziona un tag/elemento HTML nel suo documento. Nell'esempio seguente, abbiamo utilizzato i selettori `span`, `em` e `strong`.

Provi ad aggiungere una regola CSS per selezionare l'elemento `<h1>` e cambiare il suo colore in blu:

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

Il selettore di classe sensibile alle maiuscole inizia con un punto (`.`). Selezionerà tutto nel documento a cui è stata applicata quella classe. Nell'esempio live qui sotto abbiamo creato una classe chiamata `highlight`, e l'abbiamo applicata in diversi punti nel mio documento. Tutti gli elementi a cui è stata applicata la classe sono evidenziati.

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

### Mirare a classi su particolari elementi

Può creare un selettore che mirerà a specifici elementi a cui è stata applicata la classe. In questo prossimo esempio, evidenzieremo uno `<span>` con una classe di `highlight` in modo diverso rispetto a un'intestazione `<h1>` con una classe di `highlight`. Facciamo ciò utilizzando il selettore di tipo per l'elemento che vogliamo mirare, con la classe aggiunta usando un punto, senza spazi bianchi tra di essi.

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

Questo approccio riduce il campo di applicazione di una regola. La regola si applicherà solo a quella particolare combinazione di elemento e classe. Avrebbe bisogno di aggiungere un altro selettore se decidesse che la regola dovrebbe applicarsi anche ad altri elementi.

### Mirare a un elemento se ha più di una classe applicata

Può applicare più classi a un elemento e mirarle individualmente, o selezionare l'elemento solo quando tutte le classi nel selettore sono presenti. Ciò può essere utile quando si costruiscono componenti che possono essere combinati in modi diversi sul suo sito.

Nell'esempio seguente, abbiamo un `<div>` che contiene una nota. Il bordo grigio viene applicato quando la casella ha una classe di `notebox`. Se ha anche una classe di `warning` o `danger`, cambiamo il {{cssxref("border-color")}}.

Possiamo indicare al browser che vogliamo abbinare l'elemento solo se ha due classi applicate concatenandole senza spazi bianchi tra di loro. Noterà che l'ultima `<div>` non riceve alcun stile applicato, poiché ha solo la classe `danger`; ha bisogno anche di `notebox` affinché qualcosa venga applicato.

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
  border: 4px solid #666;
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

Il selettore ID sensibile alle maiuscole inizia con un `#` anziché con un punto, ma viene utilizzato nello stesso modo di un selettore di classe. La differenza è che un ID può essere usato solo una volta per pagina, e gli elementi possono avere applicato solo un singolo valore `id`. Può selezionare un elemento che ha impostato l`id` su di esso, e può precedere l'ID con un selettore di tipo per mirare solo all'elemento se entrambi l'elemento e l'ID coincidono. Può vedere entrambi questi usi nel seguente esempio:

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
> Usare lo stesso ID più volte in un documento può apparire funzionare per scopi di stilizzazione, ma non lo faccia. Risulta in codice non valido, e causerà comportamenti strani in molti contesti.

## Liste di selettori

Se ha più di un elemento che utilizza lo stesso CSS, allora i selettori individuali possono essere combinati in una _lista di selettori_ in modo che la regola venga applicata a tutti i selettori individuali. Ad esempio, se ho lo stesso CSS per un `h1` e anche per una classe di `.special`, potrei scriverlo come due regole separate.

```css
h1 {
  color: blue;
}

.special {
  color: blue;
}
```

Potrei anche combinarli in una lista di selettori, aggiungendo una virgola tra di essi.

```css-nolint
h1, .special {
  color: blue;
}
```

Lo spazio bianco è valido prima o dopo la virgola. Potrebbe anche trovare i selettori più leggibili se ciascuno è su una nuova riga.

```css
h1,
.special {
  color: blue;
}
```

Nell'esempio live qui sotto provi a combinare i due selettori che hanno dichiarazioni identiche. La visualizzazione visiva dovrebbe essere la stessa dopo averli combinati.

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

Quando raggruppa i selettori in questo modo, se un qualsiasi selettore è sintatticamente invalido, l'intera regola sarà ignorata.

Nel seguente esempio, la regola del selettore di classe invalida sarà ignorata, mentre l'`h1` verrebbe comunque stilizzato.

```css-nolint
h1 {
  color: blue;
}

..special {
  color: blue;
}
```

Quando combinati tuttavia, né l'`h1` né la classe saranno stilizzati poiché l'intera regola è considerata non valida.

```css-nolint
h1, ..special {
  color: blue;
}
```

## Il selettore universale

Il selettore universale è indicato da un asterisco (`*`). Seleziona tutto nel documento. Se `*` è concatenato usando un [combinatore discendente](/it/docs/Web/CSS/Descendant_combinator), seleziona tutto all'interno di quell'elemento antenato. Ad esempio, `p *` seleziona tutti gli elementi annidati all'interno dell'elemento `<p>`.

Nel seguente esempio, usiamo il selettore universale per rimuovere i margini su tutti gli elementi. Invece dello stile predefinito del browser, che spazia le intestazioni e i paragrafi con margini, tutto è ravvicinato.

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

Questo tipo di comportamento può a volte essere visto in "fogli di stile di reset", che eliminano tutto lo stile del browser. Poiché il selettore universale effettua cambiamenti globali, lo usiamo per situazioni molto specifiche, come quella descritta di seguito.

### Usare il selettore universale per rendere i suoi selettori più leggibili

Un uso del selettore universale è far sì che i selettori siano più leggibili e più ovvi in termini di cosa stanno facendo. Ad esempio, se volessimo selezionare qualsiasi elemento discendente di un elemento `<article>` che sia il primo figlio del loro genitore, inclusi i figli diretti, e renderli in grassetto, potremmo usare la pseudo-classe {{cssxref(":first-child")}}. Ne impareremo di più nella lezione sulle [pseudo-classi e pseudo-elementi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements):

```css
article :first-child {
  font-weight: bold;
}
```

Tuttavia, questo selettore potrebbe essere confuso con `article:first-child`, che selezionerà qualsiasi elemento `<article>` che è il primo figlio di un altro elemento.

Per evitare questa confusione, possiamo aggiungere il selettore universale alla pseudo-classe `:first-child`, in modo che sia più ovvio cosa sta facendo il selettore. Sta selezionando _qualsiasi_ elemento che è il primo figlio di un elemento `<article>`, o il primo figlio di qualsiasi elemento discendente di `<article>`:

```css
article *:first-child {
  font-weight: bold;
}
```

Anche se entrambi fanno la stessa cosa, la leggibilità è notevolmente migliorata.

## Riepilogo

In questo articolo abbiamo ricapitolato i selettori CSS, che le permettono di mirare a particolari elementi HTML, esaminando i selettori di tipo, classe e ID in modo più approfondito rispetto a quanto fatto in precedenza. Nel prossimo articolo entreremo nei selettori di attributo.

> [!NOTE]
> Per un elenco completo dei selettori, consulti il nostro [riferimento ai selettori CSS](/it/docs/Web/CSS/CSS_selectors).

## Vedere anche

- [Classi CSS](https://scrimba.com/the-frontend-developer-career-path-c0j/~01d?via=mdn), Scrimba <sup>[_Partner didattico di MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Una lezione interattiva che fornisce alcune indicazioni sulle classi CSS.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics")}}
