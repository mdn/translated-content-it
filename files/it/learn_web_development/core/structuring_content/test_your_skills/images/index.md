---
title: "Metti alla prova le tue competenze: immagini HTML"
short-title: "Test: immagini"
slug: Learn_web_development/Core/Structuring_content/Test_your_skills/Images
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content")}}

L'obiettivo di questo test sulle competenze è aiutare a valutare se si comprendono le [immagini e come incorporarle in HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images).

> [!NOTE]
> Per ricevere aiuto, leggere la guida all'uso [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Immagini 1

In questa attività, si vuole incorporare nella pagina un'immagine di alcuni mirtilli.

Per completare l'attività:

1. Aggiungere il percorso dell'immagine a un attributo appropriato per incorporarla nella pagina. L'immagine si chiama `blueberries.jpg` ed è disponibile al percorso `https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/tasks/images/blueberries.jpg?raw=true`.
2. Aggiungere del testo alternativo a un attributo appropriato per descrivere l'immagine alle persone che non possono vederla.
3. Assegnare all'elemento `<img>` un attributo `width` pari a `400` e un attributo `height` appropriato, affinché venga visualizzato con le corrette {{Glossary("aspect_ratio", "proporzioni")}} e non causi un nuovo rendering durante il caricamento. Le {{Glossary("intrinsic_size", "dimensioni intrinseche")}} dell'immagine sono 615 x 419 pixel.

Il punto di partenza dell'attività è il seguente:

{{ EmbedLiveSample('images-1', "100%", 200) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___images-1
<h1>Basic image embed</h1>

<img />

<p>You should see a picture of some blueberries above.</p>
```

<!-- Codice CSS condiviso/di configurazione -->

```css hidden live-sample___images-1 live-sample___images-2 live-sample___images-3 live-sample___images-1-finished live-sample___images-2-finished live-sample___images-3-finished
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

img {
  border: 1px solid black;
}
```

Il contenuto aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample('images-1-finished', "100%", 460) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere questo aspetto:

```html-nolint live-sample___images-1-finished
<h1>Basic image embed</h1>

<img src="https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/tasks/images/blueberries.jpg?raw=true"
     alt="blueberries" width="400" height="273" />

<p>You should see a picture of some blueberries above.</p>
```

Il valore `height` corretto è stato calcolato usando l'operazione 400 x 419/615.

</details>

## Immagini 2

In questa attività, è già presente un'immagine con tutte le funzionalità, ma si desidera aggiungere un tooltip che appaia quando il puntatore del mouse passa sopra l'immagine. Inserire nel tooltip informazioni appropriate.

Il punto di partenza dell'attività è il seguente:

{{ EmbedLiveSample('images-2', "100%", 600) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___images-2
<h1>Basic image title</h1>

<img
  src="https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/tasks/images/larch.jpg?raw=true"
  alt="Several tall evergreen trees called larches" />
```

Non viene fornito il contenuto finale per questa attività, poiché ha lo stesso aspetto del punto di partenza.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere questo aspetto:

```html-nolint live-sample___images-2-finished
<h1>Basic image title</h1>

<img
  src="https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/tasks/images/larch.jpg?raw=true"
  alt="Several tall evergreen trees called larches"
  title="And now, Number 1, The Larch" />
```

</details>

## Immagini 3

In questa attività, vengono fornite sia un'immagine con tutte le funzionalità sia del testo di didascalia. È necessario aggiungere elementi che associno l'immagine alla didascalia.

Il punto di partenza dell'attività è il seguente:

{{ EmbedLiveSample('images-3', "100%", 600) }}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___images-3
<h1>Image and caption</h1>

<img
  src="https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/tasks/images/firefox.png?raw=true"
  alt="An abstract flaming fox wrapping around a blue sphere"
  width="446"
  height="460" />
The 2019 Firefox logo
```

```css hidden live-sample___images-3 live-sample___images-3-finished
figcaption {
  font-style: italic;
}
```

Il contenuto aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample('images-3-finished', "100%", 640) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

L'HTML completato dovrebbe avere questo aspetto:

```html live-sample___images-3-finished
<h1>Image and caption</h1>

<figure>
  <img
    src="https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/tasks/images/firefox.png?raw=true"
    alt="An abstract flaming fox wrapping around a blue sphere"
    width="446"
    height="460" />
  <figcaption>The 2019 Firefox logo</figcaption>
</figure>
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_images", "Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content")}}
