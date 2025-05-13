---
title: "Metta alla prova le sue abilità: Sfondi e bordi"
short-title: Sfondi e bordi
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Backgrounds_and_borders
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se lei comprende [sfondi e bordi delle box in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders).

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (cliccando sull'icona degli appunti) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Attività 1

In questa attività, vogliamo che lei aggiunga uno sfondo, un bordo e alcuni stili di base a un'intestazione di pagina:

1. Dia alla box un bordo nero solido di 5px, con angoli arrotondati di 10px.
2. Dia al `<h2>` un colore di sfondo nero semitrasparente e renda il testo bianco.
3. Aggiunga un'immagine di sfondo e la dimensioni in modo che copra la box. Può usare la seguente immagine:

   ```plain
   https://mdn.github.io/shared-assets/images/examples/balloons.jpg
   ```

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![L'immagine mostra una box con uno sfondo fotografico, un bordo arrotondato e testo bianco su uno sfondo nero semi-trasparente.](backgrounds-task1.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___backgrounds1
<div class="box">
  <h2>Backgrounds & Borders</h2>
</div>
```

```css hidden live-sample___backgrounds1
body {
  padding: 1em;
  font: 1.2em / 1.5 sans-serif;
}
* {
  box-sizing: border-box;
}
.box {
  padding: 0.5em;
}
```

```css live-sample___backgrounds1
.box {
  /* Add styles here */
}

h2 {
  /* Add styles here */
}
```

{{EmbedLiveSample("backgrounds1", "", "200px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Dovrebbe usare `border`, `border-radius`, `background-image` e `background-size` e comprendere come utilizzare i colori RGB per rendere un colore di sfondo parzialmente trasparente:

```css
.box {
  border: 5px solid #000;
  border-radius: 10px;
  background-image: url(https://mdn.github.io/shared-assets/images/examples/balloons.jpg);
  background-size: cover;
}

h2 {
  background-color: rgb(0 0 0 / 50%);
  color: #fff;
}
```

</details>

## Attività 2

In questa attività, vogliamo che lei aggiunga immagini di sfondo, un bordo e alcuni altri stili a una box decorativa:

1. Dia alla box un bordo azzurro di 5px e arrotondi l'angolo superiore sinistro di 20px e quello inferiore destro di 40px.

2. L'intestazione usa l'immagine `star.png` come immagine di sfondo, con una singola stella centrata a sinistra e un motivo ripetuto di stelle a destra.
   Può usare la seguente immagine:

   ```plain
   https://mdn.github.io/shared-assets/images/examples/star.png
   ```

3. Si assicuri che il testo dell'intestazione non si sovrapponga all'immagine e che sia centrato — dovrà usare tecniche apprese nelle lezioni precedenti per ottenere questo risultato.

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![L'immagine mostra una box con un bordo blu arrotondato agli angoli superiore sinistro e inferiore destro. A sinistra del testo c'è una singola stella, a destra 3 stelle.](backgrounds-task2.png)

Provi ad aggiornare il codice qui sotto per ricreare l'esempio finito:

```html live-sample___backgrounds2
<div class="box">
  <h2>Backgrounds & Borders</h2>
</div>
```

```css hidden live-sample___backgrounds2
body {
  padding: 1em;
  font: 1.2em / 1.5 sans-serif;
}
* {
  box-sizing: border-box;
}
.box {
  width: 300px;
  padding: 0.5em;
}
```

```css live-sample___backgrounds2
.box {
  /* Add styles here */
}

h2 {
  /* Add styles here */
}
```

{{EmbedLiveSample("backgrounds2", "", "220px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

È necessario aggiungere padding all'intestazione in modo che non si sovrapponga all'immagine della stella - questo collega all'apprendimento dalla precedente [lezione sul Modello Box](/it/docs/Learn_web_development/Core/Styling_basics/Box_model).
Il testo dovrebbe essere allineato con la proprietà `text-align`:

```css
.box {
  border: 5px solid lightblue;
  border-top-left-radius: 20px;
  border-bottom-right-radius: 40px;
}

h2 {
  padding: 0 40px;
  text-align: center;
  background:
    url(https://mdn.github.io/shared-assets/images/examples/star.png) no-repeat
      left center,
    url(https://mdn.github.io/shared-assets/images/examples/star.png) repeat-y
      right center;
}
```

</details>

## Vedi anche

- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
