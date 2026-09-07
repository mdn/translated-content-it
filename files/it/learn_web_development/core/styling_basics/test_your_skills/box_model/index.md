---
title: "Metti alla prova le tue competenze: il box model"
short-title: "Test: box model"
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model
l10n:
  sourceCommit: a623d4459e2aa00d17dc0fd6b6bc44f56c589950
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics")}}

L'obiettivo di questo test sulle competenze è aiutare a valutare se il [box model CSS](/it/docs/Learn_web_development/Core/Styling_basics/Box_model) è stato compreso.

> [!NOTE]
> Per ottenere aiuto, leggere la nostra Guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Sfida interattiva

Prima di tutto, viene proposta una divertente sfida interattiva che riguarda la forma abbreviata di `margin`, creata dal nostro [partner per l'apprendimento](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds), [Scrimba](https://scrimba.com/home).

Guardare lo scrim incorporato e completare le attività nella sequenza temporale (le piccole icone a forma di fantasma) seguendo le istruzioni e modificando il codice. Al termine, è possibile riprendere la visione dello scrim per verificare come la soluzione dell'insegnante si confronta con la propria.

<mdn-scrim-inline url="https://scrimba.com/learn-html-and-css-c0p/~01s" scrimtitle="Margin shorthand" survey="true"></mdn-scrim-inline>

## Box model 1

In questa attività, sono presenti due riquadri: uno utilizza il box model standard, l'altro il box model alternativo. Occorre modificare la larghezza del secondo riquadro aggiungendo dichiarazioni alla classe `.alternate`, in modo che corrisponda alla larghezza visiva del primo riquadro.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("box-model1-start", "", "540px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___box-model1-start live-sample___box-model1-finish
<div class="box">I use the standard box model.</div>
<div class="box alternate">I use the alternate box model.</div>
```

```css live-sample___box-model1-start live-sample___box-model1-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
.box {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding: 40px;
  margin: 40px;
  width: 300px;
  height: 150px;
}

.alternate {
  box-sizing: border-box;
}
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("box-model1-finish", "", "540px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Occorre aumentare la larghezza del secondo blocco aggiungendo la dimensione del padding e del border:

```css live-sample___box-model1-finish
.alternate {
  box-sizing: border-box;
  width: 390px;
}
```

</details>

## Box model 2

Per completare questa attività, aggiungere le seguenti caratteristiche al riquadro fornito:

- Un border puntinato nero di `5px`.
- Un margin superiore di `20px`.
- Un margin destro di `1em`.
- Un margin inferiore di `40px`.
- Un margin sinistro di `2em`.
- Un padding di `1em` su tutti i lati.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("box-model2-start", "100%", "100px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___box-model2-start live-sample___box-model2-finish
<div class="box">I use the standard box model.</div>
```

```css live-sample___box-model2-start live-sample___box-model2-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

.box {
}
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("box-model2-finish", "100%", "140px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Questa attività richiede l'uso corretto delle proprietà `margin`, `border` e `padding`.
Si potrebbe scegliere di utilizzare le proprietà estese ({{cssxref("margin-top")}}, {{cssxref("margin-right")}}, ecc.); tuttavia, quando si impostano margin e padding su tutti i lati, probabilmente la forma abbreviata è la scelta migliore:

```css live-sample___box-model2-finish
.box {
  border: 5px dotted black;
  margin: 20px 1em 40px 2em;
  padding: 1em;
}
```

</details>

## Box model 3

In questa attività, l'elemento inline ha margin, padding e border. Tuttavia, le righe sopra e sotto si sovrappongono a esso.

Per completare questa attività, aggiornare il CSS in modo che le dimensioni di margin, padding e border vengano rispettate dalle altre righe, mantenendo comunque l'elemento inline.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("box-model3-start", "100%", "220px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___box-model3-start live-sample___box-model3-finish
<div class="box">
  <p>
    Veggies es bonus vobis, <span>proinde vos postulo</span> essum magis
    kohlrabi welsh onion daikon amaranth tatsoi tomatillo melon azuki bean
    garlic.
  </p>

  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>
</div>
```

```css live-sample___box-model3-start live-sample___box-model3-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

.box span {
  background-color: pink;
  border: 5px solid black;
  padding: 1em;
}
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("box-model3-finish", "100%", "260px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

La soluzione di questa attività richiede di comprendere quando utilizzare diversi valori di {{cssxref("display")}}.
Dopo aver aggiunto `display: inline-block`, margin, border e padding nella direzione del blocco faranno sì che le altre righe vengano allontanate dall'elemento:

```css live-sample___box-model3-finish
.box span {
  background-color: pink;
  border: 5px solid black;
  padding: 1em;
  display: inline-block;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Box_model", "Learn_web_development/Core/Styling_basics/Handling_conflicts", "Learn_web_development/Core/Styling_basics")}}
