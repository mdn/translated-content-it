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
        Familiarità basilare con il sistema operativo del computer, il software di base che si utilizzerà per costruire un sito web e i sistemi di file.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo e la funzione del CSS.</li>
          <li>Le parti di base della sintassi CSS — regole, selettori, dichiarazioni, proprietà, valori delle proprietà.</li>
          <li>Funzionalità comuni del CSS incluso il modello box, cambiando colori e font, e posizionamento degli elementi HTML.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è il CSS?

Come l'HTML, il CSS non è un linguaggio di programmazione. Non è neanche un linguaggio di marcatura. **CSS è un linguaggio di fogli di stile.** Il CSS è usato per stilare elementi HTML: si selezionano gli elementi che si desidera stilare e si impostano i valori per le loro proprietà di stile, che definiscono come appariranno.

Riprendiamo l'esempio HTML di base dall'articolo [Creare il contenuto](/it/docs/Learn_web_development/Getting_started/Your_first_website/Creating_the_content):

```html live-sample___basic-html live-sample___basic-css
<p>Instructions for life:</p>

<ul>
  <li>Eat</li>
  <li>Sleep</li>
  <li>Repeat</li>
</ul>
```

Questo viene renderizzato come segue da solo:

{{EmbedLiveSample("basic-html", "100%", "140px")}}

Se aggiungiamo del CSS nel mix, possiamo cambiare l'aspetto dell'HTML. Il seguente snippet seleziona l'elemento {{htmlelement("p")}} e gli dà un carattere [font](/it/docs/Web/CSS/font-family) diverso e un testo rosso {{cssxref("color")}}. Poi seleziona tutti gli elementi {{htmlelement("li")}} e dà a ciascuno un {{cssxref("background-color")}} verde-giallo, un {{cssxref("border")}} nero a 1 pixel, e un [margine inferiore](/it/docs/Web/CSS/margin-bottom) di 5 pixel:

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

Con il CSS applicato all'HTML, la demo ora viene renderizzata così:

{{EmbedLiveSample("basic-css", "100%", "160px")}}

Come si può vedere, con solo un po' di CSS, siamo stati in grado di cambiare l'aspetto di una lista dall'aspetto semplice.

Il CSS ha molte altre funzioni, dalla specifica di immagini di sfondo e gradiente, al controllo della tipografia e del comportamento di scorrimento, all'aggiunta di animazioni e alla costruzione di interi layout di pagine web.

## Applicare CSS al proprio HTML

Quando si usa il CSS, la prima cosa da fare correttamente è assicurarsi che il CSS venga applicato con successo al proprio HTML. In questa sezione, aggiungeremo un **foglio di stile** CSS al suo `first-website` e lo applicheremo alla sua pagina.

1. All'interno della cartella `first-website`, crei un'altra nuova cartella chiamata `styles`.
2. Usando un editor di testo, incolli il seguente CSS in un nuovo file, che darà ai suoi elementi `<p>` un colore di testo rosso. È utile iniziare con qualcosa del genere per testare se il foglio di stile viene applicato correttamente al suo HTML.

   ```css
   p {
     color: red;
   }
   ```

3. Salvi il file nella cartella `styles` con il nome `style.css`.
4. Apri il suo file `index.html`. Incolli la seguente riga all'interno della testa HTML (tra i tag {{HTMLElement("head")}} e `</head>`):

   ```html
   <link href="styles/style.css" rel="stylesheet" />
   ```

5. Salvi `index.html` e lo carichi nel suo browser. Dovrebbe vedere qualcosa di simile a questo:

![Un logo Mozilla e alcuni paragrafi. Il testo del paragrafo è stato stilizzato in rosso dal nostro css.](website-screenshot-styled.png)

Se il testo del paragrafo è rosso, congratulazioni! Il suo CSS sta funzionando. Se non lo è, esamini i passaggi sopra e controlli attentamente di aver seguito ciascuno correttamente.

## Nozioni di base sulla sintassi CSS

Nell'esempio di CSS precedente, `p` è chiamato un **selettore** — seleziona l'elemento (o gli elementi) da stilare. In particolare, `p` seleziona tutti i paragrafi nell'HTML. La riga all'interno delle graffe (`{ }`) è chiamata **dichiarazione** – imposta un valore per una proprietà specifica. In questo caso, la **proprietà** è `color`, che controlla il colore del testo dei paragrafi, e il **valore della proprietà** impostato è `red`.

L'intera struttura è chiamata **regola**. (Il termine _regola_ è spesso indicato semplicemente come _regola_.)

Esaminiamo un'altra regola, questa volta con più dichiarazioni:

```css
p {
  color: red;
  width: 500px;
  border: 1px solid black;
}
```

All'interno di una regola, deve usare un punto e virgola (`;`) per separare una dichiarazione dall'altra. All'interno di ciascuna dichiarazione, deve usare un due punti (`:`) per separare la proprietà e il suo valore.

Può anche includere più selettori in una regola, separati da virgole, per selezionare più elementi. Ad esempio:

```css
p,
.my-class,
#my-id {
  color: red;
}
```

In questa regola CSS, abbiamo incluso un selettore di **elementi** (o **tipi**), che seleziona un particolare elemento HTML. Abbiamo anche incluso altri due tipi di selettori, che non sono rilevanti per il resto di questo tutorial. Se è curioso di sapere cosa fanno, consulti la nostra [guida ai selettori di base](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors).

> [!NOTE]
> [Write your first lines of CSS!](https://scrimba.com/the-frontend-developer-career-path-c0j/~015?via=mdn) di Scrimba <sup>[_partner di apprendimento di MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce un'introduzione interattiva utile alla sintassi del CSS.

## Migliorare il testo

Torniamo al nostro esempio e usiamo il CSS per migliorare l'aspetto del testo. Imposteremo un nuovo font per la pagina e cambieremo alcune impostazioni di testo per diversi elementi.

1. Innanzitutto, trovi l'[output da Google Fonts](/it/docs/Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like#choosing_a_font) che aveva precedentemente salvato. Se non ha già scelto un font, segua il link e lo faccia ora.
2. Aggiunga gli elementi {{htmlelement("link")}} all'interno del suo `index.html` {{HTMLElement("head")}}, appena prima del tag di chiusura `</head>`. Dovrebbero apparire così:

   ```html
   <link rel="preconnect" href="https://fonts.googleapis.com" />
   <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
   <link
     href="https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100..900;1,100..900&display=swap"
     rel="stylesheet" />
   ```

   Questo codice collega la sua pagina a un foglio di stile ospitato dal servizio Google Fonts, che carica il font scelto.

3. Successivamente, vada al suo file `style.css` ed elimini la regola esistente. Non vogliamo più che i nostri paragrafi siano rossi.
4. Aggiunga le seguenti righe a `style.css`:

   ```css
   html {
     /* px means "pixels". The base font size is now 10 pixels high */
     font-size: 10px;
     /* Replace PLACEHOLDER with the font-family property value you got from Google Fonts */
     font-family: PLACEHOLDER;
   }
   ```

   > [!NOTE]
   > Tutto ciò che è tra `/*` e `*/` nel CSS è un **commento CSS**, che viene ignorato dal browser. I commenti CSS sono un modo per includere note utili sul suo codice o sulla logica, senza influenzare come viene renderizzata la sua pagina web.

5. Sostituisca la riga segnaposto `font-family` con la riga `font-family` dal suo codice Google Fonts, ad esempio:

   ```css
   font-family: "Roboto", sans-serif;
   ```

   La proprietà `font-family` imposta il o i font che vuole applicare al suo HTML. Questa regola definisce un font base globale e una dimensione del font per l'intera pagina. Tutti gli elementi all'interno dell'elemento {{HTMLElement("html")}} erediteranno la stessa `font-size` e `font-family`.

6. Ora impostiamo alcuni stili di font e testo sui nostri elementi [`<h1>`](/it/docs/Web/HTML/Reference/Elements/Heading_Elements), {{htmlelement("li")}}, e {{htmlelement("p")}}. Imposteremo nuovi valori {{cssxref("font-size")}} per ciascun elemento. Inoltre, centreremo l'intestazione usando {{cssxref("text-align")}} e aumenteremo il {{cssxref("line-height")}} e la {{cssxref("letter-spacing")}} dei paragrafi e degli elenchi per rendere il contenuto più leggibile.

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

7. Salvi il suo codice e carichi il suo HTML in un browser (lo aggiorni se l'ha già aperto da prima). Il suo lavoro in corso dovrebbe apparire simile a questo:

   ![Un logo Mozilla e alcuni paragrafi. È stato impostato un font sans-serif, le misure, l'altezza delle righe e la spaziatura tra le lettere sono state modificate, e l'intestazione principale della pagina è stata centrata](website-screenshot-font-small.png)

   > [!NOTE]
   > Provi ad aggiustare i valori `px` fino a ottenere dimensioni di carattere che le piacciono per la sua intestazione e il suo testo principale.

## Il CSS riguarda tutto le caselle

Qualcosa che noterà del CSS man mano che lo utilizza di più è che molto riguarda le caselle. La maggior parte degli elementi HTML su una pagina può essere considerata come delle caselle che siedono sopra (o accanto a) altre caselle. Può impostare valori su queste caselle per dimensione, colore, posizionamento, ecc. Questo è indicato come [**il modello box**](/it/docs/Learn_web_development/Core/Styling_basics/Box_model).

![Tre scatole posizionate l'una dentro l'altra. Dall'esterno verso l'interno sono etichettate margine, bordo e riempimento](box-model.png)

Ogni scatola che occupa spazio sulla sua pagina ha proprietà come:

- {{cssxref("padding")}}: Lo spazio attorno al contenuto. Nell'esempio precedente, è lo spazio attorno al testo del paragrafo.
- {{cssxref("border")}}: La linea solida subito fuori dal riempimento.
- {{cssxref("margin")}}: Lo spazio al di fuori del bordo.

In questa sezione, usiamo anche le seguenti proprietà, alcune delle quali ha già visto:

- {{cssxref("width")}}: La larghezza di un elemento.
- {{cssxref("background-color")}}: Il colore dietro il contenuto e il riempimento di un elemento.
- {{cssxref("color")}}: Il colore del contenuto di un elemento (solitamente il testo).
- {{cssxref("text-shadow")}}: Un'ombra al testo dentro un elemento.
- {{cssxref("display")}}: La modalità di visualizzazione di un elemento (che si riferisce fondamentalmente a come appare o è organizzato sulla pagina web).

In ciascuna delle sezioni che seguono:

1. Aggiunga il codice CSS fornito in fondo al suo file `style.css`.
2. Salvi il file e aggiorni il suo browser per vedere come il CSS ha influenzato il rendering dell'HTML.
3. Legga la spiegazione fornita per aiutarla a comprendere come funziona il CSS.
4. Se si sente avventuroso, sperimenti cambiando i valori delle proprietà per personalizzare ulteriormente la sua pagina.

## Cambiare il colore della pagina

```css
html {
  background-color: #00539f;
}
```

Questa regola imposta un colore di sfondo per l'intera pagina. Cambi il codice colore in quello scelto in [Come sarà il tuo sito web?](/it/docs/Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like#choosing_a_theme_color).

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

Il codice sopra impostai nuovi valori per diverse proprietà dell'elemento {{htmlelement("body")}}. Esaminiamoli riga per riga:

- `width: 600px;`: Questo forza il corpo ad avere sempre 600 pixel di larghezza.
- `margin: 0 auto;`: Quando imposta due valori su una proprietà come `margin` o `padding`, il primo valore influenza il lato superiore _e_ inferiore dell'elemento (impostandolo a `0` in questo caso); il secondo valore influenza i lati sinistro _e_ destro. `auto` è un valore speciale che divide lo spazio orizzontale disponibile in modo uniforme tra sinistra e destra.
- `background-color: #FF9500;`: Questo imposta il colore di sfondo dell'elemento. Nel nostro progetto, usiamo un arancione rossastro per il colore di sfondo del `<body>` per contrastare con il blu scuro usato per l'elemento {{htmlelement("html")}}.
- `padding: 0 20px 20px 20px;`: Questo imposta quattro valori per il riempimento. L'obiettivo è mettere un po' di spazio attorno al contenuto. In questo esempio, non c'è riempimento nella parte superiore del corpo e 20 pixel a destra, in basso e a sinistra. I valori impostano il riempimento in alto, a destra, in basso e a sinistra, in quell'ordine.
- `border: 5px solid black;`: Questo imposta i valori per la larghezza, lo stile e il colore del bordo. In questo caso, è un bordo nero solido largo 5 pixel attorno a tutti i lati del corpo.

## Posizionamento e stilizzazione del titolo principale della pagina

```css
h1 {
  margin: 0;
  padding: 20px 0;
  color: #00539f;
  text-shadow: 3px 3px 1px black;
}
```

Avrà notato un brutto divario in alto nel corpo. Ciò accade perché i browser applicano uno styling predefinito all'elemento `<h1>`. Questo potrebbe sembrare una cattiva idea, ma l'intento è fornire una leggibilità di base per pagine non stilizzate. Per eliminare il divario, sovrascriviamo lo styling predefinito del browser con l'impostazione `margin: 0;`.

Successivamente, impostiamo il riempimento superiore e inferiore dell'intestazione a 20 pixel e impostiamo il testo dell'intestazione per avere lo stesso colore dello sfondo HTML.

Infine, `text-shadow` applica un'ombra al contenuto del testo dell'elemento:

- Il primo valore pixel imposta lo **spostamento orizzontale** dell'ombra rispetto al testo: quanto si sposta attraverso.
- Il secondo valore pixel imposta lo **spostamento verticale** dell'ombra rispetto al testo: quanto si sposta verso il basso.
- Il terzo valore pixel imposta il **raggio di sfocatura** dell'ombra. Un valore più ampio produce un'ombra dall'aspetto più sfocato.
- Il quarto valore imposta il colore di base dell'ombra.

## Centralizzare l'immagine

```css
img {
  display: block;
  margin: 0 auto;
  max-width: 100%;
}
```

Successivamente, centralizziamo l'immagine per migliorare l'aspetto. Possiamo usare lo stesso trucco `margin: 0 auto` che abbiamo fatto per il corpo, ma ci sono differenze che richiedono un'impostazione aggiuntiva affinché il CSS funzioni.

L'elemento {{htmlelement("body")}} è un elemento di **blocco**, il che significa che occupa spazio sulla pagina e può accettare margini, riempimento e altre proprietà delle caselle. Gli elementi {{htmlelement("img")}} (immagine), d'altra parte, sono elementi **in linea**: per impostazione predefinita, non accettano valori di margine nello stesso modo in cui lo fanno gli elementi di blocco. Perché il trucco del margine automatico funzioni su questa immagine, dobbiamo darle un comportamento a livello di blocco usando `display: block;`.

Infine, impostiamo la proprietà {{cssxref("max-width")}} al `100%` per assicurarci che se l'immagine è più grande della `larghezza` impostata sul corpo (600 pixel), sarà limitata a `600px` e non si estenderà oltre.

> [!NOTE]
> Non si preoccupi troppo se non comprende completamente `display: block;` e le differenze tra un elemento di blocco e un elemento in linea, o `max-width: 100%;`. Avranno più senso man mano che continuerà il suo studio del CSS.

## Conclusione

Se ha seguito tutte le istruzioni in questo articolo, dovrebbe avere una pagina che assomiglia a questa:

![Un logo Mozilla, centrato, e un'intestazione e paragrafi. Ora sembra ben stilizzata, con uno sfondo blu per l'intera pagina e uno sfondo arancione per la striscia principale del contenuto centrato.](website-screenshot-final.png)

Può [vedere la nostra versione qui](https://mdn.github.io/beginner-html-site-styled/). Se si blocca, può sempre confrontare il suo lavoro con il nostro [codice di esempio finale su GitHub](https://github.com/mdn/beginner-html-site-styled/blob/gh-pages/styles/style.css).

In questo articolo, abbiamo solo scalfito la superficie del CSS. Imparerà molto di più nel nostro modulo Core [Nozioni di base sullo stile CSS](/it/docs/Learn_web_development/Core/Styling_basics) più avanti nel corso.

## Vedi anche

- [Learn HTML and CSS](https://scrimba.com/learn-html-and-css-c0p?via=mdn), Scrimba <sup>[_partner di apprendimento di MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : [Il corso Learn HTML and CSS di Scrimba](https://scrimba.com?via=mdn) insegna HTML e CSS creando e distribuendo cinque fantastici progetti, con lezioni e sfide interattive divertenti insegnate da docenti esperti.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Creating_the_content", "Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Your_first_website")}}
