---
title: Che cos'è CSS?
slug: Learn_web_development/Core/Styling_basics/What_is_CSS
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{NextMenu("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics")}}

**{{Glossary("CSS", "CSS")}}** (Cascading Style Sheets) permette di creare pagine web dall'aspetto accattivante, ma come funziona sotto il cofano? Questo articolo spiega cos'è il CSS, com'è strutturata la sintassi di base e come il suo browser applica il CSS all'HTML per stilarlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, conoscenze di base su
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >gestione dei file</a
        >, e familiarità con l'HTML (studiare il
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Modulo di strutturazione dei contenuti con HTML</a
        >.)
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
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Stili predefiniti del browser

Nel modulo [Structuring content with HTML](/it/docs/Learn_web_development/Core/Structuring_content), abbiamo trattato cos'è l'HTML e come viene utilizzato per marcare i documenti. Questi documenti saranno leggibili in un browser web. I titoli appariranno più grandi rispetto al testo normale, i paragrafi si interrompono su una nuova linea e c'è spazio tra di essi. I link sono colorati e sottolineati per distinguerli dal resto del testo.

Ciò che si vede sono gli stili predefiniti del browser: uno stile molto basilare che il browser applica all'HTML per assicurarsi che la pagina sia fondamentalmente leggibile anche se non è specificato alcuno stile esplicito dall'autore della pagina. Questi stili sono definiti in fogli di stile CSS predefiniti contenuti nel browser: non hanno nulla a che fare con l'HTML.

![Gli stili predefiniti usati da un browser](html-example.png)

Il web sarebbe un posto noioso se tutti i siti web sembrassero così. Questo è il motivo per cui è necessario imparare riguardo al CSS.

## A cosa serve il CSS?

Usando il CSS, è possibile controllare esattamente come appaiono gli elementi HTML nel browser, presentando i documenti agli utenti con qualsiasi design e layout si desideri.

- Un **documento** è solitamente un file di testo strutturato utilizzando un linguaggio di markup, più comunemente {{Glossary("HTML", "HTML")}} (chiamati _documenti HTML_). Potreste anche imbattervi in documenti scritti in altri linguaggi di markup come {{Glossary("SVG", "SVG")}} o {{Glossary("XML", "XML")}}. Dove in precedenza abbiamo parlato di pagine web, un documento HTML contiene il contenuto della pagina web e ne specifica la struttura.
- **Presentare** un documento a un utente significa convertirlo in una forma utilizzabile dal pubblico. I {{Glossary("browser", "browser")}} come {{Glossary("Mozilla_Firefox", "Firefox")}}, {{Glossary("Google_Chrome", "Chrome")}}, {{Glossary("Apple_Safari", "Safari")}} e {{Glossary("Microsoft_Edge", "Edge")}} sono progettati per presentare documenti in modo visivo, ad esempio, su uno schermo di computer, proiettore, dispositivo mobile o stampante. In un contesto web, questo è generalmente indicato come _rendering_; abbiamo fornito una descrizione semplificata del processo mediante il quale una pagina web viene renderizzata in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

> [!NOTE]
> Un browser è talvolta chiamato {{Glossary("User_agent", "user agent")}}, che in pratica significa un programma per computer che rappresenta una persona all'interno di un sistema informatico.

Il CSS può essere utilizzato per molti scopi legati all'aspetto e alla sensazione della vostra pagina web. I più importanti sono:

- Stilizzazione del testo, ad esempio, per cambiare il [colore](/it/docs/Web/CSS/color_value) e la [dimensione](/it/docs/Web/CSS/font-size) dei titoli e link.
- Creazione di layout, ad esempio, [trasformando una singola colonna di testo in un layout a colonne multiple](/it/docs/Web/CSS/Layout_cookbook/Column_layouts).
- Effetti speciali come [animazione](/it/docs/Web/CSS/CSS_animations).

Il linguaggio CSS è organizzato in _moduli_ che contengono funzionalità correlate. Ad esempio, date un'occhiata alle pagine di riferimento MDN per il modulo [Backgrounds and Borders](/it/docs/Web/CSS/CSS_backgrounds_and_borders) per scoprire qual è il suo scopo e le proprietà e funzionalità che contiene. In quel modulo, troverete anche un link alle _Specifiche_ che definiscono la tecnologia.

## Nozioni di base sulla sintassi CSS

Il CSS è un linguaggio basato su regole: si definiscono regole specificando gruppi di stili che dovrebbero essere applicati a particolari elementi o gruppi di elementi sulla vostra pagina web.

Ad esempio, potreste decidere di stilizzare il titolo principale della vostra pagina come testo grande e rosso. Il seguente codice mostra una regola CSS molto semplice che otterrebbe questo risultato:

```css
h1 {
  color: red;
  font-size: 2.5em;
}
```

- Nell'esempio sopra, la regola CSS inizia con un {{Glossary("CSS_Selector", "selettore")}}. Questo _seleziona_ gli elementi HTML che vogliamo stilizzare. In questo caso, stiamo stilizzando i titoli di livello uno (`{{htmlelement("Heading_Elements", "&lt;h1>")}}`).
- Abbiamo poi un set di parentesi graffe — `{ }`.
- Le parentesi contengono una o più **dichiarazioni**, che prendono la forma di coppie **proprietà** e **valore**. Specifichiamo la proprietà (ad esempio, `color` nell'esempio sopra) prima del due punti, e specifichiamo il valore della proprietà dopo il due punti (`red` è il valore impostato per la proprietà `color`).
- Questo esempio contiene due dichiarazioni, una per `color` e un'altra per `font-size`.

Diverse {{Glossary("property/CSS", "proprietà")}} CSS hanno valori consentiti differenti. Nel nostro esempio, abbiamo la proprietà `color`, che può assumere vari [valori di colore](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color). Abbiamo anche la proprietà `font-size`. Questa proprietà può assumere vari [unità di dimensione](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#numbers_lengths_and_percentages) come valore.

Un foglio di stile CSS contiene molte di queste regole, scritte una dopo l'altra.

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

Si scoprirà che si imparano rapidamente alcuni valori, mentre altri sarà necessario consultarli. Le singole pagine delle proprietà su MDN vi offrono un modo rapido per consultare le proprietà e i loro valori.

> [!NOTE]
> È possibile trovare i link a tutte le pagine delle proprietà CSS (insieme ad altre funzionalità CSS) elencate nel [riferimento CSS](/it/docs/Web/CSS/Reference) di MDN. In alternativa, dovrebbe abituarsi a cercare "mdn _css-feature-name_" nel proprio motore di ricerca preferito ogni volta che si ha bisogno di ulteriori informazioni su una funzionalità CSS. Ad esempio, provi a cercare "mdn color" o "mdn font-size"!

## Come viene applicato il CSS all'HTML?

Come spiegato in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites), quando si naviga verso una pagina web, il browser riceve per prima cosa il documento HTML contenente il contenuto della pagina web e lo converte in un **DOM tree**.

Dopo di ciò, tutte le regole CSS trovate nella pagina web (inserite direttamente nell'HTML, o in file `.css` esterni referenziati) vengono ordinate in diversi "bucket", basati sui diversi elementi a cui verranno applicate (come specificato dai loro selettori). Le regole CSS vengono poi applicate al DOM tree, risultando in un **render tree**, che viene poi disegnato sulla finestra del browser.

Vediamo un esempio. Prima di tutto, definiamo uno snippet HTML al quale il CSS potrebbe essere applicato:

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

- Seleziona tutti gli elementi `<h1>` sulla pagina, colorando il loro testo di rosso e rendendoli più grandi della dimensione di default. Poiché c'è solo un elemento `<h1>` nel nostro esempio HTML, solo quell'elemento riceverà lo stile.
- Seleziona tutti gli elementi `<p>` sulla pagina, dando loro un colore di testo e di sfondo personalizzato e un po' di spazio intorno al testo. Ci sono due elementi `<p>` nel nostro esempio HTML, e entrambi ricevono lo stile.

Quando il CSS viene applicato all'HTML, il output renderizzato è il seguente:

{{EmbedLiveSample('Come viene applicato il CSS all\'HTML?', '100%', 200)}}

> [!CALLOUT]
>
> **Prova a farlo**
>
> Provi a giocare con l'esempio sopra. Per farlo, prema il pulsante "Play" nell'angolo in alto a destra per caricarlo nel nostro editor Playground. Provare i seguenti passaggi:
>
> 1. Aggiunga un altro paragrafo di testo sotto i due esistenti e noti come la seconda regola CSS venga automaticamente applicata al nuovo paragrafo.
> 2. Aggiunga un sottotitolo `<h2>` da qualche parte sotto l'`<h1>`, magari dopo uno dei paragrafi. Provare a dargli un colore diverso aggiungendo una nuova regola al CSS. Faccia una copia della regola `h1`, cambi il selettore in `h2` e modifichi il valore di `color` da `red` a `purple`, per esempio.
> 3. Se si sente avventuroso, provare a cercare alcune nuove proprietà e valori CSS nel [riferimento CSS](/it/docs/Web/CSS/Reference) di MDN per aggiungerli alle sue regole!

## Sommario

Ora che avete una certa comprensione di cosa sia il CSS e di come funzioni, passeremo a farvi esercitare un po' nella scrittura del CSS e a spiegare la sintassi in modo più dettagliato.

## Vedi anche

- [Scrivi le tue prime righe di CSS!](https://scrimba.com/learn-html-and-css-c0p/~0j?via=mdn), Scrimba <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
  - : Questo scrim offre una spiegazione utile sulla sintassi di base del CSS e fornisce una sfida interattiva in cui ci si può esercitare ulteriormente nella scrittura di dichiarazioni CSS.

{{NextMenu("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics")}}
