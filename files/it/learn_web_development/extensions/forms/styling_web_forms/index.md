---
title: Applicare stili ai moduli web
slug: Learn_web_development/Extensions/Forms/Styling_web_forms
l10n:
  sourceCommit: 0daae80dae181e8156f76439b0df5749f1501bb3
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Other_form_controls","Learn_web_development/Extensions/Forms/Advanced_form_styling","Learn_web_development/Extensions/Forms")}}

Nei precedenti articoli è stato mostrato come creare moduli web in HTML. Ora verrà mostrato come applicare loro stili in [CSS](/it/docs/Web/CSS).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una conoscenza di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti dello stile CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le problematiche legate allo stile dei moduli e apprendere
        alcune tecniche di stile di base che saranno utili.
      </td>
    </tr>
  </tbody>
</table>

## Difficoltà nello stile dei widget dei moduli

### Cronologia

Nel 1995, [la specifica HTML 2](https://datatracker.ietf.org/doc/html/rfc1866) introdusse i controlli dei moduli (detti anche "form widget" o "form element"). Tuttavia, CSS fu rilasciato solo alla fine del 1996 e non fu supportato dalla maggior parte dei browser fino ad anni dopo; nel frattempo, i browser si affidavano al sistema operativo sottostante per il rendering dei widget dei moduli.

Anche dopo la disponibilità di CSS, inizialmente i produttori di browser erano riluttanti a rendere gli elementi dei moduli stilizzabili, poiché gli utenti erano molto abituati all'aspetto dei rispettivi browser. Le cose sono però cambiate e oggi i widget dei moduli sono per lo più stilizzabili, con poche eccezioni.

### Tipi di widget

#### Facili da stilizzare

1. {{HTMLElement("form")}}
2. {{HTMLElement("fieldset")}} e {{HTMLElement("legend")}}
3. {{HTMLElement("input")}} di testo a riga singola (ad esempio, type text, url, email), eccetto [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search).
4. {{HTMLElement("textarea")}} a più righe
5. Pulsanti (sia {{HTMLElement("input")}} sia {{HTMLElement("button")}})
6. {{HTMLElement("label")}}
7. {{HTMLElement("output")}}

#### Più difficili da stilizzare

- Checkbox e pulsanti radio
- [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search)

L'articolo [Stile avanzato dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) mostra come stilizzarli.

#### Con elementi interni che non possono essere stilizzati soltanto con CSS

- [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color)
- Controlli relativi alle date, come [`<input type="datetime-local">`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local)
- [`<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range)
- [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file)
  > [!NOTE]
  > Il pulsante che apre il selettore di file può essere stilizzato con {{cssxref("::file-selector-button")}}. Il testo accanto a esso, che indica il file selezionato, non può esserlo.
- Elementi coinvolti nella creazione di widget a elenco a discesa, inclusi {{HTMLElement("select")}}, {{HTMLElement("option")}}, {{HTMLElement("optgroup")}} e {{HTMLElement("datalist")}}.
  > [!NOTE]
  > Alcuni browser ora supportano gli [elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select), un insieme di funzionalità HTML e CSS che, insieme, consentono la completa personalizzazione degli elementi `<select>` e dei relativi contenuti, proprio come qualsiasi normale elemento DOM.
- {{HTMLElement("progress")}} e {{HTMLElement("meter")}}

Ad esempio, il calendario del selettore di date e il pulsante su `<select>` che visualizza un elenco di opzioni quando viene fatto clic non possono essere stilizzati usando solo CSS.

Gli articoli [Stile avanzato dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) e [Come creare controlli dei moduli personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls) descrivono come stilizzarli.

> [!NOTE]
> Alcuni pseudo-elementi CSS proprietari, come {{cssxref('::-moz-range-track')}}, sono in grado di stilizzare tali componenti interni, ma non sono coerenti tra i browser e quindi non sono molto affidabili. Verranno menzionati più avanti.

## Applicare stili a widget di moduli semplici

I widget "facili da stilizzare" della sezione precedente possono essere stilizzati usando le tecniche degli articoli [Il primo modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form) e [Fondamenti CSS](/it/docs/Learn_web_development/Core/Styling_basics). Esistono anche selettori speciali — le [pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes) — che consentono di applicare stili in base allo stato corrente dell'interfaccia utente.

Alla fine di questo articolo verrà analizzato un esempio, ma prima ecco alcuni aspetti speciali dello stile dei moduli che vale la pena conoscere.

### Font e testo

Le funzionalità CSS per font e testo possono essere usate facilmente con qualsiasi widget (ed è possibile usare {{cssxref("@font-face")}} con i widget dei moduli). Tuttavia, il comportamento dei browser è spesso incoerente. Per impostazione predefinita, alcuni widget non ereditano {{cssxref("font-family")}} e {{cssxref("font-size")}} dai propri elementi genitori. Molti browser usano invece l'aspetto predefinito del sistema. Per rendere coerente l'aspetto dei moduli con il resto del contenuto, è possibile aggiungere le seguenti regole al foglio di stile:

```css
button,
input,
select,
textarea {
  font-family: inherit;
  font-size: 100%;
}
```

Il valore della proprietà {{cssxref('inherit')}} fa sì che il valore della proprietà corrisponda al valore calcolato della proprietà del suo elemento genitore; in altre parole, ne eredita il valore.

Le schermate seguenti mostrano la differenza. A sinistra è riportato il rendering predefinito di `<input type="text">`, `<input type="date">`, {{htmlelement('select')}}, {{htmlelement('textarea')}}, `<input type="submit">` e un `<button>` in Chrome su macOS, con lo stile del font predefinito della piattaforma. A destra sono riportati gli stessi elementi, con la regola di stile precedente applicata.

![Controlli del modulo con famiglie di font predefinite ed ereditate. Per impostazione predefinita, alcuni tipi sono con grazie e altri sono senza grazie. L'ereditarietà dovrebbe modificare i font di tutti nella famiglia di font dell'elemento genitore, in questo caso un paragrafo. Curiosamente, input di tipo submit non eredita dal paragrafo genitore.](forms_fontfamily.png)

I valori predefiniti differivano in vari modi. L'ereditarietà dovrebbe modificare i loro font in quello della famiglia di font dell'elemento genitore — in questo caso, il font con grazie predefinito del contenitore genitore. Tutti lo fanno, con una strana eccezione: `<input type="submit">` non eredita dal paragrafo genitore in Chrome. Usa invece {{cssxref('font-family#Values', 'font-family: system-ui')}}. Questo è un altro motivo per preferire gli elementi `<button>` rispetto ai tipi input equivalenti.

Si discute molto se i moduli abbiano un aspetto migliore usando gli stili predefiniti del sistema oppure stili personalizzati progettati per adattarsi al contenuto. Questa decisione spetta a chi progetta il sito o l'applicazione web.

### Dimensionamento del riquadro

Tutti i campi di testo supportano completamente ogni proprietà relativa al CSS box model, come {{cssxref("width")}}, {{cssxref("height")}}, {{cssxref("padding")}}, {{cssxref("margin")}} e {{cssxref("border")}}. Come in precedenza, tuttavia, i browser si affidano agli stili predefiniti del sistema quando visualizzano questi widget. Spetta allo sviluppatore definire come integrarli nel proprio contenuto. Se si desidera mantenere l'aspetto nativo dei widget, si incontreranno alcune difficoltà nel tentativo di assegnare loro dimensioni coerenti.

**Questo avviene perché ogni widget ha le proprie regole per border, padding e margin.** Per assegnare la stessa dimensione a diversi widget, è possibile usare la proprietà {{cssxref("box-sizing")}} insieme ad alcuni valori coerenti per altre proprietà:

```css
input,
textarea,
select,
button {
  width: 150px;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
```

Nella schermata seguente, la colonna sinistra mostra il rendering predefinito di `<input type="radio">`, `<input type="checkbox">`, `<input type="range">`, `<input type="text">`, `<input type="date">`, {{htmlelement('select')}}, {{htmlelement('textarea')}}, `<input type="submit">` e {{htmlelement('button')}}. La colonna destra mostra invece gli stessi elementi con la regola precedente applicata. Si noti come ciò consenta di garantire che tutti gli elementi occupino la stessa quantità di spazio, nonostante le regole predefinite della piattaforma per ciascun tipo di widget.

![Le proprietà del box model influenzano la maggior parte dei tipi di input.](boxmodel_formcontrols1.png)

Ciò che potrebbe non essere evidente dalla schermata è che i controlli radio e checkbox hanno ancora lo stesso aspetto, ma sono centrati nei 150px di spazio orizzontale forniti dalla proprietà {{cssxref('width')}}. Altri browser potrebbero non centrare i widget, ma rispettano comunque lo spazio assegnato.

### Posizionamento di legend

L'elemento {{HTMLElement("legend")}} può essere stilizzato, ma controllarne il posizionamento può essere un po' complicato. Per impostazione predefinita, è sempre posizionato sopra il bordo superiore del suo elemento genitore {{HTMLElement("fieldset")}}, vicino all'angolo superiore sinistro. Per posizionarlo altrove, ad esempio all'interno del fieldset oppure vicino all'angolo inferiore sinistro, è necessario affidarsi al posizionamento.

Si consideri il seguente esempio:

```html hidden live-sample___positioned-legend
<form>
  <fieldset>
    <legend>Choose all the vegetables you like to eat</legend>
    <ul>
      <li>
        <label for="carrots">Carrots</label>
        <input
          type="checkbox"
          checked
          id="carrots"
          name="carrots"
          value="carrots" />
      </li>
      <li>
        <label for="peas">Peas</label>
        <input type="checkbox" id="peas" name="peas" value="peas" />
      </li>
      <li>
        <label for="cabbage">Cabbage</label>
        <input type="checkbox" id="cabbage" name="cabbage" value="cabbage" />
      </li>
      <li>
        <label for="cauliflower">Cauliflower</label>
        <input
          type="checkbox"
          id="cauliflower"
          name="cauliflower"
          value="cauliflower" />
      </li>
      <li>
        <label for="broccoli">Broccoli</label>
        <input type="checkbox" id="broccoli" name="broccoli" value="broccoli" />
      </li>
    </ul>
  </fieldset>
  <fieldset>
    <legend>What is your favorite meal?</legend>
    <ul>
      <li>
        <label for="soup">Soup</label>
        <input type="radio" checked id="soup" name="meal" value="soup" />
      </li>
      <li>
        <label for="curry">Curry</label>
        <input type="radio" id="curry" name="meal" value="curry" />
      </li>
      <li>
        <label for="pizza">Pizza</label>
        <input type="radio" id="pizza" name="meal" value="pizza" />
      </li>
      <li>
        <label for="tacos">Tacos</label>
        <input type="radio" id="tacos" name="meal" value="tacos" />
      </li>
      <li>
        <label for="bolognese">Bolognese</label>
        <input type="radio" id="bolognese" name="meal" value="bolognese" />
      </li>
    </ul>
  </fieldset>
</form>
```

```css hidden live-sample___positioned-legend
form {
  width: 500px;
  margin: 0 auto;
}

fieldset {
  position: relative;
  margin-bottom: 20px;
}

legend {
  position: absolute;
  color: white;
  background-color: black;
  padding: 3px;
  bottom: 0;
  right: 0;
}
```

{{EmbedLiveSample("positioned-legend", '100%', 400)}}

Per posizionare legend in questo modo, è stato usato il seguente CSS (altre dichiarazioni sono state rimosse per brevità):

```css
fieldset {
  position: relative;
}

legend {
  position: absolute;
  bottom: 0;
  right: 0;
}
```

Anche `<fieldset>` deve essere posizionato, affinché `<legend>` venga posizionato rispetto a esso (altrimenti `<legend>` verrebbe posizionato rispetto a `<body>`).

L'elemento {{HTMLElement("legend")}} è molto importante per l'accessibilità: le tecnologie assistive lo leggeranno come parte dell'etichetta di ogni elemento del modulo all'interno del fieldset. Tuttavia, usare una tecnica come quella precedente va bene. Il contenuto di legend verrà comunque letto nello stesso modo; è cambiata soltanto la posizione visiva.

> [!NOTE]
> È possibile usare anche la proprietà {{cssxref("transform")}} per facilitare il posizionamento di `<legend>`. Tuttavia, posizionandolo ad esempio con `transform: translateY();`, esso si sposta ma lascia uno sgradevole spazio vuoto nel bordo di `<fieldset>`, che non è facile eliminare.

## Un esempio specifico di stile

Esaminiamo un esempio concreto di come stilizzare un modulo HTML. Verrà creato un modulo di contatto dall'aspetto elegante a forma di "cartolina"; [qui è disponibile la versione completata](https://mdn.github.io/learning-area/html/forms/postcard-example/).

Per seguire questo esempio, creare una copia locale del file [postcard-start.html](https://github.com/mdn/learning-area/blob/main/html/forms/postcard-example/postcard-start.html) e seguire le istruzioni riportate di seguito.

### L'HTML

L'HTML è solo leggermente più articolato dell'esempio usato in [Il primo modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form); contiene soltanto alcuni ID aggiuntivi e un'intestazione.

```html
<form>
  <h1>to: Mozilla</h1>

  <div id="from">
    <label for="name">from:</label>
    <input type="text" id="name" name="user_name" />
  </div>

  <div id="reply">
    <label for="mail">reply:</label>
    <input type="email" id="mail" name="user_email" />
  </div>

  <div id="message">
    <label for="msg">Your message:</label>
    <textarea id="msg" name="user_message"></textarea>
  </div>

  <div class="button">
    <button type="submit">Send your message</button>
  </div>
</form>
```

Aggiungere il codice precedente nel body dell'HTML.

### Organizzare le risorse

Qui inizia il divertimento. Prima di iniziare a scrivere codice, sono necessarie tre risorse aggiuntive:

1. [Lo sfondo della cartolina](https://github.com/mdn/learning-area/blob/main/html/forms/postcard-example/background.jpg) — scaricare questa immagine e salvarla nella stessa directory del file HTML su cui si sta lavorando.
2. Un font da macchina da scrivere: [il font "Veteran Typewriter" da dafont.com](https://www.dafont.com/veteran-typewriter.font) — scaricare il file ZIP, estrarlo e copiare il file TTF nella stessa directory indicata sopra.
3. Un font disegnato a mano: [il font "Journal" da dafont.com](https://www.dafont.com/journal.font) — scaricare il file ZIP, estrarlo e copiare il file TTF nella stessa directory indicata sopra.

I font richiedono un'ulteriore elaborazione prima di iniziare:

1. Andare al [generatore di webfont Transfonter](https://transfonter.org/).
2. Premere il pulsante "Add fonts" e caricare entrambi i file TTF.
3. Dopo il caricamento, premere il pulsante "Convert" per generare un kit di webfont.
4. Scaricare il kit sul computer usando il collegamento "Download".
5. Estrarre il file zip fornito.
6. All'interno dei contenuti estratti si trovano alcuni file di font (al momento della scrittura, due file `.woff` e due file `.woff2`; potrebbero variare in futuro). Copiare questi file in una directory denominata `fonts`, all'interno della stessa directory indicata in precedenza. Vengono usati due file diversi per ciascun font per massimizzare la compatibilità del browser; per maggiori informazioni, consultare l'articolo [Web font](/it/docs/Learn_web_development/Core/Text_styling/Web_fonts).

### Il CSS

Ora è possibile esaminare il CSS dell'esempio. Aggiungere tutti i blocchi di codice mostrati di seguito all'interno dell'elemento {{htmlelement("style")}} fornito, uno dopo l'altro.

#### Layout generale

Per prima cosa, prepararsi definendo le regole {{cssxref("@font-face")}} e tutti gli stili di base impostati sugli elementi {{HTMLElement("body")}} e {{HTMLElement("form")}}.

Individuare i blocchi `@font-face` nel kit webfont scaricato, nel file `stylesheet.css`, e sostituire con essi i blocchi `@font-face` riportati di seguito. Aggiornare i percorsi ai file dei font e assicurarsi che i nomi `font-family` di Journal e Veteran Typewriter siano impostati rispettivamente su `handwriting` e `typewriter`. L'output di Transfonter potrebbe essere leggermente diverso da quello qui riportato, ma va bene purché vengano apportate le modifiche richieste.

```css
@font-face {
  font-family: "handwriting";
  src:
    url("fonts/Journal.woff2") format("woff2"),
    url("fonts/Journal.woff") format("woff");
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: "typewriter";
  src:
    url("fonts/VeteranTypewriter.woff2") format("woff2"),
    url("fonts/VeteranTypewriter.woff") format("woff");
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}

body {
  font: 1.3rem sans-serif;
  padding: 0.5em;
  margin: 0;
  background: #222222;
}

form {
  position: relative;
  width: 740px;
  height: 498px;
  margin: 0 auto;
  padding: 1em;
  box-sizing: border-box;
  background: white url("background.jpg");

  /* we create our grid */
  display: grid;
  gap: 20px;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: 10em 1em 1em 1em;
}
```

Si noti che sono stati usati [CSS grid](/it/docs/Web/CSS/Guides/Grid_layout) e [Flexbox](/it/docs/Web/CSS/Guides/Flexible_box_layout) per disporre il modulo. Ciò consente di posizionare facilmente gli elementi, incluso il titolo e tutti gli elementi del modulo:

```css
h1 {
  font:
    1em "typewriter",
    monospace;
  align-self: end;
}

#message {
  grid-row: 1 / 5;
}

#from,
#reply {
  display: flex;
}
```

#### Etichette e controlli

Ora è possibile iniziare a lavorare sugli elementi del modulo. Innanzitutto, assicurarsi che ai {{HTMLElement("label")}} venga assegnato il font corretto:

```css
label {
  font:
    0.8em "typewriter",
    sans-serif;
}
```

I campi di testo richiedono alcune regole comuni. In altre parole, vengono rimossi {{cssxref("border","i bordi")}} e {{cssxref("background","gli sfondi")}}, ridefinendo {{cssxref("padding")}} e {{cssxref("margin")}}:

```css
input,
textarea {
  font:
    1.4em/1.5em "handwriting",
    cursive,
    sans-serif;
  border: none;
  padding: 0 10px;
  margin: 0;
  width: 80%;
  background: none;
}
```

Quando uno di questi campi riceve il focus, viene evidenziato con uno sfondo grigio chiaro e trasparente (è sempre importante avere uno stile per il focus, per l'usabilità e l'accessibilità tramite tastiera):

```css
input:focus,
textarea:focus {
  background: rgb(0 0 0 / 10%);
  border-radius: 5px;
}
```

Ora che i campi di testo sono completi, occorre adattare la visualizzazione dei campi di testo a riga singola e a più righe affinché corrispondano, dato che normalmente non avranno lo stesso aspetto usando i valori predefiniti.

#### Regolare le aree di testo

Gli elementi {{HTMLElement("textarea")}} vengono visualizzati per impostazione predefinita come elementi inline-block. Le due proprietà importanti qui sono {{cssxref("resize")}} e {{cssxref("overflow")}}. Sebbene il design abbia dimensioni fisse e si potrebbe usare la proprietà `resize` per impedire agli utenti di ridimensionare il campo di testo a più righe, è preferibile non impedire agli utenti di ridimensionare una textarea se lo desiderano. La proprietà {{cssxref("overflow")}} viene usata per rendere il campo più coerente tra i browser. Alcuni browser usano per impostazione predefinita il valore `auto`, mentre altri usano il valore `scroll`. In questo caso, è meglio assicurarsi che tutti usino `auto`:

```css
textarea {
  display: block;

  padding: 10px;
  margin: 10px 0 0 -10px;
  width: 100%;
  height: 90%;

  border-right: 1px solid;

  /* resize  : none; */
  overflow: auto;
}
```

#### Applicare stili al pulsante di invio

L'elemento {{HTMLElement("button")}} è davvero pratico da stilizzare con CSS; è possibile fare tutto ciò che si desidera, persino usare gli [pseudo-elementi](/it/docs/Web/CSS/Reference/Selectors/Pseudo-elements):

```css
button {
  padding: 5px;
  font: bold 0.6em sans-serif;
  border: 2px solid #333333;
  border-radius: 5px;
  background: none;
  cursor: pointer;
  transform: rotate(-1.5deg);
}

button::after {
  content: " >>>";
}

button:hover,
button:focus {
  background: black;
  color: white;
}
```

### Il risultato finale

E voilà! Il modulo dovrebbe ora avere un aspetto simile a questo:

![L'aspetto finale e il layout del modulo dopo aver applicato tutti gli stili e le modifiche descritti sopra](updated-form-screenshot.jpg)

> [!NOTE]
> Se l'esempio non funziona come previsto e si desidera confrontarlo con la nostra versione, è possibile trovarla su GitHub: consultare la [versione in esecuzione](https://mdn.github.io/learning-area/html/forms/postcard-example/) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/html/forms/postcard-example)).

## Riepilogo

Come si può vedere, finché si desidera creare moduli con soli campi di testo e pulsanti, è facile stilizzarli usando CSS. [Nel prossimo articolo](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling), verrà illustrato come gestire i widget dei moduli che rientrano nelle categorie "cattiva" e "brutta".

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Other_form_controls","Learn_web_development/Extensions/Forms/Advanced_form_styling","Learn_web_development/Extensions/Forms")}}
