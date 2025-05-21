---
title: "Metti alla prova le tue abilità: Floats"
short-title: Floats
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Floats
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprendi [i float in CSS](/it/docs/Learn_web_development/Core/CSS_layout/Floats) utilizzando le proprietà e i valori {{CSSxRef("float")}} e {{CSSxRef("clear")}} oltre ad altri metodi per eliminare i float. Lavorerai su tre piccoli compiti che utilizzano diversi elementi del materiale che hai appena coperto.

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel Playground di MDN.
> Puoi anche copiare il codice (clicca sull'icona degli appunti) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se rimani bloccato, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, devi posizionare gli elementi con una classe `float1` e `float2` a sinistra e a destra, rispettivamente. Il testo dovrebbe apparire tra i due riquadri, come nell'immagine qui sotto:

![Due blocchi visualizzati a sinistra e a destra di un testo.](float-task1.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___float1
<div class="box">
  <div class="float float1">One</div>
  <div class="float float2">Two</div>
  <p>The two boxes should float to either side of this text.</p>
</div>
```

```css hidden live-sample___float1
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
  color: #fff;
  padding: 1em;
}
```

```css live-sample___float1
.float1 {
}

.float2 {
}
```

{{EmbedLiveSample("float1", "", "210px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Puoi usare `float` per entrambi i riquadri:

```css
.float1 {
  float: left;
}

.float2 {
  float: right;
}
```

</details>

## Compito 2

In questo compito, l'elemento con una classe `float` dovrebbe essere posizionato a sinistra. Poi vogliamo che la prima riga di testo venga mostrata accanto a quell'elemento, ma la riga di testo successiva (che ha una classe `below`) venga visualizzata sotto di esso.

Il risultato finale dovrebbe apparire come nell'immagine qui sotto:

![Un riquadro visualizzato a sinistra di una riga di testo, con altro testo sotto.](float-task2.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___float2
<div class="box">
  <div class="float">Float</div>
  <p>This sentence appears next to the float.</p>
  <p class="below">Make this sentence appear below the float.</p>
</div>
```

```css hidden live-sample___float2
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
  color: #fff;
  padding: 1em;
}
```

```css live-sample___float2
.float {
}

.below {
}
```

{{EmbedLiveSample("float2", "", "300px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Devi posizionare l'elemento a sinistra, quindi aggiungere `clear: left` alla classe per il secondo paragrafo:

```css
.float {
  float: left;
}

.below {
  clear: left;
}
```

</details>

## Compito 3

In questo compito, abbiamo un elemento fluttuante. Il riquadro che avvolge il float e il testo viene visualizzato dietro il float. Usa il metodo più aggiornato disponibile per fare in modo che lo sfondo del riquadro si estenda sotto il float, come nell'immagine qui sotto:

![Un blocco visualizzato a destra di un testo entrambi avvolti da un riquadro con un colore di sfondo.](float-task3.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___float3
<div class="box">
  <div class="float">Float</div>
  <p>This sentence appears next to the float.</p>
</div>
```

```css hidden live-sample___float3
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
  color: #fff;
}

.box {
  background-color: rebeccapurple;
  padding: 10px;
  color: #fff;
}
```

```css live-sample___float3
.float {
  float: right;
}

.box {
}
```

{{EmbedLiveSample("float3", "", "300px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Elimina l'effetto di float al di sotto dell'elemento fluttuante aggiungendo `display: flow-root` alla classe per `.box`.
Altri metodi potrebbero essere l'uso di `overflow` o un hack clearfix, tuttavia i materiali di apprendimento descrivono il metodo `flow-root` come il modo moderno per ottenere questo risultato.

```css
.box {
  display: flow-root;
}
```

</details>

## Vedi anche

- [Nozioni di base sulla stilizzazione CSS](/it/docs/Learn_web_development/Core/Styling_basics)
