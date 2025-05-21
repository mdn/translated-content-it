---
title: "Metti alla prova le tue competenze: Multicol"
short-title: Multicol
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Multicolumn
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è valutare se comprendi il [layout a più colonne in CSS](/it/docs/Learn_web_development/Core/CSS_layout/Multiple-column_Layout), incluse le proprietà e i valori {{CSSxRef("column-count")}}, {{CSSxRef("column-width")}}, {{CSSxRef("column-gap")}}, {{CSSxRef("column-span")}} e {{CSSxRef("column-rule")}}. Lavorerai attraverso tre piccoli compiti che utilizzano diversi elementi del materiale che hai appena esaminato.

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (clicca sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, vogliamo che tu crei tre colonne, con un gap di 50px tra ciascuna colonna.

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Tre colonne di testo](multicol-task1.png)

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

```html live-sample___multicol1
<div class="container">
  <p>
    Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
    daikon amaranth tatsoi tomatillo melon azuki bean garlic.
  </p>

  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>

  <p>
    Turnip greens yarrow ricebean rutabaga endive cauliflower sea lettuce
    kohlrabi amaranth water spinach avocado daikon napa cabbage asparagus winter
    purslane kale. Celery potato scallion desert raisin horseradish spinach
    carrot soko. Lotus root water spinach fennel kombu maize bamboo shoot green
    bean swiss chard seakale pumpkin onion chickpea gram corn pea.
  </p>
</div>
```

```css live-sample___multicol1
body {
  font: 1.2em / 1.5 sans-serif;
}
.container {
}
```

{{EmbedLiveSample("multicol1", "", "300px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Per questo compito, devi usare `column-count` e `column-gap`:

```css
.container {
  column-count: 3;
  column-gap: 50px;
}
```

</details>

## Compito 2

In questo compito, vogliamo che tu crei colonne che abbiano una larghezza minima di 200px. Poi, aggiungi una linea grigia di 5px tra ciascuna colonna, assicurandoti che ci siano 10px di spazio tra il bordo della linea e il contenuto della colonna.

Il tuo risultato finale dovrebbe apparire come l'immagine qui sotto:

![Tre colonne di testo con una linea grigia tra di esse.](multicol-task2.png)

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

```html live-sample___multicol2
<div class="container">
  <p>
    Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
    daikon amaranth tatsoi tomatillo melon azuki bean garlic.
  </p>

  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>

  <p>
    Turnip greens yarrow ricebean rutabaga endive cauliflower sea lettuce
    kohlrabi amaranth water spinach avocado daikon napa cabbage asparagus winter
    purslane kale. Celery potato scallion desert raisin horseradish spinach
    carrot soko. Lotus root water spinach fennel kombu maize bamboo shoot green
    bean swiss chard seakale pumpkin onion chickpea gram corn pea.
  </p>
</div>
```

```css live-sample___multicol2
body {
  font: 1.2em / 1.5 sans-serif;
}
.container {
}
```

{{EmbedLiveSample("multicol2", "", "300px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

Avrai bisogno di usare le proprietà `column-width` e `column-rule`.
Potresti usare le proprietà longhand `column-rule-*` invece dello shorthand, anche se non ci sono vantaggi nel farlo.
La cosa fondamentale nell'uso di `column-gap` è che tu abbia capito che la linea non aggiunge 5px di spazio al gap. Per avere 10px ai lati della linea, è necessario un gap di 25px dato che la linea viene sovrapposta ad esso.

```css
.container {
  column-width: 200px;
  column-rule: 5px solid #ccc;
  column-gap: 25px;
}
```

</details>

## Compito 3

In questo compito, vogliamo che tu faccia in modo che l'elemento contenente il titolo e il sottotitolo si estenda su tutte le colonne in modo che appaia come nell'immagine qui sotto:

![Tre colonne di testo con un titolo e un sottotitolo che si estendono su tutte e tre al centro.](multicol-task3.png)

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

```html live-sample___multicol3
<div class="container">
  <p>
    Veggies es bonus vobis, proinde vos postulo essum magis kohlrabi welsh onion
    daikon amaranth tatsoi tomatillo melon azuki bean garlic.
  </p>
  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>
  <div class="box">
    <h2>I am a level 2 heading</h2>
    <div class="subhead">Lotus root water spinach fennel</div>
  </div>
  <p>
    Turnip greens yarrow ricebean rutabaga endive cauliflower sea lettuce
    kohlrabi amaranth water spinach avocado daikon napa cabbage asparagus winter
    purslane kale. Celery potato scallion desert raisin horseradish spinach
    carrot soko. Lotus root water spinach fennel kombu maize bamboo shoot green
    bean swiss chard seakale pumpkin onion chickpea gram corn pea.
  </p>
</div>
```

```css hidden live-sample___multicol3
body {
  font: 1.2em / 1.5 sans-serif;
}
* {
  box-sizing: border-box;
}

.box {
  text-align: center;
  margin: 1em 0;
}

.box h2 {
  margin: 0;
  display: grid;
  grid-template-columns: 1fr auto 1fr;
  column-gap: 0.5em;
  align-items: center;
}

.box h2::before {
  content: "";
  border-bottom: 5px dotted #ccc;
}

.box h2::after {
  content: "";
  border-bottom: 5px dotted #ccc;
}

.subhead {
  font-style: italic;
}
```

```css live-sample___multicol3
.container {
  column-count: 3;
}

.box {
}

h2 {
}
```

{{EmbedLiveSample("multicol3", "", "400px")}}

<details>
<summary>Clicca qui per mostrare la soluzione</summary>

In questo compito, verifichiamo la comprensione della proprietà `column-span`.
Tutto ciò che devi fare è impostare l'elemento con una classe di `.box` su `column-span: all`.
Questo è principalmente un compito di verifica che selezioni l'elemento corretto.

```css
.box {
  column-span: all;
}
```

</details>

## Vedi anche

- [Basi dello styling CSS](/it/docs/Learn_web_development/Core/Styling_basics)
