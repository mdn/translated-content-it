---
title: Esempi pratici di posizionamento
slug: Learn_web_development/Core/CSS_layout/Practical_positioning_examples
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo articolo mostra come costruire alcuni esempi del mondo reale per illustrare quali tipi di cose è possibile fare con il posizionamento.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Concetti base di HTML (studiare
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare contenuti con HTML</a
        >), e l'idea di Come funziona CSS (studiare
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Basi di styling CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Farsi un'idea delle praticità del posizionamento</td>
    </tr>
  </tbody>
</table>

## Un box informazioni a schede

Il primo esempio che esamineremo è un classico box informazioni a schede — una funzione molto comune utilizzata quando si vuole inserire molte informazioni in una piccola area. Questo include app ricche di informazioni come giochi di strategia/guerra, versioni mobili di siti web dove lo schermo è stretto e lo spazio limitato, e box di informazioni compatti dove potrebbe voler rendere disponibili molte informazioni senza riempire l'intera interfaccia utente. Il nostro semplice esempio sarà simile a questo una volta completato:

![Scheda 1 è selezionata. 'Scheda 2' e 'Scheda 3' sono le altre due schede. Solo i contenuti della scheda selezionata sono visibili. Quando una scheda è selezionata, il suo colore del testo cambia da nero a bianco e il colore di sfondo cambia da arancione-rosso a marrone-sella.](tabbed-info-box.png)

> [!NOTE]
> Può vedere l'esempio finito in esecuzione dal vivo su [tabbed-info-box.html](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/tabbed-info-box.html) ([codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html)). Lo controlli per farsi un'idea di cosa costruirà in questa sezione dell'articolo.

Potrebbe chiedersi: "Perché non creare semplicemente le schede separate come pagine web separate e fare in modo che le schede clicchino su queste pagine per creare l'effetto?" Questo codice sarebbe più semplice, sì, ma poi ciascuna vista "pagina" separata sarebbe in realtà una pagina web appena caricata, il che renderebbe più difficile salvare le informazioni tra le viste e integrare questa funzione in un design di UI più grande.

Per iniziare, vorremmo che faccia una copia locale dei file di partenza — [tabbed-info-box-start.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box-start.html) e [tabs-manual.js](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabs-manual.js). Salvi questi file in una posizione appropriata sul suo computer locale e apra `tabbed-info-box-start.html` nel suo editor di testo. Diamo un'occhiata all'HTML contenuto nel corpo:

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

Quindi qui abbiamo un elemento {{htmlelement("section")}} con una `class` di `info-box`, che contiene due {{htmlelement("div")}}. Il primo div contiene tre pulsanti, che diventeranno le schede effettive su cui fare clic per visualizzare i nostri pannelli di contenuti. Il secondo div contiene tre elementi {{htmlelement("article")}}, che costituiranno i pannelli di contenuti che corrispondono a ciascuna scheda. Ogni pannello contiene alcuni contenuti di esempio.

L'idea qui è che stileremo le schede per assomigliare a un normale menu di navigazione orizzontale e stileremo i pannelli in modo che si sovrappongano usando il posizionamento assoluto. Le forniremo anche un po' di JavaScript da includere nella sua pagina per visualizzare il pannello corrispondente quando viene premuta una scheda e per stilarla. Non avrà bisogno di comprendere il codice JavaScript stesso in questa fase, ma dovrebbe pensare ad imparare alcune basi di [JavaScript](/it/docs/Learn_web_development/Getting_started/Your_first_website/Adding_interactivity) il prima possibile — più le funzioni della sua UI diventano complesse, più è probabile che avrà bisogno di JavaScript per implementare la funzionalità desiderata.

### Configurazione generale

Per iniziare, aggiunga quanto segue tra le sue tag di apertura e chiusura {{HTMLElement("style")}}:

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

Questa è solo una configurazione generale per impostare un font sans-serif sulla nostra pagina, usare il modello {{cssxref("box-sizing")}} `border-box`, e rimuovere il margine di default del {{htmlelement("body")}}.

Successivamente, aggiunga quanto segue appena sotto il suo CSS precedente:

```css
.info-box {
  width: 452px;
  height: 400px;
  margin: 1.25rem auto 0;
}
```

Questo imposta una larghezza e un'altezza specifica sul contenuto e lo centra sullo schermo usando il vecchio `margin: 1.25rem auto 0`. In precedenza nel corso, abbiamo sconsigliato di impostare un'altezza fissa sui contenitori dei contenuti se possibile; è accettabile in questa circostanza perché abbiamo contenuti fissi nelle nostre schede.

### Stilizzare le schede

Ora vogliamo stilizzare le schede per farle sembrare schede — fondamentalmente, si tratta di un menu di navigazione orizzontale, ma invece di caricare diverse pagine web quando vengono cliccate come visto precedentemente nel corso, causano la visualizzazione di diversi pannelli sulla stessa pagina. Prima, aggiunga la seguente regola in fondo al suo CSS per fare della `tablist` un contenitore {{cssxref("flex")}} e farle occupare il 100% della larghezza:

```css
.info-box [role="tablist"] {
  min-width: 100%;
  display: flex;
}
```

> [!NOTE]
> Stiamo usando selettori discendenti con `.info-box` all'inizio della catena in tutto questo esempio — questo perché possiamo inserire questa funzione in una pagina con altri contenuti già presenti su di essa, senza timore di interferire con gli stili applicati ad altre parti della pagina.

Successivamente, stileremo i pulsanti per assomigliare a schede. Aggiunga il seguente CSS:

```css
.info-box [role="tab"] {
  padding: 0 1rem 0 1rem;
  line-height: 3rem;
  background: white;
  color: #b60000;
  font-weight: bold;
  border: none;
  outline: none;
}
```

Poi imposteremo gli stati `:focus` e `:hover` delle schede in modo che appaiano diversi quando sono focalizzate/hoverate, fornendo agli utenti un po' di feedback visivo.

```css
.info-box [role="tab"]:focus span,
.info-box [role="tab"]:hover span {
  outline: 1px solid blue;
  outline-offset: 6px;
  border-radius: 4px;
}
```

Dopo di che, imposteremo una regola che evidenzia una delle schede quando la proprietà [`aria-selected`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-selected) è impostata su `true` su di essa. Imposteremo questo usando JavaScript quando una scheda viene cliccata. Collochi il seguente CSS sotto ai suoi altri stili:

```css
.info-box [role="tab"][aria-selected="true"] {
  background-color: #b60000;
  color: white;
}
```

### Stilizzare i pannelli

Il prossimo compito è stilizzare i nostri pannelli. Iniziamo!

Innanzitutto, aggiunga la seguente regola per stilizzare il contenitore {{htmlelement("div")}} `.panels`. Qui impostiamo una {{cssxref("height")}} fissa per garantire che i pannelli si adattino perfettamente all'info-box, la {{cssxref("position")}} `relative` per impostare il {{htmlelement("div")}} come contesto di posizionamento, in modo che possa poi posizionare gli elementi figlio posizionati rispetto ad esso e non rispetto al viewport iniziale, e infine {{cssxref("clear")}} il float impostato nel CSS sopra in modo che non interferisca con il resto del layout.

```css
.info-box .panels {
  height: 352px;
  clear: both;
  position: relative;
}
```

Infine per questa sezione, stileremo i singoli elementi {{htmlelement("article")}} che compongono i nostri pannelli. La prima regola che aggiungeremo posizionerà assolutamente i pannelli, facendoli appoggiare tutti a filo sul {{cssxref("top")}} e sulla {{cssxref("left")}} del loro contenitore {{htmlelement("div")}} — questa parte è fondamentale per tutta questa funzione di layout, poiché fa sì che i pannelli si fondano l'uno sull'altro. La regola dà inoltre ai pannelli la stessa altezza del contenitore e fornisce al contenuto un po' di padding, un colore del testo {{cssxref("color")}}, e un {{cssxref("background-color")}}.

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

La seconda regola che aggiungeremo qui fa in modo che un pannello con una classe `is-hidden` impostata su di esso sia nascosto. Di nuovo, aggiungeremo/rimuoveremo questa classe attraverso JavaScript al momento opportuno. Quando una scheda viene selezionata, il pannello corrispondente avrà la sua classe `is-hidden` rimossa e tutti gli altri pannelli avranno la classe `is-hidden` impostata, quindi solo un pannello sarà visibile alla volta.

```css
.info-box [role="tabpanel"].is-hidden {
  display: none;
}
```

### JavaScript

La parte finale che fa funzionare questa funzione è il codice JavaScript. Il file `tabs-manual.js` è stato incluso usando il tag [`<script>`](/it/docs/Web/HTML/Reference/Elements/script):

```html
<script src="tabs-manual.js"></script>
```

Questo codice fa quanto segue:

- Al [window load event](/it/docs/Web/API/Window/load_event) inizializza `TabsManual` [class](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript) per tutti gli elementi `tablist`.
- Quando viene creato un oggetto `TabsManual`, nel costruttore vengono raccolti tutti i riferimenti alle schede e ai pannelli nei variabili `tabs` e `tabpanels`, per poter fare facilmente cose con loro in seguito.
- Il costruttore registra anche gli event handler [`click`](/it/docs/Web/API/Element/click_event) e [`keydown`](/it/docs/Web/API/Element/keydown_event) su tutte le schede. Gli event handler includono la logica su cosa dovrebbe succedere quando una scheda è selezionata utilizzando un click o un tasto.
- Nella funzione `setSelectedTab(currentTab)`, si verificano le seguenti azioni:

  - Si utilizza un ciclo `for` per scorrere tutte le schede e deselezionarle impostando la proprietà `aria-selected` a `false` e impostando la classe `is-hidden` sui pannelli corrispondenti.
  - Sulla scheda selezionata (`currentTab`) la `aria-selected` viene impostata su `true` e la classe `is-hidden` viene rimossa dal pannello corrispondente.

- Il codice include anche una logica per supportare la navigazione tramite tastiera usando i tasti `Freccia sinistra`, `Freccia destra`, `Home`, e `Fine`.

## Un box informazioni a schede a posizione fissa

Nel nostro secondo esempio, prenderemo il nostro primo esempio — il nostro info-box — e lo inseriremo nel contesto di una pagina web completa. Ma non solo — daremo ad esso una posizione fissa in modo che rimanga nella stessa posizione nella finestra del browser. Quando il contenuto principale scorre, l'info-box resterà nella stessa posizione sullo schermo. Il nostro esempio finito sarà simile a questo:

![L'info-box è un contenitore con 3 schede con la prima scheda selezionata e solo i contenuti della prima scheda sono visualizzati. Gli è stata data una posizione fissa. L'info-box è posizionato nell'angolo in alto a sinistra della finestra con una larghezza di 452 pixel. Un contenitore di contenuti fittizi occupa la restante metà destra della finestra; il contenitore di contenuti fittizi è più alto della finestra ed è scrollabile. Quando la pagina viene scrollata, il contenitore a destra si muove mentre l'info-box resta fisso nella posizione sullo schermo.](fixed-info-box.png)

> [!NOTE]
> Può vedere l'esempio finito in esecuzione dal vivo su [fixed-info-box.html](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/fixed-info-box.html) ([codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/fixed-info-box.html)). Lo controlli per farsi un'idea di cosa costruirà in questa sezione dell'articolo.

Come punto di partenza, può utilizzare il suo esempio completato dalla prima sezione dell'articolo, oppure fare una copia locale di [tabbed-info-box.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html) dal nostro repository GitHub.

### Aggiunte HTML

Innanzitutto, abbiamo bisogno di un ulteriore HTML per rappresentare il contenuto principale della pagina web. Aggiunga la seguente {{htmlelement("section")}} appena sotto il suo tag di apertura {{htmlelement("body")}}, appena prima della sezione esistente:

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
> Può sentirsi libero di cambiare i contenuti fittizi con alcuni contenuti reali se lo desidera.

### Modifiche al CSS esistente

Successivamente abbiamo bisogno di fare alcune piccole modifiche al CSS esistente, per posizionare l'info-box e posizionarlo. Cambi la sua regola `.info-box` per eliminare `margin: 0 auto;` (non vogliamo più l'info-box centrato), aggiunga {{cssxref("position", "position: fixed;")}}, e lo attacchi al {{cssxref("top")}} del viewport del browser.

Dovrebbe ora apparire così:

```css
.info-box {
  width: 452px;
  height: 400px;
  margin: 0 auto;
  position: fixed;
  top: 0;
}
```

### Stilizzare il contenuto principale

L'unica cosa che resta per questo esempio è fornire al contenuto principale un po' di stile. Aggiunga la seguente regola sotto il resto del suo CSS:

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

Iniziamo dando al contenuto lo stesso {{cssxref("background-color")}}, {{cssxref("color")}}, e {{cssxref("padding")}} dei pannelli dell'info-box. Poi gli diamo un grande {{cssxref("margin-left")}} per spostarlo verso destra, creando spazio per l'info-box in modo che non si sovrapponga a nient'altro.

Questa è la fine del secondo esempio; speriamo che troverà il terzo altrettanto interessante.

## Un pannello nascosto scorrevole

L'ultimo esempio che presenteremo qui è un pannello che scorre dentro e fuori dallo schermo con la pressione di un'icona — come accennato in precedenza, questo è popolare per situazioni come i layout mobili, dove lo spazio disponibile sullo schermo è piccolo, quindi non si vuole usarne la maggior parte mostrando un menu o pannello informativo invece del contenuto utile.

Il nostro esempio finito sarà simile a questo:

![Uno schermo vuoto a sinistra per il 60% dello schermo con un pannello di larghezza al 40% che visualizza informazioni a destra. Un'icona a 'punto interrogativo' è nell'angolo in alto a destra. Il pannello scorre dentro e fuori dallo schermo premendo questa icona a 'punto interrogativo'.](hidden-sliding-panel.png)

> [!NOTE]
> Può vedere l'esempio finito in esecuzione dal vivo su [hidden-info-panel.html](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/hidden-info-panel.html) ([codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/hidden-info-panel.html)). Lo controlli per farsi un'idea di cosa costruirà in questa sezione dell'articolo.

Come punto di partenza, faccia una copia locale di [hidden-info-panel-start.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/hidden-info-panel-start.html) dal nostro repository GitHub. Questo non segue l'esempio precedente, quindi è richiesto un nuovo file di inizio. Diamo un'occhiata all'HTML nel file:

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

Per iniziare abbiamo un elemento {{htmlelement("button")}} con un carattere speciale a punto interrogativo come testo del pulsante. Il pulsante sarà premuto per mostrare/nascondere il pannello info [`aside`](/it/docs/Web/HTML/Reference/Elements/aside). Nelle sezioni seguenti spiegheremo come funziona tutto questo.

### Stilizzare il pulsante

Prima di tutto, occupiamoci del pulsante — aggiunga il seguente CSS tra le sue tag {{htmlelement("style")}}:

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

La prima regola stila il `<button>`; qui abbiamo:

- Impostato una {{cssxref("font-size")}} grande per fare sì che l'icona venga grande.
- Rimosso il bordo e reso lo sfondo trasparente in modo che invece del pulsante venga mostrata solo l'icona `?`.
- Impostato la {{cssxref("position")}} `absolute` su di esso, e usato {{cssxref("top")}} e {{cssxref("right")}} per posizionarlo bene nell'angolo in alto a destra.
- Impostato un {{cssxref("z-index")}} di 1 su di esso — questo è in modo che, quando il pannello informativo viene stilizzato e visualizzato, non copra l'icona; invece l'icona rimarrà sopra di esso in modo da poter essere premuta di nuovo per nascondere il pannello informativo.
- Usato la proprietà {{cssxref("cursor")}} per cambiare il cursore del mouse quando si passa sopra l'icona a un puntatore a mano (come quello che si vede quando i collegamenti vengono hoverati), come ulteriore indizio visivo per gli utenti che l'icona fa qualcosa di interessante.

### Stilizzare il pannello

Ora è il momento di stilizzare il vero pannello scorrevole. Aggiunga la seguente regola alla fine del suo CSS:

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

Tante cose qui — discutiamole un pezzo alla volta:

- Innanzitutto, impostiamo un semplice {{cssxref("background-color")}} e {{cssxref("color")}} sul box informativo.
- Successivamente, impostiamo una {{cssxref("width")}} fissa sul pannello e facciamo in modo che la sua {{cssxref("height")}} sia l'altezza dell'intero viewport del browser.
- Includiamo anche un po' di {{cssxref("padding")}} orizzontale per dare spazio.
- Successivamente impostiamo {{cssxref("position", "position: fixed;")}} sul pannello in modo che appaia sempre nella stessa posizione, anche se la pagina ha contenuti da scorrere. Lo incolliamo al {{cssxref("top")}} del viewport e lo impostiamo in modo che di default sia fuori dallo schermo a destra.
- Infine, impostiamo una {{cssxref("transition")}} sull'elemento. La transizione è una caratteristica interessante che le permette di fare in modo che i cambiamenti tra stati avvengano in modo fluido, piuttosto che semplicemente passare "on" o "off" in modo brusco. In questo caso, intendiamo fare in modo che il pannello scorra delicatamente sullo schermo quando la checkbox è selezionata. (O per dirla in altro modo, quando l'icona a punto interrogativo viene cliccata.)

### Impostare lo stato selezionato

C'è un ultimo pezzetto di CSS da aggiungere — metta quanto segue alla fine del suo CSS:

```css
#info-panel.open {
  right: 0px;
}
```

La regola afferma che quando l'info-panel ha la classe `.open` impostata su di esso, imposta la proprietà {{cssxref("right")}} dell'`<aside>` a `0px`, il che fa sì che il pannello appaia di nuovo sullo schermo (in modo fluido grazie alla transizione). Rimuovere la classe `.open` nasconde di nuovo il pannello.

Per aggiungere/rimuovere la classe `.open` dal pannello info con un clic del pulsante, abbiamo bisogno di usare un po' di JavaScript. Aggiunga il seguente codice tra le tag {{htmlelement("script")}}:

```js
const button = document.querySelector("#menu-button");
const panel = document.querySelector("#info-panel");

button.addEventListener("click", () => {
  panel.classList.toggle("open");
  button.setAttribute("aria-expanded", panel.classList.contains("open"));
});
```

Il codice aggiunge un gestore di eventi click al pulsante. Il gestore di eventi click alterna la classe `open` sul pannello info-box, che fa scorrere il pannello dentro o fuori dalla vista. Il gestore di eventi imposta anche la proprietà [`aria-expanded`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-expanded) sul pulsante per migliorare l'accessibilità.

Quindi ecco qui — il modo più semplice per creare un effetto di pannello informativo toggle.

## Sommario

Quindi questo conclude il nostro sguardo al posizionamento — a questo punto, dovrebbe avere un'idea di come funzionano le meccaniche di base, oltre a capire come iniziare ad applicarle per costruire alcune funzioni interessanti di UI. Non si preoccupi se non ha capito tutto immediatamente — il posizionamento è un argomento piuttosto avanzato e può sempre lavorare attraverso gli articoli di nuovo per aiutare la sua comprensione.
