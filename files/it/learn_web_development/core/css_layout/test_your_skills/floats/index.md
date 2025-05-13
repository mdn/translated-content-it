---
title: "Metti alla prova le tue abilità: Float"
short-title: Floats
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Floats
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprende l'uso dei [float in CSS](/it/docs/Learn_web_development/Core/CSS_layout/Floats) utilizzando le proprietà e i valori di {{CSSxRef("float")}} e {{CSSxRef("clear")}}, così come altri metodi per chiarire i float. Lei lavorerà su tre piccoli compiti che utilizzano diversi elementi del materiale che ha appena coperto.

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (faccia clic sull'icona degli appunti) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, deve fare in modo che i due elementi con una classe di `float1` e `float2` fluttuino a sinistra e a destra, rispettivamente. Il testo dovrebbe poi apparire tra le due caselle, come nell'immagine sotto:

![Due blocchi visualizzati a sinistra e a destra di un testo.](float-task1.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

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
<summary>Fare clic qui per mostrare la soluzione</summary>

Può usare `float` per entrambe le caselle:

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

In questo compito, l'elemento con una classe di `float` dovrebbe essere fatto fluttuare a sinistra. Poi vogliamo che la prima riga di testo si visualizzi accanto a quell'elemento, ma la riga di testo successiva (che ha una classe di `below`) sia visualizzata sotto di esso.

Il suo risultato finale dovrebbe avere l'aspetto dell'immagine qui sotto:

![Una casella visualizzata a sinistra di una linea di testo, con un altro testo sotto.](float-task2.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

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
<summary>Fare clic qui per mostrare la soluzione</summary>

Deve far fluttuare l'elemento a sinistra, poi aggiungere `clear: left` alla classe per il secondo paragrafo:

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

In questo compito, abbiamo un elemento fluttuante. La casella che avvolge il float e il testo viene visualizzata dietro al float. Utilizzi il metodo più aggiornato disponibile per far sì che lo sfondo della casella estenda sotto il float, come nell'immagine sotto:

![Un blocco visualizzato a destra di un testo entrambi avvolti da una casella con un colore di sfondo.](float-task3.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

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
<summary>Fare clic qui per mostrare la soluzione</summary>

Pulire la casella sotto l'elemento fluttuante aggiungendo `display: flow-root` alla classe per `.box`.
Altri metodi potrebbero essere utilizzare `overflow` o un hack clearfix, tuttavia i materiali didattici dettagliato il metodo `flow-root` come il modo moderno per raggiungere questo obiettivo.

```css
.box {
  display: flow-root;
}
```

</details>

## Vedi anche

- [Nozioni di base sulla stilizzazione CSS](/it/docs/Learn_web_development/Core/Styling_basics)
