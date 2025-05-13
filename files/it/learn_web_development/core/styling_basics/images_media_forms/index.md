---
title: Immagini, media ed elementi di form
short-title: Immagini, media, forms
slug: Learn_web_development/Core/Styling_basics/Images_media_forms
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics")}}

In questa lezione esamineremo come certi elementi speciali sono trattati in CSS. Le immagini, altri media e gli elementi di form si comportano in modo leggermente diverso rispetto ai normali box in termini di capacità di stile con CSS. Comprendere ciò che è possibile e ciò che non lo è può evitare frustrazioni, e questa lezione evidenzierà alcuni dei principali aspetti che lei deve conoscere.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        HTML <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_images">images</a>, <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_video_and_audio">video</a>, e <a href="/it/docs/Learn_web_development/Core/Structuring_content/HTML_forms">forms</a>. CSS <a href="/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units">Valori e unità</a> e <a href="/it/docs/Learn_web_development/Core/Styling_basics/Sizing">Dimensionamento</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere come gli elementi sostituiti sono dimensionati e disposti.</li>
          <li>Stilizzazione di base di elementi di form facili da stilizzare, come gli input di testo.</li>
          <li>Utilizzare un reset CSS come base su cui stilizzare elementi complessi come i form.</li>
          <li>Comprendere che non tutti gli elementi di form sono facili da stilizzare, e perché.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Elementi sostituiti

Le immagini e i video sono descritti come **{{Glossary("replaced_elements", "elementi sostituiti")}}**. Questo significa che CSS non può influenzare il layout interno di questi elementi — solo la loro posizione sulla pagina tra altri elementi. Come vedremo, però, ci sono varie cose che CSS può fare con un'immagine.

Certi elementi sostituiti, come immagini e video, sono anche descritti come aventi un **{{Glossary("aspect_ratio", "rapporto di aspetto")}}**. Ciò significa che hanno una dimensione sia nelle dimensioni orizzontali (x) che verticali (y), e verranno visualizzati utilizzando le dimensioni intrinseche del file per default.

## Dimensionamento delle immagini

Come lei sa già seguendo queste lezioni, tutto in CSS genera un box. Se lei posiziona un'immagine all'interno di un box che è più piccolo o più grande delle dimensioni intrinseche del file immagine in entrambe le direzioni, essa apparirà più piccola del box o andrà oltre i limiti del box. È necessario prendere una decisione su cosa deve accadere con il traboccamento.

Nell'esempio qui sotto abbiamo due box, entrambi di 200 pixel:

- Uno contiene un'immagine più piccola di 200 pixel — è più piccola del box e non si dilata per riempirlo.
- L'altro è più grande di 200 pixel e trabocca il box.

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

Quindi cosa possiamo fare per il problema del traboccamento?

Come abbiamo appreso in [Dimensionamento degli elementi in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Sizing), una tecnica comune è fare in modo che il {{cssxref("max-width")}} di un'immagine sia il 100%. Questo permetterà all'immagine di diventare più piccola del box ma non più grande. Questa tecnica funzionerà anche con altri elementi sostituiti come [`<video>`](/it/docs/Web/HTML/Reference/Elements/video)e, o [`<iframe>`](/it/docs/Web/HTML/Reference/Elements/iframe)e.

Provi ad aggiungere `max-width: 100%` alla regola dell'elemento `<img>` nell'esempio qui sopra. Vedrà che l'immagine più piccola rimane invariata, ma quella più grande diventa più piccola per adattarsi al box.

Può fare altre scelte riguardo alle immagini all'interno dei contenitori. Ad esempio, potrebbe voler dimensionare un'immagine in modo che copra completamente un box.

La proprietà {{cssxref("object-fit")}} può aiutare in questo caso. Quando si usa `object-fit`, l'elemento sostituito può essere dimensionato per adattarsi a un box in vari modi.

Di seguito, il primo esempio utilizza il valore `cover`, che ridimensiona l'immagine mantenendo il rapporto di aspetto in modo che riempia ordinatamente il box. Poiché il rapporto di aspetto è mantenuto, alcune parti dell'immagine verranno ritagliate dal box. Il secondo esempio utilizza `contain` come valore: questo riduce l'immagine fino a quando è abbastanza piccola da adattarsi all'interno del box. Questo si traduce in "letterboxing" poiché non è lo stesso rapporto di aspetto del box.

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

Potrebbe anche provare il valore `fill`, che riempirà il box ma non manterrà il rapporto di aspetto.

## Elementi sostituiti nel layout

Quando si utilizzano varie tecniche di layout CSS sugli elementi sostituiti, potrebbe scoprire che si comportano in modo leggermente diverso dagli altri elementi. Ad esempio, in un layout a griglia, gli elementi sono allungati per default per riempire l'intere {{Glossary("Grid_Areas", "aree di griglia")}}. Le immagini non si allungano; invece, sono allineate all'inizio delle loro aree di griglia.

Può vedere questo verificarsi nell'esempio qui sotto dove abbiamo un contenitore griglia con due colonne e due righe, che contiene quattro elementi. Tutti gli elementi `<div>` hanno un colore di sfondo e si allungano per riempire la riga e la colonna. L'immagine, tuttavia, non si allunga.

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

Non studierà il layout fino a un modulo successivo. Per ora, tenga solo a mente che gli elementi sostituiti, quando diventano parte di un sistema di layout specifico come grid o flexbox, hanno comportamenti predefiniti diversi, essenzialmente per evitare che vengano allungati in modo strano dal layout.

## Elementi di form

Gli elementi di form possono presentare complicazioni quando si tratta di stilizzarli con CSS. Il modulo [Estensioni Web Forms](/it/docs/Learn_web_development/Extensions/Forms) copre gli aspetti più complicati di stilizzazione di certi tipi di input di form, sui quali non ci soffermeremo qui. Tuttavia, ci sono alcuni elementi di base che vale la pena evidenziare in questa sezione.

Molti controlli di form sono aggiunti alla pagina attraverso l'elemento [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) — questo definisce campi di form semplici come input di testo, fino a campi più complessi come selettori di colore e data. Ci sono alcuni elementi aggiuntivi, come [`<textarea>`](/it/docs/Web/HTML/Reference/Elements/textarea) per l'input di testo su più righe, e anche elementi utilizzati per contenere ed etichettare parti di form come [`<fieldset>`](/it/docs/Web/HTML/Reference/Elements/fieldset) e [`<legend>`](/it/docs/Web/HTML/Reference/Elements/legend).

HTML contiene anche attributi che consentono agli sviluppatori web di indicare quali campi sono obbligatori e persino il tipo di contenuto che deve essere inserito. Se l'utente inserisce qualcosa di inatteso o lascia un campo obbligatorio vuoto, il browser può mostrare un messaggio di errore. I diversi browser variano tra loro quanto alla possibilità di stilizzare e personalizzare questi elementi.

## Stilizzazione degli elementi di input di testo

Gli elementi che consentono l'inserimento di testo, come `<input type="text">`, e i più specifici `<input type="email">`, e l'elemento `<textarea>` sono piuttosto semplici da stilizzare e tendono a comportarsi come altri box nella pagina. Tuttavia, la stilizzazione predefinita di questi elementi differirà in base al sistema operativo e al browser dell'utente che visita il sito.

Nell'esempio qui sotto abbiamo stilizzato alcuni input di testo usando CSS — si può vedere che elementi come i bordi, i margini e il padding si applicano come ci si aspetterebbe. Stiamo utilizzando selettori di attributi per indirizzare i diversi tipi di input. Provi a cambiare l'aspetto di questo form modificando i bordi, aggiungendo colori di sfondo ai campi e cambiando font e padding.

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
> Dovrebbe fare attenzione quando modifica lo stile degli elementi di form per assicurarsi che sia ancora ovvio all'utente che si tratta di elementi di form. Potrebbe creare un input di form senza bordi e sfondo che è quasi indistinguibile dal contenuto circostante, ma questo renderebbe molto difficile riconoscerlo e interagirci.

Molti dei tipi di input più complessi sono resi dal sistema operativo e sono inaccessibili alla stilizzazione. Perciò, dovrebbe sempre assumere che i form appariranno piuttosto diversi per diversi visitatori e testare form complessi in un numero di browser.

## Normalizzare il comportamento dei form

Gli elementi di form si comportano diversamente sui vari browser e sistemi operativi. Questa sezione esamina alcuni dei problemi più comuni e fornisce strategie per affrontarli.

### Ereditarietà e elementi di form

In alcuni browser, gli elementi di form non ereditano gli stili dei font per default. Pertanto, se desidera assicurarsi che i suoi campi di form utilizzino il font definito sul body, o su un elemento genitore, deve aggiungere questa regola al suo CSS.

```css
button,
input,
select,
textarea {
  font-family: inherit;
  font-size: 100%;
}
```

### Elementi di form e box-sizing

Nei browser, gli elementi di form utilizzano diverse regole di dimensionamento dei box per diversi widget. Ha imparato la proprietà `box-sizing` nella [nostra lezione sul modello di box](/it/docs/Learn_web_development/Core/Styling_basics/Box_model) e può utilizzare questa conoscenza quando stila i form per garantire un'esperienza coerente quando si impostano larghezze e altezze sugli elementi di form.

Per coerenza, è una buona idea impostare margini e padding a `0` su tutti gli elementi, quindi aggiungerli di nuovo quando si stanno stilizzando controlli particolari:

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

In aggiunta alle regole menzionate sopra, dovrebbe anche impostare `overflow: auto` sugli elementi `<textarea>` per impedire che i browser più vecchi mostrino una barra di scorrimento quando non è necessaria una:

```css
textarea {
  overflow: auto;
}
```

### Mettere tutto insieme in un "reset"

Come ultimo passaggio, possiamo racchiudere le varie proprietà discusse sopra nel seguente "reset di form" per fornire una base coerente su cui lavorare. Questo include tutti gli elementi menzionati nelle ultime tre sezioni:

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
> I fogli di stile di normalizzazione sono utilizzati da molti sviluppatori per creare un set di stili di base da usare in tutti i progetti. Tipicamente fanno cose simili a quelle descritte sopra, assicurandosi che qualsiasi differenza tra i browser sia impostata su un valore predefinito coerente prima di lavorare sul proprio CSS. Non sono così importanti come una volta, poiché i browser sono generalmente più coerenti rispetto al passato. Tuttavia, se desidera dare un'occhiata a un esempio, può vedere [Normalize.css](https://necolas.github.io/normalize.css/), che è un foglio di stile molto popolare utilizzato come base da molti progetti.

## Testa le tue abilità!

Ha raggiunto la fine di questo articolo, ma può ricordarsi le informazioni più importanti? Può trovare alcuni ulteriori test per verificare se ha trattenuto queste informazioni prima di proseguire — veda [Testa le tue abilità: Immagini ed elementi di form](/it/docs/Learn_web_development/Core/Styling_basics/Test_your_skills/Images).

## Riepilogo

Questa lezione ha evidenziato alcune delle differenze che incontrerà quando lavora con immagini, media e altri elementi inusuali in CSS.

Nel prossimo articolo, impareremo come stilizzare le tabelle HTML.

## Veda anche

- [Stilizzazione dei form web](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms)
- [Stilizzazione avanzata dei form](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Overflow", "Learn_web_development/Core/Styling_basics/Tables", "Learn_web_development/Core/Styling_basics")}}
