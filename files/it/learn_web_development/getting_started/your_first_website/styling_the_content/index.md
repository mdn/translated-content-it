---
title: "CSS: Styling the content"
short-title: Styling the content
slug: Learn_web_development/Getting_started/Your_first_website/Styling_the_content
l10n:
  sourceCommit: c5c84b62f3f1fbd46f77c940fa0cbfff649c46a1
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Your_first_website")}}

CSS (Cascading Style Sheets) è il codice che stila il contenuto web. Questo articolo ti guida attraverso una comprensione di base di CSS — come funziona e come migliorare l'aspetto e la sensazione della struttura del contenuto che hai creato nell'articolo precedente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del tuo computer, il software di base che userai per costruire un sito web e i sistemi di file.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo e la funzione di CSS.</li>
          <li>Le parti base della sintassi CSS — regole, selettori, dichiarazioni, proprietà, valori delle proprietà.</li>
          <li>Funzionalità comuni di CSS, inclusi modello di box, cambiamento di colori e font, e posizionamento degli elementi HTML.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è CSS?

Come HTML, CSS non è un linguaggio di programmazione. Neanche un linguaggio di markup. **CSS è un linguaggio di foglio di stile.** CSS è usato per stilare gli elementi HTML: selezioni gli elementi che vuoi stilare e imposti i valori per le loro proprietà di stile, che definiscono come appariranno.

Rivediamo l'esempio di base HTML dall'articolo [Creare il contenuto](/it/docs/Learn_web_development/Getting_started/Your_first_website/Creating_the_content):

```html live-sample___basic-html live-sample___basic-css
<p>Instructions for life:</p>

<ul>
  <li>Eat</li>
  <li>Sleep</li>
  <li>Repeat</li>
</ul>
```

Questo rende come segue da solo:

{{EmbedLiveSample("basic-html", "100%", "140px")}}

Se aggiungiamo un po' di CSS, possiamo cambiare l'aspetto dell'HTML. Il seguente frammento seleziona l'elemento {{htmlelement("p")}} e gli dà un diverso [font](/it/docs/Web/CSS/font-family) e un testo rosso {{cssxref("color")}}. Poi seleziona tutti gli elementi {{htmlelement("li")}} e dà a ciascuno un {{cssxref("background-color")}} verde-giallo, un {{cssxref("border")}} solido nero di 1 pixel, e un margine inferiore di 5 pixel [bottom margin](/it/docs/Web/CSS/margin-bottom):

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

Con il CSS applicato all'HTML, la demo ora rende così:

{{EmbedLiveSample("basic-css", "100%", "160px")}}

Come puoi vedere, con solo un po' di CSS, siamo stati in grado di cambiare l'aspetto di una lista dall'aspetto semplice.

CSS ha molte altre funzioni, dalla specificazione di immagini di sfondo e gradienti al controllo della tipografia e del comportamento dello scorrimento, fino all'aggiunta di animazioni e alla costruzione di interi layout di pagine web.

## Applicare CSS al tuo HTML

Quando usi CSS, la prima cosa da fare è assicurarti che il tuo CSS sia applicato correttamente al tuo HTML. In questa sezione, aggiungeremo un **foglio di stile** CSS al tuo `first-website` e lo applicheremo alla tua pagina.

1. All'interno della tua cartella `first-website`, crea un'altra nuova cartella chiamata `styles`.
2. Usando un editor di testo, incolla il seguente CSS in un nuovo file, che darà ai tuoi elementi `<p>` un colore del testo rosso. È utile iniziare con qualcosa del genere per verificare se il foglio di stile viene applicato correttamente al tuo HTML.

   ```css
   p {
     color: red;
   }
   ```

3. Salva il file nella cartella `styles` con il nome `style.css`.
4. Apri il tuo file `index.html`. Incolla la seguente riga all'interno dell'intestazione HTML (tra i tag {{HTMLElement("head")}} e `</head>`):

   ```html
   <link href="styles/style.css" rel="stylesheet" />
   ```

5. Salva `index.html` e caricalo nel tuo browser. Dovresti vedere qualcosa del genere:

![Un logo Mozilla e alcuni paragrafi. Il testo del paragrafo è stato stilizzato in rosso dal nostro css.](website-screenshot-styled.png)

Se il testo dei tuoi paragrafi è rosso, congratulazioni! Il tuo CSS funziona. In caso contrario, ripeti i passaggi sopra e controlla attentamente che li tu abbia seguiti correttamente.

## Nozioni di base sulla sintassi CSS

Nel precedente esempio CSS, `p` è chiamato un **selettore** — seleziona l'elemento/i da stilare. In particolare, `p` seleziona tutti i paragrafi nell'HTML. La linea all'interno delle parentesi graffe (`{ }`) è chiamata una **dichiarazione** – imposta un valore per una proprietà specifica. In questo caso, la **proprietà** è `color`, che controlla il colore del testo dei paragrafi, e il **valore della proprietà** impostato è `red`.

L'intera struttura è chiamata un **set di regole**. (Il termine _set di regole_ è spesso indicato semplicemente come _regola_.)

Esaminiamo un altro set di regole, questa volta con più dichiarazioni:

```css
p {
  color: red;
  width: 500px;
  border: 1px solid black;
}
```

All'interno di un set di regole, devi usare un punto e virgola (`;`) per separare una dichiarazione dall'altra. All'interno di ogni dichiarazione, devi usare due punti (`:`) per separare la proprietà e il suo valore.

Puoi anche includere più selettori in una regola, separati da virgole, per selezionare più elementi. Ad esempio:

```css
p,
.my-class,
#my-id {
  color: red;
}
```

In questa regola CSS, abbiamo incluso un selettore **elemento** (o **tipo**), che seleziona un elemento HTML specifico. Abbiamo anche incluso altri due tipi di selettori, che non sono rilevanti per il resto di questo tutorial. Se sei curioso di sapere cosa fanno, dai un'occhiata alla nostra [Guida ai selettori di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors).

> [!NOTE]
> Scrimba's [Scrivi le tue prime righe di CSS!](https://scrimba.com/the-frontend-developer-career-path-c0j/~015?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce un'introduzione interattiva utile alla sintassi CSS.

## Migliorare il testo

Ritorniamo al nostro esempio e usiamo CSS per migliorare l'aspetto del testo. Imposteremo un nuovo font per la pagina e cambieremo alcune impostazioni del testo per diversi elementi.

1. Prima, trova l'[output di Google Fonts](/it/docs/Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like#choosing_a_font) che hai salvato in precedenza. Se non hai ancora scelto un font, segui il link e fallo ora.
2. Aggiungi gli elementi {{htmlelement("link")}} all'interno della sezione {{HTMLElement("head")}} di `index.html`, appena prima del tag di chiusura `</head>`. Dovrebbero apparire in questo modo:

   ```html
   <link rel="preconnect" href="https://fonts.googleapis.com" />
   <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
   <link
     href="https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100..900;1,100..900&display=swap"
     rel="stylesheet" />
   ```

   Questo codice collega la tua pagina a un foglio di stile ospitato dal servizio Google Fonts, che carica il font scelto.

3. Successivamente, vai al tuo file `style.css` e elimina la regola esistente. Non vogliamo più che i nostri paragrafi siano rossi.
4. Aggiungi le seguenti righe a `style.css`:

   ```css
   html {
     /* px means "pixels". The base font size is now 10 pixels high */
     font-size: 10px;
     /* Replace PLACEHOLDER with the font-family property value you got from Google Fonts */
     font-family: PLACEHOLDER;
   }
   ```

   > [!NOTE]
   > Qualsiasi cosa in CSS tra `/*` e `*/` è un **commento CSS**, che viene ignorato dal browser. I commenti CSS sono un modo per includere note utili sul tuo codice o logica, senza influenzare l'aspetto della tua pagina web.

5. Sostituisci la riga del segnaposto `font-family` con la riga `font-family` dal tuo codice Google Fonts, ad esempio:

   ```css
   font-family: "Roboto", sans-serif;
   ```

   La proprietà `font-family` imposta il font (o i font) che vuoi applicare al tuo HTML. Questa regola definisce un font base globale e una dimensione del font per l'intera pagina. Tutti gli elementi all'interno dell'elemento {{HTMLElement("html")}} erediteranno la stessa dimensione del font (`font-size`) e il font (`font-family`).

6. Ora imposteremo alcuni stili di font e testo sui nostri elementi [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements), {{htmlelement("li")}}, e {{htmlelement("p")}}. Imposteremo nuovi valori di {{cssxref("font-size")}} per ciascun elemento. Inoltre, centreremo l'intestazione usando {{cssxref("text-align")}} e aumenteremo l'{{cssxref("line-height")}} e lo {{cssxref("letter-spacing")}} dei paragrafi e degli elementi lista per rendere il contenuto del corpo più leggibile.

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

7. Salva il tuo codice e carica il tuo HTML in un browser (aggiorna la pagina se l'hai aperta in precedenza). Il tuo lavoro in corso dovrebbe apparire simile a questo:

   ![Un logo Mozilla e alcuni paragrafi. È stato impostato un font sans-serif, le dimensioni dei font, l'altezza delle righe e lo spazio tra le lettere sono stati regolati, e l'intestazione principale della pagina è stata centrata](website-screenshot-font-small.png)

   > [!NOTE]
   > Prova a regolare i valori `px` fino a ottenere dimensioni di font che ti piacciono per il tuo testo di intestazione e corpo.

## CSS è tutto su box

Qualcosa che noterai su CSS man mano che lo utilizzi di più è che molto riguarda i box. La maggior parte degli elementi HTML su una pagina può essere pensata come box che si trovano su (o accanto) ad altri box. Puoi impostare valori su questi box per dimensione, colore, posizionamento, ecc. Questo è chiamato [**modello di box**](/it/docs/Learn_web_development/Core/Styling_basics/Box_model).

![Tre box annidati uno dentro l'altro. Dall'esterno all'interno sono etichettati margin, border e padding](box-model.png)

Ogni box che occupa spazio sulla tua pagina ha proprietà come:

- {{cssxref("padding")}}: Lo spazio intorno al contenuto. Nell'esempio precedente, è lo spazio intorno al testo del paragrafo.
- {{cssxref("border")}}: La linea solida appena fuori dal padding.
- {{cssxref("margin")}}: Lo spazio al di fuori del bordo.

In questa sezione, usiamo anche le seguenti proprietà, alcune delle quali hai già visto:

- {{cssxref("width")}}: La larghezza di un elemento.
- {{cssxref("background-color")}}: Il colore dietro il contenuto e il padding di un elemento.
- {{cssxref("color")}}: Il colore del contenuto di un elemento (solitamente il testo).
- {{cssxref("text-shadow")}}: Un'ombra al testo all'interno di un elemento.
- {{cssxref("display")}}: La modalità di visualizzazione di un elemento (che si riferisce fondamentalmente a come appare o è disposto sulla pagina web).

In ciascuna delle sezioni che seguono:

1. Aggiungi il codice CSS fornito in fondo al tuo file `style.css`.
2. Salva il file e aggiorna il tuo browser per vedere come il CSS ha influenzato il rendering dell'HTML.
3. Leggi la spiegazione fornita per aiutarti a capire come il CSS funziona.
4. Se ti senti avventuroso, sperimenta il cambiamento dei valori di proprietà per personalizzare ulteriormente la tua pagina.

## Cambiare il colore della pagina

```css
html {
  background-color: #00539f;
}
```

Questa regola imposta un colore di sfondo per l'intera pagina. Cambia il codice del colore con quello scelto in [Com'è fatto il tuo sito web?](/it/docs/Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like#choosing_a_theme_color).

## Stilizzare il corpo

```css
body {
  width: 600px;
  margin: 0 auto;
  background-color: #ff9500;
  padding: 0 20px 20px 20px;
  border: 5px solid black;
}
```

Il codice sopra imposta nuovi valori per diverse proprietà dell'elemento {{htmlelement("body")}}. Esaminiamole riga per riga:

- `width: 600px;`: Questo forza il corpo ad avere sempre una larghezza di 600 pixel.
- `margin: 0 auto;`: Quando imposti due valori su una proprietà come `margin` o `padding`, il primo valore influenza il lato superiore e inferiore dell'elemento (impostandolo su `0` in questo caso); il secondo valore influenza il lato sinistro e destro. `auto` è un valore speciale che divide uniformemente lo spazio orizzontale disponibile tra sinistra e destra.
- `background-color: #FF9500;`: Questo imposta il colore di sfondo dell'elemento. Il nostro progetto utilizza un arancione rossastro per il colore di sfondo del `<body>` per contrastare con il blu scuro usato per l'elemento {{htmlelement("html")}}.
- `padding: 0 20px 20px 20px;`: Questo imposta quattro valori per il padding. L'obiettivo è mettere dello spazio intorno al contenuto. In questo esempio, non c'è padding nella parte superiore del corpo, e 20 pixel a destra, in basso e a sinistra. I valori impostano l'imbottitura superiore, destra, inferiore e sinistra in quell'ordine.
- `border: 5px solid black;`: Questo imposta valori per la larghezza, lo stile e il colore del bordo. In questo caso, è un bordo solido nero largo 5 pixel attorno a tutti i lati del corpo.

## Posizionare e stilizzare il titolo principale della pagina

```css
h1 {
  margin: 0;
  padding: 20px 0;
  color: #00539f;
  text-shadow: 3px 3px 1px black;
}
```

Potresti aver notato un brutto spazio in alto nel corpo. Ciò accade perché i browser applicano uno stile di default all'elemento `<h1>`. Potrebbe sembrare una cattiva idea, ma l'obiettivo è fornire una leggibilità di base per le pagine non stilizzate. Per eliminare lo spazio, sovrascriviamo lo stile predefinito del browser con l'impostazione `margin: 0;`.

Successivamente, impostiamo l'imbottitura superiore e inferiore dell'intestazione su 20 pixel e impostiamo il testo dell'intestazione per avere lo stesso colore dello sfondo HTML.

Infine, `text-shadow` applica un'ombra al contenuto di testo dell'elemento:

- Il primo valore in pixel imposta l'**offset orizzontale** dell'ombra dal testo: quanto si sposta lungo.
- Il secondo valore in pixel imposta l'**offset verticale** dell'ombra dal testo: quanto si sposta giù.
- Il terzo valore in pixel imposta il **raggio di sfocatura** dell'ombra. Un valore più grande produce un'ombra dall'aspetto più sfocato.
- Il quarto valore imposta il colore di base dell'ombra.

## Centrare l'immagine

```css
img {
  display: block;
  margin: 0 auto;
  max-width: 100%;
}
```

Successivamente, facciamo in modo che l'immagine appaia meglio centrata. Possiamo usare lo stesso trucco `margin: 0 auto` che abbiamo fatto per il corpo, ma ci sono differenze che richiedono un'impostazione aggiuntiva per far funzionare il CSS.

L'elemento {{htmlelement("body")}} è un elemento **block**, il che significa che occupa spazio sulla pagina e può accettare margini, imbottiture e altre proprietà di box. Gli elementi {{htmlelement("img")}} (immagine), d'altra parte, sono elementi **inline**: per impostazione predefinita, non accettano valori di margine nello stesso modo in cui fanno gli elementi blo

 Perché il trucco del margine automatico funzioni su questa immagine, dobbiamo dare ad essa un comportamento a livello di blocco usando `display: block;`.

Infine, impostiamo la proprietà {{cssxref("max-width")}} su `100%` per garantire che se l'immagine è più grande della ${cssxref("width")}} impostata sul corpo (600 pixel), sarà limitata a `600px` e non si estenderà più ampia.

> [!NOTE]
> Non preoccuparti troppo se non comprendi completamente `display: block;` e le differenze tra un elemento blocco e un elemento inline, o `max-width: 100%;`. Avranno più senso man mano che continui il tuo studio di CSS.

## Conclusione

Se hai seguito tutte le istruzioni in questo articolo, dovresti avere una pagina simile a questa:

![Un logo Mozilla, centrato, e un'intestazione e paragrafi. Ora appare ben stilizzato, con uno sfondo blu per l'intera pagina e uno sfondo arancione per la strip di contenuto principale centrata.](website-screenshot-final.png)

Puoi [visualizzare la nostra versione qui](https://mdn.github.io/beginner-html-site-styled/). Se resti bloccato, puoi sempre confrontare il tuo lavoro con il nostro [codice d'esempio finale su GitHub](https://github.com/mdn/beginner-html-site-styled/blob/gh-pages/styles/style.css).

In questo articolo, abbiamo solo graffiato la superficie di CSS. Imparerai molto di più nel nostro modulo Core [CSS styling basics](/it/docs/Learn_web_development/Core/Styling_basics) più avanti nel corso.

## Vedi anche

- [Impara HTML e CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Il corso _Learn HTML and CSS_ di [Scrimba](https://scrimba.com?via=mdn) ti insegna HTML e CSS costruendo e distribuendo cinque progetti fantastici, con lezioni e sfide interattive e divertenti insegnate da docenti competenti.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Your_first_website")}}
