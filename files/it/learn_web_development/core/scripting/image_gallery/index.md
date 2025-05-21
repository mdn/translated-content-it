---
title: "Sfida: Galleria di immagini"
slug: Learn_web_development/Core/Scripting/Image_gallery
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Event_bubbling","Learn_web_development/Core/Scripting/Object_basics", "Learn_web_development/Core/Scripting")}}

Ora che abbiamo esaminato i blocchi fondamentali di JavaScript, metteremo alla prova la tua conoscenza di loop, funzioni, condizionali ed eventi facendoti costruire un elemento piuttosto comune che vedrai su molti siti web — una galleria di immagini alimentata da JavaScript.

## Punto di partenza

Per iniziare questa sfida, dovresti [scaricare il file ZIP](https://raw.githubusercontent.com/mdn/learning-area/main/javascript/building-blocks/gallery/gallery-start.zip) per l'esempio, decomprimerlo in una cartella sul tuo computer e fare l'esercizio localmente per cominciare.

In alternativa, potresti utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).

> [!NOTE]
> Se ti blocchi, puoi contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Indicazioni del progetto

Ti sono stati forniti alcuni file HTML, CSS e le risorse delle immagini insieme a poche righe di codice JavaScript; devi scrivere il JavaScript necessario per trasformare questo in un programma funzionante. Il corpo dell'HTML appare così:

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

![Una galleria di immagini con un'immagine grande in alto e cinque miniature sotto](gallery.png)

Le parti più interessanti del file CSS dell'esempio:

- Posiziona assolutamente tre elementi all'interno del `full-img <div>` — l'elemento `<img>` in cui viene visualizzata l'immagine a grandezza naturale, un `<div>` vuoto che ha le dimensioni dell'`<img>` e sovrapposto ad esso (questo è usato per applicare un effetto oscurante all'immagine tramite un colore di sfondo semi-trasparente), e un `<button>` usato per controllare l'effetto oscurante.
- Imposta la larghezza di tutte le immagini all'interno del `thumb-bar <div>` (immagini cosiddette "miniature") al 20% e le dispone flottanti a sinistra in modo che si allineino su una linea.

Il tuo JavaScript deve:

- Dichiarare un array `const` che elenca i nomi file di ciascuna immagine, come `'pic1.jpg'`.
- Dichiarare un oggetto `const` che elenca il testo alternativo per ciascuna immagine.
- Scorrere l'array dei nomi file e, per ciascuno di essi, inserire un elemento `<img>` all'interno del `thumb-bar <div>` che incorpora quell'immagine nella pagina insieme al suo testo alternativo.
- Aggiungere un ascoltatore di eventi di click a ciascun `<img>` all'interno del `thumb-bar <div>` in modo che, quando vengono cliccati, l'immagine e il testo alternativo corrispondenti vengano visualizzati nell'elemento `displayed-img <img>`.
- Aggiungere un ascoltatore di eventi di click al `<button>` in modo che, quando viene cliccato, venga applicato un effetto di scurimento all'immagine a grandezza naturale. Quando viene cliccato di nuovo, l'effetto di scurimento viene rimosso nuovamente.

Per darti un'idea più chiara, dai un'occhiata all'[esempio finale](https://mdn.github.io/learning-area/javascript/building-blocks/gallery/) (ma non sbirciare il codice sorgente!)

## Passi per completare

Le seguenti sezioni descrivono ciò che devi fare.

## Dichiarare un array dei nomi file delle immagini

È necessario creare un array che elenca i nomi file di tutte le immagini da includere nella galleria. L'array deve essere dichiarato come una costante.

### Scorrere le immagini

Ti abbiamo già fornito le righe che memorizzano un riferimento al `thumb-bar <div>` all'interno di una costante chiamata `thumbBar`, creano un nuovo elemento `<img>`, impostano i suoi attributi `src` e `alt` su un valore segnaposto `xxx`, e aggiungono questo nuovo elemento `<img>` all'interno di `thumbBar`.

Devi:

1. Inserire la sezione di codice sotto il commento "Scorrere le immagini" all'interno di un ciclo che attraversa tutti i nomi file nell'array.
2. In ciascuna iterazione del ciclo, sostituire i valori segnaposto `xxx` con una stringa che sarà uguale al percorso dell'immagine e agli attributi alt in ciascun caso. Impostare il valore degli attributi `src` e `alt` su questi valori in ciascun caso. Ricorda che l'immagine si trova all'interno della directory delle immagini e il suo nome è `pic1.jpg`, `pic2.jpg`, ecc.

### Aggiungere un ascoltatore di eventi di click a ciascuna immagine in miniatura

In ciascuna iterazione del ciclo, devi aggiungere un ascoltatore di eventi di click alla `newImage` corrente — questo ascoltatore dovrebbe trovare il valore dell'attributo `src` dell'immagine corrente. Imposta il valore dell'attributo `src` dell'`<img>` di `displayed-img` sul valore `src` passato come parametro. Fai poi lo stesso per l'attributo `alt`.

In alternativa, puoi aggiungere un solo ascoltatore di eventi alla barra delle miniature.

### Scrivere un gestore che esegue il pulsante scurisci/illumina

Rimane solo il nostro `<button>` scurisci/illumina — ti abbiamo già fornito una riga che memorizza un riferimento al `<button>` in una costante chiamata `btn`. È necessario aggiungere un ascoltatore di eventi di click che:

1. Controlla il nome della classe attuale impostato sul `<button>` — puoi ottenere ciò nuovamente usando `getAttribute()`.
2. Se il nome della classe è `"dark"`, cambia la classe del `<button>` in `"light"` (usando [`setAttribute()`](/it/docs/Web/API/Element/setAttribute)), il suo contenuto di testo in "Lighten" e il {{cssxref("background-color")}} dell'overlay `<div>` in `"rgb(0 0 0 / 50%)"`.
3. Se il nome della classe non è `"dark"`, cambia la classe del `<button>` in `"dark"`, il suo contenuto di testo di nuovo in "Darken" e il {{cssxref("background-color")}} dell'overlay `<div>` in `"rgb(0 0 0 / 0%)"`.

Le linee seguenti forniscono una base per ottenere le modifiche stabilite nei punti 2 e 3 di cui sopra.

```js
btn.setAttribute("class", xxx);
btn.textContent = xxx;
overlay.style.backgroundColor = xxx;
```

## Suggerimenti e consigli

- Non è necessario modificare in alcun modo l'HTML o il CSS.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Event_bubbling","Learn_web_development/Core/Scripting/Object_basics", "Learn_web_development/Core/Scripting")}}
