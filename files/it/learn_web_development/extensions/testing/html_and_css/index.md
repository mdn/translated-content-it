---
title: Gestione dei problemi comuni di HTML e CSS
short-title: Problemi comuni di HTML e CSS
slug: Learn_web_development/Extensions/Testing/HTML_and_CSS
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Testing_strategies","Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing")}}

Con il quadro impostato, esamineremo ora specificamente i problemi comuni di compatibilità tra browser che incontrerai nel codice HTML e CSS, e quali strumenti possono essere utilizzati per prevenire problemi o risolverli una volta che si presentano. Questo include la verifica del codice, la gestione dei prefissi CSS, l'uso degli strumenti di sviluppo del browser per individuare i problemi, l'uso dei polyfill per aggiungere supporto nei browser, affrontare i problemi di design responsivo e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi principali
        <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; un'idea
        dei
        <a
          href="/it/docs/Learn_web_development/Extensions/Testing/Introduction"
          >principi fondamentali dei test cross-browser</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di diagnosticare problemi comuni di HTML e CSS cross-browser e
        utilizzare strumenti e tecniche appropriate per risolverli.
      </td>
    </tr>
  </tbody>
</table>

## Il problema con HTML e CSS

Alcuni problemi con HTML e CSS derivano dal fatto che entrambe le lingue sono abbastanza semplici e spesso gli sviluppatori non li prendono sul serio, in termini di assicurarsi che il codice sia ben realizzato, efficiente e descriva semanticamente lo scopo delle funzionalità sulla pagina. Nei casi peggiori, JavaScript viene utilizzato per generare l'intero contenuto e stile della pagina web, il che rende le tue pagine inaccessibili e meno performanti (generare elementi DOM è costoso). In altri casi, le funzionalità nascenti non sono supportate in modo coerente tra i browser, il che può far sì che alcune funzionalità e stili non funzionino per alcuni utenti. Anche i problemi di design responsivo sono comuni: un sito che sembra buono in un browser desktop potrebbe offrire un'esperienza terribile su un dispositivo mobile, perché il contenuto è troppo piccolo per essere letto, o forse il sito è lento a causa di animazioni costose.

Andiamo avanti e vediamo come possiamo ridurre gli errori cross-browser derivanti da HTML/CSS.

## Prima cosa: risolvere problemi generali

Abbiamo detto nel [primo articolo di questa serie](/it/docs/Learn_web_development/Extensions/Testing/Introduction#testingdiscovery) che una buona strategia è iniziare testando in un paio di browser moderni su desktop/mobile, per assicurarsi che il tuo codice funzioni generalmente, prima di concentrarsi sui problemi cross-browser.

Nei nostri articoli [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML) e [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS), abbiamo fornito delle indicazioni di base sul debug di HTML/CSS — se non sei familiare con le basi, dovresti sicuramente studiare questi articoli prima di continuare.

Fondamentalmente, si tratta di verificare se il tuo codice HTML e CSS è ben formato e non contiene errori di sintassi.

> [!NOTE]
> Un problema comune con CSS e HTML si presenta quando diverse regole CSS entrano in conflitto tra loro. Questo può essere particolarmente problematico quando si utilizza del codice di terze parti. Ad esempio, potresti utilizzare un framework CSS e scoprire che uno dei nomi di classe che utilizza entra in conflitto con uno che hai già usato per un obiettivo diverso. Oppure potresti scoprire che l'HTML generato da una sorta di API di terze parti (generando banner pubblicitari, per esempio) include un nome di classe o ID che stai già utilizzando per uno scopo diverso. Per assicurarti che ciò non accada, devi prima ricercare gli strumenti che stai utilizzando e progettare il tuo codice attorno ad essi. Vale anche la pena "namespacciare" il CSS, ad esempio, se hai un widget, assicurati che abbia una classe distinta e poi inizia i selettori che selezionano gli elementi all'interno del widget con questa classe, in modo che i conflitti siano meno probabili. Ad esempio `.audio-player ul a`.

### Validazione

Per HTML, la validazione implica assicurarsi che tutti i tuoi tag siano correttamente chiusi e nidificati, che stai usando un doctype e che stai usando tag per il loro scopo corretto. Una buona strategia è validare regolarmente il tuo codice. Un servizio che può fare questo è il [Markup Validation Service](https://validator.w3.org/) del W3C, che ti permette di puntare al tuo codice e restituisce un elenco di errori:

![La homepage del validatore HTML](validator.png)

Il CSS ha una storia simile — devi controllare che i nomi delle proprietà siano scritti correttamente, i valori delle proprietà siano scritti correttamente e siano validi per le proprietà su cui sono utilizzati, che non manchi alcuna parentesi graffa, e così via. Anche il W3C ha un [CSS Validator](https://jigsaw.w3.org/css-validator/) disponibile per questo scopo.

### Linters

Un'altra buona opzione è scegliere un'applicazione detta Linter, che non solo indica gli errori, ma può anche segnalare avvertimenti su cattive pratiche nel tuo CSS, e altri aspetti. I Linters possono generalmente essere personalizzati per essere più rigidi o più rilassati nella loro segnalazione di errori/avvisi.

Ci sono molte applicazioni linter online, come [Dirty Markup](https://www.10bestdesign.com/dirtymarkup/) per HTML, CSS e JavaScript. Queste ti permettono di incollare il tuo codice in una finestra e segnaleranno qualsiasi errore con delle crocette, che possono poi essere passate con il mouse per ottenere un messaggio di errore che informa su quale sia il problema. Dirty Markup ti permette anche di fare correzioni al tuo markup usando il pulsante _Clean_.

![Applicazione Dirty Markup che mostra il messaggio "Unexpected character in unquoted attribute" sopra il seguente markup HTML errato: <div id=combinators">](dirty-markup.png)

Tuttavia, non è molto conveniente dover copiare e incollare il tuo codice su una pagina web per controllarne la validità più volte. Quello che desideri realmente è un linter che si inserisca nel tuo flusso di lavoro standard con il minimo fastidio.

Molti editor di codice hanno plugin linter. Ad esempio, vedi:

- [SublimeLinter](https://www.sublimelinter.com/) per Sublime Text
- [Notepad++ linter](https://sourceforge.net/projects/notepad-linter/)
- [Linters per VS Code](https://marketplace.visualstudio.com/search?target=vscode&category=Linters&sortBy=Installs)

### Strumenti di sviluppo del browser

Gli strumenti di sviluppo incorporati nella maggior parte dei browser dispongono anche di strumenti utili per scovare errori, principalmente per CSS.

> [!NOTE]
> Gli errori HTML tendono a non apparire così facilmente nei dev tools, poiché il browser tenterà di correggere automaticamente il markup mal formattato; il validatore del W3C è il modo migliore per trovare errori HTML — vedi [Validazione](#validazione) sopra.

Ad esempio, in Firefox l'ispettore CSS mostrerà le dichiarazioni CSS che non vengono applicate barrate, con un triangolo di avvertimento. Passando con il mouse sul triangolo di avvertimento verrà fornito un messaggio di errore descrittivo:

![Gli strumenti per sviluppatori evidenziano in rosso il CSS non valido e aggiungono un'icona di avvertimento attivabile al passaggio del mouse](css-message-devtools.png)

Altri strumenti di sviluppo del browser hanno caratteristiche simili.

## Problemi comuni cross-browser

Passiamo ora a esaminare alcuni dei problemi più comuni cross-browser di HTML e CSS. Le principali aree che esamineremo sono la mancanza di supporto per le funzionalità moderne e i problemi di layout.

### Browser che non supportano funzionalità moderne

Questo è un problema comune, soprattutto quando è necessario supportare browser vecchi oppure stai utilizzando funzionalità implementate in alcuni browser ma non ancora in tutti. In generale, la maggior parte delle funzionalità core di HTML e CSS (come gli elementi HTML di base, i colori di base CSS e lo stile del testo) funziona su tutti i browser che intendi supportare; più problemi emergono quando inizi a voler utilizzare nuove funzionalità HTML, CSS e API. MDN visualizza i dati di compatibilità del browser per ciascuna funzionalità documentata; ad esempio, vedi la [tabella di supporto del browser per la pseudo-classe `:has()`](/it/docs/Web/CSS/:has#browser_compatibility).

Una volta che hai identificato un elenco di tecnologie che utilizzerai che non sono supportate universalmente, è una buona idea ricercare in quali browser sono supportate e quali tecniche correlate sono utili. Vedi [Trovare aiuto](#trovare_aiuto) qui sotto.

### Comportamento fallback HTML

Alcuni problemi possono essere risolti semplicemente sfruttando il modo naturale in cui HTML/CSS funzionano.

Gli elementi HTML non riconosciuti sono trattati dal browser come elementi anonimi inline (praticamente elementi inline senza valore semantico, simili agli elementi {{htmlelement("span")}}). Puoi comunque riferirti a loro con i loro nomi e stilizzarli con CSS, per esempio — devi solo assicurarti che si comportino come desideri. Stili con CSS come faresti per qualsiasi altro elemento, includendo l'impostazione della proprietà `display` su un valore diverso da `inline` se necessario.

Elementi più complessi come HTML [`<video>`](/it/docs/Web/HTML/Reference/Elements/video), [`<audio>`](/it/docs/Web/HTML/Reference/Elements/audio), [`<picture>`](/it/docs/Web/HTML/Reference/Elements/picture), [`<object>`](/it/docs/Web/HTML/Reference/Elements/object) e [`<canvas>`](/it/docs/Web/HTML/Reference/Elements/canvas) (e altre funzionalità oltre a questo) hanno meccanismi naturali di fallback che possono essere aggiunti nel caso in cui le risorse collegate non siano supportate. Puoi aggiungere contenuto di fallback tra i tag di apertura e chiusura e i browser che non supportano l'elemento esterno ignoreranno quest'ultimo ed eseguiranno il contenuto nidificato.

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

Questo esempio include un semplice link che ti permette di scaricare il video nel caso in cui anche il player video HTML non funzioni, in modo che almeno l'utente possa accedere al video.

Un altro esempio riguarda gli elementi del modulo. Quando sono stati introdotti nuovi tipi di [`<input>`](/it/docs/Web/HTML/Reference/Elements/input) per l'inserimento di informazioni specifiche nei moduli, come orari, date, colori, numeri, ecc., se un browser non supportava la nuova funzionalità, usava il valore di default di `type="text"`. Sono stati aggiunti nuovi tipi di input, molto utili, in particolare sulle piattaforme mobili, dove fornire un modo indolore per inserire dati è molto importante per l'esperienza utente. Le piattaforme forniscono widget dell'interfaccia utente diversi a seconda del tipo di input, come un widget del calendario per l'inserimento delle date. Se un browser non supporta un tipo di input, l'utente può comunque inserire i dati richiesti.

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

L'output di questo codice è il seguente:

{{EmbedGHLiveSample("learning-area/tools-testing/cross-browser-testing/html-css/forms-test", '100%', 150)}}

> [!NOTE]
> Puoi vedere questo esempio in esecuzione live come [forms-test.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/html-css/forms-test.html) su GitHub (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/html-css/forms-test.html)).

Se visualizzi l'esempio, vedrai le funzionalità dell'interfaccia utente in azione mentre provi a inserire dati. Su dispositivi con tastiere dinamiche, verranno mostrati tastierini specifici per il tipo. Su un browser non supportato, gli input verranno mostrati come normali input di testo, il che significa che l'utente può comunque inserire le informazioni corrette.

### Comportamento fallback CSS

Il CSS è probabilmente migliore dei fallback dell'HTML. Se un browser incontra una dichiarazione o regola che non comprende, la salta completamente senza applicarla o generare un errore. Questo potrebbe essere frustrante per te e i tuoi utenti se passa inosservato nel codice di produzione, ma almeno significa che l'intero sito non si blocca a causa di un errore, e se usato in modo intelligente, puoi usarlo a tuo vantaggio.

Vediamo un esempio: un semplice box stilizzato con CSS, che ha alcune stilizzazioni fornite da varie funzionalità CSS:

![Un pulsante rosso a pillola con angoli arrotondati, ombra interna e ombra esterna](blingy-button.png)

> [!NOTE]
> Puoi vedere questo esempio in esecuzione live su GitHub come [button-with-fallback.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/html-css/button-with-fallback.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/html-css/button-with-fallback.html)).

Il pulsante ha diverse dichiarazioni di stile, ma le due che ci interessano di più sono le seguenti:

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

Qui stiamo fornendo un [colore RGB](/it/docs/Web/CSS/color_value/rgb) alla proprietà {{cssxref("background-color")}} che cambia l'opacità al passaggio del mouse per dare all'utente un suggerimento che il pulsante è interattivo, e alcune ombre {{cssxref("box-shadow")}} semi-trasparenti per dare al pulsante un po' di texture e profondità. Sebbene ora pienamente supportati, i colori RGB e le ombre non sono stati disponibili da sempre; partendo da IE9. I browser che non supportano i colori RGB ignorerebbero la dichiarazione, il che significa che nei browser più vecchi il background non verrebbe mostrato affatto e il testo sarebbe illeggibile, non va affatto bene!

![Pulsante a pillola difficile da vedere con testo bianco su uno sfondo quasi bianco](unreadable-button.png)

Per risolvere questo problema, abbiamo aggiunto una seconda dichiarazione `background-color`, che specifica solo un colore esadecimale — questo è supportato dai browser molto vecchi e funge da fallback se le funzionalità moderne e brillanti non funzionano. Ciò che accade è che un browser che visita questa pagina applicherà inizialmente il primo valore di `background-color`; quando arriva alla seconda dichiarazione di `background-color`, sovrascriverà il valore iniziale con questo valore se supporta i colori RGB. In caso contrario, ignorerà semplicemente l'intera dichiarazione e andrà avanti.

> [!NOTE]
> Lo stesso vale per altre funzionalità CSS come le [media queries](/it/docs/Web/CSS/CSS_media_queries/Using_media_queries), i blocchi [`@font-face`](/it/docs/Web/CSS/@font-face) e [`@supports`](/it/docs/Web/CSS/@supports) — se non sono supportati, il browser li ignora semplicemente.

### Supporto dei selettori

Naturalmente, nessuna funzionalità CSS verrà applicata se non usi i giusti [selettori](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) per selezionare l'elemento che desideri stilizzare!

In un elenco di selettori separati da virgole, se scrivi semplicemente un selettore in modo errato, potrebbe non corrispondere a nessun elemento. Tuttavia, se un selettore è invalido, **l'intero** elenco di selettori viene ignorato, insieme all'intero blocco di stile. Per questo motivo, includi solo una pseudo-classe o pseudo-elemento con prefisso `:-moz-` in un [elenco di selettori indulgente](/it/docs/Web/CSS/Selector_list#forgiving_selector_list), come `:where(::-moz-thumb)`. Non includere una pseudo-classe o pseudo-elemento con prefisso `:-moz-` all'interno di un gruppo separato da virgole di selettori al di fuori di un elenco di selettori indulgente come [`:is()`](/it/docs/Web/CSS/:is) o [`:where()`](/it/docs/Web/CSS/:where), poiché tutti i browser diversi da Firefox ignoreranno l'intero blocco. Nota che sia `:is()` sia `:where()` possono essere passati come parametri in altri elenchi di selettori, inclusi [`:has()`](/it/docs/Web/CSS/:has) e [`:not()`](/it/docs/Web/CSS/:not).

Troviamo che sia utile ispezionare l'elemento che stai cercando di stilizzare utilizzando gli strumenti di sviluppo del tuo browser, quindi esaminare il percorso a briciole dell'albero DOM che gli ispettori DOM tendono a fornire per vedere se il tuo selettore ha senso in confronto ad esso.

Ad esempio, negli strumenti di sviluppo di Firefox, ottieni questo tipo di output nella parte inferiore dell'ispettore DOM:

![Il percorso a briciole degli elementi è html > body > form > div.form > input#date](dom-breadcrumb-trail.png)

Se per esempio stavi cercando di utilizzare questo selettore, saresti in grado di vedere che non selezionerebbe l'elemento input come desiderato:

```css
form > #date {
  /* … */
}
```

(L'input di data non è un figlio diretto del `<form>`; sarebbe meglio usare un selettore discendente generale anziché un selettore di figli).

### Gestione dei prefissi CSS

Un altro insieme di problemi si verifica con i prefissi CSS — questi sono un meccanismo utilizzato originariamente per consentire ai fornitori di browser di implementare la loro versione di una funzionalità CSS (o JavaScript) mentre la tecnologia è in uno stato sperimentale, in modo che possano giocarci e farla correttamente senza confliggere con le implementazioni di altri browser, o con la versione finale non prefissata.

Ad esempio, Firefox utilizza `-moz-` e Chrome/Edge/Opera/Safari utilizzano `-webkit-`. Altri prefissi che potresti incontrare nel codice vecchio e che possono essere rimossi in sicurezza includono `-ms-`, utilizzato da Internet Explorer e dalle prime versioni di Edge, e `-o` che veniva utilizzato nelle versioni originarie di Opera.

Le funzionalità prefissate non dovevano mai essere utilizzate nei siti di produzione — sono soggette a cambiamenti o rimozione senza preavviso, possono causare problemi di prestazioni nelle vecchie versioni dei browser che le richiedono e sono state la causa di problemi cross-browser. Questo è un problema particolare, per esempio, quando gli sviluppatori decidono di utilizzare solo la versione `-webkit-` di una proprietà, il che implica che il sito non funzionerà in altri browser. Questo è accaduto così spesso che altri fornitori di browser hanno implementato versioni prefissate `-webkit-` di diverse proprietà CSS. Mentre i browser supportano ancora alcuni nomi di proprietà, valori di proprietà, e pseudo-classi prefissate, ora le funzionalità sperimentali sono messe dietro ai flag in modo che gli sviluppatori web possano testarle durante lo sviluppo.

Se stai usando un prefisso, assicurati che sia necessario; che la proprietà sia una delle poche funzionalità prefissate rimanenti. Puoi controllare quali browser richiedono prefissi sulle pagine di riferimento MDN e su siti come [caniuse.com](https://caniuse.com/). Se non sei sicuro, puoi anche scoprirlo facendo alcuni test direttamente nei browser. Includi la versione standard non prefissata dopo la dichiarazione prefabbricata; verrà ignorata se non supportata e utilizzata quando supportata.

```css
.masked {
  -webkit-mask-image: url(MDN.svg);
  mask-image: url(MDN.svg);
  -webkit-mask-size: 50%;
  mask-size: 50%;
}
```

Prova questo semplice esempio:

1. Utilizza questa pagina, o un altro sito che abbia un titolo prominente o altro elemento a livello di blocco.
2. Fai clic con il tasto destro o Comando + clic sull'elemento in questione e scegli Ispeziona/Ispeziona elemento (o qualunque sia l'opzione nel tuo browser) — questo dovrebbe aprire gli strumenti di sviluppo del tuo browser, con l'elemento evidenziato nell'ispettore DOM.
3. Cerca una funzionalità che puoi utilizzare per selezionare quell'elemento. Ad esempio, al momento della scrittura, questa pagina su MDN ha un logo con un ID di `mdn-docs-logo`.
4. Memorizza un riferimento a questo elemento in una variabile, ad esempio:

   ```js
   const test = document.getElementById("mdn-docs-logo");
   ```

5. Ora prova a impostare un nuovo valore per la proprietà CSS che ti interessa su quell'elemento; puoi farlo utilizzando la proprietà [style](/it/docs/Web/API/HTMLElement/style) dell'elemento, ad esempio prova a digitare questi comandi nella console JavaScript:

   ```js
   test.style.transform = "rotate(90deg)";
   ```

Mentre inizi a digitare la rappresentazione del nome della proprietà dopo il secondo punto (nota che in JavaScript, i nomi delle proprietà CSS sono scritti in {{Glossary("camel_case", "camel case")}} inferiore, non in {{Glossary("kebab_case", "kebab-case")}}), la console JavaScript dovrebbe iniziare a completare automaticamente i nomi delle proprietà che esistono nel browser e che corrispondono a quanto scritto finora. Questo è utile per scoprire quali proprietà sono implementate in quel browser.

Se devi includere funzionalità moderne, testa il supporto alle funzionalità usando [`@supports`](/it/docs/Web/CSS/@supports), che ti permette di implementare test di rilevamento delle funzionalità native e nidificare i prefissi o la nuova funzionalità all'interno del blocco `@supports`.

### Problemi di design responsivo

Il design responsivo è la pratica di creare layout web che si adattano a diverse dimensioni dei dispositivi — ad esempio, diverse larghezze dello schermo, orientamenti (ritratto o paesaggio), o risoluzioni. Un layout desktop, ad esempio, sembrerà terribile quando visualizzato su un dispositivo mobile, quindi è necessario fornire un layout mobile adatto utilizzando le [media queries](/it/docs/Web/CSS/CSS_media_queries), e assicurarsi che venga applicato correttamente utilizzando il [viewport](/it/docs/Web/HTML/Guides/Viewport_meta_element). Puoi trovare un resoconto dettagliato di tali pratiche nel [nostro tutorial sul design responsivo](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design).

Anche la risoluzione è un grande problema: ad esempiorio, i dispositivi mobili sono meno inclini ad aver bisogno di immagini grandi e pesanti rispetto ai computer desktop, e sono più inclini a avere connessioni internet più lente e piani dati costosi che rendono più problematica la larghezza di banda sprecata. Inoltre, diversi dispositivi possono avere una gamma di risoluzioni diverse, il che significa che le immagini più piccole potrebbero apparire pixelate. Esistono una serie di tecniche che permettono di aggirare tali problemi, dalle [media queries](/it/docs/Learn_web_development/Core/CSS_layout/Responsive_Design#media_queries) a tecniche di [immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images#resolution_switching_different_sizes) più complesse, inclusi {{HTMLElement('picture')}} e gli attributi [`srcset`](/it/docs/Web/HTML/Reference/Elements/img#srcset) e [`sizes`](/it/docs/Web/HTML/Reference/Elements/img#sizes) dell'elemento {{HTMLElement('img')}}.

## Trovare aiuto

Ci sono molti altri problemi che incontrerai con HTML e CSS, rendendo la conoscenza di come trovare risposte online inestimabile.

Tra le migliori fonti di informazioni di supporto ci sono il Mozilla Developer Network (è dove ti trovi ora!), [stackoverflow.com](https://stackoverflow.com/), e [caniuse.com](https://caniuse.com/).

Per usare il Mozilla Developer Network (MDN), la maggior parte delle persone fa una ricerca sul motore di ricerca della tecnologia per cui cercano informazioni, aggiungendo il termine "mdn", ad esempio, "mdn HTML video". MDN contiene diversi tipi di contenuti utili:

- Materiale di riferimento con informazioni sul supporto del browser per le tecnologie web lato client, ad esempio la [pagina di riferimento \<video>](/it/docs/Web/HTML/Reference/Elements/video).
- Altri materiali di riferimento di supporto, come la nostra [Guida ai tipi e formati media sul web](/it/docs/Web/Media/Guides/Formats),
- Tutorial utili che risolvono problemi specifici, ad esempio, [Creare un player video cross-browser](/it/docs/Web/Media/Guides/Audio_and_video_delivery/cross_browser_video_player).

[caniuse.com](https://caniuse.com/) fornisce informazioni sul supporto, insieme a alcuni link utili a risorse esterne. Per esempio, vedi <https://caniuse.com/#search=video> (basta inserire la funzionalità che si sta cercando nella casella di testo).

[stackoverflow.com](https://stackoverflow.com/) (SO) è un sito forum dove puoi fare domande e avere condivisioni di soluzioni dai colleghi sviluppatori, cercare post precedenti e aiutare altri sviluppatori. È consigliabile guardare e vedere se c'è una risposta alla tua domanda già presente, prima di postare una nuova domanda. Ad esempio, abbiamo cercato "disabilitare autofocus su dialog HTML" su SO, e abbiamo trovato rapidamente [Disable showModal auto-focusing using HTML attributes](https://stackoverflow.com/questions/63267581/disable-showmodal-auto-focusing-using-html-attributes).

A parte questo, prova a cercare la tua risposta preferita nei motori di ricerca per il tuo problema. È spesso utile cercare messaggi di errore specifici se li hai — altri sviluppatori probabilmente avranno avuto gli stessi problemi che hai tu.

## Riepilogo

Ora dovresti avere familiarità con i principali tipi di problemi cross-browser di HTML e CSS che incontrerai nello sviluppo web e sapere come procedere per risolverli.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Testing_strategies","Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing")}}
