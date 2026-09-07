---
title: "Sfida: Galleria di immagini"
slug: Learn_web_development/Core/Scripting/Image_gallery
l10n:
  sourceCommit: 50a1895c9c499b1b9207f7af945a0fe45de58cca
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/DOM_scripting","Learn_web_development/Core/Scripting/Network_requests", "Learn_web_development/Core/Scripting")}}

In questa sfida, verrà richiesto di creare un elemento piuttosto comune che si trova in molti siti web: una galleria di immagini basata su JavaScript. Durante il percorso, verranno verificate le conoscenze su cicli, funzioni, condizionali, eventi, scripting DOM e basi degli oggetti.

## Punto di partenza

Per iniziare, fare clic sul pulsante **Play** in uno dei pannelli di codice qui sotto per aprire l'esempio fornito nel Playground MDN. Seguire quindi le istruzioni nella sezione [Descrizione del progetto](#descrizione_del_progetto) per completare la funzionalità JavaScript.

L'HTML è il seguente:

```html live-sample___gallery-start live-sample___gallery-finish
<h1>Image gallery example</h1>

<div class="full-img">
  <img
    class="displayed-img"
    src="https://mdn.github.io/shared-assets/images/examples/learn/gallery/pic1.jpg"
    alt="Closeup of a human eye" />
  <div class="overlay"></div>
  <button class="dark">Darken</button>
</div>

<div class="thumb-bar"></div>
```

Il JavaScript iniziale è il seguente:

```js live-sample___gallery-start
const displayedImage = document.querySelector(".displayed-img");
const thumbBar = document.querySelector(".thumb-bar");

const btn = document.querySelector("button");
const overlay = document.querySelector(".overlay");
```

{{EmbedLiveSample("gallery-start", "100%", 700)}}

```css hidden live-sample___gallery-start live-sample___gallery-finish
body {
  font-family: sans-serif;
  width: 640px;
  margin: 0 auto;
  background-color: lightgray;
}

h1 {
  text-align: center;
}

.full-img {
  position: relative;
  display: block;
  width: 640px;
  height: 480px;
  margin-bottom: 2px;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 640px;
  height: 480px;
}

button {
  background: rgb(150 150 150 / 0.6);
  border: 1px solid #999999;
  position: absolute;
  cursor: pointer;
  top: 2px;
  left: 2px;
}

button:hover,
button:focus {
  color: rgb(150 150 150 / 1);
  background-color: black;
}

.thumb-bar {
  display: flex;
  gap: 2px;
  cursor: pointer;
}

.thumb-bar img {
  display: block;
  width: 100px;
  flex: 1;
}

.thumb-bar img:hover,
.thumb-bar img:focus {
  outline: 2px solid blue;
}
```

Il CSS della galleria è stato nascosto per brevità, ma è visibile osservando l'app nel Playground MDN.

## Descrizione del progetto

Sono stati forniti HTML, CSS e alcune righe di codice JavaScript. Il compito consiste nel seguire le istruzioni riportate di seguito, scrivendo il JavaScript necessario per trasformare questo codice in una galleria di immagini funzionante.

La galleria sarà composta da un'immagine grande e una riga di miniature. Quando si fa clic su una miniatura oppure la si raggiunge tramite tastiera e si preme <kbd>Enter</kbd>/<kbd>Return</kbd>, tale miniatura dovrebbe essere visualizzata come immagine grande. Anche l'elemento `<img>` pertinente dovrebbe essere aggiornato con il testo `alt` corretto.

Nell'angolo in alto a sinistra è presente un pulsante che, se premuto ripetutamente, alterna l'immagine grande tra una tonalità più scura e una più chiara, ottenuta modificando la trasparenza di un elemento `<div>` sovrapposto all'immagine grande.

Le immagini da incorporare nell'esempio e il relativo testo `alt` richiesto sono le seguenti:

- [`pic1.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/gallery/pic1.jpg): "Primo piano di un occhio umano".
- [`pic2.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/gallery/pic2.jpg): "Roccia che sembra un'onda".
- [`pic3.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/gallery/pic3.jpg): "Viole del pensiero viola e bianche".
- [`pic4.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/gallery/pic4.jpg): "Sezione di parete proveniente dalla tomba di un faraone".
- [`pic5.jpg`](https://mdn.github.io/shared-assets/images/examples/learn/gallery/pic5.jpg): "Grande falena su una foglia".

### Creare un oggetto dati

Prima di tutto, dichiarare un array di oggetti denominato `images`. Ogni oggetto deve contenere due proprietà:

- `filename`: il nome del file dell'immagine (non l'URL completo).
- `alt`: il testo `alt` dell'immagine.

### Aggiungere le immagini alla barra delle miniature

Successivamente, eseguire un ciclo su `images` e usare lo scripting DOM per incorporarle tutte nella pagina tramite elementi `<img>`. Dovrebbero essere incluse come elementi figli dell'elemento `<div>` con classe `thumb-bar`, al quale è già stato assegnato un riferimento nella costante `thumbBar`.

1. Creare una costante denominata `baseURL` contenente l'URL di base di ciascun file immagine (tutto l'URL escluso il nome file).
2. Creare un ciclo `for ... of` per scorrere `images`.
3. Per ogni immagine, creare un nuovo elemento `<img>`.
4. Impostare la sorgente di `<img>` in modo che corrisponda all'URL dell'immagine, che dovrebbe essere una combinazione di `baseURL` e `filename`, e l'attributo `alt` in modo che corrisponda al testo `alt`.
5. Aggiungere un altro attributo a `<img>` per renderlo attivabile tramite tastiera.
6. Aggiungere `<img>` a `thumbBar`.
7. Aggiungere un gestore dell'evento `click` a `<img>` affinché, quando si fa clic su di esso, venga eseguita una funzione denominata `updateDisplayedImage()`, che visualizza l'immagine selezionata a dimensione intera. Questa funzione verrà creata più avanti.
8. Aggiungere un altro gestore di eventi a `<img>` affinché, una volta raggiunto tramite tastiera, l'immagine selezionata possa essere visualizzata a dimensione intera premendo il tasto <kbd>Enter</kbd>/<kbd>Return</kbd> (e nessun altro tasto). Questo è un obiettivo aggiuntivo che richiederà qualche ricerca per capire come realizzarlo.

### Creare la funzione `updateDisplayedImage()`

Ora è il momento di creare la funzione per visualizzare a dimensione intera una miniatura attivata. È stato memorizzato un riferimento all'elemento `<img>` a dimensione intera nella costante `displayedImage`.

1. Definire la funzione `updateDisplayedImage()`.
2. All'interno del corpo della funzione, impostare la sorgente di `displayedImage` in modo che corrisponda alla sorgente dell'elemento `<img>` attivato.
3. Impostare il testo `alt` di `displayedImage` in modo che corrisponda al testo `alt` dell'elemento `<img>` attivato.

### Collegare il pulsante Darken/Lighten

È stato memorizzato un riferimento al `<button>` "Darken/Lighten" nella costante `btn` e un riferimento al `<div>` trasparente sovrapposto all'elemento `<img>` a dimensione intera nella costante `overlay`. È necessario:

1. Aggiungere un gestore dell'evento `click` a `<button>` con una funzione anonima impostata come funzione gestore.
2. All'interno del corpo della funzione, aggiungere una struttura condizionale che verifichi se `<button>` ha impostata una `class` pari a `dark` oppure no.
3. Se `<button>` ha una `class` `dark` quando viene selezionato, modificare il suo contenuto testuale in `Lighten` e modificare il colore di sfondo dell'elemento `overlay` in `rgb(0 0 0 / 0.5)`. Rimuovere la classe `dark` dell'elemento `<button>`.
4. Se `<button>` _non_ ha una `class` `dark` quando viene selezionato, modificare il suo contenuto testuale in `Darken` e modificare il colore di sfondo dell'elemento `overlay` in `rgb(0 0 0 / 0)`. Aggiungere la classe `dark` dell'elemento `<button>`.
5. Si riesce a pensare a un modo per alternare la classe `dark` usando una sola riga di codice, eseguita dopo la struttura condizionale? Anche questo è un obiettivo aggiuntivo, ma vale la pena provarci.

## Suggerimenti e consigli

- Non è necessario modificare HTML o CSS.

## Esempio

L'app completata dovrebbe funzionare come il seguente esempio live:

{{EmbedLiveSample("gallery-finish", "100%", 700)}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile al seguente:

```js live-sample___gallery-finish
const displayedImage = document.querySelector(".displayed-img");
const thumbBar = document.querySelector(".thumb-bar");

const btn = document.querySelector("button");
const overlay = document.querySelector(".overlay");

// Solution: Create a data object

const images = [
  { filename: "pic1.jpg", alt: "Closeup of a human eye" },
  { filename: "pic2.jpg", alt: "Rock that looks like a wave" },
  { filename: "pic3.jpg", alt: "Purple and white pansies" },
  { filename: "pic4.jpg", alt: "Section of wall from a pharaoh's tomb" },
  { filename: "pic5.jpg", alt: "Large moth on a leaf" },
];

// Solution: Loop through the images

// Create a baseURL constant containing the baseURL of the images
const baseURL =
  "https://mdn.github.io/shared-assets/images/examples/learn/gallery/";

// Loop through the images using a for...of loop
for (const image of images) {
  // Create a new image element
  const newImage = document.createElement("img");
  // Set the source and alt text for the image
  newImage.src = `${baseURL}${image.filename}`;
  newImage.alt = image.alt;
  // Make the image focusable via the keyboard
  newImage.tabIndex = "0";
  // Append the image as a child of the thumbBar
  thumbBar.appendChild(newImage);
  // Update the display to show the image full size when a thumb is clicked
  newImage.addEventListener("click", updateDisplayedImage);
  // Update the display to show the image full size when the "Enter" key
  // is pressed after it has been focused
  newImage.addEventListener("keydown", (e) => {
    if (e.code === "Enter") {
      updateDisplayedImage(e);
    }
  });
}

// Solution: Create the updateDisplayedImage() function

function updateDisplayedImage(e) {
  displayedImage.src = e.target.src;
  displayedImage.alt = e.target.alt;
}

// Solution: Wire up the Darken/Lighten button

// Add a click event listener on the button
btn.addEventListener("click", () => {
  // If the button has a "dark" class set,
  // change text to "Lighten" and make the overlay darker
  if (btn.classList.contains("dark")) {
    btn.textContent = "Lighten";
    overlay.style.backgroundColor = "rgb(0 0 0 / 0.5)";
  } else {
    // Else, change text to "Darken" and make
    // the overlay lighter
    btn.textContent = "Darken";
    overlay.style.backgroundColor = "rgb(0 0 0 / 0)";
  }
  // Toggle the class ready for the next button press
  btn.classList.toggle("dark");
});
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/DOM_scripting","Learn_web_development/Core/Scripting/Network_requests", "Learn_web_development/Core/Scripting")}}
