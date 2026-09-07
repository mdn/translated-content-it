---
title: "Sfida: applicare stili a un'app di ricerca di schemi cromatici per la casa"
short-title: "Sfida: applicare stili alla ricerca di schemi cromatici"
slug: Learn_web_development/Core/Styling_basics/Home_color_scheme_search
l10n:
  sourceCommit: 58e3af416f96bd922b27d6a805e9a699cac389b9
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics")}}

La sfida finale del nostro modulo sulle [basi dello styling](/it/docs/Learn_web_development/Core/Styling_basics) presenta un mockup dell'interfaccia utente di una "app di ricerca dei colori per la casa", la cui idea è consentire agli utenti di inserire un colore e recuperare una serie di variazioni insieme a esempi di idee per schemi cromatici. Il compito consiste nell'applicare stili ai controlli di form, tabella e pulsante forniti e assicurarsi che le immagini vengano visualizzate come previsto.

> [!NOTE]
> Le immagini colorate usate in questa sfida sono state adattate dall'originale disponibile su Flickr: [Chic Living Room](https://flickr.com/photos/145464578@N08/28362250492/), pubblicato da [Houseology Interiors](https://flickr.com/photos/145464578@N08/) con licenza [CC BY-NC 2.0](https://creativecommons.org/licenses/by-nc/2.0/deed.en).

## Punto di partenza

Per iniziare, fare clic sul pulsante **Play** in uno dei pannelli di codice seguenti per aprire l'esempio fornito nel Playground MDN. Seguire quindi le istruzioni nella sezione [Descrizione del progetto](#descrizione_del_progetto) per applicare gli stili appropriati alla pagina.

```html live-sample___app-start live-sample___app-finish
<section>
  <h1>Home color search</h1>
  <form>
    <div>
      <label for="color">Color to search for:</label>
      <input type="text" id="color" name="color" value="pink" />
    </div>
    <div>
      <label for="results-per-page">Results per page:</label>
      <input
        type="text"
        id="results-per-page"
        name="results-per-page"
        value="4" />
    </div>
    <div>
      <button type="button">Submit</button>
    </div>
  </form>
</section>
<hr />
<section>
  <h2>Search results</h2>
  <table>
    <caption>
      Sample colors and color schemes
    </caption>
    <thead>
      <tr>
        <th scope="col">Color</th>
        <th scope="col">Raw color</th>
        <th scope="col">Tags</th>
        <th scope="col">Sample color scheme</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Pink</td>
        <td><code>rgb(255 192 203)</code></td>
        <td>pink, pale, light</td>
        <td>
          <img
            src="https://mdn.github.io/shared-assets/images/examples/learn/home-color-schemes/living-room-pink.png"
            alt="Image of living room in a pink color scheme" />
        </td>
      </tr>
      <tr>
        <td>Baker-Miller pink</td>
        <td><code>rgb(255 145 175)</code></td>
        <td>pink, pale, bright</td>
        <td>
          <img
            src="https://mdn.github.io/shared-assets/images/examples/learn/home-color-schemes/living-room-baker-miller-pink.png"
            alt="Image of living room in a Baker-Miller pink color scheme" />
        </td>
      </tr>
      <tr>
        <td>Hotpink</td>
        <td><code>rgb(255 105 180)</code></td>
        <td>pink, bright, vivid</td>
        <td>
          <img
            src="https://mdn.github.io/shared-assets/images/examples/learn/home-color-schemes/living-room-hotpink.png"
            alt="Image of living room in a hotpink color scheme" />
        </td>
      </tr>
      <tr>
        <td>Fuchsia</td>
        <td><code>rgb(255 0 255)</code></td>
        <td>pink, medium, bright</td>
        <td>
          <img
            src="https://mdn.github.io/shared-assets/images/examples/learn/home-color-schemes/living-room-fuchsia.png"
            alt="Image of living room in a fuchsia color scheme" />
        </td>
      </tr>
    </tbody>
  </table>
  <div class="controls">
    <button disabled>Previous</button>
    <p>Showing page 1 of 20</p>
    <button>Next</button>
  </div>
</section>
```

```css live-sample___app-start
* {
  box-sizing: border-box;
}

html {
  font-family: "Helvetica", "Arial", sans-serif;
}

body {
  margin: 0 10px;
}

hr {
  margin: 3em 0;
}

h2 {
  margin-top: 0;
}

/* Prev/next control layout */

.controls {
  display: flex;
  padding: 10px 0;
  justify-content: space-between;
  align-items: center;
}

/* Form and button styling */

form div {
  display: flex;
  align-items: center;
  gap: 2em;
  margin-bottom: 1em;
}

label {
  text-align: right;
  flex: 1;
}

input {
  flex: 3;
}

/* Table styling */

table img {
  width: 100%;
  height: 150px;
}
```

{{embedlivesample("app-start", "100%", 650)}}

## Descrizione del progetto

Seguire i passaggi seguenti per completare il progetto, dimensionando in modo appropriato il riquadro dei contenuti e aggiungendo le decorazioni richieste.

### Aggiungere un reset del form

Prima di tutto, aggiungere alcuni stili di "reset" agli elementi `<button>` e `<input>` per fornire loro uno stato iniziale coerente tra i vari browser.

Nello specifico:

1. Fare in modo che ereditino la famiglia di font impostata per il resto della pagina.
2. Assegnare loro una dimensione del font pari a `100%`.
3. Rimuovere tutto il padding e il margin.

### Applicare stili agli input del form

Assegnare agli elementi `<input>`:

1. Un bordo solido di `2px` con colore `#999999`.
2. `10px` di padding.
3. Angoli arrotondati di `5px`.

### Applicare stili ai pulsanti

Assegnare agli elementi `<button>`:

1. Nessun bordo.
2. Un colore di sfondo `black` e un colore del testo `white`.
3. Angoli arrotondati di `5px`.
4. Padding verticale di `10px` e padding orizzontale di `2em`.
5. Un colore di sfondo `#666666` al passaggio del puntatore o quando ricevono il focus.
6. Un colore di sfondo `#aaaaaa` quando sono disabilitati.

### Applicare stili alla tabella

A questo punto, aggiungere alla tabella alcuni stili di buona pratica, appresi in precedenza nel modulo, oltre ad alcune aggiunte.

Nello specifico:

1. Assegnare alla tabella un layout fisso, una larghezza di `100%` e bordi collassati.
2. Rendere i bordi superiore e inferiore della tabella spessi `1px`, solidi e di colore `#999999`.
3. Assegnare alle celle di intestazione e alle celle normali della tabella `0.6em` di padding e allineare verticalmente il loro contenuto alla parte superiore delle celle.
4. Assegnare alle celle di intestazione della tabella un bordo inferiore spesso `1px`, solido e di colore `#999999`.
5. Assegnare a tutte le colonne della tabella una larghezza del `20%`, tranne alla quarta colonna, che deve avere una larghezza del `40%`.
6. All'interno del corpo della tabella sono presenti quattro righe. La seconda cella all'interno di ciascuna di queste righe contiene il testo relativo a un colore `rgb()`. Assegnare a ciascuna di queste celle un colore di sfondo corrispondente al proprio testo.
7. Creare strisce zebrate: assegnare a ogni riga dispari un colore di sfondo `#eeeeee`, solo all'interno del corpo della tabella.
8. Assegnare alla didascalia un padding di `1em`, uno stile del font in corsivo e una spaziatura tra le lettere di `1px`.

### Correggere la visualizzazione delle immagini

A questo punto, si presenta un problema con le immagini nella tabella: ciascuna immagine è stata impostata al `100%` della larghezza del contenitore della sua cella di tabella e a un'altezza specifica di `150px`, poiché non si voleva che le righe della tabella diventassero troppo alte. Tuttavia, questo ha distorto le proporzioni delle immagini, facendole apparire un po' schiacciate.

Applicare stili alle immagini in modo che:

1. Vengano visualizzate con le proporzioni intrinseche, ma con una piccola porzione dell'immagine ritagliata, così da rientrare comunque nelle dimensioni degli elementi `<img>`.
2. Venga mostrata la parte inferiore dell'immagine, mentre quella superiore venga ritagliata.

## Suggerimenti

- Non è necessario modificare l'HTML in alcun modo.

## Esempio

Il progetto completato dovrebbe avere questo aspetto:

{{EmbedLiveSample("app-finish", "100%", 700)}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Una possibile soluzione potrebbe essere:

```css live-sample___app-finish
* {
  box-sizing: border-box;
}

html {
  font-family: "Helvetica", "Arial", sans-serif;
}

body {
  margin: 0 10px;
}

hr {
  margin: 3em 0;
}

h2 {
  margin-top: 0;
}

/* Prev/next control layout */

.controls {
  display: flex;
  padding: 10px 0;
  justify-content: space-between;
  align-items: center;
}

/* Form and button styling */

form div {
  display: flex;
  align-items: center;
  gap: 2em;
  margin-bottom: 1em;
}

label {
  text-align: right;
  flex: 1;
}

/* Solution: Add a form reset */

button,
input {
  font-family: inherit;
  font-size: 100%;
  padding: 0;
  margin: 0;
}

input {
  flex: 3;
  /* Solution: Style the form inputs */
  border: 2px solid #999999;
  padding: 10px;
  border-radius: 5px;
}

/* Solution: Style the buttons */

button {
  background-color: black;
  border: none;
  color: white;
  border-radius: 5px;
  padding: 10px 2em;
}

button:hover,
button:focus {
  background-color: #666666;
}

button:disabled {
  background-color: #aaaaaa;
}

/* Table styling */

table img {
  width: 100%;
  height: 150px;
  /* Solution: Fixing the image display */
  object-fit: cover;
  object-position: bottom;
}

/* Solution: Style the table */

table {
  table-layout: fixed;
  width: 100%;
  border-collapse: collapse;
  border-top: 1px solid #999999;
  border-bottom: 1px solid #999999;
}

th,
td {
  vertical-align: top;
  padding: 0.6em;
}

th {
  border-bottom: 1px solid #999999;
}

/* There's no need to specify the width of the other columns
   explicitly: The 4th column has a width of 40% set, and
   the remaining columns will get the remaining 60% equally
   distributed between them (20% each) */
tr :nth-of-type(4) {
  width: 40%;
}

/* Solution: Provide background colors for the "Raw color" cells */

tr:nth-of-type(1) td:nth-of-type(2) {
  background-color: pink;
}

tr:nth-of-type(2) td:nth-of-type(2) {
  background-color: rgb(255 145 175);
}

tr:nth-of-type(3) td:nth-of-type(2) {
  background-color: hotpink;
}

tr:nth-of-type(4) td:nth-of-type(2) {
  background-color: magenta;
}

tbody tr:nth-child(odd) {
  background-color: #eeeeee;
}

caption {
  padding: 1em;
  font-style: italic;
  letter-spacing: 1px;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics/Debugging_CSS", "Learn_web_development/Core/Styling_basics")}}
