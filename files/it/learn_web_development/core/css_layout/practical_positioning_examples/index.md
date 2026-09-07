---
title: Esempi pratici di posizionamento
slug: Learn_web_development/Core/CSS_layout/Practical_positioning_examples
l10n:
  sourceCommit: 886f2641ae90a70858c5e7d0d20959c70ee44d9d
---

Questo articolo mostra come creare alcuni esempi reali per illustrare il tipo di operazioni che è possibile svolgere con il posizionamento.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >), e un'idea di come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo stile CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Comprendere gli aspetti pratici del posizionamento</td>
    </tr>
  </tbody>
</table>

## Un riquadro informativo a schede

Il primo esempio che esamineremo è un classico riquadro informativo a schede, una funzionalità molto comune quando si desidera inserire molte informazioni in uno spazio ridotto. Include applicazioni ricche di informazioni, come giochi strategici o di guerra, versioni mobili di siti web in cui lo schermo è stretto e lo spazio limitato, e riquadri informativi compatti nei quali si potrebbe voler rendere disponibili molte informazioni senza riempire l'intera UI. Il nostro semplice esempio avrà questo aspetto una volta completato:

![La scheda 1 è selezionata. "Tab 2" e "Tab 3" sono le altre due schede. Sono visibili solo i contenuti della scheda selezionata. Quando viene selezionata una scheda, il colore del testo cambia da nero a bianco e il colore di sfondo cambia da rosso-arancio a marrone sella.](tabbed-info-box.png)

> [!NOTE]
> È possibile vedere l'esempio completato in esecuzione su [tabbed-info-box.html](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/tabbed-info-box.html) ([codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html)). Consultarlo per farsi un'idea di ciò che verrà realizzato in questa sezione dell'articolo.

Si potrebbe pensare: "Perché non creare le diverse schede come pagine web separate e fare in modo che le schede rimandino alle singole pagine per creare l'effetto?" Questo codice sarebbe più semplice, sì, ma ogni visualizzazione di "pagina" separata sarebbe in realtà una pagina web appena caricata, rendendo più difficile salvare informazioni tra le visualizzazioni e integrare questa funzionalità in un design UI più ampio.

Per iniziare, creare una copia locale dei file iniziali: [tabbed-info-box-start.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box-start.html) e [tabs-manual.js](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabs-manual.js). Salvarli in una posizione appropriata sul computer locale e aprire `tabbed-info-box-start.html` nell'editor di testo. Vediamo l'HTML contenuto all'interno del body:

```html
<section class="info-box">
  <div role="tablist" class="manual">
    <button
      id="tab-1"
      type="button"
      role="tab"
      aria-selected="true"
      aria-controls="tabpanel-1">
      <span>Tab 1</span>
    </button>

    <button
      id="tab-2"
      type="button"
      role="tab"
      aria-selected="false"
      aria-controls="tabpanel-2">
      <span>Tab 2</span>
    </button>
    <button
      id="tab-3"
      type="button"
      role="tab"
      aria-selected="false"
      aria-controls="tabpanel-3">
      <span>Tab 3</span>
    </button>
  </div>

  <div class="panels">
    <article id="tabpanel-1" role="tabpanel" aria-labelledby="tab-1">
      <h2>The first tab</h2>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque
        turpis nibh, porttitor nec venenatis eu, pulvinar in augue. Vestibulum
        et orci scelerisque, vulputate tellus quis, lobortis dui. Vivamus varius
        libero at ipsum mattis efficitur ut nec nisl. Nullam eget tincidunt
        metus. Donec ultrices, urna maximus consequat aliquet, dui neque
        eleifend lorem, a auctor libero turpis at sem. Aliquam ut porttitor
        urna. Nulla facilisi.
      </p>
    </article>

    <article id="tabpanel-2" role="tabpanel" aria-labelledby="tab-2">
      <h2>The second tab</h2>
      <p>
        This tab hasn't got any Lorem Ipsum in it. But the content isn't very
        exciting all the same.
      </p>
    </article>

    <article id="tabpanel-3" role="tabpanel" aria-labelledby="tab-3">
      <h2>The third tab</h2>
      <p>
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Pellentesque
        turpis nibh, porttitor nec venenatis eu, pulvinar in augue. And now an
        ordered list: how exciting!
      </p>
      <ol>
        <li>dui neque eleifend lorem, a auctor libero turpis at sem.</li>
        <li>Aliquam ut porttitor urna.</li>
        <li>Nulla facilisi</li>
      </ol>
    </article>
  </div>
</section>
```

Qui è presente un elemento {{htmlelement("section")}} con una `class` pari a `info-box`, che contiene due {{htmlelement("div")}}. Il primo div contiene tre pulsanti, che diventeranno le effettive schede su cui fare clic per visualizzare i pannelli dei contenuti. Il secondo div contiene tre elementi {{htmlelement("article")}}, che costituiranno i pannelli dei contenuti corrispondenti a ogni scheda. Ogni pannello contiene alcuni contenuti di esempio.

L'idea è applicare alle schede uno stile che le faccia sembrare un normale menu di navigazione orizzontale e applicare ai pannelli uno stile che li faccia sovrapporre tramite il posizionamento assoluto. Verrà inoltre fornito del JavaScript da includere nella pagina per visualizzare il pannello corrispondente quando viene premuta una scheda e per applicare lo stile alla scheda stessa. Non è necessario comprendere il codice JavaScript in questa fase, ma è consigliabile imparare alcune nozioni di base di [JavaScript](/it/docs/Learn_web_development/Getting_started/Your_first_website/Adding_interactivity) il prima possibile: più complesse diventano le funzionalità UI, maggiore è la probabilità che sia necessario JavaScript per implementare la funzionalità desiderata.

### Configurazione generale

Per iniziare, aggiungere quanto segue tra i tag di apertura e chiusura {{HTMLElement("style")}}:

```css
html {
  font-family: sans-serif;
}

* {
  box-sizing: border-box;
}

body {
  margin: 0;
}
```

Si tratta semplicemente di una configurazione generale per impostare un font sans-serif nella pagina, utilizzare il modello {{cssxref("box-sizing")}} `border-box` ed eliminare il margine predefinito di {{htmlelement("body")}}.

Successivamente, aggiungere quanto segue subito sotto il CSS precedente:

```css
.info-box {
  width: 452px;
  height: 400px;
  margin: 1.25rem auto 0;
}
```

Questo imposta una larghezza e un'altezza specifiche per il contenuto e lo centra sullo schermo utilizzando il vecchio `margin: 1.25rem auto 0`. In precedenza nel corso, è stato consigliato di non impostare un'altezza fissa sui contenitori di contenuto, se possibile; in questa circostanza va bene perché le schede hanno contenuti fissi.

### Applicare lo stile alle schede

Ora vogliamo applicare alle schede uno stile che le faccia sembrare schede. In pratica, sono un menu di navigazione orizzontale, ma invece di caricare pagine web diverse quando si fa clic su di esse, come visto in precedenza nel corso, fanno visualizzare pannelli diversi nella stessa pagina. Per prima cosa, aggiungere la seguente regola alla fine del CSS per rendere `tablist` un contenitore {{cssxref("flex")}} e fargli occupare il 100% della larghezza:

```css
.info-box [role="tablist"] {
  min-width: 100%;
  display: flex;
}
```

> [!NOTE]
> In questo esempio vengono usati selettori discendenti con `.info-box` all'inizio della catena: questo permette di inserire questa funzionalità in una pagina che contiene già altri contenuti, senza timore di interferire con gli stili applicati ad altre parti della pagina.

Successivamente, applicheremo ai pulsanti uno stile che li faccia sembrare schede. Aggiungere il seguente CSS:

```css
.info-box [role="tab"] {
  padding: 0 1rem;
  line-height: 3rem;
  background: white;
  color: #b60000;
  font-weight: bold;
  border: none;
  outline: none;
}
```

Successivamente, imposteremo gli stati `:focus` e `:hover` delle schede affinché abbiano un aspetto diverso quando ricevono il focus o vi passa sopra il puntatore, fornendo agli utenti un feedback visivo.

```css
.info-box [role="tab"]:focus span,
.info-box [role="tab"]:hover span {
  outline: 1px solid blue;
  outline-offset: 6px;
  border-radius: 4px;
}
```

Quindi imposteremo una regola che evidenzia una delle schede quando la proprietà [`aria-selected`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-selected) è impostata su `true`. Questa verrà impostata tramite JavaScript quando si fa clic su una scheda. Inserire il seguente CSS sotto gli altri stili:

```css
.info-box [role="tab"][aria-selected="true"] {
  background-color: #b60000;
  color: white;
}
```

### Applicare lo stile ai pannelli

Il passo successivo consiste nell'applicare lo stile ai pannelli. Iniziamo!

Prima di tutto, aggiungere la seguente regola per applicare lo stile al contenitore {{htmlelement("div")}} `.panels`. Qui viene impostata un'altezza {{cssxref("height")}} fissa per garantire che i pannelli si adattino perfettamente al riquadro informativo, {{cssxref("position")}} `relative` per impostare il {{htmlelement("div")}} come contesto di posizionamento, in modo da poter poi posizionare gli elementi figli posizionati rispetto a esso e non rispetto alla viewport iniziale, e infine viene applicato {{cssxref("clear")}} al float impostato nel CSS precedente affinché non interferisca con il resto del layout.

```css
.info-box .panels {
  height: 352px;
  clear: both;
  position: relative;
}
```

Infine, per questa sezione, applicheremo lo stile ai singoli elementi {{htmlelement("article")}} che compongono i pannelli. La prima regola aggiunta applicherà {{cssxref("position")}} assoluto ai pannelli e farà sì che siano tutti allineati a {{cssxref("top")}} e {{cssxref("left")}} del rispettivo contenitore {{htmlelement("div")}}. Questa parte è fondamentale per l'intera funzionalità di layout, perché fa sì che i pannelli si sovrappongano. La regola assegna inoltre ai pannelli la stessa altezza fissa del contenitore e attribuisce al contenuto padding, un {{cssxref("color")}} del testo e un {{cssxref("background-color")}}.

```css
.info-box [role="tabpanel"] {
  background-color: #b60000;
  color: white;
  position: absolute;
  padding: 0.8rem 1.2rem;
  height: 352px;
  top: 0;
  left: 0;
}
```

La seconda regola aggiunta fa sì che un pannello con una class `is-hidden` impostata venga nascosto. Anche in questo caso, questa class verrà aggiunta o rimossa tramite JavaScript al momento opportuno. Quando viene selezionata una scheda, la class `is-hidden` verrà rimossa dal pannello corrispondente e impostata su tutti gli altri pannelli; di conseguenza, sarà visibile un solo pannello alla volta.

```css
.info-box [role="tabpanel"].is-hidden {
  display: none;
}
```

### JavaScript

La parte finale che fa funzionare questa funzionalità è il codice JavaScript. Il file `tabs-manual.js` è stato incluso mediante il tag [`<script>`](/it/docs/Web/HTML/Reference/Elements/script):

```html
<script src="tabs-manual.js"></script>
```

Questo codice esegue quanto segue:

- All'[evento di caricamento della window](/it/docs/Web/API/Window/load_event), inizializza la [class](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript) `TabsManual` per tutti gli elementi `tablist`.
- Quando viene creato un oggetto `TabsManual`, nel costruttore vengono raccolti tutti i riferimenti alle schede e ai pannelli nelle variabili `tabs` e `tabpanels`, in modo da poter eseguire facilmente operazioni su di essi in seguito.
- Il costruttore registra inoltre gestori di eventi [`click`](/it/docs/Web/API/Element/click_event) e [`keydown`](/it/docs/Web/API/Element/keydown_event) su tutte le schede. I gestori di eventi includono la logica relativa a ciò che deve accadere quando viene selezionata una scheda mediante un clic o la pressione di un tasto.
- Nella funzione `setSelectedTab(currentTab)`, avviene quanto segue:
  - Viene utilizzato un ciclo `for` per scorrere tutte le schede e deselezionarle impostando la proprietà `aria-selected` su `false` e impostando la class `is-hidden` sui pannelli corrispondenti.
  - Sulla scheda selezionata (`currentTab`), `aria-selected` viene impostato su `true` e la class `is-hidden` viene rimossa dal pannello corrispondente.

- Il codice contiene inoltre la logica per supportare la navigazione tramite tastiera usando i tasti `Left arrow`, `Right arrow`, `Home` e `End`.

## Un riquadro informativo a schede in posizione fissa

Nel secondo esempio, prenderemo il primo esempio, il riquadro informativo, e lo aggiungeremo nel contesto di una pagina web completa. Non solo: gli assegneremo una posizione fissa, affinché rimanga nella stessa posizione nella finestra del browser. Quando il contenuto principale scorre, il riquadro informativo rimarrà nella stessa posizione sullo schermo. L'esempio completato avrà questo aspetto:

![Il riquadro informativo è un contenitore con 3 schede, con la prima scheda selezionata e solo i contenuti della prima scheda visualizzati. Gli viene assegnata una posizione fissa. Il riquadro informativo è posizionato nell'angolo superiore sinistro della finestra con una larghezza di 452 pixel. Un contenitore di contenuti fittizi occupa la restante metà destra della finestra; il contenitore di contenuti fittizi è più alto della finestra e può essere fatto scorrere. Quando la pagina scorre, il contenitore sul lato destro si sposta mentre il riquadro informativo rimane fisso nella stessa posizione sullo schermo.](fixed-info-box.png)

> [!NOTE]
> È possibile vedere l'esempio completato in esecuzione su [fixed-info-box.html](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/fixed-info-box.html) ([codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/fixed-info-box.html)). Consultarlo per farsi un'idea di ciò che verrà realizzato in questa sezione dell'articolo.

Come punto di partenza, è possibile utilizzare l'esempio completato nella prima sezione dell'articolo oppure creare una copia locale di [tabbed-info-box.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html) dal repository GitHub.

### Aggiunte HTML

Prima di tutto, è necessario aggiungere dell'HTML per rappresentare il contenuto principale della pagina web. Aggiungere il seguente elemento {{htmlelement("section")}} subito sotto il tag di apertura {{htmlelement("body")}}, prima della sezione esistente:

```html
<section class="fake-content">
  <h1>Fake content</h1>
  <p>
    This is fake content. Your main web page contents would probably go here.
  </p>
  <p>
    This is fake content. Your main web page contents would probably go here.
  </p>
  <p>
    This is fake content. Your main web page contents would probably go here.
  </p>
  <p>
    This is fake content. Your main web page contents would probably go here.
  </p>
  <p>
    This is fake content. Your main web page contents would probably go here.
  </p>
  <p>
    This is fake content. Your main web page contents would probably go here.
  </p>
  <p>
    This is fake content. Your main web page contents would probably go here.
  </p>
  <p>
    This is fake content. Your main web page contents would probably go here.
  </p>
</section>
```

> [!NOTE]
> È possibile sostituire liberamente il contenuto fittizio con contenuto reale.

### Modifiche al CSS esistente

Successivamente, è necessario apportare alcune piccole modifiche al CSS esistente per posizionare il riquadro informativo. Modificare la regola `.info-box` per rimuovere `margin: 0 auto;` (non vogliamo più che il riquadro informativo sia centrato), aggiungere {{cssxref("position", "position: fixed;")}} e fissarlo a {{cssxref("top")}} della viewport del browser.

Ora dovrebbe avere questo aspetto:

```css
.info-box {
  width: 452px;
  height: 400px;
  margin: 0 auto;
  position: fixed;
  top: 0;
}
```

### Applicare lo stile al contenuto principale

L'unica cosa rimasta per questo esempio è fornire al contenuto principale alcuni stili. Aggiungere la seguente regola sotto il resto del CSS:

```css
.fake-content {
  background-color: #a60000;
  color: white;
  padding: 10px;
  height: 2000px;
  margin-left: 470px;
}

.fake-content p {
  margin-bottom: 200px;
}
```

Per iniziare, al contenuto vengono assegnati lo stesso {{cssxref("background-color")}}, {{cssxref("color")}} e {{cssxref("padding")}} dei pannelli del riquadro informativo. Gli viene poi assegnato un grande {{cssxref("margin-left")}} per spostarlo verso destra e creare spazio per il riquadro informativo, affinché non si sovrapponga ad altri elementi.

Questo conclude il secondo esempio; si spera che il terzo risulti altrettanto interessante.

## Un pannello nascosto scorrevole

L'ultimo esempio presentato qui è un pannello che scorre dentro e fuori dallo schermo alla pressione di un'icona. Come accennato in precedenza, questa soluzione è diffusa in situazioni come i layout mobili, dove lo spazio disponibile sullo schermo è ridotto e non si desidera utilizzarne la maggior parte mostrando un menu o un pannello informativo invece del contenuto utile.

L'esempio completato avrà questo aspetto:

![Uno schermo vuoto nel 60% sinistro dello schermo, con un pannello largo il 40% che visualizza informazioni sulla destra. Nell'angolo superiore destro è presente un'icona con un punto interrogativo. Il pannello scorre dentro e fuori dallo schermo alla pressione di questa icona con punto interrogativo.](hidden-sliding-panel.png)

> [!NOTE]
> È possibile vedere l'esempio completato in esecuzione su [hidden-info-panel.html](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/hidden-info-panel.html) ([codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/hidden-info-panel.html)). Consultarlo per farsi un'idea di ciò che verrà realizzato in questa sezione dell'articolo.

Come punto di partenza, creare una copia locale di [hidden-info-panel-start.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/hidden-info-panel-start.html) dal repository GitHub. Questo esempio non prosegue dal precedente, quindi è richiesto un nuovo file iniziale. Vediamo l'HTML nel file:

```html-nolint
<button
  type="button"
  id="menu-button"
  aria-haspopup="true"
  aria-controls="info-panel"
  aria-expanded="false">
      ❔
</button>

<aside id="info-panel" aria-labelledby="menu-button">
  …
</aside>
```

Per iniziare, è presente un elemento {{htmlelement("button")}} con uno speciale carattere punto interrogativo come testo del pulsante. Il pulsante verrà premuto per mostrare o nascondere il pannello informativo [`aside`](/it/docs/Web/HTML/Reference/Elements/aside). Nelle sezioni seguenti verrà spiegato come funziona il tutto.

### Applicare lo stile al pulsante

Per prima cosa, occupiamoci del pulsante. Aggiungere il seguente CSS tra i tag {{htmlelement("style")}}:

```css
#menu-button {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  z-index: 1;

  font-size: 3rem;
  cursor: pointer;
  border: none;
  background-color: transparent;
}
```

La prima regola applica lo stile a `<button>`; qui sono state eseguite le seguenti operazioni:

- È stato impostato un grande {{cssxref("font-size")}} per rendere l'icona bella grande.
- Sono stati rimossi il bordo e reso trasparente lo sfondo, in modo che venga mostrata soltanto l'icona `?` invece del pulsante.
- È stato impostato {{cssxref("position")}} `absolute` e sono stati utilizzati {{cssxref("top")}} e {{cssxref("right")}} per posizionarlo correttamente nell'angolo superiore destro.
- È stato impostato un {{cssxref("z-index")}} pari a 1: questo serve affinché, quando il pannello informativo viene stilizzato e mostrato, non copra l'icona. L'icona si troverà invece sopra di esso, così da poter essere premuta di nuovo per nascondere il pannello informativo.
- È stata usata la proprietà {{cssxref("cursor")}} per cambiare il cursore del mouse in un puntatore a mano quando passa sopra l'icona, come quello visibile al passaggio del puntatore sui collegamenti, fornendo agli utenti un ulteriore indizio visivo che l'icona svolge un'azione interessante.

### Applicare lo stile al pannello

Ora è il momento di applicare lo stile al pannello scorrevole vero e proprio. Aggiungere la seguente regola alla fine del CSS:

```css
#info-panel {
  background-color: #a60000;
  color: white;

  width: 340px;
  height: 100%;
  padding: 0 20px;

  position: fixed;
  top: 0;
  right: -370px;

  transition: 0.6s right ease-out;
}
```

Qui accadono molte cose: esaminiamole una alla volta.

- Prima di tutto, vengono impostati semplici {{cssxref("background-color")}} e {{cssxref("color")}} sul riquadro informativo.
- Successivamente, viene impostata una {{cssxref("width")}} fissa sul pannello e la sua {{cssxref("height")}} viene resa uguale all'intera altezza della viewport del browser.
- Viene inoltre incluso del {{cssxref("padding")}} orizzontale per distanziare leggermente il contenuto.
- Successivamente, viene impostato {{cssxref("position", "position: fixed;")}} sul pannello, affinché appaia sempre nello stesso punto, anche se la pagina dispone di contenuto da scorrere. Viene fissato a {{cssxref("top")}} della viewport e impostato in modo che, per impostazione predefinita, si trovi fuori schermo a {{cssxref("right")}}.
- Infine, viene impostata una {{cssxref("transition")}} sull'elemento. Transition è una funzionalità interessante che consente di far avvenire senza interruzioni le modifiche tra stati, anziché passare bruscamente da "attivo" a "inattivo". In questo caso, l'intenzione è far scorrere il pannello in modo fluido sullo schermo quando la casella di controllo è selezionata. In altre parole, quando viene fatto clic sull'icona del punto interrogativo.

### Impostare lo stato selezionato

Rimane un'ultima parte di CSS da aggiungere: inserire quanto segue alla fine del CSS:

```css
#info-panel.open {
  right: 0px;
}
```

La regola stabilisce che quando sul pannello informativo è impostata la class `.open`, la proprietà {{cssxref("right")}} di `<aside>` venga impostata a `0px`, facendo riapparire il pannello sullo schermo, in modo fluido grazie alla transition. Rimuovendo la class `.open`, il pannello viene nuovamente nascosto.

Per aggiungere o rimuovere la class `.open` dal pannello informativo con un clic del pulsante, è necessario usare JavaScript. Aggiungere il seguente codice tra i tag {{htmlelement("script")}}:

```js
const button = document.querySelector("#menu-button");
const panel = document.querySelector("#info-panel");

button.addEventListener("click", () => {
  panel.classList.toggle("open");
  button.setAttribute("aria-expanded", panel.classList.contains("open"));
});
```

Il codice aggiunge un gestore di eventi click al pulsante. Il gestore click attiva o disattiva la class `open` sul pannello del riquadro informativo, facendo scorrere il pannello dentro o fuori dalla visualizzazione.
Il gestore di eventi imposta inoltre la proprietà [`aria-expanded`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-expanded) sul pulsante per migliorare l'accessibilità.

Ecco quindi il modo più semplice per creare un effetto di pannello informativo attivabile o disattivabile.

## Riepilogo

Questo conclude l'analisi del posizionamento. A questo punto, dovrebbe essere chiaro come funzionano i meccanismi di base, oltre a come iniziare ad applicarli per creare alcune interessanti funzionalità UI. Non preoccuparti se non hai compreso subito tutto: il posizionamento è un argomento piuttosto avanzato ed è sempre possibile ripercorrere gli articoli per facilitare la comprensione.
