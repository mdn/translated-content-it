---
title: Stilizzazione dei moduli web
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
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS Styling basics</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere le problematiche legate alla stilizzazione dei moduli, e apprendere alcune delle
        tecniche di base di stilizzazione che saranno utili allo sviluppatore.
      </td>
    </tr>
  </tbody>
</table>

## Sfide nella stilizzazione dei widget di modulo

### Storia

Nel 1995, [la specifica HTML 2](https://datatracker.ietf.org/doc/html/rfc1866) ha introdotto i controlli dei moduli (anche noti come "widget di modulo" o "elementi di modulo"). Ma CSS non è stato rilasciato fino alla fine del 1996 e non è stato supportato dalla maggior parte dei browser per anni; quindi, nel frattempo, i browser si affidavano al sistema operativo sottostante per rendere i widget di modulo.

Anche con CSS disponibile, i fornitori di browser erano inizialmente riluttanti a rendere stilizzabili gli elementi del modulo, poiché gli utenti erano così abituati all'aspetto dei rispettivi browser. Ma le cose sono cambiate, e i widget di modulo sono ora per lo più stilizzabili, con alcune eccezioni.

### Tipi di widget

#### Facili da stilizzare

1. {{HTMLElement("form")}}
2. {{HTMLElement("fieldset")}} e {{HTMLElement("legend")}}
3. {{HTMLElement("input")}} a linea singola (ad esempio, tipo text, url, email), eccetto per [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search).
4. Multi-linea {{HTMLElement("textarea")}}
5. Pulsanti (sia {{HTMLElement("input")}} che {{HTMLElement("button")}})
6. {{HTMLElement("label")}}
7. {{HTMLElement("output")}}

#### Più difficili da stilizzare

- Checkbox e pulsanti radio
- [`<input type="search">`](/it/docs/Web/HTML/Reference/Elements/input/search)

L'articolo [Stilizzazione avanzata dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) mostra come stilizzarli.

#### Che hanno elementi interni non stilizzabili solo con CSS

- [`<input type="color">`](/it/docs/Web/HTML/Reference/Elements/input/color)
- Controlli relativi alla data come [`<input type="datetime-local">`](/it/docs/Web/HTML/Reference/Elements/input/datetime-local)
- [`<input type="range">`](/it/docs/Web/HTML/Reference/Elements/input/range)
- [`<input type="file">`](/it/docs/Web/HTML/Reference/Elements/input/file)
- Elementi coinvolti nella creazione di widget a discesa, inclusi {{HTMLElement("select")}}, {{HTMLElement("option")}}, {{HTMLElement("optgroup")}} e {{HTMLElement("datalist")}}.
  > [!NOTE]
  > Alcuni browser ora supportano [Elementi select personalizzabili](/it/docs/Learn_web_development/Extensions/Forms/Customizable_select), un insieme di funzionalità HTML e CSS che insieme permettono una completa personalizzazione degli elementi `<select>` e dei loro contenuti proprio come qualsiasi normale elemento DOM.
- {{HTMLElement("progress")}} e {{HTMLElement("meter")}}

Per esempio, il calendario del selettore data e il pulsante su \<select> che visualizza un elenco di opzioni quando cliccato, non possono essere stilizzati utilizzando solo CSS.

Gli articoli [Stilizzazione avanzata dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) e [Come costruire controlli personalizzati per i moduli](/it/docs/Learn_web_development/Extensions/Forms/How_to_build_custom_form_controls) descrivono come stilizzare questi elementi.

> [!NOTE]
> Alcuni pseudo-elementi CSS proprietari, come {{cssxref('::-moz-range-track')}}, sono capaci di stilizzare tali componenti interni, ma non sono coerenti tra i browser, quindi non sono molto affidabili. Li menzioneremo più avanti.

## Stilizzazione dei widget di modulo semplici

I widget "facili da stilizzare" nella sezione precedente possono essere stilizzati utilizzando le tecniche degli articoli [Il tuo primo modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form) e [CSS building blocks](/it/docs/Learn_web_development/Core/Styling_basics). Ci sono anche selettori speciali — [pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes) — che permettono la stilizzazione basata sullo stato corrente dell'interfaccia utente.

Passeremo attraverso un esempio alla fine di questo articolo — ma prima, ecco alcuni aspetti speciali della stilizzazione dei moduli che vale la pena conoscere.

### Font e testo

Le funzionalità CSS relative ai font e al testo possono essere utilizzate facilmente con qualsiasi widget (e sì, puoi usare {{cssxref("@font-face")}} con i widget di modulo). Tuttavia, il comportamento del browser è spesso incoerente. Di default, alcuni widget non ereditano {{cssxref("font-family")}} e {{cssxref("font-size")}} dai loro genitori. Molti browser usano invece l'aspetto predefinito del sistema. Per rendere l'aspetto dei tuoi moduli coerente con il resto del tuo contenuto, puoi aggiungere le seguenti regole al tuo foglio di stile:

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

Gli screenshot qui sotto mostrano la differenza. A sinistra c'è la resa predefinita di un `<input type="text">`, `<input type="date">`, {{htmlelement('select')}}, {{htmlelement('textarea')}}, `<input type="submit">`, e un `<button>` in Chrome su macOS, con lo stile del font predefinito della piattaforma. A destra ci sono gli stessi elementi, con la nostra regola di stile sopra applicata.

![Controlli dei moduli con famiglie di font predefinite e ereditate. Di default, alcuni tipi sono serif e altri sono sans serif. L'ereditarietà dovrebbe cambiare i font di tutti alla famiglia di font del genitore - in questo caso un paragrafo. Stranamente, input di tipo submit non eredita dal paragrafo genitore.](forms_fontfamily.png)

I valori predefiniti differiscono in molti modi. L'ereditarietà dovrebbe cambiare i loro font in quella della famiglia di font del genitore — in questo caso, il font serif predefinito del contenitore genitore. Tutti lo fanno, con un'eccezione strana — `<input type="submit">` non eredita dal paragrafo genitore in Chrome. Piuttosto, usa il {{cssxref('font-family#Values', 'font-family: system-ui')}}. Questo è un altro motivo per utilizzare elementi `<button>` rispetto ai loro tipi di input equivalenti!

C'è molto dibattito sul fatto che i moduli sembrino migliori utilizzando gli stili predefiniti del sistema o stili personalizzati progettati per adattarsi al tuo contenuto. Questa decisione spetta a te, come designer del tuo sito o applicazione web.

### Dimensioni del box

Tutti i campi di testo supportano completamente ogni proprietà relativa al modello di box CSS, come {{cssxref("width")}}, {{cssxref("height")}}, {{cssxref("padding")}}, {{cssxref("margin")}}, e {{cssxref("border")}}. Tuttavia, come prima, i browser si affidano agli stili predefiniti del sistema quando visualizzano questi widget. Spetta a te definire come desideri integrarli nel tuo contenuto. Se desideri mantenere l'aspetto e la sensazione nativa dei widget, incontrerai qualche difficoltà se desideri dare loro una dimensione coerente.

**Questo perché ogni widget ha le proprie regole per bordi, padding e margini.** Per dare la stessa dimensione a più widget diversi, puoi usare la proprietà {{cssxref("box-sizing")}} insieme a valori coerenti per altre proprietà:

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

Nello screenshot sottostante, la colonna di sinistra mostra la resa predefinita di un `<input type="radio">`, `<input type="checkbox">`, `<input type="range">`, `<input type="text">`, `<input type="date">`, {{htmlelement('select')}}, {{htmlelement('textarea')}}, `<input type="submit">`, e {{htmlelement('button')}}. La colonna di destra invece mostra gli stessi elementi con la nostra regola sopra applicata a loro. Nota come ciò ci consenta di garantire che tutti gli elementi occupino la stessa quantità di spazio, nonostante le regole predefinite della piattaforma per ogni tipo di widget.

![Le proprietà del modello di box influiscono sulla maggior parte dei tipi di input.](boxmodel_formcontrols1.png)

Ciò che potrebbe non essere evidente tramite lo screenshot è che i controlli di radio e checkbox sembrano ancora uguali, ma sono centrati nei 150 px di spazio orizzontale forniti dalla proprietà {{cssxref('width')}}. Altri browser potrebbero non centrare i widget, ma aderiscono allo spazio assegnato.

### Posizionamento della legenda

L'elemento {{HTMLElement("legend")}} è ok da stilizzare, ma può essere un po' complicato controllarne il posizionamento. Di default, è sempre posizionato sopra il bordo superiore del suo genitore {{HTMLElement("fieldset")}}, vicino all'angolo in alto a sinistra. Per posizionarlo da qualche altra parte, per esempio all'interno del fieldset da qualche parte, o vicino all'angolo in basso a sinistra, devi affidarti al posizionamento.

Prendi il seguente esempio:

{{EmbedGHLiveSample("learning-area/html/forms/native-form-widgets/positioned-legend.html", '100%', 400)}}

Per posizionare la legenda in questo modo, abbiamo utilizzato il seguente CSS (altre dichiarazioni rimosse per brevità):

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

Anche il `<fieldset>` deve essere posizionato, in modo che il `<legend>` sia posizionato relativo ad esso (altrimenti il `<legend>` sarebbe posizionato relativo a `<body>`).

L'elemento {{HTMLElement("legend")}} è molto importante per l'accessibilità — sarà pronunciato dalle tecnologie assistive come parte dell'etichetta di ogni elemento del modulo all'interno del fieldset — ma l'utilizzo di una tecnica come quella sopra è accettabile. I contenuti della legenda saranno ancora pronunciati nello stesso modo; è solo la posizione visiva che è cambiata.

> [!NOTE]
> Potresti anche utilizzare la proprietà {{cssxref("transform")}} per aiutarti a posizionare il tuo `<legend>`. Tuttavia, quando lo posizioni per esempio con un `transform: translateY();`, si sposta ma lascia un brutto gap nel bordo del `<fieldset>`, che non è facile da eliminare.

## Un esempio specifico di stilizzazione

Diamo un'occhiata a un esempio concreto di come stilizzare un modulo HTML. Costruiremo un modulo di contatto a forma di "cartolina" dall'aspetto elegante; [vedi qui la versione finale](https://mdn.github.io/learning-area/html/forms/postcard-example/).

Se vuoi seguire l'esempio, effettua una copia locale del nostro [file postcard-start.html](https://github.com/mdn/learning-area/blob/main/html/forms/postcard-example/postcard-start.html), e segui le istruzioni sotto.

### L'HTML

L'HTML è solo leggermente più complesso dell'esempio che abbiamo utilizzato in [Il tuo primo modulo](/it/docs/Learn_web_development/Extensions/Forms/Your_first_form); ha solo qualche ID extra e un'intestazione.

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

Aggiungi il codice sopra nel corpo del tuo HTML.

### Organizzare le tue risorse

Qui inizia il divertimento! Prima di iniziare a codificare, abbiamo bisogno di tre risorse aggiuntive:

1. [Lo sfondo della cartolina](https://github.com/mdn/learning-area/blob/main/html/forms/postcard-example/background.jpg) — scarica questa immagine e salvala nella stessa directory del tuo file HTML di lavoro.
2. Un font da macchina da scrivere: [Il font "Mom's Typewriter" da dafont.com](https://www.dafont.com/moms-typewriter.font?back=theme) — scarica il file TTF nella stessa directory di prima.
3. Un font disegnato a mano: [Il font "Journal" da dafont.com](https://www.dafont.com/journal.font) — scarica il file TTF nella stessa directory di prima.

I tuoi font richiedono un po' più di elaborazione prima di iniziare:

1. Vai al [Generatore di Webfont](https://www.fontsquirrel.com/tools/webfont-generator) di fontsquirrel.com.
2. Utilizzando il modulo, carica entrambi i tuoi file font e genera un kit di webfont. Scarica il kit sul tuo computer.
3. Decomprimi il file zip fornito.
4. All'interno dei contenuti decompressi troverai alcuni file font (al momento della scrittura, due file `.woff` e due file `.woff2`; potrebbero variare in futuro.) Copia questi file in una directory chiamata fonts, nella stessa directory di prima. Stiamo usando due file diversi per ogni font per massimizzare la compatibilità tra browser; consulta il nostro articolo [Web fonts](/it/docs/Learn_web_development/Core/Text_styling/Web_fonts) per molte più informazioni.

### Il CSS

Ora possiamo immergerci nel CSS per l'esempio. Aggiungi tutti i blocchi di codice mostrati sotto all'interno dell'elemento {{htmlelement("style")}}, uno dopo l'altro.

#### Layout generale

Per prima cosa, ci prepariamo definendo le nostre regole {{cssxref("@font-face")}}, e tutti gli stili di base impostati sugli elementi {{HTMLElement("body")}} e {{HTMLElement("form")}}. Se l'output di fontsquirrel fosse diverso da quello descritto sopra, puoi trovare i blocchi `@font-face` corretti all'interno del tuo kit di webfont scaricato, nel file `stylesheet.css` (dovrai sostituire i blocchi `@font-face` qui sotto con quelli, e aggiornare i percorsi ai file font):

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

Nota che abbiamo usato un po' di [CSS grid](/it/docs/Web/CSS/CSS_grid_layout) e [Flexbox](/it/docs/Web/CSS/CSS_flexible_box_layout) per impaginare il modulo. Utilizzando questo possiamo posizionare facilmente i nostri elementi, incluso il titolo e tutti gli elementi del modulo:

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

Ora possiamo iniziare a lavorare sugli elementi del modulo stessi. Per prima cosa, assicuriamoci che i {{HTMLElement("label")}} siano dotati del font giusto:

```css
label {
  font:
    0.8em "typewriter",
    sans-serif;
}
```

I campi di testo richiedono alcune regole comuni. In altre parole, rimuoviamo i loro {{cssxref("border","bordi")}} e {{cssxref("background","sfondi")}}, e ridefiniamo i loro {{cssxref("padding")}} e {{cssxref("margin")}}:

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

Quando uno di questi campi guadagna il fuoco, li evidenziamo con un leggero sfondo grigio, trasparente (è sempre importante avere uno stile di fuoco, per l'usabilità e l'accessibilità della tastiera):

```css
input:focus,
textarea:focus {
  background: rgb(0 0 0 / 10%);
  border-radius: 5px;
}
```

Ora che i nostri campi di testo sono completi, dobbiamo regolare la visualizzazione dei campi di testo a linea singola e multipla per farli corrispondere, poiché non sembreranno tipicamente uguali utilizzando i predefiniti.

#### Regolazione delle aree di testo

Gli elementi {{HTMLElement("textarea")}} sono renderizzati di default come elementi inline-block. Le due cose importanti qui sono le proprietà {{cssxref("resize")}} e {{cssxref("overflow")}}. Anche se il nostro design è un design a dimensione fissa, e potremmo usare la proprietà `resize` per impedire agli utenti di ridimensionare il nostro campo di testo multi-linea, è meglio non impedire agli utenti di ridimensionare una textarea se lo desiderano. La proprietà {{cssxref("overflow")}} è usata per rendere il campo più coerente attraverso i browser. Alcuni browser predefiniscono il valore come `auto`, mentre altri come `scroll`. Nel nostro caso, è meglio essere sicuri che tutti usino `auto`:

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

L'elemento {{HTMLElement("button")}} è davvero conveniente da stilizzare con CSS; puoi fare quello che vuoi, utilizzando anche [pseudo-elementi](/it/docs/Web/CSS/Pseudo-elements):

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

E voilà! Il tuo modulo dovrebbe ora apparire così:

![L'aspetto finale e il layout del modulo dopo aver applicato tutto lo stile e le regolazioni come descritto sopra](updated-form-screenshot.jpg)

> [!NOTE]
> Se il tuo esempio non funziona come ti aspettavi e vuoi confrontarlo con la nostra versione, puoi trovarlo su GitHub — vedi [in esecuzione live](https://mdn.github.io/learning-area/html/forms/postcard-example/) (vedi anche [il codice sorgente](https://github.com/mdn/learning-area/tree/main/html/forms/postcard-example)).

## Metti alla prova le tue capacità

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare che hai mantenuto queste informazioni prima di passare oltre — vedi [Test your skills: Styling basics](/it/docs/Learn_web_development/Extensions/Forms/Test_your_skills/Styling_basics).

## Riepilogo

Come puoi vedere, purché vogliamo costruire moduli con solo campi di testo e pulsanti, è facile stilizzarli utilizzando CSS. [Nel prossimo articolo](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling), vedremo come gestire i widget di modulo che rientrano nelle categorie "cattivi" e "brutti".

{{PreviousMenuNext("Learn_web_development/Extensions/Forms/Other_form_controls","Learn_web_development/Extensions/Forms/Advanced_form_styling","Learn_web_development/Extensions/Forms")}}
