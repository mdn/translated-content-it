---
title: "Sfida: Galleria di immagini"
slug: Learn_web_development/Core/Scripting/Image_gallery
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Event_bubbling","Learn_web_development/Core/Scripting/Object_basics", "Learn_web_development/Core/Scripting")}}

Ora che abbiamo esaminato i blocchi fondamentali di JavaScript, testeremo la sua conoscenza di cicli, funzioni, condizionali ed eventi facendo costruire un elemento abbastanza comune che vede in molti siti web — una galleria di immagini alimentata da JavaScript.

## Punto di partenza

Per iniziare questa sfida, dovrebbe [scaricare lo ZIP](https://raw.githubusercontent.com/mdn/learning-area/main/javascript/building-blocks/gallery/gallery-start.zip) del progetto, estrarlo da qualche parte sul suo computer, e fare l'esercizio localmente per iniziare.

In alternativa, potrebbe utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).

> [!NOTE]
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Sintesi del progetto

Le sono stati forniti alcuni asset HTML, CSS e immagini e poche righe di codice JavaScript; è necessario scrivere il JavaScript necessario per trasformarlo in un programma funzionante. Il corpo HTML appare così:

```html
<h1>Image gallery example</h1>

<div class="full-img">
  <img
    class="displayed-img"
    src="images/pic1.jpg"
    alt="Closeup of a blue human eye" />
  <div class="overlay"></div>
  <button class="dark">Darken</button>
</div>

<div class="thumb-bar"></div>
```

L'esempio appare così:

![Una galleria di immagini con una grande immagine in alto e cinque miniature sotto](gallery.png)

Le parti più interessanti del file CSS dell'esempio:

- Posiziona in modo assoluto i tre elementi all'interno del `full-img <div>` — l`'<img>` in cui viene visualizzata l'immagine a dimensione intera, un `<div>` vuoto che è dimensionato per essere della stessa dimensione dell`'<img>` e posizionato direttamente sopra di esso (questo è usato per applicare un effetto di oscuramento all'immagine tramite un colore di sfondo semi-trasparente), e un `<button>` che è utilizzato per controllare l'effetto di oscuramento.
- Imposta la larghezza di qualsiasi immagine all'interno del `thumb-bar <div>` (cosiddette immagini "miniatura") al 20% e le allinea a sinistra in modo che appaiano una accanto all'altra su una linea.

Il suo JavaScript deve:

- Dichiarare un array `const` con i nomi file di ciascuna immagine, come `'pic1.jpg'`.
- Dichiarare un oggetto `const` che elenchi il testo alternativo per ciascuna immagine.
- Scorrere l'array di nomi file, e per ciascuno, inserire un elemento `<img>` all'interno del `thumb-bar <div>` che incorpori quell'immagine nella pagina insieme al suo testo alternativo.
- Aggiungere un ascoltatore di eventi click a ciascun `<img>` all'interno del `thumb-bar <div>` in modo che, quando vengono cliccati, l'immagine e il testo alternativo corrispondenti siano visualizzati nell'elemento `displayed-img <img>`.
- Aggiungere un ascoltatore di eventi click al `<button>` in modo che, quando viene cliccato, si applichi un effetto di oscuramento all'immagine a dimensione intera. Quando viene cliccato di nuovo, l'effetto di oscuramento viene rimosso.

Per darle un'idea maggiore, dia un'occhiata all'[esempio finito](https://mdn.github.io/learning-area/javascript/building-blocks/gallery/) (non sbirci nel codice sorgente!)

## Passi da completare

Le sezioni seguenti descrivono cosa deve fare.

## Dichiarare un array di nomi di file di immagini

Deve creare un array che elenchi i nomi file di tutte le immagini da includere nella galleria. L'array deve essere dichiarato come una costante.

### Scorrendo le immagini

Le abbiamo già fornito le righe che memorizzano un riferimento al `thumb-bar <div>` dentro una costante chiamata `thumbBar`, crea un nuovo elemento `<img>`, imposta i suoi attributi `src` e `alt` su un valore segnaposto `xxx`, e aggiunge questo nuovo elemento `<img>` dentro `thumbBar`.

Deve:

1. Mettere il blocco di codice sotto il commento "Scorrendo le immagini" dentro un ciclo che scorre tutti i nomi file nell'array.
2. In ogni iterazione del ciclo, sostituire i valori segnaposto `xxx` con una stringa che sarà uguale al percorso per l'immagine e gli attributi alt in ogni caso. Impostare il valore degli attributi `src` e `alt` su questi valori in ogni caso. Ricordi che l'immagine si trova nella directory delle immagini, e il suo nome è `pic1.jpg`, `pic2.jpg`, ecc.

### Aggiungere un ascoltatore di eventi click a ogni immagine in miniatura

In ciascuna iterazione del ciclo, deve aggiungere un ascoltatore di eventi click all'immagine `newImage` corrente — questo ascoltatore dovrebbe trovare il valore dell'attributo `src` dell'immagine corrente. Impostare il valore dell'attributo `src` dell'`<img>` `displayed-img` al valore `src` passato come parametro. Poi fare lo stesso per l'attributo `alt`.

In alternativa, può aggiungere un ascoltatore di eventi alla barra delle miniature.

### Scrivere un gestore che esegua il bottone per oscurare/illuminare

Rimane solo il nostro `<button>` oscurare/illuminare — abbiamo già fornito una riga che memorizza un riferimento al `<button>` in una costante chiamata `btn`. Deve aggiungere un ascoltatore di eventi click che:

1. Verifichi il nome della classe attualmente impostata sul `<button>` — può ottenere ciò nuovamente utilizzando `getAttribute()`.
2. Se il nome della classe è `"dark"`, cambia la classe del `<button>` in `"light"` (utilizzando [`setAttribute()`](/it/docs/Web/API/Element/setAttribute)), il suo contenuto di testo in "Lighten", e il {{cssxref("background-color")}} dell`<div>` overlay in `"rgb(0 0 0 / 50%)"`.
3. Se il nome della classe non è `"dark"`, cambia la classe del `<button>` in `"dark"`, il suo contenuto di testo indietro a "Darken", e il {{cssxref("background-color")}} dell`<div>` overlay in `"rgb(0 0 0 / 0%)"`.

Le righe seguenti forniscono una base per ottenere i cambiamenti stipulati nei punti 2 e 3 sopra.

```js
btn.setAttribute("class", xxx);
btn.textContent = xxx;
overlay.style.backgroundColor = xxx;
```

## Suggerimenti e consigli

- Non è necessario modificare in alcun modo l'HTML o il CSS.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Event_bubbling","Learn_web_development/Core/Scripting/Object_basics", "Learn_web_development/Core/Scripting")}}
