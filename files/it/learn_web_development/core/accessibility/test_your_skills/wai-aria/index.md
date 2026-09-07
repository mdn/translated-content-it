---
title: "Metti alla prova le tue competenze: WAI-ARIA"
short-title: "Test: WAI-ARIA"
slug: Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA
l10n:
  sourceCommit: 2bda943b59604eb44f5d759708845c5f56970635
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/WAI-ARIA_basics","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}

Lo scopo di questo test delle competenze è aiutare a valutare se l'articolo sulle [basi di WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics) è stato compreso.

> [!NOTE]
> Per ottenere aiuto, leggere la guida all'uso [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci utilizzando uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## WAI-ARIA 1

La prima attività ARIA include una sezione di markup non semantico, che visivamente dovrebbe rappresentare un elenco. Supponendo che non sia possibile modificare gli elementi utilizzati, come si può consentire agli utenti di screen reader di comprenderne il significato?

Per completare l'attività, aggiungere alcune semantiche WAI-ARIA affinché gli screen reader riconoscano gli elementi `<div>` come un elenco non ordinato.

Il punto di partenza dell'attività è simile a questo:

{{ EmbedLiveSample("aria-1", "100%", 250) }}

Ecco il codice sottostante per questo punto di partenza:

<!-- Codice condiviso tra gli esempi -->

```css hidden live-sample___aria-1 live-sample___aria-2 live-sample___aria-3
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

```html live-sample___aria-1
<p>My favorite animals:</p>

<div>
  <div>Pig</div>
  <div>Gazelle</div>
  <div>Llama</div>
  <div>Majestic moose</div>
  <div>Hedgehog</div>
</div>
```

```css live-sample___aria-1
div > div {
  padding-left: 20px;
  position: relative;
}

div > div::before {
  content: " ";
  width: 8px;
  height: 8px;
  background-color: black;
  border-radius: 50%;
  position: absolute;
  left: 0;
  top: 8px;
}
```

Non è stato fornito il contenuto completato per questa attività, poiché ha lo stesso aspetto del punto di partenza.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile a questo:

```html
<p>My favorite animals:</p>

<div role="list">
  <div role="listitem">Pig</div>
  <div role="listitem">Gazelle</div>
  <div role="listitem">Llama</div>
  <div role="listitem">Majestic moose</div>
  <div role="listitem">Hedgehog</div>
</div>
```

</details>

## WAI-ARIA 2

Nella seconda attività WAI-ARIA viene presentato un modulo di ricerca di base e viene richiesto di aggiungere un paio di funzionalità WAI-ARIA per migliorarne l'accessibilità.

Per completare l'attività:

1. Aggiungere un attributo per consentire agli screen reader di identificare il modulo di ricerca come landmark separato nella pagina, in modo che sia facilmente individuabile.
2. Fornire all'input di ricerca un'etichetta adatta, senza aggiungere esplicitamente un'etichetta di testo visibile al DOM.

Il punto di partenza dell'attività è simile a questo:

{{ EmbedLiveSample("aria-2", "100%", 100) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___aria-2
<form>
  <input type="search" name="search" />
</form>
```

Non è stato fornito il contenuto completato per questa attività, poiché non appare significativamente diverso dallo stato iniziale.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere un aspetto simile a questo:

```html
<form role="search">
  <input
    type="search"
    name="search"
    aria-label="Search for your favorite content on our site" />
</form>
```

</details>

## WAI-ARIA 3

Per quest'ultima attività WAI-ARIA, si torna a un esempio già visto nel [test delle competenze su CSS e JavaScript](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript).
Come in precedenza, è presente un'app che mostra un elenco di nomi di animali. Facendo clic su uno dei nomi degli animali, viene visualizzata un'ulteriore descrizione dell'animale in un riquadro sotto l'elenco. Qui si parte da una versione accessibile tramite mouse e tastiera.

Il problema ora è che, quando il DOM cambia per mostrare una nuova descrizione, gli screen reader non possono rilevare cosa sia cambiato. È possibile aggiornarla in modo che le modifiche alle descrizioni vengano annunciate dallo screen reader?

Il punto di partenza dell'attività è simile a questo:

{{ EmbedLiveSample("aria-3", "100%", 400) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___aria-3
<section class="preview">
  <div class="animal-list">
    <h1>Animal summaries</h1>

    <p>
      The following list of animals can be clicked to display a description of
      that animal.
    </p>

    <ul>
      <li
        tabindex="0"
        data-description="A type of wild mountain goat, with large recurved horns, found in Eurasia, North Africa, and East Africa.">
        Ibex
      </li>
      <li
        tabindex="0"
        data-description="A medium-sized marine mammal, similar to a manatee, but with a Dolphin-like tail.">
        Dugong
      </li>
      <li
        tabindex="0"
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

```css hidden live-sample___aria-3
p {
  color: purple;
  margin: 0.5em 0;
}

* {
  box-sizing: border-box;
}

li {
  cursor: pointer;
}
```

```js hidden live-sample___aria-3
const listItems = document.querySelectorAll("li");
const descHeading = document.querySelector(".animal-description h2");
const descPara = document.querySelector(".animal-description p");

listItems.forEach((item) => {
  item.addEventListener("mouseup", handleSelection);
  item.addEventListener("keyup", (e) => {
    if (e.key === "Enter") {
      handleSelection(e);
    }
  });
});

function handleSelection(e) {
  const heading = e.target.textContent;
  const description = e.target.getAttribute("data-description");
  descHeading.textContent = heading;
  descPara.textContent = description;
}
```

Non è stato fornito il contenuto completato per questa attività, poiché ha lo stesso aspetto del punto di partenza.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Esistono due modi per risolvere il problema descritto in questa attività:

- Aggiungere un attributo `aria-live=""` al `<div>` della descrizione dell'animale per trasformarlo in una live region, in modo che, quando il suo contenuto cambia, il contenuto aggiornato venga letto da uno screen reader. Il valore migliore è probabilmente `assertive`, che fa leggere allo screen reader il contenuto aggiornato non appena cambia. `polite` significa che lo screen reader attenderà che le altre descrizioni siano state lette prima di iniziare a leggere il contenuto modificato.
- Aggiungere un attributo `role="alert"` al `<div>` della descrizione dell'animale, per attribuirgli la semantica di una casella di avviso. Questo produce sullo screen reader lo stesso effetto dell'impostazione di `aria-live="assertive"`.

</details>

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/WAI-ARIA_basics","Learn_web_development/Core/Accessibility/Multimedia", "Learn_web_development/Core/Accessibility")}}
