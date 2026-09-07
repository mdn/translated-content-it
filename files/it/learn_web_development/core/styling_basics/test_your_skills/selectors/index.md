---
title: "Metti alla prova le tue competenze: Selettori"
short-title: "Test: Selettori"
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Selectors
l10n:
  sourceCommit: 28f5f3b9b463fa842fa686ccc73c9e1d9b06282b
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics")}}

Lo scopo di questo test di competenze è aiutare a valutare se si comprendono i [selettori CSS](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors).

Per completare queste attività è necessario modificare solo il CSS, non l'HTML.

> [!NOTE]
> Per ottenere aiuto, leggere la Guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Selettori 1

Per completare l'attività:

1. Rendere blu le intestazioni `<h1>`.
2. Assegnare alle intestazioni `<h2>` uno sfondo blu e testo bianco.
3. Fare in modo che il testo racchiuso in uno `<span>` abbia un `font-size` del `200%`.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("selectors1-start", "", "370px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___selectors1-start live-sample___selectors1-finish
<div class="container">
  <h1>This is a heading</h1>
  <p>
    Veggies es <span>bonus vobis</span>, proinde vos postulo essum magis
    kohlrabi welsh onion daikon amaranth tatsoi tomatillo melon azuki bean
    garlic.
  </p>
  <h2>A level 2 heading</h2>
  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>
</div>
```

```css live-sample___selectors1-start live-sample___selectors1-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
/* Add styles here */
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("selectors1-finish", "", "400px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

È necessario selezionare i selettori `h1`, `h2` e `span` per modificarne il colore o la dimensione.

```css live-sample___selectors1-finish
h1 {
  color: blue;
}

h2 {
  background-color: blue;
  color: white;
}

span {
  font-size: 200%;
}
```

</details>

## Selettori 2

Per completare l'attività:

1. Assegnare uno sfondo giallo all'elemento con un id pari a `special`.
2. Assegnare all'elemento con una classe pari a `alert` un bordo grigio continuo di `2px`.
3. Se l'elemento con una classe pari a `alert` ha anche una classe pari a `stop`, rendere lo sfondo rosso.
4. Se l'elemento con una classe pari a `alert` ha anche una classe pari a `go`, rendere lo sfondo verde.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("selectors2-start", "", "480px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___selectors2-start live-sample___selectors2-finish
<div class="container">
  <h1>This is a heading</h1>
  <p>
    Veggies es <span class="alert">bonus vobis</span>, proinde vos postulo
    <span class="alert stop">essum magis</span> kohlrabi welsh onion daikon
    amaranth tatsoi tomatillo melon azuki bean garlic.
  </p>
  <h2 id="special">A level 2 heading</h2>
  <p>Gumbo beet greens corn soko endive gumbo gourd.</p>
  <h2>Another level 2 heading</h2>
  <p>
    <span class="alert go">Parsley shallot</span> courgette tatsoi pea sprouts
    fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber
    earthnut pea peanut soko zucchini.
  </p>
</div>
```

```css live-sample___selectors2-start live-sample___selectors2-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
/* Add styles here */
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("selectors2-finish", "", "480px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Questo verifica la comprensione della differenza tra selettori di classe e id, e anche di come selezionare più classi su un elemento.

```css live-sample___selectors2-finish
#special {
  background-color: yellow;
}

.alert {
  border: 2px solid grey;
}

.alert.stop {
  background-color: red;
}

.alert.go {
  background-color: green;
}
```

</details>

## Selettori 3

Per completare l'attività:

1. Applicare uno stile ai link: rendere arancione lo stato del link, verdi i link visitati e rimuovere la sottolineatura al passaggio del mouse.
2. Assegnare al primo elemento all'interno del contenitore `font-size: 150%` e rendere rossa la prima riga di quell'elemento.
3. Creare righe alternate nella tabella selezionando una riga sì e una no e assegnando loro un colore di sfondo `#333333` e testo bianco.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("selectors3-start", "", "440px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___selectors3-start live-sample___selectors3-finish
<div class="container">
  <p>
    Veggies es <a href="http://example.com">bonus vobis</a>, proinde vos postulo
    essum magis kohlrabi welsh onion daikon amaranth tatsoi tomatillo melon
    azuki bean garlic.
  </p>
  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>
  <table>
    <tbody>
      <tr>
        <th>Fruits</th>
        <th>Vegetables</th>
      </tr>
      <tr>
        <td>Apple</td>
        <td>Potato</td>
      </tr>
      <tr>
        <td>Orange</td>
        <td>Carrot</td>
      </tr>
      <tr>
        <td>Tomato</td>
        <td>Parsnip</td>
      </tr>
      <tr>
        <td>Kiwi</td>
        <td>Onion</td>
      </tr>
      <tr>
        <td>Banana</td>
        <td>Beet</td>
      </tr>
    </tbody>
  </table>
</div>
```

```css live-sample___selectors3-start live-sample___selectors3-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
* {
  box-sizing: border-box;
}

table {
  border-collapse: collapse;
  width: 300px;
}

td,
th {
  padding: 0.2em;
  text-align: left;
}

/* Add styles here */
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("selectors3-finish", "", "540px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Applicare una pseudo-classe (`:first-child`) e uno pseudo-elemento (`::first-line`) al contenuto.
Applicare uno stile agli stati `:link`, `:visited` e `:hover` dell'elemento `a`, quindi creare righe alternate nella tabella usando la pseudo-classe `:nth-child`.

```css live-sample___selectors3-finish
.container p:first-child {
  font-size: 150%;
}

.container p:first-child::first-line {
  color: red;
}

a:link {
  color: orange;
}

a:visited {
  color: green;
}

a:hover {
  text-decoration: none;
}

tr:nth-child(even) {
  background-color: #333333;
  color: white;
}
```

</details>

## Selettori 4

Per completare l'attività:

1. Rendere rosso qualsiasi paragrafo che segue direttamente un elemento `<h2>`.
2. Applicare il seguente stile agli elementi di elenco che sono figli diretti dell'elemento `<ul>` con classe `list`:
   - Rimuovere i punti elenco.
   - Assegnare loro un bordo inferiore grigio di `1px`.

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("selectors4-start", "", "500px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___selectors4-start live-sample___selectors4-finish
<div class="container">
  <h2>This is a heading</h2>
  <p>This paragraph comes after the heading.</p>
  <p>This is the second paragraph.</p>

  <h2>Another heading</h2>
  <p>This paragraph comes after the heading.</p>
  <ul class="list">
    <li>One</li>
    <li>
      Two
      <ul>
        <li>2.1</li>
        <li>2.2</li>
      </ul>
    </li>
    <li>Three</li>
  </ul>
</div>
```

```css live-sample___selectors4-start live-sample___selectors4-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

/* Add styles here */
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("selectors4-finish", "", "500px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Questa attività verifica la comprensione dell'uso di diversi combinatori.
Ecco una soluzione appropriata:

```css live-sample___selectors4-finish
h2 + p {
  color: red;
}

.list > li {
  list-style: none;
  border-bottom: 1px solid #cccccc;
}
```

</details>

## Selettori 5

Per completare l'attività, fornire soluzioni per le seguenti sfide utilizzando i selettori di attributo:

1. Selezionare l'elemento `<a>` con un attributo `title` e rendere rosa il bordo (`border-color: pink`).
2. Selezionare l'elemento `<a>` con un attributo `href` che contiene la parola `contact` in qualsiasi punto del suo valore e rendere arancione il bordo (`border-color: orange`).
3. Selezionare l'elemento `<a>` con un valore `href` che inizia con `https` e assegnargli un bordo verde (`border-color: green`).

Il punto di partenza dell'attività è simile a questo:

{{EmbedLiveSample("selectors5-start", "", "300px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___selectors5-start live-sample___selectors5-finish
<ul>
  <li><a href="https://example.com">Link 1</a></li>
  <li><a href="http://example.com" title="Visit example.com">Link 2</a></li>
  <li><a href="/contact">Link 3</a></li>
  <li><a href="../contact/index.html">Link 4</a></li>
</ul>
```

```css live-sample___selectors5-start live-sample___selectors5-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  margin-bottom: 0.5em;
}

a {
  display: block;
  padding: 0.5em;
}

a {
  border: 5px solid grey;
}

/* Add styles here */
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("selectors5-finish", "", "300px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

- Per selezionare elementi con un attributo `title`, è possibile aggiungere `title` all'interno delle parentesi quadre (`a[title]`); questo selezionerà il secondo link, che è l'unico con un attributo `title`.

- Selezionare l'elemento `<a>` con un attributo `href` che contiene la parola "contact" in qualsiasi punto del suo valore e rendere arancione il bordo (`border-color: orange`).
  Vi sono due elementi da trovare: il valore `href` `/contact` e anche `../contact`. Occorre quindi trovare la stringa "contact" in qualsiasi punto del valore usando `*=`. Questo selezionerà il terzo e il quarto link.

- Selezionare l'elemento `<a>` con un valore `href` che inizia con `https` e assegnargli un bordo verde (`border-color: green`).
  Cercare un valore `href` che inizi con "https", quindi usare `^=` per selezionare solo il primo link.

```css live-sample___selectors5-finish
a[title] {
  border-color: pink;
}
a[href*="contact"] {
  border-color: orange;
}
a[href^="https"] {
  border-color: green;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Combinators", "Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics")}}
