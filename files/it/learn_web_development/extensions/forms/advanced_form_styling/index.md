---
title: Stile avanzato dei moduli
slug: Learn_web_development/Extensions/Forms/Advanced_form_styling
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms")}}

In questo articolo, vedremo cosa si può fare con CSS per stilizzare i tipi di controllo dei moduli che sono più difficili da stilizzare — le categorie "brutte" e "cattive". Come abbiamo visto [nell'articolo precedente](/it/docs/Learn_web_development/Extensions/Forms/Styling_web_forms), i campi di testo e i pulsanti sono perfettamente facili da stilizzare; ora ci addentreremo nella stilizzazione delle parti più problematiche.

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
        Comprendere quali parti dei moduli sono difficili da stilizzare e perché; imparare cosa si può fare per personalizzarle.
      </td>
    </tr>
  </tbody>
</table>

Per riassumere ciò che abbiamo detto nell'articolo precedente, abbiamo:

**I cattivi**: Alcuni elementi sono più difficili da stilizzare, richiedendo CSS più complesso o alcuni trucchi più specifici:

- Checkbox e radio button
- [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search)

**I brutti**: Alcuni elementi non possono essere stilizzati a fondo usando CSS. Questi includono:

- Elementi coinvolti nella creazione di widget a discesa, inclusi {{HTMLElement("select")}}, {{HTMLElement("option")}}, {{HTMLElement("optgroup")}} e {{HTMLElement("datalist")}}.
  > [!NOTE]
  > Alcuni browser ora supportano [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select), un insieme di funzionalità HTML e CSS che insieme permettono la piena personalizzazione degli elementi `<select>` e dei loro contenuti proprio come qualsiasi elemento DOM regolare.
- [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color)
- Controlli relativi alle date come [`<input type="datetime-local">`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local)
- [`<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range)
- [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file)
- {{HTMLElement("progress")}} e {{HTMLElement("meter")}}

Parliamo innanzitutto della proprietà [`appearance`](/it/docs/Web/CSS/appearance), che è molto utile per rendere tutti gli elementi sopra elencati più stilizzabili.

## appearance: controllo dello stile a livello di sistema operativo

Nell'articolo precedente abbiamo detto che storicamente, lo stile dei controlli dei moduli web era in gran parte determinato dal sistema operativo sottostante, che è parte del problema nella personalizzazione dell'aspetto di questi controlli.

La proprietà {{cssxref("appearance")}} è stata creata come un modo per controllare quale stile a livello di sistema operativo venisse applicato ai controlli dei moduli web. Di gran lunga il valore più utile, e probabilmente l'unico che userai, è `none`. Questo evita che qualsiasi controllo a cui viene applicato utilizzi lo stile a livello di sistema operativo, nella misura del possibile, permettendoti di costruire tu stesso gli stili usando CSS.

Ad esempio, prendiamo i seguenti controlli:

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

Il seguente esempio dal vivo mostra come appaiono nel tuo sistema — predefinito a sinistra, e con il CSS sopra applicato a destra ([trovalo anche qui](https://mdn.github.io/learning-area/html/forms/styling-examples/appearance-tester.html) se vuoi testarlo su altri sistemi).

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/appearance-tester.html", '100%', 400)}}

Nella maggior parte dei casi, l'effetto è quello di rimuovere il bordo stilizzato, il che rende un po' più facile lo styling CSS, ma non è davvero essenziale. In un paio di casi — caselle di ricerca e pulsanti radio/checkbox, diventa molto più utile. Vediamo ora questi casi.

### Domare le caselle di ricerca

[`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search) è fondamentalmente solo un input di testo, quindi perché `appearance: none;` è utile qui? La risposta è che le caselle di ricerca di Safari hanno alcune restrizioni di stile — non puoi regolare la loro `height` o `font-size` liberamente, ad esempio.

Questo problema può essere risolto usando `appearance: none;`, che disabilita l'aspetto predefinito:

```css
input[type="search"] {
  appearance: none;
}
```

Nell'esempio sotto, puoi vedere due caselle di ricerca stilizzate identiche. Quella a destra ha `appearance: none;` applicato, e quella a sinistra no. Se la guardi su Safari su macOS vedrai che quella a sinistra non è dimensionata correttamente.

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/search-appearance.html", '100%', 200)}}

È interessante notare che impostare un bordo/sfondo sul campo di ricerca risolve anche questo problema. La seguente ricerca stilizzata non ha `appearance: none;` applicato, ma non soffre dello stesso problema su Safari come l'esempio precedente.

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/styled-search.html", '100%', 200)}}

> [!NOTE]
> Potresti aver notato che nel campo di ricerca, l'icona di cancellazione "x", che appare quando il valore della ricerca non è nullo, scompare quando l'input perde il focus su Edge e Chrome, ma rimane visibile su Safari. Per rimuoverla via CSS, puoi usare `input[type="search"]:not(:focus, :active)::-webkit-search-cancel-button { display: none; }`.

### Stilizzare checkbox e pulsanti radio

Stilizzare un checkbox o un pulsante radio è complicato di default. Le dimensioni dei checkbox e dei pulsanti radio non sono pensate per essere cambiate con i loro design predefiniti, e i browser reagiscono in modo molto diverso quando provi.

Ad esempio, considera questo semplice caso di test:

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

Diversi browser gestiscono il checkbox e lo span in modi diversi, spesso estetici non graditi:

| Browser                             | Rendering                                                                                              |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Firefox 71 (macOS)                  | ![Angoli arrotondati e bordo grigio chiaro 1px](firefox-mac-checkbox.png)                                 |
| Firefox 57 (Windows 10)             | ![Angoli rettangolari con bordo grigio medio 1px](firefox-windows-checkbox.png)                       |
| Chrome 77 (macOS), Safari 13, Opera | ![Angoli arrotondati con bordo grigio medio 1px](chrome-mac-checkbox.png)                                 |
| Chrome 63 (Windows 10)              | ![Bordi rettangolari con sfondo leggermente grigiastro anziché bianco.](chrome-windows-checkbox.png) |
| Edge 16 (Windows 10)                | ![Bordi rettangolari con sfondo leggermente grigiastro anziché bianco.](edge-checkbox.png)           |

#### Usare appearance: none su radio/checkbox

Come abbiamo mostrato prima, puoi rimuovere completamente l'aspetto predefinito di un checkbox o un pulsante radio con {{cssxref("appearance", "appearance: none;")}}. Prendiamo questo esempio HTML:

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

Ora, stilizziamo questi con un design di checkbox personalizzato. Iniziamo rimuovendo lo stile originale delle caselle di controllo:

```css
input[type="checkbox"] {
  appearance: none;
}
```

Possiamo usare le pseudo-classi {{cssxref(":checked")}} e {{cssxref(":disabled")}} per cambiare l'aspetto della nostra casella di controllo personalizzata man mano che il suo stato cambia:

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

Scoprirai di più su queste pseudo-classi e altro nel [prossimo articolo](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes); quelle sopra fanno quanto segue:

- `:checked` — la casella di controllo (o il pulsante radio) è in uno stato selezionato — l'utente l'ha cliccata/attivata.
- `:disabled` — la casella di controllo (o il pulsante radio) è in uno stato disabilitato — non può essere interagita.

Puoi vedere il risultato dal vivo:

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/checkboxes-styled.html", '100%', 200)}}

Abbiamo anche creato un paio di altri esempi per darti più idee:

- [Radio button stilizzati](https://mdn.github.io/learning-area/html/forms/styling-examples/radios-styled.html): Stile personalizzato per pulsanti radio.
- [Esempio di interruttore a levetta](https://mdn.github.io/learning-area/html/forms/toggle-switch-example/): Una casella di controllo stilizzata per sembrare un interruttore a levetta.

Se visualizzi queste caselle di controllo in un browser che non supporta {{cssxref("appearance")}}, il tuo design personalizzato andrà perduto, ma appariranno ancora come caselle di controllo e saranno utilizzabili.

## Cosa si può fare con gli elementi "brutti"?

Passiamo ora la nostra attenzione ai controlli "brutti" — quelli che sono davvero difficili da stilizzare a fondo. In breve, questi sono caselle a discesa, tipi di controllo complessi come [`color`](/it/docs/Web/HTML/Reference/Elements/input/color) e [`datetime-local`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local), e controlli orientati al feedback come {{HTMLElement("progress")}} e {{HTMLElement("meter")}}.

Il problema è che questi elementi hanno aspetti predefiniti molto diversi tra i browser, e mentre puoi stilizzarli in alcuni modi, alcune parti dei loro elementi interni sono letteralmente impossibili da stilizzare.

Se sei disposto a convivere con alcune differenze di aspetto e comportamento, puoi arrangiarti con una stilizzazione semplice per rendere coerenti le dimensioni, uno stile uniforme per cose come i colori di sfondo e l'uso di `appearance` per eliminare alcuni stili a livello di sistema.

Prendi il seguente esempio, che mostra una serie di caratteristiche dei moduli "brutti" in azione:

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/ugly-controls.html", '100%', 750)}}

Questo esempio ha il seguente CSS applicato:

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
> Se vuoi testare questi esempi su un numero di browser contemporaneamente, puoi [trovarli dal vivo qui](https://mdn.github.io/learning-area/html/forms/styling-examples/ugly-controls.html) (vedi anche [qui per il codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/styling-examples/ugly-controls.html)).
>
> Tieni anche presente che abbiamo aggiunto un po' di JavaScript alla pagina che elenca i file selezionati dal selettore di file, sotto il controllo stesso. Questa è una versione semplificata dell'esempio trovato sulla pagina di riferimento [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file#examples).

Come puoi vedere, abbiamo fatto abbastanza bene nel far sì che questi risultino uniformi attraverso i browser moderni.

Abbiamo applicato un po' di CSS globale di normalizzazione a tutti i controlli e alle loro etichette, per far sì che abbiano le stesse dimensioni, adottino il font del padre, ecc., come menzionato nell'articolo precedente:

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

Abbiamo anche aggiunto un'ombra uniforme e angoli arrotondati ai controlli per cui aveva senso:

```css
input[type="text"],
input[type="datetime-local"],
input[type="color"],
select {
  box-shadow: inset 1px 1px 3px #ccc;
  border-radius: 5px;
}
```

Su altri controlli come tipi di intervallo, barre di progresso e meter aggiungono solo un brutto riquadro intorno all'area di controllo, quindi non ha senso.

Parliamo di alcuni aspetti specifici di ciascuno di questi tipi di controllo, evidenziando le difficoltà lungo il percorso.

### Select e datalist

Alcuni browser ora supportano [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select), un insieme di funzionalità HTML e CSS che insieme permettono la piena personalizzazione degli `<select>` e dei loro contenuti proprio come qualsiasi elemento DOM regolare. Nei browser e nelle codebase che supportano, non devi più preoccuparti delle tecniche legacy descritte di seguito per gli elementi `<select>`.

La stilizzazione dei datalist e dei select (nei browser che non supportano i select personalizzabili) permette un livello di personalizzazione accettabile a condizione che tu non voglia variare troppo l'aspetto e il comportamento dai predefiniti. Siamo riusciti a ottenere l'aspetto di base delle caselle in modo piuttosto uniforme e coerente. Il controllo invocativo del datalist è comunque un `<input type="text">`, quindi sapevamo che non sarebbe stato un problema.

Due cose sono leggermente più problematiche. Prima di tutto, l'icona "freccia" del select che indica che è una discesa differisce tra i browser. Inoltre, tende a cambiare se aumenti le dimensioni della casella select, o si ridimensiona in modo brutto. Per risolvere questo nel nostro esempio abbiamo utilizzato prima il nostro vecchio amico `appearance: none` per rimuovere l'icona del tutto:

```css
select {
  appearance: none;
}
```

Abbiamo quindi creato la nostra icona utilizzando contenuti generati. Abbiamo messo un wrapper extra intorno al controllo, perché [`::before`](/it/docs/Web/CSS/::before)/[`::after`](/it/docs/Web/CSS/::after) non funzionano sugli elementi `<select>` (perché il loro contenuto è completamente controllato dal browser):

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

Abbiamo quindi usato contenuti generati per generare una piccola freccia verso il basso e posizionarla nel posto giusto usando il posizionamento:

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

Il secondo problema, leggermente più importante, è che non hai controllo sulla casella che appare contenente le opzioni quando clicchi sulla casella `<select>` per aprirla. Puoi ereditare il font impostato sul padre, ma non riuscirai a impostare cose come spaziatura e colori. Lo stesso vale per la lista di completamento automatico che appare con {{HTMLElement("datalist")}}.

Se hai davvero bisogno di pieno controllo sullo stile delle opzioni, dovrai utilizzare qualche tipo di libreria per generare un controllo personalizzato, creare il tuo controllo personalizzato, oppure nel caso di select usare l'attributo `multiple`, che fa apparire tutte le opzioni sulla pagina, aggirando questo particolare problema:

```html
<label for="select">Select fruits</label>
<select id="select" name="select" multiple>
  …
</select>
```

Naturalmente, questo potrebbe anche non adattarsi al design che stai perseguendo, ma è bene notarlo!

### Tipi di input per date

I tipi di input per data/ora ([`datetime-local`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local), [`time`](/it/docs/Web/HTML/Reference/Elements/input/time), [`week`](/it/docs/Web/HTML/Reference/Elements/input/week), [`month`](/it/docs/Web/HTML/Reference/Elements/input/month)) hanno tutti lo stesso problema principale associato. La casella contenente è facile da stilizzare quanto qualsiasi input di testo, e ciò che abbiamo in questa demo sembra a posto.

Tuttavia, le parti interne del controllo (ad esempio, il calendario popup che usi per selezionare una data, lo spinner che puoi usare per incrementare/decrementare i valori) non sono per nulla stilizzabili, e non puoi eliminarli usando `appearance: none;`. Se hai davvero bisogno di pieno controllo sulla stilizzazione, dovrai utilizzare qualche tipo di libreria per generare un controllo personalizzato o costruire il tuo.

> [!NOTE]
> Vale la pena menzionare anche [`<input type="number">`](/it/docs/Web/HTML/Reference/Elements/input/number) — questo ha anche uno spinner che puoi usare per incrementare/decrementare i valori, quindi potenzialmente soffre dello stesso problema. Tuttavia, nel caso del tipo `number` i dati raccolti sono più semplici, ed è facile utilizzare invece un input di tipo `tel` che ha l'aspetto di `text`, ma visualizza la tastiera numerica nei dispositivi con tastiere touch.

### Tipi di input per intervalli

[`<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range) è fastidioso da stilizzare. Puoi usare qualcosa del genere per rimuovere completamente il cursore predefinito e sostituirlo con uno stile personalizzato (in questo caso un cursore sottile rosso):

```css
input[type="range"] {
  appearance: none;
  background: red;
  height: 2px;
  padding: 0;
  outline: 1px solid transparent;
}
```

Tuttavia, è molto difficile personalizzare lo stile della manopola di controllo del range - per ottenere pieno controllo sulla stilizzazione dell'intervallo dovrai usare un'intera serie di codice CSS complesso, comprese molteplici pseudo-elementi non standard e specifici per browser. Dai un'occhiata a [Stylizing Cross-Browser Compatible Range Inputs with CSS](https://css-tricks.com/styling-cross-browser-compatible-range-inputs-css/) su CSS tricks per una descrizione dettagliata di ciò che è necessario.

### Tipi di input per il colore

I controlli di tipo colore non sono troppo male. Nei browser che li supportano, tendono a mostrarti solo un blocco di colore solido con un piccolo bordo.

Puoi rimuovere il bordo, lasciando solo il blocco di colore, usando qualcosa del genere:

```css
input[type="color"] {
  border: 0;
  padding: 0;
}
```

Tuttavia, una soluzione personalizzata è l'unico modo per ottenere qualcosa di significativamente diverso.

### Tipi di input per file

Gli input di tipo file sono generalmente OK — come hai visto nel nostro esempio, è abbastanza facile creare qualcosa che si adatti bene al resto della pagina — la linea di output che fa parte del controllo erediterà il font del parent se dici all'input di farlo, e puoi stilizzare la lista personalizzata dei nomi e delle dimensioni dei file come vuoi; l'abbiamo creata dopotutto.

L'unico problema con i selettori di file è che il pulsante fornito che premi per aprire il selettore di file non è completamente stilizzabile — non può essere dimensionato né colorato, e non accetterà neanche un font diverso.

Un modo per risolvere questo è sfruttare il fatto che se hai un'etichetta associata a un controllo del modulo, cliccando sull'etichetta si attiverà il controllo. Quindi puoi nascondere l'input del modulo effettivo usando qualcosa del genere:

```css
input[type="file"] {
  height: 0;
  padding: 0;
  opacity: 0;
}
```

E poi stilizzare l'etichetta per fungere da pulsante, che quando premuto aprirà il selettore di file come previsto:

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

Puoi vedere il risultato della stilizzazione CSS sopra nell'esempio dal vivo qui sotto (vedi anche [styled-file-picker.html](https://mdn.github.io/learning-area/html/forms/styling-examples/styled-file-picker.html) dal vivo, e il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/forms/styling-examples/styled-file-picker.html)).

{{EmbedGHLiveSample("learning-area/html/forms/styling-examples/styled-file-picker.html", '100%', 200)}}

### Meter e barre di progresso

[`<meter>`](/it/docs/Web/HTML/Reference/Elements/meter) e [`<progress>`](/it/docs/Web/HTML/Reference/Elements/progress) sono forse i peggiori del gruppo. Come hai visto nell'esempio precedente, possiamo impostarli alla larghezza desiderata in modo abbastanza accurato. Ma, oltre a questo, sono davvero difficili da stilizzare in qualsiasi modo. Non gestiscono le impostazioni di altezza in modo coerente tra di loro e tra i browser, puoi colorare lo sfondo, ma non la barra in primo piano, e impostare `appearance: none` su di essi peggiora le cose, non le migliora.

È più facile semplicemente creare la tua soluzione personalizzata per queste funzionalità, se vuoi essere in grado di controllare la stilizzazione, o utilizzare una soluzione di terze parti come [progressbar.js](https://kimmobrunfeldt.github.io/progressbar.js/#examples).

## Riepilogo

Anche se ci sono ancora difficoltà nell'utilizzare CSS con i moduli HTML, ci sono modi per aggirare molti dei problemi. Non ci sono soluzioni pulite e universali, ma i browser moderni offrono nuove possibilità. Per ora, la soluzione migliore è imparare di più su come i diversi browser supportano CSS quando applicato ai controlli dei moduli HTML.

Nel prossimo articolo di questo modulo, esploreremo la creazione di [elementi `<select>` completamente personalizzati](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select) utilizzando le funzionalità HTML e CSS dedicate e moderne disponibili per questo scopo.

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Styling_web_forms", "Learn_web_development/Extensions/Forms/Customizable_select", "Learn_web_development/Extensions/Forms")}}
