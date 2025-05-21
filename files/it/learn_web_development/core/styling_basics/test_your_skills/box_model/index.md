---
title: "Metti alla prova le tue competenze: Il modello a scatola"
short-title: Modello a scatola
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Box_model
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è verificare se hai compreso il [modello a scatola CSS](/it/docs/Learn_web_development/Core/Styling_basics/Box_model).

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice qui sotto per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (cliccando sull'icona della clipboard) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se incontri difficoltà, puoi contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Attività 1

In questa attività, ci sono due scatole qui sotto, una usa il modello a scatola standard, l'altra il modello a scatola alternativo. Modifica la larghezza della seconda scatola aggiungendo dichiarazioni alla classe `.alternate`, in modo che corrisponda alla larghezza visiva della prima scatola.

Il tuo risultato finale dovrebbe assomigliare all'immagine sotto:

![Due scatole della stessa dimensione](mdn-box-model1.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finito:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Dovrai aumentare la larghezza del secondo blocco, per aggiungere la dimensione del padding e del bordo:

```css
.alternate {
  box-sizing: border-box;
  width: 390px;
}
```

</details>

## Attività 2

In questa attività, aggiungi le seguenti cose alla scatola:

- Un bordo tratteggiato nero di 5px.
- Un margine superiore di 20px.
- Un margine destro di 1em.
- Un margine inferiore di 40px.
- Un margine sinistro di 2em.
- Padding su tutti i lati di 1em.

Il tuo risultato finale dovrebbe assomigliare all'immagine sotto:

![Una scatola con un bordo tratteggiato](mdn-box-model2.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finito:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Questa attività richiede l'uso corretto delle proprietà margin, border e padding.
Potresti scegliere di utilizzare le proprietà estese ({{cssxref("margin-top")}}, {{cssxref("margin-right")}}, ecc.), tuttavia quando imposti un margine e un padding su tutti i lati, la forma abbreviata è probabilmente la scelta migliore:

```css
.box {
  border: 5px dotted black;
  margin: 20px 1em 40px 2em;
  padding: 1em;
}
```

</details>

## Attività 3

In questa attività, l'elemento inline ha un margine, padding e bordo. Tuttavia, le righe sopra e sotto lo stanno sovrapponendo. Cosa puoi aggiungere al tuo CSS per fare in modo che la dimensione del margine, padding e bordo venga rispettata dalle altre righe, mantenendo comunque l'elemento inline?

Il tuo risultato finale dovrebbe assomigliare all'immagine sotto:

![Una scatola inline con spazio tra sé e il testo circostante.](mdn-box-model3.png)

Prova ad aggiornare il codice qui sotto per ricreare l'esempio finito:

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
<summary>Clicca qui per mostrare la soluzione</summary>

Risolvere questa attività richiede di capire quando utilizzare diversi valori di {{cssxref("display")}}.
Dopo aver aggiunto `display: inline-block`, il margine, il bordo e il padding in direzione blocco provocheranno l'allontanamento delle altre righe dall'elemento:

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

- [Basi di styling CSS](/it/docs/Learn_web_development/Core/Styling_basics)
