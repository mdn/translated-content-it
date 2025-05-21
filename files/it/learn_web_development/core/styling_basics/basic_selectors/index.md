---
title: Selettori CSS di base
short-title: Selettori di base
slug: Learn_web_development/Core/Styling_basics/Basic_selectors
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics")}}

Hai già visto come, nei {{Glossary("CSS", "CSS")}}, i selettori siano utilizzati per mirare agli elementi {{Glossary("HTML", "HTML")}} nelle nostre pagine web che vogliamo stilizzare. Esiste una vasta gamma di selettori CSS disponibili, che permette una precisione dettagliata nella selezione degli elementi da stilizzare, e nei prossimi articoli esamineremo in dettaglio i diversi tipi. In questo articolo ripasseremo alcuni fondamenti dei selettori, inclusi i tipi di base, i selettori di classi e ID e gli elenchi di selettori. Introdurremo anche il selettore universale.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studia
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base dell'HTML</a
        >).
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I tipi di selettore di base — tipo di elemento, classe, ID.</li>
          <li>Capire che gli ID sono unici per documento — si dovrebbe usare un ID per selezionare un elemento specifico.</li>
          <li>Capire che puoi avere più classi per elemento e queste possono essere usate per applicare stili come richiesto.</li>
          <li>Elenchi di selettori.</li>
          <li>Selettore universale.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è un selettore?

Un selettore CSS è la prima parte di una regola CSS. È un modello di elementi e altri termini che indicano al browser quali elementi HTML devono essere selezionati per applicare i valori delle proprietà CSS all'interno della regola. L'elemento o gli elementi selezionati dal selettore si riferiscono come il _soggetto del selettore_.

![Del codice con h1 evidenziato.](selector.png)

Negli articoli precedenti potresti aver incontrato alcuni selettori diversi e aver appreso che ci sono selettori che mirano al documento in modi diversi — ad esempio selezionando un elemento come `h1` o una classe come `.special`. Iniziamo ripassando i principali che hai già visto.

## Selettori di tipo

Un **selettore di tipo** a volte viene chiamato _selettore di nome tag_ o _selettore di elemento_ perché seleziona un tag/elemento HTML nel tuo documento. Nell'esempio seguente, abbiamo utilizzato i selettori `span`, `em` e `strong`.

Prova ad aggiungere una regola CSS per selezionare l'elemento `<h1>` e cambiare il suo colore in blu:

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

Il selettore di classe sensibile alle maiuscole inizia con un carattere punto (`.`). Selezionerà tutto nel documento a cui è stata applicata quella classe. Nell'esempio attivo sotto, abbiamo creato una classe chiamata `highlight`, che è stata applicata in diversi punti del mio documento. Tutti gli elementi a cui è stata applicata la classe sono evidenziati.

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

### Mirare alle classi su elementi particolari

Puoi creare un selettore che miri a specifici elementi con la classe applicata. In questo prossimo esempio, evidenzieremo uno `<span>` con una classe `highlight` in modo diverso rispetto a un'intestazione `<h1>` con una classe `highlight`. Facciamo questo usando il selettore di tipo per l'elemento che vogliamo mirare, con la classe aggiunta usando un punto, senza spazi bianchi nel mezzo.

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

Questo approccio riduce l'ambito di una regola. La regola si applicherà solo a quella particolare combinazione di elemento e classe. Avresti bisogno di aggiungere un altro selettore se decidessi che la regola dovrebbe applicarsi anche ad altri elementi.

### Mirare a un elemento se ha più di una classe applicata

Puoi applicare più classi a un elemento e mirarle individualmente, o selezionare l'elemento solo quando tutte le classi nel selettore sono presenti. Questo può essere utile quando costruisci componenti che possono essere combinati in modi diversi sul tuo sito.

Nell'esempio seguente, abbiamo un `<div>` che contiene una nota. Il bordo grigio viene applicato quando il box ha una classe `notebox`. Se ha anche una classe `warning` o `danger`, cambiamo il {{cssxref("border-color")}}.

Possiamo dire al browser che vogliamo solo corrispondere all'elemento se ha due classi applicate legandole insieme senza spazi bianchi tra loro. Vedrai che l'ultimo `<div>` non riceve alcuno stile applicato, poiché ha solo la classe `danger`; ha bisogno anche di `notebox` affinché venga applicato qualcosa.

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

## Selettori di ID

Il selettore di ID sensibile alle maiuscole inizia con un `#` anziché con un carattere punto, ma viene usato nello stesso modo di un selettore di classe. La differenza è che un ID può essere utilizzato una sola volta per pagina ed elementi possono avere solo un singolo valore `id` applicato a loro. Può selezionare un elemento a cui è impostato l'`id`, e puoi precedere l'ID con un selettore di tipo per mirare solo all'elemento se sia l'elemento che l'ID corrispondono. Puoi vedere entrambi questi usi nel seguente esempio:

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
> Usare lo stesso ID più volte in un documento può sembrare funzionare per scopi di stile, ma non farlo. Risulta in codice non valido, e causerà comportamenti strani in molti contesti.

## Elenchi di selettori

Se hai più di una cosa che utilizza lo stesso CSS, i singoli selettori possono essere combinati in un _elenco di selettori_ in modo che la regola venga applicata a tutti i selettori individuali. Ad esempio, se ho lo stesso CSS per un `h1` e anche una classe `.special`, potrei scrivere questo come due regole separate.

```css
h1 {
  color: blue;
}

.special {
  color: blue;
}
```

Potrei anche combinarli in un elenco di selettori, aggiungendo una virgola tra loro.

```css-nolint
h1, .special {
  color: blue;
}
```

Lo spazio bianco è valido prima o dopo la virgola. Potresti anche trovare i selettori più leggibili se ciascuno è su una nuova riga.

```css
h1,
.special {
  color: blue;
}
```

Nell'esempio attivo sotto, prova a combinare i due selettori che hanno dichiarazioni identiche. La visualizzazione visiva dovrebbe essere la stessa dopo averli combinati.

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

Quando raggruppi i selettori in questo modo, se un qualsiasi selettore è sintatticamente non valido, l'intera regola verrà ignorata.

Nel seguente esempio, la regola del selettore di classe non valida sarà ignorata, mentre il `h1` verrà comunque stilizzato.

```css-nolint
h1 {
  color: blue;
}

..special {
  color: blue;
}
```

Quando combinati, tuttavia, né il `h1` né la classe saranno stilizzati poiché l'intera regola è considerata non valida.

```css-nolint
h1, ..special {
  color: blue;
}
```

## Il selettore universale

Il selettore universale è indicato da un asterisco (`*`). Seleziona tutto nel documento. Se `*` è concatenato usando un [combinatore discendente](/it/docs/Web/CSS/Descendant_combinator), seleziona tutto all'interno di quell'elemento antenato. Ad esempio, `p *` seleziona tutti gli elementi annidati all'interno dell'elemento `<p>`.

Nel seguente esempio, usiamo il selettore universale per rimuovere i margini su tutti gli elementi. Invece dello stile predefinito del browser, che spazia le intestazioni e i paragrafi con margini, tutto è vicino.

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

Questo tipo di comportamento può a volte essere visto in "foglio di stile reset", i quali eliminano tutta la stilizzazione del browser. Poiché il selettore universale apporta modifiche globali, lo usiamo per situazioni molto specifiche, come quella descritta di seguito.

### Utilizzare il selettore universale per rendere più leggibili i selettori

Uno degli utilizzi del selettore universale è rendere i selettori più leggibili ed evidenti per quanto riguarda ciò che stanno facendo. Ad esempio, se volessimo selezionare qualsiasi elemento discendente di un elemento `<article>` che è il primo figlio del suo genitore, compresi i figli diretti, e rendere il testo in grassetto, potremmo usare la pseudo-classe {{cssxref(":first-child")}}. Impareremo di più su questo nell'articolo sulle [pseudo-classi e pseudo-elementi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements):

```css
article :first-child {
  font-weight: bold;
}
```

Tuttavia, questo selettore potrebbe essere confuso con `article:first-child`, che selezionerà qualsiasi elemento `<article>` che è il primo figlio di un altro elemento.

Per evitare questa confusione, possiamo aggiungere il selettore universale alla pseudo-classe `:first-child`, in modo che sia più evidente cosa stia facendo il selettore. Seleziona _qualsiasi_ elemento che è il primo figlio di un elemento `<article>`, o il primo figlio di qualsiasi elemento discendente di `<article>`:

```css
article *:first-child {
  font-weight: bold;
}
```

Sebbene entrambi facciano la stessa cosa, la leggibilità è significativamente migliorata.

## Sommario

In questo articolo abbiamo ripassato i selettori CSS, che ti consentono di mirare a specifici elementi HTML, esaminando i selettori di tipo, classe e ID in modo più approfondito rispetto a quanto fatto in precedenza. Nel prossimo articolo ci addentreremo nei selettori di attributi.

> [!NOTE]
> Per un elenco completo dei selettori, consulta la nostra [reference sui selettori CSS](/it/docs/Web/CSS/CSS_selectors).

## Vedi anche

- [Classi CSS](https://scrimba.com/the-frontend-developer-career-path-c0j/~01d?via=mdn), Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Una lezione interattiva che fornisce alcune indicazioni sulle classi CSS.

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Styling_a_bio_page", "Learn_web_development/Core/Styling_basics/Attribute_selectors", "Learn_web_development/Core/Styling_basics")}}
