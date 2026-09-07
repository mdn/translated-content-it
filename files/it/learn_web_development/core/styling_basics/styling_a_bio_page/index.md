---
title: "Sfida: applicare stili a una pagina biografica"
short-title: "Sfida: pagina biografica"
slug: Learn_web_development/Core/Styling_basics/Styling_a_bio_page
l10n:
  sourceCommit: 9381ac06accc1f6340cda5c90cec69cc66f67136
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics")}}

In questa sfida verranno applicati stili a una semplice pagina biografica, mettendo alla prova alcune delle competenze apprese nelle ultime lezioni, tra cui la scrittura di selettori, la colorazione degli sfondi e lo stile del testo. Verrà inoltre proposto di cercare alcune funzionalità CSS di base non ancora trattate, per mettere alla prova le capacità di ricerca.

## Punto di partenza

Per iniziare, fare clic sul pulsante **Play** in uno dei pannelli di codice seguenti per aprire l'esempio fornito nell'MDN Playground. Quindi, seguire le istruzioni nelle sezioni successive per applicare alla pagina gli stili appropriati.

```html live-sample___style-bio-start live-sample___style-bio-finish
<h1>Jane Doe</h1>
<div class="job-title">Web Developer</div>
<p>
  Far far away, behind the word mountains, far from the countries Vokalia and
  Consonantia, there live the blind texts. Separated they live in Bookmarksgrove
  right at the coast of the Semantics, a large language ocean.
</p>

<p>
  A small river named Duden flows by their place and supplies it with the
  necessary regelialia. It is a paradisematic country, in which roasted parts of
  sentences fly into your mouth.
</p>

<h2>Contact information</h2>
<ul>
  <li>Email: <a href="mailto:jane@example.com">jane@example.com</a></li>
  <li>Web: <a href="http://example.com">http://example.com</a></li>
  <li>Tel: <a href="tel:12345678">123 45678</a></li>
</ul>
```

```css live-sample___style-bio-start
html {
  background-color: white;
}

body {
  font: 1.2em / 1.5 system-ui;
}
```

{{EmbedLiveSample("style-bio-start", "100%", "400px")}}

## Descrizione del progetto

Seguire le istruzioni seguenti per applicare gli stili alla biografia. Provare a cercare le funzionalità CSS necessarie nel [riferimento CSS di MDN](/it/docs/Web/CSS/Reference).

### Stili dei riquadri

1. Assegnare all'elemento `<body>` un padding di `20px` su tutti i lati e una larghezza di `500px`.
2. Assegnare all'elemento `<body>` un colore di sfondo pari a `#efefef` (un valore {{cssxref("&lt;hex-color>")}} grigio chiaro).
3. Centrare l'elemento `<body>` all'interno della viewport impostando i margini superiore e inferiore a `0` e i margini sinistro e destro ad `auto`.
4. Assegnare all'elemento `<ul>` usato per i dettagli di contatto un colore di sfondo `white` e un bordo viola solido di 5px su tutti i lati. Assegnare a `<ul>` un padding di `30px` su tutti i lati per allontanare il contenuto dal bordo.
5. Assegnare a `<ul>` un raggio del bordo di `20px`.

### Stili del testo

1. Rendere l'intestazione di livello uno grigio scuro, usando la parola chiave di colore CSS `darkslategray`, e assegnare all'intestazione un bordo inferiore puntinato di `10px`, usando la parola chiave di colore CSS `purple`.
2. Rendere l'intestazione di livello due in corsivo.
3. Assegnare all'intestazione di livello uno una dimensione del carattere di `2rem` e all'intestazione di livello due una dimensione del carattere di `1.5rem`.
4. Selezionare il `<div>` usando un selettore di classe e assegnargli il colore `darkslategray` e un peso del carattere grassetto.
5. Rendere i link `green`.
6. Rendere i link `darkgreen` quando vi si passa sopra con il puntatore del mouse o quando ricevono il focus tramite tastiera (sarà necessario usare un paio di {{cssxref("pseudo-classes")}}).
7. Fare in modo che i link perdano la sottolineatura quando si passa sopra di essi o ricevono il focus.

## Suggerimenti e consigli

- Usare il [W3C CSS Validator](https://jigsaw.w3.org/css-validator/) per individuare errori involontari nel CSS, errori che altrimenti potrebbero passare inosservati, in modo da poterli correggere.
- Provare a cercare alcune funzionalità CSS più avanzate (anche in questo caso, il [riferimento CSS di MDN](/it/docs/Web/CSS/Reference) sarà utile) e aggiungere altri stili alla soluzione. Sperimentare!
- Ricordare che non esiste una risposta sbagliata: a questo punto dell'apprendimento ci si può permettere di divertirsi un po'.

## Esempio

L'esempio completato dovrebbe avere un aspetto simile al seguente:

{{EmbedLiveSample("style-bio-finish", "100%", "400px")}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il CSS applicato all'esempio live completato è il seguente:

```css live-sample___style-bio-finish
html {
  background-color: white;
}

body {
  font: 1.2em / 1.5 system-ui;
  padding: 20px;
  width: 500px;
  background-color: #efefef;
  margin: 0 auto;
}

h1 {
  color: darkslategray;
  border-bottom: 10px dotted purple;
  font-size: 2rem;
}

h2 {
  font-style: italic;
  font-size: 1.5rem;
}

.job-title {
  color: darkslategray;
  font-weight: bold;
}

ul {
  background-color: white;
  border: 5px solid purple;
  padding: 30px;
  border-radius: 20px;
}

a {
  color: green;
}

a:hover,
a:focus {
  color: darkgreen;
  text-decoration: none;
}
```

Le proprietà CSS usate per risolvere la sfida sono elencate di seguito; ciascuna rimanda alla relativa pagina della proprietà su MDN, che fornisce ulteriori esempi di utilizzo.

- {{cssxref("background-color")}}
- {{cssxref("border")}} o le relative proprietà longhand.
- {{cssxref("color")}}
- {{cssxref("font-size")}}
- {{cssxref("font-style")}}
- {{cssxref("font-weight")}}
- {{cssxref("margin")}} o le relative proprietà longhand.
- {{cssxref("padding")}} o le relative proprietà longhand.
- {{cssxref("text-decoration")}}
- {{cssxref("width")}}

</details>

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics/Basic_selectors", "Learn_web_development/Core/Styling_basics")}}
