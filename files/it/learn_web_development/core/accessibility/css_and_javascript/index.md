---
title: "CSS e JavaScript: migliori pratiche per l'accessibilità"
short-title: CSS e JS accessibili
slug: Learn_web_development/Core/Accessibility/CSS_and_JavaScript
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility/WAI-ARIA_basics", "Learn_web_development/Core/Accessibility")}}

CSS e JavaScript, se utilizzati correttamente, possono consentire esperienze web accessibili, o possono danneggiare significativamente l'accessibilità se usati in modo improprio. Questo articolo descrive alcune delle migliori pratiche CSS e JavaScript che dovrebbero essere considerate per garantire che anche i contenuti complessi siano il più accessibili possibile.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, una <a href="/it/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">comprensione di base dei concetti di accessibilità</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Dimensionamento e layout del testo accessibili.</li>
          <li>Contrasto del colore.</li>
          <li>L'importanza degli stili <code>:focus</code> e <code>:hover</code>.</li>
          <li>Uso sensato dell'animazione — usa l'animazione in modo sottile e fornisci controlli per disattivarla.</li>
          <li>Migliori pratiche per nascondere i contenuti affinché non diventino inaccessibili.</li>
          <li>Il fatto che esista una cosa come troppi JavaScript, e il valore del JavaScript discreto.</li>
          <li>Utilizzare gli eventi in modo sensato per non bloccare tipi di controlli specifici.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## CSS e JavaScript sono accessibili?

CSS e JavaScript non hanno la stessa importanza immediata per l'accessibilità come l'HTML, ma possono comunque aiutare o danneggiare l'accessibilità, a seconda di come vengono utilizzati. Detto in un altro modo, è importante considerare alcuni consigli sulle migliori pratiche per assicurarsi che l'uso di CSS e JavaScript non rovini l'accessibilità dei documenti.

## CSS

Iniziamo dando un'occhiata al CSS.

### Semantica corretta e aspettativa dell'utente

È possibile utilizzare il CSS per fare in modo che qualsiasi elemento HTML abbia l'aspetto di _qualsiasi cosa_, ma questo non significa che dovresti farlo. Come abbiamo spesso menzionato nel nostro articolo [HTML: Una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML), dovresti utilizzare l'elemento semantico appropriato per il lavoro, ogni volta che è possibile. Se non lo fai, può causare confusione e problemi di usabilità per tutti, ma in particolare per gli utenti con disabilità. L'uso corretto della semantica ha molto a che fare con le aspettative degli utenti: gli elementi appaiono e si comportano in determinati modi, in base alla loro funzionalità, e queste convenzioni comuni sono attese dagli utenti.

Ad esempio, un utente di un lettore di schermo non può navigare in una pagina tramite elementi di intestazione se lo sviluppatore non ha utilizzato correttamente gli elementi di intestazione per contrassegnare il contenuto. Allo stesso modo, un'intestazione perde il suo scopo visivo se la stilizzi in modo che non sembri più un'intestazione.

In sintesi, puoi aggiornare lo stile di una caratteristica della pagina per adattarla al tuo design, ma non cambiarla così tanto da non apparire o comportarsi come previsto. Le sezioni seguenti riassumono le principali caratteristiche HTML da considerare.

#### Struttura del contenuto testuale "standard"

Intestazioni, paragrafi, elenchi: il nucleo del contenuto testuale della tua pagina:

```html
<h1>Heading</h1>

<p>Paragraph</p>

<ul>
  <li>My list</li>
  <li>has two items.</li>
</ul>
```

Un tipico CSS potrebbe apparire così:

```css
h1 {
  font-size: 5rem;
}

p,
li {
  line-height: 1.5;
  font-size: 1.6rem;
}
```

Dovresti:

- Selezionare dimensioni di font sensibili, altezze di riga, spaziatura delle lettere, ecc. per rendere il tuo testo logico, leggibile e confortevole da leggere.
- Assicurarti che le tue intestazioni si distinguano dal testo del corpo, tipicamente grandi e in grassetto come lo stile predefinito. I tuoi elenchi dovrebbero apparire come elenchi.
- Il colore del testo dovrebbe contrastare bene con il colore di sfondo.

Vedi [Intestazioni e paragrafi in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs) e [Stilizzazione del testo in CSS](/it/docs/Learn_web_development/Core/Text_styling) per ulteriori informazioni.

#### Testo evidenziato

Markup in linea che conferisce enfasi specifica al testo su cui si applica:

```html
<p>The water is <em>very hot</em>.</p>

<p>
  Water droplets collecting on surfaces is called <strong>condensation</strong>.
</p>
```

Potresti voler aggiungere una colorazione semplice al tuo testo enfatizzato:

```css
strong,
em {
  color: #a60000;
}
```

Tuttavia, raramente avrai bisogno di stilizzare gli elementi di enfasi in modo significativo. Le convenzioni standard di testo in grassetto e corsivo sono molto riconoscibili, e cambiare lo stile può causare confusione. Per ulteriori informazioni sull'enfasi, vedi [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance).

#### Abbreviazioni

Un elemento che consente che un'abbreviazione, un acronimo o un'inizializzazione venga associata alla sua espansione:

```html
<p>
  Web content is marked up using Hypertext Markup Language, or
  <abbr>HTML</abbr>.
</p>
```

Ancora una volta, potresti volerlo stilizzare in modo semplice:

```css
abbr {
  color: #a60000;
}
```

La convenzione di stile riconosciuta per le abbreviazioni è una sottolineatura punteggiata, ed è sconsigliato deviare significativamente da questa. Per ulteriori informazioni sulle abbreviazioni, vedi [Abbreviazioni](/it/docs/Learn_web_development/Core/Structuring_content/Advanced_text_features#abbreviations).

#### Link

Iperlink — il modo per raggiungere nuovi posti sul web:

```html
<p>Visit the <a href="https://www.mozilla.org">Mozilla homepage</a>.</p>
```

È mostrato di seguito uno stile di link molto semplice:

```css
a {
  color: #ff0000;
}

a:hover,
a:visited,
a:focus {
  color: #a60000;
  text-decoration: none;
}

a:active {
  color: #000000;
  background-color: #a60000;
}
```

Le convenzioni standard per i link sono sottolineati e di un colore diverso (predefinito: blu) nel loro stato standard, un'altra variazione di colore quando il link è stato precedentemente visitato (predefinito: viola) ed un ulteriore colore quando il link è attivato (predefinito: rosso). Inoltre, il puntatore del mouse cambia in un'icona a puntatore quando i link sono al passaggio del mouse, e il link riceve un'evidenziazione quando è messo a fuoco (per es., tramite tab) o attivato. Le immagini seguenti mostrano l'evidenziazione sia in Firefox (una linea tratteggiata) che in Chrome (una linea blu):

![Schermata di un elenco di link nel browser Firefox. L'elenco contiene 4 elementi. Il secondo elemento dell'elenco è evidenziato utilizzando una linea tratteggiata blu quando è messo a fuoco tramite tabulazione.](focus-highlight-firefox.png)

![Schermata di un elenco di link nel browser Chrome. L'elenco contiene 4 elementi. Il terzo elemento dell'elenco è evidenziato utilizzando una linea blu quando è messo a fuoco tramite tabulazione.](focus-highlight-chrome.png)

Puoi essere creativo con gli stili dei link, fintanto che continui a dare feedback agli utenti quando interagiscono con i link. Qualcosa dovrebbe sicuramente succedere quando cambiano gli stati, e non dovresti eliminare il cursore a puntatore o il contorno — entrambi sono aiuti importanti per l'accessibilità per coloro che utilizzano controlli da tastiera.

#### Elementi del modulo

Elementi che consentono agli utenti di inserire dati nei siti web:

```html
<div>
  <label for="name">Enter your name</label>
  <input type="text" id="name" name="name" />
</div>
```

Puoi vedere un buon esempio di CSS nei moduli nel nostro esempio [form-css.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-css.html) (vedi anche [la demo dal vivo](https://mdn.github.io/learning-area/accessibility/css/form-css.html)).

La maggior parte del CSS che scriverai per i moduli sarà per dimensionare gli elementi, allineare le etichette e gli input, e renderli puliti e ordinati.

Non dovresti però deviare troppo dal feedback visivo atteso che gli elementi del modulo ricevono quando sono a fuoco, che è fondamentalmente lo stesso dei link (vedi sopra). Potresti stilizzare gli stati di fuoco/sospensione dei moduli per rendere questo comportamento più consistente tra i browser o adattarsi meglio al design della tua pagina, ma non eliminarli del tutto: ancora una volta, le persone si affidano a questi indizi per sapere cosa sta succedendo.

#### Tabelle

Tabelle per la presentazione di dati tabulari.

Puoi vedere un buon esempio semplice di HTML e CSS per le tabelle nel nostro esempio [table-css.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/table-css.html) (vedi anche [la demo dal vivo](https://mdn.github.io/learning-area/accessibility/css/table-css.html)).

Il CSS delle tabelle serve generalmente a far sì che la tabella si adatti meglio al tuo design e appaia meno brutta. È una buona idea assicurarsi che le intestazioni delle tabelle si distinguano (normalmente usando il grassetto) e utilizzare la striatura zebrata per rendere più facile analizzare le diverse righe.

### Colore e contrasto del colore

Quando scegli uno schema di colori per il tuo sito web, assicurati che il colore del testo (in primo piano) contrasti bene con il colore di sfondo. Il tuo design potrebbe sembrare cool, ma non serve a nulla se le persone con problemi di vista come il daltonismo non riescono a leggere il tuo contenuto.

Esiste un modo semplice per verificare se il contrasto è sufficientemente grande da non causare problemi. Ci sono diversi strumenti di controllo del contrasto online in cui puoi inserire i tuoi colori di primo piano e di sfondo, per controllarli. Ad esempio, il [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM è semplice da usare e fornisce una spiegazione di ciò che è necessario per conformarsi ai criteri WCAG sul contrasto del colore.

> [!NOTE]
> Un alto rapporto di contrasto consentirà anche a chiunque utilizzi uno smartphone o un tablet con uno schermo lucido di leggere meglio le pagine in un ambiente luminoso, come alla luce del sole.

Un altro consiglio è di non fare affidamento sul solo colore per i cartelli/indicazioni, poiché questo non va bene per chi non è in grado di vedere il colore. Invece di contrassegnare i campi obbligatori del modulo in rosso, ad esempio, contrassegnali con un asterisco e in rosso.

### Nascondere contenuti

Ci sono molte istanze in cui un design visivo richiederà che non tutti i contenuti vengano mostrati contemporaneamente. Ad esempio, nel nostro [esempio di scatola informazioni a schede](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/tabbed-info-box.html) (vedi [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html)) abbiamo tre pannelli di informazioni, ma li stiamo [posizionando](/it/docs/Learn_web_development/Core/CSS_layout/Positioning) uno sopra l'altro e fornendo schede che possono essere cliccate per mostrare ciascuno (è anche accessibile da tastiera - è possibile utilizzare alternativamente Tab e Invio/Ritorno per selezionarli).

![Interfaccia a tre schede con la scheda 1 selezionata e solo i suoi contenuti vengono visualizzati. I contenuti delle altre schede sono nascosti. Se una scheda è selezionata, il colore del testo cambia da nero a bianco e il colore di sfondo cambia da rosso arancio a marrone scuro.](tabbed-info-box.png)

Gli utenti dei lettori di schermo non si preoccupano di tutto questo — sono soddisfatti del contenuto finché l'ordine delle sorgenti ha senso e possono accedere a tutto. Il posizionamento assoluto (come usato in questo esempio) è generalmente considerato uno dei migliori meccanismi per nascondere contenuti a scopo visivo perché non impedisce ai lettori di schermo di accedervi.

D'altra parte, non dovresti usare {{cssxref("visibility", "visibility: hidden")}} o {{cssxref("display", "display: none")}}, perché nascondono effettivamente i contenuti dai lettori di schermo. A meno che, naturalmente, non ci sia un buon motivo per cui desideri che questo contenuto sia nascosto ai lettori di schermo.

> **Nota:** [Contenuti invisibili solo per gli utenti di lettori schermo](https://webaim.org/techniques/css/invisiblecontent/) fornisce molti altri dettagli utili su questo argomento.

### Accettare che gli utenti possano sovrascrivere gli stili

È possibile che gli utenti possano sovrascrivere i tuoi stili con i loro stili personalizzati, ad esempio:

- Vedi la guida di Sarah Maddox su [Come utilizzare un foglio di stile personalizzato (CSS) con Firefox](https://www.itsupportguides.com/knowledge-base/computer-accessibility/how-to-use-a-custom-style-sheet-css-with-firefox/) per una guida utile su come fare manualmente in Firefox.
- Probabilmente è più semplice farlo utilizzando un'estensione. Ad esempio, l'estensione Stylus è disponibile per [Firefox](https://addons.mozilla.org/en-US/firefox/addon/styl-us/), con Stylish che è un equivalente per [Chrome](https://chromewebstore.google.com/detail/stylish-custom-themes-for/fjnbnpbmkenffdnngjfgmeleoegfcffe).

Gli utenti potrebbero farlo per una varietà di motivi. Un utente con problemi di vista potrebbe voler ingrandire il testo su tutti i siti web che visita, o un utente con una grave carenza di colore potrebbe voler impostare tutti i siti web a colori ad alto contrasto facili da vedere per loro. Qualunque sia la necessità, dovresti essere a tuo agio con questo e rendere i tuoi design abbastanza flessibili da consentire che tali cambiamenti funzionino nel tuo design. Ad esempio, potresti voler assicurarti che l'area di contenuto principale possa gestire un testo più grande (forse inizierà a scorrere per consentire che tutto venga visto) e che non nasconderà il testo o si romperà completamente.

## JavaScript

Anche JavaScript può rompere l'accessibilità, a seconda di come viene utilizzato.

Il JavaScript moderno è un linguaggio potente e possiamo fare così tanto con esso oggigiorno, da semplici contenuti e aggiornamenti UI a giochi 2D e 3D completamente realizzati. Non esiste una regola che dice che tutti i contenuti devono essere al 100% accessibili a tutte le persone, devi solo fare ciò che puoi e rendere le tue applicazioni il più accessibili possibile.

I contenuti e le funzionalità semplici sono probabilmente facili da rendere accessibili — ad esempio testo, immagini, tabelle, moduli e pulsanti push che attivano funzioni. Come abbiamo visto nel nostro articolo [HTML: Una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML), le considerazioni principali sono:

- Buona semantica: utilizzo del giusto elemento per il lavoro giusto. Ad esempio, assicurarti di utilizzare intestazioni e paragrafi, e gli elementi {{htmlelement("button")}} e {{htmlelement("a")}}
- Assicurarti che il contenuto sia disponibile come testo, sia direttamente come contenuto testuale, buone etichette di testo per gli elementi del modulo, o [alternative testuali](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives), ad es., testo alternativo per le immagini.

Abbiamo anche guardato un esempio di come usare JavaScript per integrare la funzionalità dove manca — vedi [Reintegrazione dell'accessibilità da tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in). Questo non è l'ideale — davvero dovresti semplicemente usare l'elemento giusto per il lavoro giusto — ma mostra che è possibile in situazioni in cui per qualche motivo non puoi controllare il markup utilizzato. Un altro modo per migliorare l'accessibilità per widget JavaScript non semantici è utilizzare WAI-ARIA per fornire semantica extra per gli utenti di lettori di schermo. Il prossimo articolo coprirà anche questo in dettaglio.

La funzionalità complessa come i giochi 3D non è così facile da rendere accessibile — un gioco 3D complesso creato utilizzando [WebGL](/it/docs/Web/API/WebGL_API) verrà renderizzato su un elemento {{htmlelement("canvas")}}, che non ha attualmente la capacità di fornire alternative testuali o altre informazioni per utenti gravemente ipovedenti di cui beneficiare. È discutibile che un tale gioco non abbia davvero questo gruppo di persone come parte del suo pubblico principale, e sarebbe irragionevole aspettarsi di renderlo al 100% accessibile ai non vedenti. Tuttavia, potresti implementare [controlli da tastiera](/it/docs/Games/Techniques/Control_mechanisms/Desktop_with_mouse_and_keyboard) in modo che sia utilizzabile da utenti non mouse e rendere lo schema di colori abbastanza contrastante da essere utilizzabile da chi ha carenze di colore.

### Il problema con troppo JavaScript

Il problema spesso si verifica quando le persone si affidano troppo a JavaScript. A volte vedrai un sito web dove tutto è stato fatto con JavaScript — l'HTML è stato generato da JavaScript, il CSS è stato generato da JavaScript, ecc. Questo ha tutti i tipi di problemi di accessibilità e altri associati, quindi non è consigliato.

Oltre a usare l'elemento giusto per il lavoro giusto, dovresti anche assicurarti di utilizzare la tecnologia giusta per il lavoro giusto! Pensa attentamente se hai bisogno di quella scatola informativa 3D alimentata da JavaScript, o se del semplice testo sarebbe sufficiente. Pensa attentamente se hai bisogno di un widget di modulo complesso non standard, o se un input di testo sarebbe sufficiente. E non generare tutto il tuo contenuto HTML usando JavaScript se è possibile evitarlo.

### Mantenere discreto

Dovresti tenere a mente il JavaScript **discreto** quando crei il tuo contenuto. L'idea del JavaScript discreto è che dovrebbe essere usato ovunque possibile per migliorare la funzionalità, non costruirla completamente — le funzioni di base dovrebbero idealmente funzionare anche senza JavaScript, anche se si apprezza che ciò non sia sempre possibile. Ma ancora una volta, gran parte di ciò è utilizzare la funzionalità incorporata del browser ove possibile.

Usi di buon esempio di JavaScript discreto includono:

- Fornire la validazione dei moduli lato client, che avverte gli utenti dei problemi con le loro voci del modulo in modo rapido, senza dover aspettare che il server controlli i dati. Se non è disponibile, il modulo funzionerà ancora, ma la validazione potrebbe essere più lenta.
- Fornire controlli personalizzati per `<video>` HTML che sono accessibili a utenti solo con tastiera, insieme a un collegamento diretto al video che può essere utilizzato per accedervi se JavaScript non è disponibile (i controlli del browser `<video>` predefiniti non sono accessibili da tastiera nella maggior parte dei browser).

Ad esempio, abbiamo scritto un rapido e sporco esempio di validazione del modulo lato client — vedi [form-validation.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) (vedi anche [demo dal vivo](https://mdn.github.io/learning-area/accessibility/css/form-validation.html)). Qui vedrai un semplice modulo; quando provi a inviare il modulo lasciando uno o entrambi i campi vuoti, l'invio fallisce e appare una finestra di messaggio di errore per dirti cosa c'è di sbagliato.

Questo tipo di validazione del modulo è discreta — puoi comunque usare il modulo con assolutamente nessun problema anche senza che JavaScript sia disponibile, e qualsiasi implementazione del modulo sensata avrà anche la validazione lato server attiva, perché è troppo facile per gli utenti malintenzionati bypassare la validazione lato client (ad esempio, disattivando JavaScript nel browser). La validazione lato client è comunque davvero utile per segnalare errori — gli utenti possono conoscere immediatamente gli errori che commettono, piuttosto che dover aspettare un round trip al server e un ricaricamento della pagina. Questo è decisamente un vantaggio in termini di usabilità.

> [!NOTE]
> La validazione lato server non è stata implementata in questa semplice demo.

Abbiamo reso questa validazione dei moduli piuttosto accessibile. Abbiamo utilizzato gli elementi {{htmlelement("label")}} per assicurarci che le etichette dei moduli siano inconfondibilmente collegate ai loro input, in modo che i lettori di schermo possano leggerle contemporaneamente:

```html
<label for="name">Enter your name:</label>
<input type="text" name="name" id="name" />
```

Eseguiamo la validazione solo quando il modulo è inviato — questo è per evitare di aggiornare troppo spesso l'interfaccia utente e potenzialmente confondere gli utenti di lettore di schermo (e possibilmente anche altri utenti):

```js
form.onsubmit = validate;

function validate(e) {
  errorList.textContent = "";
  for (let i = 0; i < formItems.length; i++) {
    const testItem = formItems[i];
    if (testItem.input.value === "") {
      errorField.style.left = "360px";
      createLink(testItem);
    }
  }

  if (errorList.hasChildNodes()) {
    e.preventDefault();
  }
}
```

> [!NOTE]
> In questo esempio, stiamo nascondendo e mostrando la finestra di messaggio di errore utilizzando il posizionamento assoluto piuttosto che un altro metodo come visibilità o display, perché ciò non interferisce con il fatto che il lettore di schermo sia in grado di leggere il contenuto da esso.

La vera validazione dei moduli sarebbe molto più complessa di questa — vorresti controllare che il nome inserito sembri effettivamente un nome, l'età inserita sia effettivamente un numero e sia realistica (ad es., non negativa e inferiore a 4 cifre). Qui abbiamo semplicemente implementato un controllo semplice che un valore sia stato compilato in ciascun campo di input (`if (testItem.input.value === '')`).

Quando la validazione è stata eseguita, se i test passano allora il modulo è inviato. Se ci sono errori (`if (errorList.hasChildNodes())`) allora fermiamo l'invio del modulo (usando [`preventDefault()`](/it/docs/Web/API/Event/preventDefault)), e mostriamo eventuali messaggi di errore che sono stati creati (vedi sotto). Questo meccanismo significa che gli errori vengono mostrati solo se ci sono errori, il che è meglio per l'usabilità.

Per ogni input che non ha un valore inserito quando il modulo è inviato, creiamo un elemento di lista con un link e lo inseriamo nell'elenco `errorList`.

```js
function createLink(testItem) {
  const listItem = document.createElement("li");
  const anchor = document.createElement("a");

  const name = testItem.input.name;
  anchor.textContent = `${name} field is empty: fill in your ${name}.`;
  anchor.href = `#${name}`;
  listItem.appendChild(anchor);
  errorList.appendChild(listItem);
}
```

Ogni link ha un duplice scopo — ti informa su quale sia l'errore e puoi cliccarlo/attivarlo per saltare direttamente all'elemento di input in questione e correggere la tua voce.

Inoltre, il campo `errorField` è posizionato all'inizio dell'ordine delle sorgenti (sebbene sia posizionato diversamente nell'interfaccia utente utilizzando CSS), il che significa che gli utenti possono scoprire esattamente cosa c'è di sbagliato con le loro invii di moduli e arrivare agli elementi di input in questione tornando all'inizio della pagina.

Infine, abbiamo utilizzato alcuni attributi WAI-ARIA nella nostra demo per aiutare a risolvere i problemi di accessibilità causati da aree di contenuto che si aggiornano costantemente senza un ricaricamento della pagina (i lettori di schermo non ne parlano spontaneamente né allertano gli utenti):

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

Spiegheremo questi attributi nel nostro prossimo articolo, che copre [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics) in molto più dettaglio.

> [!NOTE]
> Alcuni di voi probabilmente staranno pensando al fatto che gli HTML moduli hanno meccanismi di validazione integrati come gli attributi `required`, `min`/`minlength` e `max`/`maxlength` (vedi la referenza dell'elemento {{htmlelement("input")}} per ulteriori informazioni). Non abbiamo usato questi nel demo perché il supporto cross-browser è discontinuo (ad esempio IE10 e versioni superiori solo).

> [!NOTE]
> La [Validazione dei moduli e recupero degli errori utilizzabili e accessibili](https://webaim.org/techniques/formvalidation/) di WebAIM fornisce ulteriori informazioni utili sulla validazione dei moduli accessibili.

### Altre preoccupazioni per l'accessibilità di JavaScript

Ci sono altre cose da considerare quando si implementa JavaScript e si pensa all'accessibilità. Aggiungeremo di più man mano che le troveremo.

#### Eventi specifici del mouse

Come saprete, la maggior parte delle interazioni utente sono implementate in JavaScript lato client utilizzando gestori di eventi, che ci consentono di eseguire funzioni in risposta a determinati eventi che si verificano. Alcuni eventi possono avere problemi di accessibilità. L'esempio principale che incontrerai sono gli eventi specifici del mouse come [mouseover](/it/docs/Web/API/Element/mouseover_event), [mouseout](/it/docs/Web/API/Element/mouseout_event), [dblclick](/it/docs/Web/API/Element/dblclick_event), ecc. La funzionalità che viene eseguita in risposta a questi eventi non sarà accessibile utilizzando altri meccanismi, come i controlli da tastiera.

Per mitigare tali problemi, dovresti raddoppiare questi eventi con eventi simili che possono essere attivati in altri modi (i cosiddetti handler di eventi indipendenti dal dispositivo) — [focus](/it/docs/Web/API/Element/focus_event) e [blur](/it/docs/Web/API/Element/blur_event) fornirebbero accessibilità per gli utenti di tastiera.

Diamo un'occhiata a un esempio che evidenzia quando ciò potrebbe essere utile. Forse vogliamo fornire un'immagine in miniatura che mostri una versione più grande dell'immagine quando è al passaggio del mouse o è a fuoco (come vedresti su un catalogo di prodotti di e-commerce).

Abbiamo fatto un esempio molto semplice, che puoi trovare su [mouse-and-keyboard-events.html](https://mdn.github.io/learning-area/accessibility/css/mouse-and-keyboard-events.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/accessibility/css/mouse-and-keyboard-events.html)). Il codice presenta due funzioni che mostrano e nascondono l'immagine ingrandita; queste vengono eseguite dalle seguenti righe che le impostano come gestori di eventi:

```js
imgThumb.onmouseover = showImg;
imgThumb.onmouseout = hideImg;

imgThumb.onfocus = showImg;
imgThumb.onblur = hideImg;
```

Le prime due righe eseguono le funzioni quando il puntatore del mouse passa sopra e smette di passare sopra la miniatura, rispettivamente. Questo non ci permetterà però di accedere alla vista ingrandita tramite tastiera — per permetterlo, abbiamo incluso le ultime due righe, che eseguono le funzioni quando l'immagine è a fuoco e non è più a fuoco (quando si perde il fuoco). Questo può essere fatto andando con tab sull'immagine, poiché abbiamo incluso `tabindex="0"` su di essa.

L'evento [click](/it/docs/Web/API/Element/click_event) è interessante — sembra dipendere dal mouse, ma la maggior parte dei browser attiverà i gestori di eventi [onclick](/it/docs/Web/API/Element/click_event) dopo che Invio/Ritorno viene premuto su un link o elemento del modulo che ha il fuoco, o quando tale elemento viene toccato su un dispositivo touchscreen. Questo però non funziona di default quando consenti a un evento non di default a fuoco di avere il fuoco usando tabindex — in tali casi devi rilevare specificamente quando quel tasto esatto viene premuto (vedi [Reintegrazione dell'accessibilità da tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in)).

## Metti alla prova le tue abilità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare di aver trattenuto queste informazioni prima di passare avanti — vedi [Metti alla prova le tue abilità: CSS e JavaScript per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript).

## Sommario

Speriamo che questo articolo le abbia dato un buon dettaglio e comprensione sui problemi di accessibilità legati all'uso di CSS e JavaScript sulle pagine web.

Avanti, WAI-ARIA!

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/HTML", "Learn_web_development/Core/Accessibility/WAI-ARIA_basics", "Learn_web_development/Core/Accessibility")}}
