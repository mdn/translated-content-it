---
title: "Metti alla prova le tue competenze: accessibilità CSS e JavaScript"
short-title: "Test: a11y CSS/JS"
slug: Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript
l10n:
  sourceCommit: 2bda943b59604eb44f5d759708845c5f56970635
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/WAI-ARIA_basics", "Learn_web_development/Core/Accessibility")}}

Lo scopo di questo test delle competenze è aiutare a valutare se sono state comprese le nostre [buone pratiche di accessibilità CSS e JavaScript](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript).

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida sull'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Accessibilità CSS 1

Nel primo compito viene presentato un elenco di link. Tuttavia, la loro accessibilità è piuttosto scarsa: non c'è alcun modo per capire realmente che sono link, né per capire su quale di essi è attivo il focus dell'utente. Si supponga che il ruleset esistente con il selettore `a` sia fornito da un CMS e che non possa essere modificato.

Per completare il compito, creare nuove regole affinché i link abbiano l'aspetto e il comportamento di link e l'utente possa capire quale link dell'elenco ha il focus.

<!-- Codice condiviso tra gli esempi -->

```css hidden live-sample___css-js-ally-1 live-sample___css-js-ally-2 live-sample___css-js-ally-3 live-sample___css-js-ally-1-finish live-sample___css-js-ally-2-finish
body {
  background-color: white;
  color: #333333;
  font:
    1em / 1.4 "Helvetica Neue",
    "Helvetica",
    "Arial",
    sans-serif;
  padding: 1em;
  margin: 0;
}

* {
  box-sizing: border-box;
}
```

<!-- Codice specifico dell'esempio -->

Il punto di partenza del compito è simile al seguente:

{{ EmbedLiveSample("css-js-ally-1", "100%", 200) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___css-js-ally-1 live-sample___css-js-ally-1-finish
<ul>
  <li><a href="">Animals</a></li>
  <li><a href="">Computers</a></li>
  <li><a href="">Diversity and inclusion</a></li>
  <li><a href="">Food</a></li>
  <li><a href="">Medicine</a></li>
  <li><a href="">Music</a></li>
</ul>
```

```css live-sample___css-js-ally-1
a {
  text-decoration: none;
  color: #666666;
  outline: none;
}

/* Don't edit the above code! */

/* Add your code here */
```

Una volta completato il compito, i link dovrebbero avere un aspetto simile al seguente:

{{ EmbedLiveSample("css-js-ally-1-finish", "100%", 200) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il CSS completato potrebbe essere simile al seguente:

```css
/* ... */
/* Don't edit the above code! */

li a {
  text-decoration: underline;
  color: rgb(150 0 0);
}

li a:hover,
li a:focus {
  text-decoration: none;
  color: red;
}
```

```css hidden live-sample___css-js-ally-1-finish
a {
  text-decoration: none;
  color: #666666;
  outline: none;
}

li a {
  text-decoration: underline;
  color: rgb(150 0 0);
}

li a:hover,
li a:focus {
  text-decoration: none;
  color: red;
}
```

</details>

## Accessibilità CSS 2

In questo compito successivo viene presentato un semplice contenuto, composto soltanto da intestazioni e paragrafi. Sono presenti problemi di accessibilità relativi ai colori e alle dimensioni del testo, che devono essere corretti.

Per completare il compito:

1. Riflettere sui problemi e sulle linee guida che indicano i valori accettabili per colore e dimensionamento.
2. Aggiornare il CSS con nuovi valori per `color` e `font-size` per risolvere il problema.
3. Testare il codice per assicurarsi che il problema sia stato risolto. Spiegare quali strumenti o metodi sono stati usati per selezionare i nuovi valori e testare il codice.

Il punto di partenza del compito è simile al seguente:

{{ EmbedLiveSample("css-js-ally-2", "100%", 240) }}

Ecco il codice sottostante per questo punto di partenza:

<!-- spellchecker: disable -->

```html live-sample___css-js-ally-2 live-sample___css-js-ally-2-finish
<main>
  <h1>I am the eggman</h1>

  <p>
    Prow scuttle parrel provost Sail ho shrouds spirits boom mizzenmast yardarm.
    Pinnace holystone mizzenmast quarter crow's nest nipperkin grog yardarm
    hempen halter furl.
  </p>

  <h2>They are the eggman</h2>

  <p>
    Swab barque interloper chantey doubloon starboard grog black jack gangway
    rutters.
  </p>

  <h2>I am the walrus</h2>

  <p>
    Deadlights jack lad schooner scallywag dance the hempen jig carouser
    broadside cable strike colors.
  </p>
</main>
```

<!-- spellchecker: enable -->

```css live-sample___css-js-ally-2
/* Edit the CSS to fix the a11y problems */

main {
  padding: 20px;
  background-color: red;
}

h1,
h2,
p {
  color: #999999;
}

h1 {
  font-size: 2vw;
}

h2 {
  font-size: 1.5vw;
}

p {
  font-size: 1.2vw;
}
```

Il contenuto aggiornato dovrebbe avere un aspetto simile al seguente:

{{ EmbedLiveSample("css-js-ally-2-finish", "100%", 600) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

1. I problemi sono:
   - Il contrasto di colore non è accettabile, secondo i criteri WCAG [1.4.3 (AA)](https://w3c.github.io/wcag/guidelines/22/#contrast-minimum) e [1.4.6 (AAA)](https://w3c.github.io/wcag/guidelines/22/#contrast-enhanced).
   - Il testo è dimensionato usando unità `vw`, il che significa che nella maggior parte dei browser non è possibile ingrandirlo. [WCAG 1.4.4 (AA)](https://w3c.github.io/wcag/guidelines/22/#resize-text) afferma che il testo dovrebbe essere ridimensionabile.
2. Per correggere il codice, è necessario:
   - Scegliere un insieme di colori di sfondo e primo piano con un contrasto migliore.
   - Usare unità diverse per dimensionare il testo, come `rem` o anche `px`, oppure implementare qualcosa che utilizzi una combinazione di `vw` e altre unità, se si desidera che sia ridimensionabile ma ancora relativo alla dimensione del viewport.
3. Per il test:
   - È possibile testare il contrasto di colore usando uno strumento come [aXe](https://www.deque.com/axe/), l'[Accessibility Inspector di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/) o anche un semplice strumento su una pagina web autonoma come [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/).
   - Per il ridimensionamento del testo, è necessario caricare l'esempio in un browser e provare a ridimensionarlo. Il ridimensionamento di testo dimensionato con unità `vw` funziona in Safari, ma non in Firefox o nei browser basati su Chromium.

Per il codice aggiornato, qualcosa di simile risolverebbe il contrasto di colore:

```css live-sample___css-js-ally-2-finish
main {
  padding: 20px;
  background-color: red;
}

h1,
h2,
p {
  color: black;
}
```

E qualcosa di simile funzionerebbe per il dimensionamento del font:

```css live-sample___css-js-ally-2-finish
h1 {
  font-size: 2.5rem;
}

h2 {
  font-size: 2rem;
}

p {
  font-size: 1.2rem;
}
```

Oppure questo, se si desidera fare qualcosa di più sofisticato che fornisca testo ridimensionabile relativo al viewport:

```css
h1 {
  font-size: calc(1.5vw + 1rem);
}

h2 {
  font-size: calc(1.2vw + 0.7rem);
}

p {
  font-size: calc(1vw + 0.4rem);
}
```

</details>

## Accessibilità JavaScript 1

Nel nostro compito finale sull'accessibilità, occorre scrivere del JavaScript. È disponibile un'app che presenta un elenco di nomi di animali. Facendo clic su uno dei nomi degli animali, viene visualizzata un'ulteriore descrizione di quell'animale in una casella sotto l'elenco.

Tuttavia, non è molto accessibile: nel suo stato attuale è possibile utilizzarla soltanto con il mouse. Occorre aggiungere HTML e JavaScript per renderla accessibile anche tramite tastiera.

Il punto di partenza del compito è simile al seguente:

{{ EmbedLiveSample("css-js-ally-3", "100%", 400) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___css-js-ally-3
<section class="preview">
  <div class="animal-list">
    <h1>Animal summaries</h1>

    <p>
      The following list of animals can be clicked to display a description of
      that animal.
    </p>

    <ul>
      <li
        data-description="A type of wild mountain goat, with large recurved horns, found in Eurasia, North Africa, and East Africa.">
        Ibex
      </li>
      <li
        data-description="A medium-sized marine mammal, similar to a manatee, but with a Dolphin-like tail.">
        Dugong
      </li>
      <li
        data-description="A rare marsupial, which looks rather like a tiny kangaroo, measuring around 50 to 75 centimeters.">
        Quokka
      </li>
    </ul>
  </div>

  <div class="animal-description">
    <h2></h2>

    <p></p>
  </div>
</section>
```

```css hidden live-sample___css-js-ally-3
p {
  color: purple;
  margin: 0.5em 0;
}

li {
  cursor: pointer;
}
```

```js live-sample___css-js-ally-3
const listItems = document.querySelectorAll("li");
const descHeading = document.querySelector(".animal-description h2");
const descPara = document.querySelector(".animal-description p");

listItems.forEach((item) => {
  item.addEventListener("mouseup", handleSelection);
});

function handleSelection(e) {
  const heading = e.target.textContent;
  const description = e.target.getAttribute("data-description");
  descHeading.textContent = heading;
  descPara.textContent = description;
}
```

Non abbiamo fornito contenuto completato per questo compito, poiché ha lo stesso aspetto del punto di partenza.

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

1. Per iniziare, sarà necessario aggiungere `tabindex="0"` agli elementi dell'elenco per renderli selezionabili tramite tastiera.
2. Quindi sarà necessario aggiungere un altro event listener all'interno del ciclo `forEach()`, affinché il codice risponda alla pressione dei tasti mentre gli elementi dell'elenco sono selezionati. Probabilmente è una buona idea farlo rispondere a un tasto specifico, ad esempio "Enter"; in questo caso, qualcosa di simile al seguente è probabilmente accettabile:

```js
item.addEventListener("keyup", (e) => {
  if (e.key === "Enter") {
    handleSelection(e);
  }
});
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/CSS_and_JavaScript","Learn_web_development/Core/Accessibility/WAI-ARIA_basics", "Learn_web_development/Core/Accessibility")}}
