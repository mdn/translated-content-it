---
title: Applicare stili ai link
slug: Learn_web_development/Core/Text_styling/Styling_links
l10n:
  sourceCommit: 19c36d7464c349372e38f9b39bf3e7de73687397
---

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/Text_styling")}}

Quando si applicano stili ai [link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links), è importante comprendere perché gli stili predefiniti dei link sono importanti, come usare le pseudo-classi per applicare efficacemente stili agli stati dei link e come applicare stili ai link da usare in funzionalità comuni e varie dell'interfaccia, come menu di navigazione e schede. In questo articolo verranno esaminati tutti questi argomenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        > e
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sugli stili CSS</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere perché gli stili predefiniti dei link sono importanti per l'usabilità sul web: sono familiari e aiutano gli utenti a riconoscere i link.</li>
          <li>Applicare stili agli stati dei link: <code>:hover</code>, <code>:focus</code>, <code>:visited</code> e <code>:active</code>.</li>
          <li>Comprendere perché gli stati dei link sono necessari per l'accessibilità e l'usabilità.</li>
          <li>Includere icone nei link.</li>
          <li>Creare un menu di navigazione con elenchi e link.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Stati dei link

La prima cosa da comprendere è il concetto di stati dei link: i diversi stati in cui possono trovarsi i link. Questi possono essere stilizzati usando diverse [pseudo-classi](/it/docs/Learn_web_development/Core/Styling_basics/Pseudo_classes_and_elements):

- **Link**: un link che ha una destinazione (ovvero non soltanto un'ancora con nome), stilizzato usando la pseudo-classe {{cssxref(":link")}}.
- **Visitato**: un link che è già stato visitato (presente nella cronologia del browser), stilizzato usando la pseudo-classe {{cssxref(":visited")}}.
- **Hover**: un link su cui si trova il puntatore del mouse dell'utente, stilizzato usando la pseudo-classe {{cssxref(":hover")}}.
- **Focus**: un link che ha il focus (ad esempio, raggiunto da un utente che usa la tastiera mediante il tasto <kbd>Tab</kbd> o qualcosa di simile, oppure a cui viene assegnato il focus programmaticamente usando [`HTMLElement.focus()`](/it/docs/Web/API/HTMLElement/focus)): viene stilizzato usando la pseudo-classe {{cssxref(":focus")}}.
- **Attivo**: un link che viene attivato (ad esempio, facendo clic), stilizzato usando la pseudo-classe {{cssxref(":active")}}.

## Stili predefiniti

L'esempio seguente illustra l'aspetto e il comportamento predefiniti di un link, anche se il CSS ingrandisce e centra il testo per farlo risaltare maggiormente. È possibile confrontare l'aspetto e il comportamento dello stile predefinito nell'esempio con quelli degli altri link in questa pagina, ai quali sono applicati più stili CSS. I link predefiniti hanno le seguenti proprietà:

- I link sono sottolineati.
- I link non visitati sono blu.
- I link visitati sono viola.
- Passando sopra un link, il puntatore del mouse cambia in una piccola icona a forma di mano.
- I link con focus hanno un contorno attorno a essi: dovrebbe essere possibile assegnare il focus ai link in questa pagina con la tastiera premendo il tasto Tab.
- I link attivi sono rossi. Provare a tenere premuto il pulsante del mouse mentre si fa clic sul link.

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
> Tutti gli esempi di link in questa pagina rimandano alla parte superiore della relativa finestra incorporata. Il frammento vuoto (`href="#"`) viene usato per creare esempi semplici e garantire che gli esempi live, ciascuno contenuto in un {{HTMLElement("iframe")}}, non si interrompano.

È interessante notare che gli stili predefiniti sono quasi gli stessi di quelli dei primi browser della metà degli anni Novanta. Questo perché gli utenti conoscono e si aspettano questo comportamento: se i link avessero stili diversi, le persone si confonderebbero. Ciò non significa che non si debbano applicare stili ai link. Significa soltanto che non ci si dovrebbe discostare troppo dal comportamento previsto. Come minimo, occorre:

- Usare la sottolineatura per i link, ma non per altri elementi. Se non si desidera sottolineare i link, evidenziarli almeno in qualche altro modo.
- Fare in modo che reagiscano in qualche modo quando ricevono hover/focus, e in un modo leggermente diverso quando vengono attivati.

Gli stili predefiniti possono essere disattivati/modificati usando le seguenti proprietà CSS:

- {{cssxref("color")}} per il colore del testo.
- {{cssxref("cursor")}} per lo stile del puntatore del mouse: non dovrebbe essere disattivato senza un'ottima ragione.
- {{cssxref("outline")}} per il contorno del testo. Un contorno è simile a un bordo. L'unica differenza è che un bordo occupa spazio nel box, mentre un contorno no: i contorni si sovrappongono allo sfondo. Il contorno è un utile ausilio di accessibilità, quindi non dovrebbe essere rimosso senza aggiungere un altro metodo per indicare il link con focus.

> [!NOTE]
> Non si è limitati alle proprietà elencate sopra per stilizzare i link: è possibile usare qualsiasi proprietà desiderata.

## Applicare stili ai link

Dopo aver esplorato in dettaglio gli stati predefiniti, vediamo un insieme tipico di stili per i link.

Per iniziare, verranno scritti i set di regole vuoti:

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

Questo ordine è importante perché gli stili dei link si basano gli uni sugli altri. Ad esempio, gli stili nella prima regola verranno applicati a tutte quelle successive. Quando un link viene attivato, solitamente riceve anche hover. Se le regole sono nell'ordine sbagliato e si modificano le stesse proprietà in ogni set di regole, il risultato non sarà quello previsto. Per ricordare l'ordine, si può provare a usare un mnemonico come **L**o**V**e **F**ears **HA**te.

Ora aggiungiamo qualche altra informazione per applicare correttamente gli stili:

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

Verrà inoltre fornito dell'HTML di esempio a cui applicare il CSS:

```html
<p>
  There are several browsers available, such as <a href="#">Mozilla Firefox</a>,
  <a href="#">Google Chrome</a>, and <a href="#">Microsoft Edge</a>.
</p>
```

Combinando i due elementi si ottiene questo risultato:

{{ EmbedLiveSample('Styling_some_links', '100%', 200) }}

Cosa è stato fatto qui? L'aspetto è certamente diverso dallo stile predefinito, ma offre comunque un'esperienza sufficientemente familiare affinché gli utenti comprendano cosa sta succedendo:

- Le prime due regole non sono particolarmente interessanti per questa discussione.
- La terza regola usa il selettore `a` per rimuovere il contorno del focus, che comunque varia tra i browser.
- Successivamente, i selettori `a:link` e `a:visited` vengono usati per impostare alcune variazioni di colore sui link non visitati e visitati, in modo da distinguerli.
- Le due regole seguenti usano `a:focus` e `a:hover` per impostare i link con focus e hover senza sottolineatura e con colori di sfondo diversi.
- Infine, `a:active` viene usato per assegnare ai link una combinazione di colori invertita durante l'attivazione, rendendo chiaro che sta accadendo qualcosa di importante.

## Applicare stili ai propri link

Per questa attività, occorre prendere il set vuoto di regole e aggiungere dichiarazioni personalizzate per rendere i link davvero accattivanti. Usare l'immaginazione e sperimentare liberamente. Sicuramente è possibile creare qualcosa di più interessante e altrettanto funzionale rispetto all'esempio precedente.

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio nel playground di MDN.
2. Assegnare ai link uno stile predefinito applicato in ogni momento. Non è necessario limitarsi al colore del testo, ma assicurarsi che i link siano ancora riconoscibili come tali.
3. Assegnare ai link _visitati_ un colore leggermente diverso dagli stili predefiniti dei link impostati.
4. Assegnare agli stati _focus_ e _hover_ dei link uno stile distinto che li evidenzi rispetto agli altri link. Rimuovere inoltre la sottolineatura predefinita quando i link ricevono focus/hover.
5. Assegnare allo stato _active_ uno stile ancora diverso.

Se si commette un errore, è possibile cancellare il lavoro usando il pulsante _Reset_ nel playground di MDN. Se si resta davvero bloccati, è possibile visualizzare una soluzione di esempio sotto l'output dell'esempio.

```html-nolint live-sample___link_styling
<p>
  There are several browsers available, such as
  <a href="https://www.mozilla.org/firefox/new/" target="_blank">Mozilla Firefox</a>,
  <a href="https://www.google.co.uk/chrome/" target="_blank">Google Chrome</a>, and
  <a href="https://www.microsoft.com/edge" target="_blank">Microsoft Edge</a>.
</p>
```

```css-nolint hidden live-sample___link_styling
p {
  font-size: 1.2rem;
  font-family: sans-serif;
  line-height: 1.4;
}

```

```css live-sample___link_styling
a {
}

a:visited {
}

a:focus,
a:hover {
}

a:active {
}
```

{{ EmbedLiveSample('link_styling', "100%", 100) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il CSS completato potrebbe essere simile a questo:

```css
a {
  outline-color: transparent;
  padding: 2px 1px 0;
  color: #265301;
}

a:visited {
  color: #437a16;
}

a:hover,
a:focus {
  background: #bae498;
  text-decoration: none;
}

a:active {
  background: #265301;
  color: #cdfeaa;
}
```

</details>

## Includere icone nei link

Una pratica comune consiste nell'includere icone nei link per indicare il tipo di contenuto a cui punta il link. Vediamo un esempio di base che aggiunge un'icona ai link esterni, ovvero link che portano ad altri siti. Le icone dei link esterni sono solitamente frecce che puntano fuori da riquadri. Per questo esempio verrà usata un'[icona di link esterno da icons8.com](https://icons8.com/icon/741/external-link).

Per prima cosa, un semplice HTML a cui applicare gli stili:

```html-nolint
<p>
  For more information on the weather, visit our <a href="#">weather page</a>,
  look at <a href="https://en.wikipedia.org/">weather on Wikipedia</a>, or check
  out
  <a href="https://www.nationalgeographic.org/topics/resource-library-weather/">
    weather on National Geographic</a>.
</p>
```

Successivamente, ecco del CSS che produrrà l'effetto desiderato:

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

Cosa sta succedendo qui? Verrà tralasciata la maggior parte del CSS, poiché è uguale a quello degli esempi precedenti. L'ultima regola, tuttavia, è interessante: viene usato il selettore di pseudo-elemento {{cssxref("::after")}}. Lo pseudo-elemento `0.8em x 0.8em` viene renderizzato dopo il testo dell'ancora come blocco inline, e l'icona a cui si fa riferimento nella relativa proprietà {{cssxref("background-image")}} viene inserita nello sfondo dello pseudo-elemento. È stato inoltre incluso del {{cssxref("margin-left")}} per creare spazio tra l'icona e la parola che la precede.

Viene usata un'[unità relativa](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#relative_length_units): `em`. Essa dimensiona l'icona in proporzione alla dimensione del testo dell'ancora. Se la dimensione del testo dell'ancora cambia, anche la dimensione dell'icona si adegua di conseguenza.

Un'ultima osservazione: come sono stati selezionati soltanto i link esterni? Se si scrivono correttamente i propri [link HTML](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links), si dovrebbero usare URL assoluti soltanto per i link esterni: è più efficiente usare link relativi per collegare altre parti del proprio sito, come nel primo link. Il testo "http" dovrebbe quindi comparire soltanto nei link esterni, come nel secondo e nel terzo, e può essere selezionato con un [selettore di attributo](/it/docs/Learn_web_development/Core/Styling_basics/Attribute_selectors): `a[href^="http"]` seleziona gli elementi {{htmlelement("a")}}, ma soltanto se hanno un attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) con un valore che inizia con "http".

Questo è tutto. Provare a tornare alla sezione dell'attività precedente e sperimentare questa nuova tecnica.

> [!NOTE]
> Non preoccuparti se [sfondi](/it/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders) e [responsive web design](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design) non sono ancora familiari: vengono spiegati altrove.

## Applicare stili ai link come pulsanti

Gli strumenti esplorati finora in questo articolo possono essere usati anche in altri modi. Ad esempio, stati come hover possono essere usati per applicare stili a molti elementi diversi, non soltanto ai link: potrebbe essere utile stilizzare lo stato hover di paragrafi, elementi di elenco o altri elementi.

Inoltre, i link vengono comunemente stilizzati per comportarsi come pulsanti. Un menu di navigazione di un sito web può essere marcato come un insieme di link e stilizzato in modo da apparire come un insieme di pulsanti di controllo o schede che consentono all'utente di accedere ad altre parti del sito. Vediamo come.

Per prima cosa, dell'HTML:

```html
<nav class="container">
  <a href="#">Home</a>
  <a href="#">Pizza</a>
  <a href="#">Music</a>
  <a href="#">Wombats</a>
  <a href="#">Finland</a>
</nav>
```

E ora il CSS:

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

Questo produce il seguente risultato:

{{ EmbedLiveSample('Styling_links_as_buttons', '100%', 120) }}

L'HTML definisce un elemento {{HTMLElement("nav")}} con una classe `"container"`. Il `<nav>` contiene i link.

Il CSS include gli stili per il contenitore e per i link che contiene.

- La seconda regola stabilisce quanto segue:
  - Il contenitore è un [flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox). Gli elementi che contiene, in questo caso i link, saranno _flex item_.
  - Lo spazio tra i flex item sarà pari allo `0.625%` della larghezza del contenitore.
- La terza regola applica stili ai link:
  - La prima dichiarazione, `flex: 1`, significa che le larghezze degli elementi verranno adattate in modo da usare tutto lo spazio disponibile nel contenitore.
  - Successivamente, vengono disattivati {{cssxref("text-decoration")}} e {{cssxref("outline")}} predefiniti: non si vuole che rovinino l'aspetto desiderato.
  - Le ultime tre dichiarazioni servono a centrare il testo all'interno di ciascun link, impostare {{cssxref("line-height")}} a 3 per assegnare altezza ai pulsanti, con l'ulteriore vantaggio di centrare il testo verticalmente, e impostare il colore del testo su nero.

## Riepilogo

Si spera che questo articolo abbia fornito tutto ciò che occorre sapere sui link, almeno per ora. L'articolo finale del modulo sugli stili del testo spiega come usare font web personalizzati nei siti web.

{{PreviousMenuNext("Learn_web_development/Core/Text_styling/Styling_lists", "Learn_web_development/Core/Text_styling/Web_fonts", "Learn_web_development/Core/Text_styling")}}
