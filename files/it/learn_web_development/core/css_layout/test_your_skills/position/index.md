---
title: "Metti alla prova le tue competenze: Posizionamento"
short-title: Positioning
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Position
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprende il [posizionamento in CSS](/it/docs/Learn_web_development/Core/CSS_layout/Positioning) utilizzando la proprietà e i valori {{CSSxRef("position")}} di CSS. Lei lavorerà su due piccoli compiti che utilizzano diversi elementi del materiale appena coperto.

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice sottostanti per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (cliccando sull'icona del clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se ha difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, desideriamo che posizioni l'elemento con una classe di `target` in alto a destra del contenitore, che ha il bordo grigio di 5px.

Il suo risultato finale dovrebbe assomigliare all'immagine sottostante:

![La scatola verde è in alto a destra di un contenitore con un bordo grigio.](position-task1.png)

**Domanda bonus:** Può modificare il target per visualizzarlo sotto il testo?

Provi ad aggiornare il codice sottostante per ricreare l'esempio finale:

```html live-sample___position1
<div class="container">
  <p>
    Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
    daikon amaranth tatsoi tomatillo melon azuki bean garlic.
  </p>
  <div class="target">Target</div>
  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>
</div>
```

```css hidden live-sample___position1
body {
  font: 1.2em / 1.5 sans-serif;
}
* {
  box-sizing: border-box;
}

.container {
  padding: 0.5em;
  border: 5px solid #ccc;
}

.target {
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: #663398;
  padding: 1em;
  color: white;
}
```

```css live-sample___position1
.container {
}

.target {
}
```

{{EmbedLiveSample("position1", "", "400px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Questo richiede `position: relative` e `position: absolute` e comprende come si relazionano tra loro in termini di creazione di un nuovo contesto di posizionamento relativo.
Un problema comune potrebbe essere quello di aggiungere `position: absolute` al figlio senza applicare `position: relative` al contenitore. In quel caso, il target finirà per essere posizionato secondo la viewport.

```css
.container {
  position: relative;
}

.target {
  position: absolute;
  top: 0;
  right: 0;
}
```

Per la domanda bonus, è necessario aggiungere un `z-index` negativo al target, ad esempio `z-index: -2`.

</details>

## Compito 2

In questo compito, se scorri la casella nell'esempio sottostante, la sidebar scorre con il contenuto. Modifichi affinché la sidebar (`<div class="sidebar">`) rimanga in posizione e solo il contenuto scorra.

![Il contenuto è scorrevole, ma la sidebar è rimasta in posizione.](position-task2.png)

Provi ad aggiornare il codice sottostante per ricreare l'esempio finale:

```html live-sample___position2
<div class="container">
  <div class="sidebar">
    <p>
      This is the sidebar. It should remain in position as the content scrolls.
    </p>
  </div>
  <div class="content">
    <p>
      Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh
      onion daikon amaranth tatsoi tomatillo melon azuki bean garlic.
    </p>
    <p>
      Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
      tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
      Dandelion cucumber earthnut pea peanut soko zucchini.
    </p>
    <p>
      Turnip greens yarrow ricebean rutabaga endive cauliflower sea lettuce
      kohlrabi amaranth water spinach avocado daikon napa cabbage asparagus
      winter purslane kale. Celery potato scallion desert raisin horseradish
      spinach carrot soko. Lotus root water spinach fennel kombu maize bamboo
      shoot green bean swiss chard seakale pumpkin onion chickpea gram corn pea.
      Brussels sprout coriander water chestnut gourd swiss chard wakame kohlrabi
      beetroot carrot watercress. Corn amaranth salsify bunya nuts nori azuki
      bean chickweed potato bell pepper artichoke.
    </p>
  </div>
</div>
```

```css hidden live-sample___position2
body {
  font: 1.2em / 1.5 sans-serif;
}
* {
  box-sizing: border-box;
}

.container {
  height: 400px;
  padding: 0.5em;
  border: 5px solid #ccc;
  overflow: auto;
}

.sidebar {
  color: white;
  background-color: #663398;
  padding: 1em;
  float: left;
  width: 150px;
}

.content {
  padding: 1em;
  margin-left: 160px;
}
```

```css live-sample___position2
.container {
}

.sidebar {
}
```

{{EmbedLiveSample("position2", "", "400px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Stiamo testando la sua comprensione di `position: fixed` con un esempio leggermente diverso da quelli nei materiali didattici.

```css
.sidebar {
  position: fixed;
}
```

</details>

## Vedi anche

- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
