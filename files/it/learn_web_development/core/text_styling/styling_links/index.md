---
title: Styled links
slug: Learn_web_development/Core/Text_styling/Styling_links
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/Text_styling")}}

Quando si stilizzano i [link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links), è importante capire perché gli stili predefiniti dei link sono importanti, come utilizzare le pseudo-classi per stilizzare efficacemente gli stati dei link e come stilizzare i link per l'uso in funzionalità di interfaccia comuni come i menu di navigazione e le schede. Esamineremo tutti questi argomenti in questo articolo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        > e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di stilizzazione CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere perché gli stili predefiniti dei link sono importanti per l'usabilità sul web — sono familiari e aiutano gli utenti a riconoscere i link.</li>
          <li>Stilizzare gli stati dei link: <code>:hover</code>, <code>:focus</code>, <code>:visited</code>, e <code>:active</code>.</li>
          <li>Comprendere perché gli stati dei link sono necessari per l'accessibilità e l'usabilità.</li>
          <li>Includere icone sui link.</li>
          <li>Creare un menu di navigazione con liste e link.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Stati dei link

La prima cosa da capire è il concetto di stati dei link — diversi stati in cui i link possono esistere. Questi possono essere stilizzati utilizzando diverse [pseudo-classi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements):

- **Link**: Un link che ha una destinazione (cioè, non solo un ancoraggio nominato), stilizzato utilizzando la pseudo-classe {{cssxref(":link")}}.
- **Visited**: Un link che è già stato visitato (presente nella cronologia del browser), stilizzato utilizzando la pseudo-classe {{cssxref(":visited")}}.
- **Hover**: Un link su cui si sta passando il mouse, stilizzato utilizzando la pseudo-classe {{cssxref(":hover")}}.
- **Focus**: Un link che è stato messo a fuoco (ad esempio, tramite un utente che utilizza la tastiera con il tasto <kbd>Tab</kbd> o qualcosa di simile, o messo a fuoco programmaticamente usando [`HTMLElement.focus()`](/it/docs/Web/API/HTMLElement/focus)) — questo è stilizzato usando la pseudo-classe {{cssxref(":focus")}}.
- **Active**: Un link che è stato attivato (ad esempio, cliccato), stilizzato utilizzando la pseudo-classe {{cssxref(":active")}}.

## Stili predefiniti

L'esempio seguente illustra come apparirà e si comporterà un link per impostazione predefinita; tuttavia, il CSS sta ingrandendo e centrando il testo per farlo risaltare di più. Può confrontare l'aspetto e il comportamento degli stili predefiniti nell'esempio con l'aspetto e il comportamento di altri link su questa pagina a cui sono stati applicati più stili CSS. I link predefiniti hanno le seguenti proprietà:

- I link sono sottolineati.
- I link non visitati sono blu.
- I link visitati sono viola.
- Passando con il mouse su un link, il puntatore del mouse cambia in una piccola icona a mano.
- I link concentrati hanno un contorno intorno a loro — è possibile concentrare i link su questa pagina con la tastiera premendo il tasto Tab.

- I link attivi sono rossi. Provi a tenere premuto il tasto destro sul link mentre lo clicca.

```html
<p><a href="#">A simple link</a></p>
```

```css
p {
  font-size: 2rem;
  text-align: center;
}
```

{{ EmbedLiveSample('Default_styles', '100%', 130) }}

> [!NOTE]
> Tutti gli esempi di link su questa pagina collegano alla parte superiore della loro finestra. Il frammento vuoto (`href="#"`) è utilizzato per creare esempi semplici e garantire che gli esempi dal vivo, ciascuno contenuto in un {{HTMLElement("iframe")}}, non si rompano.

È interessante notare che questi stili predefiniti sono quasi gli stessi di quanto erano nei primi giorni dei browser a metà degli anni '90. Questo perché gli utenti conoscono e si aspettano questo comportamento — se i link fossero stilizzati diversamente, confonderebbe molte persone. Questo non significa che non si debba stilizzare i link. Significa solo che non si dovrebbe allontanarsi troppo dal comportamento previsto. Dovrebbe almeno:

- Utilizzare la sottolineatura per i link, ma non per altre cose. Se non vuole sottolineare i link, almeno li evidenzi in qualche altro modo.
- Fare in modo che reagiscano in qualche modo quando passati o messi a fuoco, e in un modo leggermente diverso quando attivati.

Gli stili predefiniti possono essere disattivati/cambiati utilizzando le seguenti proprietà CSS:

- {{cssxref("color")}} per il colore del testo.
- {{cssxref("cursor")}} per lo stile del puntatore del mouse — non dovrebbe disattivare questo a meno che non abbia un motivo molto valido.
- {{cssxref("outline")}} per il contorno del testo. Un contorno è simile a un bordo. La differenza è che un bordo occupa spazio nella scatola e un contorno no; resta semplicemente sopra lo sfondo. Il contorno è un utile aiuto per l'accessibilità, quindi non dovrebbe essere rimosso senza aggiungere un altro metodo per indicare il link messo a fuoco.

> [!NOTE]
> Non è limitato solo alle proprietà sopra per stilizzare i suoi link — è libero di usare qualsiasi proprietà le piaccia.

## Stilizzazione dei link

Ora che abbiamo esaminato gli stati predefiniti in dettaglio, diamo un'occhiata a un tipico set di stili di link.

Per iniziare, scriviamo i nostri set di regole vuoti:

```css
a {
}

a:link {
}

a:visited {
}

a:focus {
}

a:hover {
}

a:active {
}
```

Questo ordine è importante perché gli stili dei link si costruiscono uno sull'altro. Ad esempio, gli stili nella prima regola si applicheranno a tutte le successive. Quando un link è attivato, di solito è anche sotto hovering. Se le mette nell'ordine sbagliato e sta cambiando le stesse proprietà in ciascun set di regole, le cose non funzioneranno come previsto. Per ricordare l'ordine, potrebbe utilizzare un mnemonico come **L**a **V**ostra **PA**ura.

Ora aggiungiamo alcune informazioni in più per ottenere i link stilizzati correttamente:

```css
body {
  width: 300px;
  margin: 0 auto;
  font-size: 1.2rem;
  font-family: sans-serif;
}

p {
  line-height: 1.4;
}

a {
  outline-color: transparent;
}

a:link {
  color: #6900ff;
}

a:visited {
  color: #a5c300;
}

a:focus {
  text-decoration: none;
  background: #bae498;
}

a:hover {
  text-decoration: none;
  background: #cdfeaa;
}

a:active {
  background: #6900ff;
  color: #cdfeaa;
}
```

Forniremo anche un po' di HTML di esempio a cui applicare il CSS:

```html
<p>
  There are several browsers available, such as <a href="#">Mozilla Firefox</a>,
  <a href="#">Google Chrome</a>, and <a href="#">Microsoft Edge</a>.
</p>
```

Mettendo insieme i due, otteniamo questo risultato:

{{ EmbedLiveSample('Styling_some_links', '100%', 200) }}

Quindi, cosa abbiamo fatto qui? Questo sicuramente sembra diverso dalla stilizzazione predefinita, ma fornisce ancora un'esperienza abbastanza familiare per gli utenti da capire cosa sta succedendo:

- Le prime due regole non sono così interessanti per questa discussione.
- La terza regola utilizza il selettore `a` per eliminare il contorno del focus (che comunque varia tra i browser).
- Successivamente, utilizziamo i selettori `a:link` e `a:visited` per impostare alcune variazioni di colore sui link non visitati e visitati, così sono distinti.
- Le due regole successive usano `a:focus` e `a:hover` per impostare i link concentrati e in hovering in modo che non abbiano una sottolineatura e abbiano colori di sfondo diversi.
- Infine, `a:active` viene utilizzato per fornire ai link uno schema di colori invertito mentre vengono attivati, per rendere chiaro che sta accadendo qualcosa di importante!

## Apprendimento attivo: Stili i suoi link

In questa sessione di apprendimento attivo, vorremmo che prendesse il nostro set di regole vuoto e aggiungesse le sue dichiarazioni per far sembrare i link davvero fantastici. Usi la sua immaginazione, si scateni. Siamo sicuri che riuscirà a trovare qualcosa di più fresco e altrettanto funzionale del nostro esempio sopra.

Se commette un errore, può sempre reimpostare utilizzando il pulsante _Reset_. Se è davvero bloccato, prema il pulsante _Mostra soluzione_ per inserire l'esempio che abbiamo mostrato sopra.

```html hidden
<div
  class="body-wrapper"
  style="font-family: 'Open Sans Light',Helvetica,Arial,sans-serif;">
  <h2>HTML Input</h2>
  <textarea
    id="code"
    class="html-input"
    style="width: 90%;height: 10em;padding: 10px;border: 1px solid #0095dd;">
<p>There are several browsers available, such as <a href="#">Mozilla
 Firefox</a>, <a href="#">Google Chrome</a>, and
<a href="#">Microsoft Edge</a>.</p>
  </textarea>

  <h2>CSS Input</h2>
  <textarea
    id="code"
    class="css-input"
    style="width: 90%;height: 10em;padding: 10px;border: 1px solid #0095dd;">
a {

}

a:link {

}

a:visited {

}

a:focus {

}

a:hover {

}

a:active {

}
  </textarea>

  <h2>Output</h2>
  <div
    class="output"
    style="width: 90%;height: 10em;padding: 10px;border: 1px solid #0095dd;"></div>
  <div class="controls">
    <input
      id="reset"
      type="button"
      value="Reset"
      style="margin: 10px 10px 0 0;" />
    <input
      id="solution"
      type="button"
      value="Show solution"
      style="margin: 10px 0 0 10px;" />
  </div>
</div>
```

```js hidden
const htmlInput = document.querySelector(".html-input");
const cssInput = document.querySelector(".css-input");
const reset = document.getElementById("reset");
const htmlCode = htmlInput.value;
const cssCode = cssInput.value;
const output = document.querySelector(".output");
const solution = document.getElementById("solution");

const styleElem = document.createElement("style");
const headElem = document.querySelector("head");
headElem.appendChild(styleElem);

function drawOutput() {
  output.innerHTML = htmlInput.value;
  styleElem.textContent = cssInput.value;
}

reset.addEventListener("click", () => {
  htmlInput.value = htmlCode;
  cssInput.value = cssCode;
  drawOutput();
});

solution.addEventListener("click", () => {
  htmlInput.value = htmlCode;
  cssInput.value = `p {
  font-size: 1.2rem;
  font-family: sans-serif;
  line-height: 1.4;
}

a {
  outline-color: transparent;
  text-decoration: none;
  padding: 2px 1px 0;
}

a:link {
  color: #265301;
}

a:visited {
  color: #437A16;
}

a:focus {
  border-bottom: 1px solid;
  background: #BAE498;
}

a:hover {
  border-bottom: 1px solid;
  background: #CDFEAA;
}

a:active {
  background: #265301;
  color: #CDFEAA;
}`;
  drawOutput();
});

htmlInput.addEventListener("input", drawOutput);
cssInput.addEventListener("input", drawOutput);
window.addEventListener("load", drawOutput);
```

{{ EmbedLiveSample('Active_learning_Style_your_own_links', 700, 800) }}

## Includere icone sui link

Una pratica comune è includere icone sui link per fornire più di un'indicazione sul tipo di contenuto a cui il link punta. Diamo un'occhiata a un esempio molto semplice che aggiunge un'icona nei link esterni (link che portano ad altri siti). Un'icona del genere di solito appare come una piccola freccia che punta fuori da una scatola. Per questo esempio, utilizzeremo l'[icona del link esterno di icons8.com](https://icons8.com/icon/741/external-link).

Diamo un'occhiata a qualche HTML e CSS che ci darà l'effetto desiderato. Innanzitutto, un po' di HTML semplice da stilizzare:

```html-nolint
<p>
  For more information on the weather, visit our <a href="#">weather page</a>,
  look at <a href="https://en.wikipedia.org/">weather on Wikipedia</a>, or check
  out
  <a href="https://www.nationalgeographic.org/topics/resource-library-weather/">
    weather on National Geographic</a>.
</p>
```

Successivamente, il CSS:

```css
body {
  width: 300px;
  margin: 0 auto;
  font-family: sans-serif;
}

a[href^="http"]::after {
  content: "";
  display: inline-block;
  width: 0.8em;
  height: 0.8em;
  margin-left: 0.25em;

  background-size: 100%;
  background-image: url("external-link-52.png");
}
```

{{ EmbedLiveSample('Including_icons_on_links', '100%', 150) }}

Quindi, cosa sta succedendo qui? Saltando oltre la maggior parte del CSS, poiché si tratta delle stesse informazioni che ha già osservato, l'ultima regola è interessante: stiamo utilizzando il {{cssxref("::after")}} pseudo-elemento. Il pseudo-elemento `0.8rem x 0.8rem` viene inserito dopo il testo dell'ancora come blocco in linea. E l'icona viene resa come {{cssxref("background")}} del pseudo-elemento.

Abbiamo utilizzato un'[ unità relativa ](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#relative_length_units) `em`. Dimensiona l'icona in proporzione alla dimensione del testo dell'ancora. Se la dimensione del testo dell'ancora cambia, anche la dimensione dell'icona si adatta di conseguenza.

Una parola finale: come abbiamo selezionato solo i link esterni? Bene, se sta scrivendo correttamente i suoi [link HTML](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links), dovrebbe utilizzare solo URL assoluti per i link esterni — è più efficiente utilizzare link relativi per collegarsi ad altre parti del proprio sito (come con il primo link). Pertanto, il testo "http" dovrebbe apparire solo nei link esterni (come il secondo e il terzo), e possiamo selezionarlo con un [selettore di attributi](/it/docs/Learn_web_development/Core/Styling_basics/Attribute_selectors): `a[href^="http"]` seleziona gli elementi {{htmlelement("a")}}, ma solo se hanno un attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) con un valore che inizia per "http".

Quindi, è tutto. Provi a rivisitare la sezione di apprendimento attivo sopra e provare questa nuova tecnica!

> [!NOTE]
> Non si preoccupi se non è ancora familiare con [gli sfondi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders) e il [design reattivo](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design); questi sono spiegati in altri luoghi.

## Stilizzazione dei link come pulsanti

Gli strumenti che ha esplorato finora in questo articolo possono anche essere utilizzati in altri modi. Ad esempio, stati come quello di hover possono essere utilizzati per stilizzare molti elementi diversi, non solo i link — potrebbe voler stilizzare lo stato di hover dei paragrafi, degli elementi di lista o di altre cose.

Inoltre, i link sono abbastanza comunemente stilizzati per assomigliare e comportarsi come pulsanti in determinate circostanze. Un menu di navigazione di un sito web può essere contrassegnato come un insieme di link, e questo può essere stilizzato per apparire come un insieme di pulsanti di controllo o schede che forniscono all'utente l'accesso ad altre parti del sito. Esploriamo come.

Innanzitutto, un po' di HTML:

```html
<nav class="container">
  <a href="#">Home</a>
  <a href="#">Pizza</a>
  <a href="#">Music</a>
  <a href="#">Wombats</a>
  <a href="#">Finland</a>
</nav>
```

E ora il nostro CSS:

```css
body,
html {
  margin: 0;
  font-family: sans-serif;
}

.container {
  display: flex;
  gap: 0.625%;
}

a {
  flex: 1;
  text-decoration: none;
  outline-color: transparent;
  text-align: center;
  line-height: 3;
  color: black;
}

a:link,
a:visited,
a:focus {
  background: palegoldenrod;
  color: black;
}

a:hover {
  background: orange;
}

a:active {
  background: darkred;
  color: white;
}
```

Questo ci dà il seguente risultato:

{{ EmbedLiveSample('Styling_links_as_buttons', '100%', 120) }}

L'HTML definisce un elemento {{HTMLElement("nav")}} con una classe `"container"`. Il `<nav>` contiene i nostri link.

Il CSS include la stilizzazione per il contenitore e i link che contiene.

- La seconda regola dice:
  - Il contenitore è un [flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox). Gli elementi che contiene — i link, in questo caso — saranno _elementi flessibili_.
  - Il divario tra gli elementi flessibili sarà `0.625%` della larghezza del contenitore.
- La terza regola stilizza i link:
  - La prima dichiarazione, `flex: 1`, significa che le larghezze degli elementi saranno regolate in modo che utilizzino tutto lo spazio disponibile nel contenitore.
  - Successivamente, disattiviamo la {{cssxref("text-decoration")}} predefinita e il {{cssxref("outline")}} — non vogliamo che rovinino l'aspetto.
  - Le ultime tre dichiarazioni servono a centrare il testo all'interno di ciascun link, impostare la {{cssxref("line-height")}} su 3 per dare ai pulsanti un po' di altezza (che ha anche il vantaggio di centrare il testo verticalmente) e impostare il colore del testo su nero.

## Sommario

Speriamo che questo articolo le abbia fornito tutto ciò che ha bisogno di sapere sui link — per ora! L'articolo finale nel nostro modulo di stile del testo dettaglia come utilizzare i font personalizzati sui suoi siti web (o font web, come sono meglio conosciuti).

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/Text_styling")}}
