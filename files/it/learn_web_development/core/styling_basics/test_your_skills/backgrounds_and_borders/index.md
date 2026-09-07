---
title: "Metti alla prova le tue competenze: sfondi e bordi"
short-title: "Test: sfondi e bordi"
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Backgrounds_and_borders
l10n:
  sourceCommit: 00d961466c7e388bad444f2bb1b34d5bed629686
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics")}}

Lo scopo di questo test di competenze è aiutare a valutare se si comprendono gli [sfondi e i bordi delle box in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders).

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Sfondi e bordi 1

In questa attività, si devono aggiungere uno sfondo, un bordo e alcuni stili di base all'intestazione di una pagina.

Per completare l'attività:

1. Assegnare alla box un bordo nero solido di 5px, con angoli arrotondati di 10px.
2. Assegnare a `<h2>` un colore di sfondo nero semitrasparente e rendere il testo bianco.
3. Aggiungere un'immagine di sfondo e dimensionarla in modo che copra la box. È possibile usare la seguente immagine:

   ```plain
   https://mdn.github.io/shared-assets/images/examples/balloons.jpg
   ```

Il punto di partenza dell'attività è simile al seguente:

{{EmbedLiveSample("backgrounds1-start", "", "160px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___backgrounds1-start live-sample___backgrounds1-finish
<div class="box">
  <h2>Backgrounds & Borders</h2>
</div>
```

```css live-sample___backgrounds1-start live-sample___backgrounds1-finish
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

.box {
  /* Add styles here */
}

h2 {
  /* Add styles here */
}
```

Lo stile aggiornato dovrebbe essere simile al seguente:

{{EmbedLiveSample("backgrounds1-finish", "", "160px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Si dovrebbero usare `border`, `border-radius`, `background-image` e `background-size`, e comprendere come usare i colori RGB per rendere un colore di sfondo parzialmente trasparente:

```css live-sample___backgrounds1-finish
.box {
  border: 5px solid black;
  border-radius: 10px;
  background-image: url("https://mdn.github.io/shared-assets/images/examples/balloons.jpg");
  background-size: cover;
}

h2 {
  background-color: rgb(0 0 0 / 50%);
  color: white;
}
```

</details>

## Sfondi e bordi 2

In questa attività, si devono aggiungere immagini di sfondo, un bordo e altri stili a una box decorativa.

Per completare l'attività:

1. Assegnare alla box un bordo `lightblue` di 5px e arrotondare l'angolo superiore sinistro di 20px e quello inferiore destro di 40px.
2. Il titolo usa l'immagine `star.png` come immagine di sfondo, con una singola stella centrata a sinistra e un motivo ripetuto di stelle a destra.
   È possibile usare la seguente immagine:

   ```plain
   https://mdn.github.io/shared-assets/images/examples/star.png
   ```

3. Assicurarsi che il testo del titolo non si sovrapponga all'immagine e che sia centrato: per ottenere questo risultato sarà necessario usare tecniche apprese nelle lezioni precedenti.

Il punto di partenza dell'attività è simile al seguente:

{{EmbedLiveSample("backgrounds2-start", "", "200px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___backgrounds2-start live-sample___backgrounds2-finish
<div class="box">
  <h2>Backgrounds & Borders</h2>
</div>
```

```css live-sample___backgrounds2-start live-sample___backgrounds2-finish
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

.box {
  /* Add styles here */
}

h2 {
  /* Add styles here */
}
```

Lo stile aggiornato dovrebbe essere simile al seguente:

{{EmbedLiveSample("backgrounds2-finish", "", "220px")}}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

È necessario aggiungere padding al titolo affinché non si sovrapponga all'immagine della stella: questo richiama quanto appreso nella precedente [lezione sul Box Model](/it/docs/Learn_web_development/Core/Styling_basics/Box_model).
Il testo dovrebbe essere allineato con la proprietà `text-align`:

```css live-sample___backgrounds2-finish
.box {
  border: 5px solid lightblue;
  border-top-left-radius: 20px;
  border-bottom-right-radius: 40px;
}

h2 {
  padding: 0 40px;
  text-align: center;
  background:
    url("https://mdn.github.io/shared-assets/images/examples/star.png")
      no-repeat left center,
    url("https://mdn.github.io/shared-assets/images/examples/star.png") repeat-y
      right center;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Backgrounds_and_borders", "Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics")}}
