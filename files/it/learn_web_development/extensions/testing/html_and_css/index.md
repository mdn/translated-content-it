---
title: Gestire i problemi comuni di HTML e CSS
short-title: Problemi comuni di HTML e CSS
slug: Learn_web_development/Extensions/Testing/HTML_and_CSS
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Testing_strategies", "Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing")}}

Con il quadro impostato, ora esamineremo specificamente i problemi comuni di compatibilità tra browser che si possono incontrare nel codice HTML e CSS, e quali strumenti possono essere utilizzati per prevenire o correggere tali problemi. Questo include la verifica del codice, la gestione dei prefissi CSS, l'uso degli strumenti di sviluppo del browser per individuare problemi, l'uso dei polyfill per aggiungere supporto nei browser, affrontare i problemi di design responsive e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi core <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; un'idea
        dei principi di
        <a
          href="/it/docs/Learn_web_development/Extensions/Testing/Introduction"
          >test cross browser</a
        > a un alto livello.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di diagnosticare i problemi comuni di compatibilità tra browser di HTML e CSS e utilizzare i giusti strumenti e tecniche per risolverli.
      </td>
    </tr>
  </tbody>
</table>

## I problemi con HTML e CSS

Alcuni problemi con HTML e CSS derivano dal fatto che entrambi i linguaggi sono piuttosto semplici e spesso gli sviluppatori non li prendono sul serio, in termini di assicurarsi che il codice sia ben realizzato, efficiente e semanticamente descriva lo scopo delle funzionalità sulla pagina. Nei casi peggiori, JavaScript viene utilizzato per generare tutto il contenuto e lo stile della pagina web, il che rende le pagine inaccessibili e meno performanti (generare elementi DOM è costoso). In altri casi, funzionalità nascenti non sono supportate in modo coerente tra i browser, il che può far sì che alcune funzionalità e stili non funzionino per alcuni utenti. Anche i problemi di design responsive sono comuni — un sito che ha un aspetto gradevole in un browser desktop potrebbe offrire un'esperienza terribile su un dispositivo mobile, perché il contenuto è troppo piccolo per essere letto, o forse il sito è lento a causa di animazioni dispendiose.

Procediamo a vedere come possiamo ridurre gli errori di compatibilità tra browser che derivano da HTML/CSS.

## Prima cosa: correggere i problemi generali

Abbiamo detto nel [primo articolo di questa serie](/it/docs/Learn_web_development/Extensions/Testing/Introduction#testingdiscovery) che una buona strategia iniziale è testare in un paio di browser moderni su desktop/mobile, per assicurarsi che il codice funzioni in generale, prima di concentrarsi sui problemi di compatibilità tra browser.

Nei nostri articoli su [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML) e [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS), abbiamo fornito alcune indicazioni di base su come correggere gli errori di HTML/CSS — se non si ha familiarità con le basi, è sicuramente consigliabile studiare questi articoli prima di proseguire.

Fondamentalmente, si tratta di verificare che il codice HTML e CSS sia ben formato e non contenga errori di sintassi.

> [!NOTE]
> Un problema comune con CSS e HTML si ha quando diverse regole CSS iniziano a entrare in conflitto tra loro. Questo può essere particolarmente problematico quando si utilizza codice di terzi. Ad esempio, si potrebbe usare un framework CSS e scoprire che uno dei nomi di classe che utilizza confligge con uno che lei ha già utilizzato per un altro scopo. Oppure si potrebbe scoprire che l'HTML generato da qualche tipo di API di terzi (per esempio, per generare banner pubblicitari) include un nome di classe o un ID che lei sta già utilizzando per un altro scopo. Per evitare che ciò accada, è necessario effettuare ricerche sugli strumenti che si stanno utilizzando e progettare il proprio codice attorno a essi. Vale anche la pena "namespacing" il CSS, ad esempio, se ha un widget, assicurarsi che abbia una classe distinta e poi iniziare i selettori che selezionano elementi all'interno del widget con questa classe, così i conflitti sono meno probabili. Ad esempio, `.audio-player ul a`.

### Validazione

Per l'HTML, la validazione implica assicurarsi che tutti i tag siano chiusi e nidificati correttamente, che si stia usando un doctype e che si stiano utilizzando i tag per il loro scopo corretto. Una buona strategia è quella di validare regolarmente il codice. Un servizio che può fare questo è il [Markup Validation Service](https://validator.w3.org/) del W3C, che permette di puntare al proprio codice e restituisce un elenco di errori:

![La homepage del validatore HTML](validator.png)

Il CSS segue una storia simile — è necessario controllare che i nomi delle proprietà siano scritti correttamente, i valori delle proprietà siano scritti correttamente e siano validi per le proprietà su cui sono usati, non manchino parentesi graffe e così via. Il W3C ha anche un [CSS Validator](https://jigsaw.w3.org/css-validator/) disponibile a questo scopo.

### Linters

Un'altra buona opzione è utilizzare un'applicazione Linter, che non solo segnala gli errori, ma può anche evidenziare avvisi su pratiche scorrette nel CSS e altri punti. I Linters possono generalmente essere personalizzati per essere più rigidi o più rilassati nella segnalazione di errori/avvisi.

Ci sono molte applicazioni linter online, come [Dirty Markup](https://www.10bestdesign.com/dirtymarkup/) per HTML, CSS e JavaScript. Queste permettono di incollare il proprio codice in una finestra e segnaleranno eventuali errori con delle croci, sulle quali si può passare il mouse per ottenere un messaggio di errore che informa qual è il problema. Dirty Markup consente anche di apportare correzioni al proprio markup usando il pulsante _Clean_.

![L'applicazione Dirty Markup mostra il messaggio "Unexpected character in unquoted attribute" su un markup HTML errato: <div id=combinators">](dirty-markup.png)

Tuttavia, non è molto conveniente dover copiare e incollare il proprio codice in una pagina web per controllarne la validità più volte. Ciò che lei desidera davvero è un linter che si integri nel suo flusso di lavoro standard con il minimo fastidio possibile.

Molti editor di codice hanno plugin per linter. Ad esempio, veda:

- [SublimeLinter](https://www.sublimelinter.com/) per Sublime Text
- [Notepad++ linter](https://sourceforge.net/projects/notepad-linter/)
- [vs Code linters](https://marketplace.visualstudio.com/search?target=vscode&category=Linters&sortBy=Installs)

### Strumenti di sviluppo del browser

Gli strumenti di sviluppo integrati nella maggior parte dei browser presentano anche strumenti utili per individuare errori, principalmente per CSS.

> [!NOTE]
> Gli errori HTML tendono a non apparire facilmente negli strumenti di sviluppo, poiché il browser tenterà di correggere automaticamente il markup malformato; il validatore del W3C è il modo migliore per trovare errori HTML — veda [Validation](#validazione) sopra.

Ad esempio, in Firefox l'ispettore CSS mostra le dichiarazioni CSS che non vengono applicate barrate, con un triangolo di avviso. Passando con il mouse sul triangolo di avviso verrà fornito un messaggio di errore descrittivo:

![Gli strumenti di sviluppo barcano il CSS non valido e aggiungono un'icona di avviso su cui passare con il mouse](css-message-devtools.png)

Altri strumenti di sviluppo dei browser hanno caratteristiche simili.

## Problemi comuni di compatibilità tra browser

Ora passiamo ad analizzare alcuni dei problemi più comuni di incompatibilità tra browser in HTML e CSS. Le principali aree che analizzeremo sono la mancanza di supporto per le funzionalità moderne e i problemi di layout.

### Browser che non supportano funzionalità moderne

Questo è un problema comune, specialmente quando è necessario supportare vecchi browser o si stanno utilizzando funzionalità implementate in alcuni browser ma non ancora in tutti. In generale, la maggior parte delle funzionalità core di HTML e CSS (come gli elementi base di HTML, i colori di base di CSS e lo stile del testo) funziona tra tutti i browser che si desidera supportare; più problemi vengono riscontrati quando si inizia a voler utilizzare nuovi HTML, CSS e API. MDN mostra i dati di compatibilità dei browser per ciascuna funzionalità documentata; per esempio, veda la [tabella di supporto del browser per la pseudo-classe `:has()`](/it/docs/Web/CSS/:has#browser_compatibility).

Una volta identificata una lista di tecnologie che si prevede di utilizzare che non sono supportate universalmente, è una buona idea cercare in quali browser sono supportate e quali tecniche correlate sono utili. Veda [Finding help](#trovare_aiuto) di seguito.

### Comportamento di fallback di HTML

Alcuni problemi possono essere risolti semplicemente sfruttando il modo naturale in cui HTML/CSS funzionano.

Gli elementi HTML non riconosciuti sono trattati dal browser come elementi anonimi in linea (in effetti elementi in linea senza valore semantico, simili agli elementi {{htmlelement("span")}}). È ancora possibile riferirsi ad essi per nome e stilizzarli con CSS, per esempio — è sufficiente assicurarsi che si comportino come desiderato. Stilizzarli come si farebbe con qualsiasi altro elemento, includendo l'impostazione della proprietà `display` a qualcosa di diverso da `inline`, se necessario.

Elementi più complessi come [`<video>`](/it/docs/Web/HTML/Reference/Elements/video), [`<audio>`](/it/docs/Web/HTML/Reference/Elements/audio), [`<picture>`](/it/docs/Web/HTML/Reference/Elements/picture), [`<object>`](/it/docs/Web/HTML/Reference/Elements/object) e [`<canvas>`](/it/docs/Web/HTML/Reference/Elements/canvas) HTML (e altre funzionalità oltre a queste) hanno meccanismi naturali per cui possono essere aggiunti fallback nel caso in cui le risorse collegate non siano supportate. È possibile aggiungere contenuto di fallback tra i tag di apertura e chiusura e i browser che non supportano la funzionalità ignoreranno in pratica l'elemento esterno e eseguiranno il contenuto annidato.

Per esempio:

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

Questo esempio include un semplice link che permette di scaricare il video se anche il lettore video HTML non funziona, così almeno l'utente può accedere al video.

Un altro esempio sono gli elementi del modulo. Quando sono stati introdotti nuovi tipi di [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) per inserire informazioni specifiche nei moduli, come orari, date, colori, numeri, ecc., se un browser non supportava la nuova funzionalità, il browser utilizzava il valore predefinito `type="text"`. Sono stati aggiunti tipi di input che sono molto utili, particolarmente su piattaforme mobili, dove fornire un metodo di inserimento dati senza problemi è molto importante per l'esperienza utente. Le piattaforme forniscono diversi widget UI a seconda del tipo di input, come un widget calendario per inserire le date. Qualora un browser non supporti un tipo di input, l'utente può comunque inserire i dati richiesti.

Il seguente esempio mostra input di data e ora:

```html
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

Il risultato di questo codice è il seguente:

{{EmbedGHLiveSample("learning-area/tools-testing/cross-browser-testing/html-css/forms-test", '100%', 150)}}

> [!NOTE]
> È possibile vedere anche questo esempio in esecuzione come [forms-test.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/html-css/forms-test.html) su GitHub (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/html-css/forms-test.html)).

Se visualizza l'esempio, vedrà le funzionalità dell'interfaccia utente in azione mentre prova a inserire i dati. Su dispositivi con tastiere dinamiche, verranno visualizzate tastiere specifiche per tipo. Su un browser non supportato, gli input verranno semplicemente impostati su input di testo normali, il che significa che l'utente può comunque inserire le informazioni corrette.

### Comportamento di fallback di CSS

Il CSS è indubbiamente migliore dei fallback dell’HTML. Se un browser incontra una dichiarazione o una regola che non capisce, la salta completamente senza applicarla né generare un errore. Questo potrebbe essere frustrante per lei e i suoi utenti se un errore del genere si infiltra nel codice di produzione, ma almeno significa che l'intero sito non crolla a causa di un errore, e se usato con intelligenza, può essere sfruttato a proprio vantaggio.

Diamo un'occhiata a un esempio — un semplice pulsante stilizzato con CSS, che ha alcune stilizzazioni fornite da vari funzionalità CSS:

![Un pulsante rosso con angoli arrotondati, un'ombra interna e un'ombra esterna](blingy-button.png)

> [!NOTE]
> Può vedere anche questo esempio in esecuzione su GitHub come [button-with-fallback.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/html-css/button-with-fallback.html) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/html-css/button-with-fallback.html)).

Il pulsante ha un certo numero di dichiarazioni che lo stilizzano, ma le due che ci interessano di più sono le seguenti:

```css
button {
  /* … */

  background-color: #ff0000;
  background-color: rgb(255 0 0 / 100%);
  box-shadow:
    inset 1px 1px 3px rgb(255 255 255 / 40%),
    inset -1px -1px 3px rgb(0 0 0 / 40%);
}

button:hover {
  background-color: rgb(255 0 0 / 50%);
}

button:active {
  box-shadow:
    inset 1px 1px 3px rgb(0 0 0 / 40%),
    inset -1px -1px 3px rgb(255 255 255 / 40%);
}
```

Qui stiamo fornendo un {{cssxref("background-color")}} [RGB](/it/docs/Web/CSS/color_value/rgb) che cambia opacità al passaggio del mouse per dare all'utente un suggerimento che il pulsante è interattivo, e alcune ombre semi-trasparenti {{cssxref("box-shadow")}} interne per dare al pulsante un po' di texture e profondità. Anche se ormai pienamente supportate, i colori RGB e le ombre non sono sempre esistiti; sono stati introdotti a partire da IE9. I browser che non supportavano i colori RGB ignoravano la dichiarazione, il che significa che nei vecchi browser il fondo non appariva affatto quindi il testo era illeggibile, davvero pessimo!

![Pulsante difficile da vedere con testo bianco su un fondo quasi bianco](unreadable-button.png)

Per risolvere questo problema, abbiamo aggiunto una seconda dichiarazione `background-color`, che specifica solo un colore esadecimale — questo è supportato indietro nei browser davvero vecchi e funge da fallback se le funzionalità moderne e lucide non funzionano. Quello che succede è che un browser che visita questa pagina applica prima il primo valore `background-color`; quando arriva alla seconda dichiarazione `background-color`, sovrascriverà il valore iniziale con questo se supporta i colori RGB. In caso contrario, ignorerà semplicemente l'intera dichiarazione e passerà oltre.

> [!NOTE]
> Lo stesso vale per altre funzionalità CSS come le [media queries](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries), i blocchi [`@font-face`](/it/docs/Web/CSS/@font-face) e [`@supports`](/it/docs/Web/CSS/@supports) — se non sono supportati, il browser li ignora semplicemente.

### Supporto del selettore

Ovviamente, nessuna funzionalità CSS avrà effetto se non usa i giusti [selettori](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) per selezionare l'elemento che desidera stilizzare!

In una lista separata da virgole di selettori, se scrive semplicemente un selettore in modo errato, potrebbe non corrispondere a nessun elemento. Se, tuttavia, un selettore non è valido, l'**intera** lista di selettori viene ignorata, insieme all'intero blocco di stile. Per questo motivo, includere solo una pseudo-classe o pseudo-elemento con prefisso `-moz-` in una [lista di selettori tollerante](/it/docs/Web/CSS/Selector_list#forgiving_selector_list), come `:where(::-moz-thumb)`. Non includere una pseudo-classe o pseudo-elemento con prefisso `-moz-` all’interno di un gruppo separato da virgole di selettori al di fuori di una lista di selettori tollerante come [`:is()`](/it/docs/Web/CSS/:is) o [`:where()`](/it/docs/Web/CSS/:where) poiché tutti i browser diversi da Firefox ignoreranno l'intero blocco. Si noti che sia `:is()` che `:where()` possono essere passati come parametri in altre liste di selettori, inclusi [`:has()`](/it/docs/Web/CSS/:has) e [`:not()`](/it/docs/Web/CSS/:not).

Troviamo che sia utile ispezionare l'elemento che sta cercando di stilizzare usando gli strumenti di sviluppo del suo browser, quindi osservare il percorso breadcrumb dell'albero DOM che tende a fornire gli ispettori DOM per vedere se il suo selettore ha senso rispetto a esso.

Ad esempio, negli strumenti di sviluppo di Firefox, ottiene questo tipo di output nella parte inferiore dell'ispettore DOM:

![Il breadcrumb degli elementi è html > body > form > div.form > input#date](dom-breadcrumb-trail.png)

Se, ad esempio, stesse cercando di utilizzare questo selettore, riuscirebbe a vedere che non selezionerebbe l'elemento di input come desiderato:

```css
form > #date {
  /* … */
}
```

(L'input di data non è un figlio diretto del `<form>`; sarebbe meglio usare un selettore discendente generale anziché un selettore figlio).

### Gestione dei prefissi CSS

Un altro set di problemi deriva dai prefissi CSS — questi sono un meccanismo originariamente utilizzato per consentire ai venditori di browser di implementare la propria versione di una funzionalità CSS (o JavaScript) mentre la tecnologia è in uno stato sperimentale, così possono sperimentarla e migliorarla senza conflitti con le implementazioni di altri browser, o con le implementazioni finali non prefissate.

Ad esempio, Firefox usa `-moz-` e Chrome/Edge/Opera/Safari usano `-webkit-`. Altri prefissi che può incontrare in codice vecchio e che possono essere rimossi in sicurezza includono `-ms-`, usati da Internet Explorer e le prime versioni di Edge, e `-o` che veniva utilizzato nelle versioni originali di Opera.

Le funzionalità con prefisso non dovevano mai essere utilizzate in siti web di produzione — sono soggette a modifica o rimozione senza preavviso, possono causare problemi di prestazioni in vecchie versioni di browser che le richiedono, e sono state la causa di problemi di compatibilità tra browser. Questo è stato particolarmente un problema, ad esempio, quando gli sviluppatori decidevano di utilizzare solo la versione `-webkit-` di una proprietà, il che implicava che il sito non funzionasse in altri browser. Questo in realtà è accaduto così di frequente che altri fornitori di browser hanno implementato versioni prefissate `-webkit-` di diverse proprietà CSS. Anche se i browser supportano ancora alcuni nomi di proprietà prefissati, valori di proprietà e pseudo-classi, ora le funzionalità sperimentali sono poste dietro flag in modo che gli sviluppatori web possano testarle durante lo sviluppo.

Se usa un prefisso, si assicuri che sia necessario; che la proprietà è una delle poche caratteristiche rimanenti con prefisso. Può controllare quali browser richiedono prefissi sulle pagine di riferimento su MDN e siti come [caniuse.com](https://caniuse.com/). Se è incerta, può anche scoprire facendo alcuni test direttamente nei browser. Includa la versione standard non prefissata dopo la dichiarazione di stile prefissata; verrà ignorata se non supportata e utilizzata quando supportata.

```css
.masked {
  -webkit-mask-image: url(MDN.svg);
  mask-image: url(MDN.svg);
  -webkit-mask-size: 50%;
  mask-size: 50%;
}
```

Provi questo semplice esempio:

1. Usi questa pagina, o un altro sito che abbia un titolo prominente o un altro elemento a livello di blocco.
2. Cliccare destro/Cmd + su sull'elemento in questione e scegliere Ispeziona/Ispeziona elemento (o qualunque sia l'opzione nel suo browser) — questo dovrebbe aprire gli strumenti di sviluppo nel suo browser, con l'elemento evidenziato nell'ispettore DOM.
3. Cerchi una funzionalità che può usare per selezionare quell'elemento. Ad esempio, al momento della scrittura, questa pagina su MDN ha un logo con un ID di `mdn-docs-logo`.
4. Memorizzi un riferimento a questo elemento in una variabile, per esempio:

   ```js
   const test = document.getElementById("mdn-docs-logo");
   ```

5. Ora provi a impostare un nuovo valore per la proprietà CSS che la interessa su quell'elemento; può farlo usando la proprietà [style](/it/docs/Web/API/HTMLElement/style) dell'elemento, ad esempio provi a digitare questi nel console JavaScript:

   ```js
   test.style.transform = "rotate(90deg)";
   ```

Appena inizia a digitare rappresentazioni di nomi di proprietà dopo il secondo punto (notare che in JavaScript, i nomi delle proprietà CSS sono scritti in {{Glossary("camel_case", "lower camel case")}}, non in {{Glossary("kebab_case", "kebab-case")}}), la console JavaScript dovrebbe iniziare a completare automaticamente i nomi delle proprietà che esistono nel browser e coincidono con quanto scritto finora. Questo è utile per scoprire quali proprietà sono implementate in quel browser.

Se deve includere funzionalità moderne, testi il supporto alla funzionalità utilizzando [`@supports`](/it/docs/Web/CSS/@supports), che le permette di implementare test di rilevamento delle funzionalità nativi, e inglobi il prefissato o la nuova funzionalità all'interno del blocco `@supports`.

### Problemi di design responsive

Il design responsive è la pratica di creare layout web che cambiano per adattarsi ai diversi formati dei dispositivi — ad esempio, diverse larghezze dello schermo, orientamenti (ritratto o paesaggio), o risoluzioni. Un layout desktop, ad esempio, avrà un aspetto terribile se visualizzato su un dispositivo mobile, quindi è necessario fornire un layout mobile adeguato utilizzando le [media queries](/it/docs/Web/CSS/CSS_media_queries) e assicurarsi che vengano applicate correttamente utilizzando il [viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element). Può trovare un resoconto dettagliato di tali pratiche nel [nostro tutorial sul design responsive](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design).

Anche la risoluzione è un grosso problema — ad esempio, i dispositivi mobili avranno meno probabilità di aver bisogno di immagini pesanti di grandi dimensioni rispetto ai computer desktop e saranno più propensi a avere connessioni internet più lente e forse anche piani dati costosi che rendono la larghezza di banda sprecata un problema più grande. Inoltre, diversi dispositivi possono avere una gamma di risoluzioni diverse, il che significa che le immagini più piccole potrebbero apparire pixelate. Ci sono una serie di tecniche che le permettono di aggirare tali problemi, dalle [media queries](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design#media_queries) a tecniche più complesse di [immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images#resolution_switching_different_sizes), incluse {{HTMLElement('picture')}} e gli attributi [`srcset`](/it/docs/Web/HTML/Reference/Elements/img#srcset) e [`sizes`](/it/docs/Web/HTML/Reference/Elements/img#sizes) dell'elemento {{HTMLElement('img')}}.

## Trovare aiuto

Ci sono molti altri problemi che incontrerà con HTML e CSS, rendendo la conoscenza su come trovare risposte online inestimabile.

Tra le migliori fonti di informazione ci sono il Mozilla Developer Network (è dove si trova ora!), [stackoverflow.com](https://stackoverflow.com/), e [caniuse.com](https://caniuse.com/).

Per utilizzare il Mozilla Developer Network (MDN), la maggior parte delle persone fa una ricerca su un motore di ricerca della tecnologia su cui stanno cercando informazioni, più il termine "mdn", per esempio, "mdn HTML video". MDN contiene diversi tipi di contenuto utili:

- Materiale di riferimento con informazioni di supporto per i browser per le tecnologie web lato client, ad esempio, la [pagina di riferimento \<video>](/it/docs/Web/HTML/Reference/Elements/video).
- Altri materiali di riferimento di supporto, ad esempio la nostra [Guida ai tipi e formati di media sul web](/it/docs/Web/Media/Guides/Formats),
- Tutorial utili che risolvono problemi specifici, ad esempio, [Creare un lettore video cross-browser](/it/docs/Web/Media/Guides/Audio_and_video_delivery/cross_browser_video_player).

[caniuse.com](https://caniuse.com/) fornisce informazioni di supporto, insieme a pochi link di risorse esterne utili. Ad esempio, vedere <https://caniuse.com/#search=video> (è sufficiente inserire la funzionalità che si sta cercando nella casella di testo).

[stackoverflow.com](https://stackoverflow.com/) (SO) è un sito forum dove può porre domande e far sì che altri sviluppatori condividano le loro soluzioni, cercare post precedenti e aiutare altri sviluppatori. Si consiglia di cercare e vedere se c'è già una risposta alla sua domanda, prima di postare una nuova domanda. Ad esempio, abbiamo cercato "disabilitare messa a fuoco automatica su dialogo HTML" su SO e abbiamo trovato rapidamente [Disabilita il focus automatico showModal utilizzando attributi HTML](https://stackoverflow.com/questions/63267581/disable-showmodal-auto-focusing-using-html-attributes).

A parte questo, provi a cercare il suo motore di ricerca preferito per una risposta al suo problema. È spesso utile cercare messaggi di errore specifici se li ha — altri sviluppatori avranno probabilmente avuto gli stessi problemi.

## Riepilogo

Ora dovrebbe avere confidenza con i tipi principali di problemi di compatibilità tra browser di HTML e CSS che incontrerà nello sviluppo web e sapere come risolverli.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Testing_strategies", "Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing")}}
