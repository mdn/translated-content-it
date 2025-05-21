---
title: "Metti alla prova le tue abilità: Posizionamento"
short-title: Positioning
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Position
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test è valutare se comprendi il [posizionamento in CSS](/it/docs/Learn_web_development/Core/CSS_layout/Positioning) utilizzando la proprietà {{CSSxRef("position")}} e i suoi valori. Lavorerai su due piccoli compiti che utilizzano diversi elementi del materiale che hai appena studiato.

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (clicca sull'icona degli appunti) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se hai difficoltà, puoi contattare noi in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, ti chiediamo di posizionare l'elemento con la classe `target` in alto a destra del contenitore, che ha il bordo grigio di 5px.

Il tuo risultato finale dovrebbe essere simile all'immagine qui sotto:

![La casella verde è in alto a destra di un contenitore con bordo grigio.](position-task1.png)

**Domanda bonus:** Riesci a fare in modo che il target sia visualizzato sotto il testo?

Prova a aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Fai clic qui per mostrare la soluzione</summary>

Questo richiede `position: relative` e `position: absolute` e comprendere come si relazionano tra loro in termini di creazione di un nuovo contesto di posizionamento da parte del `position: relative`.
Un problema comune potrebbe essere aggiungere `position: absolute` al figlio senza applicare `position: relative` al contenitore. In tal caso, il target risulterà posizionato in base alla viewport.

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

Per la domanda bonus, devi aggiungere un `z-index` negativo al target, ad esempio `z-index: -2`.

</details>

## Compito 2

In questo compito, se scorri la casella nell'esempio qui sotto, la sidebar scorre con il contenuto. Modifica il codice in modo che la sidebar (`<div class="sidebar">`) rimanga al suo posto e solo il contenuto venga scrollato.

![Il contenuto è scrollato ma la sidebar è rimasta al suo posto.](position-task2.png)

Prova a aggiornare il codice qui sotto per ricreare l'esempio finale:

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
<summary>Fai clic qui per mostrare la soluzione</summary>

Stiamo testando la tua comprensione di `position: fixed` con un esempio leggermente diverso rispetto a quelli presenti nei materiali didattici.

```css
.sidebar {
  position: fixed;
}
```

</details>

## Vedi anche

- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
