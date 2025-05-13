---
title: "Metti alla prova le tue competenze: Il modello a riquadri"
short-title: Modello a riquadri
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di competenza è valutare se comprende il [modello a riquadri CSS](/it/docs/Learn_web_development/Core/Styling_basics/Box_model).

> [!NOTE]
> Clicchi su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Può anche copiare il codice (cliccando sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se ha difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Esercizio 1

In questo esercizio, ci sono due riquadri qui sotto: uno utilizza il modello a riquadri standard, l'altro il modello a riquadri alternativo. Modifichi la larghezza del secondo riquadro aggiungendo dichiarazioni alla classe `.alternate`, in modo che corrisponda alla larghezza visiva del primo riquadro.

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![Due riquadri della stessa dimensione](mdn-box-model1.png)

Cerchi di aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___box-models
<div class="box">I use the standard box model.</div>
<div class="box alternate">I use the alternate box model.</div>
```

```css live-sample___box-models
body {
  font: 1.2em / 1.5 sans-serif;
}
.box {
  border: 5px solid rebeccapurple;
  background-color: lightgray;
  padding: 40px;
  margin: 40px;
  width: 300px;
  height: 150px;
}

.alternate {
  box-sizing: border-box;
}
```

{{EmbedLiveSample("box-models", "", "540px")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Dovrà aumentare la larghezza del secondo blocco per aggiungere la dimensione del padding e del bordo:

```css
.alternate {
  box-sizing: border-box;
  width: 390px;
}
```

</details>

## Esercizio 2

In questo esercizio, aggiunga le seguenti cose al riquadro:

- Un bordo puntinato nero da 5px.
- Un margine superiore di 20px.
- Un margine destro di 1em.
- Un margine inferiore di 40px.
- Un margine sinistro di 2em.
- Padding su tutti i lati di 1em.

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![Un riquadro con un bordo puntinato](mdn-box-model2.png)

Cerchi di aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___mbp
<div class="box">I use the standard box model.</div>
```

```css live-sample___mbp
body {
  font: 1.2em / 1.5 sans-serif;
}

.box {
}
```

{{EmbedLiveSample("mbp")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Questo esercizio richiede l'uso corretto delle proprietà di margine, bordo e padding.
Potrebbe scegliere di utilizzare le proprietà per singolo lato ({{cssxref("margin-top")}}, {{cssxref("margin-right")}}, ecc.), tuttavia, quando si imposta margine e padding su tutti i lati, l'uso dello shorthand è probabilmente la scelta migliore:

```css
.box {
  border: 5px dotted black;
  margin: 20px 1em 40px 2em;
  padding: 1em;
}
```

</details>

## Esercizio 3

In questo esercizio, l'elemento inline ha un margine, un padding e un bordo. Tuttavia, le linee sopra e sotto lo stanno sovrapponendo. Cosa può aggiungere al suo CSS per far sì che la dimensione del margine, del padding e del bordo venga rispettata dalle altre linee, mantenendo comunque l'elemento inline?

Il suo risultato finale dovrebbe assomigliare all'immagine qui sotto:

![Un riquadro inline con spazio tra esso e il testo circostante.](mdn-box-model3.png)

Cerchi di aggiornare il codice qui sotto per ricreare l'esempio finale:

```html live-sample___inline-block
<div class="box">
  <p>
    Veggies es bonus vobis, <span>proinde vos postulo</span> essum magis
    kohlrabi welsh onion daikon amaranth tatsoi tomatillo melon azuki bean
    garlic.
  </p>

  <p>
    Gumbo beet greens corn soko endive gumbo gourd. Parsley shallot courgette
    tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato.
    Dandelion cucumber earthnut pea peanut soko zucchini.
  </p>
</div>
```

```css live-sample___inline-block
body {
  font: 1.2em / 1.5 sans-serif;
}

.box span {
  background-color: pink;
  border: 5px solid black;
  padding: 1em;
}
```

{{EmbedLiveSample("inline-block")}}

<details>
<summary>Clicchi qui per mostrare la soluzione</summary>

Risolvere questo esercizio richiede che comprenda quando utilizzare diversi valori di {{cssxref("display")}}.
Dopo aver aggiunto `display: inline-block`, il margine, il bordo e il padding nella direzione del blocco faranno sì che le altre linee vengano spostate via dall'elemento:

```css
.box span {
  background-color: pink;
  border: 5px solid black;
  padding: 1em;
  display: inline-block;
}
```

</details>

## Vedi anche

- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
