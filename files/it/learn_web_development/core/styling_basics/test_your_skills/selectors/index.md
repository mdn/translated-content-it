---
title: "Metti alla prova le sue abilità: Selettori"
short-title: Selectors
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Selectors
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprende i [selettori CSS](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors).

> [!NOTE]
> Faccia clic su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (cliccando sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se ha difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Esercizio 1

In questo esercizio, utilizzi il CSS per fare le seguenti cose, senza cambiare l'HTML:

- Rendere i titoli `<h1>` blu.
- Dare ai titoli `<h2>` uno sfondo blu e testo bianco.
- Fare in modo che il testo racchiuso in uno `<span>` abbia una dimensione del carattere del 200%.

Il suo risultato finale dovrebbe assomigliare all'immagine sottostante:

![Testo con il CSS applicato per la soluzione all'esercizio 1.](selectors1.jpg)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___type
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

```css live-sample___type
body {
  font: 1.2em / 1.5 sans-serif;
}
/* Add styles here */
```

{{EmbedLiveSample("type", "", "260px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Deve mirare ai selettori `h1`, `h2` e `span` per cambiare il loro colore o dimensione.

```css
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

## Esercizio 2

In questo esercizio, le chiediamo di apportare le seguenti modifiche all'aspetto del contenuto in questo esempio, senza cambiare l'HTML:

- Dare all'elemento con un id di `special` uno sfondo giallo.
- Dare all'elemento con una classe di `alert` un bordo grigio di 2px.
- Se l'elemento con una classe di `alert` ha anche una classe di `stop`, rendere lo sfondo rosso.
- Se l'elemento con una classe di `alert` ha anche una classe di `go`, rendere lo sfondo verde.

Il suo risultato finale dovrebbe assomigliare all'immagine sottostante:

![Testo con il CSS applicato per la soluzione all'esercizio 2.](selectors2.jpg)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___class-id
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

```css live-sample___class-id
body {
  font: 1.2em / 1.5 sans-serif;
}
/* Add styles here */
```

{{EmbedLiveSample("class-id", "", "320px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Questo test serve a verificare che lei comprenda la differenza tra selettori di classe e id e anche come mirare a più classi su un elemento.

```css
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

## Esercizio 3

In questo esercizio, le chiediamo di apportare le seguenti modifiche senza cambiare l'HTML:

- Stilare i link, rendendo lo stato del link arancione, i link visitati verdi, e rimuovere la sottolineatura al passaggio del mouse.
- Rendere il primo elemento all'interno del contenitore con `font-size: 150%` e la prima riga di quel elemento rossa.
- Strisciare ogni altra riga della tabella selezionando queste righe e dando loro un colore di sfondo `#333` e il testo bianco.

Il suo risultato finale dovrebbe assomigliare all'immagine sottostante:

![Testo con il CSS applicato per la soluzione all'esercizio 3.](selectors3.jpg)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___pseudo
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

```css hidden live-sample___pseudo
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
```

```css live-sample___pseudo
/* Add styles here */
```

{{EmbedLiveSample("pseudo", "", "320px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Applichi una pseudo-classe (`:first-child`) e un pseudo-elemento (`::first-line`) al contenuto.
Stili gli stati `:link`, `:visited` e `:hover` dell'elemento `a`, e crei righe di tabella a strisce usando la pseudo-classe `:nth-child`.

```css
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
  background-color: #333;
  color: #fff;
}
```

</details>

## Esercizio 4

In questo esercizio, le chiediamo di fare quanto segue:

- Rendere rossi tutti i paragrafi che seguono direttamente un elemento `<h2>`.
- Rimuovere i punti elenco e aggiungere un bordo inferiore grigio di 1px solo agli elementi della lista che sono un figlio diretto di un `ul` con una classe di `list`.

Il suo risultato finale dovrebbe assomigliare all'immagine sottostante:

![Testo con il CSS applicato per la soluzione all'esercizio 4.](selectors4.jpg)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___combinators
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

```css live-sample___combinators
body {
  font: 1.2em / 1.5 sans-serif;
}
/* Add styles here */
```

{{EmbedLiveSample("combinators", "", "350px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Questo esercizio verifica che lei comprenda come utilizzare diversi combinatori.
Ecco una soluzione appropriata:

```css
h2 + p {
  color: red;
}

.list > li {
  list-style: none;
  border-bottom: 1px solid #ccc;
}
```

</details>

## Esercizio 5

In questo esercizio, aggiunga il CSS usando i selettori di attributo per fare quanto segue:

- Mirare all'elemento `<a>` con un attributo `title` e rendere il bordo rosa (`border-color: pink`).
- Mirare all'elemento `<a>` con un attributo `href` che contiene la parola `contact` da qualche parte nel suo valore e rendere il bordo arancione (`border-color: orange`).
- Mirare all'elemento `<a>` con un valore `href` che inizia con `https` e dare ad esso un bordo verde (`border-color: green`).

Il suo risultato finale dovrebbe assomigliare all'immagine sottostante:

![Quattro link con bordi di colori diversi.](selectors-attribute.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___attribute-links
<ul>
  <li><a href="https://example.com">Link 1</a></li>
  <li><a href="http://example.com" title="Visit example.com">Link 2</a></li>
  <li><a href="/contact">Link 3</a></li>
  <li><a href="../contact/index.html">Link 4</a></li>
</ul>
```

```css hidden live-sample___attribute-links
body {
  font: 1.2em / 1.5 sans-serif;
}

ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

li {
  margin: 0 0 0.5em 0;
}

a {
  display: block;
  padding: 0.5em;
}
```

```css live-sample___attribute-links
a {
  border: 5px solid grey;
}
/* Add styles here */
```

{{EmbedLiveSample("attribute-links", "", "300px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

- Per selezionare elementi con un attributo title possiamo aggiungere title dentro le parentesi quadre (`a[title]`), il che selezionerà il secondo link, che è l'unico con un attributo title.

- Mirare all'elemento `<a>` con un attributo `href` che contiene la parola "contact" da qualche parte nel suo valore e rendere il bordo arancione (`border-color: orange`).
  Ci sono due cose che vogliamo abbinare qui, il valore href `/contact` e anche `../contact`. Quindi dobbiamo abbinare la stringa "contact" da qualche parte nel valore usando `*=`. Questo selezionerà il terzo e quarto link.

- Mirare all'elemento `<a>` con un valore href che inizia con `https` e dare ad esso un bordo verde (`border-color: green`).
  Cercare un valore `href` che inizia con "https", quindi usare `^=` per selezionare solo il primo link.

```css
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

## Vedere anche

- [Basi della stilizzazione CSS](/it/docs/Learn_web_development/Core/Styling_basics)
