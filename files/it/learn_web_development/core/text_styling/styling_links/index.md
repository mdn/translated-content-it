---
title: Styling links
slug: Learn_web_development/Core/Text_styling/Styling_links
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/Text_styling")}}

Quando si stilizzano i [link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links), è importante capire perché gli stili di default dei link sono importanti, come utilizzare le pseudo-classi per stilizzare efficacemente gli stati dei link e come stilizzare i link per l'uso in comuni funzionalità di interfaccia varia, come i menu di navigazione e le schede. Esamineremo tutti questi argomenti in questo articolo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare il contenuto con HTML</a
        > e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo styling CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere perché gli stili di default dei link sono importanti per l'usabilità sul web — sono familiari e aiutano gli utenti a riconoscere i link.</li>
          <li>Stilizzare gli stati dei link: <code>:hover</code>, <code>:focus</code>, <code>:visited</code> e <code>:active</code>.</li>
          <li>Comprendere perché gli stati dei link sono necessari per accessibilità e usabilità.</li>
          <li>Includere icone sui link.</li>
          <li>Creare un menu di navigazione con liste e link.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Stati dei link

La prima cosa da comprendere è il concetto di stati dei link — i diversi stati in cui i link possono esistere. Questi possono essere stilizzati usando diverse [pseudo-classi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements):

- **Link**: Un link che ha una destinazione (cioè, non solo un'ancora nominata), stilizzato usando la pseudo-classe {{cssxref(":link")}}.
- **Visited**: Un link che è già stato visitato (esiste nella cronologia del browser), stilizzato usando la pseudo-classe {{cssxref(":visited")}}.
- **Hover**: Un link su cui è posizionato il puntatore del mouse dell'utente, stilizzato usando la pseudo-classe {{cssxref(":hover")}}.
- **Focus**: Un link che è messo a fuoco (ad esempio, raggiunto da un utente tramite tastiera usando il tasto <kbd>Tab</kbd> o qualcosa di simile, o messo a fuoco programmaticamente usando [`HTMLElement.focus()`](/it/docs/Web/API/HTMLElement/focus)) — questo è stilizzato usando la pseudo-classe {{cssxref(":focus")}}.
- **Active**: Un link che è stato attivato (ad esempio, su cui è stato fatto clic), stilizzato usando la pseudo-classe {{cssxref(":active")}}.

## Stili di default

L'esempio seguente illustra come apparirà e si comporterà un link di default; sebbene il CSS ingrandisca e centri il testo per renderlo più visibile. Puoi confrontare l'aspetto e il comportamento degli stili di default nell'esempio con l'aspetto e il comportamento di altri link su questa pagina che hanno più stili CSS applicati. I link di default hanno le seguenti proprietà:

- I link sono sottolineati.
- I link non visitati sono blu.
- I link visitati sono viola.
- Passare il mouse su un link fa cambiare il cursore a un'icona a forma di mano.
- I link a fuoco hanno un contorno intorno a loro — dovresti essere in grado di focalizzare i link su questa pagina con la tastiera premendo il tasto tab.

- I link attivi sono rossi. Prova a tenere premuto il pulsante del mouse sul link mentre fai clic.

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
> Tutti gli esempi di link su questa pagina puntano alla parte superiore della loro finestra. Il frammento vuoto (`href="#"`) è usato per creare esempi semplici e assicurare che gli esempi dal vivo, che sono ognuno contenuti in un {{HTMLElement("iframe")}}, non si rompano.

È interessante notare che questi stili di default sono quasi gli stessi di quanto fossero nei primi giorni dei browser, a metà degli anni '90. Questo perché gli utenti conoscono e si aspettano questo comportamento — se i link fossero stilizzati diversamente, confonderebbero molte persone. Questo non significa che non si debbano stilizzare i link. Significa solo che non si dovrebbe allontanarsi troppo dal comportamento atteso. Dovresti almeno:

- Usare la sottolineatura per i link, ma non per altre cose. Se non vuoi sottolineare i link, almeno evidenziali in qualche altro modo.
- Fai in modo che reagiscano in qualche modo quando sono in hover/focus, e in un modo leggermente diverso quando sono attivati.

Gli stili di default possono essere disattivati/modificati utilizzando le seguenti proprietà CSS:

- {{cssxref("color")}} per il colore del testo.
- {{cssxref("cursor")}} per lo stile del puntatore del mouse — non dovresti disattivarlo a meno che tu non abbia una ragione molto valida.
- {{cssxref("outline")}} per il contorno del testo. Un contorno è simile a un bordo. L'unica differenza è che un bordo occupa spazio nel riquadro e un contorno no; si limita a sedersi sopra lo sfondo. Il contorno è un valido ausilio per l'accessibilità, quindi non dovrebbe essere rimosso senza aggiungere un altro metodo per indicare il link focalizzato.

> [!NOTE]
> Non sei limitato solo alle proprietà sopra per stilizzare i tuoi link — sei libero di usare qualsiasi proprietà tu voglia.

## Stilizzare i link

Ora che abbiamo esaminato gli stati di default in qualche dettaglio, guardiamo a un insieme tipico di stili per i link.

Per iniziare, scriveremo i nostri set di regole vuoti:

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

Questo ordine è importante perché gli stili dei link si costruiscono uno sull'altro. Ad esempio, gli stili della prima regola si applicheranno a tutti i successivi. Quando un link è attivato, di solito è anche in hover. Se li metti nell'ordine sbagliato e stai cambiando le stesse proprietà in ogni set di regole, le cose non funzioneranno come ti aspetti. Per ricordare l'ordine, potresti provare a usare un mnemonico come **L**o**V**e **F**ears **HA**te.

Ora aggiungiamo alcune informazioni per stilizzare correttamente:

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

Forniremo anche un esempio di HTML a cui applicare il CSS:

```html
<p>
  There are several browsers available, such as <a href="#">Mozilla Firefox</a>,
  <a href="#">Google Chrome</a>, and <a href="#">Microsoft Edge</a>.
</p>
```

Unendo i due otteniamo questo risultato:

{{ EmbedLiveSample('Styling_some_links', '100%', 200) }}

Quindi, cosa abbiamo fatto qui? Questo appare certamente diverso dallo stile di default, ma fornisce ancora un'esperienza abbastanza familiare agli utenti per capire cosa stia succedendo:

- Le prime due regole non sono molto interessanti per questa discussione.
- La terza regola utilizza il selettore `a` per rimuovere il contorno del focus (che varia comunque da browser a browser).
- Successivamente, utilizziamo i selettori `a:link` e `a:visited` per impostare alcune variazioni di colore sui link non visitati e visitati, in modo che siano distinti.
- Le successive due regole utilizzano `a:focus` e `a:hover` per impostare i link focalizzati e in hover senza sottolineatura e con colori di sfondo diversi.
- Infine, `a:active` è usato per fornire ai link uno schema di colori invertito mentre vengono attivati, per rendere chiaro che sta accadendo qualcosa di importante!

## Apprendimento attivo: Stilisare i tuoi link

In questa sessione di apprendimento attivo, ti invitiamo a prendere il nostro set di regole vuoto e aggiungere le tue dichiarazioni per rendere i link davvero cool. Usa la tua immaginazione, vai alla grande. Siamo sicuri che puoi inventare qualcosa di più interessante e altrettanto funzionale del nostro esempio sopra.

Se commetti un errore, puoi sempre resettarlo usando il pulsante _Reset_. Se sei davvero bloccato, premi il pulsante _Show solution_ per inserire l'esempio che abbiamo mostrato sopra.

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

È pratica comune includere icone sui link per fornire un maggiore indicatore del tipo di contenuto a cui il link punta. Esaminiamo un esempio molto semplice che aggiunge un'icona ai link esterni (link che portano ad altri siti). Una tale icona di solito appare come una piccola freccia che esce da una scatola. Per questo esempio, useremo [l'icona di link esterno da icons8.com](https://icons8.com/icon/741/external-link).

Osserviamo un po' di HTML e CSS che ci daranno l'effetto desiderato. Prima, un semplice HTML da stilizzare:

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

Quindi, cosa sta succedendo qui? Sorvoleremo sulla maggior parte del CSS, poiché è solo la stessa informazione che hai già visto. L'ultima regola, tuttavia, è interessante: stiamo usando {{cssxref("::after")}} pseudo-elemento. Il pseudo-elemento `0.8rem x 0.8rem` viene posizionato dopo il testo dell'ancoraggio come un blocco in linea. E l'icona viene renderizzata come {{cssxref("background")}} del pseudo-elemento.

Abbiamo utilizzato un'unità [relativa](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#relative_length_units) `em`. Imposta la dimensione dell'icona in proporzione alla dimensione del testo dell'ancora. Se la dimensione del testo dell'ancora cambia, anche la dimensione dell'icona si adatta di conseguenza.

Una parola finale: come abbiamo selezionato solo i link esterni? Ebbene, se stai scrivendo correttamente i tuoi [link HTML](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links), dovresti usare solo URL assoluti per i link esterni — è più efficiente usare link relativi per collegare altre parti del tuo stesso sito (come con il primo link). Il testo "http" dovrebbe quindi apparire solo nei link esterni (come il secondo e il terzo), e possiamo selezionare questo con un [selettore di attributi](/it/docs/Learn_web_development/Core/Styling_basics/Attribute_selectors): `a[href^="http"]` seleziona gli elementi {{htmlelement("a")}}, ma solo se hanno un attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) con un valore che inizia con "http".

Quindi è tutto. Prova a rivisitare la sezione di apprendimento attivo sopra e prova questa nuova tecnica!

> [!NOTE]
> Non preoccuparti se non hai familiarità con [sfondi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders) e [design responsivo](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design) ancora; questi sono spiegati in altri luoghi.

## Stilizzare i link come pulsanti

Gli strumenti che hai esplorato finora in questo articolo possono anche essere usati in altri modi. Ad esempio, stati come hover possono essere utilizzati per stilizzare molti elementi diversi, non solo i link — potresti voler stilizzare lo stato di hover dei paragrafi, degli elementi di lista o di altre cose.

Inoltre, i link sono spesso stilizzati per sembrare e comportarsi come pulsanti in alcune circostanze. Un menu di navigazione di un sito web può essere marcato come un insieme di link, e questo può essere stilizzato per sembrare un insieme di pulsanti di controllo o schede che forniscono all'utente l'accesso ad altre parti del sito. Esploriamo come.

Prima, un po' di HTML:

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

Il CSS include lo stile per il container e i link che contiene.

- La seconda regola dice:
  - Il container è un [flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox). Gli elementi che contiene — i link, in questo caso — saranno _elementi flex_.
  - Lo spazio tra gli elementi flex sarà `0.625%` della larghezza del container.
- La terza regola stila i link:
  - La prima dichiarazione, `flex: 1`, significa che le larghezze degli elementi saranno regolate così da usare tutto lo spazio disponibile nel container.
  - Successivamente, disattiviamo la {{cssxref("text-decoration")}} e il {{cssxref("outline")}} di default — non vogliamo che rovinino il nostro look.
  - Le ultime tre dichiarazioni servono a centrare il testo dentro ogni link, impostare la {{cssxref("line-height")}} a 3 per dare ai pulsanti un po' di altezza (che ha anche il vantaggio di centrare il testo verticalmente) e impostare il colore del testo su nero.

## Sommario

Speriamo che questo articolo ti abbia fornito tutto quello che devi sapere sui link — per ora! L'articolo finale nel nostro modulo Styling text spiega come utilizzare font personalizzati sui tuoi siti web (o web font, come sono meglio conosciuti).

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/Text_styling")}}
