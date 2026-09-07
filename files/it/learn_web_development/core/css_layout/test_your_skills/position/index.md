---
title: "Metti alla prova le tue competenze: Posizionamento"
short-title: "Test: Posizionamento"
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Position
l10n:
  sourceCommit: 143f7345a4276156679d816a153470fe1fc6f3f8
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout")}}

L'obiettivo di questo test di competenze è aiutare a valutare se si comprende il [posizionamento in CSS](/it/docs/Learn_web_development/Core/CSS_layout/Positioning) mediante la proprietà CSS {{CSSxRef("position")}} e i relativi valori. Verranno svolte due piccole attività che utilizzano diversi elementi del materiale appena trattato.

> [!NOTE]
> Per ottenere aiuto, leggere la guida all'uso [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Posizionamento 1

Per completare questa attività, posizionare l'elemento con la classe `target` nell'angolo in alto a destra del contenitore con un bordo grigio di `5px`.

**Domanda bonus:** è possibile modificare il target affinché venga visualizzato sotto il testo?

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("position1-start", "", "400px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___position1-start live-sample___position1-finish
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

```css live-sample___position1-start live-sample___position1-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

* {
  box-sizing: border-box;
}

.container {
  padding: 0.5em;
  border: 5px solid #cccccc;
}

.target {
  width: 150px;
  height: 150px;
  border-radius: 5px;
  background-color: #663398;
  padding: 1em;
  color: white;
}

.container {
  /* Add styles here */
}

.target {
  /* Add styles here */
}
```

Una volta completata l'attività, il posizionamento del target dovrebbe essere simile a questo:

{{EmbedLiveSample("position1-finish", "", "250px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Sono necessari `position: relative` e `position: absolute`, nonché la comprensione di come si relazionano tra loro: il posizionamento relativo crea un nuovo contesto di posizionamento.
Un possibile problema potrebbe essere l'aggiunta di `position: absolute` al figlio senza applicare `position: relative` al contenitore. In tal caso, il target finirà per essere posizionato relativamente al viewport.

```css live-sample___position1-finish
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

## Posizionamento 2

Nello stato iniziale di questa attività, se si scorre il contenuto, la barra laterale scorre insieme al contenuto. È necessario aggiornare il codice affinché la barra laterale (`<div class="sidebar">`) rimanga fissa e venga fatto scorrere solo il contenuto.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("position2-start", "", "400px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___position2-start live-sample___position2-finish
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

```css live-sample___position2-start live-sample___position2-finish
body {
  font: 1.2em / 1.5 sans-serif;
}

* {
  box-sizing: border-box;
}

.container {
  height: 400px;
  padding: 0.5em;
  border: 5px solid #cccccc;
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

.sidebar {
  /* Add styles here */
}
```

Il layout completato dovrebbe essere visualizzato in questo modo (scorrere per osservare il comportamento):

{{EmbedLiveSample("position2-finish", "", "400px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il CSS finale della barra laterale dovrebbe essere simile a questo:

```css live-sample___position2-finish
.sidebar {
  position: fixed;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Positioning", "Learn_web_development/Core/CSS_layout/Flexbox", "Learn_web_development/Core/CSS_layout")}}
