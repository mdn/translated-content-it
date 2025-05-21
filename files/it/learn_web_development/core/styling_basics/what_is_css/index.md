---
title: Cos'è CSS?
slug: Learn_web_development/Core/Styling_basics/What_is_CSS
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{NextMenu("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics")}}

**{{Glossary("CSS", "CSS")}}** (Cascading Style Sheets) consente di creare pagine web dall'aspetto attraente, ma come funziona dietro le quinte? Questo articolo spiega cos'è il CSS, come appare la sintassi di base e come il tuo browser applica il CSS all'HTML per stilizzarlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a>, conoscenza di base su
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >lavorare con i file</a>, e familiarità con l'HTML (studia il
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Modulo di strutturazione del contenuto con HTML</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo del CSS.</li>
          <li>Che l'HTML non ha nulla a che fare con lo stile.</li>
          <li>Il concetto di stili predefiniti del browser.</li>
          <li>Come appare il codice CSS.</li>
          <li>Come il CSS viene applicato all'HTML.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Stili predefiniti del browser

Nel modulo [Strutturazione del contenuto con HTML](/it/docs/Learn_web_development/Core/Structuring_content), abbiamo trattato cosa sia l'HTML e come viene usato per contrassegnare i documenti. Questi documenti saranno leggibili in un browser web. Gli heading appariranno più grandi rispetto al testo normale, i paragrafi si interrompono su una nuova riga e hanno spazio tra di loro. I link sono colorati e sottolineati per distinguerli dal resto del testo.

Quello che stai vedendo sono gli stili predefiniti del browser — uno stile molto basico che il browser applica all'HTML per assicurarsi che la pagina sia sostanzialmente leggibile anche se non viene specificato uno stile esplicito dall'autore della pagina. Questi stili sono definiti in fogli di stile CSS predefiniti contenuti all'interno del browser — non hanno nulla a che fare con l'HTML.

![Gli stili predefiniti usati da un browser](html-example.png)

Il web sarebbe un luogo noioso se tutti i siti web apparissero così. Ecco perché è necessario imparare il CSS.

## A cosa serve CSS?

Usando CSS, è possibile controllare esattamente come appaiono gli elementi HTML nel browser, presentando i documenti agli utenti con qualsiasi design e layout si desideri.

- Un **documento** è di solito un file di testo strutturato utilizzando un linguaggio di markup, più comunemente {{Glossary("HTML", "HTML")}} (questi sono denominati _documenti HTML_). Potresti anche imbatterti in documenti scritti in altri linguaggi di markup come {{Glossary("SVG", "SVG")}} o {{Glossary("XML", "XML")}}. Dove precedentemente abbiamo parlato di pagine web, un documento HTML contiene il contenuto della pagina web e ne specifica la struttura.
- **Presentare** un documento a un utente significa convertirlo in una forma utilizzabile dal tuo pubblico. I {{Glossary("browser", "browser")}} come {{Glossary("Mozilla_Firefox", "Firefox")}}, {{Glossary("Google_Chrome", "Chrome")}}, {{Glossary("Apple_Safari", "Safari")}}, e {{Glossary("Microsoft_Edge", "Edge")}} sono progettati per presentare documenti visivamente, ad esempio su uno schermo del computer, proiettore, dispositivo mobile o stampante. In un contesto web, questo è generalmente chiamato _rendering_; abbiamo fornito una descrizione semplificata del processo tramite il quale una pagina web è renderizzata in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

> [!NOTE]
> Un browser è talvolta chiamato {{Glossary("User_agent", "user agent")}}, che essenzialmente è un programma informatico che rappresenta una persona all'interno di un sistema informatico.

CSS può essere usato per molti scopi legati all'aspetto e alla sensazione della tua pagina web. I più importanti sono:

- Stilizzazione del testo, ad esempio, per cambiare il [colore](/it/docs/Web/CSS/color_value) e la [dimensione](/it/docs/Web/CSS/font-size) degli heading e dei link.
- Creare layout, ad esempio, [trasformare una singola colonna di testo in un layout a più colonne](/it/docs/Web/CSS/Layout_cookbook/Column_layouts).
- Effetti speciali come [animazioni](/it/docs/Web/CSS/CSS_animations).

Il linguaggio CSS è organizzato in _moduli_ che contengono funzionalità correlate. Ad esempio, dai un'occhiata alle pagine di riferimento su MDN per il modulo [Sfondi e Bordi](/it/docs/Web/CSS/CSS_backgrounds_and_borders) per scoprire qual è il suo scopo e quali proprietà e funzionalità contiene. In quel modulo, troverai anche un link alle _Specifiche_ che definiscono la tecnologia.

## Sintassi di base del CSS

CSS è un linguaggio basato su regole — si definiscono regole specificando gruppi di stili che dovrebbero essere applicati a particolari elementi o gruppi di elementi sulla tua pagina web.

Ad esempio, potresti decidere di stilizzare l'heading principale sulla tua pagina come testo rosso grande. Il codice seguente mostra una regola CSS molto semplice che lo raggiungerebbe:

```css
h1 {
  color: red;
  font-size: 2.5em;
}
```

- Nell'esempio sopra, la regola CSS si apre con un {{Glossary("CSS_Selector", "selettore")}}. Questo _seleziona_ gli elementi HTML che andremo a stilizzare. In questo caso, stiamo stilizzando gli heading di livello uno (`{{htmlelement("Heading_Elements", "&lt;h1>")}}`).
- Poi abbiamo una serie di parentesi graffe — `{ }`.
- Le parentesi contengono una o più **dichiarazioni**, che prendono la forma di coppie **proprietà** e **valore**. Si specifica la proprietà (ad esempio, `color` nell'esempio sopra) prima del due punti, e si specifica il valore della proprietà dopo il due punti (`red` è il valore che viene impostato per la proprietà `color`).
- Questo esempio contiene due dichiarazioni, una per `color` e un'altra per `font-size`.

Diverse {{Glossary("property/CSS", "proprietà")}} CSS hanno diversi valori consentiti. Nel nostro esempio, abbiamo la proprietà `color`, che può prendere vari [valori di colore](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color). Abbiamo anche la proprietà `font-size`. Questa proprietà può prendere vari [unità di misura](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#numbers_lengths_and_percentages) come valore.

Un foglio di stile CSS contiene molte di tali regole, scritte una dopo l'altra.

```css
h1 {
  color: red;
  font-size: 2.5em;
}

p {
  color: aqua;
  padding: 5px;
  background: midnightblue;
}
```

Scoprirai che impari rapidamente alcuni valori, mentre altri dovrai cercarli. Le singole pagine delle proprietà su MDN ti offrono un modo rapido per consultare le proprietà e i loro valori.

> [!NOTE]
> Puoi trovare i link a tutte le pagine delle proprietà CSS (insieme ad altre funzionalità CSS) elencate nella [referenza CSS](/it/docs/Web/CSS/Reference) su MDN. In alternativa, dovresti abituarti a cercare "mdn _nome-funzionalità-css_" nel tuo motore di ricerca preferito ogni volta che hai bisogno di saperne di più su una funzionalità CSS. Ad esempio, prova a cercare "mdn color" o "mdn font-size"!

## Come viene applicato il CSS all'HTML?

Come spiegato in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites), quando navighi verso una pagina web, il browser riceve prima il documento HTML contenente il contenuto della pagina web e lo converte in un **albero DOM**.

Dopo di ciò, tutte le regole CSS trovate nella pagina web (inserite direttamente nell'HTML o nei file `.css` esterni referenziati) vengono ordinate in diversi "bucket", basati sui diversi elementi a cui saranno applicate (come specificato dai loro selettori). Le regole CSS vengono poi applicate all'albero DOM, risultando in un **albero di rendering**, che viene quindi dipinto nella finestra del browser.

Vediamo un esempio. Prima di tutto, definiamo un frammento HTML a cui il CSS potrebbe essere applicato:

```html
<h1>CSS is great</h1>

<p>You can style text.</p>

<p>And create layouts and special effects.</p>
```

Ora, il nostro CSS, ripetuto dalla sezione precedente:

```css
h1 {
  color: red;
  font-size: 2.5em;
}

p {
  color: aqua;
  padding: 5px;
  background: midnightblue;
}
```

Questo CSS:

- Seleziona tutti gli elementi `<h1>` sulla pagina, colorando il loro testo di rosso e rendendoli più grandi della loro dimensione predefinita. Poiché c'è solo un `<h1>` nel nostro esempio HTML, solo quell'elemento riceverà lo stile.
- Seleziona tutti gli elementi `<p>` sulla pagina, assegnando loro un colore di testo e di sfondo personalizzato e un po' di spazio attorno al testo. Ci sono due elementi `<p>` nel nostro esempio HTML, e entrambi ricevono lo stile.

Quando il CSS è applicato all'HTML, l'output renderizzato è il seguente:

{{EmbedLiveSample('How is CSS applied to HTML?', '100%', 200)}}

> [!CALLOUT]
>
> **Provalo**
>
> Prova a giocare con l'esempio sopra. Per farlo, premi il pulsante "Play" nell'angolo in alto a destra per caricarlo nel nostro editor Playground. Prova quanto segue:
>
> 1. Aggiungi un altro paragrafo di testo sotto i due esistenti e nota come la seconda regola CSS venga automaticamente applicata al nuovo paragrafo.
> 2. Aggiungi un `<h2>` subheading da qualche parte sotto l'`<h1>`, magari dopo uno dei paragrafi. Prova a dargli un colore diverso aggiungendo una nuova regola al CSS. Fai una copia della regola `h1`, cambia il selettore in `h2`, e cambia il valore di `color` da `red` a `purple`, per esempio.
> 3. Se ti senti avventuroso, prova a cercare alcune nuove proprietà e valori CSS nella [referenza CSS](/it/docs/Web/CSS/Reference) su MDN da aggiungere alle tue regole!

## Riepilogo

Ora che hai una certa comprensione di cosa sia il CSS e come funziona, passiamo a darti un po' di pratica per scrivere CSS da solo e spiegare la sintassi in modo più dettagliato.

## Vedi anche

- [Scrivi le tue prime righe di CSS!](https://scrimba.com/learn-html-and-css-c0p/~0j?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>:
  - Questo scrim offre un utile riassunto della sintassi di base del CSS e fornisce una sfida interattiva dove puoi praticare di più nello scrivere dichiarazioni CSS.

{{NextMenu("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics")}}
