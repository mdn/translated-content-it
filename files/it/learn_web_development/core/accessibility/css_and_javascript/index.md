---
title: Migliori pratiche di accessibilità CSS e JavaScript
short-title: CSS e JS accessibili
slug: Learn_web_development/Core/Accessibility/CSS_and_JavaScript
l10n:
  sourceCommit: b5437b737639d6952d18b95ebd1045ed73e4bfa7
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/HTML","Learn_web_development/Core/Accessibility/WAI-ARIA_basics", "Learn_web_development/Core/Accessibility")}}

CSS e JavaScript, se usati correttamente, hanno anche il potenziale per consentire esperienze web accessibili, o possono danneggiare significativamente l'accessibilità se utilizzati in modo improprio. Questo articolo delinea alcune migliori pratiche di CSS e JavaScript che dovrebbero essere considerate per garantire che anche i contenuti complessi siano il più accessibili possibile.

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
          <li>Dimensionamento del testo e layout accessibili.</li>
          <li>Contrasto dei colori.</li>
          <li>L'importanza degli stili <code>:focus</code> e <code>:hover</code>.</li>
          <li>Uso sensato delle animazioni — utilizzare animazioni in modo sottile e fornire controlli per disattivarle.</li>
          <li>Migliori pratiche per nascondere contenuti in modo che non diventino inaccessibili.</li>
          <li>Il concetto che esiste un eccesso di JavaScript, e il valore di un JavaScript poco invasivo.</li>
          <li>Utilizzare gli eventi in modo sensato per non bloccare tipi di controllo specifici.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## CSS e JavaScript sono accessibili?

CSS e JavaScript non hanno la stessa immediata importanza per l'accessibilità come l'HTML, ma possono comunque aiutare o danneggiare l'accessibilità, a seconda di come vengono utilizzati. In altre parole, è importante che si consideri alcune indicazioni di buone pratiche per garantire che l'uso di CSS e JavaScript non rovini l'accessibilità dei documenti.

## CSS

Iniziamo esaminando il CSS.

### Semantica corretta e aspettativa dell'utente

È possibile utilizzare CSS per far apparire un qualsiasi elemento HTML in qualsiasi forma, ma ciò non significa che si dovrebbe farlo. Come abbiamo frequentemente menzionato nel nostro articolo [HTML: Una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML), si dovrebbe utilizzare l'elemento semantico appropriato per il compito, quando possibile. Se non lo si fa, si possono creare confusione e problemi di usabilità per tutti, ma in particolare per gli utenti con disabilità. Utilizzare la semantica corretta ha molto a che fare con le aspettative degli utenti — gli elementi hanno un certo aspetto e si comportano in determinati modi, a seconda della loro funzionalità, e queste convenzioni comuni sono attese dagli utenti.

Ad esempio, un utente di screen reader non può navigare in una pagina tramite elementi di intestazione se lo sviluppatore non ha utilizzato correttamente gli elementi di intestazione per marcare i contenuti. Allo stesso modo, un'intestazione perde il suo scopo visivo se viene stilizzata in modo tale che non sembri un'intestazione.

In sintesi, si può aggiornare lo stile di una caratteristica di pagina per adattarla al proprio design, ma non cambiarlo così tanto da non farla più apparire o comportarsi come previsto. Le seguenti sezioni riassumono le principali funzionalità HTML da considerare.

#### Struttura del contenuto "standard" del testo

Intestazioni, paragrafi, elenchi — il contenuto testuale principale della pagina:

```html
<h1>Heading</h1>

<p>Paragraph</p>

<ul>
  <li>My list</li>
  <li>has two items.</li>
</ul>
```

Un tipico CSS potrebbe essere il seguente:

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

Si dovrebbe:

- Selezionare dimensioni di carattere, altezze di linea, spaziatura tra lettere, ecc. sensate per rendere il testo logico, leggibile e confortevole da leggere.
- Assicurarsi che le intestazioni si distinguano dal testo del corpo, tipicamente grandi e in grassetto come lo stile predefinito. I propri elenchi dovrebbero sembrare elenchi.
- Il colore del testo dovrebbe contrastare bene con il colore di sfondo.

Vedi [Intestazioni e paragrafi in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs) e [Stile del testo CSS](/it/docs/Learn_web_development/Core/Text_styling) per ulteriori informazioni.

#### Testo enfatizzato

Markup inline che conferisce un'enfasi specifica al testo che avvolge:

```html
<p>The water is <em>very hot</em>.</p>

<p>
  Water droplets collecting on surfaces is called <strong>condensation</strong>.
</p>
```

Si potrebbe voler aggiungere un po' di colorazione semplice al testo enfatizzato:

```css
strong,
em {
  color: #a60000;
}
```

Tuttavia, raramente avrai bisogno di stilizzare elementi di enfasi in modo significativo. Le convenzioni standard del testo in grassetto e corsivo sono molto riconoscibili e cambiare lo stile può causare confusione. Per ulteriori informazioni sull'enfasi, vedi [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance).

#### Abbreviazioni

Un elemento che consente a un'abbreviazione, acronimo o inizializzazione di essere associata alla sua espansione:

```html
<p>
  Web content is marked up using Hypertext Markup Language, or
  <abbr>HTML</abbr>.
</p>
```

Ancora una volta, si potrebbe volerlo stilizzare in un modo semplice:

```css
abbr {
  color: #a60000;
}
```

La convenzione di stile riconosciuta per le abbreviazioni è una sottolineatura punteggiata, ed è sconsigliabile deviare significativamente da essa. Per maggiori dettagli sulle abbreviazioni, vedi [Abbreviazioni](/it/docs/Learn_web_development/Core/Structuring_content/Advanced_text_features#abbreviations).

#### Link

Iperlink — il modo per raggiungere nuovi luoghi sul web:

```html
<p>Visit the <a href="https://www.mozilla.org">Mozilla homepage</a>.</p>
```

Sotto è mostrato un semplice stile di link:

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

Le convenzioni standard dei link sono sottolineati e di colore diverso (predefinito: blu) nel loro stato standard, un'altra variazione di colore quando il link è stato precedentemente visitato (predefinito: viola) e un altro colore ancora quando il link è attivato (predefinito: rosso). Inoltre, il puntatore del mouse cambia in un'icona a freccia quando i link sono sorvolati con il mouse e il link riceve un'evidenziazione quando è a fuoco (ad esempio, tramite tabulazione) o attivato. Le seguenti immagini mostrano l'evidenziazione sia in Firefox (un contorno tratteggiato) che in Chrome (un contorno blu):

![Schermata di un elenco di link nel browser Firefox. L'elenco contiene 4 elementi. Il secondo elemento dell'elenco è evidenziato utilizzando un contorno blu tratteggiato quando è focalizzato tramite tabulazione.](focus-highlight-firefox.png)

![Schermata di un elenco di link nel browser Chrome. L'elenco contiene 4 elementi. Il terzo elemento dell'elenco è evidenziato utilizzando un contorno blu quando è focalizzato tramite tabulazione.](focus-highlight-chrome.png)

Si può essere creativi con gli stili dei link, purché si continui a fornire feedback agli utenti quando interagiscono con i link. Qualcosa dovrebbe sicuramente accadere quando gli stati cambiano e non si dovrebbe rimuovere il cursore a freccia o il contorno — entrambi sono aiuti importanti per l'accessibilità per coloro che utilizzano i controlli da tastiera.

#### Elementi del modulo

Elementi che consentono agli utenti di inserire dati nei siti web:

```html
<div>
  <label for="name">Enter your name</label>
  <input type="text" id="name" name="name" />
</div>
```

Puoi vedere un buon esempio di CSS nei moduli nel nostro esempio [form-css.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-css.html) (vedi anche [l'esempio dal vivo](https://mdn.github.io/learning-area/accessibility/css/form-css.html)).

La maggior parte del CSS che scriverai per i moduli sarà per dimensionare gli elementi, allineare etichette e input e farli apparire ordinati.

Non si dovrebbe però deviare troppo dal feedback visivo previsto che gli elementi del modulo ricevono quando sono a fuoco, che è fondamentalmente lo stesso dei link (vedi sopra). Si potrebbe stilizzare gli stati di focus/hover del modulo per rendere questo comportamento più coerente tra i browser o adattarlo meglio al design della pagina, ma non eliminarlo del tutto — ancora, le persone si affidano a questi indizi per sapere cosa sta succedendo.

#### Tabelle

Tabelle per presentare dati tabulari.

Puoi vedere un buon, semplice esempio di HTML e CSS per una tabella nel nostro esempio [table-css.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/table-css.html) (vedi anche [l'esempio dal vivo](https://mdn.github.io/learning-area/accessibility/css/table-css.html)).

Il CSS delle tabelle generalmente serve a far sì che la tabella si integri meglio nel design e appaia meno brutta. È una buona idea assicurarsi che le intestazioni della tabella si distinguano (normalmente usando il grassetto) e utilizzare strisce zebrate per rendere più facile l'analisi delle righe.

### Colore e contrasto dei colori

Quando si sceglie uno schema di colori per il proprio sito web, assicurarsi che il colore del testo (in primo piano) contrasti bene con il colore di sfondo. Il design potrebbe sembrare interessante, ma non serve a nulla se persone con disabilità visive come il daltonismo non possono leggere i tuoi contenuti.

Esiste un modo semplice per verificare se il contrasto è sufficientemente elevato per non causare problemi. Ci sono diversi strumenti online per il controllo del contrasto in cui puoi inserire i colori del tuo primo piano e dello sfondo, per verificarli. Ad esempio, il [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM è semplice da usare e fornisce una spiegazione di ciò di cui hai bisogno per conformarti ai criteri WCAG relativi al contrasto di colore.

> [!NOTE]
> Un alto rapporto di contrasto consentirà anche a chi utilizza uno smartphone o un tablet con uno schermo lucido di leggere meglio le pagine quando si trovano in un ambiente luminoso, come alla luce del sole.

Un altro suggerimento è di non affidarsi solo al colore per indicazioni/informazioni, in quanto non sarà utile per coloro che non possono vedere il colore. Invece di contrassegnare i campi del modulo obbligatori in rosso, ad esempio, contrassegnali con un asterisco e in rosso.

### Nascondere le cose

Ci sono molte istanze in cui un design visivo richiederà che non tutto il contenuto venga mostrato contemporaneamente. Ad esempio, nel nostro [esempio di box informativo a schede](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/tabbed-info-box.html) (vedi [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html)) abbiamo tre pannelli di informazioni, ma li stiamo [posizionando](/it/docs/Learn_web_development/Core/CSS_layout/Positioning) uno sopra l'altro e fornendo tabulazioni che possono essere cliccate per mostrarne ciascuno (è anche accessibile da tastiera — si può alternativamente usare Tab e Invio/Ritorno per selezionarli).

![Interfaccia a tre schede con Scheda 1 selezionata e solo i suoi contenuti visualizzati. I contenuti delle altre schede sono nascosti. Se una scheda viene selezionata, il colore del testo cambia da nero a bianco e il colore di sfondo cambia da rosso-arancio a marrone sella.](tabbed-info-box.png)

Gli utenti dei lettori di schermo non si preoccupano di tutto ciò — sono felici con il contenuto purché l'ordine delle fonti abbia senso e possano accedervi tutto. Il posizionamento assoluto (come usato in questo esempio) è generalmente considerato uno dei migliori meccanismi per nascondere il contenuto per effetti visivi perché non impedisce ai lettori di schermo di accedervi.

D'altra parte, non si dovrebbe usare {{cssxref("visibility", "visibility: hidden")}} o {{cssxref("display", "display: none")}}, perché nascondono il contenuto dai lettori di schermo. A meno che, naturalmente, non vi sia una buona ragione per cui si desidera che questo contenuto sia nascosto dai lettori di schermo.

> **Nota:** [Content invisibile solo per gli utenti dei lettori di schermo](https://webaim.org/techniques/css/invisiblecontent/) ha molti più dettagli utili su questo argomento.

### Accettare che gli utenti possano sovrascrivere gli stili

È possibile per gli utenti sovrascrivere i tuoi stili con i propri stili personalizzati, ad esempio:

- Vedi il [Come utilizzare un foglio di stile personalizzato (CSS) con Firefox](https://www.itsupportguides.com/knowledge-base/computer-accessibility/how-to-use-a-custom-style-sheet-css-with-firefox/) di Sarah Maddox per una guida utile su come fare questo manualmente in Firefox.
- È probabilmente più semplice farlo utilizzando un'estensione. Ad esempio, l'estensione Stylus è disponibile per [Firefox](https://addons.mozilla.org/en-US/firefox/addon/styl-us/), con Stylish che è un equivalente per [Chrome](https://chromewebstore.google.com/detail/stylish-custom-themes-for/fjnbnpbmkenffdnngjfgmeleoegfcffe).

Gli utenti potrebbero farlo per diverse ragioni. Un utente con disabilità visiva potrebbe voler rendere il testo più grande su tutti i siti web che visita, o un utente con una grave deficienza dei colori potrebbe voler impostare tutti i siti web in colori ad alto contrasto che siano facili da vedere. Qualunque sia la necessità, si dovrebbe essere a proprio agio con ciò, e rendere flessibili i propri design in modo che tali modifiche funzionino nel proprio design. Ad esempio, si potrebbe voler garantire che l'area dei contenuti principali possa gestire il testo più grande (forse inizierà a scorrere per consentire di vedere tutto) e non lo nasconda o si rompa completamente.

## JavaScript

Anche il JavaScript può compromettere l'accessibilità, a seconda di come viene usato.

Il JavaScript moderno è un linguaggio potente, e possiamo fare molto con esso al giorno d'oggi, dai semplici aggiornamenti di contenuti e UI a giochi 2D e 3D completi. Non c'è una regola che imponga che tutti i contenuti debbano essere completamente accessibili a tutte le persone — devi solo fare ciò che puoi, e rendere le tue applicazioni il più accessibili possibile.

Contenuti e funzionalità semplici sono in teoria facili da rendere accessibili — ad esempio testo, immagini, tabelle, moduli e pulsanti che attivano funzioni. Come abbiamo esaminato nel nostro articolo [HTML: Una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML), le considerazioni chiave sono:

- Buona semantica: Utilizzare l'elemento giusto per il lavoro giusto. Ad esempio, assicurarsi di usare intestazioni e paragrafi, e gli elementi {{htmlelement("button")}} e {{htmlelement("a")}}
- Assicurarsi che i contenuti siano disponibili come testo, sia direttamente come contenuti testuali, buone etichette di testo per gli elementi del modulo, o [alternative testuali](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives), ad esempio, testo alternativo per le immagini.

Abbiamo anche esaminato un esempio di come utilizzare JavaScript per costruire funzionalità laddove mancano — vedi [Ricostruzione dell'accessibilità da tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in). Questo non è l'ideale — in realtà dovresti solo utilizzare l'elemento giusto per il lavoro giusto — ma mostra che è possibile in situazioni in cui per qualche motivo non puoi controllare il markup utilizzato. Un altro modo per migliorare l'accessibilità per widget JavaScript non semantici è utilizzare WAI-ARIA per fornire semantica aggiuntiva agli utenti dei lettori di schermo. Il prossimo articolo affronterà questo argomento in dettaglio.

Funzionalità complesse come i giochi 3D non sono così facili da rendere accessibili — un gioco 3D complesso creato utilizzando [WebGL](/it/docs/Web/API/WebGL_API) sarà reso su un elemento {{htmlelement("canvas")}}, che non ha attualmente la possibilità di fornire alternative testuali o altre informazioni che utenti con gravi disabilità visive possano utilizzare. È discutibile che un gioco del genere non abbia realmente questo gruppo di persone come parte del suo pubblico target principale, e sarebbe irragionevole aspettarsi che tu lo renda completamente accessibile alle persone non vedenti. Tuttavia, si potrebbero implementare [controlli da tastiera](/it/docs/Games/Techniques/Control_mechanisms/Desktop_with_mouse_and_keyboard) affinché sia utilizzabile dagli utenti che non usano il mouse e rendere lo schema dei colori abbastanza contrastante da essere utilizzabile da chi ha carenze di colore.

### Il problema con troppo JavaScript

Il problema spesso si verifica quando le persone si affidano troppo al JavaScript. A volte si vede un sito web in cui tutto è stato fatto con JavaScript — l'HTML è stato generato dal JavaScript, il CSS è stato generato dal JavaScript, ecc. Ciò comporta vari problemi di accessibilità e altri problemi associati, quindi non è consigliato.

Oltre a utilizzare l'elemento giusto per il lavoro giusto, si dovrebbe anche assicurarsi di utilizzare la tecnologia giusta per il lavoro giusto! Pensa attentamente se hai bisogno di quella scintillante casella informativa 3D alimentata da JavaScript o se andrebbe bene il testo normale. Pensa attentamente se hai bisogno di un widget di modulo complesso e non standard o se basterebbe un input di testo. E non generare tutti i tuoi contenuti HTML usando JavaScript se possibile.

### Mantenerlo poco invasivo

Dovresti tenere a mente **JavaScript poco invasivo** quando crei i tuoi contenuti. L'idea del JavaScript poco invasivo è che dovrebbe essere utilizzato ove possibile per migliorare la funzionalità, non costruirla interamente — le funzioni di base dovrebbero idealmente funzionare senza JavaScript, sebbene si apprezzi che ciò non sia sempre un'opzione. Ma ancora, una gran parte di questo è l'utilizzo della funzionalità integrata del browser dove possibile.

Esempi d'uso di JavaScript poco invasivo includono:

- Fornire validazione dei moduli lato client, che avvisa rapidamente gli utenti sui problemi con le loro voci di modulo, senza dover aspettare che il server verifichi i dati. Se non disponibile, il modulo funzionerà comunque, ma la convalida potrebbe essere più lenta.
- Fornire controlli personalizzati per i `<video>` HTML che siano accessibili agli utenti che utilizzano solo la tastiera, insieme a un link diretto al video che può essere utilizzato per accedervi se JavaScript non è disponibile (i controlli browser predefiniti del `<video>` non sono accessibili da tastiera nella maggior parte dei browser).

Ad esempio, abbiamo scritto un esempio rapido e sporco di validazione dei moduli lato client — vedi [form-validation.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) (vedi anche [l'esempio demo dal vivo](https://mdn.github.io/learning-area/accessibility/css/form-validation.html)). Qui vedrai un modulo semplice; quando si tenta di inviare il modulo con uno o entrambi i campi lasciati vuoti, l'invio fallisce e appare una finestra di messaggio di errore per dirti cosa è sbagliato.

Questo tipo di convalida del modulo è poco invasiva — puoi ancora usare il modulo assolutamente senza problemi senza che il JavaScript sia disponibile, e qualsiasi implementazione sensata del modulo avrà anche la convalida lato server attiva, perché è troppo facile per utenti malintenzionati bypassare la convalida lato client (ad esempio, disattivando JavaScript nel browser). La convalida lato client è ancora davvero utile per segnalare errori — gli utenti possono sapere immediatamente degli errori e correggerli piuttosto che aspettare un viaggio di andata e ritorno al server e il ricaricamento della pagina. Questo è un vantaggio di usabilità certo.

> [!NOTE]
> La convalida lato server non è stata implementata in questo semplice demo.

Abbiamo reso questo esempio di validazione dei moduli piuttosto accessibile. Abbiamo usato elementi {{htmlelement("label")}} per assicurarci che le etichette dei moduli siano chiaramente collegate ai loro input, così i lettori di schermo possono leggerle insieme:

```html
<label for="name">Enter your name:</label>
<input type="text" name="name" id="name" />
```

Eseguiamo la convalida solo quando il modulo viene inviato — questo per evitare di aggiornare troppo spesso l'interfaccia utente e potenzialmente confondere gli utenti di lettori di schermo (e forse altri):

```js
form.onsubmit = validate;

function validate(e) {
  errorList.textContent = "";
  for (const testItem of formItems) {
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
> In questo esempio, stiamo nascondendo e mostrando la finestra di errore utilizzando il posizionamento assoluto piuttosto che un altro metodo come visibilità o display, perché non interferisce con la capacità dello screen reader di leggere i contenuti.

La vera convalida dei moduli sarebbe molto più complessa di questa — si vorrebbe verificare che il nome inserito sembri effettivamente un nome, che l'età inserita sia effettivamente un numero e sia realistico (ad esempio, non negativo e con meno di 4 cifre). Qui abbiamo solo implementato un semplice controllo che un valore sia stato inserito in ciascun campo di input (`if (testItem.input.value === '')`).

Quando la convalida è stata effettuata, se i test passano, il modulo viene inviato. Se ci sono errori (`if (errorList.hasChildNodes())`) allora fermiamo l'invio del modulo (utilizzando [`preventDefault()`](/it/docs/Web/API/Event/preventDefault)), e visualizziamo eventuali messaggi di errore che sono stati creati (vedi sotto). Questo meccanismo significa che gli errori verranno mostrati solo se ci sono errori, il che è migliore per l'usabilità.

Per ogni input che non ha un valore inserito quando il modulo viene inviato, creiamo un elemento di lista con un link e lo inseriamo in `errorList`.

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

Ogni link ha un duplice scopo — ti dice qual è l'errore, inoltre puoi cliccarci sopra / attivarlo per passare direttamente all'elemento di input in questione e correggere la tua voce.

Inoltre, `errorField` è posizionato all'inizio dell'ordine delle sorgenti (anche se è posizionato diversamente nell'interfaccia utente usando CSS), il che significa che gli utenti possono sapere esattamente cosa non va con le loro invii moduli e accedere agli elementi di input in questione tornando indietro all'inizio della pagina.

Come nota finale, abbiamo usato alcuni attributi WAI-ARIA nel nostro demo per aiutare a risolvere i problemi di accessibilità causati da aree di contenuto che si aggiornano costantemente senza un ricaricamento della pagina (i lettori di schermo non rilevano questo né avvertono gli utenti di default):

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

Spiegheremo questi attributi nel nostro prossimo articolo, che tratta in dettaglio [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics).

> [!NOTE]
> Alcuni di voi probabilmente staranno pensando al fatto che i moduli HTML hanno meccanismi di convalida integrati come gli attributi `required`, `min`/`minlength` e `max`/`maxlength` (vedere il riferimento all'elemento {{htmlelement("input")}} per ulteriori informazioni). Non abbiamo finito per usare questi nel demo perché il supporto incrociato tra browser è lacunoso (ad esempio solo IE10 e versioni successive).

> [!NOTE]
> Il documento di WebAIM [Usable and Accessible Form Validation and Error Recovery](https://webaim.org/techniques/formvalidation/) fornisce ulteriori utili informazioni sulla convalida dei moduli accessibili.

### Altri problemi di accessibilità di JavaScript

Ci sono altre cose da tenere a mente quando si implementa JavaScript pensando all'accessibilità. Aggiungeremo altri argomenti man mano che li troviamo.

#### eventi specifici per il mouse

Come saprai, la maggior parte delle interazioni degli utenti sono implementate in JavaScript lato client utilizzando gestori di eventi, che ci permettono di eseguire funzioni in risposta a determinati eventi che accadono. Alcuni eventi possono causare problemi di accessibilità. L'esempio principale che incontrerai sono gli eventi specifici per il mouse come [mouseover](/it/docs/Web/API/Element/mouseover_event), [mouseout](/it/docs/Web/API/Element/mouseout_event), [dblclick](/it/docs/Web/API/Element/dblclick_event), ecc. Le funzionalità che vengono eseguite in risposta a questi eventi non saranno accessibili utilizzando altri meccanismi, come i controlli da tastiera.

Per mitigare tali problemi, si dovrebbe duplicare questi eventi con eventi simili che possono essere attivati con altri mezzi (i cosiddetti gestori di eventi indipendenti dal dispositivo) — [focus](/it/docs/Web/API/Element/focus_event) e [blur](/it/docs/Web/API/Element/blur_event) offrirebbero accessibilità agli utenti di tastiera.

Esaminiamo un esempio che evidenzia quando ciò potrebbe essere utile. Forse vogliamo fornire un'immagine in miniatura che mostra una versione più grande dell'immagine quando è sorvolata con il mouse o focalizzata (come si vede in un catalogo di prodotti e-commerce).

Abbiamo realizzato un esempio molto semplice, che puoi trovare in [mouse-and-keyboard-events.html](https://mdn.github.io/learning-area/accessibility/css/mouse-and-keyboard-events.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/accessibility/css/mouse-and-keyboard-events.html)). Il codice presenta due funzioni che mostrano e nascondono l'immagine ingrandita; queste sono eseguite dalle seguenti righe che le impostano come gestori di eventi:

```js
imgThumb.onmouseover = showImg;
imgThumb.onmouseout = hideImg;

imgThumb.onfocus = showImg;
imgThumb.onblur = hideImg;
```

Le prime due righe eseguono le funzioni quando il puntatore del mouse passa sopra e smette di passare sopra la miniatura, rispettivamente. Tuttavia questo non ci permetterà di accedere alla vista ingrandita da tastiera — per permetterlo, abbiamo incluso le ultime due righe, che eseguono le funzioni quando l'immagine è focalizzata e sfocata (quando il fuoco si interrompe). Questo può essere fatto passando sopra l'immagine, poiché abbiamo incluso `tabindex="0"` su di essa.

L'evento [click](/it/docs/Web/API/Element/click_event) è interessante — sembra dipendente dal mouse, ma la maggior parte dei browser attiverà i gestori di eventi [onclick](/it/docs/Web/API/Element/click_event) dopo che il tasto Invio/Ritorno è premuto su un link o un elemento form che ha il focus, o quando tale elemento è toccato su un dispositivo touchscreen. Questo non funziona di default tuttavia quando si permette che un evento con messa a fuoco non predefinito abbia il focus usando tabindex — in tali casi è necessario rilevare specificamente quando quel tasto esatto viene premuto (vedi [Ricostruzione dell'accessibilità da tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in)).

## Metti alla prova le tue capacità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue capacità: accessibilità CSS e JavaScript](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript).

## Riepilogo

Speriamo che questo articolo ti abbia fornito una buona quantità di dettagli e comprensione sui problemi di accessibilità legati all'uso di CSS e JavaScript nelle pagine web.

Prossimamente, WAI-ARIA!

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/HTML","Learn_web_development/Core/Accessibility/WAI-ARIA_basics", "Learn_web_development/Core/Accessibility")}}
