---
title: Best practice di accessibilità per CSS e JavaScript
short-title: CSS e JS accessibili
slug: Learn_web_development/Core/Accessibility/CSS_and_JavaScript
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/HTML","Learn_web_development/Core/Accessibility/WAI-ARIA_basics", "Learn_web_development/Core/Accessibility")}}

CSS e JavaScript, quando usati correttamente, hanno il potenziale di permettere esperienze web accessibili, o possono danneggiare significativamente l'accessibilità se usati in modo errato. Questo articolo illustra alcune pratiche migliori per CSS e JavaScript che dovrebbero essere considerate per garantire che anche i contenuti più complessi siano il più accessibili possibile.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, una <a href="/it/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">comprensione di base dei concetti di accessibilità</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Dimensionamento del testo e layout accessibile.</li>
          <li>Contrasto del colore.</li>
          <li>L'importanza degli stili <code>:focus</code> e <code>:hover</code>.</li>
          <li>Utilizzo sensato delle animazioni — usare l'animazione in modo sottile e fornire controlli per disattivarla.</li>
          <li>Migliori pratiche per nascondere contenuti in modo che non diventi inaccessibile.</li>
          <li>Che esiste un eccesso di JavaScript e il valore di JavaScript discreto.</li>
          <li>Utilizzo sensato degli eventi in modo da non escludere tipi di controllo specifici.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## CSS e JavaScript sono accessibili?

CSS e JavaScript non hanno la stessa importanza immediata per l'accessibilità come l'HTML, ma possono comunque aiutare o danneggiare l'accessibilità, a seconda di come vengono utilizzati. In altre parole, è importante considerare alcuni consigli sulle buone pratiche per assicurarsi che l'uso di CSS e JavaScript non rovini l'accessibilità dei tuoi documenti.

## CSS

Iniziamo osservando il CSS.

### Semantica corretta e aspettativa dell'utente

È possibile usare CSS per far sì che qualsiasi elemento HTML sembri _qualsiasi cosa_, ma questo non significa che dovresti farlo. Come abbiamo menzionato frequentemente nel nostro articolo [HTML: Una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML), dovresti usare l'elemento semantico appropriato per il lavoro, ogni volta che possibile. Se non lo fai, può causare confusione e problemi di usabilità per tutti, ma in particolare per gli utenti con disabilità. Usare la corretta semantica ha molto a che fare con le aspettative degli utenti — gli elementi appaiono e si comportano in modi certi, secondo la loro funzionalità, e queste convenzioni comuni sono attese dagli utenti.

Ad esempio, un utente di lettore di schermo non può navigare in una pagina tramite gli elementi di intestazione se lo sviluppatore non ha usato appropriatamente gli elementi di intestazione per marcare il contenuto. Allo stesso modo, un'intestazione perde il suo scopo visivo se la stili in modo che non sembri un'intestazione.

In sostanza, puoi aggiornare lo stile di una caratteristica della pagina per adattarla al tuo design, ma non cambiarla così tanto da farla sembrare o comportarsi in modo inappropriato. Le seguenti sezioni riassumono le principali caratteristiche HTML da considerare.

#### Struttura del contenuto testuale "standard"

Intestazioni, paragrafi, elenchi — il contenuto testuale principale della tua pagina:

```html
<h1>Heading</h1>

<p>Paragraph</p>

<ul>
  <li>My list</li>
  <li>has two items.</li>
</ul>
```

Alcuni tipici CSS potrebbero apparire così:

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

- Selezionare dimensioni di font sensate, altezze di riga, spaziatura tra lettere, ecc. per rendere il tuo testo logico, leggibile e confortevole da leggere.
- Assicurarti che le tue intestazioni si distinguano dal corpo del testo, tipicamente grandi e in grassetto come lo stile predefinito. I tuoi elenchi dovrebbero sembrare elenchi.
- Il colore del tuo testo dovrebbe contrastare bene con il colore di sfondo.

Vedi [Intestazioni e paragrafi in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs) e [Stile del testo in CSS](/it/docs/Learn_web_development/Core/Text_styling) per maggiori informazioni.

#### Testo enfatizzato

Markup inline che conferisce un'enfasi specifica al testo che avvolge:

```html
<p>The water is <em>very hot</em>.</p>

<p>
  Water droplets collecting on surfaces is called <strong>condensation</strong>.
</p>
```

Potresti voler aggiungere una semplice colorazione al tuo testo enfatizzato:

```css
strong,
em {
  color: #a60000;
}
```

Tuttavia, raramente avrai bisogno di stilizzare gli elementi di enfasi in modo significativo. Le convenzioni standard del testo in grassetto e corsivo sono molto riconoscibili, e cambiare lo stile può causare confusione. Per maggiori informazioni sull'enfasi, vedi [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance).

#### Abbreviazioni

Un elemento che consente a un'abbreviazione, acronimo o inizializzazione di essere associata alla sua espansione:

```html
<p>
  Web content is marked up using Hypertext Markup Language, or
  <abbr>HTML</abbr>.
</p>
```

Ancora, potresti volerlo stilizzare in modo semplice:

```css
abbr {
  color: #a60000;
}
```

La convenzione di stile riconosciuta per le abbreviazioni è una sottolineatura punteggiata, ed è imprudente deviare significativamente da questo. Per più informazioni sulle abbreviazioni, vedi [Abbreviazioni](/it/docs/Learn_web_development/Core/Structuring_content/Advanced_text_features#abbreviations).

#### Link

Ipercollegamenti — il modo in cui raggiungi nuovi luoghi sul web:

```html
<p>Visit the <a href="https://www.mozilla.org">Mozilla homepage</a>.</p>
```

Di seguito è mostrato uno stile di link molto semplice:

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

Le convenzioni standard di link prevedono che siano sottolineati e di un colore diverso (predefinito: blu) nel loro stato standard, un'altra variazione di colore quando il link è stato precedentemente visitato (predefinito: viola), e un altro colore quando il link è attivato (predefinito: rosso). Inoltre, il puntatore del mouse cambia in un'icona a forma di puntatore quando i link vengono passati sopra con il mouse, e il link riceve un'evidenziazione quando è focalizzato (ad esempio, tramite tabulazione) o attivato. Le seguenti immagini mostrano l'evidenziazione sia in Firefox (un contorno punteggiato) che in Chrome (un contorno blu):

![Screenshot di un elenco di link nel browser Firefox. L'elenco contiene 4 elementi. Il secondo elemento dell'elenco è evidenziato utilizzando un contorno blu punteggiato quando è focalizzato tramite tabulazione.](focus-highlight-firefox.png)

![Screenshot di un elenco di link nel browser Chrome. L'elenco contiene 4 elementi. Il terzo elemento dell'elenco è evidenziato utilizzando un contorno blu quando è focalizzato tramite tabulazione.](focus-highlight-chrome.png)

Puoi essere creativo con gli stili dei link, purché continui a fornire feedback agli utenti quando interagiscono con i link. Qualcosa dovrebbe decisamente succedere quando gli stati cambiano, e non dovresti eliminare il cursore a puntatore o il contorno — entrambi sono aiuti di accessibilità molto importanti per chi usa i controlli da tastiera.

#### Elementi di form

Elementi per consentire agli utenti di inserire dati nei siti web:

```html
<div>
  <label for="name">Enter your name</label>
  <input type="text" id="name" name="name" />
</div>
```

Puoi vedere un buon esempio di CSS in [form-css.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-css.html) nel nostro esempio (vedi anche la [versione live](https://mdn.github.io/learning-area/accessibility/css/form-css.html)).

La maggior parte del CSS che scriverai per i moduli riguarderà il dimensionamento degli elementi, l'allineamento delle etichette e degli input, e l'ottenimento di un aspetto ordinato e pulito.

Tuttavia, non dovresti discostarti troppo dal feedback visivo previsto che gli elementi di form ricevono quando sono focalizzati, che è fondamentalmente lo stesso dei link (vedi sopra). Potresti stilare gli stati di focus/hover del form per rendere questo comportamento più coerente tra i browser o adattarlo meglio al design della tua pagina, ma non eliminarlo completamente — ancora, le persone si affidano a questi indizi per capire cosa sta succedendo.

#### Tabelle

Tabelle per la presentazione di dati tabulari.

Puoi vedere un buon esempio semplice di HTML e CSS per tabelle nel nostro [table-css.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/table-css.html) esempio (vedi anche la [versione live](https://mdn.github.io/learning-area/accessibility/css/table-css.html)).

Il CSS per le tabelle serve generalmente per far sì che la tabella si integri meglio nel tuo design e sia meno sgradevole all'aspetto. È una buona idea assicurarsi che le intestazioni delle tabelle si distinguano (normalmente usando il grassetto), e usare motivi a zebre per rendere più facile la lettura delle diverse righe.

### Colore e contrasto del colore

Quando scegli uno schema di colori per il tuo sito web, assicurati che il colore del testo (primo piano) contrasti bene con il colore di sfondo. Il tuo design potrebbe sembrare interessante, ma non serve a nulla se le persone con disabilità visive come daltonismo non riescono a leggere il tuo contenuto.

Esiste un modo semplice per verificare se il tuo contrasto è sufficiente per non causare problemi. Esistono diversi strumenti online di verifica del contrasto in cui puoi inserire i tuoi colori di primo piano e di sfondo per controllarli. Ad esempio, il [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM è semplice da usare e fornisce una spiegazione di ciò che è necessario per conformarsi ai criteri WCAG sul contrasto del colore.

> [!NOTE]
> Un elevato rapporto di contrasto permetterà inoltre a chi utilizza uno smartphone o tablet con uno schermo lucido di leggere meglio le pagine in un ambiente luminoso, come sotto il sole.

Un altro suggerimento è di non affidarsi solo al colore per indicazioni/informazioni, poiché non servirà a chi non riesce a vedere il colore. Invece di segnare i campi obbligatori di un modulo in rosso, ad esempio, contrassegnali con un asterisco e in rosso.

### Nascondere elementi

Ci sono molti casi in cui un design visivo richiederà che non tutti i contenuti siano mostrati contemporaneamente. Ad esempio, nel nostro [esempio di box informativo con schede](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/tabbed-info-box.html) (vedi [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html)) abbiamo tre pannelli di informazioni, ma stiamo [posizionando](/it/docs/Learn_web_development/Core/CSS_layout/Positioning) uno sopra l'altro e fornendo schede che possono essere cliccate per mostrare ciascuna (è anche accessibile tramite tastiera — puoi alternativamente usare Tab e Invio/Return per selezionarle).

![Interfaccia a tre schede con Scheda 1 selezionata e solo il suo contenuto è visualizzato. I contenuti delle altre schede sono nascosti. Se una scheda è selezionata, il colore del testo cambia da nero a bianco e il colore di sfondo cambia da rosso-arancio a marrone-sella.](tabbed-info-box.png)

Gli utenti di lettori di schermo non si preoccupano di tutto questo — sono felici del contenuto purché l'ordine delle fonti abbia senso e possano accedervi tutto. Il posizionamento assoluto (come usato in questo esempio) è generalmente visto come uno dei migliori meccanismi per nascondere contenuti per effetto visivo perché non impedisce ai lettori di schermo di accedervi.

D'altra parte, non dovresti usare {{cssxref("visibility", "visibility: hidden")}} o {{cssxref("display", "display: none")}}, perché nascondono i contenuti anche ai lettori di schermo. A meno, ovviamente, che non ci sia una buona ragione per cui vuoi che questo contenuto sia nascosto ai lettori di schermo.

> **Note:** [Contenuti invisibili solo per gli utenti di lettori di schermo](https://webaim.org/techniques/css/invisiblecontent/) offre molti più dettagli utili su questo argomento.

### Accettare che gli utenti possano sovrascrivere gli stili

È possibile per gli utenti sovrascrivere i tuoi stili con i loro stili personalizzati, ad esempio:

- Vedi il [Come usare un foglio di stile personalizzato (CSS) con Firefox](https://www.itsupportguides.com/knowledge-base/computer-accessibility/how-to-use-a-custom-style-sheet-css-with-firefox/) di Sarah Maddox per una guida utile su come farlo manualmente in Firefox.
- Probabilmente è più facile farlo utilizzando un'estensione. Ad esempio, l'estensione Stylus è disponibile per [Firefox](https://addons.mozilla.org/en-US/firefox/addon/styl-us/), mentre Stylish è un equivalente per [Chrome](https://chromewebstore.google.com/detail/stylish-custom-themes-for/fjnbnpbmkenffdnngjfgmeleoegfcffe).

Gli utenti potrebbero fare questo per una varietà di ragioni. Un utente con disabilità visiva potrebbe voler ingrandire il testo su tutti i siti che visita, o un utente con grave carenza di colore potrebbe voler mettere tutti i siti in colori ad alto contrasto che sono facili per lui da vedere. Qualunque sia la necessità, dovresti sentirti a tuo agio con questo e rendere i tuoi design abbastanza flessibili in modo che tali cambiamenti funzionino nel tuo design. Ad esempio, potresti voler assicurarti che la tua area di contenuto principale possa gestire un testo più grande (magari inizierà a scorrere per consentire di vedere tutto) e non nasconderlo semplicemente o rompersi completamente.

## JavaScript

Anche JavaScript può rompere l'accessibilità, a seconda di come viene utilizzato.

Il JavaScript moderno è un linguaggio potente e possiamo fare tanto con esso oggi, dai semplici aggiornamenti di contenuto e interfaccia utente a giochi completamente sviluppati in 2D e 3D. Non c'è una regola che dice che tutti i contenuti devono essere 100% accessibili a tutte le persone — devi solo fare ciò che puoi e rendere le tue applicazioni il più accessibili possibile.

I contenuti e le funzionalità semplici sono considerati più facili da rendere accessibili — ad esempio testo, immagini, tabelle, moduli e pulsanti che attivano funzioni. Come abbiamo visto nel nostro articolo [HTML: Una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML), le considerazioni chiave sono:

- Buona semantica: Usare l'elemento giusto per il lavoro giusto. Ad esempio, assicurarti di usare titoli e paragrafi e elementi {{htmlelement("button")}} e {{htmlelement("a")}}.
- Assicurarsi che il contenuto sia disponibile come testo, sia direttamente come contenuto testuale, buone etichette testuali per gli elementi del modulo o [alternative testuali](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives), ad esempio, testo alternativo per le immagini.

Abbiamo anche esaminato un esempio di come utilizzare JavaScript per costruire funzionalità dove mancano — vedi [Ricostruire l'accessibilità da tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in). Questo non è ideale — in realtà dovresti solo usare l'elemento giusto per il lavoro giusto — ma mostra che è possibile in situazioni in cui per qualche motivo non puoi controllare il markup che viene utilizzato. Un altro modo per migliorare l'accessibilità per i widget non semantici alimentati da JavaScript è usare WAI-ARIA per fornire ulteriori semantiche per gli utenti di lettori di schermo. Il prossimo articolo coprirà anche questo in dettaglio.

Funzionalità complesse come i giochi 3D non sono così facili da rendere accessibili — un gioco 3D complesso creato usando [WebGL](/it/docs/Web/API/WebGL_API) verrà renderizzato su un elemento {{htmlelement("canvas")}}, che al momento non ha la capacità di fornire alternative testuali o altre informazioni per gli utenti con gravi disabilità visive. È discutibile che un tale gioco non abbia davvero questo gruppo di persone come parte del suo pubblico di destinazione principale e sarebbe irragionevole aspettarsi che tu lo renda 100% accessibile alle persone non vedenti. Tuttavia, potresti implementare [controlli da tastiera](/it/docs/Games/Techniques/Control_mechanisms/Desktop_with_mouse_and_keyboard) in modo che sia utilizzabile dagli utenti che non usano il mouse, e rendere lo schema di colori contrastante abbastanza da essere utilizzabile da quelli con carenze di colore.

### Il problema con troppo JavaScript

Il problema spesso si presenta quando le persone si affidano troppo a JavaScript. A volte vedrai un sito web in cui tutto è stato fatto con JavaScript — l'HTML è stato generato da JavaScript, il CSS è stato generato da JavaScript, ecc. Questo ha tutti i tipi di problemi di accessibilità e altri associati, quindi non è consigliato.

Oltre a usare l'elemento giusto per il lavoro giusto, dovresti anche assicurarti di usare la tecnologia giusta per il lavoro giusto! Pensa attentamente se hai bisogno di quel luccicante box informativo 3D alimentato da JavaScript o se il semplice testo andrebbe bene. Pensa attentamente se hai bisogno di un widget di form complesso non standard o se un input di testo andrebbe bene. E non generare tutto il tuo contenuto HTML utilizzando JavaScript se possibile.

### Mantenere JavaScript discreto

Dovresti tenere a mente **JavaScript discreto** quando crei i tuoi contenuti. L'idea di JavaScript discreto è che dovrebbe essere usato dove possibile per migliorare la funzionalità, non costruirla completamente — le funzioni di base dovrebbero idealmente funzionare senza essere necessarie a JavaScript, sebbene sia apprezzato che ciò non sia sempre un'opzione. Ma ancora, una gran parte di questo è usare la funzionalità incorporata del browser dove possibile.

Buoni esempi di uso di JavaScript discreto includono:

- Fornire convalida del modulo lato client, avvisando gli utenti di problemi con le loro inserzioni di moduli rapidamente, senza dover aspettare che il server controlli i dati. Se non è disponibile, il modulo funzionerà comunque, ma la convalida potrebbe essere più lenta.
- Fornire controlli personalizzati per un `<video>` HTML che siano accessibili agli utenti che usano solo la tastiera, insieme a un link diretto al video che può essere usato per accedervi se JavaScript non è disponibile (i controlli predefiniti del browser per `<video>` non sono accessibili alla tastiera nella maggior parte dei browser).

Per esempio, abbiamo scritto un semplice esempio di convalida del modulo lato client — vedi [form-validation.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) (vedi anche la [demo live](https://mdn.github.io/learning-area/accessibility/css/form-validation.html)). Qui vedrai un semplice modulo; quando provi a inviare il modulo lasciando uno o entrambi i campi vuoti, l'invio fallisce e appare una casella di messaggio di errore per dirti cosa è sbagliato.

Questo tipo di convalida del modulo è discreto — puoi comunque usare il modulo assolutamente bene senza che JavaScript sia disponibile, e qualsiasi implementazione di modulo sensata avrà anche una convalida lato server attiva, perché è troppo facile per gli utenti malintenzionati bypassare la convalida lato client (ad esempio, disattivando JavaScript nel browser). La convalida lato client è comunque davvero utile per riportare gli errori — gli utenti possono essere informati immediatamente degli errori che commettono invece di dover aspettare un round trip al server e un ricaricamento della pagina. Questo è un vantaggio di usabilità decisivo.

> [!NOTE]
> La convalida lato server non è stata implementata in questa semplice demo.

Abbiamo reso questa convalida del modulo abbastanza accessibile. Abbiamo usato elementi {{htmlelement("label")}} per assicurarci che le etichette del modulo siano inequivocabilmente collegate ai loro input, in modo che i lettori di schermo possano leggerle insieme:

```html
<label for="name">Enter your name:</label>
<input type="text" name="name" id="name" />
```

Effettuiamo la convalida solo quando il modulo viene inviato — questo per evitare di aggiornare l'interfaccia utente troppo spesso e potenzialmente confondere gli utenti di lettori di schermo (e possibilmente altri).

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
> In questo esempio, stiamo nascondendo e mostrando la casella di messaggio di errore usando la posizione assoluta piuttosto che un altro metodo come la visibilità o il display, perché non interferisce con la capacità del lettore di schermo di leggere il contenuto.

Una vera convalida del modulo sarebbe molto più complessa di questa — vorresti controllare che il nome inserito sembri effettivamente un nome, l'età inserita sia effettivamente un numero ed è realistica (ad esempio, non negativo e meno di 4 cifre). Qui abbiamo implementato solo un semplice controllo che un valore sia stato inserito in ogni campo di input (`if (testItem.input.value === '')`).

Quando la convalida è stata eseguita, se i test passano, il modulo viene inviato. Se ci sono errori (`if (errorList.hasChildNodes())`), evitiamo che il modulo venga inviato (usando [`preventDefault()`](/it/docs/Web/API/Event/preventDefault)) e visualizziamo i messaggi di errore eventualmente creati (vedi sotto). Questo meccanismo significa che gli errori verranno mostrati solo se ci sono errori, il che è meglio per l'usabilità.

Per ogni input che non ha un valore inserito quando il modulo viene inviato, creiamo un elemento di elenco con un link e lo inseriamo nel `errorList`.

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

Ogni link serve a uno scopo doppio — ti dice qual è l'errore, e puoi fare clic su di esso/attivarlo per andare direttamente all'elemento di input in questione e correggere la tua inserzione.

Inoltre, il `errorField` è posizionato all'inizio dell'ordine delle fonti (anche se è posizionato diversamente nell'interfaccia utente usando il CSS), il che significa che gli utenti possono scoprire esattamente cosa c'è che non va con i loro invii del modulo e accedere agli elementi di input in questione tornando all'inizio della pagina.

Come nota finale, abbiamo utilizzato alcuni attributi WAI-ARIA nella nostra demo per aiutare a risolvere i problemi di accessibilità causati da aree di contenuto che si aggiornano costantemente senza un ricaricamento della pagina (i lettori di schermo non lo noteranno o lo segnaleranno agli utenti di default):

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

Spiegheremo questi attributi nel nostro prossimo articolo, che copre [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics) in modo molto più dettagliato.

> [!NOTE]
> Alcuni di voi probabilmente staranno pensando al fatto che i moduli HTML hanno meccanismi di convalida incorporati come gli attributi `required`, `min`/`minlength` e `max`/`maxlength` (vedi il riferimento all'elemento {{htmlelement("input")}} per maggiori informazioni). Non li abbiamo usati nella demo perché il supporto cross-browser per loro è lacunoso (ad esempio, IE10 e versioni successive solo).

> [!NOTE]
> Il [Form Validation and Error Recovery Usabile e Accessibile](https://webaim.org/techniques/formvalidation/) di WebAIM fornisce ulteriori informazioni utili sulla convalida accessibile dei moduli.

### Altre preoccupazioni sull'accessibilità di JavaScript

Ci sono altre cose di cui essere consapevoli quando si implementa JavaScript e si pensa all'accessibilità. Aggiungeremo di più quando li troveremo.

#### Eventi specifici per mouse

Come saprai, la maggior parte delle interazioni degli utenti viene implementata in JavaScript lato client utilizzando gestori di eventi, che ci permettono di eseguire funzioni in risposta a determinati eventi che si verificano. Alcuni eventi possono avere problemi di accessibilità. L'esempio principale che incontrerai sono gli eventi specifici per mouse come [mouseover](/it/docs/Web/API/Element/mouseover_event), [mouseout](/it/docs/Web/API/Element/mouseout_event), [dblclick](/it/docs/Web/API/Element/dblclick_event), ecc. Le funzionalità che vengono eseguite in risposta a questi eventi non saranno accessibili utilizzando altri meccanismi, come i controlli da tastiera.

Per attenuare tali problemi, dovresti raddoppiare questi eventi con eventi simili che possono essere attivati tramite altri mezzi (i cosiddetti gestori di eventi indipendenti dal dispositivo) — [focus](/it/docs/Web/API/Element/focus_event) e [blur](/it/docs/Web/API/Element/blur_event) fornirebbero l'accessibilità per gli utenti che utilizzano la tastiera.

Vediamo un esempio che evidenzia quando ciò potrebbe essere utile. Forse vogliamo fornire un'immagine in miniatura che mostri una versione più grande dell'immagine quando viene passata con il mouse o è focalizzata (come vedresti in un catalogo di prodotti di e-commerce).

Abbiamo realizzato un esempio molto semplice, che puoi trovare in [mouse-and-keyboard-events.html](https://mdn.github.io/learning-area/accessibility/css/mouse-and-keyboard-events.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/accessibility/css/mouse-and-keyboard-events.html)). Il codice presenta due funzioni che mostrano e nascondono l'immagine ingrandita; queste vengono eseguite dalle seguenti righe che le impostano come gestori di eventi:

```js
imgThumb.onmouseover = showImg;
imgThumb.onmouseout = hideImg;

imgThumb.onfocus = showImg;
imgThumb.onblur = hideImg;
```

Le prime due righe eseguono le funzioni quando il puntatore del mouse passa sopra e smette di passare sopra la miniatura, rispettivamente. Tuttavia, questo non ci permetterà di accedere alla vista ingrandita tramite tastiera — per permetterlo, abbiamo incluso le ultime due righe che eseguono le funzioni quando l'immagine è focalizzata e sfocata (quando termina il focus). Questo può essere fatto tabulando sull'immagine, perché abbiamo incluso `tabindex="0"` su di essa.

L'evento [click](/it/docs/Web/API/Element/click_event) è interessante — sembra dipendente dal mouse, ma la maggior parte dei browser attiverà i gestori di eventi [onclick](/it/docs/Web/API/Element/click_event) dopo che Enter/Return viene premuto su un link o un elemento del modulo che ha il focus, o quando un tale elemento viene toccato su un dispositivo touchscreen. Tuttavia, questo non funziona di default quando consenti che un evento non focalizzabile di default ottenga il focus utilizzando tabindex — in tali casi devi specificamente rilevare quando esattamente quel tasto viene premuto (vedi [Ricostruire l'accessibilità da tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in)).

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare che hai trattenuto queste informazioni prima di passare oltre — vedi [Metti alla prova le tue abilità: Accessibilità CSS e JavaScript](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript).

## Riepilogo

Speriamo che questo articolo ti abbia fornito una buona dose di dettagli e comprensione sui problemi di accessibilità legati all'uso di CSS e JavaScript sulle pagine web.

Prossimamente, WAI-ARIA!

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/HTML","Learn_web_development/Core/Accessibility/WAI-ARIA_basics", "Learn_web_development/Core/Accessibility")}}
