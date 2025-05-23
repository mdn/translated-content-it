---
title: Cos'è JavaScript?
slug: Learn_web_development/Core/Scripting/What_is_JavaScript
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{NextMenu("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting")}}

Benvenuti al corso per principianti di JavaScript di MDN! In questo articolo esamineremo JavaScript da un punto di vista generale, rispondendo a domande come "Cos'è?" e "Cosa puoi fare con esso?", e ci assicureremo che ti senta a tuo agio con lo scopo di JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi di CSS</a>.</td>
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

JavaScript è un linguaggio di scripting o programmazione che ti consente di implementare funzionalità complesse sulle pagine web — ogni volta che una pagina web fa qualcosa di più che visualizzare semplicemente informazioni statiche — mostrando aggiornamenti tempestivi dei contenuti, mappe interattive, grafiche 2D/3D animate, jukebox video scorrevoli, ecc. — puoi scommettere che probabilmente è coinvolto JavaScript. È il terzo strato della torta a strati delle tecnologie standard del web, due delle quali ([HTML](/it/docs/Learn_web_development/Core/Structuring_content) e [CSS](/it/docs/Learn_web_development/Core/Styling_basics)) abbiamo trattato in modo molto più dettagliato in altre parti dell'Area di Apprendimento.

![I tre strati delle tecnologie web standard; HTML, CSS e JavaScript](cake.png)

- {{Glossary("HTML", "HTML")}} è il linguaggio di markup che utilizziamo per strutturare e dare significato ai nostri contenuti web, per esempio definendo paragrafi, intestazioni e tabelle di dati, o incorporando immagini e video nella pagina.
- {{Glossary("CSS", "CSS")}} è un linguaggio di regole di stile che usiamo per applicare lo stile ai nostri contenuti HTML, per esempio impostando colori di sfondo e font, e disponendo i nostri contenuti in più colonne.
- {{Glossary("JavaScript", "JavaScript")}} è un linguaggio di scripting che ti consente di creare contenuti aggiornati dinamicamente, controllare multimedia, animare immagini, e praticamente tutto il resto. (Ok, non tutto, ma è sorprendente cosa puoi ottenere con poche righe di codice JavaScript.)

I tre strati si costruiscono uno sull'altro in modo armonioso. Prendiamo come esempio un pulsante. Possiamo marcarlo usando HTML per dargli struttura e scopo:

```html live-sample___string-concat-name
<button type="button">Player 1: Chris</button>
```

![Pulsante che mostra Player 1: Chris senza stile](just-html.png)

Poi possiamo aggiungere del CSS nel mix per farlo apparire carino:

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

![Pulsante che mostra Player 1: Chris con stile](html-and-css.png)

E infine, possiamo aggiungere del JavaScript per implementare un comportamento dinamico:

```js live-sample___string-concat-name
function updateName() {
  const name = prompt("Enter a new name");
  button.textContent = `Player 1: ${name}`;
}

const button = document.querySelector("button");

button.addEventListener("click", updateName);
```

Puoi cliccare su "Play" per vedere e modificare l'esempio nel MDN Playground. Prova a cliccare sull'etichetta di testo per vedere cosa succede.

{{EmbedLiveSample('string-concat-name', , '80', , , , , 'allow-modals')}}

JavaScript può fare molto di più di così — esploriamo cosa in maggiore dettaglio.

> [!NOTE]
> Prima di andare avanti, perché non approfitti subito di una sfida proposta da Scrimba in questa fase iniziale? Dai un'occhiata a [Render a welcome message](https://scrimba.com/learn-javascript-c0v/~0n?via=mdn) <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>. Se non sai come scrivere questo codice, non preoccuparti; puoi provare a fare qualche ricerca sul web per trovare alcune risposte, o visualizzare la soluzione alla fine del percorso.

## Quindi cosa può davvero fare?

Il linguaggio JavaScript lato client core è composto da alcune funzionalità di programmazione comuni che ti consentono di fare cose come:

- Archiviare valori utili all'interno di variabili. Nell'esempio sopra, ad esempio, chiediamo che venga inserito un nuovo nome e poi memorizziamo quel nome in una variabile chiamata `name`.
- Operazioni su parti di testo (note come "stringhe" in programmazione). Nell'esempio sopra prendiamo la stringa "Player 1: " e la uniamo alla variabile `name` per creare l'etichetta di testo completa, ad es., "Player 1: Chris".
- Eseguire codice in risposta a determinati eventi che si verificano su una pagina web. Abbiamo usato un evento [`click`](/it/docs/Web/API/Element/click_event) nel nostro esempio sopra per rilevare quando l'etichetta viene cliccata e poi eseguire il codice che aggiorna l'etichetta di testo.
- E molto di più!

Ciò che è ancora più eccitante però è la funzionalità costruita sopra il linguaggio JavaScript lato client. Le cosiddette **Application Programming Interfaces** (**API**) ti forniscono poteri extra da utilizzare nel tuo codice JavaScript.

Le API sono insiemi di blocchi di codice predefiniti che permettono a un sviluppatore di implementare programmi che altrimenti sarebbero difficili o impossibili da implementare. Fanno la stessa cosa per la programmazione che i kit di mobili preassemblati fanno per la costruzione domestica — è molto più facile prendere pannelli già tagliati e avvitarli insieme per fare una libreria piuttosto che progettare il tutto da soli, andare a cercare il legno corretto, tagliare tutti i pannelli nella misura e forma giusta, trovare le viti della dimensione corretta, e _poi_ assemblarli insieme per fare una libreria.

In genere cadono in due categorie.

![Due categorie di API; le API di terze parti sono mostrate al lato del browser e le API del browser sono nel browser](browser.png)

**Browser APIs** sono integrate nel tuo browser e possono esporre dati dall'ambiente del computer circostante o fare cose complesse utili. Per esempio:

- L'[API DOM (Document Object Model)](/it/docs/Web/API/Document_Object_Model) ti consente di manipolare HTML e CSS, creando, rimuovendo e modificando HTML, applicando dinamicamente nuovi stili alla tua pagina, ecc.
  Ogni volta che vedi comparire una finestra popup su una pagina o visualizzare nuovo contenuto (come abbiamo visto sopra nel nostro semplice demo), per esempio, è il DOM in azione.
- L'[API Geolocation](/it/docs/Web/API/Geolocation_API) recupera informazioni geografiche.
  Questo è il modo in cui [Google Maps](https://www.google.com/maps) è in grado di trovare la tua posizione e tracciarla su una mappa.
- Le API [Canvas](/it/docs/Web/API/Canvas_API) e [WebGL](/it/docs/Web/API/WebGL_API) ti consentono di creare grafica 2D e 3D animata.
  Le persone stanno facendo cose sorprendenti usando queste tecnologie web — vedi [Chrome Experiments](https://experiments.withgoogle.com/collection/chrome) e [webglsamples](https://webglsamples.org/).
- Le [API Audio e Video](/it/docs/Web/Media/Guides/Audio_and_video_delivery) come [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) e [WebRTC](/it/docs/Web/API/WebRTC_API) ti permettono di fare cose davvero interessanti con i multimedia, come riprodurre audio e video direttamente in una pagina web, o catturare video dalla tua webcam e visualizzarlo sul computer di qualcun altro (prova il nostro semplice [demo Snapshot](https://chrisdavidmills.github.io/snapshot/) per capire il concetto).

**Third party APIs** non sono integrate nel browser per impostazione predefinita, e generalmente devi ottenere il loro codice e informazioni da qualche parte sul Web. Per esempio:

- L'[API Bluesky](https://docs.bsky.app/) ti permette di fare cose come visualizzare i tuoi ultimi post sul tuo sito web.
- L'[API di Google Maps](https://developers.google.com/maps/) e l'[API di OpenStreetMap](https://wiki.openstreetmap.org/wiki/API) ti permettono di incorporare mappe personalizzate nel tuo sito web, e altre funzionalità simili.

> [!NOTE]
> Queste API sono avanzate e non le copriremo in questo modulo. Puoi scoprire molto di più su queste nel nostro [modulo Client-side web APIs](/it/docs/Learn_web_development/Extensions/Client-side_APIs).

C'è anche molto altro disponibile! Tuttavia, non farti prendere troppo dall'entusiasmo proprio ora. Non sarai in grado di creare il prossimo Facebook, Google Maps, o Instagram dopo aver studiato JavaScript per 24 ore — c'è una base da coprire prima. Ed è per questo che sei qui — andiamo avanti!

## Cosa sta facendo JavaScript sulla tua pagina?

Qui cominceremo a guardare effettivamente del codice e, mentre lo facciamo, esploriamo cosa accade effettivamente quando esegui del JavaScript nella tua pagina.

Ricapitoliamo brevemente la storia di cosa succede quando carichi una pagina web in un browser (di cui si parla per la prima volta nel nostro articolo [Cos'è CSS?](/it/docs/Learn_web_development/Core/Styling_basics/What_is_CSS#how_is_css_applied_to_html)). Quando carichi una pagina web nel tuo browser, stai eseguendo il tuo codice (l'HTML, il CSS e il JavaScript) in un ambiente di esecuzione (la scheda del browser). Questo è come una fabbrica che prende materie prime (il codice) e produce un prodotto (la pagina web).

![HTML, CSS e JavaScript si uniscono per creare il contenuto nella scheda del browser quando la pagina è caricata](execution.png)

Un uso molto comune di JavaScript è modificare dinamicamente HTML e CSS per aggiornare un'interfaccia utente, tramite l'API Documento Modello Oggetto (DOM) (come menzionato sopra).

### Sicurezza del browser

Ogni scheda del browser ha il suo contenitore separato per eseguire il codice (questi contenitori sono chiamati "ambienti di esecuzione" in termini tecnici) — questo significa che nella maggior parte dei casi il codice in ciascuna scheda viene eseguito completamente separatamente, e il codice in una scheda non può influenzare direttamente il codice in un'altra scheda — o su un altro sito web. Questa è una buona misura di sicurezza — se così non fosse, i pirati potrebbero iniziare a scrivere codice per rubare informazioni da altri siti web, e altre cose brutte del genere.

> [!NOTE]
> Esistono modi per inviare codice e dati tra siti web/schede diversi in modo sicuro, ma queste sono tecniche avanzate che non copriremo in questo corso.

### Ordine di esecuzione di JavaScript

Quando il browser incontra un blocco di JavaScript, generalmente lo esegue in ordine, dall'alto verso il basso. Questo significa che devi stare attento all'ordine in cui metti le cose. Per esempio, torniamo al blocco di JavaScript che abbiamo visto nel nostro primo esempio:

```js
function updateName() {
  const name = prompt("Enter a new name");
  button.textContent = `Player 1: ${name}`;
}

const button = document.querySelector("button");

button.addEventListener("click", updateName);
```

Qui, per prima cosa definiamo un blocco di codice chiamato `updateName()` (questi tipi di blocchi di codice riutilizzabili sono chiamati **funzioni**), che chiede all'utente un nuovo nome e inserisce quel nome nel testo di un pulsante. Poi memorizziamo un riferimento a un pulsante utilizzando `document.querySelector` e attacchiamo un event listener ad esso usando `addEventListener` in modo che quando il pulsante viene cliccato, la funzione `updateName()` venga eseguita.

Se scambi l'ordine delle righe `const button = ...` e `button.addEventListener(...)`, il codice non funzionerà più — invece, otterrai un errore restituito nella [console per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) — `Uncaught ReferenceError: Cannot access 'button' before initialization`. Questo significa che l'oggetto `button` non è stato ancora inizializzato, quindi non possiamo aggiungergli un event listener.

> [!NOTE]
> Non è sempre vero che JavaScript esegue esattamente in ordine dall'alto verso il basso, a causa di comportamenti come {{Glossary("Hoisting", "hoisting")}}, ma per ora, tieni presente che generalmente gli elementi devono essere definiti prima che tu li possa usare. Questa è una fonte comune di errori.

### Codice interpretato versus compilato

Potresti sentire i termini **interpretato** e **compilato** nel contesto della programmazione. Nei linguaggi interpretati, il codice viene eseguito dall'alto verso il basso e il risultato dell'esecuzione del codice viene immediatamente restituito. Non devi trasformare il codice in un'altra forma prima che il browser lo esegua. Il codice viene ricevuto nel suo formato testuale amichevole per i programmatori e processato direttamente da quello.

I linguaggi compilati invece vengono trasformati (compilati) in un'altra forma prima di essere eseguiti dal computer. Per esempio, C/C++ vengono compilati in codice macchina che viene poi eseguito dal computer. Il programma viene eseguito da un formato binario, che è stato generato dal codice sorgente originale del programma.

JavaScript è un linguaggio di programmazione interpretato leggero. Il browser web riceve il codice JavaScript nella sua forma testuale originale ed esegue lo script da quello. Da un punto di vista tecnico, la maggior parte degli interpreti JavaScript moderni utilizza in realtà una tecnica chiamata **compilazione just-in-time** per migliorare le prestazioni; il codice sorgente JavaScript viene compilato in un formato binario più veloce mentre lo script è in uso, in modo che possa essere eseguito il più rapidamente possibile. Tuttavia, JavaScript è ancora considerato un linguaggio interpretato, dato che la compilazione viene gestita durante il tempo di esecuzione, piuttosto che in anticipo.

Ci sono vantaggi in entrambi i tipi di linguaggio, ma non li discuteremo ora.

### Codice lato server versus lato client

Potresti anche sentire i termini **codice lato server** e **lato client**, specialmente nel contesto dello sviluppo web. Il codice lato client è codice che viene eseguito sul computer dell'utente — quando viene visualizzata una pagina web, il codice lato client della pagina viene scaricato, quindi eseguito e visualizzato dal browser. In questo modulo stiamo parlando esplicitamente di **JavaScript lato client**.

Il codice lato server, invece, viene eseguito sul server, poi i suoi risultati vengono scaricati e visualizzati nel browser. Esempi di linguaggi web lato server popolari includono PHP, Python, Ruby, C#, e persino JavaScript! JavaScript può essere usato anche come linguaggio lato server, per esempio nell'ambiente popolare Node.js — puoi scoprire di più su JavaScript lato server nel nostro argomento [Dynamic Websites – Server-side programming](/it/docs/Learn_web_development/Extensions/Server-side).

### Codice dinamico versus statico

La parola **dinamico** viene usata per descrivere sia il JavaScript lato client che i linguaggi lato server — si riferisce alla capacità di aggiornare la visualizzazione di una pagina web/app per mostrare cose diverse in circostanze diverse, generando nuovo contenuto come necessario. Il codice lato server genera dinamicamente nuovo contenuto sul server, ad esempio, estraendo dati da un database, mentre il JavaScript lato client genera dinamicamente nuovo contenuto all'interno del browser sul client, ad esempio, creando una nuova tabella HTML, riempiendola con i dati richiesti dal server, poi visualizzando la tabella in una pagina web mostrata all'utente. Il significato è leggermente diverso nei due contesti, ma correlato, e spesso entrambi gli approcci (lato server e lato client) lavorano insieme.

Una pagina web senza contenuti aggiornati dinamicamente viene definita **statica** — mostra semplicemente lo stesso contenuto tutto il tempo.

## Come si aggiunge JavaScript alla pagina?

JavaScript si applica alla tua pagina HTML in modo simile al CSS. Mentre il CSS utilizza gli elementi {{htmlelement("link")}} per applicare fogli di stile esterni e gli elementi {{htmlelement("style")}} per applicare fogli di stile interni all'HTML, JavaScript ha bisogno solo di un amico nel mondo dell'HTML — l'elemento {{htmlelement("script")}}. Impariamo come funziona questo.

> [!NOTE]
> Il tutorial interattivo [Setting up our JavaScript file](https://scrimba.com/learn-javascript-c0v/~03?via=mdn) <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba spiega alcuni modi diversi per aggiungere JavaScript al tuo HTML.

### JavaScript interno

1. Prima di tutto, crea una copia locale del nostro file di esempio [apply-javascript.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript.html). Salvalo in una directory da qualche parte sensata.
2. Apri il file nel tuo browser web e nel tuo editor di testo. Vedrai che l'HTML crea una semplice pagina web contenente un pulsante cliccabile.
3. Successivamente, vai al tuo editor di testo e aggiungi quanto segue in fondo al tuo body — appena prima del tuo tag di chiusura `</body>`:

   ```html
   <script>
     // JavaScript goes here
   </script>
   ```

   Nota che il codice nei tuoi documenti web viene generalmente caricato ed eseguito nell'ordine in cui appare sulla pagina. Posizionando il JavaScript in fondo, ci assicuriamo che tutti gli elementi HTML siano caricati. (Vedi anche [Strategie di caricamento degli script](#strategie_di_caricamento_degli_script) qui sotto.)

4. Ora aggiungeremo del JavaScript all'interno del nostro elemento {{htmlelement("script")}} per far fare alla pagina qualcosa di più interessante — aggiungi il seguente codice appena sotto la riga "// JavaScript goes here":

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

5. Salva il tuo file e aggiorna il browser — ora dovresti vedere che quando clicchi il pulsante, viene generato un nuovo paragrafo e posizionato sotto.

> [!NOTE]
> Se il tuo esempio sembra non funzionare, ripercorri i passaggi e verifica di aver fatto tutto correttamente.
> Hai salvato la tua copia locale del codice iniziale come un file `.html`?
> Hai aggiunto il tuo elemento {{htmlelement("script")}} subito prima del tag `</body>`?
> Hai inserito il JavaScript esattamente come mostrato? **JavaScript è case sensitive e molto delicato, quindi devi inserire la sintassi esattamente come mostrato, altrimenti potrebbe non funzionare.**

> [!NOTE]
> Puoi vedere questa versione su GitHub come [apply-javascript-internal.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html) ([vedila dal vivo anche](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html)).

### JavaScript esterno

Questo funziona bene, ma cosa succede se vogliamo mettere il nostro JavaScript in un file esterno? Esploriamo questo ora.

1. Prima, crea un nuovo file nella stessa directory del tuo file HTML di esempio. Chiamalo `script.js` — assicurati che abbia quell'estensione di file .js, poiché è così che viene riconosciuto come JavaScript.
2. Rimuovi il tuo attuale elemento {{htmlelement("script")}} in fondo a `</body>` e aggiungi il seguente poco prima del tag di chiusura `</head>` (in modo che il browser possa iniziare a caricare il file prima che sia alla fine):

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

4. Salva e aggiorna il tuo browser. Scoprirai che cliccando il pulsante non avrà effetto e se controlli la console del tuo browser, vedrai un errore del tipo `Cross-origin request blocked`. Questo perché come molte risorse esterne, i moduli JavaScript devono essere caricati dal [same origin](/it/docs/Web/Security/Same-origin_policy) dell'HTML, e gli URL `file://` non qualificano. Ci sono due soluzioni per risolvere questo problema:
   - La nostra soluzione raccomandata è [impostare un server di test locale](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server). Con il programma del server in esecuzione e che serve i file `apply-javascript-external.html` e `script.js` sulla porta `8000`, apri il tuo browser e vai su `http://localhost:8000`.
   - Se non puoi eseguire un server locale, puoi anche usare `<script defer src="script.js"></script>` al posto di `<script type="module" src="script.js"></script>`. Vedi [Strategie di caricamento degli script](#strategie_di_caricamento_degli_script) sotto per maggiori informazioni. Ma nota che le funzionalità che usiamo in altre parti del tutorial potrebbero richiedere comunque un server HTTP locale.
5. Ora il sito web funziona esattamente come prima, ma ora abbiamo il nostro JavaScript in un file esterno. Questo è generalmente una buona cosa in termini di organizzazione del tuo codice e per renderlo riutilizzabile su più file HTML. Inoltre, l'HTML è più facile da leggere senza enormi blocchi di script al suo interno.

> [!NOTE]
> Puoi vedere questa versione su GitHub come [apply-javascript-external.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html) e [script.js](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/script.js) ([vedila dal vivo anche](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html)).

### Gestori JavaScript inline

Nota che a volte ti imbatterai in pezzi di effettivo codice JavaScript all'interno dell'HTML. Potrebbe apparire qualcosa del genere:

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

Questa demo ha esattamente la stessa funzionalità delle due sezioni precedenti, tranne che l'elemento {{htmlelement("button")}} include un gestore `onclick` inline per far sì che la funzione sia eseguita quando il pulsante viene premuto.

**Per favore, non farlo però.** È una cattiva pratica contaminare il tuo HTML con JavaScript, ed è inefficiente — dovresti includere l'attributo `onclick="createParagraph()"` su ogni pulsante a cui vuoi che JavaScript si applichi.

### Usare addEventListener invece

Invece di includere JavaScript nel tuo HTML, usa una costrutto puro di JavaScript. La funzione `querySelectorAll()` ti permette di selezionare tutti i pulsanti su una pagina. Puoi quindi scorrere i pulsanti, assegnando un gestore per ciascuno utilizzando `addEventListener()`. Il codice per questo è mostrato sotto:

```js
const buttons = document.querySelectorAll("button");

for (const button of buttons) {
  button.addEventListener("click", createParagraph);
}
```

Questo potrebbe essere un po' più lungo dell'attributo `onclick`, ma funzionerà per tutti i pulsanti — non importa quanti sono sulla pagina, né quanti vengono aggiunti o rimossi. Il JavaScript non necessita di essere cambiato.

> [!NOTE]
> Prova a modificare la tua versione di `apply-javascript.html` e aggiungi qualche pulsante in più nel file. Quando ricarichi, dovresti scoprire che tutti i pulsanti quando cliccati creeranno un paragrafo. Carino, vero?

### Strategie di caricamento degli script

Tutto l'HTML su una pagina viene caricato nell'ordine in cui appare. Se stai usando JavaScript per manipolare elementi sulla pagina (o più accuratamente, il [Document Object Model](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#the_document_object_model)), il tuo codice non funzionerà se il JavaScript è caricato e interpretato prima dell'HTML a cui cerchi di fare qualcosa.

Ci sono alcune strategie diverse per assicurarti che il tuo JavaScript venga eseguito solo dopo che l'HTML è stato interpretato:

- Nell'esempio di JavaScript interno sopra, l'elemento script è posizionato in fondo al corpo del documento, e quindi viene eseguito solo dopo che il resto dell'HTML del corpo è stato interpretato.
- Nell'esempio di JavaScript esterno sopra, l'elemento script è posizionato nella testa del documento, prima che il corpo HTML sia interpretato. Ma poiché usiamo `<script type="module">`, il codice è trattato come un [modulo](/it/docs/Web/JavaScript/Guide/Modules) e il browser aspetta che tutto l'HTML sia elaborato prima di eseguire i moduli JavaScript. (Si potrebbe anche posizionare gli script esterni in fondo al corpo. Ma se c'è molto HTML e la rete è lenta, potrebbe passare molto tempo prima che il browser possa anche solo cominciare a scaricare e caricare lo script, quindi posizionare gli script esterni nella testa è solitamente meglio.)
- Se vuoi ancora usare script non-modulo nella testa del documento, che potrebbe bloccare tutta la pagina dal caricamento, e potrebbe causare errori perché viene eseguito prima che l'HTML sia interpretato:

  - Per gli script esterni, dovresti aggiungere l'attributo `defer` (o se non hai bisogno che l'HTML sia pronto, l'attributo `async`) all'elemento {{htmlelement("script")}}.
  - Per gli script interni, dovresti avvolgere il codice in un [event listener `DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event).

  Questo è oltre lo scopo del tutorial a questo punto, tuttavia, a meno che tu non debba supportare browser molto vecchi, non devi fare questo e puoi semplicemente usare `<script type="module">` invece.

## Commenti

Come con HTML e CSS, è possibile scrivere commenti nel tuo codice JavaScript che verranno ignorati dal browser, ed esistono per fornire istruzioni ai tuoi compagni sviluppatori su come funziona il codice (e a te, se torni sul tuo codice dopo sei mesi e non riesci a ricordare cosa hai fatto). I commenti sono molto utili e dovresti usarli spesso, specialmente per le applicazioni più grandi. Ci sono due tipi:

- Un commento su una singola riga è scritto dopo una doppia barra (`//`), es.

  ```js
  // I am a comment
  ```

- Un commento su più righe è scritto tra le stringhe `/*` e `*/`, es.

  ```js
  /*
    I am also
    a comment
  */
  ```

Quindi, per esempio, potremmo annotare il JavaScript del nostro ultimo demo con commenti come questo:

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
> In generale, più commenti sono solitamente migliori di meno, ma dovresti stare attento se ti accorgi di aggiungere molti commenti per spiegare cos'è una variabile (i tuoi nomi di variabili forse dovrebbero essere più intuitivi), o per spiegare operazioni molto semplici (forse il tuo codice è troppo complicato).

## Riassunto

Ecco quindi, il tuo primo passo nel mondo di JavaScript. Abbiamo iniziato solo con la teoria, per cominciare a farti capire perché utilizzeresti JavaScript e che tipo di cose puoi fare con esso. Lungo la strada, hai visto alcuni esempi di codice e imparato come JavaScript si integra con il resto del codice sul tuo sito web, fra le altre cose.

JavaScript può sembrare un po' intimidatorio ora, ma non preoccuparti — in questo corso, ti accompagneremo attraverso semplici passaggi che avranno senso in futuro. Nell'articolo successivo, ci tufferemo direttamente nella pratica, facendoti entrare e costruire i tuoi esempi di JavaScript.

{{NextMenu("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting")}}
