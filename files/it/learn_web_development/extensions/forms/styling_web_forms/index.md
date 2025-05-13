---
title: Styling web forms
slug: Learn_web_development/Extensions/Forms/Styling_web_forms
l10n:
  sourceCommit: a1ac64fa4da965d2a152f08221b1a9aed638fd16
---

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Other_form_controls","Learn_web_development/Extensions/Forms/Advanced_form_styling","Learn_web_development/Extensions/Forms")}}

Nei precedenti articoli, abbiamo mostrato come creare moduli web in HTML. Ora, mostreremo come stilizzarli in [CSS](/it/docs/Web/CSS).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione di base di
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi del CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le problematiche dietro la stilizzazione dei moduli e apprendere alcune delle
        tecniche di base di stilizzazione che le saranno utili.
      </td>
    </tr>
  </tbody>
</table>

## Sfide nella stilizzazione dei widget del modulo

### Storia

Nel 1995, [la specifica HTML 2](https://datatracker.ietf.org/doc/html/rfc1866) introdusse i controlli dei moduli (alias "widget del modulo" o "elementi del modulo"). Tuttavia, il CSS non fu rilasciato fino alla fine del 1996 e non fu supportato dalla maggior parte dei browser fino a molti anni dopo; quindi, nel frattempo, i browser si affidavano al sistema operativo sottostante per renderizzare i widget del modulo.

Anche con il CSS disponibile, i fornitori di browser inizialmente erano riluttanti a rendere stilizzabili gli elementi del modulo, perché gli utenti erano così abituati all'aspetto dei rispettivi browser. Ma le cose sono cambiate, e i widget del modulo sono ora per lo più stilizzabili, con alcune eccezioni.

### Tipi di widget

#### Facili da stilizzare

1. {{HTMLElement("form")}}
2. {{HTMLElement("fieldset")}} e {{HTMLElement("legend")}}
3. {{HTMLElement("input")}} di testo a singola riga (ad esempio, type text, url, email), eccetto [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search).
4. {{HTMLElement("textarea")}} multilinea
5. Pulsanti (sia {{HTMLElement("input")}} che {{HTMLElement("button")}})
6. {{HTMLElement("label")}}
7. {{HTMLElement("output")}}

#### Più difficili da stilizzare

- Checkbox e radio button
- [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search)

L'articolo [Stilizzazione avanzata dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) mostra come stilizzare questi.

#### Con elementi interni che non possono essere stilizzati solo con CSS

- [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color)
- Controlli relativi alle date come [`<input type="datetime-local">`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local)
- [`<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range)
- [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file)
- Elementi coinvolti nella creazione di widget a discesa, inclusi {{HTMLElement("select")}}, {{HTMLElement("option")}}, {{HTMLElement("optgroup")}} e {{HTMLElement("datalist")}}.
  > [!NOTE]
  > Alcuni browser ora supportano [Select elements personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select), un set di funzionalità HTML e CSS che insieme consentono la completa personalizzazione degli elementi `<select>` e dei loro contenuti proprio come qualsiasi elemento DOM regolare.
- {{HTMLElement("progress")}} e {{HTMLElement("meter")}}

Ad esempio, il calendario di selezione della data e il pulsante su \<select> che visualizza un elenco di opzioni quando cliccato, non possono essere stilizzati solo con CSS.

Gli articoli [Stilizzazione avanzata dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) e [Come costruire controlli di modulo personalizzati](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls) descrivono come stilizzare questi.

> [!NOTE]
> Alcuni pseudo-elementi CSS proprietari, come {{cssxref('::-moz-range-track')}}, sono in grado di stilizzare tali componenti interni, ma non sono coerenti tra i browser, quindi non sono molto affidabili. Li menzioneremo più avanti.

## Stilizzazione di semplici widget del modulo

I widget "facili da stilizzare" nella sezione precedente possono essere stilizzati utilizzando le tecniche degli articoli [Il suo primo modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form) e [Blocchi di costruzione CSS](/it/docs/Learn_web_development/Core/Styling_basics). Ci sono anche selettori speciali — [pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes) — che consentono di stilizzare in base allo stato corrente della UI.

Esamineremo un esempio alla fine di questo articolo — ma prima, ecco alcuni aspetti speciali della stilizzazione dei moduli che vale la pena conoscere.

### Font e testo

Le funzionalità di font e testo CSS possono essere utilizzate facilmente con qualsiasi widget (e sì, può usare {{cssxref("@font-face")}} con i widget del modulo). Tuttavia, il comportamento del browser è spesso incoerente. Per impostazione predefinita, alcuni widget non ereditano {{cssxref("font-family")}} e {{cssxref("font-size")}} dai loro genitori. Molti browser utilizzano invece l'aspetto predefinito del sistema. Per rendere l'aspetto dei moduli coerente con il resto del suo contenuto, può aggiungere le seguenti regole al suo foglio di stile:

```css
button,
input,
select,
textarea {
  font-family: inherit;
  font-size: 100%;
}
```

Il valore della proprietà {{cssxref('inherit')}} fa sì che il valore della proprietà corrisponda al valore calcolato della proprietà del suo elemento genitore; ereditando il valore del genitore.

Gli screenshot seguenti mostrano la differenza. A sinistra c'è il rendering predefinito di un `<input type="text">`, `<input type="date">`, {{htmlelement('select')}}, {{htmlelement('textarea')}}, `<input type="submit">`, e un `<button>` in Chrome su macOS, con lo stile del font predefinito della piattaforma in uso. A destra ci sono gli stessi elementi, con la nostra regola di stile sopra applicata.

![Controlli dei moduli con famiglie di font predefinite e ereditate. Per impostazione predefinita, alcuni tipi sono serif e altri sono sans serif. L'eredità dovrebbe cambiare i font di tutti nella famiglia di font del genitore — in questo caso un paragrafo. Stranamente, l'input di tipo submit non eredita dal paragrafo genitore.](forms_fontfamily.png)

I valori predefiniti differivano in diversi modi. L'eredità dovrebbe cambiare i loro font nella famiglia di font del genitore — in questo caso, il font serif predefinito del contenitore genitore. Tutti lo fanno, con un'eccezione strana — `<input type="submit">` non eredita dal paragrafo genitore in Chrome. Piuttosto, utilizza {{cssxref('font-family#Values', 'font-family: system-ui')}}. Questo è un altro motivo per utilizzare gli elementi `<button>` rispetto ai tipi di input equivalenti!

C'è molto dibattito sul fatto che i moduli appaiano meglio utilizzando gli stili predefiniti di sistema o stili personalizzati progettati per abbinarsi al suo contenuto. Questa decisione spetta a lei, in quanto progettista del suo sito o applicazione web.

### Dimensionamento della scatola

Tutti i campi di testo supportano completamente ogni proprietà relativa al modello di scatola CSS, come {{cssxref("width")}}, {{cssxref("height")}}, {{cssxref("padding")}}, {{cssxref("margin")}}, e {{cssxref("border")}}. Come prima, tuttavia, i browser si affidano agli stili predefiniti del sistema quando visualizzano questi widget. Spetta a lei definire come desidera integrarli nel suo contenuto. Se desidera mantenere l'aspetto nativo e il feeling dei widget, avrà qualche difficoltà se desidera dare loro una dimensione coerente.

**Ciò è dovuto al fatto che ogni widget ha le proprie regole per il bordo, il riempimento e il margine.** Per dare la stessa dimensione a diversi widget, può utilizzare la proprietà {{cssxref("box-sizing")}} insieme ad alcuni valori coerenti per le altre proprietà:

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

Nello screenshot sottostante, la colonna di sinistra mostra il rendering predefinito di un `<input type="radio">`, `<input type="checkbox">`, `<input type="range">`, `<input type="text">`, `<input type="date">`, {{htmlelement('select')}}, {{htmlelement('textarea')}}, `<input type="submit">`, e {{htmlelement('button')}}. La colonna di destra, invece, mostra gli stessi elementi con la nostra regola sopra applicata a loro. Noti come questo ci consente di assicurarci che tutti gli elementi occupino la stessa quantità di spazio, nonostante le regole predefinite della piattaforma per ogni tipo di widget.

![Le proprietà del modello di scatola influenzano la maggior parte dei tipi di input.](boxmodel_formcontrols1.png)

Ciò che potrebbe non essere evidente dallo screenshot è che i controlli radio e checkbox sembrano ancora gli stessi, ma sono centrati nei 150px di spazio orizzontale forniti dalla proprietà {{cssxref('width')}}. Altri browser potrebbero non centrare i widget, ma aderiscono allo spazio assegnato.

### Posizionamento della leggenda

L'elemento {{HTMLElement("legend")}} si può stilizzare, ma può essere un po' complicato controllarne il posizionamento. Per impostazione predefinita, è sempre posizionato sopra il bordo superiore del suo genitore {{HTMLElement("fieldset")}}, vicino all'angolo in alto a sinistra. Per posizionarlo da qualche altra parte, ad esempio all'interno del fieldset da qualche parte, o vicino all'angolo in basso a sinistra, è necessario affidarsi al posizionamento.

Prenda il seguente esempio:

{{EmbedGHLiveSample("learning-area/html/forms/native-form-widgets/positioned-legend.html", '100%', 400)}}

Per posizionare la leggenda in questo modo, abbiamo utilizzato il seguente CSS (altre dichiarazioni rimosse per brevità):

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

Anche il `<fieldset>` deve essere posizionato, in modo che la `<legend>` sia posizionata rispetto a esso (altrimenti la `<legend>` verrebbe posizionata rispetto al `<body>`).

L'elemento {{HTMLElement("legend")}} è molto importante per l'accessibilità — sarà pronunciato dalle tecnologie assistive come parte dell'etichetta di ciascun elemento del modulo all'interno del fieldset — ma usare una tecnica come quella sopra va bene. I contenuti della leggenda saranno ancora pronunciati nello stesso modo; è solo la posizione visiva che è cambiata.

> [!NOTE]
> Lei potrebbe anche utilizzare la proprietà {{cssxref("transform")}} per aiutarla nel posizionamento della sua `<legend>`. Tuttavia, quando la posiziona con ad esempio un `transform: translateY();`, si sposta ma lascia un brutto gap nel bordo del `<fieldset>`, che non è facile da rimuovere.

## Un esempio specifico di stilizzazione

Esaminiamo un esempio concreto di come stilizzare un modulo HTML. Costruiremo un elegante modulo di contatto "cartolina"; [vedi qui per la versione finale](https://mdn.github.io/learning-area/html/forms/postcard-example/).

Se vuole seguire questo esempio, faccia una copia locale del nostro [file postcard-start.html](https://github.com/mdn/learning-area/blob/main/html/forms/postcard-example/postcard-start.html), e segua le istruzioni sottostanti.

### L'HTML

L'HTML è solo leggermente più complesso rispetto all'esempio utilizzato in [Il suo primo modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form); ha solo alcuni ID extra e un'intestazione.

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

Aggiunga il codice sopra nel body del suo HTML.

### Organizzazione dei suoi asset

Qui inizia il divertimento! Prima di iniziare a programmare, abbiamo bisogno di tre asset aggiuntivi:

1. [Lo sfondo della cartolina](https://github.com/mdn/learning-area/blob/main/html/forms/postcard-example/background.jpg) — scarichi questa immagine e la salvi nella stessa directory del suo file HTML di lavoro.
2. Un font da macchina da scrivere: [Il font "Mom's Typewriter" da dafont.com](https://www.dafont.com/moms-typewriter.font?back=theme) — scarichi il file TTF nella stessa directory di cui sopra.
3. Un font a mano libera: [Il font "Journal" da dafont.com](https://www.dafont.com/journal.font) — scarichi il file TTF nella stessa directory di cui sopra.

I suoi font necessitano di un ulteriore processamento prima di iniziare:

1. Vada al generatore di Webfont di fontsquirrel.com [Webfont Generator](https://www.fontsquirrel.com/tools/webfont-generator).
2. Utilizzando il modulo, carichi entrambi i suoi file di font e generi un kit di webfont. Scarichi il kit sul suo computer.
3. Decomprima il file zip fornito.
4. All'interno dei contenuti decompressi troverà alcuni file di font (al momento della scrittura, due file `.woff` e due file `.woff2`; potrebbero variare in futuro). Copi questi file in una directory chiamata fonts, nella stessa directory di prima. Stiamo utilizzando due file diversi per ogni font per massimizzare la compatibilità tra browser; veda il nostro articolo [Web font](/it/docs/Learn_web_development/Core/Text_styling/Web_fonts) per molte più informazioni.

### Il CSS

Ora possiamo approfondire il CSS per l'esempio. Aggiunga tutti i blocchi di codice mostrati di seguito all'interno dell'elemento {{htmlelement("style")}}, uno dopo l'altro.

#### Layout generale

Per prima cosa, ci prepariamo definendo le nostre regole {{cssxref("@font-face")}}, e tutte le basi definiti sugli elementi {{HTMLElement("body")}} e {{HTMLElement("form")}}. Se l'output di fontsquirrel era diverso da quanto descritto sopra, può trovare i blocchi `@font-face` corretti all'interno del suo kit di webfont scaricato, nel file `stylesheet.css` (dovrà sostituire i seguenti blocchi `@font-face` con essi, e aggiornare i percorsi dei file dei font):

```css
@font-face {
  font-family: "handwriting";
  src:
    url("fonts/journal-webfont.woff2") format("woff2"),
    url("fonts/journal-webfont.woff") format("woff");
  font-weight: normal;
  font-style: normal;
}

@font-face {
  font-family: "typewriter";
  src:
    url("fonts/momot___-webfont.woff2") format("woff2"),
    url("fonts/momot___-webfont.woff") format("woff");
  font-weight: normal;
  font-style: normal;
}

body {
  font: 1.3rem sans-serif;
  padding: 0.5em;
  margin: 0;
  background: #222;
}

form {
  position: relative;
  width: 740px;
  height: 498px;
  margin: 0 auto;
  padding: 1em;
  box-sizing: border-box;
  background: #fff url(background.jpg);

  /* we create our grid */
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: 10em 1em 1em 1em;
}
```

Noti che abbiamo utilizzato alcuni [CSS grid](/it/docs/Web/CSS/CSS_grid_layout) e [Flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout) per disporre il modulo. Utilizzando questo possiamo facilmente posizionare i nostri elementi, incluso il titolo e tutti gli elementi del modulo:

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

Ora possiamo iniziare a lavorare sugli elementi del modulo stessi. Per prima cosa, assicuriamoci che i {{HTMLElement("label")}} ricevano il font corretto:

```css
label {
  font:
    0.8em "typewriter",
    sans-serif;
}
```

I campi di testo richiedono alcune regole comuni. In altre parole, rimuoviamo i loro {{cssxref("border","bordi")}} e {{cssxref("background","sfondi")}}, e rideterminiamo la loro {{cssxref("padding")}} e {{cssxref("margin")}}:

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

Quando uno di questi campi ottiene il focus, li evidenziamo con un leggero sfondo grigio, trasparente (è sempre importante avere stile di focus, per usabilità e accessibilità da tastiera):

```css
input:focus,
textarea:focus {
  background: rgb(0 0 0 / 10%);
  border-radius: 5px;
}
```

Ora che i nostri campi di testo sono completi, dobbiamo regolare la visualizzazione dei campi di testo a riga singola e multipla per adattarle, dato che tipicamente non sembreranno uguali utilizzando i valori predefiniti.

#### Regolazione delle aree di testo

Gli elementi {{HTMLElement("textarea")}} per impostazione predefinita sono resi come un elemento inline-block. Le due cose importanti qui sono le proprietà {{cssxref("resize")}} e {{cssxref("overflow")}}. Mentre il nostro design è un design a dimensione fissa, e potremmo utilizzare la proprietà `resize` per impedire agli utenti di ridimensionare il nostro campo di testo multiliena, è meglio non impedire agli utenti di ridimensionare un textarea se lo desiderano. La proprietà {{cssxref("overflow")}} è utilizzata per rendere il campo più coerente tra i browser. Alcuni browser impostano il valore predefinito su `auto`, mentre altri su `scroll`. Nel nostro caso, è meglio assicurarsi che tutti utilizzino `auto`:

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

#### Stilizzazione del pulsante di invio

L'elemento {{HTMLElement("button")}} è davvero conveniente da stilizzare con il CSS; può fare quello che vuole, anche utilizzando [pseudo-elementi](/it/docs/Web/CSS/Pseudo-elements):

```css
button {
  padding: 5px;
  font: bold 0.6em sans-serif;
  border: 2px solid #333;
  border-radius: 5px;
  background: none;
  cursor: pointer;
  transform: rotate(-1.5deg);
}

button:after {
  content: " >>>";
}

button:hover,
button:focus {
  background: #000;
  color: #fff;
}
```

### Il risultato finale

E voilà! Il suo modulo dovrebbe ora apparire così:

![L'aspetto finale e il layout del modulo dopo aver applicato tutto lo stile e averlo regolato come descritto sopra](updated-form-screenshot.jpg)

> [!NOTE]
> Se il suo esempio non funziona esattamente come si aspettava e vuole confrontarlo con la nostra versione, può trovarlo su GitHub — veda [l'esecuzione live](https://mdn.github.io/learning-area/html/forms/postcard-example/) (veda anche [il codice sorgente](https://github.com/mdn/learning-area/tree/main/html/forms/postcard-example)).

## Testi le sue competenze

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di aver conservato queste informazioni prima di continuare — veda [Testi le sue competenze: Le basi della stilizzazione](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Styling_basics).

## Riepilogo

Come può vedere, finché vogliamo costruire moduli con solo campi di testo e pulsanti, è semplice stilizzarli usando il CSS. [Nel prossimo articolo](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling), vedremo come gestire i widget del modulo che rientrano nelle categorie "brutti" e "cattivi".

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Other_form_controls","Learn_web_development/Extensions/Forms/Advanced_form_styling","Learn_web_development/Extensions/Forms")}}
