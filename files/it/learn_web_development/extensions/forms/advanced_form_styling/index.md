---
title: Stile avanzato dei moduli
slug: Learn_web_development/Extensions/Forms/Advanced_form_styling
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms")}}

In questo articolo, vedremo cosa si può fare con CSS per stilizzare i tipi di controllo del modulo che sono più difficili da stilizzare — le categorie "cattivo" e "brutto". Come abbiamo visto [nell'articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms), i campi di testo e i pulsanti sono perfettamente facili da stilizzare; ora esploreremo gli elementi più problematici.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere quali parti dei moduli sono difficili da stilizzare e perché; imparare
        cosa si può fare per personalizzarli.
      </td>
    </tr>
  </tbody>
</table>

Per riepilogare ciò che abbiamo detto nell'articolo precedente, abbiamo:

**Il cattivo**: Alcuni elementi sono più difficili da stilizzare, richiedendo CSS più complesso o alcuni trucchi più specifici:

- Checkbox e pulsanti radio
- [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search)

**Il brutto**: Alcuni elementi non possono essere stilizzati completamente usando CSS. Questi includono:

- Elementi coinvolti nella creazione di widget a discesa, compresi {{HTMLElement("select")}}, {{HTMLElement("option")}}, {{HTMLElement("optgroup")}} e {{HTMLElement("datalist")}}.
  > [!NOTE]
  > Alcuni browser ora supportano [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select), un insieme di funzionalità HTML e CSS che insieme consentono la piena personalizzazione degli elementi `<select>` e dei loro contenuti proprio come qualsiasi elemento DOM regolare.
- [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color)
- Controlli relativi alla data come [`<input type="datetime-local">`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local)
- [`<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range)
- [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file)
- {{HTMLElement("progress")}} e {{HTMLElement("meter")}}

Parliamo prima della proprietà [`appearance`](/it/docs/Web/CSS/appearance), che è piuttosto utile per rendere tutti gli elementi sopra elencati più stilizzabili.

## appearance: controllare lo stile a livello di sistema operativo

Nell'articolo precedente abbiamo detto che storicamente, lo stile dei controlli dei moduli web era in gran parte preso dal sistema operativo sottostante, il che è parte del problema con la personalizzazione dell'aspetto di questi controlli.

La proprietà {{cssxref("appearance")}} è stata creata come un modo per controllare quale stile a livello di sistema operativo o di sistema fosse applicato ai controlli dei moduli web. Di gran lunga il valore più utile, e probabilmente l'unico che utilizzerà, è `none`. Questo impedisce a qualsiasi controllo cui viene applicato di utilizzare lo stile a livello di sistema, per quanto possibile, e consente a lei di costruire gli stili da sola usando CSS.

Ad esempio, consideriamo i seguenti controlli:

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

Applicando loro il seguente CSS, si rimuove lo stile a livello di sistema.

```css
input {
  appearance: none;
}
```

Il seguente esempio live mostra come appaiono nel suo sistema — default a sinistra, e con il CSS sopra applicato a destra ([trova anche qui](https://mdn.github.io/learning-area/html/forms/styling-examples/appearance-tester.html) se desidera testarlo su altri sistemi).

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/appearance-tester.html", '100%', 400)}}

Nella maggior parte dei casi, l'effetto è di rimuovere il bordo stilizzato, il che rende la stilizzazione con CSS un po' più facile, ma non è realmente essenziale. In un paio di casi — caselle di ricerca e pulsanti radio/checkbox, diventa molto più utile. Vedremo ora questi casi.

### Addomesticare le caselle di ricerca

[`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search) è fondamentalmente solo un input di testo, quindi perché `appearance: none;` è utile qui? La risposta è che le caselle di ricerca di Safari hanno alcune restrizioni di stile — non può regolare la loro `height` o `font-size` liberamente, per esempio.

Questo può essere risolto usando il nostro amico `appearance: none;`, che disabilita l'aspetto predefinito:

```css
input[type="search"] {
  appearance: none;
}
```

Nell'esempio qui sotto, può vedere due caselle di ricerca identiche stilizzate. Quella di destra ha `appearance: none;` applicato, e quella di sinistra no. Se la guarda in Safari su macOS, vedrà che quella di sinistra non è dimensionata correttamente.

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/search-appearance.html", '100%', 200)}}

Interessante, impostando il bordo/lo sfondo sul campo di ricerca si risolve anche questo problema. La ricerca stilizzata seguente non ha `appearance: none;` applicato, ma non soffre dello stesso problema in Safari rispetto all'esempio precedente.

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/styled-search.html", '100%', 200)}}

> [!NOTE]
> Potrebbe aver notato che nel campo di ricerca, l'icona "x" per eliminare, che appare quando il valore della ricerca non è nullo, scompare quando l'input perde il focus in Edge e Chrome, ma rimane in Safari. Per rimuoverla tramite CSS, può usare `input[type="search"]:not(:focus, :active)::-webkit-search-cancel-button { display: none; }`.

### Stilizzazione di checkbox e pulsanti radio

Stilizzare una checkbox o un pulsante radio è complicato di default. Le dimensioni delle checkbox e dei pulsanti radio non sono pensate per essere cambiate con i loro design predefiniti, e i browser reagiscono in modo molto diverso quando si tenta.

Ad esempio, consideri questo semplice caso di test:

```html
<label
  ><span><input type="checkbox" name="q5" value="true" /></span> True</label
>
<label
  ><span><input type="checkbox" name="q5" value="false" /></span> False</label
>
```

```css
span {
  display: inline-block;
  background: red;
}

input[type="checkbox"] {
  width: 100px;
  height: 100px;
}
```

I diversi browser gestiscono la checkbox e span in modi diversi, spesso brutti:

| Browser                             | Rendering                                                                                              |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Firefox 71 (macOS)                  | ![Angoli arrotondati e bordo grigio chiaro di 1px](firefox-mac-checkbox.png)                           |
| Firefox 57 (Windows 10)             | ![Angoli rettangolari con bordo grigio medio di 1px](firefox-windows-checkbox.png)                     |
| Chrome 77 (macOS), Safari 13, Opera | ![Angolo arrotondato con bordo grigio medio di 1px](chrome-mac-checkbox.png)                           |
| Chrome 63 (Windows 10)              | ![Bordi rettangolari con sfondo leggermente grigiastro invece di bianco.](chrome-windows-checkbox.png) |
| Edge 16 (Windows 10)                | ![Bordi rettangolari con sfondo leggermente grigiastro invece di bianco.](edge-checkbox.png)           |

#### Utilizzo di appearance: none su radio/checkbox

Come abbiamo mostrato prima, può rimuovere completamente l'aspetto predefinito di una checkbox o di un pulsante radio con {{cssxref("appearance", "appearance: none;")}}. Prendiamo questo esempio HTML:

```html
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

Ora, stiliamoli con un design personalizzato per la checkbox. Iniziamo rimuovendo lo stile originale delle checkbox:

```css
input[type="checkbox"] {
  appearance: none;
}
```

Possiamo usare le pseudo-classi {{cssxref(":checked")}} e {{cssxref(":disabled")}} per cambiare l'aspetto della nostra checkbox personalizzata man mano che il suo stato cambia:

```css
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
  background: #ddd;
  color: gray;
}
```

Troverà ulteriori informazioni su tali pseudo-classi e altro nel [prossimo articolo](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes); quelle sopra fanno quanto segue:

- `:checked` — la checkbox (o il pulsante radio) è in stato selezionato — l'utente lo ha cliccato/attivato.
- `:disabled` — la checkbox (o il pulsante radio) è in stato disabilitato — non può essere interagito.

Può vedere il risultato dal vivo:

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/checkboxes-styled.html", '100%', 200)}}

Abbiamo anche creato un paio di altri esempi per darle più idee:

- [Pulsanti radio stilizzati](https://mdn.github.io/learning-area/html/forms/styling-examples/radios-styled.html): Styling personalizzato dei pulsanti radio.
- [Esempio di switch toggle](https://mdn.github.io/learning-area/html/forms/toggle-switch-example/): Una checkbox stilizzata per sembrare un interruttore a levetta.

Se visualizza queste checkbox in un browser che non supporta {{cssxref("appearance")}}, il suo design personalizzato andrà perso, ma appariranno comunque come checkbox e saranno utilizzabili.

## Cosa si può fare con gli elementi "brutti"?

Ora rivolgiamo la nostra attenzione ai controlli "brutti" — quelli che sono davvero difficili da stilizzare completamente. In breve, questi sono le caselle a discesa, i tipi di controllo complessi come [`color`](/it/docs/Web/HTML/Reference/Elements/input/color) e [`datetime-local`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local), e i controlli orientati al feedback come {{HTMLElement("progress")}} e {{HTMLElement("meter")}}.

Il problema è che questi elementi hanno aspetti predefiniti molto diversi tra i browser, e sebbene possa stilizzarli in certi modi, alcune parti del loro interno sono letteralmente impossibili da stilizzare.

Se è pronto ad accettare alcune differenze nell'aspetto e nel funzionamento, può utilizzare uno stile semplice per rendere coerenti le dimensioni, uno stile uniforme per cose come i colori di sfondo e l'uso di appearance per eliminare un po' di stile a livello di sistema.

Prendiamo il seguente esempio, che mostra in azione una serie di funzionalità di modulo "brutte":

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/ugly-controls.html", '100%', 750)}}

Questo esempio ha applicato il seguente CSS:

```css
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
  box-shadow: inset 1px 1px 3px #ccc;
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

> [!NOTE]
> Se desidera testare questi esempi su un numero di browser contemporaneamente, può [trovarlo dal vivo qui](https://mdn.github.io/learning-area/html/forms/styling-examples/ugly-controls.html) (anche [vedi qui per il codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/styling-examples/ugly-controls.html)).
>
> Tenga anche presente che abbiamo aggiunto un po' di JavaScript alla pagina che elenca i file selezionati dal selettore di file, sotto il controllo stesso. Questo è una versione semplificata dell'esempio che si trova sulla pagina di riferimento [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file#examples).

Come può vedere, siamo riusciti abbastanza bene a farli apparire uniformi tra i browser moderni.

Abbiamo applicato alcuni CSS normalizzanti globalmente a tutti i controlli e le loro etichette, per far sì che dimensionino nello stesso modo, adottino il loro font principale, ecc., come accennato nell'articolo precedente:

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

Abbiamo anche aggiunto alcune ombre uniformi e angoli arrotondati ai controlli sui quali aveva senso:

```css
input[type="text"],
input[type="datetime-local"],
input[type="color"],
select {
  box-shadow: inset 1px 1px 3px #ccc;
  border-radius: 5px;
}
```

Su altri controlli come i tipi range, barre di progresso, e metri aggiungono solo un brutto riquadro intorno all'area del controllo, quindi non ha senso.

Parliamo di alcune specifiche di ciascuno di questi tipi di controllo, evidenziando le difficoltà lungo il percorso.

### Select e datalist

Alcuni browser ora supportano [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select), un insieme di funzionalità HTML e CSS che insieme consentono la piena personalizzazione degli elementi `<select>` e dei loro contenuti proprio come qualsiasi elemento DOM regolare. Nei browser e nei codici che supportano, non deve più preoccuparsi delle tecniche legacy descritte di seguito per gli elementi `<select>`.

Stilizzare datalist e select (nei browser che non supportano select personalizzabili) consente un livello accettabile di personalizzazione a condizione che non voglia variare troppo l'aspetto e il funzionamento dai predefiniti. Siamo riusciti a far apparire abbastanza uniforme e coerente l'aspetto di base delle caselle. Il controllo che invoca il datalist è comunque un `<input type="text">`, quindi sapevamo che non sarebbe stato un problema.

Due cose sono leggermente più problematiche. Prima di tutto, l'icona "freccia" del select che indica che è un menu a discesa differisce tra i browser. Inoltre, tende a cambiare se aumenta la dimensione della casella select o si ridimensiona in modo brutto. Per risolvere questo nel nostro esempio abbiamo prima utilizzato il nostro vecchio amico `appearance: none` per rimuovere completamente l'icona:

```css
select {
  appearance: none;
}
```

Abbiamo quindi creato la nostra icona usando il contenuto generato. Abbiamo messo un wrapper extra intorno al controllo, perché [`::before`](/it/docs/Web/CSS/::before)/[`::after`](/it/docs/Web/CSS/::after) non funzionano sugli elementi `<select>` (perché il loro contenuto è completamente controllato dal browser):

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

Abbiamo quindi utilizzato contenuto generato per generare una piccola freccia verso il basso e posizionarla nel posto giusto utilizzando il posizionamento:

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

Il secondo problema, leggermente più importante, è che non si ha controllo sulla casella che appare contenente le opzioni quando si clicca sulla casella `<select>` per aprirla. Si può ereditare il font impostato sul genitore, ma non sarà possibile impostare cose come spaziatura e colori. Lo stesso vale per la lista di completamento automatico che appare con {{HTMLElement("datalist")}}.

Se ha davvero bisogno di un controllo completo sullo stile delle opzioni, dovrà utilizzare una libreria per generare un controllo personalizzato, costruire il proprio controllo personalizzato, oppure nel caso di select utilizzare l'attributo `multiple`, che fa apparire tutte le opzioni sulla pagina, aggirando questo particolare problema:

```html
<label for="select">Select fruits</label>
<select id="select" name="select" multiple>
  …
</select>
```

Ovviamente, questo potrebbe non adattarsi al design che sta cercando, ma è degno di nota!

### Tipi di input per date

I tipi di input per la data/ora ([`datetime-local`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local), [`time`](/it/docs/Web/HTML/Reference/Elements/input/time), [`week`](/it/docs/Web/HTML/Reference/Elements/input/week), [`month`](/it/docs/Web/HTML/Reference/Elements/input/month)) hanno tutti lo stesso problema principale associato. La casella contenente vera e propria è facile da stilizzare come qualsiasi input di testo, e ciò che abbiamo in questa demo sembra andare bene.

Comunque, le parti interne del controllo (ad esempio, il calendario popup che si utilizza per selezionare una data, lo spinner che si può utilizzare per incrementare/decrementare i valori) non sono stilabili affatto, e non è possibile eliminarle utilizzando `appearance: none;`. Se ha bisogno di un controllo completo sullo stile, dovrà utilizzare una libreria per generare un controllo personalizzato o costruire il proprio.

> [!NOTE]
> Vale la pena menzionare anche [`<input type="number">`](/it/docs/Web/HTML/Reference/Elements/input/number) qui — questa ha anche uno spinner che si può utilizzare per incrementare/decrementare i valori, quindi potenzialmente soffre dello stesso problema. Tuttavia, nel caso del tipo `number`, i dati raccolti sono più semplici ed è facile utilizzare il tipo di input `tel` invece che ha l'aspetto di `text`, ma visualizza il tastierino numerico nei dispositivi con tastiere touch.

### Tipi di input per range

[`<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range) è fastidioso da stilizzare. Può utilizzare qualcosa di simile per rimuovere completamente il cursore track predefinito e sostituirlo con uno stile personalizzato (un track rosso sottile, in questo caso):

```css
input[type="range"] {
  appearance: none;
  background: red;
  height: 2px;
  padding: 0;
  outline: 1px solid transparent;
}
```

Tuttavia, è molto difficile personalizzare lo stile del manico di trascinamento del controllo range — per avere un controllo completo sullo stile del range dovrà utilizzare una serie di codice CSS complesso, comprese molte pseudo-elementi specifiche del browser non standard. Controlli [Styling Cross-Browser Compatible Range Inputs with CSS](https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/) su CSS tricks per un'analisi dettagliata di ciò che è necessario.

### Tipi di input per colori

I controlli di input di tipo color non sono troppo male. Nei browser che lo supportano, tendono solo a fornire un blocco di colore solido con un piccolo bordo.

Può rimuovere il bordo, lasciando solo il blocco di colore, utilizzando qualcosa di simile:

```css
input[type="color"] {
  border: 0;
  padding: 0;
}
```

Tuttavia, una soluzione personalizzata è l'unico modo per ottenere qualcosa di significativamente diverso.

### Tipi di input per file

Gli input di tipo file sono generalmente OK — come ha visto nel nostro esempio, è abbastanza facile creare qualcosa che si adatta bene al resto della pagina — la linea di output che è parte del controllo erediterà il font principale se dice all'input di farlo, e può stilizzare la lista personalizzata di nomi di file e dimensioni in qualsiasi modo desidera; dopotutto, l'abbiamo creata noi.

L'unico problema con i selettori di file è che il pulsante fornito che si preme per aprire il selettore di file è completamente non stilizzabile — non può essere ridimensionato o colorato, e non accetterà nemmeno un font diverso.

Un modo per aggirare ciò è sfruttare il fatto che se ha un'etichetta associata a un controllo del modulo, cliccando sull'etichetta si attiverà il controllo. Quindi potrebbe nascondere il vero input del modulo utilizzando qualcosa di simile:

```css
input[type="file"] {
  height: 0;
  padding: 0;
  opacity: 0;
}
```

E poi stilizzare l'etichetta per agire come un pulsante, che quando viene premuto aprirà il selettore di file come previsto:

```css
label[for="file"] {
  box-shadow: 1px 1px 3px #ccc;
  background: linear-gradient(to bottom, #eee, #ccc);
  border: 1px solid rgb(169 169 169);
  border-radius: 5px;
  text-align: center;
  line-height: 1.5;
}

label[for="file"]:hover {
  background: linear-gradient(to bottom, #fff, #ddd);
}

label[for="file"]:active {
  box-shadow: inset 1px 1px 3px #ccc;
}
```

Può vedere il risultato dello stile CSS sopra nell'esempio live qui sotto (veda anche [styled-file-picker.html](https://mdn.github.io/learning-area/html/forms/styling-examples/styled-file-picker.html) live, e il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/styling-examples/styled-file-picker.html)).

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/styled-file-picker.html", '100%', 200)}}

### Metri e barre di progresso

[`<meter>`](/it/docs/Web/HTML/Reference/Elements/meter) e [`<progress>`](/it/docs/Web/HTML/Reference/Elements/progress) sono probabilmente i peggiori del gruppo. Come ha visto nell'esempio precedente, possiamo impostarli in una larghezza desiderata relativamente accurata. Ma oltre a ciò, è davvero difficile stilizzarli in alcun modo. Non gestiscono le impostazioni di altezza in modo coerente tra loro e tra i browser, può colorare lo sfondo, ma non la barra del primo piano, e impostare `appearance: none` su di loro peggiora le cose, non le migliora.

È più facile semplicemente creare una soluzione personalizzata per queste funzionalità, se desidera essere in grado di controllarne lo stile, o utilizzare una soluzione di terze parti come [progressbar.js](https://kimmobrunfeldt.github.io/progressbar.js/#examples).

## Sommario

Mentre ci sono ancora difficoltà a usare CSS con i moduli HTML, ci sono modi per evitare molti dei problemi. Non ci sono soluzioni pulite e universali, ma i browser moderni offrono nuove possibilità. Per ora, la migliore soluzione è imparare di più su come i diversi browser supportano CSS quando applicato ai controlli dei moduli HTML.

Nel prossimo articolo di questo modulo, esploreremo la creazione di [elementi `<select>` completamente personalizzati](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select) utilizzando le funzionalità HTML e CSS moderne dedicate a questo scopo.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms")}}
