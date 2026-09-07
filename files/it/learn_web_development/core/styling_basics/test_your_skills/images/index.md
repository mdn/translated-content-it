---
title: "Metti alla prova le tue competenze: immagini ed elementi dei moduli"
short-title: "Test: immagini e moduli"
slug: Learn_web_development/Core/Styling_basics/Test_your_skills/Images
l10n:
  sourceCommit: a623d4459e2aa00d17dc0fd6b6bc44f56c589950
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics")}}

Lo scopo di questo test delle competenze è valutare se si comprende come gli elementi speciali, quali [immagini, media ed elementi dei moduli, vengono gestiti in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Images_media_forms).

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Immagini e moduli 1

In questa attività è presente un'immagine che fuoriesce dal riquadro. Ridimensionare l'immagine in modo che rientri nel riquadro senza spazio bianco aggiuntivo; non importa se una parte dell'immagine viene ritagliata. Aggiornare il CSS per ottenere questo risultato.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("images-forms1-start", "", "260px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___images-forms1-start live-sample___images-forms1-finish
<div class="box">
  <img
    alt="Hot air balloons flying in clear sky, and a crowd of people in the foreground"
    src="https://mdn.github.io/shared-assets/images/examples/balloons.jpg" />
</div>
```

```css live-sample___images-forms1-start live-sample___images-forms1-finish
.box {
  border: 5px solid black;
  width: 400px;
  height: 200px;
}

img {
  /* Add styles here */
}
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("images-forms1-finish", "", "260px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Va bene se alcune parti dell'immagine vengono ritagliate.
L'uso di `object-fit: cover` è la scelta migliore; è inoltre necessario impostare larghezza e altezza su `100%`:

```css live-sample___images-forms1-finish
img {
  height: 100%;
  width: 100%;
  object-fit: cover;
}
```

</details>

## Immagini e moduli 2

In questa attività è presente un modulo di base.

Per completare l'attività:

1. Usare i selettori di attributo per selezionare il campo di ricerca e il pulsante all'interno di `.my-form`.
2. Fare in modo che il campo del modulo e il pulsante utilizzino la stessa dimensione del testo del resto del modulo.
3. Assegnare al campo del modulo e al pulsante un `padding` di `10px`.
4. Assegnare al pulsante uno sfondo `rebeccapurple`, un primo piano bianco, nessun bordo e angoli arrotondati di 5px.

Il punto di partenza dell'attività è il seguente:

{{EmbedLiveSample("images-forms2-start", "", "80px")}}

Ecco il codice sottostante per questo punto di partenza:

```html live-sample___images-forms2-start live-sample___images-forms2-finish
<form action="" class="my-form" method="post">
  <div>
    <label for="fldSearch">Keywords</label>
    <input id="fldSearch" name="keywords" type="search" />
    <input name="btnSubmit" type="submit" value="Search" />
  </div>
</form>
```

```css live-sample___images-forms2-start live-sample___images-forms2-finish
body {
  font: 1.2em / 1.5 sans-serif;
}
.my-form {
  border: 2px solid black;
  padding: 5px;
}
```

Lo stile aggiornato dovrebbe apparire così:

{{EmbedLiveSample("images-forms2-finish", "", "80px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Ecco un esempio di soluzione per l'attività:

```css live-sample___images-forms2-finish
.my-form {
  border: 2px solid black;
  padding: 5px;
}

.my-form input[type="search"] {
  padding: 10px;
  font-size: inherit;
}

.my-form input[type="submit"] {
  padding: 10px;
  font-size: inherit;
  background-color: rebeccapurple;
  color: white;
  border: 0;
  border-radius: 5px;
}
```

</details>

## Immagini e moduli 3

La soluzione per questa verifica è abbastanza libera e offre molta flessibilità su ciò che è possibile fare. Per questo motivo non viene fornito un esempio di rendering.

Il CSS deve includere quanto segue:

1. Un leggero "reset" per rendere inizialmente più coerenti font, `padding`, margini e dimensionamento, come descritto in [Normalizzazione del comportamento dei moduli](/it/docs/Learn_web_development/Core/Styling_basics/Images_media_forms#normalizing_form_behavior).
2. Uno stile gradevole e coerente per gli input e il pulsante.
3. Una tecnica di layout per allineare ordinatamente input ed etichette.

Il punto di partenza dell'attività è il seguente:

{{ EmbedLiveSample("forms-2", "100%", 250) }}

Ecco il codice sottostante per questo punto di partenza:

```html hidden live-sample___forms-2
<form>
  <h2>Edit your preferences</h2>
  <ul>
    <li>
      <label for="email">Email:</label>
      <input type="email" id="email" name="email" />
    </li>
    <li>
      <label for="website">Website:</label>
      <input type="url" id="website" name="website" />
    </li>
    <li>
      <label for="phone">Phone number:</label>
      <input type="tel" id="phone" name="phone" />
    </li>
    <li>
      <label for="food">Favorite food:</label>
      <select name="food" id="food">
        <option>Salad</option>
        <option>Curry</option>
        <option>Pizza</option>
        <option>Fajitas</option>
      </select>
    </li>
    <li>
      <button>Update preferences</button>
    </li>
  </ul>
</form>
```

```css live-sample___forms-2
* {
  box-sizing: border-box;
}

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
  width: 500px;
}

/* Don't edit the code above here! */

/* Add your code here */
```

Non è stato fornito il contenuto finale per questa attività, poiché sono possibili molte soluzioni valide.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il CSS completato potrebbe avere un aspetto simile al seguente:

```css
/* ... */
/* Don't edit the code above here! */

button,
input,
select {
  font-family: inherit;
  font-size: 100%;
  padding: 0;
  margin: 0;
}

li {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

li:last-of-type {
  margin-top: 30px;
}

label {
  flex: 0 40%;
  text-align: right;
  padding-right: 10px;
}

input,
select {
  flex: auto;
  height: 2em;
}

input,
select,
button {
  display: block;
  padding: 5px 10px;
  border: 1px solid #cccccc;
  border-radius: 3px;
}

select {
  padding: 5px;
}

button {
  margin: 0 auto;
  padding: 5px 20px;
  line-height: 1.5;
  background: #eeeeee;
}

button:hover,
button:focus {
  background: #dddddd;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Images_media_forms", "Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics")}}
