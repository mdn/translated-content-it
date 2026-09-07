---
title: Immagini, media ed elementi dei moduli
short-title: Immagini, media, moduli
slug: Learn_web_development/Core/Styling_basics/Images_media_forms
l10n:
  sourceCommit: 3143a6094e7b87cf1a96b61f9551fb4d95049777
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Size_decorate_content_panel", "Learn_web_development/Core/Styling_basics/Test_your_skills/Images", "Learn_web_development/Core/Styling_basics")}}

In questa lezione verrà esaminato come alcuni elementi speciali vengono gestiti in CSS. Le immagini, altri media e gli elementi dei moduli si comportano in modo leggermente diverso dalle normali box per quanto riguarda la possibilità di applicare loro stili con CSS. Comprendere cosa è possibile fare e cosa non lo è può evitare qualche frustrazione, e questa lezione evidenzierà alcuni dei principali aspetti da conoscere.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_images"
          >immagini</a
        >, <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio"
          >video</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_forms"
          >moduli</a
        > HTML. CSS <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere come gli elementi sostituiti vengono dimensionati e disposti.</li>
          <li>Stilizzazione di base degli elementi dei moduli facili da stilizzare, come gli input di testo.</li>
          <li>Utilizzare un reset CSS come base su cui stilizzare elementi complessi come i moduli.</li>
          <li>Comprendere che non tutti gli elementi dei moduli sono facili da stilizzare e il motivo.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Elementi sostituiti

Le immagini e i video sono descritti come **{{Glossary("replaced_elements", "elementi sostituiti")}}**. Ciò significa che CSS non può influenzare il layout interno di questi elementi, ma solo la loro posizione nella pagina rispetto agli altri elementi. Come verrà mostrato, tuttavia, CSS può fare diverse cose con un'immagine.

Alcuni elementi sostituiti, come immagini e video, sono descritti anche come dotati di un **{{Glossary("aspect_ratio", "rapporto d'aspetto")}}**. Ciò significa che hanno una dimensione sia orizzontale (x) sia verticale (y) e, per impostazione predefinita, vengono visualizzati usando le dimensioni intrinseche del file.

## Dimensionare le immagini

Come già noto seguendo queste lezioni, ogni elemento in CSS genera una box. Se un'immagine viene inserita in una box più piccola o più grande delle dimensioni intrinseche del file immagine in una delle due direzioni, l'immagine apparirà più piccola della box oppure traboccherà dalla box. È necessario decidere come gestire il traboccamento.

Nell'esempio seguente sono presenti due box, entrambe di 200 pixel:

- Una contiene un'immagine più piccola di 200 pixel: è più piccola della box e non si estende per riempirla.
- L'altra è più grande di 200 pixel e trabocca dalla box.

```html live-sample___size
<div class="wrapper">
  <div class="box">
    <img
      alt="star"
      src="https://mdn.github.io/shared-assets/images/examples/big-star.png" />
  </div>
  <div class="box">
    <img
      alt="balloons"
      src="https://mdn.github.io/shared-assets/images/examples/balloons.jpg" />
  </div>
</div>
```

```css live-sample___size
.wrapper {
  display: flex;
  align-items: flex-start;
}

.wrapper > * {
  margin: 20px;
}

.box {
  border: 5px solid darkblue;
  width: 200px;
}

img {
}
```

{{EmbedLiveSample("size", "", "250px")}}

Cosa si può fare per il problema del traboccamento?

Come appreso in [Dimensionare gli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing), una tecnica comune consiste nell'impostare {{cssxref("max-width")}} dell'immagine a `100%`. Questo consentirà all'immagine di diventare più piccola della box, ma non più grande. Questa tecnica funziona anche con altri elementi sostituiti come gli [`<video>`](/it/docs/Web/HTML/Reference/Elements/video) o gli [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe).

Provare ad aggiungere `max-width: 100%` alla regola dell'elemento `<img>` nell'esempio precedente. L'immagine più piccola rimarrà invariata, mentre quella più grande si ridurrà per adattarsi alla box.

### Gestire i problemi di visualizzazione delle immagini con `object-fit`

L'esempio precedente rivela un altro insieme di problemi relativi alla visualizzazione delle immagini all'interno dei contenitori. Dopo aver impostato `max-width: 100%` sulle immagini, la seconda immagine non riempie completamente il proprio contenitore: rimane uno spazio nella parte inferiore. Questo accade perché assegnare a un'immagine una larghezza specifica fa sì che la sua altezza venga impostata in modo da preservarne il {{Glossary("aspect_ratio", "rapporto d'aspetto")}}.

Come si può dimensionare l'immagine affinché copra completamente il suo contenitore? Si potrebbe impostare il contenitore con `width` e `height` fissi, quindi assegnare all'immagine `width` e `height` pari a `100%`, come mostrato nell'esempio successivo:

```html live-sample___object-fit1
<div class="box">
  <img
    alt="balloons"
    src="https://mdn.github.io/shared-assets/images/examples/balloons.jpg" />
</div>
```

```css live-sample___object-fit1
.box {
  border: 5px solid darkblue;
  width: 200px;
  height: 200px;
  margin: 20px;
}

img {
  width: 100%;
  height: 100%;
}
```

{{EmbedLiveSample("object-fit1", "", "250px")}}

Tuttavia, l'immagine risulta distorta perché il suo rapporto d'aspetto è stato modificato: appare _allungata_. Per risolvere il problema, è possibile usare la proprietà {{cssxref("object-fit")}}, che definisce come l'immagine viene ridimensionata per adattarsi al suo contenitore, ovvero l'elemento `<img>`. La proprietà `object-fit` può assumere diversi valori; i più utili sono i seguenti:

- `cover`: l'immagine riempie completamente l'elemento `<img>` mantenendo il proprio rapporto d'aspetto; di conseguenza, alcune parti dell'immagine non vengono visualizzate.
- `contain`: l'immagine si adatta completamente all'interno dell'elemento `<img>` mantenendo il proprio rapporto d'aspetto; di conseguenza, alcune parti dell'elemento `<img>` non vengono riempite. Questo produce un effetto di "letterboxing" o "pillarboxing".

L'esempio successivo mostra i valori `cover` e `contain` impostati su due copie dell'immagine mostrata nell'esempio precedente, in modo da poterne osservare gli effetti:

```html live-sample___object-fit
<div class="wrapper">
  <div class="box">
    <img
      alt="balloons"
      class="cover"
      src="https://mdn.github.io/shared-assets/images/examples/balloons.jpg" />
  </div>
  <div class="box">
    <img
      alt="balloons"
      class="contain"
      src="https://mdn.github.io/shared-assets/images/examples/balloons.jpg" />
  </div>
</div>
```

```css live-sample___object-fit
.wrapper {
  display: flex;
  align-items: flex-start;
}

.wrapper > * {
  margin: 20px;
}

.box {
  border: 5px solid darkblue;
  width: 200px;
  height: 200px;
}

img {
  height: 100%;
  width: 100%;
}

.cover {
  object-fit: cover;
}

.contain {
  object-fit: contain;
}
```

{{EmbedLiveSample("object-fit", "", "250px")}}

> [!NOTE]
> I punti principali da ricordare sono:
>
> 1. La proprietà `object-fit` ridimensiona l'immagine stessa affinché si adatti all'interno dell'elemento `<img>` che la incorpora nella pagina.
> 2. L'elemento `<img>` deve essere ridimensionato affinché `object-fit` abbia effetto.
>
> Se l'elemento `<img>` non viene ridimensionato, l'immagine verrà mostrata con le proprie dimensioni e il proprio rapporto d'aspetto originali, o _intrinseci_; pertanto, `object-fit` non avrà effetto.

## Elementi sostituiti nel layout

Quando si usano diverse tecniche di layout CSS sugli elementi sostituiti, è possibile notare che si comportano in modo leggermente diverso rispetto agli altri elementi. Ad esempio, in un layout grid, gli elementi vengono estesi per impostazione predefinita per riempire interamente le loro {{Glossary("Grid_Areas", "aree della griglia")}}. Le immagini non vengono estese; vengono invece allineate all'inizio delle rispettive aree della griglia.

Questo comportamento è visibile nell'esempio seguente, in cui è presente un contenitore grid a due colonne e due righe con quattro elementi. Tutti gli elementi `<div>` hanno un colore di sfondo e si estendono per riempire la riga e la colonna. L'immagine, invece, non si estende.

```html live-sample___layout
<div class="wrapper">
  <img
    alt="star"
    src="https://mdn.github.io/shared-assets/images/examples/big-star.png" />
  <div></div>
  <div></div>
  <div></div>
</div>
```

```css live-sample___layout
.wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 100px 100px;
  gap: 20px;
}

.wrapper > div {
  background-color: rebeccapurple;
  border-radius: 0.5em;
}
```

{{EmbedLiveSample("layout", "", "220px")}}

Il layout verrà studiato in un modulo successivo. Per ora, è sufficiente ricordare che gli elementi sostituiti, quando diventano parte di uno specifico sistema di layout come grid o flexbox, hanno comportamenti predefiniti diversi, essenzialmente per evitare che il layout li estenda in modo anomalo.

## Elementi dei moduli

Gli elementi dei moduli presentano alcune difficoltà quando si tratta di stilizzarli con CSS. Il [modulo di estensione sui moduli Web](/it/docs/Learn_web_development/Extensions/Forms) tratta gli aspetti più complessi della stilizzazione di alcuni tipi di input dei moduli, che non verranno approfonditi qui. Esistono tuttavia alcune nozioni fondamentali importanti da evidenziare in questa sezione.

Molti controlli dei moduli vengono aggiunti alla pagina tramite l'elemento [`<input>`](/it/docs/Web/HTML/Reference/Elements/input), che definisce campi semplici, come gli input di testo, fino a campi più complessi come selettori di colore e data. Esistono alcuni elementi aggiuntivi, come [`<textarea>`](/it/docs/Web/HTML/Reference/Elements/textarea) per l'input di testo su più righe, ed elementi usati per contenere ed etichettare parti dei moduli, come [`<fieldset>`](/it/docs/Web/HTML/Reference/Elements/fieldset) e [`<legend>`](/it/docs/Web/HTML/Reference/Elements/legend).

HTML contiene anche attributi che consentono agli sviluppatori Web di indicare quali campi sono obbligatori e persino il tipo di contenuto che deve essere inserito. Se l'utente inserisce qualcosa di imprevisto o lascia vuoto un campo obbligatorio, il browser può mostrare un messaggio di errore. I diversi browser differiscono per quanto riguarda la quantità di stilizzazione e personalizzazione che consentono per tali elementi.

## Stilizzare gli elementi di input del testo

Gli elementi che consentono l'input di testo, come `<input type="text">`, il più specifico `<input type="email">` e l'elemento `<textarea>`, sono abbastanza facili da stilizzare e tendono a comportarsi proprio come le altre box della pagina. Lo stile predefinito di questi elementi varia tuttavia in base al sistema operativo e al browser con cui l'utente visita il sito.

Nell'esempio seguente, alcuni input di testo sono stati stilizzati usando CSS. È possibile osservare che aspetti come bordi, margini e padding vengono applicati come previsto. Vengono usati selettori di attributo per selezionare i diversi tipi di input.

Provare a modificare l'esempio per cambiare l'aspetto del modulo regolando i bordi, aggiungendo colori di sfondo ai campi e modificando font e padding.

```html live-sample___form
<form>
  <div><label for="name">Name</label> <input id="name" type="text" /></div>
  <div><label for="email">Email</label> <input id="email" type="email" /></div>

  <div class="buttons"><input type="submit" value="Submit" /></div>
</form>
```

```css hidden live-sample___form
body {
  font-family: sans-serif;
}
form > div {
  display: flex;
}

label {
  width: 10em;
}

.buttons {
  justify-content: center;
}
```

```css live-sample___form
input[type="text"],
input[type="email"] {
  border: 2px solid black;
  margin-bottom: 1em;
  padding: 10px;
  width: 80%;
}

input[type="submit"] {
  border: 3px solid #333333;
  background-color: #999999;
  border-radius: 5px;
  padding: 10px 2em;
  font-weight: bold;
  color: white;
}

input[type="submit"]:hover,
input[type="submit"]:focus {
  background-color: #333333;
}
```

{{EmbedLiveSample("form")}}

> [!WARNING]
> Occorre prestare attenzione quando si modifica lo stile degli elementi dei moduli, assicurandosi che sia ancora evidente all'utente che si tratta di elementi del modulo. Si potrebbe creare un input del modulo senza bordi e con uno sfondo quasi indistinguibile dal contenuto circostante, ma ciò renderebbe molto difficile riconoscerlo e interagire con esso.

Molti dei tipi di input più complessi vengono renderizzati dal sistema operativo e non sono accessibili alla stilizzazione. Pertanto, occorre sempre presumere che i moduli avranno un aspetto piuttosto diverso per visitatori diversi e testare i moduli complessi in vari browser.

## Normalizzare il comportamento dei moduli

Gli elementi dei moduli si comportano in modo diverso nei vari browser e sistemi operativi. Questa sezione esamina alcuni dei problemi più comuni e fornisce strategie per affrontarli.

### Ereditarietà ed elementi dei moduli

In alcuni browser, gli elementi dei moduli non ereditano lo stile del font per impostazione predefinita. Pertanto, per essere sicuri che i campi del modulo utilizzino il font definito sul body o su un elemento genitore, è opportuno aggiungere questa regola al CSS.

```css
button,
input,
select,
textarea {
  font-family: inherit;
  font-size: 100%;
}
```

### Elementi dei moduli e box-sizing

Nei vari browser, gli elementi dei moduli usano regole diverse di dimensionamento delle box per widget differenti. La proprietà `box-sizing` è stata trattata nella [lezione sul box model](/it/docs/Learn_web_development/Core/Styling_basics/Box_model), e questa conoscenza può essere applicata quando si stilizzano i moduli per garantire un'esperienza coerente nell'impostazione di larghezze e altezze degli elementi del modulo.

Per coerenza, è una buona idea impostare margini e padding a `0` su tutti gli elementi, quindi aggiungerli di nuovo quando si stilizzano controlli specifici:

```css
button,
input,
select,
textarea {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}
```

### Altre impostazioni utili

Oltre alle regole menzionate sopra, è opportuno impostare anche `overflow: auto` sugli elementi `<textarea>` per impedire che alcuni browser meno recenti mostrino una barra di scorrimento quando non è necessaria:

```css
textarea {
  overflow: auto;
}
```

### Riunire tutto in un "reset"

Come passaggio finale, è possibile raccogliere le diverse proprietà discusse sopra nel seguente "reset dei moduli", per fornire una base coerente da cui partire. Include tutti gli elementi menzionati nelle ultime tre sezioni:

```css
button,
input,
select,
textarea {
  font-family: inherit;
  font-size: 100%;
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

textarea {
  overflow: auto;
}
```

> [!NOTE]
> I fogli di stile di normalizzazione sono usati da molti sviluppatori per creare un insieme di stili di base da utilizzare in tutti i progetti. In genere svolgono funzioni simili a quelle descritte sopra, assicurando che ogni differenza tra browser venga impostata su un valore predefinito coerente prima di iniziare il proprio lavoro sul CSS. Non sono importanti quanto lo erano un tempo, poiché i browser sono generalmente più coerenti rispetto al passato. Tuttavia, per osservare un esempio, consultare [Normalize.css](https://necolas.github.io/normalize.css/), un foglio di stile molto popolare usato come base da molti progetti.

## Riepilogo

Questa lezione ha evidenziato alcune delle differenze che si incontrano lavorando con immagini, media e altri elementi insoliti in CSS.

Nel prossimo articolo verranno proposti alcuni test da usare per verificare quanto bene siano state comprese e memorizzate le informazioni fornite sulla gestione di immagini ed elementi dei moduli in CSS.

## Vedi anche

- [Stilizzare i moduli Web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms)
- [Stilizzazione avanzata dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Size_decorate_content_panel", "Learn_web_development/Core/Styling_basics/Test_your_skills/Images", "Learn_web_development/Core/Styling_basics")}}
