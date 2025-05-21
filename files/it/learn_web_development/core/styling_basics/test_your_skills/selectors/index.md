---
title: "Metti alla prova le tue competenze: Selettori"
short-title: Selectors
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Selectors
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test sulle competenze è valutare se comprendi i [selettori CSS](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors).

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice sottostanti per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (clicca sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se hai difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, usa CSS per fare le seguenti cose, senza cambiare l'HTML:

- Rendere blu gli intestazioni `<h1>`.
- Dare agli intestazioni `<h2>` uno sfondo blu e testo bianco.
- Far sì che il testo racchiuso in uno `<span>` abbia una dimensione del carattere del 200%.

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Testo con il CSS applicato per la soluzione del compito 1.](selectors1.jpg)

Prova ad aggiornare il codice sottostante per ricreare l'esempio finale:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Devi selezionare i selettori `h1`, `h2` e `span` per cambiare il loro colore o dimensione.

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

## Compito 2

In questo compito, vogliamo che tu faccia le seguenti modifiche all'aspetto del contenuto in questo esempio, senza cambiare l'HTML:

- Dare all'elemento con l'id `special` uno sfondo giallo.
- Dare all'elemento con la classe `alert` un bordo grigio di 2px.
- Se l'elemento con una classe `alert` ha anche una classe `stop`, rendere lo sfondo rosso.
- Se l'elemento con una classe `alert` ha anche una classe `go`, rendere lo sfondo verde.

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Testo con il CSS applicato per la soluzione del compito 2.](selectors2.jpg)

Prova ad aggiornare il codice sottostante per ricreare l'esempio finale:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Questo test verifica se comprendi la differenza tra selettori di classi e id e anche come selezionare elementi con più classi.

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

## Compito 3

In questo compito, vogliamo che tu apporti le seguenti modifiche senza cambiare l'HTML:

- Stilizzare i collegamenti, rendendo lo stato del link arancione, i link visitati verdi e rimuovere la sottolineatura al passaggio del mouse.
- Rendere il primo elemento all'interno del contenitore `font-size: 150%` e la prima riga di quell'elemento rossa.
- Stile ogni altra riga nella tabella selezionandole e dando loro un colore di sfondo di `#333` e testo bianco.

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Testo con il CSS applicato per la soluzione del compito 3.](selectors3.jpg)

Prova ad aggiornare il codice sottostante per ricreare l'esempio finale:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Applica una pseudo-classe (`:first-child`) e un pseudo-elemento (`::first-line`) al contenuto.
Stilizza gli stati `:link`, `:visited` e `:hover` dell'elemento `a` e crea righe tabellari a righe alterne usando la pseudo-classe `:nth-child`.

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

## Compito 4

In questo compito, vogliamo che tu faccia quanto segue:

- Rendere rosso qualsiasi paragrafo che segua direttamente un elemento `<h2>`.
- Rimuovere i punti elenco e aggiungere un bordo inferiore grigio di 1px solo agli elementi di lista che sono figli diretti del `ul` con una classe `list`.

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Testo con il CSS applicato per la soluzione del compito 4.](selectors4.jpg)

Prova ad aggiornare il codice sottostante per ricreare l'esempio finale:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Questo compito verifica se comprendi come utilizzare i diversi combinatori.
Ecco una soluzione adeguata:

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

## Compito 5

In questo compito, aggiungi CSS utilizzando i selettori di attributi per fare quanto segue:

- Selezionare l'elemento `<a>` con un attributo `title` e rendere il bordo rosa (`border-color: pink`).
- Selezionare l'elemento `<a>` con un attributo `href` che contiene la parola `contact` in qualsiasi parte del suo valore e rendere il bordo arancione (`border-color: orange`).
- Selezionare l'elemento `<a>` con un valore `href` che inizia con `https` e dargli un bordo verde (`border-color: green`).

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Quattro link con bordi di colore diverso.](selectors-attribute.png)

Prova ad aggiornare il codice sottostante per ricreare l'esempio finale:

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
<summary>Clicca qui per mostrare la soluzione</summary>

- Per selezionare gli elementi con un attributo title, possiamo aggiungere title tra parentesi quadre (`a[title]`), che selezionerà il secondo link, che è l'unico con un attributo title.

- Seleziona l'elemento `<a>` con un attributo `href` che contiene la parola "contact" ovunque nel suo valore e rendi il bordo arancione (`border-color: orange`).
  Ci sono due valori che vogliamo abbinare qui, il valore href `/contact` e anche `../contact`. Quindi dobbiamo abbinare la stringa "contact" ovunque nel valore usando `*=`. Questo selezionerà il terzo e quarto link.

- Seleziona l'elemento `<a>` con un valore href che inizia con `https` e dagli un bordo verde (`border-color: green`).
  Cerca un valore `href` che inizi con "https", quindi usa `^=` per selezionare solo il primo link.

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

## Vedi anche

- [CSS styling basics](/it/docs/Learn_web_development/Core/Styling_basics)
