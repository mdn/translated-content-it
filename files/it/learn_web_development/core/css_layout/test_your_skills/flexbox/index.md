---
title: "Metti alla prova le tue competenze: Flexbox"
short-title: "Test: Flexbox"
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Flexbox
l10n:
  sourceCommit: 143f7345a4276156679d816a153470fe1fc6f3f8
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout")}}

L'obiettivo di questo test di competenze è aiutare a valutare se si comprende il comportamento di [flexbox e flex item](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox). Di seguito sono disponibili quattro serie di problemi di progettazione risolvibili usando flexbox. Il compito consiste nel risolvere i problemi.

> [!NOTE]
> Per ricevere assistenza, leggere la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Sfida interattiva

Prima di tutto, viene proposta una divertente sfida interattiva su flexbox creata dal nostro [partner didattico](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds), [Scrimba](https://scrimba.com/home).

Guardare lo scrim incorporato e completare tutte le attività nella timeline (le piccole icone a forma di fantasma) seguendo le istruzioni e modificando il codice. Al termine, è possibile riprendere la visione dello scrim per verificare in che modo la soluzione dell'insegnante corrisponde alla propria.

<mdn-scrim-inline url="https://scrimba.com/frontend-path-c0j/~03a" scrimtitle="Sfide di allineamento Flexbox" survey="true"></mdn-scrim-inline>

## Flexbox 1

In questa attività, vengono utilizzati alcuni elementi di elenco per creare la navigazione di un sito. Per completare l'attività, usare flexbox per disporre gli elementi di elenco in una riga, con uno spazio uguale tra ciascun elemento.

Il punto di partenza dell'attività è questo:

{{EmbedLiveSample("flexbox1-start", "", "240px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___flexbox1-start live-sample___flexbox1-finish
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About Us</a></li>
    <li><a href="/products">Our Products</a></li>
    <li><a href="/contact">Contact Us</a></li>
  </ul>
</nav>
```

```css live-sample___flexbox1-start live-sample___flexbox1-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
nav ul {
  max-width: 750px;
  list-style: none;
  padding: 0;
  margin: 0;
}
nav a:link,
nav a:visited {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: white;
  padding: 0.5em;
  display: inline-block;
  text-decoration: none;
}

nav ul {
  /* Add styles here */
}
```

Quando l'attività è completata, gli elementi dovrebbero apparire così:

{{EmbedLiveSample("flexbox1-finish", "", "100px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

È possibile applicare `display: flex` e controllare la spaziatura usando la proprietà `justify-content`:

```css live-sample___flexbox1-finish
nav ul {
  display: flex;
  justify-content: space-between;
}
```

</details>

## Flexbox 2

In questa attività, gli elementi di elenco hanno tutti dimensioni diverse, ma devono essere visualizzati come tre colonne di uguali dimensioni, indipendentemente dal contenuto di ciascun elemento.

**Domanda bonus:** È ora possibile rendere il primo elemento grande il doppio degli altri elementi?

Il punto di partenza dell'attività è questo:

{{EmbedLiveSample("flexbox2-start", "", "240px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___flexbox2-start live-sample___flexbox2-finish
<ul>
  <li>I am small</li>
  <li>I have more content than the very small item.</li>
  <li>
    I have lots of content. So much content that I don't know where it is all
    going to go. I'm glad that CSS is pretty good at dealing with situations
    where we end up with more words than expected!
  </li>
</ul>
```

```css live-sample___flexbox2-start live-sample___flexbox2-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
ul {
  max-width: 750px;
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: white;
  padding: 0.5em;
}

ul {
  /* Add styles here */
}

li {
  /* Add styles here */
}
```

Quando l'attività è completata, gli elementi dovrebbero apparire così:

{{EmbedLiveSample("flexbox2-finish", "", "380px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

È preferibile usare le abbreviazioni, quindi in questo scenario `flex: 1` è probabilmente la risposta migliore e il risultato più ottimale sarebbe:

```css live-sample___flexbox2-finish
ul {
  display: flex;
}

li {
  flex: 1;
}
```

Per la domanda bonus, aggiungere un selettore che selezioni il primo elemento e imposti `flex: 2;` (oppure `flex: 2 0 0;` o `flex-grow: 2`):

```css live-sample___flexbox2-finish
li:first-child {
  flex: 2;
}
```

</details>

## Flexbox 3

In questa attività, viene richiesto di disporre gli elementi di elenco in righe.

Il punto di partenza dell'attività è questo:

{{EmbedLiveSample("flexbox3-start", "", "260px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___flexbox3-start live-sample___flexbox3-finish
<ul>
  <li>Turnip</li>
  <li>greens</li>
  <li>yarrow</li>
  <li>ricebean</li>
  <li>rutabaga</li>
  <li>endive</li>
  <li>cauliflower</li>
  <li>sea lettuce</li>
  <li>kohlrabi</li>
  <li>amaranth</li>
</ul>
```

```css live-sample___flexbox3-start live-sample___flexbox3-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
ul {
  width: 450px;
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: white;
  padding: 0.5em;
  margin: 0.5em;
}

ul {
  /* Add styles here */
}

li {
  /* Add styles here */
}
```

Quando l'attività è completata, gli elementi dovrebbero apparire così:

{{EmbedLiveSample("flexbox3-finish", "", "260px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Questa attività richiede di comprendere la proprietà `flex-wrap` per mandare a capo le righe flex. Inoltre, per assicurarsi di ottenere un risultato simile all'esempio, è necessario impostare `flex: auto` sull'elemento figlio (oppure `flex: 1 1 auto;`).

```css live-sample___flexbox3-finish
ul {
  display: flex;
  flex-wrap: wrap;
}

li {
  flex: auto;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout")}}
