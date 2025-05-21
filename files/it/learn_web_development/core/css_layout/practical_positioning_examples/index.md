---
title: Esempi pratici di posizionamento
slug: Learn_web_development/Core/CSS_layout/Practical_positioning_examples
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo articolo mostra come costruire alcuni esempi del mondo reale per illustrare che tipo di cose si possono fare con il posizionamento.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Nozioni di base su HTML (studia
        <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >Strutturare i contenuti con HTML</a
        >), e un'idea di come funziona CSS (studia
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">Nozioni di base sullo styling CSS</a>.)
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Acquisire un'idea delle praticità del posizionamento</td>
    </tr>
  </tbody>
</table>

## Un box di informazioni con schede

Il primo esempio che esamineremo è un classico box di informazioni con schede — una funzionalità molto comune utilizzata quando si vuole racchiudere molte informazioni in un piccolo spazio. Questo include app ricche di informazioni come giochi di strategia/guerra, versioni mobili di siti web in cui lo schermo è stretto e lo spazio è limitato, e box di informazioni compatti in cui si potrebbe voler rendere disponibili molte informazioni senza riempire l'intera interfaccia utente. Il nostro semplice esempio avrà questo aspetto una volta terminato:

![La scheda 1 è selezionata. 'Scheda 2' e 'Scheda 3' sono le altre due schede. Solo i contenuti della scheda selezionata sono visibili. Quando una scheda è selezionata, il colore del testo cambia da nero a bianco e il colore di sfondo cambia da rosso-arancio a marrone-sella.](tabbed-info-box.png)

> [!NOTE]
> Puoi vedere l'esempio finito in esecuzione live su [tabbed-info-box.html](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/tabbed-info-box.html) ([codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html)). Controllalo per farti un'idea di ciò che costruirai in questa sezione dell'articolo.

Potresti pensare "Perché non creare semplicemente le schede separate come pagine web separate, e avere solo le schede che passano alle pagine separate per creare l'effetto?" Questo codice sarebbe più semplice, sì, ma ogni diversa visualizzazione "pagina" sarebbe in realtà una pagina web caricata di nuovo, il che renderebbe più difficile salvare le informazioni tra le visualizzazioni e integrare questa funzione in un progetto di interfaccia utente più ampio.

Per iniziare, ti invitiamo a fare una copia locale dei file di partenza — [tabbed-info-box-start.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box-start.html) e [tabs-manual.js](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabs-manual.js). Salva questi file in un punto adeguato sul tuo computer locale e apri `tabbed-info-box-start.html` nel tuo editor di testo. Diamo un'occhiata all'HTML contenuto nel corpo:

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

Quindi qui abbiamo un elemento {{htmlelement("section")}} con una `class` di `info-box`, che contiene due {{htmlelement("div")}}. Il primo div contiene tre pulsanti, che diventeranno le schede effettive su cui fare clic per visualizzare i nostri pannelli di contenuti. Il secondo div contiene tre elementi {{htmlelement("article")}}, che costituiranno i pannelli di contenuto corrispondenti a ciascuna scheda. Ogni pannello contiene un po' di contenuti di esempio.

L'idea qui è di stilizzare le schede per farle sembrare un menu di navigazione orizzontale standard e di stilizzare i pannelli per sovrapporsi l'un l'altro usando il posizionamento assoluto. Inoltre, forniremo un po' di JavaScript da includere nella tua pagina per visualizzare il pannello corrispondente quando viene premuta una scheda e stilizzare la scheda stessa. Non sarà necessario comprendere il codice JavaScript in questa fase, ma dovresti pensare di imparare un po' di [JavaScript](/it/docs/Learn_web_development/Getting_started/Your_first_website/Adding_interactivity) di base il prima possibile — più complessi diventano i tuoi elementi di interfaccia utente, più è probabile che avrai bisogno di un po' di JavaScript per implementare la funzionalità desiderata.

### Configurazione generale

Per iniziare, aggiungi quanto segue tra i tuoi tag di apertura e chiusura {{HTMLElement("style")}}:

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

Questa è solo una configurazione generale per impostare un font sans-serif sulla nostra pagina, utilizzare il modello {{cssxref("box-sizing")}} `border-box` e rimuovere il margine predefinito del {{htmlelement("body")}}.

Successivamente, aggiungi il seguente CSS appena sotto il precedente:

```css
.info-box {
  width: 452px;
  height: 400px;
  margin: 1.25rem auto 0;
}
```

Questo imposta una larghezza e un'altezza specifiche sul contenuto e lo centra sullo schermo utilizzando il vecchio `margin: 1.25rem auto 0`. In precedenza nel corso, avevamo consigliato di evitare di impostare un'altezza fissa sui contenitori di contenuto se possibile; è accettabile in questa circostanza perché abbiamo contenuti fissi nelle nostre schede.

### Stilizzare le nostre schede

Ora vogliamo stilizzare le schede per farle sembrare tali — fondamentalmente, queste sono un menu di navigazione orizzontale, ma invece di caricarsi su pagine web diverse quando vengono cliccate come visto in precedenza nel corso, esse causano la visualizzazione di diversi pannelli sulla stessa pagina. Per prima cosa, aggiungi la seguente regola alla fine del tuo CSS per rendere il `tablist` un contenitore {{cssxref("flex")}} e farlo occupare il 100% della larghezza:

```css
.info-box [role="tablist"] {
  min-width: 100%;
  display: flex;
}
```

> [!NOTE]
> Stiamo usando selettori discendenti con `.info-box` all'inizio della catena durante tutto questo esempio — questo è per poter inserire questa funzionalità in una pagina con altri contenuti già presenti, senza timore di interferire con gli stili applicati ad altre parti della pagina.

Successivamente, stilizzeremo i pulsanti in modo che sembrino schede. Aggiungi il seguente CSS:

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

Ora imposteremo gli stati `:focus` e `:hover` delle schede in modo che appaiano diversi quando sono focalizzati/puntati, fornendo agli utenti un feedback visivo.

```css
.info-box [role="tab"]:focus span,
.info-box [role="tab"]:hover span {
  outline: 1px solid blue;
  outline-offset: 6px;
  border-radius: 4px;
}
```

Quindi imposteremo una regola che evidenzia una delle schede quando la proprietà [`aria-selected`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-selected) è impostata su `true` su di essa. Imposteremo questo tramite JavaScript quando viene cliccata una scheda. Inserisci il seguente CSS sotto i tuoi altri stili:

```css
.info-box [role="tab"][aria-selected="true"] {
  background-color: #b60000;
  color: white;
}
```

### Stilizzare i pannelli

Il prossimo lavoro consiste nel stilizzare i nostri pannelli. Iniziamo!

Per prima cosa, aggiungi la seguente regola per stilizzare il contenitore {{htmlelement("div")}} `.panels`. Qui impostiamo un'altezza fissa {{cssxref("height")}} per garantire che i pannelli si adattino comodamente all'interno del box informazioni, {{cssxref("position")}} `relative` per impostare il {{htmlelement("div")}} come contesto di posizionamento, in modo da poter quindi posizionare elementi figli posizionati rispetto a esso e non al viewport iniziale, e infine cancelliamo {{cssxref("clear")}} il float impostato nel CSS sopra in modo che non interferisca con il resto del layout.

```css
.info-box .panels {
  height: 352px;
  clear: both;
  position: relative;
}
```

Infine per questa sezione, stileremo gli elementi {{htmlelement("article")}} individuali che costituiscono i nostri pannelli. La prima regola che aggiungeremo imposterà assolutamente {{cssxref("position")}} i pannelli, e li farà sedere tutti a filo con la parte superiore {{cssxref("top")}} e {{cssxref("left")}} del loro contenitore {{htmlelement("div")}} — questa parte è fondamentale per tutta la funzionalità del layout, poiché fa sì che i pannelli si sovrappongano. La regola imposta anche la stessa altezza del contenitore sui pannelli, e fornisce al contenuto un po' di padding, un colore del testo {{cssxref("color")}}, e un colore di sfondo {{cssxref("background-color")}}.

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

La seconda regola che aggiungeremo qui fa sì che un pannello con una classe `is-hidden` impostata su di esso sarà nascosto. Di nuovo, aggiungeremo/rimuoveremo questa classe utilizzando JavaScript al momento opportuno. Quando una scheda è selezionata, il pannello corrispondente avrà la sua classe `is-hidden` rimossa e tutti gli altri pannelli avranno impostata la classe `is-hidden`, in questo modo solo un pannello sarà visibile alla volta.

```css
.info-box [role="tabpanel"].is-hidden {
  display: none;
}
```

### JavaScript

L'ultima parte che fa funzionare questa caratteristica è il codice JavaScript. Il file `tabs-manual.js` è stato incluso utilizzando il tag [`<script>`](/it/docs/Web/HTML/Reference/Elements/script):

```html
<script src="tabs-manual.js"></script>
```

Questo codice fa quanto segue:

- All'evento di caricamento della finestra [window load event](/it/docs/Web/API/Window/load_event) inizializza la [classe](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript) `TabsManual` per tutti gli elementi `tablist`.
- Quando viene creato un oggetto `TabsManual`, nel costruttore vengono raccolti tutti i riferimenti alle schede e ai pannelli nelle variabili `tabs` e `tabpanels`, in modo da poter fare facilmente operazioni su di essi in seguito.
- Il costruttore registra anche gestori degli eventi [`click`](/it/docs/Web/API/Element/click_event) e [`keydown`](/it/docs/Web/API/Element/keydown_event) su tutte le schede. I gestori degli eventi includono la logica su cosa dovrebbe accadere quando una scheda viene selezionata tramite un clic o un press tasto.
- Nella funzione `setSelectedTab(currentTab)`, avviene quanto segue:

  - Un ciclo `for` viene utilizzato per eseguire un loop su tutte le schede e deselezionarle impostando la proprietà `aria-selected` su `false` e impostando la classe `is-hidden` sui pannelli corrispondenti.
  - Sulla scheda selezionata (`currentTab`) `aria-selected` viene impostato su `true` e la classe `is-hidden` viene rimossa dal pannello corrispondente.

- Il codice include anche la logica per supportare la navigazione tramite tastiera utilizzando i tasti `Freccia Sinistra`, `Freccia Destra`, `Home` e `End`.

## Un box di informazioni a posizione fissa

Nel nostro secondo esempio, prenderemo il nostro primo esempio — il nostro box informazioni — e lo inseriremo nel contesto di una pagina web completa. Ma non solo — gli daremo una posizione fissa in modo che rimanga nella stessa posizione nella finestra del browser. Quando il contenuto principale scorre, il box informazioni rimarrà nella stessa posizione sullo schermo. Il nostro esempio finale avrà questo aspetto:

![Il box informazioni è un contenitore con 3 schede con la prima scheda selezionata e solo i contenuti della prima scheda sono visualizzati. È stato dato una posizione fissa. Il box informazioni è posizionato nell'angolo in alto a sinistra della finestra con una larghezza di 452 pixel. Un contenitore di contenuto fasullo occupa il resto della metà destra della finestra; il contenitore di contenuto fasullo è più alto della finestra ed è scrollabile. Quando la pagina viene scrollata, il contenitore a destra si muove mentre il box informazioni rimane fisso nella stessa posizione sullo schermo.](fixed-info-box.png)

> [!NOTE]
> Puoi vedere l'esempio finito in esecuzione live su [fixed-info-box.html](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/fixed-info-box.html) ([codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/fixed-info-box.html)). Controllalo per farti un'idea di ciò che costruirai in questa sezione dell'articolo.

Come punto di partenza, puoi usare il tuo esempio completato dalla prima sezione dell'articolo, oppure fare una copia locale di [tabbed-info-box.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html) dal nostro repository GitHub.

### Aggiunte HTML

Prima di tutto, abbiamo bisogno di un po' di HTML aggiuntivo per rappresentare il contenuto principale della pagina web. Aggiungi il seguente {{htmlelement("section")}} appena sotto il tuo tag di apertura {{htmlelement("body")}}, subito prima della sezione esistente:

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
> Puoi sentirti libero di cambiare il contenuto fasullo con del contenuto reale se desideri.

### Modifiche al CSS esistente

Successivamente dobbiamo fare alcune piccole modifiche al CSS esistente, per posizionare correttamente il box informazioni. Modifica la tua regola `.info-box` per eliminare `margin: 0 auto;` (non vogliamo più che il box informazioni sia centrato), aggiungi {{cssxref("position", "position: fixed;")}}, e incollalo nella parte superiore {{cssxref("top")}} del viewport del browser.

Ora dovrebbe apparire così:

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

L'unica cosa ancora da fare per questo esempio è fornire al contenuto principale un po' di stile. Aggiungi la seguente regola sotto il resto del tuo CSS:

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

Per iniziare, diamo al contenuto lo stesso colore di sfondo {{cssxref("background-color")}}, colore {{cssxref("color")}}, e padding {{cssxref("padding")}} dei pannelli del box informazioni. Gli diamo poi un grande {{cssxref("margin-left")}} per spostarlo verso destra, creando spazio per il box informazioni, in modo che non si sovrapponga ad altro.

Questo segna la fine del secondo esempio; speriamo che trovi il terzo altrettanto interessante.

## Un pannello nascosto scorrevole

L'ultimo esempio che presenteremo qui è un pannello che scorre entro e fuori dallo schermo alla pressione di un'icona — come menzionato in precedenza, questo è popolare in situazioni come i layout mobili, dove lo spazio disponibile sullo schermo è piccolo, quindi non si vuole usarne la maggior parte mostrando un menu o un pannello informativo invece che un contenuto utile.

Il nostro esempio finale avrà questo aspetto:

![Uno schermo vuoto sulla sinistra del 60% dello schermo con un pannello di larghezza del 40% che visualizza informazioni sulla destra. Un'icona a forma di 'punto interrogativo' si trova nell'angolo in alto a destra. Il pannello scivola fuori e dentro lo schermo alla pressione di questa icona 'punto interrogativo'.](hidden-sliding-panel.png)

> [!NOTE]
> Puoi vedere l'esempio finito in esecuzione live su [hidden-info-panel.html](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/hidden-info-panel.html) ([codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/hidden-info-panel.html)). Controllalo per farti un'idea di ciò che costruirai in questa sezione dell'articolo.

Come punto di partenza, fai una copia locale di [hidden-info-panel-start.html](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/hidden-info-panel-start.html) dal nostro repository GitHub. Questo non segue dall'esempio precedente, quindi è necessaria un'inizio nuovo file. Diamo un'occhiata al codice HTML nel file:

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

Per cominciare abbiamo un elemento {{htmlelement("button")}} con un carattere speciale a punto interrogativo come testo del pulsante. Il pulsante sarà premuto per mostrare/nascondere il pannello delle informazioni [`aside`](/it/docs/Web/HTML/Reference/Elements/aside). Nelle sezioni sottostanti spiegheremo come funziona tutto ciò.

### Stilizzare il pulsante

Per prima cosa occupiamoci del pulsante — aggiungi il seguente CSS tra i tuoi tag {{htmlelement("style")}}:

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

- Impostato un grande {{cssxref("font-size")}} per rendere l'icona bella grande.
- Rimosso il bordo e reso lo sfondo trasparente in modo che invece del pulsante solo l'icona `?` sia visibile.
- Impostato la {{cssxref("position")}} `absolute` su di esso, e utilizzato {{cssxref("top")}} e {{cssxref("right")}} per posizionarlo bene nell'angolo in alto a destra.
- Impostato un {{cssxref("z-index")}} di 1 su di esso — questo serve in modo che quando il pannello informativo è stilizzato e mostrato, non copra l'icona; invece l'icona sarà posizionata sopra di esso in modo da poter essere premuta di nuovo per nascondere il pannello informativo.
- Utilizzato la proprietà {{cssxref("cursor")}} per cambiare il cursore del mouse quando è sopra l'icona a mano puntatrice (come quella che vedi quando i link sono sovrapposti), come ulteriore indizio visivo per gli utenti che l'icona fa qualcosa di interessante.

### Stilizzare il pannello

È ora di stilizzare effettivamente il pannello scorrevole stesso. Aggiungi la seguente regola in fondo al tuo CSS:

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

Molti elementi qui — discutiamoli a pezzi:

- Per prima cosa, impostiamo alcuni semplici colori di sfondo {{cssxref("background-color")}} e colore {{cssxref("color")}} sul box informativo.
- Successivamente, impostiamo una larghezza fissa {{cssxref("width")}} sul pannello, e rendiamo la sua altezza {{cssxref("height")}} l'intera altezza del viewport del browser.
- Includiamo anche un po' di padding {{cssxref("padding")}} orizzontale per spaziarlo un po'.
- Successivamente impostiamo la {{cssxref("position", "position: fixed;")}} sul pannello in modo che appaia sempre nello stesso posto, anche se la pagina ha contenuto da scorrere. Lo incolliamo in alto al viewport, e lo impostiamo in modo che per impostazione predefinita sia fuori schermo a destra {{cssxref("right")}}.
- Infine, impostiamo una {{cssxref("transition")}} sull'elemento. La transizione è una caratteristica interessante che consente di effettuare cambiamenti tra stati in modo fluido, piuttosto che passare bruscamente da "acceso" a "spento". In questo caso, intendiamo far scorrere il pannello sullo schermo quando la casella è selezionata. (O per dirlo in altro modo, quando l'icona a punto interrogativo è cliccata.)

### Impostare lo stato selezionato

C'è un ultimo pezzo di CSS da aggiungere — metti quanto segue in fondo al tuo CSS:

```css
#info-panel.open {
  right: 0px;
}
```

La regola afferma che quando il pannello informativo ha impostata su di esso la classe `.open`, imposta la proprietà {{cssxref("right")}} del `<aside>` su `0px`, il che fa sì che il pannello riappare sullo schermo (dolcemente a causa della transizione). La rimozione della classe `.open` nasconde di nuovo il pannello.

Per aggiungere/rimuovere la classe `.open` dal pannello informativo con un clic del pulsante, è necessario utilizzare un po' di JavaScript. Aggiungi il seguente codice tra i tag {{htmlelement("script")}}:

```js
const button = document.querySelector("#menu-button");
const panel = document.querySelector("#info-panel");

button.addEventListener("click", () => {
  panel.classList.toggle("open");
  button.setAttribute("aria-expanded", panel.classList.contains("open"));
});
```

Il codice aggiunge un gestore eventi click al pulsante. Il gestore del click alterna la classe `open` sul pannello del box informativo che fa scorrere il pannello dentro o fuori dalla vista. Il gestore dell'evento imposta anche la proprietà [`aria-expanded`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-expanded) sul pulsante per migliorare l'accessibilità.

Ecco fatto — il modo più semplice per creare un effetto di pannello informativo che si attiva e disattiva.

## Sommario

E così si conclude il nostro sguardo sul posizionamento — ormai dovresti avere un'idea di come funzionano le meccaniche di base, oltre a comprendere come iniziare ad applicarle per costruire alcune caratteristiche interessanti dell'interfaccia utente. Non ti preoccupare se non hai capito tutto immediatamente — il posizionamento è un argomento abbastanza avanzato, e puoi sempre ripassare gli articoli per migliorare la tua comprensione.
