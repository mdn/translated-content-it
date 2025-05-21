---
title: "Metti alla prova le tue abilità: Flexbox"
short-title: Flexbox
slug: Learn_web_development/Core/CSS_layout/Test_your_skills/Flexbox
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se comprendi come [flexbox e gli elementi flex](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox) si comportano. Di seguito sono riportati quattro schemi di design comuni che potresti utilizzare flexbox per creare. Il tuo compito è costruirli.

> [!NOTE]
> Clicca su **"Play"** nei blocchi di codice sottostanti per modificare gli esempi nel MDN Playground.
> Puoi anche copiare il codice (clicca sull'icona degli appunti) e incollarlo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Compito 1

In questo compito, gli elementi della lista sono la navigazione per un sito. Dovrebbero essere disposti in una riga, con una quantità uguale di spazio tra ciascun elemento.

Il tuo risultato finale dovrebbe assomigliare all'immagine sottostante:

![Elementi flex disposti come una riga con spazio tra di loro.](flex-task1.png)

Prova a aggiornare il codice sottostante per ricreare l'esempio finito:

```html live-sample___flexbox1
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About Us</a></li>
    <li><a href="/products">Our Products</a></li>
    <li><a href="/contact">Contact Us</a></li>
  </ul>
</nav>
```

```css hidden live-sample___flexbox1
body {
  font: 1.2em / 1.5 sans-serif;
}
nav ul {
  max-width: 700px;
  list-style: none;
  padding: 0;
  margin: 0;
}
nav a:link,
nav a:visited {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: #fff;
  padding: 0.5em;
  display: inline-block;
  text-decoration: none;
}
```

```css live-sample___flexbox1
nav ul {
}
```

{{EmbedLiveSample("flexbox1", "", "240px")}}

<details>
<summary>Fai clic qui per mostrare la soluzione</summary>

Puoi applicare `display: flex` e controllare lo spazio utilizzando la proprietà `justify-content`:

```css
nav ul {
  display: flex;
  justify-content: space-between;
}
```

</details>

## Compito 2

In questo compito, gli elementi della lista hanno dimensioni diverse, ma vogliamo che vengano visualizzati come tre colonne di dimensioni uguali, indipendentemente dal contenuto presente in ciascun elemento.

Il tuo risultato finale dovrebbe assomigliare all'immagine sottostante:

![Elementi flex disposti come tre colonne di uguali dimensioni con quantità diverse di contenuto.](flex-task2.png)

**Domanda bonus:** Puoi ora rendere il primo elemento grande il doppio degli altri elementi?

Prova a aggiornare il codice sottostante per ricreare l'esempio finito:

```html live-sample___flexbox2
<ul>
  <li>I am small</li>
  <li>I have more content than the very small item.</li>
  <li>
    I have lots of content. So much content that I don't know where it is all
    going to go. I'm glad that CSS is pretty good at dealing with situations
    where we end up with more words than expected!
  </li>
</ul>
```

```css hidden live-sample___flexbox2
body {
  font: 1.2em / 1.5 sans-serif;
}
ul {
  max-width: 700px;
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: #fff;
  padding: 0.5em;
}
```

```css live-sample___flexbox2
ul {
}

li {
}
```

{{EmbedLiveSample("flexbox2", "", "240px")}}

<details>
<summary>Fai clic qui per mostrare la soluzione</summary>

È meglio utilizzare gli shorthands, quindi in questo scenario `flex: 1` è probabilmente la risposta migliore, e quindi il risultato più ottimale sarebbe:

```css
ul {
  display: flex;
}

li {
  flex: 1;
}
```

Per la domanda bonus, aggiungi un selettore che si rivolge al primo elemento e imposta `flex: 2;` (o `flex: 2 0 0;` o `flex-grow: 2`):

```css
li:first-child {
  flex: 2;
}
```

</details>

## Compito 3

In questo compito, ci sono due elementi nell'HTML sottostante, un elemento `<div>` con una classe `parent` che contiene un altro elemento `<div>` con una classe `child`. Usa flexbox per centrare il bambino all'interno del genitore. Esiste più di una soluzione possibile qui.

Il tuo risultato finale dovrebbe assomigliare all'immagine sottostante:

![Una casella centrata all'interno di un'altra casella.](flex-task3.png)

Prova a aggiornare il codice sottostante per ricreare l'esempio finito:

```html live-sample___flexbox3
<div class="parent">
  <div class="child">Center me.</div>
</div>
```

```css hidden live-sample___flexbox3
body {
  font: 1.2em / 1.5 sans-serif;
}
.parent {
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  height: 200px;
}

.child {
  background-color: #4d7298;
  color: #fff;
  padding: 0.5em;
  width: 150px;
}
```

```css hidden live-sample___flexbox3
.parent {
}

.child {
}
```

{{EmbedLiveSample("flexbox3", "", "210px")}}

<details>
<summary>Fai clic qui per mostrare la soluzione</summary>

È necessario solo modificare gli stili del genitore per centrare un elemento orizzontalmente e verticalmente:

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

</details>

## Compito 4

In questo compito, vogliamo che tu disponga questi elementi in righe come nell'immagine sottostante:

![Un insieme di elementi mostrati come righe.](flex-task4.png)

Prova a aggiornare il codice sottostante per ricreare l'esempio finito:

```html live-sample___flexbox4
<ul>
  <li>Turnip</li>
  <li>greens</li>
  <li>yarrow</li>
  <li>ricebean</li>
  <li>rutabaga</li>
  <li>endive</li>
  <li>cauliflower</li>
  <li>sea lettuce</li>
  <li>kohlrabi</li>
  <li>amaranth</li>
</ul>
```

```css hidden live-sample___flexbox4
body {
  font: 1.2em / 1.5 sans-serif;
}
ul {
  width: 450px;
  list-style: none;
  padding: 0;
  margin: 0;
}

li {
  background-color: #4d7298;
  border: 2px solid #77a6b6;
  border-radius: 0.5em;
  color: #fff;
  padding: 0.5em;
  margin: 0.5em;
}
```

```css live-sample___flexbox4
ul {
}

li {
}
```

{{EmbedLiveSample("flexbox4", "", "260px")}}

<details>
<summary>Fai clic qui per mostrare la soluzione</summary>

Questo compito richiede una comprensione della proprietà `flex-wrap` per avvolgere le righe flex. Inoltre, per assicurarti di ottenere qualcosa che assomigli all'esempio, devi impostare `flex: auto` sul figlio (o `flex: 1 1 auto;`).

```css
ul {
  display: flex;
  flex-wrap: wrap;
}

li {
  flex: auto;
}
```

</details>

## Vedi anche

- [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics)
