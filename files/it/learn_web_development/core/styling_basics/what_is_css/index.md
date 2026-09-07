---
title: Che cos'è CSS?
slug: Learn_web_development/Core/Styling_basics/What_is_CSS
l10n:
  sourceCommit: 013458b2380d3c68e0df0002ec151f3d8eeb84c0
---

{{NextMenu("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics")}}

**{{Glossary("CSS", "CSS")}}** (Cascading Style Sheets) consente di creare pagine web dall'aspetto eccellente, ma come funziona internamente? Questo articolo spiega che cos'è CSS, qual è l'aspetto della sintassi di base e come il browser applica CSS a HTML per definirne lo stile.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, conoscenze di base su come
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >lavorare con i file</a
        > e familiarità con HTML (studiare il modulo
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >).
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo di CSS.</li>
          <li>Che HTML non ha nulla a che fare con lo stile.</li>
          <li>Il concetto di stili predefiniti del browser.</li>
          <li>L'aspetto del codice CSS.</li>
          <li>Come CSS viene applicato a HTML.</li>
        <ul>
      </td>
    </tr>
  </tbody>
</table>

## Stili predefiniti del browser

Nel modulo [Strutturare i contenuti con HTML](/it/docs/Learn_web_development/Core/Structuring_content), abbiamo trattato che cos'è HTML e come viene utilizzato per marcare i documenti. Questi documenti saranno leggibili in un browser web. Le intestazioni appariranno più grandi del testo normale, i paragrafi andranno a capo su una nuova riga e avranno spazio tra loro. I link saranno colorati e sottolineati per distinguerli dal resto del testo.

Quello che viene visualizzato sono gli stili predefiniti del browser: uno stile molto basilare che il browser applica a HTML per assicurarsi che la pagina sia leggibile anche se l'autore della pagina non specifica esplicitamente alcuno stile. Questi stili sono definiti nei fogli di stile CSS predefiniti contenuti nel browser e non hanno nulla a che fare con HTML.

![Gli stili predefiniti utilizzati da un browser](html-example.png)

Il web sarebbe un posto noioso se tutti i siti web avessero questo aspetto. Ecco perché è necessario imparare CSS.

## A cosa serve CSS?

Usando CSS, è possibile controllare esattamente l'aspetto degli elementi HTML nel browser, presentando i documenti agli utenti con qualsiasi design e layout desiderato.

- Un **documento** è solitamente un file di testo strutturato mediante un linguaggio di markup, più comunemente {{Glossary("HTML", "HTML")}} (questi sono chiamati _documenti HTML_). Si possono incontrare anche documenti scritti in altri linguaggi di markup, come {{Glossary("SVG", "SVG")}} o {{Glossary("XML", "XML")}}. Un documento HTML contiene il contenuto di una pagina web e ne specifica la struttura.
- **Presentare** un documento a un utente significa convertirlo in una forma utilizzabile dal pubblico. I {{Glossary("browser", "browser")}} come {{Glossary("Mozilla_Firefox", "Firefox")}}, {{Glossary("Google_Chrome", "Chrome")}}, {{Glossary("Apple_Safari", "Safari")}} ed {{Glossary("Microsoft_Edge", "Edge")}} sono progettati per presentare visivamente i documenti, ad esempio su uno schermo di computer, un proiettore, un dispositivo mobile o una stampante. Nel contesto web, questo viene generalmente chiamato _rendering_; viene fornita una descrizione semplificata del processo attraverso cui viene eseguito il rendering di una pagina web in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites).

> [!NOTE]
> Un browser viene talvolta chiamato {{Glossary("User_agent", "user agent")}}, che significa essenzialmente un programma informatico che rappresenta una persona all'interno di un sistema informatico.

CSS può essere utilizzato per molti scopi legati all'aspetto e alla funzionalità della pagina web, ad esempio:

- Lo stile del testo, incluso il cambiamento del [colore](/it/docs/Web/CSS/Reference/Values/color_value) e della [dimensione](/it/docs/Web/CSS/Reference/Properties/font-size) delle intestazioni e dei link.
- La creazione di layout, come i [layout a griglia](/it/docs/Learn_web_development/Core/CSS_layout/Grids) o i [layout a più colonne](/it/docs/Web/CSS/How_to/Layout_cookbook/Column_layouts).
- Effetti speciali come le [animazioni](/it/docs/Web/CSS/Guides/Animations).

Il linguaggio CSS è organizzato in _moduli_ che contengono funzionalità correlate. Ad esempio, consultare le pagine di riferimento MDN del modulo [Sfondi e bordi](/it/docs/Web/CSS/Guides/Backgrounds_and_borders) per scoprire quale sia il suo scopo e quali proprietà e funzionalità contenga. Nelle pagine dei moduli si trovano anche link alle _Specifiche_ che definiscono le tecnologie.

## Nozioni di base sulla sintassi CSS

CSS è un linguaggio basato su regole: le regole vengono definite specificando gruppi di stili che devono essere applicati a un particolare elemento o a gruppi di elementi nella pagina web.

Ad esempio, si potrebbe decidere di applicare all'intestazione principale della pagina uno stile con testo rosso di grandi dimensioni. Il codice seguente mostra una regola CSS molto semplice che permetterebbe di ottenere questo risultato:

```css
h1 {
  color: red;
  font-size: 2.5em;
}
```

- Nell'esempio precedente, la regola CSS inizia con un {{Glossary("CSS_Selector", "selettore")}}. Questo _seleziona_ gli elementi HTML a cui verrà applicato lo stile. In questo caso, vengono applicati stili alle intestazioni di livello uno (`{{htmlelement("Heading_Elements", "&lt;h1>")}}`).
- Viene quindi incluso un insieme di parentesi graffe (`{ }`) per creare un **blocco di dichiarazioni**.
- Il blocco di dichiarazioni contiene una o più **dichiarazioni**, che assumono la forma di coppie **proprietà** e **valore**. La proprietà viene specificata prima dei due punti (ad esempio, `color` nell'esempio precedente), mentre il valore della proprietà viene specificato dopo i due punti (`red` è il valore impostato per la proprietà `color`).
- Questo esempio contiene due dichiarazioni: una per `color` e un'altra per `font-size`.

Diverse {{Glossary("property/CSS", "proprietà")}} CSS hanno valori consentiti diversi. Nell'esempio è presente la proprietà `color`, che può accettare vari [valori di colore](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#color). È presente anche la proprietà `font-size`. Questa proprietà può accettare varie [unità di misura](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#numbers_lengths_and_percentages) come valore.

Un foglio di stile CSS contiene molte regole di questo tipo, scritte una dopo l'altra.

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

Alcuni valori verranno appresi rapidamente, mentre per altri sarà necessario effettuare una ricerca. Le singole pagine delle proprietà su MDN offrono un modo rapido per cercare proprietà e relativi valori.

> [!NOTE]
> I link a tutte le pagine delle proprietà CSS, insieme ad altre funzionalità CSS, sono elencati nel [riferimento CSS](/it/docs/Web/CSS/Reference) di MDN. In alternativa, è consigliabile abituarsi a cercare "mdn _nome-funzionalità-css_" nel motore di ricerca preferito ogni volta che è necessario trovare ulteriori informazioni su una funzionalità CSS. Ad esempio, provare a cercare "mdn color" o "mdn font-size"!

## Come viene applicato CSS a HTML?

Come spiegato in [Come i browser caricano i siti web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites), quando si naviga verso una pagina web, il browser riceve innanzitutto il documento HTML contenente il contenuto della pagina web e lo converte in un **albero DOM**.

Successivamente, tutte le regole CSS trovate nella pagina web, inserite direttamente nell'HTML oppure in file `.css` esterni referenziati, vengono ordinate in diversi "contenitori", in base ai diversi elementi a cui saranno applicate (come specificato dai relativi selettori). Le regole CSS vengono quindi applicate all'albero DOM, producendo un **render tree**, che viene poi disegnato nella finestra del browser.

Vediamo un esempio. Innanzitutto, definiamo uno snippet HTML a cui potrebbe essere applicato il CSS:

```html
<h1>CSS is great</h1>

<p>You can style text.</p>

<p>And create layouts and special effects.</p>
```

Ora, il CSS, ripetuto dalla sezione precedente:

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

- Seleziona tutti gli elementi `<h1>` della pagina, colorando il loro testo di rosso e rendendoli più grandi della dimensione predefinita. Poiché nell'HTML di esempio è presente un solo `<h1>`, solo quell'elemento riceverà lo stile.
- Seleziona tutti gli elementi `<p>` della pagina, assegnando loro un colore personalizzato per il testo e lo sfondo, nonché dello spazio attorno al testo. Nell'HTML di esempio sono presenti due elementi `<p>` ed entrambi ricevono lo stile.

Quando il CSS viene applicato all'HTML, l'output renderizzato è il seguente:

{{EmbedLiveSample('How is CSS applied to HTML?', '100%', 200)}}

## Sperimentare con CSS

Provare a sperimentare con l'esempio precedente. Per farlo, premere il pulsante "Play" nell'angolo superiore destro per caricarlo nel nostro editor MDN Playground.

Procedere come segue:

1. Aggiungere un altro paragrafo di testo sotto i due esistenti e notare come la seconda regola CSS venga applicata automaticamente al nuovo paragrafo.
2. Aggiungere una sottointestazione `<h2>` da qualche parte sotto l'`<h1>`, magari dopo uno dei paragrafi.
3. Provare ad assegnare agli elementi `<h2>` un colore diverso aggiungendo una nuova regola al CSS. Copiare la regola `h1`, modificare il selettore in `h2` e cambiare il valore di `color` da `red` a `purple`, ad esempio.
4. Per chi desidera sperimentare ulteriormente, provare a cercare alcune nuove proprietà e valori CSS nel [riferimento CSS](/it/docs/Web/CSS/Reference) di MDN da aggiungere alle regole.

Per ulteriore pratica con le nozioni di base di CSS, consultare [Scrivi le tue prime righe di CSS!](https://scrimba.com/learn-html-and-css-c0p/~0j?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>. Questo scrim offre una panoramica utile della sintassi CSS di base e propone una sfida interattiva in cui è possibile fare ulteriore pratica scrivendo dichiarazioni CSS.

## Riepilogo

Ora che è stata acquisita una certa comprensione di cosa sia CSS e di come funzioni, passiamo a fare pratica scrivendo CSS e a spiegare la sintassi in modo più dettagliato.

{{NextMenu("Learn_web_development/Core/Styling_basics/Getting_started", "Learn_web_development/Core/Styling_basics")}}
