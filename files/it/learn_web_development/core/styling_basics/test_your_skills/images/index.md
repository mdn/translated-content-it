---
title: "Metti alla prova le tue abilità: Immagini ed elementi del modulo"
short-title: Immagini e moduli
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Images
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprende come elementi speciali come [immagini, media e elementi del modulo sono trattati in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Images_media_forms).

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice sottostanti per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (cliccare sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se ha difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, ha un'immagine che si estende oltre il riquadro. Vogliamo che l'immagine si ridimensioni per adattarsi all'interno del riquadro senza alcuno spazio bianco in eccesso, ma non ci dispiace se una parte dell'immagine viene ritagliata.

Il risultato finale dovrebbe apparire come l'immagine qui sotto:

![Un'immagine in un riquadro](mdn-images-object-fit.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito in modo che l'immagine non esca dal riquadro:

```html live-sample___object-fit
<div class="box">
  <img
    alt="Hot air balloons flying in clear sky, and a crowd of people in the foreground"
    src="https://mdn.github.io/shared-assets/images/examples/balloons.jpg" />
</div>
```

```css live-sample___object-fit
.box {
  border: 5px solid #000;
  width: 400px;
  height: 200px;
}

img {
  /* Add styles here */
}
```

{{EmbedLiveSample("object-fit", "", "400px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Va bene se alcune parti dell'immagine vengono ritagliate.
Utilizzando `object-fit: cover` è la scelta migliore, deve anche impostare la larghezza e l'altezza al `100%`:

```css
img {
  height: 100%;
  width: 100%;
  object-fit: cover;
}
```

</details>

## Compito 2

In questo compito, ha un modulo base. Il suo compito è apportare le seguenti modifiche:

- Utilizzare i selettori di attributo per selezionare il campo di ricerca e il pulsante all'interno di `.my-form`.
- Rendere il campo del modulo e il pulsante utilizzino la stessa dimensione del testo del resto del modulo.
- Dare al campo del modulo e al pulsante un padding di 10px.
- Dare al pulsante uno sfondo `rebeccapurple`, primo piano bianco, nessun bordo e angoli arrotondati di 5px.

Il risultato finale dovrebbe apparire come l'immagine qui sotto:

![Un modulo su una singola linea](mdn-images-form.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___form
<form action="" class="my-form" method="post">
  <div>
    <label for="fldSearch">Keywords</label>
    <input id="fldSearch" name="keywords" type="search" />
    <input name="btnSubmit" type="submit" value="Search" />
  </div>
</form>
```

```css live-sample___form
body {
  font: 1.2em / 1.5 sans-serif;
}
.my-form {
  border: 2px solid #000;
  padding: 5px;
}
```

{{EmbedLiveSample("form")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Ecco un esempio di soluzione per il compito:

```css
.my-form {
  border: 2px solid #000;
  padding: 5px;
}

.my-form input[type="search"] {
  padding: 10px;
  font-size: inherit;
}

.my-form input[type="submit"] {
  padding: 10px;
  font-size: inherit;
  background-color: rebeccapurple;
  color: white;
  border: 0;
  border-radius: 5px;
}
```

</details>

## Vedi anche

- [Concetti base di stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
