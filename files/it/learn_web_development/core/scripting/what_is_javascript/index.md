---
title: Cos'è JavaScript?
slug: Learn_web_development/Core/Scripting/What_is_JavaScript
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{NextMenu("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting")}}

Benvenuto al corso di JavaScript per principianti di MDN! In questo articolo esamineremo JavaScript da un punto di vista generale, rispondendo a domande come "Che cos'è?" e "Cosa puoi fare con esso?" e facendo in modo che tu sia a tuo agio con lo scopo di JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è JavaScript e come si integra in un sito web.</li>
          <li>Cosa puoi fare con JavaScript.</li>
          <li>Aggiungere JavaScript a una pagina web.</li>
          <li>Scrivere commenti all'interno di JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Una definizione ad alto livello

JavaScript è un linguaggio di scripting o di programmazione che consente di implementare funzionalità complesse sulle pagine web — ogni volta che una pagina web fa più che semplicemente mostrare informazioni statiche — mostrando aggiornamenti di contenuti in tempo reale, mappe interattive, grafica animata 2D/3D, videoteca scorrevole, ecc. — si può scommettere che probabilmente è coinvolto JavaScript. È il terzo strato della "torta a strati" delle tecnologie web standard, due delle quali ([HTML](/it/docs/Learn_web_development/Core/Structuring_content) e [CSS](/it/docs/Learn_web_development/Core/Styling_basics)) abbiamo approfondito in altre parti dell'Area di apprendimento.

![I tre strati delle tecnologie web standard; HTML, CSS e JavaScript](cake.png)

- {{Glossary("HTML", "HTML")}} è il linguaggio di markup che usiamo per strutturare e dare significato ai nostri contenuti web, per esempio definendo paragrafi, intestazioni e tabelle di dati, o incorporando immagini e video nella pagina.
- {{Glossary("CSS", "CSS")}} è un linguaggio di regole di stile che usiamo per applicare lo stile ai nostri contenuti HTML, per esempio impostando colori di sfondo e font, e disponendo i nostri contenuti in più colonne.
- {{Glossary("JavaScript", "JavaScript")}} è un linguaggio di scripting che ti consente di creare contenuti che si aggiornano dinamicamente, controllare i multimedia, animare le immagini e praticamente tutto il resto. (OK, non tutto, ma è incredibile cosa puoi realizzare con poche righe di codice JavaScript.)

I tre strati si costruiscono l'uno sull'altro in maniera armoniosa. Prendiamo un pulsante come esempio. Possiamo marcarlo usando HTML per dargli struttura e scopo:

```html live-sample___string-concat-name
<button type="button">Player 1: Chris</button>
```

![Pulsante che mostra Giocatore 1: Chris senza stile](just-html.png)

Poi possiamo aggiungere un po' di CSS nel mix per farlo apparire bello:

```css live-sample___string-concat-name
button {
  font-family: "helvetica neue", helvetica, sans-serif;
  letter-spacing: 1px;
  text-transform: uppercase;
  border: 2px solid rgb(200 200 0 / 60%);
  background-color: rgb(0 217 217 / 60%);
  color: rgb(100 0 0 / 100%);
  box-shadow: 1px 1px 2px rgb(0 0 200 / 40%);
  border-radius: 10px;
  padding: 3px 10px;
  cursor: pointer;
}
```

![Pulsante che mostra Giocatore 1: Chris con stile](html-and-css.png)

E infine, possiamo aggiungere un po' di JavaScript per implementare un comportamento dinamico:

```js live-sample___string-concat-name
function updateName() {
  const name = prompt("Enter a new name");
  button.textContent = `Player 1: ${name}`;
}

const button = document.querySelector("button");

button.addEventListener("click", updateName);
```

Puoi cliccare su "Play" per vedere e modificare l'esempio nel MDN Playground. Prova a cliccare sull'etichetta del testo per vedere cosa succede.

{{EmbedLiveSample('string-concat-name', , '80', , , , , 'allow-modals')}}

JavaScript può fare molto di più — esploriamo in dettaglio.

> [!NOTE]
> Prima di andare avanti, perché non entrare subito in un esercizio con una sfida di Scrimba a questo punto iniziale? Controlla [Render a welcome message](https://scrimba.com/learn-javascript-c0v/~0n?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>. Se non sai come scrivere questo codice, non preoccuparti; potresti provare a fare delle ricerche sul web per trovare alcune risposte, o visualizzare la soluzione alla fine dell'esercizio su Scrimba.

## Quindi cosa può fare realmente?

Il nucleo del linguaggio JavaScript lato client consiste in alcune caratteristiche di programmazione comuni che permettono di fare cose come:

- Memorizzare valori utili all'interno di variabili. Nell'esempio sopra, ad esempio, chiediamo di inserire un nuovo nome, poi memorizziamo quel nome in una variabile chiamata `name`.
- Operazioni su porzioni di testo (note come "stringhe" in programmazione). Nell'esempio sopra prendiamo la stringa "Player 1: " e la uniamo alla variabile `name` per creare l'etichetta di testo completa, ad esempio "Player 1: Chris".
- Eseguire il codice in risposta a certi eventi che si verificano su una pagina web. Abbiamo utilizzato un evento [`click`](/it/docs/Web/API/Element/click_event) nel nostro esempio per rilevare quando l'etichetta viene cliccata e poi eseguire il codice che aggiorna l'etichetta di testo.
- E molto altro!

Ciò che è ancora più eccitante, tuttavia, è la funzionalità costruita sopra il linguaggio JavaScript lato client. Le cosiddette **Application Programming Interfaces** (**API**) ti forniscono superpoteri extra da usare nel tuo codice JavaScript.

Le API sono insiemi di blocchetti di codice preconfezionati che permettono al sviluppatore di implementare programmi che altrimenti sarebbero difficili o impossibili da implementare. Fanno la stessa cosa per la programmazione che fanno i kit di mobili preconfezionati per la costruzione di case — è molto più facile prendere pannelli già tagliati e avvitarli insieme per costruire una libreria che non progettare tutto da zero, trovare il legno giusto, tagliare i pannelli alla dimensione e forma giuste, trovare le viti delle dimensioni corrette, e _poi_ montarli per realizzare una libreria.

Generalmente rientrano in due categorie.

![Due categorie di API; le API di terze parti sono visualizzate a lato del browser e le API del browser sono nel browser](browser.png)

**API del Browser** sono integrate nel tuo browser web, e sono in grado di esporre dati dall'ambiente computer circostante, o fare cose complesse. Per esempio:

- L'[API DOM (Document Object Model)](/it/docs/Web/API/Document_Object_Model) ti permette di manipolare HTML e CSS, creando, rimuovendo e cambiando HTML, applicando dinamicamente nuovi stili alla tua pagina, ecc. Ogni volta che vedi una finestra popup apparire su una pagina, o qualche nuovo contenuto visualizzato (come abbiamo visto sopra nel nostro semplice demo) per esempio, quello è il DOM in azione.
- L'[API Geolocation](/it/docs/Web/API/Geolocation_API) recupera informazioni geografiche. Questo è come [Google Maps](https://www.google.com/maps) è in grado di trovare la tua posizione e tracciarla su una mappa.
- L'[API Canvas](/it/docs/Web/API/Canvas_API) e l'[API WebGL](/it/docs/Web/API/WebGL_API) ti permettono di creare grafica animata 2D e 3D. Le persone stanno facendo cose sorprendenti usando queste tecnologie web — vedi [Chrome Experiments](https://experiments.withgoogle.com/collection/chrome) e [webglsamples](https://webglsamples.org/).
- Le [API Audio e Video](/it/docs/Web/Media/Guides/Audio_and_video_delivery) come [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) e [WebRTC](/it/docs/Web/API/WebRTC_API) ti permettono di fare cose veramente interessanti con i multimedia, come riprodurre audio e video direttamente in una pagina web, o acquisire video dalla tua webcam e visualizzarlo sul computer di qualcun altro (prova il nostro semplice [demo di Snapshot](https://chrisdavidmills.github.io/snapshot/) per farti un'idea).

**API di terze parti** non sono integrate nel browser di default, e generalmente devi ottenere il loro codice e le informazioni da qualche parte sul Web. Per esempio:

- L'[API Bluesky](https://docs.bsky.app/) ti permette di fare cose come visualizzare i tuoi ultimi post sul tuo sito web.
- L'[API di Google Maps](https://developers.google.com/maps/) e l'[API di OpenStreetMap](https://wiki.openstreetmap.org/wiki/API) ti permettono di inserire mappe personalizzate nel tuo sito web e altre funzionalità del genere.

> [!NOTE]
> Queste API sono avanzate e non ne copriremo nessuna in questo modulo. Puoi trovare molto di più su queste nel nostro modulo [API web lato client](/it/docs/Learn_web_development/Extensions/Client-side_APIs).

C'è molto altro disponibile, anche! Tuttavia, non entusiasmarti troppo subito. Non sarai in grado di costruire il prossimo Facebook, Google Maps, o Instagram dopo aver studiato JavaScript per 24 ore — ci sono molte basi da coprire prima. Ed è per questo che sei qui — andiamo avanti!

## Cosa sta facendo JavaScript sulla tua pagina?

Qui inizieremo davvero a esaminare un po' di codice, ed esplorare cosa accade realmente quando esegui del JavaScript nella tua pagina.

Ricordiamo brevemente la storia di ciò che accade quando carichi una pagina web in un browser (di cui abbiamo parlato per la prima volta nel nostro articolo [Cos'è CSS?](/it/docs/Learn_web_development/Core/Styling_basics/What_is_CSS#how_is_css_applied_to_html)). Quando carichi una pagina web nel tuo browser, stai eseguendo il tuo codice (l'HTML, il CSS e il JavaScript) all'interno di un ambiente di esecuzione (la scheda del browser). Questo è come una fabbrica che prende in ingresso materie prime (il codice) e produce un prodotto (la pagina web).

![Codice HTML, CSS e JavaScript si combinano per creare il contenuto nella scheda del browser quando la pagina è carregata](execution.png)

Un uso molto comune di JavaScript è quello di modificare dinamicamente HTML e CSS per aggiornare un'interfaccia utente, tramite l'API del Document Object Model (come menzionato sopra).

### Sicurezza del browser

Ogni scheda del browser ha il suo contenitore separato per l'esecuzione del codice (questi contenitori sono chiamati "ambienti di esecuzione" in termini tecnici) — ciò significa che nella maggior parte dei casi il codice in ciascuna scheda viene eseguito completamente separato, e il codice in una scheda non può influenzare direttamente il codice in un'altra scheda — o su un altro sito web. Questa è una buona misura di sicurezza — se non fosse così, i pirati potrebbero iniziare a scrivere codice per rubare informazioni da altri siti web, e altri comportamenti dannosi.

> [!NOTE]
> Ci sono modi per inviare codice e dati tra diversi siti web/schede in modo sicuro, ma queste sono tecniche avanzate che non copriremo in questo corso.

### Ordine di esecuzione di JavaScript

Quando il browser incontra un blocco di JavaScript, generalmente lo esegue in ordine, dall'alto verso il basso. Questo significa che devi fare attenzione all'ordine in cui metti le cose. Per esempio, torniamo al blocco di JavaScript che abbiamo visto nel nostro primo esempio:

```js
function updateName() {
  const name = prompt("Enter a new name");
  button.textContent = `Player 1: ${name}`;
}

const button = document.querySelector("button");

button.addEventListener("click", updateName);
```

Qui, prima definiamo un blocco di codice chiamato `updateName()` (questi tipi di blocchi di codice riutilizzabili sono chiamati **funzioni**), che chiede all'utente un nuovo nome e inserisce quel nome nel testo di un pulsante. Poi memorizziamo un riferimento a un pulsante usando `document.querySelector` e gli assegniamo un event listener usando `addEventListener`, così che quando il pulsante viene cliccato, la funzione `updateName()` viene eseguita.

Se dovessi invertire l'ordine delle righe `const button = ...` e `button.addEventListener(...)`, il codice non funzionerebbe più — invece, otterresti un errore nella [console degli sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) — `Uncaught ReferenceError: Cannot access 'button' before initialization`. Questo significa che l'oggetto `button` non è stato ancora inizializzato, quindi non possiamo aggiungergli un event listener.

> [!NOTE]
> Non è sempre vero che JavaScript viene eseguito esattamente in ordine dall'alto verso il basso, a causa di comportamenti come {{Glossary("Hoisting", "hoisting")}}, ma per ora, tieni presente che generalmente gli elementi devono essere definiti prima di poterli utilizzare. Questo è una fonte comune di errori.

### Codice interpretato vs compilato

Potresti sentire i termini **interpretato** e **compilato** nel contesto della programmazione. Nei linguaggi interpretati, il codice viene eseguito dall'alto verso il basso e il risultato dell'esecuzione del codice viene immediatamente restituito. Non devi trasformare il codice in un'altra forma prima che il browser lo esegua. Il codice viene ricevuto nella sua forma testuale, comprensibile al programmatore e viene processato direttamente da quella.

I linguaggi compilati invece vengono trasformati (compilati) in un'altra forma prima di essere eseguiti dal computer. Per esempio, C/C++ vengono compilati in codice macchina che viene poi eseguito dal computer. Il programma viene eseguito da un formato binario, generato dal codice sorgente originale del programma.

JavaScript è un linguaggio di programmazione leggero e interpretato. Il browser web riceve il codice JavaScript nella sua forma testuale originale e esegue lo script da quello. Da un punto di vista tecnico, la maggior parte degli interpreti JavaScript moderni utilizza in realtà una tecnica chiamata **compilazione just-in-time** per migliorare le prestazioni; il codice sorgente JavaScript viene compilato in un formato binario più veloce mentre lo script viene utilizzato, così che possa essere eseguito il più rapidamente possibile. Tuttavia, JavaScript è ancora considerato un linguaggio interpretato, poiché la compilazione viene gestita in fase di esecuzione, piuttosto che in anticipo.

Ci sono vantaggi per entrambi i tipi di linguaggio, ma non li discuteremo adesso.

### Codice lato server vs codice lato client

Potresti anche sentire i termini **lato server** e **lato client**, soprattutto nel contesto dello sviluppo web. Il codice lato client è il codice che viene eseguito sul computer dell'utente — quando si visualizza una pagina web, il codice lato client della pagina viene scaricato, quindi eseguito e visualizzato dal browser. In questo modulo parliamo esplicitamente di **JavaScript lato client**.

Il codice lato server invece viene eseguito sul server, quindi i suoi risultati vengono scaricati e visualizzati nel browser. Esempi di linguaggi web lato server popolari includono PHP, Python, Ruby, C# e persino JavaScript! JavaScript può essere usato anche come linguaggio lato server, per esempio nell'ambiente popolare Node.js — puoi trovare ulteriori informazioni sul JavaScript lato server nel nostro argomento [Siti web dinamici - Programmazione lato server](/it/docs/Learn_web_development/Extensions/Server-side).

### Codice dinamico vs statico

La parola **dinamico** è usata per descrivere sia JavaScript lato client, sia i linguaggi lato server — si riferisce alla capacità di aggiornare la visualizzazione di una pagina/app web per mostrare cose diverse in circostanze diverse, generando nuovi contenuti come richiesto. Il codice lato server genera dinamicamente nuovi contenuti sul server, ad esempio, recuperando dati da un database, mentre JavaScript lato client genera dinamicamente nuovi contenuti all'interno del browser sul client, ad esempio, creando una nuova tabella HTML, riempiendola con dati richiesti dal server, quindi visualizzando la tabella in una pagina web mostrata all'utente. Il significato è leggermente diverso nei due contesti, ma correlato, e entrambi gli approcci (lato server e lato client) di solito lavorano insieme.

Una pagina web senza contenuti che si aggiornano dinamicamente viene definita **statica** — mostra semplicemente lo stesso contenuto tutto il tempo.

## Come aggiungi JavaScript alla tua pagina?

JavaScript viene applicato alla tua pagina HTML in maniera simile al CSS. Mentre il CSS utilizza {{htmlelement("link")}} elementi per applicare fogli di stile esterni e {{htmlelement("style")}} elementi per applicare fogli di stile interni all'HTML, JavaScript ha bisogno solo di un amico nel mondo dell'HTML — l'elemento {{htmlelement("script")}}. Impariamo come funziona.

### JavaScript interno

1. Per prima cosa, fai una copia locale del nostro file esempio [apply-javascript.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript.html). Salvalo in una directory da qualche parte in modo sensato.
2. Apri il file nel tuo browser web e nel tuo editor di testo. Vedrai che l'HTML crea una semplice pagina web contenente un pulsante cliccabile.
3. Successivamente, vai al tuo editor di testo e aggiungi il seguente codice alla fine del tuo corpo — appena prima del tuo tag di chiusura `</body>`:

   ```html
   <script>
     // JavaScript goes here
   </script>
   ```

   Nota che il codice nei tuoi documenti web viene generalmente caricato ed eseguito nell'ordine in cui appare sulla pagina. Posizionando JavaScript in fondo, garantiamo che tutti gli elementi HTML siano caricati. (Vedi anche [strategie di caricamento script](#strategie_di_caricamento_script) sotto.)

4. Ora aggiungeremo un po' di JavaScript all'interno del nostro elemento {{htmlelement("script")}} per far fare alla pagina qualcosa di più interessante — aggiungi il seguente codice appena sotto la riga "// JavaScript goes here":

   ```js
   function createParagraph() {
     const para = document.createElement("p");
     para.textContent = "You clicked the button!";
     document.body.appendChild(para);
   }

   const buttons = document.querySelectorAll("button");

   for (const button of buttons) {
     button.addEventListener("click", createParagraph);
   }
   ```

5. Salva il tuo file e aggiorna il browser — ora dovresti vedere che quando clicchi sul pulsante, viene generato un nuovo paragrafo e posizionato sotto.

> [!NOTE]
> Se il tuo esempio non sembra funzionare, ripercorri i passaggi e verifica di aver fatto tutto correttamente. Hai salvato la tua copia locale del codice iniziale come file `.html`? Hai aggiunto il tuo elemento {{htmlelement("script")}} appena prima del tag `</body>`? Hai inserito il JavaScript esattamente come mostrato? **JavaScript è sensibile alle maiuscole e molto pignolo, quindi devi inserire la sintassi esattamente come mostrato, altrimenti potrebbe non funzionare.**

> [!NOTE]
> Puoi vedere questa versione su GitHub come [apply-javascript-internal.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html) ([vedila anche in live](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html)).

### JavaScript esterno

Questo funziona alla grande, ma cosa succede se volessimo mettere il nostro JavaScript in un file esterno? Esploriamo questo adesso.

1. Prima, crea un nuovo file nella stessa directory del tuo file HTML di esempio. Chiamalo `script.js` — assicurati che abbia l'estensione di filename .js, poiché è così che viene riconosciuto come JavaScript.
2. Rimuovi il tuo attuale elemento {{htmlelement("script")}} in fondo al `</body>` e aggiungi il seguente appena prima del tag di chiusura `</head>` (in questo modo il browser può iniziare a caricare il file prima che sia in fondo):

   ```html
   <script type="module" src="script.js"></script>
   ```

3. All'interno di `script.js`, aggiungi il seguente script:

   ```js
   function createParagraph() {
     const para = document.createElement("p");
     para.textContent = "You clicked the button!";
     document.body.appendChild(para);
   }

   const buttons = document.querySelectorAll("button");

   for (const button of buttons) {
     button.addEventListener("click", createParagraph);
   }
   ```

4. Salva e aggiorna il tuo browser. Scoprirai che cliccando sul pulsante non produce effetti, e se controlli la console del tuo browser, vedrai un errore del tipo `Cross-origin request blocked`. Questo perché, come molte risorse esterne, i moduli JavaScript devono essere caricati dalla [stessa origine](/it/docs/Web/Security/Same-origin_policy) dell'HTML, e gli URL `file://` non lo qualificano. Ci sono due soluzioni per risolvere questo problema:
   - La nostra soluzione raccomandata è [configurare un server di prova locale](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server). Con il server in esecuzione e che serve i file `apply-javascript-external.html` e `script.js` sulla porta `8000`, apri il tuo browser e vai su `http://localhost:8000`.
   - Se non puoi eseguire un server locale, puoi anche usare `<script defer src="script.js"></script>` invece di `<script type="module" src="script.js"></script>`. Vedi le [strategie di caricamento script](#strategie_di_caricamento_script) per ulteriori informazioni. Ma nota che le funzionalità che utilizziamo in altre parti del tutorial potrebbero richiedere comunque un server HTTP locale.
5. Ora il sito funziona proprio come prima, ma ora abbiamo il nostro JavaScript in un file esterno. Questo è generalmente un bene in termini di organizzazione del tuo codice e lo rende riutilizzabile su più file HTML. Inoltre, l'HTML è più facile da leggere senza grossi blocchi di script inseriti.

> [!NOTE]
> Puoi vedere questa versione su GitHub come [apply-javascript-external.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html) e [script.js](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/script.js) ([vedila anche in live](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html)).

### Gestori JavaScript inline

Nota che a volte potresti incontrare frammenti di codice JavaScript effettivamente all'interno dell'HTML. Potrebbe sembrare qualcosa di simile a questo:

```js example-bad
function createParagraph() {
  const para = document.createElement("p");
  para.textContent = "You clicked the button!";
  document.body.appendChild(para);
}
```

```html example-bad
<button onclick="createParagraph()">Click me!</button>
```

Puoi provare questa versione del nostro demo qui sotto.

{{ EmbedLiveSample('Inline_JavaScript_handlers', '100%', 150) }}

Questo demo ha esattamente la stessa funzionalità delle due sezioni precedenti, tranne che l'elemento {{htmlelement("button")}} include un gestore `onclick` inline per far eseguire la funzione quando il pulsante viene premuto.

**Per favore non farlo, tuttavia.** È una cattiva pratica contaminare il tuo HTML con JavaScript, ed è inefficiente — dovresti includere l'attributo `onclick="createParagraph()"` su ogni pulsante a cui desideri applicare il JavaScript.

### Utilizzare addEventListener invece

Invece di includere JavaScript nel tuo HTML, usa una costruzione pura di JavaScript. La funzione `querySelectorAll()` ti permette di selezionare tutti i pulsanti su una pagina. Puoi quindi scorrere i pulsanti, assegnando un gestore per ciascuno usando `addEventListener()`. Il codice per questo è mostrato di seguito:

```js
const buttons = document.querySelectorAll("button");

for (const button of buttons) {
  button.addEventListener("click", createParagraph);
}
```

Questo potrebbe essere un po' più lungo dell'attributo `onclick`, ma funzionerà per tutti i pulsanti — non importa quanti siano sulla pagina, né quanti ne vengano aggiunti o rimossi. Il JavaScript non deve essere modificato.

> [!NOTE]
> Prova a modificare la tua versione di `apply-javascript.html` e aggiungi alcuni altri pulsanti al file. Quando ricarichi, dovresti scoprire che tutti i pulsanti, quando cliccati, creeranno un paragrafo. Bello, vero?

### Strategie di caricamento script

Tutto l'HTML su una pagina viene caricato nell'ordine in cui appare. Se stai usando JavaScript per manipolare elementi nella pagina (o più precisamente, il [Document Object Model](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#the_document_object_model)), il tuo codice non funzionerà se JavaScript è caricato e analizzato prima dell'HTML a cui stai cercando di fare qualcosa.

Ci sono alcune strategie diverse per garantire che il tuo JavaScript venga eseguito solo dopo che l'HTML è stato analizzato:

- Nell'esempio di JavaScript interno sopra, l'elemento script è posizionato in fondo al corpo del documento, e quindi viene eseguito solo dopo che il resto del corpo HTML è stato analizzato.
- Nell'esempio di JavaScript esterno sopra, l'elemento script è posizionato nella testa del documento, prima che il corpo HTML sia analizzato. Ma poiché stiamo usando `<script type="module">`, il codice viene trattato come un [modulo](/it/docs/Web/JavaScript/Guide/Modules) e il browser aspetta che tutto l'HTML sia processato prima di eseguire i moduli JavaScript. (Potresti anche posizionare script esterni in fondo al corpo. Ma se c'è molto HTML e la rete è lenta, potrebbe volerci molto tempo prima che il browser possa iniziare a recuperare e caricare lo script, quindi di solito è meglio posizionare gli script esterni nella testa.)
- Se desideri comunque utilizzare script non-modulo nella testa del documento, cosa che potrebbe bloccare l'intera pagina dall'essere visualizzata e causare errori perché viene eseguito prima che l'HTML sia analizzato:

  - Per gli script esterni, dovresti aggiungere l'attributo `defer` (o se non hai bisogno che l'HTML sia pronto, l'attributo `async`) all'elemento {{htmlelement("script")}}.
  - Per gli script interni, dovresti racchiudere il codice in un [listener di eventi `DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event).

  Questo va oltre lo scopo del tutorial a questo punto, ma a meno che tu non debba supportare browser molto vecchi, non devi fare questo e puoi semplicemente usare `<script type="module">` al suo posto.

## Commenti

Come con HTML e CSS, è possibile inserire commenti nel tuo codice JavaScript che verranno ignorati dal browser ed esistono per fornire istruzioni agli sviluppatori tuoi colleghi su come funziona il codice (e a te stesso, se torni al tuo codice dopo sei mesi e non riesci a ricordare cosa hai fatto). I commenti sono molto utili, e dovresti usarli spesso, particolarmente per applicazioni più grandi. Ci sono due tipi:

- Un commento su una singola riga è scritto dopo una doppia barra obliqua (`//`), ad esempio.

  ```js
  // I am a comment
  ```

- Un commento multi-linea è scritto tra le stringhe `/*` e `*/`, ad esempio.

  ```js
  /*
    I am also
    a comment
  */
  ```

Quindi, per esempio, potremmo annotare il javascript del nostro ultimo demo con commenti come segue:

```js
// Function: creates a new paragraph and appends it to the bottom of the HTML body.

function createParagraph() {
  const para = document.createElement("p");
  para.textContent = "You clicked the button!";
  document.body.appendChild(para);
}

/*
  1. Get references to all the buttons on the page in an array format.
  2. Loop through all the buttons and add a click event listener to each one.

  When any button is pressed, the createParagraph() function will be run.
*/

const buttons = document.querySelectorAll("button");

for (const button of buttons) {
  button.addEventListener("click", createParagraph);
}
```

> [!NOTE]
> In generale più commenti sono solitamente meglio che meno, ma dovresti essere cauto se ti trovi ad aggiungere molti commenti per spiegare cosa sono le variabili (forse i tuoi nomi di variabili dovrebbero essere più intuitivi), o per spiegare operazioni molto semplici (forse il tuo codice è sovracomplesso).

## Riassunto

Ecco fatto, il tuo primo passo nel mondo di JavaScript. Abbiamo iniziato solo con la teoria, per iniziare a farti abituare al motivo per cui utilizzeresti JavaScript e al tipo di cose che puoi fare con esso. Lungo la strada, hai visto alcuni esempi di codice e imparato come JavaScript si inserisce nel resto del codice del tuo sito web, tra le altre cose.

JavaScript può sembrare un po' intimidatorio in questo momento, ma non preoccuparti — in questo corso, ti guideremo attraverso semplici passaggi che avranno senso andando avanti. Nel prossimo articolo, ci tufferemo direttamente nella pratica, facendoti iniziare a costruire i tuoi esempi di JavaScript.

{{NextMenu("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting")}}
