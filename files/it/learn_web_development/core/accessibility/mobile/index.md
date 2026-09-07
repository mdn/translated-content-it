---
title: Accessibilità sui dispositivi mobili
slug: Learn_web_development/Core/Accessibility/Mobile
l10n:
  sourceCommit: f99d00a1c3697e26a679925954e26564e7e79b98
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Multimedia","Learn_web_development/Core/Accessibility/Accessibility_troubleshooting", "Learn_web_development/Core/Accessibility")}}

Poiché l'accesso al web dai dispositivi mobili è così diffuso e piattaforme note come iOS e Android dispongono di strumenti di accessibilità completi, è importante considerare l'accessibilità dei contenuti web su queste piattaforme. Questo articolo esamina le considerazioni sull'accessibilità specifiche per i dispositivi mobili.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e le buone pratiche di accessibilità insegnate nelle lezioni precedenti del modulo.</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Familiarità con gli screen reader su iOS e Android.</li>
          <li>Familiarità con i problemi di accessibilità legati ad alcuni tipi di eventi.</li>
          <li>Tecniche specifiche per meccanismi di input dell'utente più utilizzabili sui dispositivi mobili.</li>
          <li>Conoscere i vantaggi specifici di usabilità che i browser mobili forniscono per specifici tipi di <code>&lt;input&gt;</code>, come <code>number</code> o <code>tel</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Accessibilità sui dispositivi mobili

Lo stato dell'accessibilità — e del supporto degli standard web in generale — è buono nei moderni dispositivi mobili. Sono ormai lontani i tempi in cui i dispositivi mobili utilizzavano tecnologie web completamente diverse dai browser desktop, costringendo gli sviluppatori a usare il rilevamento del browser e a fornire siti completamente separati (anche se parecchie aziende rilevano ancora l'utilizzo di dispositivi mobili e forniscono un dominio mobile separato).

Oggi i dispositivi mobili sono generalmente in grado di gestire siti web completi e le principali piattaforme includono persino screen reader integrati, per consentire alle persone con disabilità visive di usarli efficacemente. I moderni browser mobili tendono inoltre ad avere un buon supporto per [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics).

Per rendere un sito web accessibile e utilizzabile sui dispositivi mobili, è sufficiente seguire le buone pratiche generali di progettazione web e accessibilità.

Esistono alcune eccezioni che richiedono particolare attenzione sui dispositivi mobili; le principali sono:

- Meccanismi di controllo — Assicurarsi che i controlli dell'interfaccia, come i pulsanti, siano accessibili sui dispositivi mobili (ossia principalmente touchscreen), oltre che su desktop e laptop (principalmente mouse e tastiera).
- Input dell'utente — Rendere i requisiti di input dell'utente il meno gravosi possibile sui dispositivi mobili (ad esempio, nei moduli, ridurre al minimo la digitazione).
- Design responsive — Assicurarsi che i layout funzionino sui dispositivi mobili, ridurre le dimensioni di download delle immagini e considerare la disponibilità di immagini per schermi ad alta risoluzione.

## Riepilogo dei test con screen reader su Android e iOS

Le piattaforme mobili più comuni dispongono di screen reader pienamente funzionali. Questi operano in modo molto simile agli screen reader desktop, tranne per il fatto che vengono utilizzati principalmente tramite gesti tattili anziché combinazioni di tasti.

Esaminiamo i due principali: TalkBack su Android e VoiceOver su iOS.

### TalkBack su Android

Lo screen reader TalkBack è integrato nel sistema operativo Android.

Per attivarlo, verificare il modello di telefono e la versione di Android in uso, quindi cercare dove si trova il menu TalkBack. La posizione tende a differire notevolmente tra le versioni di Android e persino tra diversi modelli di telefono. Alcuni produttori di telefoni (ad esempio Samsung) non dispongono nemmeno di TalkBack nei telefoni più recenti e hanno invece scelto il proprio screen reader.

Una volta trovato il menu TalkBack, premere l'interruttore a scorrimento per attivarlo. Seguire eventuali ulteriori istruzioni visualizzate sullo schermo.

Quando TalkBack è attivo, i controlli di base del dispositivo Android saranno leggermente diversi. Ad esempio:

1. Un singolo tocco su un'app la selezionerà e il dispositivo leggerà il nome dell'app.
2. Scorrere verso sinistra e destra consentirà di spostarsi tra le app o tra pulsanti e controlli, se ci si trova in una barra di controllo. Il dispositivo leggerà ogni opzione.
3. Un doppio tocco in qualsiasi punto aprirà l'app o selezionerà l'opzione.
4. È inoltre possibile "esplorare al tocco": tenere il dito premuto sullo schermo e trascinarlo; il dispositivo leggerà le diverse app o elementi attraversati.

Per disattivare TalkBack:

1. Tornare alla schermata del menu TalkBack, utilizzando i diversi gesti attualmente abilitati.
2. Raggiungere l'interruttore a scorrimento e attivarlo per disattivarlo.

> [!NOTE]
> È possibile raggiungere la schermata iniziale in qualsiasi momento scorrendo verso l'alto e a sinistra con un movimento fluido. Se è presente più di una schermata iniziale, è possibile spostarsi tra esse scorrendo con due dita verso sinistra e destra.

Per un elenco più completo dei gesti di TalkBack, vedere [Utilizzare i gesti TalkBack](https://support.google.com/accessibility/android/answer/6151827).

#### Sbloccare il telefono

Quando TalkBack è attivo, lo sblocco del telefono è leggermente diverso.

È possibile scorrere verso l'alto con due dita dalla parte inferiore della schermata di blocco. Se è stato impostato un codice di accesso o una sequenza per sbloccare il dispositivo, verrà quindi visualizzata la schermata di inserimento pertinente.

È inoltre possibile esplorare al tocco per trovare il pulsante _Unlock_ nella parte centrale inferiore dello schermo, quindi effettuare un doppio tocco.

#### Menu globali e locali

TalkBack consente di accedere ai menu contestuali globali e locali, ovunque ci si sia spostati sul dispositivo. Il primo fornisce opzioni globali relative al dispositivo nel suo insieme, mentre il secondo fornisce opzioni relative soltanto all'app o alla schermata corrente.

Per accedere a questi menu:

1. Accedere al menu globale scorrendo rapidamente verso il basso e poi verso destra.
2. Accedere al menu locale scorrendo rapidamente verso l'alto e poi verso destra.
3. Scorrere verso sinistra e destra per passare tra le diverse opzioni.
4. Dopo aver selezionato l'opzione desiderata, effettuare un doppio tocco per sceglierla.

Per informazioni dettagliate su tutte le opzioni disponibili nei menu contestuali globali e locali, vedere [Utilizzare i menu contestuali globali e locali](https://support.google.com/accessibility/android/answer/6007066).

#### Navigare nelle pagine web

È possibile utilizzare il menu contestuale locale mentre ci si trova in un browser web per trovare opzioni che permettono di navigare nelle pagine web usando soltanto titoli, controlli dei moduli o collegamenti, oppure di navigare riga per riga e così via.

Ad esempio, con TalkBack attivo:

1. Aprire il browser web.
2. Attivare la barra dell'URL.
3. Inserire una pagina web contenente molti titoli, come la pagina iniziale di bbc.co.uk. Per inserire il testo dell'URL:
   - Selezionare la barra dell'URL scorrendo verso sinistra/destra fino a raggiungerla, quindi effettuando un doppio tocco.
   - Tenere premuto il dito sulla tastiera virtuale finché non si raggiunge il carattere desiderato, quindi sollevare il dito per digitarlo. Ripetere l'operazione per ogni carattere.
   - Una volta terminato, trovare il tasto Invio e premerlo.

4. Scorrere verso sinistra e destra per spostarsi tra i diversi elementi della pagina.
5. Scorrere verso l'alto e verso destra con un movimento fluido per accedere al menu dei contenuti locali.
6. Scorrere verso destra finché non si trova l'opzione "Headings and Landmarks".
7. Effettuare un doppio tocco per selezionarla. Ora sarà possibile scorrere verso sinistra e destra per spostarsi tra titoli e landmark ARIA.
8. Per tornare alla modalità predefinita, accedere nuovamente al menu contestuale locale scorrendo verso l'alto e verso destra, selezionare "Default", quindi effettuare un doppio tocco per attivarla.

> [!NOTE]
> Per una documentazione più completa, vedere [Iniziare a usare TalkBack su Android](https://support.google.com/accessibility/android/answer/6283677?hl=en&ref_topic=3529932).

### VoiceOver su iOS

Una versione mobile di VoiceOver è integrata nel sistema operativo iOS.

Per attivarla, aprire l'app _Impostazioni_ e selezionare _Accessibilità > VoiceOver_. Premere il cursore _VoiceOver_ per abilitarla; in questa pagina sono disponibili anche diverse altre opzioni relative a VoiceOver.

> [!NOTE]
> Alcuni dispositivi iOS meno recenti hanno il menu VoiceOver in _app Impostazioni_ > _Generali_ > _Accessibilità_ > _VoiceOver_.

Dopo aver abilitato VoiceOver, i gesti di controllo di base di iOS saranno leggermente diversi:

1. Un singolo tocco selezionerà l'elemento toccato; il dispositivo pronuncerà l'elemento selezionato.
2. È inoltre possibile navigare tra gli elementi sullo schermo scorrendo verso sinistra e destra per spostarsi tra essi, oppure facendo scivolare il dito sullo schermo per passare tra elementi diversi (quando si trova l'elemento desiderato, è possibile sollevare il dito per selezionarlo).
3. Per attivare l'elemento selezionato, ad esempio per aprire un'app selezionata, effettuare un doppio tocco in qualsiasi punto dello schermo.
4. Scorrere con tre dita per spostarsi in una pagina.
5. Toccare con due dita per eseguire un'azione pertinente al contesto, ad esempio scattare una foto nell'app fotocamera.

Per disattivarlo nuovamente, tornare a _Impostazioni > Generali > Accessibilità > VoiceOver_ usando i gesti descritti sopra e riportare il cursore _VoiceOver_ su disattivato.

#### Sbloccare il telefono

Per sbloccare il telefono, occorre premere il pulsante Home, oppure scorrere, come di consueto. Se è impostato un codice di accesso, è possibile selezionare ciascun numero scorrendo o facendo scivolare il dito, come spiegato sopra, quindi effettuare un doppio tocco per inserire ogni numero una volta trovato quello corretto.

#### Usare il Rotor

Quando VoiceOver è attivo, è disponibile una funzionalità di navigazione denominata Rotor, che consente di scegliere rapidamente tra numerose opzioni utili comuni. Per utilizzarla:

1. Ruotare due dita sullo schermo come se si stesse girando una manopola. Ogni opzione verrà letta ad alta voce man mano che si continua a ruotare. È possibile muoversi avanti e indietro per passare tra le opzioni.
2. Dopo aver trovato l'opzione desiderata:
   - Sollevare le dita per selezionarla.
   - Se si tratta di un'opzione il cui valore può essere modificato, come Volume o Velocità di lettura, è possibile scorrere verso l'alto o verso il basso per aumentare o diminuire il valore dell'elemento selezionato.

Le opzioni disponibili nel Rotor dipendono dal contesto: differiranno in base all'app o alla visualizzazione in uso; vedere sotto per un esempio.

#### Navigare nelle pagine web

Proviamo a navigare sul web con VoiceOver:

1. Aprire il browser web.
2. Attivare la barra dell'URL.
3. Inserire una pagina web contenente molti titoli, come la pagina iniziale di bbc.co.uk. Per inserire il testo dell'URL:
   - Selezionare la barra dell'URL scorrendo verso sinistra/destra fino a raggiungerla, quindi effettuando un doppio tocco.
   - Per ogni carattere, tenere il dito premuto sulla tastiera virtuale finché non si raggiunge il carattere desiderato, quindi sollevare il dito per selezionarlo. Effettuare un doppio tocco per digitarlo.
   - Una volta terminato, trovare il tasto Invio e premerlo.

4. Scorrere verso sinistra e destra per spostarsi tra gli elementi della pagina. È possibile effettuare un doppio tocco su un elemento per selezionarlo, ad esempio per seguire un collegamento.
5. Per impostazione predefinita, l'opzione Rotor selezionata sarà Velocità di lettura; è possibile scorrere verso l'alto e verso il basso per aumentare o diminuire la velocità di lettura.
6. Ora ruotare due dita sullo schermo come una manopola per mostrare il Rotor e spostarsi tra le sue opzioni. Ecco alcuni esempi delle opzioni disponibili:
   - _Velocità di lettura_: modifica la velocità di lettura.
   - _Contenitori_: sposta tra diversi contenitori semantici nella pagina.
   - _Titoli_: sposta tra i titoli nella pagina.
   - _Collegamenti_: sposta tra i collegamenti nella pagina.
   - _Controlli modulo_: sposta tra i controlli dei moduli nella pagina.
   - _Lingua_: sposta tra diverse traduzioni, se disponibili.

7. Selezionare _Titoli_. Ora sarà possibile scorrere verso l'alto e verso il basso per spostarsi tra i titoli della pagina.

> [!NOTE]
> Per un riferimento più completo sui gesti VoiceOver disponibili e altri suggerimenti sui test di accessibilità su iOS, vedere la [documentazione VoiceOver di Apple](https://developer.apple.com/documentation/accessibility/voiceover/).

## Meccanismi di controllo

Nel nostro articolo sull'accessibilità CSS e JavaScript, abbiamo esaminato il concetto di eventi specifici di un determinato tipo di meccanismo di controllo; vedere [Eventi specifici del mouse](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript#mouse-specific_events). In sintesi, questi causano problemi di accessibilità perché altri meccanismi di controllo non possono attivare la funzionalità associata.

Ad esempio, l'evento [click](/it/docs/Web/API/Element/click_event) è valido in termini di accessibilità: un event handler associato può essere invocato facendo clic sull'elemento su cui è impostato l'handler, raggiungendolo con Tab e premendo Invio, oppure toccandolo su un dispositivo touchscreen. Provare il seguente esempio di pulsante di base per vedere cosa si intende:

```html hidden live-sample___basic-button
<button>Press me!</button>
```

```css hidden live-sample___basic-button
html {
  height: 100%;
}

body {
  height: inherit;
  font-family: sans-serif;
  display: flex;
  align-items: center;
}

h1 {
  text-align: center;
}

button {
  width: 70%;
  margin: 0 auto;
  display: block;
  font-size: 150%;
  line-height: 1.5;
}
```

```js hidden live-sample___basic-button
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  alert("Ouch, that hurt!");
});
```

{{embedlivesample("basic-button", "100%", "100")}}

Gli eventi specifici del mouse, tuttavia, come [mousedown](/it/docs/Web/API/Element/mousedown_event) e [mouseup](/it/docs/Web/API/Element/mouseup_event), creano problemi: i rispettivi event handler non possono essere invocati utilizzando controlli diversi dal mouse.

L'esempio successivo usa codice simile al seguente per consentire di trascinare una casella sullo schermo con il mouse:

```js
div.addEventListener("mousedown", () => {
  initialBoxX = div.offsetLeft;
  initialBoxY = div.offsetTop;
  movePanel();
});

document.addEventListener("mouseup", stopMove);
```

```html hidden live-sample___mouse-drag live-sample___multi-drag
<div></div>
```

```css hidden live-sample___mouse-drag live-sample___multi-drag
html {
  font-family: sans-serif;
  overflow: hidden;
}

body {
  background: #ffe;
  margin: 0;
}

div {
  background-color: #1fe200;
  background-image: linear-gradient(
    to bottom right,
    rgb(0 0 0 / 0),
    rgb(0 0 0 / 0.4)
  );
  width: 200px;
  height: 150px;
  border: 1px solid green;
  position: absolute;
}
```

```js hidden live-sample___mouse-drag
document.body.width = window.innerWidth;
document.body.height = window.innerHeight;

let mouseX, mouseY;

document.addEventListener("mousemove", (e) => {
  mouseX = e.clientX;
  mouseY = e.clientY;
});

const div = document.querySelector("div");

let initialMouseX = null;

let initialMouseY = null;

var initialBoxX, initialBoxY, rAF;

div.addEventListener("mousedown", () => {
  initialBoxX = div.offsetLeft;
  initialBoxY = div.offsetTop;
  movePanel();
});

document.addEventListener("mouseup", stopMove);

function movePanel() {
  if (initialMouseX === null) {
    initialMouseX = mouseX;
    initialMouseY = mouseY;
  } else {
    let mouseMoveX = mouseX - initialMouseX;
    let mouseMoveY = mouseY - initialMouseY;

    let offsetX = initialBoxX + mouseMoveX;
    let offsetY = initialBoxY + mouseMoveY;
    console.log(offsetX + " " + offsetY);

    div.style.left = offsetX + "px";
    div.style.top = offsetY + "px";
  }

  rAF = requestAnimationFrame(movePanel);
}

function stopMove() {
  cancelAnimationFrame(rAF);

  console.log("mousemove stopped");

  initialMouseX = null;
  initialMouseY = null;
}
```

{{embedlivesample("mouse-drag", "100%", "400")}}

Tuttavia, se si prova a trascinarla con un dito su un dispositivo touchscreen, non funzionerà. Per abilitare altre forme di controllo, occorre utilizzare eventi diversi ma equivalenti: ad esempio, gli eventi touch funzionano sui dispositivi touchscreen:

```js
div.addEventListener("touchstart", (e) => {
  initialBoxX = div.offsetLeft;
  initialBoxY = div.offsetTop;
  positionHandler(e);
  movePanel();
});

document.addEventListener("touchend", stopMove);
```

```js hidden live-sample___multi-drag
document.body.width = window.innerWidth;
document.body.height = window.innerHeight;

let posX, posY;

document.addEventListener("mousemove", positionHandler);
document.addEventListener("touchmove", positionHandler);

function positionHandler(e) {
  if (e.clientX && e.clientY) {
    posX = e.clientX;
    posY = e.clientY;
  } else if (e.targetTouches) {
    posX = e.targetTouches[0].clientX;
    posY = e.targetTouches[0].clientY;
    e.preventDefault();
  }
}

const div = document.querySelector("div");

let initialPosX = null;

let initialPosY = null;

let rAF;

div.addEventListener("mousedown", () => {
  initialBoxX = div.offsetLeft;
  initialBoxY = div.offsetTop;
  movePanel();
});

div.addEventListener("touchstart", (e) => {
  initialBoxX = div.offsetLeft;
  initialBoxY = div.offsetTop;
  positionHandler(e);
  movePanel();
});

document.addEventListener("mouseup", stopMove);
document.addEventListener("touchend", stopMove);

function movePanel() {
  if (initialPosX === null) {
    initialPosX = posX;
    initialPosY = posY;
  } else {
    let posMoveX = posX - initialPosX;
    let posMoveY = posY - initialPosY;

    let offsetX = initialBoxX + posMoveX;
    let offsetY = initialBoxY + posMoveY;

    div.style.left = offsetX + "px";
    div.style.top = offsetY + "px";
  }

  rAF = requestAnimationFrame(movePanel);
}

function stopMove() {
  cancelAnimationFrame(rAF);

  initialPosX = null;
  initialPosY = null;
}
```

La versione aggiornata funzionerà sia con il trascinamento tramite mouse sia con quello tramite tocco:

{{embedlivesample("multi-drag", "100%", "400")}}

> [!NOTE]
> È inoltre possibile vedere esempi pienamente funzionali che mostrano come implementare diversi meccanismi di controllo in [Implementazione dei meccanismi di controllo nei giochi](/it/docs/Games/Techniques/Control_mechanisms).

## Design responsive

Il [design responsive](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design) è la pratica di fare in modo che i layout e le altre funzionalità delle app cambino dinamicamente in base a fattori quali dimensioni e risoluzione dello schermo, affinché siano utilizzabili e accessibili agli utenti di diversi tipi di dispositivo.

In particolare, i problemi più comuni da affrontare sui dispositivi mobili sono:

- Adeguatezza dei layout per i dispositivi mobili. Un layout a più colonne, ad esempio, non funzionerà altrettanto bene su uno schermo stretto e potrebbe essere necessario aumentare la dimensione del testo affinché sia leggibile. Questi problemi possono essere risolti creando un layout responsive utilizzando tecnologie quali [media query](/it/docs/Web/CSS/Guides/Media_queries), [viewport](/it/docs/Web/HTML/Reference/Elements/meta/name/viewport) e [flexbox](/it/docs/Learn_web_development/Core/CSS_layout/Flexbox).
- Riduzione delle dimensioni delle immagini scaricate. In generale, i dispositivi con schermi piccoli non necessitano di immagini grandi quanto quelle delle controparti desktop ed è più probabile che utilizzino connessioni di rete lente. È quindi opportuno fornire immagini più piccole ai dispositivi con schermi stretti, quando appropriato. Questo può essere gestito mediante [tecniche per immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images).
- Considerazione delle alte risoluzioni. Molti dispositivi mobili hanno schermi ad alta risoluzione e necessitano quindi di immagini a risoluzione più elevata affinché la visualizzazione rimanga nitida e definita. Anche in questo caso, è possibile fornire immagini appropriate usando tecniche per immagini responsive. Inoltre, molti requisiti relativi alle immagini possono essere soddisfatti tramite il formato di immagini vettoriali SVG, oggi ben supportato dai browser. SVG ha dimensioni ridotte e rimane nitido indipendentemente dalla dimensione in cui viene visualizzato; per maggiori dettagli, vedere [Includere grafica vettoriale in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Including_vector_graphics_in_HTML).

> [!NOTE]
> Non verrà fornita qui una discussione completa sulle tecniche di design responsive, poiché sono trattate in altre sezioni di MDN; vedere i collegamenti sopra.

### Considerazioni specifiche per dispositivi mobili

Esistono altri importanti aspetti da considerare per rendere i siti più accessibili sui dispositivi mobili. Ne sono elencati alcuni qui, ma ne verranno aggiunti altri quando saranno individuati.

#### Non disabilitare lo zoom

Usando [viewport](/it/docs/Web/HTML/Reference/Elements/meta/name/viewport), è possibile disabilitare lo zoom. Assicurarsi sempre che il ridimensionamento sia abilitato e impostare la larghezza sulla larghezza del dispositivo nell'elemento {{htmlelement("head")}}:

```html
<meta name="viewport" content="width=device-width; user-scalable=yes" />
```

Non impostare mai `user-scalable=no` se possibile: molte persone dipendono dallo zoom per poter visualizzare il contenuto del sito web, quindi rimuovere questa funzionalità è davvero una cattiva idea. Esistono situazioni specifiche in cui lo zoom potrebbe compromettere l'interfaccia utente; in questi casi, se sembra necessario disabilitare lo zoom, occorre fornire un'altra soluzione equivalente, come un controllo per aumentare la dimensione del testo in un modo che non comprometta l'interfaccia utente.

#### Mantenere accessibili i menu

Poiché lo schermo è molto più stretto sui dispositivi mobili, è molto comune utilizzare media query e altre tecnologie per ridurre il menu di navigazione a una piccola icona nella parte superiore dello schermo, che può essere premuta per mostrare il menu soltanto quando necessario, quando il sito viene visualizzato su dispositivi mobili. Questo è comunemente rappresentato da un'icona con "tre linee orizzontali" e il pattern di progettazione è quindi noto come "menu hamburger".

Quando si implementa un tale menu, occorre assicurarsi che il controllo per mostrarlo sia accessibile tramite meccanismi di controllo appropriati, normalmente il tocco sui dispositivi mobili, come discusso nella sezione [Meccanismi di controllo](#meccanismi_di_controllo) sopra, e che il resto della pagina venga spostato o nascosto in qualche modo durante l'accesso al menu, per evitare confusione nella navigazione.

Fare clic qui per un [buon esempio di menu hamburger](https://fritz-weisshart.de/meg_men/).

## Input dell'utente

Sui dispositivi mobili, l'inserimento dei dati tende a essere più fastidioso per gli utenti rispetto all'esperienza equivalente sui computer desktop. È più comodo digitare testo negli input dei moduli usando una tastiera desktop o laptop rispetto a una tastiera virtuale touchscreen o a una piccola tastiera fisica mobile.

Per questa ragione, è opportuno cercare di ridurre al minimo la quantità di digitazione necessaria. Ad esempio, invece di chiedere agli utenti di compilare ogni volta il proprio titolo professionale usando un normale input di testo, si potrebbe offrire un menu {{htmlelement("select")}} contenente le opzioni più comuni, che favorisce anche la coerenza nell'inserimento dei dati, e offrire un'opzione "Altro" che mostri un campo di testo in cui digitare eventuali valori non inclusi. È possibile vedere un semplice esempio di questa idea in azione nell'esempio seguente:

```html hidden live-sample___select-text-combo
<form>
  <div>
    <label for="job">Job type:</label>
    <select id="job" name="job">
      <option value="">-- select job --</option>
      <option value="butcher">Butcher</option>
      <option value="baker">Baker</option>
      <option value="candle">Candlestick maker</option>
      <option value="other">Other</option>
    </select>
  </div>
  <div>
    <label for="other-job">Other job:</label>
    <input type="text" name="other-job" id="other-job" />
  </div>
</form>
```

```css hidden live-sample___select-text-combo
html {
  font-family: sans-serif;
}

div {
  margin-bottom: 10px;
}
```

```js hidden live-sample___select-text-combo
const select = document.querySelector("select");
const other = document.querySelector("input");

other.parentElement.style.display = "none";

select.onchange = function () {
  if (select.value === "other") {
    other.parentElement.style.display = "block";
  } else {
    other.parentElement.style.display = "none";
  }
};
```

{{embedlivesample("select-text-combo", "100%", "80")}}

Vale inoltre la pena considerare l'uso dei tipi di input dei moduli HTML sulle piattaforme mobili, poiché vengono gestiti bene sia da Android sia da iOS.

Ad esempio:

- I tipi `number`, `tel` e `email` mostrano tastiere virtuali adatte per l'inserimento di numeri e numeri telefonici.
- I tipi `time` e `date` mostrano selettori appropriati per scegliere orari e date.

Per provarli, vedere gli esempi interattivi disponibili in [I tipi di input HTML5](/it/docs/Learn_web_development/Extensions/Forms/HTML5_input_types).

Se si desidera fornire una soluzione diversa per i desktop, è sempre possibile fornire markup differente ai dispositivi mobili usando il rilevamento delle funzionalità. Per ulteriori informazioni, consultare il nostro [articolo sul rilevamento delle funzionalità](/it/docs/Learn_web_development/Extensions/Testing/Feature_detection).

## Riepilogo

In questo articolo sono stati forniti alcuni dettagli sui comuni problemi di accessibilità specifici dei dispositivi mobili e su come superarli. È stato inoltre illustrato l'uso degli screen reader più comuni per agevolare i test di accessibilità.

## Vedi anche

- [Linee guida per lo sviluppo web mobile](https://www.smashingmagazine.com/2012/07/guidelines-for-mobile-web-development/) — Un elenco di articoli in _Smashing Magazine_ che tratta diverse tecniche per il web design mobile.
- [Far funzionare il sito sui dispositivi touch](https://www.creativebloq.com/javascript/make-your-site-work-touch-devices-51411644) — Articolo utile sull'uso degli eventi touch per far funzionare le interazioni sui dispositivi mobili.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Multimedia","Learn_web_development/Core/Accessibility/Accessibility_troubleshooting", "Learn_web_development/Core/Accessibility")}}
