---
title: "Metti alla prova le tue abilità: Sfondi e bordi"
short-title: Sfondi e bordi
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Backgrounds_and_borders
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprendi [sfondi e bordi dei box in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders).

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (clicca sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, vogliamo che tu aggiunga uno sfondo, un bordo e alcuni stili di base a un'intestazione di pagina:

1. Assegna al box un bordo nero solido di 5px, con angoli arrotondati di 10px.
2. Assegna al `<h2>` un colore di sfondo nero semitrasparente e colora il testo di bianco.
3. Aggiungi un'immagine di sfondo e dimensiona in modo che copra il box. Puoi utilizzare la seguente immagine:

   ```plain
   https://mdn.github.io/shared-assets/images/examples/balloons.jpg
   ```

Il tuo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![L'immagine mostra un box con sfondo fotografico, bordo arrotondato e testo bianco su sfondo nero semitrasparente.](backgrounds-task1.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finito:

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
<summary>Fai clic qui per mostrare la soluzione</summary>

Dovresti usare `border`, `border-radius`, `background-image` e `background-size` e capire come usare i colori RGB per rendere parzialmente trasparente un colore di sfondo:

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

## Compito 2

In questo compito, vogliamo che tu aggiunga immagini di sfondo, un bordo e altri stili a un box decorativo:

1. Assegna al box un bordo azzurro di 5px e arrotonda l'angolo superiore sinistro di 20px e l'angolo inferiore destro di 40px.

2. L'intestazione utilizza l'immagine `star.png` come immagine di sfondo, con una singola stella centrata a sinistra e un pattern ripetuto di stelle a destra.
   Puoi utilizzare la seguente immagine:

   ```plain
   https://mdn.github.io/shared-assets/images/examples/star.png
   ```

3. Assicurati che il testo dell'intestazione non si sovrapponga all'immagine e che sia centrato — dovrai utilizzare tecniche apprese nelle lezioni precedenti per ottenere questo risultato.

Il tuo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![L'immagine mostra un box con un bordo blu arrotondato negli angoli in alto a sinistra e in basso a destra. A sinistra del testo c'è una singola stella, a destra 3 stelle.](backgrounds-task2.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finito:

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
<summary>Fai clic qui per mostrare la soluzione</summary>

Devi aggiungere padding all'intestazione in modo che non si sovrapponga all'immagine della stella - ciò è correlato all'apprendimento della [lezione del Modello Box](/it/docs/Learn_web_development/Core/Styling_basics/Box_model).
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
