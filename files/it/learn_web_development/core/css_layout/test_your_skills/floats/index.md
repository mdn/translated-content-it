---
title: "Metti alla prova le tue competenze: Float"
short-title: "Test: Float"
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Floats
l10n:
  sourceCommit: 143f7345a4276156679d816a153470fe1fc6f3f8
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout")}}

Lo scopo di questo test di competenze è aiutare a valutare se si comprendono i [float in CSS](/it/docs/Learn_web_development/Core/CSS_layout/Floats), utilizzando le proprietà e i valori {{CSSxRef("float")}} e {{CSSxRef("clear")}}, nonché altri metodi per cancellare i float. Verranno proposti tre piccoli esercizi che utilizzano diversi elementi del materiale appena trattato.

> [!NOTE]
> Per ottenere assistenza, leggere la guida sull'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Float 1

Per completare questo esercizio, applicare il float a sinistra e a destra rispettivamente ai due elementi con classi `float1` e `float2`. Il testo dovrebbe quindi apparire tra i due elementi.

Il punto di partenza dell'esercizio è il seguente:

{{EmbedLiveSample("float1-start", "", "440px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___float1-start live-sample___float1-finish
<div class="box">
  <div class="float float1">One</div>
  <div class="float float2">Two</div>
  <p>The two boxes should float to either side of this text.</p>
</div>
```

```css live-sample___float1-start live-sample___float1-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

* {
  box-sizing: border-box;
}

.box {
  padding: 0.5em;
}

.float {
  margin: 15px;
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: rebeccapurple;
  color: white;
  padding: 1em;
}

.float1 {
  /* Add styles here */
}

.float2 {
  /* Add styles here */
}
```

Una volta completato l'esercizio, il layout dovrebbe essere simile a questo:

{{EmbedLiveSample("float1-finish", "", "210px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

È possibile usare `float` per entrambi i riquadri:

```css live-sample___float1-finish
.float1 {
  float: left;
}

.float2 {
  float: right;
}
```

</details>

## Float 2

Per completare questo esercizio:

1. Applicare il float a sinistra all'elemento con classe `float`.
2. Aggiornare il codice in modo che la prima riga di testo venga visualizzata accanto a quell'elemento, mentre la riga di testo successiva, con classe `below`, venga visualizzata sotto di esso.

Il punto di partenza dell'esercizio è il seguente:

{{EmbedLiveSample("float2-start", "", "300px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___float2-start live-sample___float2-finish
<div class="box">
  <div class="float">Float</div>
  <p>This sentence appears next to the float.</p>
  <p class="below">Make this sentence appear below the float.</p>
</div>
```

```css live-sample___float2-start live-sample___float2-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

* {
  box-sizing: border-box;
}

.box {
  padding: 0.5em;
}

.float {
  margin: 15px;
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: rebeccapurple;
  color: white;
  padding: 1em;
}

.float {
  /* Add styles here */
}

.below {
  /* Add styles here */
}
```

Il layout completato dovrebbe essere simile a questo:

{{EmbedLiveSample("float2-finish", "", "300px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Occorre posizionare l'elemento a sinistra, quindi aggiungere `clear: left` alla classe per il secondo paragrafo:

```css live-sample___float2-finish
.float {
  float: left;
}

.below {
  clear: left;
}
```

</details>

## Float 3

In questo esercizio è presente un elemento con float. Il riquadro di sfondo che avvolge il float e il testo attualmente non si estende sotto l'elemento con float.

Per completare questo esercizio, utilizzare il metodo più aggiornato per assicurarsi che il riquadro di sfondo contenga l'elemento con float e si estenda sotto di esso.

Il punto di partenza dell'esercizio è il seguente:

{{EmbedLiveSample("float3-start", "", "220px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___float3-start live-sample___float3-finish
<div class="box">
  <div class="float">Float</div>
  <p>This sentence appears next to the float.</p>
</div>
```

```css live-sample___float3-start live-sample___float3-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

* {
  box-sizing: border-box;
}

.box {
  padding: 0.5em;
}

.float {
  margin: 15px;
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: rgb(207 232 220);
  padding: 1em;
  color: white;
}

.box {
  background-color: rebeccapurple;
  padding: 10px;
  color: white;
}

.float {
  float: right;
}

.box {
  /* Add styles here */
}
```

Una volta completato l'esercizio, il riquadro di sfondo e l'elemento con float dovrebbero apparire così:

{{EmbedLiveSample("float3-finish", "", "220px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Cancellare il riquadro sotto l'elemento con float aggiungendo `display: flow-root` alla classe `.box`.
Altri metodi potrebbero utilizzare `overflow` o un hack clearfix; tuttavia, il materiale didattico descrive il metodo `flow-root` come il modo moderno per ottenere questo risultato.

```css live-sample___float3-finish
.box {
  display: flow-root;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Floats", "Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout")}}
