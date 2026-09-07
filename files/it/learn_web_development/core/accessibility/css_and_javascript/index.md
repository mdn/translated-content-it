---
title: Buone pratiche di accessibilità per CSS e JavaScript
short-title: CSS e JS accessibili
slug: Learn_web_development/Core/Accessibility/CSS_and_JavaScript
l10n:
  sourceCommit: 0c62b082755017d0773ecaaee7e74efd5e066d0b
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Test_your_skills/HTML","Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript", "Learn_web_development/Core/Accessibility")}}

CSS e JavaScript, se usati correttamente, possono consentire esperienze web accessibili, oppure possono danneggiare significativamente l'accessibilità se usati in modo improprio. Questo articolo descrive alcune buone pratiche relative a CSS e JavaScript da considerare per garantire che anche i contenuti complessi siano il più possibile accessibili.

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
          <li>Dimensionamento e layout del testo accessibili.</li>
          <li>Contrasto del colore.</li>
          <li>L'importanza degli stili <code>:focus</code> e <code>:hover</code>.</li>
          <li>Uso sensato delle animazioni — usare le animazioni in modo discreto e fornire controlli per disattivarle.</li>
          <li>Buone pratiche per nascondere contenuti senza renderli inaccessibili.</li>
          <li>Il fatto che troppo JavaScript può essere eccessivo e il valore del JavaScript non invasivo.</li>
          <li>Uso sensato degli eventi per non escludere specifici tipi di controllo.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## CSS e JavaScript sono accessibili?

CSS e JavaScript non hanno la stessa importanza immediata per l'accessibilità di HTML, ma possono comunque favorirla o danneggiarla, a seconda di come vengono usati. In altre parole, è importante considerare alcune buone pratiche per assicurarsi che l'uso di CSS e JavaScript non comprometta l'accessibilità dei documenti.

## CSS

Iniziamo esaminando CSS.

### Semantica corretta e aspettative degli utenti

È possibile usare CSS per rendere qualsiasi elemento HTML simile a _qualsiasi cosa_, ma questo non significa che sia opportuno farlo. Come menzionato spesso nell'articolo [HTML: una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML), occorre usare l'elemento semantico appropriato per lo scopo, ogni volta che è possibile. In caso contrario, ciò può causare confusione e problemi di usabilità per tutti, ma in particolare per gli utenti con disabilità. L'uso di una semantica corretta è strettamente legato alle aspettative degli utenti: gli elementi hanno un determinato aspetto e comportamento in base alla loro funzionalità, e gli utenti si aspettano queste convenzioni comuni.

Per esempio, un utente di screen reader non può navigare in una pagina tramite gli elementi di intestazione se lo sviluppatore non ha usato in modo appropriato gli elementi di intestazione per marcare il contenuto. Allo stesso modo, un'intestazione perde il suo scopo visivo se viene applicato uno stile che fa sì che non sembri un'intestazione.

In sintesi, è possibile aggiornare lo stile di una funzionalità della pagina per adattarla al design, ma non bisogna modificarla così tanto da farle perdere l'aspetto o il comportamento atteso. Le sezioni seguenti riassumono le principali funzionalità HTML da considerare.

#### Struttura del contenuto testuale "standard"

Intestazioni, paragrafi, elenchi: il contenuto testuale fondamentale della pagina:

```html
<h1>Heading</h1>

<p>Paragraph</p>

<ul>
  <li>My list</li>
  <li>has two items.</li>
</ul>
```

Alcuni CSS tipici potrebbero essere simili a questo:

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

Occorre:

- Scegliere dimensioni dei font, altezze di riga, spaziatura tra lettere e così via sensate, per rendere il testo logico, leggibile e confortevole da leggere.
- Assicurarsi che le intestazioni risaltino rispetto al testo del corpo, in genere grandi e in grassetto come nello stile predefinito. Gli elenchi devono sembrare elenchi.
- Assicurarsi che il colore del testo contrasti adeguatamente con il colore di sfondo.

Per ulteriori informazioni, vedere [Intestazioni e paragrafi in HTML](/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs) e [Stilizzazione del testo CSS](/it/docs/Learn_web_development/Core/Text_styling).

#### Testo enfatizzato

Markup inline che conferisce un'enfasi specifica al testo che racchiude:

```html
<p>The water is <em>very hot</em>.</p>

<p>
  Water droplets collecting on surfaces is called <strong>condensation</strong>.
</p>
```

Potrebbe essere opportuno aggiungere una semplice colorazione al testo enfatizzato:

```css
strong,
em {
  color: #a60000;
}
```

Tuttavia, raramente sarà necessario applicare uno stile significativo agli elementi di enfasi. Le convenzioni standard del testo in grassetto e in corsivo sono facilmente riconoscibili e modificare lo stile può causare confusione. Per ulteriori informazioni sull'enfasi, vedere [Enfasi e importanza](/it/docs/Learn_web_development/Core/Structuring_content/Emphasis_and_importance).

#### Abbreviazioni

Un elemento che consente di associare un'abbreviazione, un acronimo o un'inizializzazione alla sua forma estesa:

```html
<p>
  Web content is marked up using Hypertext Markup Language, or
  <abbr>HTML</abbr>.
</p>
```

Anche in questo caso, potrebbe essere opportuno applicare uno stile semplice:

```css
abbr {
  color: #a60000;
}
```

La convenzione di stile riconosciuta per le abbreviazioni è una sottolineatura tratteggiata, ed è poco saggio discostarsi in modo significativo da essa. Per ulteriori informazioni sulle abbreviazioni, vedere [Abbreviazioni](/it/docs/Learn_web_development/Core/Structuring_content/Advanced_text_features#abbreviations).

#### Link

Hyperlink: il modo per raggiungere nuove destinazioni sul web:

```html
<p>Visit the <a href="https://www.mozilla.org">Mozilla homepage</a>.</p>
```

Di seguito viene mostrato uno stile molto semplice per i link:

```css
a {
  color: red;
}

a:hover,
a:visited,
a:focus {
  color: #a60000;
  text-decoration: none;
}

a:active {
  color: black;
  background-color: #a60000;
}
```

Le convenzioni standard per i link prevedono la sottolineatura e un colore diverso (predefinito: blu) nel loro stato normale, un'altra variazione di colore quando il link è stato visitato in precedenza (predefinito: viola) e un ulteriore colore quando il link è attivato (predefinito: rosso). Inoltre, il puntatore del mouse cambia in un'icona a forma di mano quando passa sui link e il link riceve un'evidenziazione quando riceve il focus, ad esempio tramite la navigazione con Tab, oppure quando viene attivato. L'immagine seguente mostra l'evidenziazione sia in Firefox (un contorno tratteggiato) sia in Chrome (un contorno blu):

![Screenshot di un elenco di link nel browser Firefox. L'elenco contiene 4 elementi. Il secondo elemento dell'elenco è evidenziato con un contorno tratteggiato blu quando riceve il focus tramite la navigazione con Tab.](focus-highlight-firefox.png)

![Screenshot di un elenco di link nel browser Chrome. L'elenco contiene 4 elementi. Il terzo elemento dell'elenco è evidenziato con un contorno blu quando riceve il focus tramite la navigazione con Tab.](focus-highlight-chrome.png)

È possibile essere creativi con gli stili dei link, purché si continui a fornire agli utenti un feedback quando interagiscono con i link. Deve sicuramente accadere qualcosa quando gli stati cambiano e non bisogna rimuovere il cursore a forma di puntatore o il contorno: entrambi sono importanti strumenti di accessibilità per chi usa i controlli da tastiera.

#### Elementi dei moduli

Elementi che consentono agli utenti di inserire dati nei siti web:

```html
<div>
  <label for="name">Enter your name</label>
  <input type="text" id="name" name="name" />
</div>
```

È possibile vedere alcuni buoni esempi di CSS nell'esempio [form-css.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-css.html) ([visualizzarlo dal vivo](https://mdn.github.io/learning-area/accessibility/css/form-css.html)).

La maggior parte del CSS scritto per i moduli riguarderà il dimensionamento degli elementi, l'allineamento di etichette e input e il conferimento di un aspetto ordinato.

Tuttavia, non bisogna discostarsi troppo dal feedback visivo atteso che gli elementi dei moduli ricevono quando hanno il focus, che è sostanzialmente uguale a quello dei link (vedere sopra). È possibile applicare stili agli stati di focus/hover del modulo per rendere questo comportamento più coerente tra browser o più adatto al design della pagina, ma non bisogna eliminarlo del tutto: le persone si basano su questi indizi per capire cosa sta accadendo.

#### Tabelle

Tabelle per presentare dati tabulari.

È possibile vedere un buon esempio semplice di HTML e CSS per una tabella nell'esempio [table-css.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/table-css.html) ([visualizzarlo dal vivo](https://mdn.github.io/learning-area/accessibility/css/table-css.html)).

In genere, il CSS delle tabelle serve a far sì che la tabella si integri meglio nel design e abbia un aspetto meno sgradevole. È una buona idea assicurarsi che le intestazioni della tabella risaltino, normalmente usando il grassetto, e usare l'alternanza di righe per rendere più facile distinguere le diverse righe.

### Colore e contrasto del colore

Quando si sceglie una combinazione di colori per un sito web, assicurarsi che il colore del testo (primo piano) contrasti adeguatamente con il colore di sfondo. Il design potrebbe sembrare interessante, ma non è utile se le persone con disabilità visive, come il daltonismo, non riescono a leggere il contenuto.

Esiste un modo semplice per verificare se il contrasto è sufficientemente elevato da non causare problemi. Esistono diversi strumenti online per il controllo del contrasto nei quali è possibile inserire i colori di primo piano e di sfondo per verificarli. Per esempio, il [Color Contrast Checker](https://webaim.org/resources/contrastchecker/) di WebAIM è semplice da usare e fornisce una spiegazione dei requisiti necessari per conformarsi ai criteri WCAG relativi al contrasto del colore.

> [!NOTE]
> Un elevato rapporto di contrasto consente inoltre a chiunque utilizzi uno smartphone o un tablet con schermo lucido di leggere meglio le pagine in un ambiente luminoso, come alla luce del sole.

Un altro suggerimento è non fare affidamento esclusivamente sul colore per segnali o informazioni, poiché questo non è utile per chi non riesce a vedere il colore. Invece di contrassegnare in rosso i campi obbligatori di un modulo, per esempio, contrassegnarli con un asterisco e in rosso.

### Nascondere elementi

Esistono molte situazioni in cui un design visivo richiede che non tutto il contenuto venga mostrato contemporaneamente. Per esempio, nell'[esempio di riquadro informativo a schede](https://mdn.github.io/learning-area/css/css-layout/practical-positioning-examples/tabbed-info-box.html) (vedere il [codice sorgente](https://github.com/mdn/learning-area/blob/main/css/css-layout/practical-positioning-examples/tabbed-info-box.html)) sono presenti tre pannelli di informazioni, ma vengono [posizionati](/it/docs/Learn_web_development/Core/CSS_layout/Positioning) uno sopra l'altro e vengono fornite schede selezionabili per mostrarne una alla volta. L'esempio è anche accessibile tramite tastiera: in alternativa, è possibile usare Tab e Invio per selezionarle.

![Interfaccia con tre schede, con la scheda 1 selezionata e visualizzato solo il suo contenuto. I contenuti delle altre schede sono nascosti. Se una scheda è selezionata, il suo text-color cambia da nero a bianco e il background-color cambia da rosso-arancio a marrone sella.](tabbed-info-box.png)

Gli utenti di screen reader non si preoccupano di tutto questo: il contenuto va bene purché l'ordine nel sorgente sia sensato e sia possibile accedere a tutto. Il posizionamento assoluto, come quello usato in questo esempio, è generalmente considerato uno dei migliori meccanismi per nascondere contenuti a scopo visivo, perché non impedisce agli screen reader di accedervi.

D'altra parte, non bisogna usare {{cssxref("visibility", "visibility: hidden")}} o {{cssxref("display", "display: none")}}, perché nascondono il contenuto agli screen reader. A meno che, naturalmente, non ci sia una buona ragione per cui questo contenuto debba essere nascosto agli screen reader.

> [!NOTE]
> [Invisible Content Just for Screen Reader Users](https://webaim.org/techniques/css/invisiblecontent/) contiene molti altri dettagli utili su questo argomento.

### Accettare che gli utenti possano sovrascrivere gli stili

Gli utenti possono sovrascrivere gli stili con i propri stili personalizzati, per esempio:

- Vedere [How to use a custom style sheet (CSS) with Firefox](https://www.itsupportguides.com/knowledge-base/computer-accessibility/how-to-use-a-custom-style-sheet-css-with-firefox/) di Sarah Maddox, una Guida utile che spiega come farlo manualmente in Firefox.
- Probabilmente è più semplice farlo usando un'estensione. Per esempio, l'estensione Stylus è disponibile per [Firefox](https://addons.mozilla.org/en-US/firefox/addon/styl-us/), mentre Stylish è un equivalente per [Chrome](https://chromewebstore.google.com/detail/stylish-custom-themes-for/fjnbnpbmkenffdnngjfgmeleoegfcffe).

Gli utenti potrebbero farlo per diversi motivi. Un utente con disabilità visiva potrebbe voler ingrandire il testo in tutti i siti web visitati, oppure un utente con grave deficit nella percezione dei colori potrebbe voler impostare tutti i siti web con colori ad alto contrasto che riesce facilmente a vedere. Qualunque sia la necessità, occorre accettarla e rendere i design sufficientemente flessibili affinché tali modifiche funzionino. Per esempio, è opportuno assicurarsi che l'area del contenuto principale possa gestire testo più grande, magari iniziando a scorrere per consentire di visualizzarlo interamente, e non lo nasconda o si interrompa completamente.

## JavaScript

JavaScript può anche compromettere l'accessibilità, a seconda di come viene usato.

Il JavaScript moderno è un linguaggio potente e oggi è possibile fare moltissime cose con esso, da semplici aggiornamenti di contenuto e UI fino a giochi 2D e 3D completi. Non esiste una regola secondo cui tutti i contenuti debbano essere accessibili al 100% a tutte le persone: occorre semplicemente fare ciò che è possibile e rendere le applicazioni il più accessibili possibile.

I contenuti e le funzionalità semplici sono probabilmente facili da rendere accessibili, per esempio testo, immagini, tabelle, moduli e pulsanti che attivano funzioni. Come esaminato nell'articolo [HTML: una buona base per l'accessibilità](/it/docs/Learn_web_development/Core/Accessibility/HTML), le considerazioni principali sono:

- Buona semantica: usare l'elemento giusto per lo scopo giusto. Per esempio, assicurarsi di usare intestazioni e paragrafi, nonché gli elementi {{htmlelement("button")}} e {{htmlelement("a")}}.
- Assicurarsi che il contenuto sia disponibile come testo, direttamente come contenuto testuale, tramite buone etichette testuali per gli elementi dei moduli oppure tramite [alternative testuali](/it/docs/Learn_web_development/Core/Accessibility/HTML#text_alternatives), ad esempio il testo alt per le immagini.

È stato inoltre esaminato un esempio di come usare JavaScript per aggiungere funzionalità dove mancano: vedere [Ripristinare l'accessibilità tramite tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in). Non è l'ideale: in realtà, occorrerebbe semplicemente usare l'elemento giusto per lo scopo giusto, ma l'esempio mostra che è possibile farlo nelle situazioni in cui, per qualche motivo, non si può controllare il markup utilizzato. Un altro modo per migliorare l'accessibilità dei widget non semantici alimentati da JavaScript consiste nell'usare WAI-ARIA per fornire semantica aggiuntiva agli utenti di screen reader. Anche il prossimo articolo approfondirà questo argomento.

Funzionalità complesse come i giochi 3D non sono altrettanto facili da rendere accessibili: un gioco 3D complesso creato con [WebGL](/it/docs/Web/API/WebGL_API) verrà renderizzato su un elemento {{htmlelement("canvas")}}, che al momento non offre alcuna funzionalità per fornire alternative testuali o altre informazioni utilizzabili da utenti con gravi disabilità visive. Si può sostenere che un gioco di questo tipo non abbia realmente questo gruppo di persone tra il proprio pubblico di destinazione principale e che sarebbe irragionevole aspettarsi che sia accessibile al 100% alle persone cieche. Tuttavia, sarebbe possibile implementare [controlli da tastiera](/it/docs/Games/Techniques/Control_mechanisms/Desktop_with_mouse_and_keyboard) in modo che sia utilizzabile da utenti che non usano il mouse e rendere la combinazione di colori sufficientemente contrastata da essere utilizzabile da chi ha deficit nella percezione dei colori.

### Il problema di troppo JavaScript

Il problema si presenta spesso quando si fa troppo affidamento su JavaScript. Talvolta si incontra un sito web in cui tutto è stato realizzato con JavaScript: l'HTML è stato generato da JavaScript, il CSS è stato generato da JavaScript e così via. Ciò comporta ogni tipo di problema relativo all'accessibilità e non solo, quindi non è consigliabile.

Oltre a usare l'elemento giusto per lo scopo giusto, occorre assicurarsi di usare anche la tecnologia giusta per lo scopo giusto. Valutare attentamente se serve quel vistoso riquadro informativo 3D alimentato da JavaScript oppure se sarebbe sufficiente del semplice testo. Valutare attentamente se serve un widget di modulo complesso e non standard oppure se sarebbe sufficiente un input di testo. E non generare tutto il contenuto HTML usando JavaScript, se possibile.

### Mantenerlo non invasivo

Durante la creazione dei contenuti, occorre tenere presente il **JavaScript non invasivo**. L'idea del JavaScript non invasivo è che debba essere usato, ove possibile, per migliorare la funzionalità, non per costruirla interamente: le funzioni di base dovrebbero idealmente funzionare senza JavaScript, anche se non sempre è possibile. Ma ancora una volta, gran parte di questo consiste nell'usare le funzionalità integrate del browser quando possibile.

Buoni esempi di uso del JavaScript non invasivo includono:

- Fornire la convalida dei moduli lato client, che avvisa rapidamente gli utenti dei problemi nelle voci del modulo senza dover attendere che il server controlli i dati. Se non è disponibile, il modulo continuerà a funzionare, ma la convalida potrebbe essere più lenta.
- Fornire controlli personalizzati per `<video>` HTML accessibili agli utenti che usano solo la tastiera, insieme a un link diretto al video che può essere usato per accedervi se JavaScript non è disponibile. I controlli `<video>` predefiniti del browser non sono accessibili tramite tastiera nella maggior parte dei browser.

Come esempio, è stato scritto un esempio rapido e semplice di convalida dei moduli lato client: vedere [form-validation.html](https://github.com/mdn/learning-area/blob/main/accessibility/css/form-validation.html) ([visualizzare anche la demo dal vivo](https://mdn.github.io/learning-area/accessibility/css/form-validation.html)). Qui è possibile vedere un semplice modulo: quando si tenta di inviarlo lasciando vuoto uno o entrambi i campi, l'invio non riesce e viene visualizzato un riquadro di messaggio di errore che indica il problema.

Questo tipo di convalida dei moduli è non invasivo: il modulo può comunque essere usato senza problemi anche se JavaScript non è disponibile e qualsiasi implementazione sensata di un modulo avrà attiva anche la convalida lato server, perché è troppo facile per utenti malevoli aggirare la convalida lato client, per esempio disattivando JavaScript nel browser. La convalida lato client è comunque molto utile per segnalare gli errori: gli utenti possono conoscere immediatamente gli sbagli commessi, invece di dover attendere un viaggio di andata e ritorno al server e il ricaricamento della pagina. Si tratta di un chiaro vantaggio in termini di usabilità.

> [!NOTE]
> La convalida lato server non è stata implementata in questa semplice demo.

Anche questa convalida del modulo è stata resa piuttosto accessibile. Sono stati usati elementi {{htmlelement("label")}} per assicurarsi che le etichette del modulo siano collegate in modo inequivocabile ai relativi input, così che gli screen reader possano leggerle insieme:

```html
<label for="name">Enter your name:</label>
<input type="text" name="name" id="name" />
```

La convalida viene effettuata solo quando il modulo viene inviato: questo evita di aggiornare troppo spesso la UI e potenzialmente confondere gli utenti di screen reader, e forse anche altri utenti:

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
> In questo esempio, il riquadro del messaggio di errore viene nascosto e mostrato usando il posizionamento assoluto anziché un altro metodo come visibility o display, perché non interferisce con la capacità dello screen reader di leggere il contenuto al suo interno.

La convalida reale di un modulo sarebbe molto più complessa: sarebbe necessario verificare che il nome inserito assomigli effettivamente a un nome, che l'età inserita sia effettivamente un numero e sia realistica, per esempio non negativa e con meno di 4 cifre. Qui è stato implementato solo un semplice controllo per verificare che sia stato inserito un valore in ogni campo di input (`if (testItem.input.value === '')`).

Dopo avere eseguito la convalida, se i controlli hanno esito positivo il modulo viene inviato. Se sono presenti errori (`if (errorList.hasChildNodes())`), l'invio del modulo viene impedito usando [`preventDefault()`](/it/docs/Web/API/Event/preventDefault) e vengono visualizzati tutti i messaggi di errore creati (vedere sotto). Questo meccanismo implica che gli errori vengano mostrati solo quando sono presenti, migliorando l'usabilità.

Per ogni input senza un valore inserito al momento dell'invio del modulo, viene creato un elemento di elenco con un link e inserito in `errorList`.

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

Ogni link ha un duplice scopo: indica qual è l'errore e può essere selezionato o attivato per passare direttamente all'elemento di input in questione e correggere l'immissione.

Inoltre, `errorField` è posto all'inizio dell'ordine nel sorgente, anche se viene posizionato diversamente nella UI tramite CSS, il che significa che gli utenti possono scoprire esattamente cosa non va nell'invio del modulo e raggiungere gli elementi di input interessati tornando all'inizio della pagina.

Infine, nella demo sono stati usati alcuni attributi WAI-ARIA per aiutare a risolvere problemi di accessibilità causati da aree di contenuto che si aggiornano costantemente senza un ricaricamento della pagina. Gli screen reader, per impostazione predefinita, non rilevano questo né avvisano gli utenti:

```html
<div class="errors" role="alert" aria-relevant="all">
  <ul></ul>
</div>
```

Questi attributi verranno spiegati nel prossimo articolo, che tratta [WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics) in modo molto più dettagliato.

> [!NOTE]
> Alcuni potrebbero pensare al fatto che i moduli HTML dispongono di meccanismi di convalida integrati, come gli attributi `required`, `min`/`minlength` e `max`/`maxlength` (per ulteriori informazioni, vedere il riferimento dell'elemento {{htmlelement("input")}}). Non sono stati usati nella demo perché il supporto tra browser è discontinuo, per esempio solo IE10 e versioni successive.

> [!NOTE]
> [Usable and Accessible Form Validation and Error Recovery](https://webaim.org/techniques/formvalidation/) di WebAIM fornisce ulteriori informazioni utili sulla convalida accessibile dei moduli.

### Altri aspetti di accessibilità relativi a JavaScript

Esistono altri aspetti da considerare quando si implementa JavaScript e si riflette sull'accessibilità. Ne verranno aggiunti altri man mano che verranno individuati.

#### Eventi specifici del mouse

Come noto, la maggior parte delle interazioni utente viene implementata in JavaScript lato client usando event handler, che consentono di eseguire funzioni in risposta al verificarsi di determinati eventi. Alcuni eventi possono comportare problemi di accessibilità. L'esempio principale che si incontrerà è quello degli eventi specifici del mouse, come [mouseover](/it/docs/Web/API/Element/mouseover_event), [mouseout](/it/docs/Web/API/Element/mouseout_event), [dblclick](/it/docs/Web/API/Element/dblclick_event) e così via. Le funzionalità eseguite in risposta a questi eventi non saranno accessibili usando altri meccanismi, come i controlli da tastiera.

Per attenuare questi problemi, occorre affiancare tali eventi a eventi simili che possono essere attivati con altri mezzi, i cosiddetti event handler indipendenti dal dispositivo. Gli eventi [focus](/it/docs/Web/API/Element/focus_event) e [blur](/it/docs/Web/API/Element/blur_event) fornirebbero accessibilità agli utenti della tastiera.

Vediamo un esempio che evidenzia quando questo potrebbe essere utile. Si potrebbe voler fornire un'immagine in miniatura che mostra una versione più grande dell'immagine quando il mouse vi passa sopra o quando riceve il focus, come avviene in un catalogo di prodotti e-commerce.

È stato creato un esempio molto semplice, disponibile in [mouse-and-keyboard-events.html](https://mdn.github.io/learning-area/accessibility/css/mouse-and-keyboard-events.html) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/accessibility/css/mouse-and-keyboard-events.html)). Il codice include due funzioni che mostrano e nascondono l'immagine ingrandita; queste vengono eseguite dalle righe seguenti, che le impostano come event handler:

```js
imgThumb.onmouseover = showImg;
imgThumb.onmouseout = hideImg;

imgThumb.onfocus = showImg;
imgThumb.onblur = hideImg;
```

Le prime due righe eseguono le funzioni rispettivamente quando il puntatore del mouse passa sopra e smette di passare sopra la miniatura. Tuttavia, ciò non consentirebbe di accedere alla vista ingrandita tramite tastiera. Per consentirlo, sono incluse le ultime due righe, che eseguono le funzioni quando l'immagine riceve e perde il focus. Questo può essere fatto passando con Tab sull'immagine, perché su di essa è stato incluso `tabindex="0"`.

L'evento [click](/it/docs/Web/API/Element/click_event) è interessante: sembra dipendere dal mouse, ma la maggior parte dei browser attiva gli event handler [onclick](/it/docs/Web/API/Element/click_event) dopo la pressione di Invio su un link o elemento di modulo con focus, oppure quando tale elemento viene toccato su un dispositivo touchscreen. Tuttavia, questo non funziona per impostazione predefinita quando si consente a un elemento non focalizzabile per impostazione predefinita di ricevere il focus usando tabindex: in questi casi è necessario rilevare specificamente la pressione di quel tasto esatto (vedere [Ripristinare l'accessibilità tramite tastiera](/it/docs/Learn_web_development/Core/Accessibility/HTML#building_keyboard_accessibility_back_in)).

## Riepilogo

Ci auguriamo che questo articolo abbia fornito una buona quantità di dettagli e comprensione dei problemi di accessibilità relativi all'uso di CSS e JavaScript nelle pagine web.

Nel prossimo articolo verranno forniti alcuni test da usare per verificare quanto bene siano state comprese e ricordate tutte queste informazioni.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Test_your_skills/HTML","Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript", "Learn_web_development/Core/Accessibility")}}
