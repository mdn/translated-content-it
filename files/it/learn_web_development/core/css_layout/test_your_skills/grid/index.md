---
title: "Metti alla prova le tue competenze: griglie CSS"
short-title: "Test: griglia CSS"
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Grid
l10n:
  sourceCommit: 143f7345a4276156679d816a153470fe1fc6f3f8
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension", "Learn_web_development/Core/CSS_layout")}}

Lo scopo di questo test delle competenze è aiutare a valutare se si comprende come si comportano una [griglia e i relativi elementi](/it/docs/Learn_web_development/Core/CSS_layout/Grids). Verranno svolte diverse piccole attività che utilizzano differenti elementi del materiale appena trattato.

> [!NOTE]
> Per ottenere aiuto, leggere la Guida all'uso [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Griglie CSS 1

In questa attività, occorre creare una griglia nella quale i quattro elementi figli verranno posizionati automaticamente. La griglia deve avere tre colonne che condividono equamente lo spazio disponibile, con uno spazio di `20px` tra le tracce di colonne e righe. Successivamente, provare ad aggiungere altri elementi figli all'interno del contenitore genitore con la classe `grid` e osservare come si comportano per impostazione predefinita.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("grid1-start", "", "220px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___grid1-start live-sample___grid1-finish
<div class="grid">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
</div>
```

```css live-sample___grid1-start live-sample___grid1-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

.grid > * {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: white;
  padding: 0.5em;
}

.grid {
  /* Add styles here */
}
```

Il layout completato dovrebbe avere questo aspetto:

{{EmbedLiveSample("grid1-finish", "", "160px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Creare una griglia usando `display: grid` con tre colonne tramite `grid-template-columns` e un `gap` tra gli elementi:

```css live-sample___grid1-finish
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
}
```

</details>

## Griglie CSS 2

In questa attività, è già definita una griglia. Occorre modificare le regole CSS per i due elementi figli affinché ciascuno si estenda su più tracce della griglia. Il secondo elemento deve sovrapporsi al primo.

**Domanda bonus:** È ora possibile fare in modo che il primo elemento venga visualizzato sopra l'altro senza modificare l'ordine degli elementi nel sorgente?

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("grid2-start", "", "340px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___grid2-start live-sample___grid2-finish
<div class="grid">
  <div class="item1">One</div>
  <div class="item2">Two</div>
</div>
```

```css live-sample___grid2-start live-sample___grid2-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
.grid > * {
  border-radius: 0.5em;
  color: white;
  padding: 0.5em;
}

.item1 {
  background-color: rgb(74 102 112 / 70%);
  border: 5px solid rgb(74 102 112 / 100%);
}

.item2 {
  background-color: rgb(214 162 173 / 70%);
  border: 5px solid rgb(214 162 173 / 100%);
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 100px 100px 100px;
  gap: 10px;
}

.item1 {
  /* Add styles here */
}

.item2 {
  /* Add styles here */
}
```

Il layout dovrebbe avere questo aspetto dopo il completamento dell'attività:

{{EmbedLiveSample("grid2-finish", "", "340px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

È possibile disporre gli elementi su livelli facendoli occupare le stesse celle della griglia.
Un'opzione consiste nell'utilizzare le abbreviazioni riportate di seguito, tuttavia sarebbe corretto usare, ad esempio, la forma estesa `grid-row-start`.

```css live-sample___grid2-finish
.item1 {
  grid-column: 1 / 4;
  grid-row: 1 / 3;
}

.item2 {
  grid-column: 2 / 5;
  grid-row: 2 / 4;
}
```

Per la domanda bonus, un modo per ottenere questo risultato consiste nell'usare `order`, già incontrato nel tutorial su flexbox.

```css live-sample___grid2-finish
.item1 {
  order: 1;
}
```

Un'altra soluzione valida consiste nell'usare `z-index`:

```css
.item1 {
  z-index: 1;
}
```

</details>

## Griglie CSS 3

In questa attività, la griglia contiene quattro figli diretti. Attualmente vengono posizionati automaticamente nella griglia.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("grid3-start", "", "200px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___grid3-start live-sample___grid3-finish
<div class="grid">
  <div class="one">One</div>
  <div class="two">Two</div>
  <div class="three">Three</div>
  <div class="four">Four</div>
</div>
```

```css live-sample___grid3-start live-sample___grid3-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
.grid > * {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: white;
  padding: 0.5em;
}

.grid {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 10px;
}
```

Per completare questa attività, usare le proprietà `grid-area` e `grid-template-areas` per disporre gli elementi come mostrato qui:

{{EmbedLiveSample("grid3-finish", "", "200px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Ogni parte del layout necessita di un nome tramite la proprietà `grid-area` e di `grid-template-areas` per disporle. Possibili punti di confusione potrebbero essere non rendersi conto che è necessario inserire un `.` per lasciare vuota una cella, oppure che occorre ripetere il nome affinché un elemento si estenda su più di una traccia:

```css live-sample___grid3-finish
.grid {
  display: grid;
  gap: 20px;
  grid-template-columns: 1fr 2fr;
  grid-template-areas:
    "aa aa"
    "bb cc"
    ". dd";
}

.one {
  grid-area: aa;
}

.two {
  grid-area: bb;
}

.three {
  grid-area: cc;
}

.four {
  grid-area: dd;
}
```

</details>

## Griglie CSS 4

In questa attività, sarà necessario utilizzare sia il layout a griglia sia flexbox per ricreare il layout completato. Lo spazio tra le tracce di colonne e righe deve essere `10px`. Non è necessario apportare modifiche all'HTML per ottenere questo risultato.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("grid4-start", "", "400px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___grid4-start live-sample___grid4-finish
<div class="container">
  <div class="card">
    <img
      alt="a single red balloon"
      src="https://mdn.github.io/shared-assets/images/examples/balloons1.jpg" />
    <ul class="tags">
      <li>balloon</li>
      <li>red</li>
      <li>sky</li>
      <li>blue</li>
      <li>Hot air balloon</li>
    </ul>
  </div>
  <div class="card">
    <img
      alt="balloons over some houses"
      src="https://mdn.github.io/shared-assets/images/examples/balloons2.jpg" />
    <ul class="tags">
      <li>balloons</li>
      <li>houses</li>
      <li>train</li>
      <li>harborside</li>
    </ul>
  </div>
  <div class="card">
    <img
      alt="close-up of balloons inflating"
      src="https://mdn.github.io/shared-assets/images/examples/balloons3.jpg" />
    <ul class="tags">
      <li>balloons</li>
      <li>inflating</li>
      <li>green</li>
      <li>blue</li>
    </ul>
  </div>
  <div class="card">
    <img
      alt="a balloon in the sun"
      src="https://mdn.github.io/shared-assets/images/examples/balloons4.jpg" />
    <ul class="tags">
      <li>balloon</li>
      <li>sun</li>
      <li>sky</li>
      <li>summer</li>
      <li>bright</li>
    </ul>
  </div>
</div>
```

```css live-sample___grid4-start live-sample___grid4-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

.card {
  display: grid;
  grid-template-rows: 200px min-content;
}

.card > img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.tags {
  margin: 0;
  padding: 0;
  list-style: none;
}

.tags > * {
  background-color: #999999;
  color: white;
  padding: 0.2em 0.8em;
  border-radius: 0.2em;
  font-size: 80%;
  margin: 5px;
}

.container {
  /* Add styles here */
}

.tags {
  /* Add styles here */
}
```

Il layout dovrebbe avere questo aspetto dopo il completamento dell'attività:

{{EmbedLiveSample("grid4-finish", "", "400px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il contenitore dovrà usare un layout a griglia, poiché è presente un allineamento in righe e colonne, ovvero bidimensionale.
L'elemento `<ul>` deve essere un contenitore flex, poiché i tag (elementi `<li>`) non sono allineati in colonne, ma solo in righe, e sono centrati nello spazio con la proprietà di allineamento `justify-content` impostata su `center`.

Si potrebbe provare a usare flexbox sul contenitore e limitare le card con valori percentuali. Si potrebbe anche provare a trasformare gli elementi in un layout a griglia; in tal caso, notare che gli elementi non sono allineati in due dimensioni, quindi flexbox non è la scelta migliore.

```css live-sample___grid4-finish
.container {
  display: grid;
  gap: 10px;
  grid-template-columns: 1fr 1fr 1fr;
}

.tags {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Grids", "Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension", "Learn_web_development/Core/CSS_layout")}}
