---
title: "Testa le tue competenze: Grid"
short-title: Grid
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Grid
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprende come si comportano una [griglia e gli elementi della griglia](/it/docs/Learn_web_development/Core/CSS_layout/Grids). Lavorerà attraverso diversi piccoli compiti che utilizzano diversi elementi del materiale che ha appena coperto.

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (cliccare sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se si trova in difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, dovrebbe creare una griglia nella quale i quattro elementi figli si posizioneranno automaticamente. La griglia dovrebbe avere tre colonne che condividono lo spazio disponibile in modo equo e uno spazio di 20 pixel tra le colonne e le righe. Successivamente, provi ad aggiungere ulteriori contenitori figli all'interno del contenitore padre con la classe `grid` e osservi come si comportano per impostazione predefinita.

Il suo risultato finale dovrebbe apparire come nell'immagine seguente:

![Una griglia a tre colonne con quattro elementi posizionati al suo interno.](grid-task1.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio terminato:

```html live-sample___grid1
<div class="grid">
  <div>One</div>
  <div>Two</div>
  <div>Three</div>
  <div>Four</div>
</div>
```

```css hidden live-sample___grid1
body {
  font: 1.2em / 1.5 sans-serif;
}
.grid > * {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: #fff;
  padding: 0.5em;
}
```

```css live-sample___grid1
.grid {
}
```

{{EmbedLiveSample("grid1", "", "200px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Crei una griglia utilizzando `display: grid` con tre colonne usando `grid-template-columns` e uno `gap` tra gli elementi:

```css
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
}
```

</details>

## Compito 2

In questo compito, abbiamo già definito una griglia. Modificando le regole CSS per i due elementi figli, li faccia estendere su più tracce della griglia ciascuno. Il secondo elemento dovrebbe sovrapporsi al primo come nell'immagine seguente:

![Una casella con due elementi all'interno, uno che si sovrappone all'altro.](grid-task2.png)

**Domanda bonus:** Può ora fare in modo che il primo elemento venga visualizzato sopra senza cambiare l'ordine degli elementi nel sorgente?

Provi ad aggiornare il codice qui sotto per ricreare l'esempio terminato:

```html live-sample___grid2
<div class="grid">
  <div class="item1">One</div>
  <div class="item2">Two</div>
</div>
```

```css hidden live-sample___grid2
body {
  font: 1.2em / 1.5 sans-serif;
}
.grid > * {
  border-radius: 0.5em;
  color: #fff;
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
```

```css live-sample___grid2
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr;
  grid-template-rows: 100px 100px 100px;
  gap: 10px;
}

.item1 {
}

.item2 {
}
```

{{EmbedLiveSample("grid2", "", "340px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

È possibile stratificare gli elementi facendoli occupare le stesse celle della griglia.
Una opzione è utilizzare le seguenti scorciatoie, tuttavia sarebbe corretto utilizzare la forma lunga, ad esempio `grid-row-start`.

```css
.item1 {
  grid-column: 1 / 4;
  grid-row: 1 / 3;
}

.item2 {
  grid-column: 2 / 5;
  grid-row: 2 / 4;
}
```

Per la domanda bonus, un modo per ottenere ciò sarebbe utilizzare `order`, che abbiamo incontrato nel tutorial sul flexbox.

```css
.item1 {
  order: 1;
}
```

Un'altra soluzione valida è utilizzare `z-index`:

```css
.item1 {
  z-index: 1;
}
```

</details>

## Compito 3

In questo compito, ci sono quattro figli diretti in questa griglia. Il punto di partenza li ha visualizzati usando il posizionamento automatico. Utilizzi le proprietà `grid-area` e `grid-template-areas` per disporre gli elementi come mostrato nell'immagine seguente:

![Quattro elementi visualizzati in una griglia.](grid-task3.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio terminato:

```html live-sample___grid3
<div class="grid">
  <div class="one">One</div>
  <div class="two">Two</div>
  <div class="three">Three</div>
  <div class="four">Four</div>
</div>
```

```css hidden live-sample___grid3
body {
  font: 1.2em / 1.5 sans-serif;
}
.grid > * {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: #fff;
  padding: 0.5em;
}
```

```css live-sample___grid3
.grid {
  display: grid;
  grid-template-columns: 1fr 2fr;
  gap: 10px;
}
```

{{EmbedLiveSample("grid3", "", "200px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Ogni parte del layout necessita di un nome utilizzando la proprietà `grid-area` e `grid-template-areas` per disporli. Possibili aree di confusione potrebbero essere non rendersi conto che si dovrebbe posizionare un `.` per lasciare una cella vuota, o che si dovrebbe ripetere il nome per fare in modo che un elemento si estenda su più di una traccia:

```css
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

## Compito 4

In questo compito, dovrà utilizzare sia il layout a griglia che il flexbox per ricreare l'esempio come visto nell'immagine seguente. Lo spazio tra le colonne e le righe dovrebbe essere di 10px. Non ha bisogno di apportare alcuna modifica all'HTML per raggiungere questo risultato.

![Due righe di carte, ciascuna con un'immagine e un insieme di tag.](grid-task4.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio terminato:

```html live-sample___grid4
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

```css hidden live-sample___grid4
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
  background-color: #999;
  color: #fff;
  padding: 0.2em 0.8em;
  border-radius: 0.2em;
  font-size: 80%;
  margin: 5px;
}
```

```css live-sample___grid4
.container {
}

.tags {
}
```

{{EmbedLiveSample("grid4", "", "400px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Il contenitore avrà bisogno di essere un layout a griglia, poiché abbiamo allineamento in righe e colonne - a due dimensioni.
Il `<ul>` deve essere un contenitore flessibile dato che i tag (elementi `<li>`) non sono allineati in colonne, solo in righe e sono centrati nello spazio con la proprietà di allineamento `justify-content` impostata su `center`.

Può provare a utilizzare flexbox sul contenitore e limitare le carte con valori percentuali. Può anche provare a trasformare gli elementi in un layout a griglia nel qual caso, noti che gli elementi non sono allineati su due dimensioni quindi il flexbox non è la scelta migliore.

```css
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

## Vedi anche

- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
