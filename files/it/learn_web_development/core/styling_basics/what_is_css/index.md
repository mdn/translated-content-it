---
title: Cos'è il CSS?
slug: Learn_web_development/Core/Styling_basics/What_is_CSS
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{NextMenu("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics")}}

**{{Glossary("CSS", "CSS")}}** (Fogli di stile a cascata) ti consente di creare pagine web dall'aspetto eccellente, ma come funziona dietro le quinte? Questo articolo spiega cos'è il CSS, come appare la sintassi di base e come il tuo browser applica il CSS all'HTML per stilizzarlo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, conoscenza di base di
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >lavorare con i file</a
        >, e familiarità con HTML (studia il
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        > modulo.)
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo del CSS.</li>
          <li>Che l'HTML non ha nulla a che fare con lo stile.</li>
          <li>Il concetto di stili predefiniti del browser.</li>
          <li>Che aspetto ha il codice CSS.</li>
          <li>Come il CSS viene applicato all'HTML.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Stili predefiniti del browser

Nel modulo [Strutturare il contenuto con HTML](/it/docs/Learn_web_development/Core/Structuring_content), abbiamo trattato cos'è l'HTML e come viene usato per marcare i documenti. Questi documenti saranno leggibili in un browser web. I titoli appariranno più grandi rispetto al testo normale, i paragrafi andranno a capo e avranno spazio tra loro. I link sono colorati e sottolineati per distinguerli dal resto del testo.

Ciò che vedi sono gli stili predefiniti del browser: uno styling molto basilare che il browser applica all'HTML per assicurarsi che la pagina sia leggibile anche se non è specificato alcuno stile esplicito dall'autore della pagina. Questi stili sono definiti nei fogli di stile CSS predefiniti contenuti all'interno del browser, e non hanno nulla a che fare con l'HTML.

![Gli stili predefiniti utilizzati da un browser](html-example.png)

Il web sarebbe un posto noioso se tutti i siti Web avessero quell'aspetto. Questo è il motivo per cui è necessario imparare CSS.

## A cosa serve il CSS?

Usando il CSS, puoi controllare esattamente come appaiono gli elementi HTML nel browser, presentando i tuoi documenti agli utenti con qualsiasi design e layout desideri.

- Un **documento** è solitamente un file di testo strutturato utilizzando un linguaggio di markup, più comunemente {{Glossary("HTML", "HTML")}} (questi sono chiamati _documenti HTML_). Potresti anche incontrare documenti scritti in altri linguaggi di markup come {{Glossary("SVG", "SVG")}} o {{Glossary("XML", "XML")}}. Dove abbiamo precedentemente parlato di pagine web, un documento HTML contiene il contenuto della pagina web e ne specifica la struttura.
- **Presentare** un documento a un utente significa convertirlo in una forma utilizzabile dal tuo pubblico. I {{Glossary("browser", "browser")}} come {{Glossary("Mozilla_Firefox", "Firefox")}}, {{Glossary("Google_Chrome", "Chrome")}}, {{Glossary("Apple_Safari", "Safari")}} e {{Glossary("Microsoft_Edge", "Edge")}} sono progettati per presentare documenti visivamente, ad esempio, su uno schermo di computer, proiettore, dispositivo mobile o stampante. In un contesto web, questo è generalmente chiamato _rendering_; abbiamo fornito una descrizione semplificata del processo mediante il quale una pagina web viene renderizzata in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

> [!NOTE]
> Un browser è a volte chiamato {{Glossary("User_agent", "user agent")}}, che significa fondamentalmente un programma per computer che rappresenta una persona all'interno di un sistema informatico.

Il CSS può essere utilizzato per molti scopi relativi all'aspetto della tua pagina web. I più importanti sono:

- Styling del testo, ad esempio, per cambiare il [colore](/it/docs/Web/CSS/color_value) e la [dimensione](/it/docs/Web/CSS/font-size) di titoli e link.
- Creare layout, ad esempio, [trasformando una singola colonna di testo in un layout a colonne multiple](/it/docs/Web/CSS/Layout_cookbook/Column_layouts).
- Effetti speciali come le [animazioni](/it/docs/Web/CSS/CSS_animations).

Il linguaggio CSS è organizzato in _moduli_ che contengono funzionalità correlate. Ad esempio, dai un'occhiata alle pagine di riferimento di MDN per il modulo [Sfondi e Bordi](/it/docs/Web/CSS/CSS_backgrounds_and_borders) per scoprire qual è il suo scopo e le proprietà e le funzionalità che contiene. In quel modulo, troverai anche un link alle _Specifiche_ che definiscono la tecnologia.

## Nozioni di base sulla sintassi CSS

Il CSS è un linguaggio basato su regole: definisci le regole specificando gruppi di stili che dovrebbero essere applicati a particolari elementi o gruppi di elementi nella tua pagina web.

Ad esempio, potresti decidere di stilizzare il titolo principale della tua pagina come un testo rosso grande. Il seguente codice mostra una regola CSS molto semplice che otterrebbe questo risultato:

```css
h1 {
  color: red;
  font-size: 2.5em;
}
```

- Nell'esempio sopra, la regola CSS si apre con un {{Glossary("CSS_Selector", "selettore")}}. Questo _seleziona_ gli elementi HTML che stiamo per stilizzare. In questo caso, stiamo stilizzando i titoli di livello uno (`{{htmlelement("Heading_Elements", "&lt;h1>")}}`).
- Poi abbiamo una serie di parentesi graffe — `{ }`.
- Le parentesi contengono una o più **dichiarazioni**, che prendono la forma di coppie di **proprietà** e **valori**. Specifichiamo la proprietà (ad esempio, `color` nell'esempio sopra) prima del due punti e specifichiamo il valore della proprietà dopo il due punti (`red` è il valore impostato per la proprietà `color`).
- Questo esempio contiene due dichiarazioni, una per `color` e un'altra per `font-size`.

Diverse {{Glossary("property/CSS", "proprietà")}} CSS hanno valori ammissibili diversi. Nel nostro esempio, abbiamo la proprietà `color`, che può prendere vari [valori di colore](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color). Abbiamo anche la proprietà `font-size`. Questa proprietà può assumere vari [unità di dimensione](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#numbers_lengths_and_percentages) come valore.

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

Scoprirai che impari rapidamente alcuni valori, mentre altri dovrai consultarli. Le pagine delle singole proprietà su MDN ti offrono un modo rapido per consultare le proprietà e i loro valori.

> [!NOTE]
> Puoi trovare i link a tutte le pagine delle proprietà CSS (insieme ad altre funzionalità CSS) elencati nella [referenza CSS](/it/docs/Web/CSS/Reference) di MDN. In alternativa, dovresti abituarti a cercare "mdn _css-feature-name_" nel tuo motore di ricerca preferito ogni volta che hai bisogno di maggiori informazioni su una funzionalità CSS. Ad esempio, prova a cercare "mdn color" o "mdn font-size"!

## Come il CSS viene applicato all'HTML?

Come spiegato in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites), quando navighi su una pagina web, il browser riceve prima il documento HTML contenente il contenuto della pagina web e lo converte in un **DOM tree**.

Dopo di che, tutte le regole CSS trovate nella pagina web (inserite direttamente nell'HTML o in file `.css` esterni collegati) sono suddivise in diversi "contenitori", in base ai diversi elementi a cui verranno applicate (come specificato dai loro selettori). Le regole CSS vengono quindi applicate al DOM tree, risultando in un **render tree**, che viene poi dipinto nella finestra del browser.

Diamo un'occhiata a un esempio. Prima di tutto, definiamo un frammento HTML a cui il CSS potrebbe essere applicato:

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

- Seleziona tutti gli elementi `<h1>` nella pagina, colorando il loro testo di rosso e rendendoli più grandi rispetto alla loro dimensione predefinita. Poiché c'è solo un `<h1>` nel nostro esempio HTML, solo quell'elemento riceverà lo stile.
- Seleziona tutti gli elementi `<p>` nella pagina, dando loro un colore del testo e dello sfondo personalizzato e un po' di spazio intorno al testo. Ci sono due elementi `<p>` nel nostro esempio HTML e entrambi ricevono lo stile.

Quando il CSS viene applicato all'HTML, l'output renderizzato è il seguente:

{{EmbedLiveSample('How is CSS applied to HTML?', '100%', 200)}}

> [!CALLOUT]
>
> **Provalo**
>
> Prova a giocare con l'esempio sopra. Per farlo, premi il pulsante "Play" in alto a destra per caricarlo nel nostro editor Playground. Prova quanto segue:
>
> 1. Aggiungi un altro paragrafo di testo sotto i due esistenti e nota come la seconda regola CSS venga automaticamente applicata al nuovo paragrafo.
> 2. Aggiungi un sottotitolo `<h2>` da qualche parte sotto il `<h1>`, magari dopo uno dei paragrafi. Prova a dargli un colore diverso aggiungendo una nuova regola al CSS. Fai una copia della regola `h1`, cambia il selettore in `h2` e cambia il valore di `color` da `red` a `purple`, ad esempio.
> 3. Se ti senti avventuroso, prova a cercare alcune nuove proprietà CSS e valori nella [referenza CSS](/it/docs/Web/CSS/Reference) di MDN da aggiungere alle tue regole!
>
> Per un po' di pratica aggiuntiva con le basi del CSS, vedi [Scrivi le tue prime righe di CSS!](https://scrimba.com/learn-html-and-css-c0p/~0j?via=mdn) da Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>. Questo scrim offre una panoramica utile della sintassi di base del CSS e fornisce una sfida interattiva in cui puoi ottenere più pratica con la scrittura delle dichiarazioni CSS.

## Riassunto

Ora che hai una certa comprensione di cos'è il CSS e di come funziona, passiamo a darti un po' di pratica nella scrittura del CSS tu stesso e a spiegare la sintassi in modo più dettagliato.

{{NextMenu("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics")}}
