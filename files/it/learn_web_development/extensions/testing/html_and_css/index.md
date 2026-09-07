---
title: Gestione dei problemi comuni di HTML e CSS
short-title: Problemi comuni di HTML e CSS
slug: Learn_web_development/Extensions/Testing/HTML_and_CSS
l10n:
  sourceCommit: f4c14731a1a157fc8d8f7357ac4d74d14a7d7fb5
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Testing_strategies","Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing")}}

Dopo aver definito il contesto, verranno ora esaminati in modo specifico i problemi comuni tra browser che si possono incontrare nel codice HTML e CSS, nonché gli strumenti che possono essere usati per impedire che si verifichino problemi o per risolvere quelli che si presentano. Ciò include il linting del codice, la gestione dei prefissi CSS, l'uso degli strumenti per sviluppatori del browser per individuare i problemi, l'uso di polyfill per aggiungere supporto nei browser, la risoluzione dei problemi di responsive design e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; conoscenza
        dei principi di alto livello
        <a
          href="/it/docs/Learn_web_development/Extensions/Testing/Introduction"
          >del testing tra browser</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di diagnosticare problemi comuni di HTML e CSS tra browser e
        utilizzare strumenti e tecniche appropriati per risolverli.
      </td>
    </tr>
  </tbody>
</table>

## I problemi di HTML e CSS

Alcuni problemi di HTML e CSS derivano dal fatto che entrambi i linguaggi sono piuttosto semplici e spesso gli sviluppatori non li prendono seriamente, in termini di assicurarsi che il codice sia ben realizzato, efficiente e descriva semanticamente lo scopo delle funzionalità presenti nella pagina. Nei casi peggiori, JavaScript viene usato per generare l'intero contenuto e lo stile della pagina web, rendendo le pagine inaccessibili e meno performanti (generare elementi DOM è costoso). In altri casi, funzionalità nascenti non sono supportate in modo coerente tra browser, e ciò può impedire il funzionamento di alcune funzionalità e stili per alcuni utenti. Anche i problemi di responsive design sono comuni: un sito che appare bene in un browser desktop potrebbe offrire un'esperienza terribile su un dispositivo mobile, perché il contenuto è troppo piccolo da leggere o forse perché il sito è lento a causa di animazioni costose.

Vediamo quindi come ridurre gli errori tra browser risultanti da HTML/CSS.

## Prima di tutto: risolvere i problemi generali

Nel [primo articolo di questa serie](/it/docs/Learn_web_development/Extensions/Testing/Introduction#testingdiscovery) è stato detto che una buona strategia iniziale consiste nel testare in un paio di browser moderni su desktop/mobile, per assicurarsi che il codice funzioni in generale, prima di concentrarsi sui problemi tra browser.

Negli articoli [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML) e [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS), sono state fornite indicazioni molto basilari sul debugging di HTML/CSS: se non si ha familiarità con le basi, è opportuno studiare questi articoli prima di proseguire.

In sostanza, si tratta di verificare che il codice HTML e CSS sia ben formato e non contenga errori di sintassi.

> [!NOTE]
> Un problema comune con CSS e HTML si presenta quando diverse regole CSS iniziano a entrare in conflitto tra loro. Questo può essere particolarmente problematico quando si usa codice di terze parti. Ad esempio, si potrebbe usare un framework CSS e scoprire che uno dei nomi di classe utilizzati entra in conflitto con uno già impiegato per uno scopo diverso. Oppure, si potrebbe scoprire che HTML generato da qualche tipo di API di terze parti, ad esempio per generare banner pubblicitari, include un nome di classe o un ID già usato per uno scopo diverso. Per assicurarsi che ciò non accada, occorre prima studiare gli strumenti utilizzati e progettare il codice attorno a essi. Vale anche la pena applicare uno "spazio dei nomi" al CSS: ad esempio, se è presente un widget, assicurarsi che abbia una classe distinta, quindi iniziare i selettori che selezionano gli elementi interni al widget con questa classe, così i conflitti saranno meno probabili. Ad esempio `.audio-player ul a`.

### Validazione

Per HTML, la validazione consiste nell'assicurarsi che tutti i tag siano chiusi e annidati correttamente, che venga usato un doctype e che i tag siano utilizzati per il loro scopo corretto. Una buona strategia consiste nel validare regolarmente il codice. Un servizio in grado di farlo è il [Markup Validation Service](https://validator.w3.org/) del W3C, che permette di indicare il proprio codice e restituisce un elenco di errori:

![La home page del validatore HTML](validator.png)

CSS presenta una situazione simile: è necessario verificare che i nomi delle proprietà siano scritti correttamente, che i valori delle proprietà siano scritti correttamente e validi per le proprietà su cui vengono utilizzati, che non manchino parentesi graffe e così via. Anche il W3C mette a disposizione un [CSS Validator](https://jigsaw.w3.org/css-validator/) per questo scopo.

### Linter

Un'altra buona opzione è scegliere una cosiddetta applicazione linter, che non solo segnala gli errori, ma può anche evidenziare avvisi riguardanti cattive pratiche nel CSS e altri aspetti. In genere i linter possono essere personalizzati per essere più rigidi o più permissivi nella segnalazione di errori/avvisi.

Esistono molte applicazioni linter online, come [Dirty Markup](https://www.10bestdesign.com/dirtymarkup/) per HTML, CSS e JavaScript. Queste permettono di incollare il codice in una finestra e segnalano gli eventuali errori con delle croci; passando il puntatore su di esse viene visualizzato un messaggio di errore che informa sul problema. Dirty Markup permette inoltre di correggere il markup usando il pulsante _Clean_.

![L'applicazione Dirty Markup mostra il messaggio "Unexpected character in unquoted attribute" sul seguente markup HTML non corretto: <div id=combinators">](dirty-markup.png)

Tuttavia, non è molto comodo dover copiare e incollare il codice in una pagina web per verificarne più volte la validità. Ciò che serve davvero è un linter che si integri nel flusso di lavoro standard con il minimo sforzo.

Molti editor di codice dispongono di plugin linter. Ad esempio:

- [SublimeLinter](https://www.sublimelinter.com/) per Sublime Text
- [Linter per Notepad++](https://sourceforge.net/projects/notepad-linter/)
- [Linter per VS Code](https://marketplace.visualstudio.com/search?target=vscode&category=Linters&sortBy=Installs)

### Strumenti per sviluppatori del browser

Gli strumenti per sviluppatori integrati nella maggior parte dei browser includono anche strumenti utili per individuare gli errori, principalmente nel CSS.

> [!NOTE]
> Gli errori HTML tendono a non apparire facilmente negli strumenti per sviluppatori, perché il browser tenterà di correggere automaticamente il markup malformato; il validatore W3C è il modo migliore per trovare gli errori HTML. Vedere [Validazione](#validazione) sopra.

Ad esempio, in Firefox l'ispettore CSS mostrerà barrate le dichiarazioni CSS che non vengono applicate, con un triangolo di avviso. Passando il puntatore sul triangolo di avviso viene visualizzato un messaggio di errore descrittivo:

![Gli strumenti per sviluppatori barrano il CSS non valido e aggiungono un'icona di avviso su cui è possibile passare il puntatore](css-message-devtools.png)

Gli strumenti per sviluppatori degli altri browser hanno funzionalità simili.

## Problemi comuni tra browser

Passiamo ora ad analizzare alcuni dei problemi più comuni di HTML e CSS tra browser. Le aree principali esaminate saranno la mancanza di supporto per funzionalità moderne e i problemi di layout.

### Browser che non supportano funzionalità moderne

Questo è un problema comune, soprattutto quando occorre supportare browser vecchi o quando si utilizzano funzionalità implementate in alcuni browser ma non ancora in tutti. In generale, la maggior parte delle funzionalità fondamentali di HTML e CSS, come elementi HTML di base, colori CSS di base e stilizzazione del testo, funziona in tutti i browser che si vorranno supportare; emergono più problemi quando si iniziano a usare HTML, CSS e API più recenti. MDN mostra dati sulla compatibilità del browser per ogni funzionalità documentata; ad esempio, vedere la [tabella di supporto dei browser per la pseudo-classe `:has()`](/it/docs/Web/CSS/Reference/Selectors/:has#browser_compatibility).

Dopo aver identificato un elenco di tecnologie da usare che non sono supportate universalmente, è una buona idea cercare in quali browser sono supportate e quali tecniche correlate risultano utili. Vedere [Trovare aiuto](#trovare_aiuto) più avanti.

### Comportamento di fallback HTML

Alcuni problemi possono essere risolti semplicemente sfruttando il modo naturale in cui funzionano HTML/CSS.

Gli elementi HTML non riconosciuti vengono trattati dal browser come elementi inline anonimi, ovvero di fatto elementi inline senza valore semantico, simili agli elementi {{htmlelement("span")}}. È comunque possibile fare riferimento a essi tramite i loro nomi e applicare loro stili CSS, ad esempio; occorre solo assicurarsi che si comportino come desiderato. Applicare loro stili proprio come a qualsiasi altro elemento, incluso impostare la proprietà `display` a qualcosa di diverso da `inline`, se necessario.

Elementi più complessi come HTML [`<video>`](/it/docs/Web/HTML/Reference/Elements/video), [`<audio>`](/it/docs/Web/HTML/Reference/Elements/audio), [`<picture>`](/it/docs/Web/HTML/Reference/Elements/picture), [`<object>`](/it/docs/Web/HTML/Reference/Elements/object) e [`<canvas>`](/it/docs/Web/HTML/Reference/Elements/canvas), oltre ad altre funzionalità, dispongono di meccanismi naturali per aggiungere fallback nel caso in cui le risorse collegate non siano supportate. È possibile aggiungere contenuto di fallback tra i tag di apertura e chiusura e i browser che non supportano l'elemento ignoreranno di fatto l'elemento esterno ed eseguiranno il contenuto annidato.

Ad esempio:

```html
<video id="video" controls preload="metadata" poster="img/poster.jpg">
  <source
    src="video/tears-of-steel-battle-clip-medium.webm"
    type="video/webm" />
  <!-- Offer download -->
  <p>
    Your browser does not support WebM video; here is a link to
    <a href="video/tears-of-steel-battle-clip-medium.mp4"
      >view the video directly</a
    >
  </p>
</video>
```

Questo esempio include un semplice link che consente di scaricare il video se nemmeno il lettore video HTML funziona, così almeno l'utente può comunque accedere al video.

Un altro esempio sono gli elementi dei moduli. Quando sono stati introdotti nuovi tipi [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) per inserire informazioni specifiche nei moduli, come orari, date, colori, numeri e così via, se un browser non supportava la nuova funzionalità, usava il valore predefinito `type="text"`. Sono stati aggiunti tipi di input molto utili, in particolare sulle piattaforme mobili, dove fornire un modo semplice per inserire dati è molto importante per l'esperienza utente. Le piattaforme forniscono diversi widget dell'interfaccia utente in base al tipo di input, come un widget calendario per inserire le date. Se un browser non supporta un tipo di input, l'utente può comunque inserire i dati richiesti.

L'esempio seguente mostra input per data e ora:

```html live-sample___form-test
<form>
  <div>
    <label for="date">Enter a date:</label>
    <input id="date" type="date" />
  </div>
  <div>
    <label for="time">Enter a time:</label>
    <input id="time" type="time" />
  </div>
</form>
```

```css hidden live-sample___form-test
div {
  margin-bottom: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
}

input {
  flex: 2;
}

label {
  flex: 1;
  text-align: right;
}

body {
  width: 400px;
  margin: 0 auto;
}
```

L'output di questo codice è il seguente:

{{EmbedLiveSample("form-test", '100%', 100)}}

È possibile premere il pulsante **Play** per aprire l'esempio in MDN Playground e modificare il codice sorgente.

Visualizzando l'esempio, si potranno osservare le funzionalità dell'interfaccia utente in azione durante l'inserimento dei dati. Sui dispositivi con tastiere dinamiche verranno visualizzati tastierini specifici per tipo. In un browser che non offre supporto, gli input useranno semplicemente input di testo normali, consentendo comunque all'utente di inserire le informazioni corrette.

### Comportamento di fallback CSS

CSS è probabilmente migliore di HTML nella gestione dei fallback. Se un browser incontra una dichiarazione o una regola che non comprende, la salta semplicemente del tutto senza applicarla né generare un errore. Questo può essere frustrante per sviluppatori e utenti se un tale errore arriva nel codice di produzione, ma almeno significa che l'intero sito non smette di funzionare a causa di un singolo errore e, se usato in modo intelligente, può essere sfruttato a proprio vantaggio.

Vediamo un esempio: una semplice casella stilizzata con CSS, a cui viene applicato uno stile fornito da varie funzionalità CSS:

```html hidden live-sample___blingy-button
<button>Press me</button>
```

```css hidden live-sample___blingy-button
html {
  font-family: sans-serif;
  height: 100%;
}

button {
  width: 150px;
  margin: auto;
  line-height: 2;
  font-size: 1.1rem;
  text-align: center;
  color: white;
  text-shadow: 1px 1px 1px black;
  border-radius: 20px / 15px;
  border: none;
  cursor: pointer;

  background-color: red;
  background-color: rgb(255 0 0 / 90%);
  box-shadow:
    inset 3px 3px 3px rgb(255 255 255 / 40%),
    inset -3px -3px 3px rgb(0 0 0 / 40%);
}

button:hover,
button:focus {
  background-color: rgb(255 0 0 / 50%);
}

button:active {
  box-shadow:
    inset 3px 3px 3px rgb(0 0 0 / 40%),
    inset -3px -3px 3px rgb(255 255 255 / 40%);
}

body {
  height: inherit;
  display: flex;
  align-items: center;
}
```

{{EmbedLiveSample("blingy-button", "100%", 60)}}

È possibile premere il pulsante **Play** per aprire l'esempio in MDN Playground e sperimentare con il codice sorgente.

Al pulsante sono applicate diverse dichiarazioni, ma quelle di maggiore interesse sono le seguenti:

```css
button {
  /* … */

  background-color: red;
  background-color: rgb(255 0 0 / 90%);
  box-shadow:
    inset 3px 3px 3px rgb(255 255 255 / 40%),
    inset -3px -3px 3px rgb(0 0 0 / 40%);
}

button:hover,
button:focus {
  background-color: rgb(255 0 0 / 50%);
}

button:active {
  box-shadow:
    inset 3px 3px 3px rgb(0 0 0 / 40%),
    inset -3px -3px 3px rgb(255 255 255 / 40%);
}
```

Qui viene fornito un {{cssxref("background-color")}} [RGB](/it/docs/Web/CSS/Reference/Values/color_value/rgb) che cambia opacità al passaggio del puntatore per suggerire all'utente che il pulsante è interattivo, oltre ad alcune ombre {{cssxref("box-shadow")}} interne semitrasparenti per dare al pulsante un po' di texture e profondità. Sebbene siano ora completamente supportati, i colori RGB e le ombre delle caselle non sono sempre esistiti; sono disponibili a partire da IE9. I browser che non supportavano i colori RGB ignoravano la dichiarazione, il che significava che nei browser vecchi lo sfondo non veniva affatto visualizzato e il testo risultava illeggibile: tutt'altro che ideale.

![Pulsante a forma di pillola difficile da vedere, con testo bianco su sfondo quasi bianco](unreadable-button.png)

Per risolvere il problema, è stata aggiunta una dichiarazione iniziale `background-color`, che specifica semplicemente la parola chiave del colore `red`: questa è supportata anche da browser molto vecchi e agisce come fallback se le funzionalità moderne più elaborate non funzionano. Un browser che visita questa pagina applica prima il primo valore di `background-color`; quando raggiunge la seconda dichiarazione `background-color`, sovrascrive il valore iniziale con questo valore se supporta i colori RGB. In caso contrario, ignora l'intera dichiarazione e prosegue.

> [!NOTE]
> Lo stesso vale per altre funzionalità CSS come le [media query](/it/docs/Web/CSS/Guides/Media_queries/Using), i blocchi {{cssxref("@font-face")}} e {{cssxref("@supports")}}: se non sono supportati, il browser li ignora semplicemente.

### Supporto dei selettori

Naturalmente, nessuna funzionalità CSS verrà applicata se non vengono utilizzati i [selettori](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) corretti per selezionare l'elemento da stilizzare.

In un elenco di selettori separati da virgole, se un selettore viene semplicemente scritto in modo errato, potrebbe non corrispondere a nessun elemento. Se invece un selettore non è valido, viene ignorato l'elenco **intero** dei selettori, insieme all'intero blocco di stile. Per questo motivo, includere una pseudo-classe o pseudo-elemento con prefisso `:-moz-` solo in un [elenco di selettori permissivo](/it/docs/Web/CSS/Reference/Selectors/Selector_list#forgiving_selector_list), come `:where(::-moz-thumb)`. Non includere una pseudo-classe o pseudo-elemento con prefisso `:-moz-` in un gruppo di selettori separati da virgole al di fuori di un elenco di selettori permissivo {{cssxref(":is()")}} o {{cssxref(":where()")}}, poiché tutti i browser diversi da Firefox ignoreranno l'intero blocco. Si noti che sia `:is()` sia `:where()` possono essere passati come parametri in altri elenchi di selettori, inclusi {{cssxref(":has()")}} e {{cssxref(":not()")}}.

È utile ispezionare l'elemento che si sta cercando di stilizzare usando gli strumenti per sviluppatori del browser, quindi osservare la traccia breadcrumb dell'albero DOM che gli ispettori DOM tendono a fornire, per verificare se il selettore ha senso rispetto a essa.

Ad esempio, negli strumenti per sviluppatori di Firefox si ottiene questo tipo di output nella parte inferiore dell'ispettore DOM:

![La breadcrumb degli elementi è html > body > form > div.form > input#date](dom-breadcrumb-trail.png)

Se, ad esempio, si cercasse di utilizzare questo selettore, sarebbe possibile vedere che non selezionerebbe l'elemento input come desiderato:

```css
form > #date {
  /* … */
}
```

(L'input del modulo `date` non è un figlio diretto di `<form>`; sarebbe meglio usare un selettore discendente generale invece di un selettore figlio).

### Gestione dei prefissi CSS

Un altro insieme di problemi deriva dai prefissi CSS: sono un meccanismo originariamente utilizzato per consentire ai fornitori di browser di implementare una propria versione di una funzionalità CSS, o JavaScript, mentre la tecnologia si trova in una fase sperimentale, in modo da poterla sperimentare e perfezionare senza entrare in conflitto con le implementazioni di altri browser o con le implementazioni finali senza prefisso.

Ad esempio, Firefox usa `-moz-` e Chrome/Edge/Opera/Safari usano `-webkit-`. Altri prefissi che si possono incontrare nel codice vecchio e che possono essere rimossi senza rischi includono `-ms-`, usato da Internet Explorer e dalle prime versioni di Edge, e `-o`, usato nelle versioni originali di Opera.

Le funzionalità con prefisso non sono mai state pensate per essere utilizzate nei siti web di produzione: sono soggette a modifiche o rimozione senza preavviso, possono causare problemi di prestazioni nelle vecchie versioni dei browser che le richiedono e sono state causa di problemi tra browser. Ciò costituisce un problema particolare, ad esempio, quando gli sviluppatori decidono di usare solo la versione `-webkit-` di una proprietà, implicando che il sito non funzionerà in altri browser. Questo è accaduto così spesso che altri fornitori di browser hanno implementato versioni con prefisso `-webkit-` di diverse proprietà CSS. Sebbene i browser supportino ancora alcuni nomi di proprietà, valori di proprietà e pseudo-classi con prefisso, ora le funzionalità sperimentali vengono poste dietro flag affinché gli sviluppatori web possano testarle durante lo sviluppo.

Se si utilizza un prefisso, assicurarsi che sia necessario e che la proprietà sia una delle poche funzionalità con prefisso ancora esistenti. È possibile verificare quali browser richiedono prefissi nelle pagine di riferimento MDN e su siti come [caniuse.com](https://caniuse.com/). In caso di dubbi, è anche possibile scoprirlo effettuando test direttamente nei browser. Includere la versione standard senza prefisso dopo la dichiarazione di stile con prefisso; sarà ignorata se non supportata e usata quando supportata.

```css
.masked {
  -webkit-mask-image: url("MDN.svg");
  mask-image: url("MDN.svg");
  -webkit-mask-size: 50%;
  mask-size: 50%;
}
```

Provare questo semplice esempio:

1. Usare questa pagina o un altro sito che abbia un titolo ben visibile o un altro elemento a livello di blocco.
2. Fare clic destro/Cmd + clic sull'elemento in questione e scegliere Ispeziona/Ispeziona elemento, o qualunque sia l'opzione nel browser: questo dovrebbe aprire gli strumenti per sviluppatori nel browser, con l'elemento evidenziato nell'ispettore DOM.
3. Cercare una funzionalità che possa essere usata per selezionare quell'elemento. Ad esempio, al momento della stesura, questa pagina su MDN ha un logo con ID `mdn-docs-logo`.
4. Memorizzare un riferimento a questo elemento in una variabile, ad esempio:

   ```js
   const test = document.getElementById("mdn-docs-logo");
   ```

5. Ora provare a impostare un nuovo valore per la proprietà CSS di interesse su quell'elemento; è possibile farlo usando la proprietà [style](/it/docs/Web/API/HTMLElement/style) dell'elemento. Ad esempio, provare a digitare quanto segue nella console JavaScript:

   ```js
   test.style.transform = "rotate(90deg)";
   ```

Quando si inizia a digitare la rappresentazione del nome della proprietà dopo il secondo punto, si noti che in JavaScript i nomi delle proprietà CSS sono scritti in {{Glossary("camel_case", "lower camel case")}}, non in {{Glossary("kebab_case", "kebab-case")}}, la console JavaScript dovrebbe iniziare a completare automaticamente i nomi delle proprietà esistenti nel browser e corrispondenti a quanto scritto finora. Questo è utile per scoprire quali proprietà sono implementate in quel browser.

Se occorre includere funzionalità moderne, testare il supporto delle funzionalità usando {{cssxref("@supports")}}, che consente di implementare test nativi di feature detection e annidare la funzionalità con prefisso o la nuova funzionalità all'interno del blocco `@supports`.

### Problemi di responsive design

Il responsive design è la pratica di creare layout web che cambiano per adattarsi a diversi fattori di forma dei dispositivi, ad esempio diverse larghezze dello schermo, orientamenti, verticale o orizzontale, oppure risoluzioni. Un layout desktop, ad esempio, apparirà terribile se visualizzato su un dispositivo mobile; è quindi necessario fornire un layout mobile adeguato usando le [media query](/it/docs/Web/CSS/Guides/Media_queries) e assicurarsi che venga applicato correttamente usando [viewport](/it/docs/Web/HTML/Reference/Elements/meta/name/viewport). Una descrizione dettagliata di queste pratiche è disponibile nel [tutorial sul responsive design](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design).

Anche la risoluzione è un problema importante: ad esempio, i dispositivi mobili hanno meno probabilità di richiedere immagini grandi e pesanti rispetto ai computer desktop e hanno maggiori probabilità di disporre di connessioni Internet più lente e forse anche di piani dati costosi, che rendono lo spreco di larghezza di banda un problema maggiore. Inoltre, dispositivi diversi possono avere una vasta gamma di risoluzioni, il che significa che immagini più piccole potrebbero apparire pixelate. Esistono diverse tecniche che consentono di aggirare tali problemi, dalle [media query](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design#media_queries) a tecniche più complesse per [immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images#resolution_switching_different_sizes), inclusi {{HTMLElement('picture')}} e gli attributi [`srcset`](/it/docs/Web/HTML/Reference/Elements/img#srcset) e [`sizes`](/it/docs/Web/HTML/Reference/Elements/img#sizes) dell'elemento {{HTMLElement('img')}}.

## Trovare aiuto

Esistono molti altri problemi che si incontreranno con HTML e CSS, rendendo preziosa la conoscenza di come trovare risposte online.

Tra le migliori fonti di informazioni di supporto vi sono Mozilla Developer Network, cioè il sito in cui ci si trova ora, [stackoverflow.com](https://stackoverflow.com/) e [caniuse.com](https://caniuse.com/).

Per utilizzare Mozilla Developer Network (MDN), la maggior parte delle persone esegue una ricerca su un motore di ricerca della tecnologia su cui vuole trovare informazioni, aggiungendo il termine "mdn", ad esempio "mdn HTML video". MDN contiene diversi tipi utili di contenuti:

- Materiale di riferimento con informazioni sul supporto dei browser per le tecnologie web lato client, ad esempio la [pagina di riferimento di `<video>`](/it/docs/Web/HTML/Reference/Elements/video).
- Altro materiale di riferimento di supporto, ad esempio la [Guida ai tipi e ai formati multimediali sul web](/it/docs/Web/Media/Guides/Formats).
- Tutorial utili che risolvono problemi specifici, ad esempio [Creare un lettore video tra browser](/it/docs/Web/Media/Guides/Audio_and_video_delivery/cross_browser_video_player).

[caniuse.com](https://caniuse.com/) fornisce informazioni sul supporto, insieme ad alcuni utili link a risorse esterne. Ad esempio, vedere <https://caniuse.com/#search=video>; basta inserire la funzionalità cercata nella casella di testo.

[stackoverflow.com](https://stackoverflow.com/) (SO) è un sito forum in cui è possibile fare domande, ricevere soluzioni da altri sviluppatori, cercare post precedenti e aiutare altri sviluppatori. Prima di pubblicare una nuova domanda, è consigliabile cercare se esiste già una risposta. Ad esempio, è stata cercata su SO la stringa "disabling autofocus on HTML dialog" e si è trovata rapidamente la domanda [Disable showModal auto-focusing using HTML attributes](https://stackoverflow.com/questions/63267581/disable-showmodal-auto-focusing-using-html-attributes).

Oltre a questo, provare a cercare una risposta al problema nel proprio motore di ricerca preferito. È spesso utile cercare messaggi di errore specifici, se disponibili: è probabile che altri sviluppatori abbiano avuto gli stessi problemi.

## Riepilogo

Ora dovrebbero essere noti i principali tipi di problemi HTML e CSS tra browser che si incontreranno nello sviluppo web, nonché come affrontarli e risolverli.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Testing_strategies","Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing")}}
