---
title: Immagini, media e elementi di modulo
short-title: Immagini, media, moduli
slug: Learn_web_development/Core/Styling_basics/Images_media_forms
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics")}}

In questa lezione esamineremo come alcuni elementi speciali sono trattati in CSS. Immagini, altri media e elementi di modulo si comportano in modo leggermente diverso dai box normali in termini di capacità di essere stilizzati con CSS. Comprendere cosa è possibile e cosa non lo è può risparmiare frustrazioni, e questa lezione metterà in evidenza alcune delle cose principali che è necessario sapere.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Immagini in HTML <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_images"
          >images</a
        >, <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio"
          >video</a
        >, e <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_forms"
          >moduli</a
        >. Valori e unità in CSS <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Values and units</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Sizing</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere come gli elementi sostituiti sono dimensionati e disposti.</li>
          <li>Stilizzare in modo basilare gli elementi di modulo facili da stilizzare, come i campi di testo.</li>
          <li>Utilizzare un reset CSS come base per stilizzare elementi complessi come i moduli.</li>
          <li>Comprendere che non tutti gli elementi di modulo sono facili da stilizzare e perché.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Elementi sostituiti

Le immagini e i video sono descritti come **{{Glossary("replaced_elements", "elementi sostituiti")}}**. Questo significa che il CSS non può influenzare il layout interno di questi elementi, ma solo la loro posizione nella pagina in mezzo ad altri elementi. Come vedremo, tuttavia, ci sono varie cose che il CSS può fare con un'immagine.

Certi elementi sostituiti, come immagini e video, sono anche descritti come aventi un **{{Glossary("aspect_ratio", "rapporto d'aspetto")}}**. Ciò significa che hanno una dimensione sia in dimensione orizzontale (x) che verticale (y), e verranno visualizzati utilizzando le dimensioni intrinseche del file di default.

## Dimensionamento delle immagini

Come già saprai da queste lezioni, tutto in CSS genera un box. Se posizioni un'immagine all'interno di un box che è più piccolo o più grande delle dimensioni intrinseche del file immagine in qualsiasi direzione, essa apparirà più piccola del box o traboccerà dal box. Devi decidere cosa fare con il trabocco.

Nell'esempio sotto abbiamo due box, entrambi di 200 pixel di dimensione:

- Uno contiene un'immagine più piccola di 200 pixel — è più piccola del box e non si estende per riempirlo.
- L'altro è più grande di 200 pixel e trabocca dal box.

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

Quindi cosa possiamo fare riguardo al problema del trabocco?

Come abbiamo imparato in [Dimensionare gli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing), una tecnica comune è impostare il {{cssxref("max-width")}} di un'immagine al 100%. Questo permetterà all'immagine di diventare più piccola del box ma non più grande. Questa tecnica funzionerà anche con altri elementi sostituiti come [`<video>`](/it/docs/Web/HTML/Reference/Elements/video)s o [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe)s.

Prova ad aggiungere `max-width: 100%` alla regola dell'elemento `<img>` nell'esempio sopra. Noterai che l'immagine più piccola rimane invariata, ma quella più grande diventa più piccola per adattarsi al box.

Puoi fare altre scelte riguardo alle immagini all'interno dei contenitori. Ad esempio, potresti voler dimensionare un'immagine in modo che copra completamente un box.

La proprietà {{cssxref("object-fit")}} può aiutarti qui. Quando si usa `object-fit`, l'elemento sostituito può essere dimensionato per adattarsi a un box in vari modi.

Sotto, il primo esempio usa il valore `cover`, che ridimensiona l'immagine mantenendo il rapporto d'aspetto in modo che riempia perfettamente il box. Poiché il rapporto d'aspetto è mantenuto, alcune parti dell'immagine verranno tagliate dal box. Il secondo esempio usa `contain` come valore: questo ridimensiona l'immagine finché non è abbastanza piccola da adattarsi al box. Questo risulta in un "letterboxing" poiché non è lo stesso rapporto d'aspetto del box.

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

Puoi anche provare il valore `fill`, che riempirà il box ma non manterrà il rapporto d'aspetto.

## Elementi sostituiti nel layout

Quando si utilizzano varie tecniche di layout CSS su elementi sostituiti, potresti scoprire che si comportano in modo leggermente diverso rispetto ad altri elementi. Ad esempio, in un layout a griglia, gli elementi sono allungati per riempire le loro intere {{Glossary("Grid_Areas", "aree della griglia")}} di default. Le immagini non si allungano; invece, sono allineate all'inizio delle loro aree della griglia.

Puoi vedere questo succedere nell'esempio sotto dove abbiamo un container a due colonne e due righe, che ha quattro elementi al suo interno. Tutti gli elementi `<div>` hanno un colore di sfondo e si allungano per riempire la riga e la colonna. L'immagine, tuttavia, non si allunga.

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

Non studierai il layout fino a un modulo successivo. Per ora, tieni a mente che gli elementi sostituiti, quando diventano parte di un sistema di layout specifico come la griglia o il flexbox, hanno comportamenti predefiniti diversi, essenzialmente per evitare che vengano allungati in modo strano dal layout.

## Elementi di modulo

Gli elementi di modulo possono essere una questione complicata quando si tratta di stilizzarli con CSS. Il modulo [Web Forms extensions](/it/docs/Learn_web_development/Extensions/Forms) copre gli aspetti più complicati della stilizzazione di certi tipi di input modulo, che non esploreremo qui. Tuttavia, ci sono alcune basi chiave che vale la pena evidenziare in questa sezione.

Molti controlli del modulo vengono aggiunti alla tua pagina tramite l'elemento [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) — questo definisce campi di modulo semplici come i campi di testo, fino a campi più complessi come i selettori di colori e date. Ci sono alcuni elementi aggiuntivi, come [`<textarea>`](/it/docs/Web/HTML/Reference/Elements/textarea) per l'input di testo multilinea, e anche elementi usati per contenere e etichettare parti di moduli come [`<fieldset>`](/it/docs/Web/HTML/Reference/Elements/fieldset) e [`<legend>`](/it/docs/Web/HTML/Reference/Elements/legend).

HTML include anche attributi che consentono agli sviluppatori web di indicare quali campi sono richiesti e persino il tipo di contenuto che deve essere inserito. Se l'utente inserisce qualcosa di sorprendente, o lascia vuoto un campo richiesto, il browser può mostrare un messaggio di errore. I diversi browser variano tra loro nella quantità di stilizzazione e personalizzazione che consentono per tali elementi.

## Stilizzazione degli elementi di input di testo

Gli elementi che consentono l'input di testo, come `<input type="text">`, e i più specifici `<input type="email">`, e l'elemento `<textarea>` sono piuttosto facili da stilizzare e tendono a comportarsi proprio come gli altri box sulla tua pagina. Tuttavia, la stilizzazione predefinita di questi elementi varia in base al sistema operativo e al browser con cui l'utente visita il sito.

Nell'esempio sotto abbiamo stilizzato alcuni input di testo usando CSS — puoi vedere che cose come bordi, margini e padding si applicano come ti aspetteresti. Stiamo usando selettori di attributi per mirare ai diversi tipi di input. Prova a cambiare l'aspetto di questo modulo regolando i bordi, aggiungendo colori di sfondo ai campi, e cambiando font e padding.

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
  border: 2px solid #000;
  margin: 0 0 1em 0;
  padding: 10px;
  width: 80%;
}

input[type="submit"] {
  border: 3px solid #333;
  background-color: #999;
  border-radius: 5px;
  padding: 10px 2em;
  font-weight: bold;
  color: #fff;
}

input[type="submit"]:hover,
input[type="submit"]:focus {
  background-color: #333;
}
```

{{EmbedLiveSample("form")}}

> [!WARNING]
> Dovresti prestare attenzione quando cambi lo stile degli elementi di modulo per assicurarti che sia ancora ovvio per l'utente che si tratta di elementi di modulo. Potresti creare un input di modulo senza bordi e sfondo che è quasi indistinguibile dal contenuto circostante, ma questo lo renderebbe molto difficile da riconoscere e con cui interagire.

Molti dei tipi di input più complessi vengono resi dal sistema operativo e sono inaccessibili alla stilizzazione. Dovresti quindi sempre presumere che i moduli avranno un aspetto piuttosto diverso per i diversi visitatori e testare moduli complessi in diversi browser.

## Normalizzare il comportamento del modulo

Gli elementi di modulo si comportano diversamente tra i vari browser e sistemi operativi. Questa sezione esamina alcuni dei problemi più comuni e fornisce strategie per affrontarli.

### Ereditarietà e elementi di modulo

In alcuni browser, gli elementi di modulo non ereditano lo stile del font per impostazione predefinita. Pertanto, se vuoi essere sicuro che i campi del tuo modulo utilizzino il font definito sul body, o su un elemento genitore, dovresti aggiungere questa regola nel tuo CSS.

```css
button,
input,
select,
textarea {
  font-family: inherit;
  font-size: 100%;
}
```

### Elementi di modulo e box-sizing

Tra i browser, gli elementi di modulo usano diverse regole di dimensionamento del box per diversi widget. Hai appreso della proprietà `box-sizing` nella [nostra lezione sul modello di box](/it/docs/Learn_web_development/Core/Styling_basics/Box_model) e puoi usare questa conoscenza quando stai stilizzando i moduli per garantire un'esperienza coerente quando imposti larghezze e altezze sugli elementi del modulo.

Per coerenza, è una buona idea impostare margini e padding su `0` per tutti gli elementi, quindi aggiungerli di nuovo quando stai stilizzando particolari controlli:

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

In aggiunta alle regole menzionate sopra, dovresti anche impostare `overflow: auto` sugli elementi `<textarea>` per impedire ad alcuni vecchi browser di mostrare una barra di scorrimento quando non è necessaria:

```css
textarea {
  overflow: auto;
}
```

### Integrazione in un "reset"

Come passo finale, possiamo avvolgere le varie proprietà discusse sopra nel seguente "form reset" per fornire una base coerente da cui lavorare. Questo include tutti gli elementi menzionati nelle ultime tre sezioni:

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
> I fogli di stile di normalizzazione sono utilizzati da molti sviluppatori per creare un set di stili di base da usare su tutti i progetti. In genere fanno cose simili a quelle sopra descritte, assicurandosi che qualsiasi cosa diversa tra i browser sia impostata su un valore predefinito coerente prima di eseguire il proprio lavoro sul CSS. Non sono così importanti come una volta, poiché i browser sono tipicamente più coerenti rispetto al passato. Tuttavia, se vuoi dare un'occhiata a un esempio, dai un'occhiata a [Normalize.css](https://necolas.github.io/normalize.css/), che è un foglio di stile molto popolare usato come base da molti progetti.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Immagini ed elementi di modulo](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Images).

## Riepilogo

Questa lezione ha evidenziato alcune delle differenze che incontrerai lavorando con immagini, media e altri elementi insoliti in CSS.

Nel prossimo articolo impareremo come stilizzare le tabelle HTML.

## Vedi anche

- [Stilizzare i moduli web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms)
- [Stilizzazione avanzata dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics")}}
