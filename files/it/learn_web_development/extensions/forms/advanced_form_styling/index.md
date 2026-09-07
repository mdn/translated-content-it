---
title: Stile avanzato dei moduli
slug: Learn_web_development/Extensions/Forms/Advanced_form_styling
l10n:
  sourceCommit: 0daae80dae181e8156f76439b0df5749f1501bb3
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms")}}

In questo articolo vedremo cosa si può fare con CSS per applicare stili ai tipi di controlli dei moduli più difficili da stilizzare, ovvero le categorie "cattive" e "brutte". Come abbiamo visto [nell'articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms), i campi di testo e i pulsanti sono perfettamente semplici da stilizzare; ora approfondiremo le parti più problematiche.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una conoscenza di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere quali parti dei moduli sono difficili da stilizzare e perché;
        imparare cosa si può fare per personalizzarle.
      </td>
    </tr>
  </tbody>
</table>

Per riassumere quanto detto nell'articolo precedente, abbiamo:

**Le cattive**: alcuni elementi sono più difficili da stilizzare e richiedono CSS più complesso o alcuni accorgimenti più specifici:

- Caselle di controllo e pulsanti di opzione
- [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search)

**Le brutte**: alcuni elementi non possono essere stilizzati completamente usando CSS. Tra questi sono inclusi:

- Elementi coinvolti nella creazione di widget a discesa, inclusi {{HTMLElement("select")}}, {{HTMLElement("option")}}, {{HTMLElement("optgroup")}} e {{HTMLElement("datalist")}}.
  > [!NOTE]
  > Alcuni browser ora supportano gli [elementi `select` personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select), un insieme di funzionalità HTML e CSS che insieme consentono la completa personalizzazione degli elementi `<select>` e dei relativi contenuti, proprio come qualsiasi normale elemento DOM.
- [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color)
- Controlli relativi alla data, come [`<input type="datetime-local">`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local)
- [`<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range)
- [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file)
- {{HTMLElement("progress")}} e {{HTMLElement("meter")}}

Parliamo prima della proprietà {{cssxref("appearance")}}, utile per rendere tutti gli elementi precedenti più stilizzabili.

## `appearance`: controllare lo stile a livello di sistema operativo

Nell'articolo precedente abbiamo menzionato che, storicamente, lo stile dei controlli dei moduli web derivava in larga parte dal sistema operativo sottostante, ed è questo uno dei motivi della difficoltà nel personalizzare l'aspetto di tali controlli.

La proprietà {{cssxref("appearance")}} è stata creata come modo per controllare quale stile a livello di sistema operativo o di sistema venisse applicato ai controlli dei moduli web. Di gran lunga il valore più utile, e probabilmente l'unico che verrà usato, è `none`. Questo impedisce a qualsiasi controllo a cui viene applicato di usare, per quanto possibile, lo stile a livello di sistema e consente di costruire gli stili manualmente usando CSS.

Per esempio, consideriamo i seguenti controlli:

```html
<form>
  <p>
    <label for="search">search: </label>
    <input id="search" name="search" type="search" />
  </p>
  <p>
    <label for="text">text: </label>
    <input id="text" name="text" type="text" />
  </p>
  <p>
    <label for="date">date: </label>
    <input id="date" name="date" type="datetime-local" />
  </p>
  <p>
    <label for="radio">radio: </label>
    <input id="radio" name="radio" type="radio" />
  </p>
  <p>
    <label for="checkbox">checkbox: </label>
    <input id="checkbox" name="checkbox" type="checkbox" />
  </p>
  <p><input type="submit" value="submit" /></p>
  <p><input type="button" value="button" /></p>
</form>
```

Applicando loro il seguente CSS si rimuove lo stile a livello di sistema.

```css
input {
  appearance: none;
}
```

L'esempio interattivo seguente mostra il loro aspetto nel sistema in uso: il valore predefinito a sinistra e il CSS precedente applicato a destra.

```html hidden live-sample___appearance-tester
<div>
  <form>
    <div>
      <label for="search1">search: </label>
      <input id="search1" name="search1" type="search" />
    </div>
    <div>
      <label for="text1">text: </label>
      <input id="text1" name="text1" type="text" />
    </div>
    <div>
      <label for="date1">date: </label>
      <input id="date1" name="date1" type="datetime-local" />
    </div>
    <div>
      <label for="radio1">radio: </label>
      <input id="radio1" name="radio1" type="radio" />
    </div>
    <div>
      <label for="checkbox1">checkbox: </label>
      <input id="checkbox1" name="checkbox1" type="checkbox" />
    </div>
    <div><input type="submit" value="submit" /></div>
    <div><input type="button" value="button" /></div>
  </form>
</div>
<div class="appearance">
  <form>
    <div>
      <label for="search2">search: </label>
      <input id="search2" name="search2" type="search" />
    </div>
    <div>
      <label for="text2">text: </label>
      <input id="text2" name="text2" type="text" />
    </div>
    <div>
      <label for="date2">date: </label>
      <input id="date2" name="date2" type="datetime-local" />
    </div>
    <div>
      <label for="radio2">radio: </label>
      <input id="radio2" name="radio2" type="radio" />
    </div>
    <div>
      <label for="checkbox2">checkbox: </label>
      <input id="checkbox2" name="checkbox2" type="checkbox" />
    </div>
    <div><input type="submit" value="submit" /></div>
    <div><input type="button" value="button" /></div>
  </form>
</div>
```

```css hidden live-sample___appearance-tester
body {
  margin: 20px auto;
  max-width: 800px;
  justify-content: space-around;
}

body,
form > div {
  display: flex;
}

form > div {
  margin-bottom: 20px;
}

.appearance input {
  appearance: none;
}
```

{{EmbedLiveSample("appearance-tester", '100%', 350)}}

Nella maggior parte dei casi, l'effetto consiste nella rimozione del bordo stilizzato, che rende lo stile CSS un po' più semplice, ma non è essenziale. In un paio di casi, come i pulsanti di opzione e le caselle di controllo, diventa molto più utile. Vediamoli ora.

### Caselle di ricerca e `appearance`

Il valore `appearance: none;` era particolarmente utile per stilizzare in modo coerente gli elementi [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search). Senza di esso, Safari non consentiva di impostare su tali elementi i valori {{cssxref("height")}} o {{cssxref("font-size")}}. Tuttavia, questo non è più il caso in Safari 16 e versioni successive. Potrebbe comunque essere utile selezionare esplicitamente `input[type="search"]` con `appearance: none;` se la matrice di supporto dei browser include versioni di Safari precedenti alla 16.

Negli input di ricerca, il pulsante di eliminazione "x", che appare quando il valore non è nullo, scompare quando l'input perde il focus in Edge e Chrome, ma rimane in Safari. Per rimuoverlo tramite CSS, è possibile usare la seguente regola:

```css
input[type="search"]:not(:focus, :active)::-webkit-search-cancel-button {
  display: none;
}
```

### Impostare le tonalità di colore dei controlli dei moduli usando `accent-color`

Se si desidera stilizzare soltanto il colore della tonalità principale di caselle di controllo, pulsanti di opzione o slider di intervallo, {{cssxref("accent-color")}} è sufficiente senza richiedere `appearance: none`. Questa soluzione è utile per i casi di stile di base, poiché i controlli mantengono il loro stile a livello di sistema operativo, ma con un colore principale modificato.

```html live-sample___accent-color
<form>
  <fieldset>
    <legend>Fruit preferences</legend>

    <p>
      <label>
        <input type="checkbox" name="fruit" value="cherry" checked />
        I like cherry
      </label>
    </p>
    <p>
      <label>
        <input type="radio" name="favorite" value="banana" checked />
        Banana is my favorite
      </label>
    </p>
    <p>
      <label>
        How much do you like fruit?
        <input type="range" name="amount" min="0" max="10" value="7" />
      </label>
    </p>
  </fieldset>
</form>
```

```css live-sample___accent-color
input {
  accent-color: rebeccapurple;
}
```

{{EmbedLiveSample("accent-color", '100%', 200)}}

Poiché i controlli mantengono il loro aspetto nativo, seguono le convenzioni della piattaforma, incluse le modalità forced-colors, senza ulteriore lavoro. Inoltre, il browser sceglie automaticamente un colore secondario complementare con contrasto sufficiente rispetto a `accent-color`, in modo da mantenere il controllo accessibile. Provare l'esempio interattivo precedente e impostare alcuni valori `accent-color` chiari e scuri per osservare gli effetti.

### Stilizzare caselle di controllo e pulsanti di opzione usando `appearance`

Applicare ulteriore stile a una casella di controllo o a un pulsante di opzione richiede maggiore impegno. Le dimensioni predefinite delle caselle di controllo e dei pulsanti di opzione non sono pensate per essere modificate e i browser reagiscono in modi molto diversi quando si prova a farlo. Alcuni aumentano la dimensione del controllo, mentre altri la mantengono invariata e aggiungono spazio extra attorno al controllo.

Un approccio molto migliore consiste nel rimuovere completamente l'aspetto predefinito delle caselle di controllo e dei pulsanti di opzione con {{cssxref("appearance", "appearance: none;")}}, quindi aggiungere stili personalizzati ai loro vari stati.

Consideriamo questo esempio HTML:

```html live-sample___checkboxes-styled
<form>
  <fieldset>
    <legend>Fruit preferences</legend>

    <p>
      <label>
        <input type="checkbox" name="fruit" value="cherry" />
        I like cherry
      </label>
    </p>
    <p>
      <label>
        <input type="checkbox" name="fruit" value="banana" disabled />
        I can't like banana
      </label>
    </p>
    <p>
      <label>
        <input type="checkbox" name="fruit" value="strawberry" />
        I like strawberry
      </label>
    </p>
  </fieldset>
</form>
```

Stilizziamoli con un design personalizzato per le caselle di controllo. Inizieremo rimuovendo gli stili originali della casella di controllo:

```css live-sample___checkboxes-styled
input[type="checkbox"] {
  appearance: none;
}
```

Possiamo quindi usare le pseudo-classi {{cssxref(":checked")}} e {{cssxref(":disabled")}} per modificare l'aspetto delle caselle di controllo personalizzate quando il loro stato cambia:

```css live-sample___checkboxes-styled
input[type="checkbox"] {
  position: relative;
  width: 1em;
  height: 1em;
  border: 1px solid gray;
  /* Adjusts the position of the checkboxes on the text baseline */
  vertical-align: -2px;
  /* Set here so that Windows' High-Contrast Mode can override */
  color: green;
}

input[type="checkbox"]::before {
  content: "✔";
  position: absolute;
  font-size: 1.2em;
  right: -1px;
  top: -0.3em;
  visibility: hidden;
}

input[type="checkbox"]:checked::before {
  /* Use `visibility` instead of `display` to avoid recalculating layout */
  visibility: visible;
}

input[type="checkbox"]:disabled {
  border-color: black;
  background: #dddddd;
  color: gray;
}
```

Nell'[articolo successivo](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes) verranno approfondite queste pseudo-classi e altre ancora; quelle precedenti eseguono quanto segue:

- `:checked` — la casella di controllo, o il pulsante di opzione, è nello stato selezionato: l'utente ha fatto clic su di essa o lo ha attivato.
- `:disabled` — la casella di controllo, o il pulsante di opzione, è nello stato disabilitato: non è possibile interagire con essa.

È possibile vedere il risultato interattivo:

{{EmbedLiveSample("checkboxes-styled", '100%', 200)}}

Abbiamo inoltre creato un paio di altri esempi per fornire ulteriori idee:

- [Pulsanti di opzione stilizzati](https://mdn.github.io/learning-area/html/forms/custom-radio-styles/index.html): stile personalizzato per i pulsanti di opzione.
- [Esempio di interruttore a levetta](https://mdn.github.io/learning-area/html/forms/toggle-switch-example/): una casella di controllo stilizzata per sembrare un interruttore a levetta.

## Cosa si può fare con gli elementi "brutti"?

Ora rivolgiamo l'attenzione ai controlli "brutti", quelli veramente difficili da stilizzare completamente. In breve, si tratta di caselle a discesa, tipi di controllo complessi come [`color`](/it/docs/Web/HTML/Reference/Elements/input/color) e [`datetime-local`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local), e controlli orientati al feedback come {{HTMLElement("progress")}} e {{HTMLElement("meter")}}.

Il problema è che questi elementi hanno aspetti predefiniti molto diversi tra browser e, sebbene sia possibile stilizzarli in alcuni modi, alcune parti dei loro elementi interni sono impossibili da stilizzare.

Se si è disposti ad accettare alcune differenze nell'aspetto e nel comportamento, è possibile usare uno stile semplice per migliorare notevolmente la situazione. Questo include dimensioni coerenti, lo stile di proprietà come `background-color` e l'uso di `appearance` per rimuovere parte dello stile a livello di sistema.

Consideriamo l'esempio seguente, che mostra in azione varie funzionalità "brutte" dei moduli:

```html hidden live-sample___ugly-styling
<form>
  <div>
    <label for="select">Select box:</label>
    <div class="select-wrapper">
      <select id="select" name="select">
        <option>Banana</option>
        <option>Cherry</option>
        <option>Lemon</option>
      </select>
    </div>
  </div>
  <div>
    <label for="myFruit">"Favorite fruit?" datalist:</label>
    <input type="text" name="myFruit" id="myFruit" list="mySuggestion" />
    <datalist id="mySuggestion">
      <option>Apple</option>
      <option>Banana</option>
      <option>Blackberry</option>
      <option>Blueberry</option>
      <option>Lemon</option>
      <option>Lychee</option>
      <option>Peach</option>
      <option>Pear</option>
    </datalist>
  </div>
  <div>
    <label for="date1">Datetime local: </label>
    <input id="date1" name="date1" type="datetime-local" />
  </div>
  <div>
    <label for="range">Range: </label>
    <input id="range" name="range" type="range" />
  </div>
  <div>
    <label for="color">Color: </label>
    <input id="color" name="color" type="color" />
  </div>
  <div>
    <label for="file">File picker: </label>
    <input id="file" name="file" type="file" multiple />
    <ul id="file-list"></ul>
  </div>
  <div>
    <label for="progress">Progress: </label>
    <progress max="100" value="75" id="progress">75/100</progress>
  </div>
  <div>
    <label for="meter">Meter: </label>
    <meter
      id="meter"
      min="0"
      max="100"
      value="75"
      low="33"
      high="66"
      optimum="50">
      75
    </meter>
  </div>
  <div><button>Submit?</button></div>
</form>
```

{{EmbedLiveSample("ugly-styling", '100%', 750)}}

È anche possibile premere il pulsante **Play** per eseguire l'esempio in MDN Playground e modificare il codice sorgente.

A questo esempio viene applicato il seguente CSS:

```css live-sample___ugly-styling
body {
  font-family: "Josefin Sans", sans-serif;
  margin: 20px auto;
  max-width: 400px;
}

form > div {
  margin-bottom: 20px;
}

select {
  appearance: none;
  width: 100%;
  height: 100%;
}

.select-wrapper {
  position: relative;
}

.select-wrapper::after {
  content: "▼";
  font-size: 1rem;
  top: 3px;
  right: 10px;
  position: absolute;
}

button,
label,
input,
select,
progress,
meter {
  display: block;
  font-family: inherit;
  font-size: 100%;
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
  height: 30px;
}

input[type="text"],
input[type="datetime-local"],
input[type="color"],
select {
  box-shadow: inset 1px 1px 3px #cccccc;
  border-radius: 5px;
}

label {
  margin-bottom: 5px;
}

button {
  width: 60%;
  margin: 0 auto;
}
```

Abbiamo aggiunto alla pagina del codice JavaScript che elenca i file selezionati dal selettore di file, sotto il controllo stesso. Si tratta di una versione semplificata dell'esempio presente nella pagina di riferimento di [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file#examples):

```js live-sample___ugly-styling
const fileInput = document.querySelector("#file");
const fileList = document.querySelector("#file-list");

fileInput.addEventListener("change", updateFileList);

function updateFileList() {
  while (fileList.firstChild) {
    fileList.removeChild(fileList.firstChild);
  }

  const curFiles = fileInput.files;

  if (!(curFiles.length === 0)) {
    for (const file of curFiles) {
      const listItem = document.createElement("li");
      listItem.textContent = `File name: ${file.name}; file size: ${returnFileSize(file.size)}.`;
      fileList.appendChild(listItem);
    }
  }
}

function returnFileSize(number) {
  if (number < 1e3) {
    return `${number} bytes`;
  } else if (number >= 1e3 && number < 1e6) {
    return `${(number / 1e3).toFixed(1)} KB`;
  }
  return `${(number / 1e6).toFixed(1)} MB`;
}
```

### Stili "globali"

Nell'esempio precedente, siamo riusciti abbastanza bene a rendere i controlli brutti uniformi nei browser moderni.

Abbiamo applicato del CSS globale di normalizzazione a tutti i controlli e alle loro etichette, affinché avessero le stesse dimensioni, adottassero il carattere del genitore e così via, come menzionato nell'articolo precedente:

```css
button,
label,
input,
select,
progress,
meter {
  display: block;
  font-family: inherit;
  font-size: 100%;
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
  height: 30px;
}
```

Abbiamo inoltre aggiunto ombre uniformi e angoli arrotondati ai controlli per cui ha senso farlo:

```css
input[type="text"],
input[type="datetime-local"],
input[type="color"],
select {
  box-shadow: inset 1px 1px 3px #cccccc;
  border-radius: 5px;
}
```

Su altri controlli, come i tipi range, le barre di avanzamento e i misuratori, questi aggiungono soltanto una brutta casella attorno all'area del controllo, quindi non ha senso.

Parliamo di alcune specificità di ciascuno di questi tipi di controllo, evidenziando le difficoltà lungo il percorso.

### Select e datalist

Alcuni browser ora supportano gli [elementi `select` personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select), un insieme di funzionalità HTML e CSS che insieme consentono la completa personalizzazione degli elementi `<select>` e dei relativi contenuti, proprio come qualsiasi normale elemento DOM. Nei browser e nelle basi di codice che li supportano, non è più necessario preoccuparsi delle tecniche legacy descritte di seguito per gli elementi `<select>`.

Stilizzare datalist e select, nei browser che non supportano select personalizzabili, consente un livello di personalizzazione accettabile, purché non si desideri variare troppo l'aspetto e il comportamento rispetto ai valori predefiniti. Siamo riusciti a ottenere caselle dall'aspetto abbastanza uniforme e coerente. Il controllo che richiama il datalist è comunque un `<input type="text">`, quindi sapevamo che non sarebbe stato un problema.

Due aspetti sono leggermente più problematici. Prima di tutto, l'icona a forma di "freccia" del select che indica che si tratta di un elenco a discesa differisce tra browser. Inoltre, tende a cambiare in modo sgradevole se si aumenta la dimensione della casella select o la si ridimensiona. Per risolvere questo problema nel nostro esempio, abbiamo prima usato il vecchio amico `appearance: none` per eliminare completamente l'icona:

```css
select {
  appearance: none;
}
```

Abbiamo quindi creato un'icona personalizzata usando contenuto generato. Abbiamo inserito un wrapper aggiuntivo attorno al controllo, poiché {{cssxref("::before")}}/{{cssxref("::after")}} non funzionano sugli elementi `<select>`: il loro contenuto è completamente controllato dal browser.

```html
<label for="select">Select a fruit</label>
<div class="select-wrapper">
  <select id="select" name="select">
    <option>Banana</option>
    <option>Cherry</option>
    <option>Lemon</option>
  </select>
</div>
```

Usiamo quindi contenuto generato per creare una piccola freccia verso il basso e la posizioniamo nel punto corretto usando il posizionamento:

```css
.select-wrapper {
  position: relative;
}

.select-wrapper::after {
  content: "▼";
  font-size: 1rem;
  top: 6px;
  right: 10px;
  position: absolute;
}
```

Il secondo problema, leggermente più importante, è che non si ha controllo sulla casella contenente le opzioni che appare quando si fa clic sulla casella `<select>` per aprirla. È possibile ereditare il carattere impostato sul genitore, ma non si potranno impostare elementi come spaziatura e colori. Lo stesso vale per l'elenco di completamento automatico che appare con {{HTMLElement("datalist")}}.

Se è davvero necessario il controllo completo sullo stile delle opzioni, occorrerà usare una libreria per generare un controllo personalizzato oppure crearne uno. Nel caso di `<select>`, è anche possibile usare l'attributo `multiple`, che fa apparire tutte le opzioni nella pagina, aggirando questo particolare problema:

```html
<label for="select">Select fruits</label>
<select id="select" name="select" multiple>
  …
</select>
```

Naturalmente, potrebbe anche non adattarsi al design desiderato, ma vale la pena segnalarlo.

### Tipi di input per data

I tipi di input data/ora ([`datetime-local`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local), [`time`](/it/docs/Web/HTML/Reference/Elements/input/time), [`week`](/it/docs/Web/HTML/Reference/Elements/input/week), [`month`](/it/docs/Web/HTML/Reference/Elements/input/month)) hanno tutti lo stesso problema principale associato. La casella contenitore effettiva è semplice da stilizzare quanto qualsiasi input di testo e ciò che abbiamo in questa demo ha un bell'aspetto.

Tuttavia, le parti interne del controllo, ad esempio il calendario popup usato per scegliere una data e lo spinner usato per incrementare o decrementare i valori, non sono affatto stilizzabili e non è possibile eliminarle usando `appearance: none;`. Se è davvero necessario il controllo completo sullo stile, occorrerà usare una libreria per generare un controllo personalizzato oppure crearne uno.

> [!NOTE]
> Anche [`<input type="number">`](/it/docs/Web/HTML/Reference/Elements/input/number) dispone di uno spinner e le sue parti interne non sono più semplici da stilizzare. Per rimuovere lo spinner, usare [`<input type="text">`](/it/docs/Web/HTML/Reference/Elements/input/text) con [`inputmode="numeric"`](/it/docs/Web/HTML/Reference/Global_attributes/inputmode) impostato per visualizzare un tastierino numerico sui dispositivi con tastiere touch e un attributo [`pattern`](/it/docs/Web/HTML/Reference/Attributes/pattern) che limita i valori di input a un numero. Vedere anche [`<input type="number">` > Accessibilità](/it/docs/Web/HTML/Reference/Elements/input/number#accessibility).

### Tipi di input range

[`<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range) è fastidioso da stilizzare. È possibile usare qualcosa di simile al seguente per rimuovere completamente la traccia predefinita dello slider e sostituirla con uno stile personalizzato, in questo caso una traccia rossa sottile:

```css
input[type="range"] {
  appearance: none;
  background: red;
  height: 2px;
  padding: 0;
  outline: 1px solid transparent;
}
```

Tuttavia, è molto difficile personalizzare lo stile della maniglia di trascinamento del controllo range. Per ottenere il controllo completo sullo stile di range, sarà necessario usare codice CSS complesso, incluse molteplici pseudo-elementi non standard e specifici del browser. Per una descrizione dettagliata di ciò che è necessario, consultare [Styling Cross-Browser Compatible Range Inputs with CSS](https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/) su CSS Tricks.

### Tipi di input color

I controlli di input di tipo color non sono troppo problematici. Nei browser che li supportano, tendono a fornire un blocco di colore pieno con un piccolo bordo.

È possibile rimuovere il bordo, lasciando soltanto il blocco di colore, usando qualcosa di simile a questo:

```css
input[type="color"] {
  border: 0;
  padding: 0;
}
```

Tuttavia, una soluzione personalizzata è l'unico modo per ottenere qualcosa di significativamente diverso.

### Tipi di input file

Gli input di tipo file sono generalmente adeguati: è abbastanza semplice creare qualcosa che si integri bene con il resto della pagina. La riga di output che fa parte del controllo erediterà il carattere del genitore se viene indicato all'input di farlo e l'elenco personalizzato dei nomi e delle dimensioni dei file può essere stilizzato nel modo desiderato.

Il pulsante premuto per aprire il selettore di file può essere stilizzato con lo pseudo-elemento {{cssxref("::file-selector-button")}}, che accetta le stesse proprietà di qualsiasi altro pulsante:

```html live-sample___file-selector-button
<form>
  <label for="avatar">Choose a profile picture</label>
  <input id="avatar" name="avatar" type="file" />
</form>
```

```css live-sample___file-selector-button
input[type="file"]::file-selector-button {
  border: 1px solid darkgrey;
  border-radius: 5px;
  background: linear-gradient(to bottom, #eeeeee, #cccccc);
  padding: 0.25em 0.75em;
  font: inherit;
}
```

{{EmbedLiveSample("file-selector-button", '100%', 100)}}

Non è possibile stilizzare il testo accanto al pulsante, ovvero il messaggio "nessun file scelto", né il nome del file visualizzato una volta scelto. Il browser genera quel testo e non lo espone a CSS. Per aggirare questo problema, usare l'etichetta del controllo e il fatto che fare clic sull'etichetta attiva il controllo.

È possibile nascondere l'input del modulo effettivo usando qualcosa di simile a questo:

```css
input[type="file"] {
  height: 0;
  padding: 0;
  opacity: 0;
}
```

Quindi stilizzare l'etichetta affinché agisca come un pulsante che, quando viene premuto, apre il selettore di file come previsto:

```css
label[for="file"] {
  box-shadow: 1px 1px 3px #cccccc;
  background: linear-gradient(to bottom, #eeeeee, #cccccc);
  border: 1px solid darkgrey;
  border-radius: 5px;
  text-align: center;
  line-height: 1.5;
}

label[for="file"]:hover {
  background: linear-gradient(to bottom, white, #dddddd);
}

label[for="file"]:active {
  box-shadow: inset 1px 1px 3px #cccccc;
}
```

Il risultato dello stile CSS precedente è visibile nell'esempio interattivo seguente.

```html hidden live-sample___styled-file-picker
<form>
  <div>
    <label for="file">Choose a file to upload</label>
    <input id="file" name="file" type="file" multiple />
    <ul id="file-list"></ul>
  </div>
  <div><button>Submit?</button></div>
</form>
```

```css hidden live-sample___styled-file-picker
@import "https://fonts.googleapis.com/css2?family=Josefin+Sans:ital,wght@0,100..700;1,100..700&display=swap";

body {
  font-family: "Josefin Sans", sans-serif;
  margin: 20px auto;
  max-width: 400px;
}

form > div {
  margin-bottom: 20px;
}

button,
label,
input {
  display: block;
  font-family: inherit;
  font-size: 100%;
  margin: 0;
  box-sizing: border-box;
  width: 100%;
  padding: 5px;
  height: 30px;
}

input[type="file"] {
  height: 0;
  padding: 0;
  opacity: 0;
}

label[for="file"] {
  box-shadow: 1px 1px 3px #cccccc;
  background: linear-gradient(to bottom, #eeeeee, #cccccc);
  border: 1px solid darkgrey;
  border-radius: 5px;
  text-align: center;
  line-height: 1.5;
}

label[for="file"]:hover {
  background: linear-gradient(to bottom, white, #dddddd);
}

label[for="file"]:active {
  box-shadow: inset 1px 1px 3px #cccccc;
}

button {
  width: 60%;
  margin: 0 auto;
}
```

```js hidden live-sample___styled-file-picker
const fileInput = document.querySelector("#file");
const fileList = document.querySelector("#file-list");

fileInput.addEventListener("change", updateFileList);

function updateFileList() {
  while (fileList.firstChild) {
    fileList.removeChild(fileList.firstChild);
  }

  let curFiles = fileInput.files;

  if (!(curFiles.length === 0)) {
    for (const file of curFiles) {
      const listItem = document.createElement("li");
      listItem.textContent = `File name: ${file.name}; file size: ${returnFileSize(file.size)}.`;
      fileList.appendChild(listItem);
    }
  }
}

function returnFileSize(number) {
  if (number < 1e3) {
    return `${number} bytes`;
  } else if (number >= 1e3 && number < 1e6) {
    return `${(number / 1e3).toFixed(1)} KB`;
  }
  return `${(number / 1e6).toFixed(1)} MB`;
}
```

{{EmbedLiveSample("styled-file-picker", '100%', 200)}}

È anche possibile premere il pulsante **Play** per eseguire l'esempio in MDN Playground e consultare il codice sorgente completo.

### Misuratori e barre di avanzamento

[`<meter>`](/it/docs/Web/HTML/Reference/Elements/meter) e [`<progress>`](/it/docs/Web/HTML/Reference/Elements/progress) sono probabilmente i peggiori in assoluto. Come visto nell'esempio precedente, possiamo impostarli con relativa precisione alla larghezza desiderata. Oltre a questo, però, sono davvero difficili da stilizzare. Non gestiscono le impostazioni di altezza in modo coerente tra loro e tra browser; è possibile colorare lo sfondo ma non la barra in primo piano e impostare `appearance: none` su di essi peggiora la situazione anziché migliorarla.

È più semplice creare una soluzione personalizzata per controllare lo stile di queste funzionalità oppure usare una soluzione di terze parti, come [progressbar.js](https://kimmobrunfeldt.github.io/progressbar.js/#examples).

## Riepilogo

Stilizzare i moduli HTML presenta alcune difficoltà; tuttavia, esistono modi per aggirarne molte. Non esistono soluzioni pulite e universali, ma i browser moderni offrono nuove possibilità. Per il momento, la soluzione migliore è imparare di più sul supporto CSS offerto dai diversi browser quando applicato ai controlli dei moduli HTML.

Nel prossimo articolo esploreremo la creazione di [elementi `<select>` completamente personalizzati](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select) usando le funzionalità HTML e CSS moderne dedicate disponibili a questo scopo.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms")}}
