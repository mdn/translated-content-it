---
title: "CSS: dare stile ai contenuti"
short-title: Dare stile ai contenuti
slug: Learn_web_development/Getting_started/Your_first_website/Styling_the_content
l10n:
  sourceCommit: b5ee197a87ea18acbc4dd9544efa8c0e46253785
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Your_first_website")}}

CSS (Cascading Style Sheets) è il codice che applica lo stile ai contenuti web. Questo articolo offre una comprensione di base di CSS: come funziona e come migliorare l'aspetto e la sensazione della struttura dei contenuti creata nell'articolo precedente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, con il software di base che verrà utilizzato per creare un sito web e con i file system.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo e la funzione di CSS.</li>
          <li>Le parti fondamentali della sintassi CSS: insiemi di regole, selettori, dichiarazioni, proprietà, valori delle proprietà.</li>
          <li>Funzionalità CSS comuni, tra cui il box model, la modifica di colori e font e il posizionamento degli elementi HTML.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è CSS?

Come HTML, CSS non è un linguaggio di programmazione. Non è nemmeno un linguaggio di markup. **CSS è un linguaggio per fogli di stile.** CSS viene utilizzato per applicare stile agli elementi HTML: si selezionano gli elementi da stilizzare e si impostano valori per le relative proprietà di stile, che ne definiscono l'aspetto.

Rivediamo l'esempio HTML di base dall'articolo [Creare i contenuti](/it/docs/Learn_web_development/Getting_started/Your_first_website/Creating_the_content):

```html live-sample___basic-html live-sample___basic-css
<p>Instructions for life:</p>

<ul>
  <li>Eat</li>
  <li>Sleep</li>
  <li>Repeat</li>
</ul>
```

Da solo, viene renderizzato nel modo seguente:

{{EmbedLiveSample("basic-html", "100%", "140px")}}

Se si aggiunge del CSS, è possibile cambiare l'aspetto dell'HTML. Il frammento seguente seleziona l'elemento {{htmlelement("p")}} e gli assegna un [font](/it/docs/Web/CSS/Reference/Properties/font-family) diverso e un {{cssxref("color")}} del testo rosso. Quindi seleziona tutti gli elementi {{htmlelement("li")}} e assegna a ciascuno un {{cssxref("background-color")}} giallo-verde, un {{cssxref("border")}} nero solido di 1 pixel e un [margine inferiore](/it/docs/Web/CSS/Reference/Properties/margin-bottom) di 5 pixel:

```css live-sample___basic-css
p {
  font-family: sans-serif;
  color: red;
}

li {
  background-color: greenyellow;
  border: 1px solid black;
  margin-bottom: 5px;
}
```

Con il CSS applicato all'HTML, la demo viene ora renderizzata così:

{{EmbedLiveSample("basic-css", "100%", "160px")}}

Come si può vedere, con solo un po' di CSS è stato possibile modificare l'aspetto di un elenco dall'aspetto semplice.

CSS offre molte altre funzionalità, dalla specifica di immagini di sfondo e gradienti, al controllo della tipografia e del comportamento di scorrimento, fino all'aggiunta di animazioni e alla creazione di layout completi per pagine web.

## Applicare CSS all'HTML

Quando si utilizza CSS, la prima cosa da fare correttamente è assicurarsi che il CSS venga applicato correttamente all'HTML. In questa sezione verrà aggiunto un **foglio di stile** CSS a `first-website` e applicato alla pagina.

1. All'interno della cartella `first-website`, creare un'altra nuova cartella denominata `styles`.
2. Utilizzando un editor di testo, incollare il seguente CSS in un nuovo file, che assegnerà agli elementi `<p>` un colore del testo rosso. È utile iniziare con qualcosa di simile per verificare se il foglio di stile viene applicato correttamente all'HTML.

   ```css
   p {
     color: red;
   }
   ```

3. Salvare il file nella cartella `styles` con il nome file `style.css`.
4. Aprire il file `index.html`. Incollare la riga seguente all'interno dell'head HTML (tra i tag {{HTMLElement("head")}} e `</head>`):

   ```html
   <link href="styles/style.css" rel="stylesheet" />
   ```

5. Salvare `index.html` e caricarlo nel browser. Dovrebbe essere visualizzato qualcosa di simile:

![Un logo Mozilla e alcuni paragrafi. Il testo dei paragrafi è stato stilizzato in rosso dal nostro CSS.](website-screenshot-styled.png)

Se il testo dei paragrafi è rosso, congratulazioni! Il CSS funziona. In caso contrario, ripercorrere i passaggi precedenti e verificare attentamente di aver seguito correttamente ciascuno di essi.

## Nozioni di base sulla sintassi CSS

Nell'esempio CSS precedente, `p` è chiamato **selettore**: seleziona gli elementi a cui applicare lo stile. In particolare, `p` seleziona tutti i paragrafi nell'HTML. La riga all'interno delle parentesi graffe (`{ }`) è chiamata **dichiarazione**: imposta un valore per una proprietà specifica. In questo caso, la **proprietà** è `color`, che controlla il colore del testo dei paragrafi, e il **valore della proprietà** impostato è `red`.

L'intera struttura è chiamata **insieme di regole**. Il termine _ruleset_ viene spesso indicato semplicemente come _rule_.

Osserviamo un altro insieme di regole, questa volta con più dichiarazioni:

```css
p {
  color: red;
  width: 500px;
  border: 1px solid black;
}
```

All'interno di un insieme di regole, è necessario utilizzare un punto e virgola (`;`) per separare una dichiarazione dalla successiva. All'interno di ciascuna dichiarazione, è necessario utilizzare i due punti (`:`) per separare la proprietà dal relativo valore.

È inoltre possibile includere più selettori in una regola, separati da virgole, per selezionare più elementi. Per esempio:

```css
p,
.my-class,
#my-id {
  color: red;
}
```

In questa regola CSS è stato incluso un selettore di **elemento** (o di **tipo**), che seleziona uno specifico elemento HTML. Sono stati inclusi anche altri due tipi di selettore, che non sono rilevanti per il resto di questo tutorial. Per scoprire cosa fanno, consultare la nostra Guida ai [selettori di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors).

> [!NOTE]
> [Write your first lines of CSS!](https://scrimba.com/the-frontend-developer-career-path-c0j/~015?via=mdn) di Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre un'utile introduzione interattiva alla sintassi CSS.

## Migliorare il testo

Torniamo all'esempio e utilizziamo CSS per migliorare l'aspetto del testo. Verrà impostato un nuovo font per la pagina e verranno modificate alcune impostazioni del testo per diversi elementi.

1. Per prima cosa, trovare l'[output di Google Fonts](/it/docs/Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like#choosing_a_font) salvato in precedenza. Se non è stato ancora scelto un font, seguire il collegamento e farlo ora.
2. Aggiungere gli elementi {{htmlelement("link")}} all'interno di {{HTMLElement("head")}} di `index.html`, subito prima del tag di chiusura `</head>`. Dovrebbero avere un aspetto simile al seguente:

   ```html
   <link rel="preconnect" href="https://fonts.googleapis.com" />
   <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
   <link
     href="https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100..900;1,100..900&display=swap"
     rel="stylesheet" />
   ```

   Questo codice collega la pagina a un foglio di stile ospitato dal servizio Google Fonts, che carica il font scelto.

3. Successivamente, aprire il file `style.css` ed eliminare la regola esistente. Non si desidera più che i paragrafi siano rossi.
4. Aggiungere le righe seguenti a `style.css`:

   ```css
   html {
     /* px means "pixels". The base font size is now 10 pixels high */
     font-size: 10px;
     /* Replace PLACEHOLDER with the font-family property value you got from Google Fonts */
     font-family: PLACEHOLDER;
   }
   ```

   > [!NOTE]
   > Tutto ciò che in CSS si trova tra `/*` e `*/` è un **commento CSS**, che viene ignorato dal browser. I commenti CSS consentono di includere note utili sul codice o sulla logica, senza influire sul rendering della pagina web.

5. Sostituire la riga segnaposto `font-family` con la riga `font-family` del codice Google Fonts, per esempio:

   ```css
   font-family: "Roboto", sans-serif;
   ```

   La proprietà `font-family` imposta i font da applicare all'HTML. Questa regola definisce un font e una dimensione del font di base globali per l'intera pagina. Tutti gli elementi all'interno dell'elemento {{HTMLElement("html")}} erediteranno gli stessi `font-size` e `font-family`.

6. Ora impostiamo alcuni stili per font e testo sugli elementi [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements), {{htmlelement("li")}} e {{htmlelement("p")}}. Verranno impostati nuovi valori di {{cssxref("font-size")}} per ogni elemento. Verrà inoltre centrata l'intestazione con {{cssxref("text-align")}} e aumentati {{cssxref("line-height")}} e {{cssxref("letter-spacing")}} dei paragrafi e degli elementi dell'elenco per rendere il contenuto del corpo più leggibile.

   ```css
   h1 {
     font-size: 60px;
     text-align: center;
   }

   p,
   li {
     font-size: 16px;
     line-height: 2;
     letter-spacing: 1px;
   }
   ```

7. Salvare il codice e caricare l'HTML in un browser, aggiornando la pagina se era già aperta. Il lavoro in corso dovrebbe avere un aspetto simile a questo:

   ![Un logo Mozilla e alcuni paragrafi. È stato impostato un font sans-serif, sono state regolate le dimensioni del font, l'altezza della riga e la spaziatura tra le lettere e l'intestazione principale della pagina è stata centrata.](website-screenshot-font-small.png)

   > [!NOTE]
   > Provare a regolare i valori in `px` fino a ottenere dimensioni del font gradite per l'intestazione e il testo del corpo.

## CSS riguarda soprattutto i box

Utilizzando sempre più CSS, si noterà che gran parte di esso riguarda i box. La maggior parte degli elementi HTML in una pagina può essere considerata come box che si trovano sopra, o accanto, ad altri box. È possibile impostare valori su questi box per dimensioni, colore, posizionamento e così via. Questo è chiamato [**box model**](/it/docs/Learn_web_development/Core/Styling_basics/Box_model).

![Tre box uno dentro l'altro. Dall'esterno verso l'interno sono etichettati margin, border e padding.](box-model.png)

Ogni box che occupa spazio nella pagina possiede proprietà quali:

- {{cssxref("padding")}}: lo spazio intorno al contenuto. Nell'esempio precedente, è lo spazio intorno al testo del paragrafo.
- {{cssxref("border")}}: la linea solida immediatamente all'esterno del padding.
- {{cssxref("margin")}}: lo spazio esterno al border.

In questa sezione vengono utilizzate anche le seguenti proprietà, alcune delle quali già viste:

- {{cssxref("width")}}: la larghezza di un elemento.
- {{cssxref("background-color")}}: il colore dietro il contenuto e il padding di un elemento.
- {{cssxref("color")}}: il colore del contenuto di un elemento, generalmente il testo.
- {{cssxref("text-shadow")}}: un'ombra esterna sul testo all'interno di un elemento.
- {{cssxref("display")}}: la modalità di visualizzazione di un elemento, che sostanzialmente indica come appare o viene disposto nella pagina web.

In ciascuna delle sezioni seguenti:

1. Aggiungere il codice CSS fornito in fondo al file `style.css`.
2. Salvare il file e aggiornare il browser per vedere in che modo il CSS ha influenzato il rendering HTML.
3. Leggere la spiegazione fornita per comprendere il funzionamento del CSS.
4. Se si desidera sperimentare, provare a modificare i valori delle proprietà per personalizzare ulteriormente la pagina.

## Modificare il colore della pagina

Aggiungere quanto segue:

```css
html {
  background-color: #00539f;
}
```

Questa regola imposta un colore di sfondo per l'intera pagina. Modificare il codice colore con il colore scelto in [Come sarà il sito web?](/it/docs/Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like#choosing_a_theme_color).

## Dare stile al body

Successivamente, aggiungere questa regola:

```css
body {
  width: 600px;
  margin: 0 auto;
  background-color: #ff9500;
  padding: 0 20px 20px 20px;
  border: 5px solid black;
}
```

Il codice precedente imposta nuovi valori per diverse proprietà dell'elemento {{htmlelement("body")}}. Analizziamole riga per riga:

- `width: 600px;`: questo forza il body ad avere sempre una larghezza di 600 pixel.
- `margin: 0 auto;`: quando si impostano due valori su una proprietà come `margin` o `padding`, il primo valore influenza il lato superiore _e_ inferiore dell'elemento, impostandolo in questo caso a `0`; il secondo valore influenza il lato sinistro _e_ destro. `auto` è un valore speciale che divide uniformemente lo spazio orizzontale disponibile tra sinistra e destra.
- `background-color: #FF9500;`: questo imposta il colore di sfondo dell'elemento. Il progetto utilizza un arancione rossastro per il colore di sfondo di `<body>`, in contrasto con il blu scuro utilizzato per l'elemento {{htmlelement("html")}}.
- `padding: 0 20px 20px 20px;`: questo imposta quattro valori per il padding. L'obiettivo è inserire spazio intorno al contenuto. In questo esempio, non c'è padding nella parte superiore del body e ci sono 20 pixel a destra, in basso e a sinistra. I valori impostano il padding superiore, destro, inferiore e sinistro, in quest'ordine.
- `border: 5px solid black;`: questo imposta valori per larghezza, stile e colore del border. In questo caso, si tratta di un border nero solido, largo 5 pixel, attorno a tutti i lati del body.

### Nota sulle proprietà shorthand

I valori delle proprietà CSS che impostano più proprietà in una sola volta sono chiamati **proprietà shorthand**. Ad esempio, `padding: 0 20px 20px 20px` è equivalente alle quattro proprietà seguenti:

```css
padding-top: 0;
padding-right: 20px;
padding-bottom: 20px;
padding-left: 20px;
```

> [!NOTE]
> [Margin/padding shorthand](https://scrimba.com/frontend-path-c0j/~0g?via=mdn) di Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> è una lezione interattiva che offre una panoramica pratica dell'uso delle forme shorthand di margin e padding.

## Posizionare e stilizzare il titolo principale della pagina

Ora aggiungere questo:

```css
h1 {
  margin: 0;
  padding: 20px 0;
  color: #00539f;
  text-shadow: 3px 3px 1px black;
}
```

Potrebbe essere stato notato un terribile spazio vuoto nella parte superiore del body. Ciò accade perché i browser applicano uno stile predefinito all'elemento `<h1>`. Potrebbe sembrare una cattiva idea, ma l'obiettivo è fornire una leggibilità di base alle pagine senza stile. Per eliminare lo spazio, si sovrascrive lo stile predefinito del browser con l'impostazione `margin: 0;`.

Successivamente, si imposta il padding superiore e inferiore dell'intestazione a 20 pixel e il testo dell'intestazione allo stesso colore del colore di sfondo HTML.

Infine, `text-shadow` applica un'ombra al contenuto testuale dell'elemento:

- Il primo valore in pixel imposta l'**offset orizzontale** dell'ombra dal testo: quanto si sposta orizzontalmente.
- Il secondo valore in pixel imposta l'**offset verticale** dell'ombra dal testo: quanto si sposta verso il basso.
- Il terzo valore in pixel imposta il **raggio di sfocatura** dell'ombra. Un valore più grande produce un'ombra dall'aspetto più sfumato.
- Il quarto valore imposta il colore di base dell'ombra.

## Centrare l'immagine

Infine, inserire questa regola:

```css
img {
  display: block;
  margin: 0 auto;
  max-width: 100%;
}
```

Successivamente, l'immagine viene centrata per migliorarne l'aspetto. Si può utilizzare lo stesso trucco `margin: 0 auto` usato per il body, ma esistono differenze che richiedono un'impostazione aggiuntiva affinché il CSS funzioni.

L'elemento {{htmlelement("body")}} è un elemento **block**, ovvero occupa spazio nella pagina e può accettare margin, padding e altre proprietà del box. Gli elementi {{htmlelement("img")}} (immagine), invece, sono elementi **inline**: per impostazione predefinita, non accettano valori di margin nello stesso modo degli elementi block. Per far funzionare il trucco del margine automatico su questa immagine, è necessario attribuirle un comportamento a livello di blocco utilizzando `display: block;`.

Infine, la proprietà {{cssxref("max-width")}} viene impostata su `100%` per garantire che, se l'immagine è più grande della `width` impostata sul body, ovvero 600 pixel, venga vincolata a `600px` e non si estenda oltre.

> [!NOTE]
> Non è necessario preoccuparsi troppo se `display: block;`, le differenze tra un elemento block e un elemento inline, o `max-width: 100%;` non sono completamente chiari. Diventeranno più comprensibili continuando lo studio di CSS.

## Conclusione

Seguendo tutte le istruzioni di questo articolo, si dovrebbe ottenere una pagina simile alla seguente:

![Un logo Mozilla centrato, un'intestazione e paragrafi. Ora appare ben stilizzata, con uno sfondo blu per l'intera pagina e uno sfondo arancione per la striscia di contenuto principale centrata.](website-screenshot-final.png)

È possibile [visualizzare la nostra versione qui](https://mdn.github.io/beginner-html-site-styled/). In caso di difficoltà, è sempre possibile confrontare il proprio lavoro con il [codice dell'esempio completato su GitHub](https://github.com/mdn/beginner-html-site-styled/blob/main/styles/style.css).

In questo articolo è stata solo sfiorata la superficie di CSS. Molto altro verrà appreso nel modulo Core [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics), più avanti nel corso.

## Vedi anche

- [Impara HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn HTML and CSS_ di [Scrimba](https://scrimba.com?via=mdn) insegna HTML e CSS attraverso la creazione e la pubblicazione di cinque fantastici progetti, con lezioni interattive e sfide divertenti tenute da insegnanti esperti.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Your_first_website")}}
