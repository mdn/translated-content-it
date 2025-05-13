---
title: Cos'è JavaScript?
slug: Learn_web_development/Core/Scripting/What_is_JavaScript
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{NextMenu("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting")}}

Benvenuti al corso di JavaScript per principianti di MDN!
In questo articolo esamineremo JavaScript da un punto di vista generale, rispondendo a domande come "Cos'è?" e "Cosa si può fare con esso?", assicurandoci che lei sia a suo agio con lo scopo di JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di base dell'<a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti del CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è JavaScript e come si integra in un sito web.</li>
          <li>Cosa si può fare con JavaScript.</li>
          <li>Aggiungere JavaScript a una pagina web.</li>
          <li>Scrivere commenti all'interno di JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Una definizione di alto livello

JavaScript è un linguaggio di scripting o programmazione che consente di implementare funzionalità complesse su pagine web — ogni volta che una pagina web fa più che semplicemente presentare informazioni statiche per essere visualizzate — visualizzazione di aggiornamenti di contenuto tempestivi, mappe interattive, grafica animata 2D/3D, jukebox video a scorrimento, ecc. — si può scommettere che probabilmente JavaScript è coinvolto.
È il terzo strato della torta a strati delle tecnologie standard del web, due delle quali ([HTML](/it/docs/Learn_web_development/Core/Structuring_content) e [CSS](/it/docs/Learn_web_development/Core/Styling_basics)) abbiamo trattato in modo più dettagliato in altre parti dell'area di apprendimento.

![I tre strati delle tecnologie standard del web: HTML, CSS e JavaScript](cake.png)

- {{Glossary("HTML", "HTML")}} è il linguaggio di markup che utilizziamo per strutturare e dare significato ai nostri contenuti web, ad esempio definendo paragrafi, intestazioni e tabelle dati, o incorporando immagini e video nella pagina.
- {{Glossary("CSS", "CSS")}} è un linguaggio di regole di stile che utilizziamo per applicare lo stile ai nostri contenuti HTML, ad esempio impostando colori di sfondo e font e disponendo i nostri contenuti in più colonne.
- {{Glossary("JavaScript", "JavaScript")}} è un linguaggio di scripting che consente di creare contenuti che si aggiornano dinamicamente, controllare i multimedia, animare le immagini e praticamente tutto il resto. (Okay, non tutto, ma è straordinario ciò che si può ottenere con poche righe di codice JavaScript.)

I tre strati si costruiscono uno sopra l'altro in modo coerente. Prendiamo ad esempio un pulsante. Possiamo contrassegnarlo usando HTML per dargli una struttura e uno scopo:

```html live-sample___string-concat-name
<button type="button">Player 1: Chris</button>
```

![Pulsante che mostra Player 1: Chris senza stile](just-html.png)

Successivamente, possiamo aggiungere del CSS al mix per renderlo esteticamente gradevole:

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

Può fare clic su "Play" per visualizzare e modificare l'esempio nel MDN Playground.
Provi a fare clic sull'etichetta di testo per vedere cosa accade.

{{EmbedLiveSample('string-concat-name', , '80', , , , , 'allow-modals')}}

JavaScript può fare molto di più — esploriamo in dettaglio cosa può fare.

> [!NOTE]
> Prima di andare avanti, perché non cimentarsi subito in una sfida di Scrimba a questo primo stadio? Visiti [Rendere un messaggio di benvenuto](https://scrimba.com/learn-javascript-c0v/~0n?via=mdn) <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>. Se non sa come scrivere questo codice, non si preoccupi affatto; può provare a fare delle ricerche sul web per trovare alcune risposte o visualizzare la soluzione alla fine dello scrim.

## Quindi cosa può realmente fare?

Il linguaggio JavaScript lato client di base è costituito da alcune caratteristiche di programmazione comuni che consentono di fare cose come:

- Memorizzare valori utili all'interno delle variabili. Nell'esempio sopra, ad esempio, chiediamo di inserire un nuovo nome e quindi lo immagazziniamo in una variabile chiamata `name`.
- Operazioni su porzioni di testo (note come "stringhe" nella programmazione). Nell'esempio sopra prendiamo la stringa "Player 1: " e la uniamo alla variabile `name` per creare l'etichetta di testo completa, ad esempio, "Player 1: Chris".
- Eseguire codice in risposta a determinati eventi che si verificano su una pagina web. Abbiamo utilizzato un evento di [`click`](/it/docs/Web/API/Element/click_event) nel nostro esempio sopra per rilevare quando l'etichetta viene cliccata e quindi eseguire il codice che aggiorna l'etichetta di testo.
- E molto altro!

Quello che è ancora più eccitante è la funzionalità costruita sopra il linguaggio JavaScript lato client. Le cosiddette **Interfacce di Programmazione delle Applicazioni** (**API**) le forniscono ulteriori superpoteri da utilizzare nel suo codice JavaScript.

Le API sono set di blocchi di codice già pronti che consentono a un sviluppatore di implementare programmi che altrimenti sarebbero difficili o impossibili da implementare.
Fanno lo stesso per la programmazione di quanto i kit di mobili già pronti fanno per la costruzione delle case — è molto più facile prendere pannelli già tagliati e avvitarli insieme per fare una libreria che progettare il tutto da soli, andare a trovare il legno corretto, tagliare tutti i pannelli nella giusta dimensione e forma, trovare le viti della grandezza giusta, e _poi_ metterli insieme per fare una libreria.

In genere, rientrano in due categorie.

![Due categorie di API; API di terze parti sono mostrate a lato del browser e le API del browser sono nel browser](browser.png)

**Le API del browser** sono integrate nel suo browser web e sono in grado di esporre dati dall'ambiente informatico circostante o fare cose complesse utili. Ad esempio:

- L'[API DOM (Document Object Model)](/it/docs/Web/API/Document_Object_Model) consente di manipolare HTML e CSS, creando, rimuovendo e modificando l'HTML, applicando dinamicamente nuovi stili alla sua pagina, ecc.
  Ogni volta che vede una finestra popup apparire su una pagina, o qualche nuovo contenuto visualizzato (come abbiamo visto sopra nel nostro semplice demo) per esempio, quello è il DOM in azione.
- L'[API di Geolocalizzazione](/it/docs/Web/API/Geolocation_API) recupera informazioni geografiche.
  È così che [Google Maps](https://www.google.com/maps) è in grado di trovare la sua posizione e tracciarla su una mappa.
- Le API [Canvas](/it/docs/Web/API/Canvas_API) e [WebGL](/it/docs/Web/API/WebGL_API) le consentono di creare grafica animata 2D e 3D.
  Le persone stanno facendo cose straordinarie utilizzando queste tecnologie web — veda [Chrome Experiments](https://experiments.withgoogle.com/collection/chrome) e [webglsamples](https://webglsamples.org/).
- Le [API Audio e Video](/it/docs/Web/Media/Guides/Audio_and_video_delivery) come [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) e [WebRTC](/it/docs/Web/API/WebRTC_API) permettono di fare cose davvero interessanti con i multimedia, come riprodurre audio e video direttamente in una pagina web, o catturare video dalla sua webcam e visualizzarlo sul computer di qualcun altro (provi il nostro semplice [demo Snapshot](https://chrisdavidmills.github.io/snapshot/) per capire l'idea).

**Le API di terze parti** non sono integrate nel browser per impostazione predefinita e in genere è necessario ottenere il loro codice e informazioni da qualche parte sul Web. Ad esempio:

- L'[API di Bluesky](https://docs.bsky.app/) le consente di fare cose come visualizzare i suoi ultimi post sul suo sito web.
- L'[API di Google Maps](https://developers.google.com/maps/) e l'[API di OpenStreetMap](https://wiki.openstreetmap.org/wiki/API) le consentono di incorporare mappe personalizzate nel suo sito web e altre funzionalità simili.

> [!NOTE]
> Queste API sono avanzate e non tratteremo nessuna di esse in questo modulo. Può scoprirne di più nel nostro modulo [API web lato client](/it/docs/Learn_web_development/Extensions/Client-side_APIs).

C'è molto altro disponibile, anche! Tuttavia, non si ecciti troppo. Non sarà in grado di costruire il prossimo Facebook, Google Maps o Instagram dopo aver studiato JavaScript per 24 ore — ci sono molte basi da coprire prima. Ed è per questo che lei è qui — andiamo avanti!

## Cosa sta facendo JavaScript sulla sua pagina?

Qui inizieremo a guardare effettivamente del codice, e, mentre lo facciamo, esploreremo cosa succede realmente quando esegue del JavaScript sulla sua pagina.

Ripassiamo brevemente la storia di ciò che accade quando carica una pagina web in un browser (di cui si è parlato per la prima volta nel nostro articolo [Cos'è CSS?](/it/docs/Learn_web_development/Core/Styling_basics/What_is_CSS#how_is_css_applied_to_html)). Quando carica una pagina web nel suo browser, sta eseguendo il suo codice (l'HTML, il CSS e il JavaScript) all'interno di un ambiente di esecuzione (la scheda del browser). Questo è come una fabbrica che prende in input materie prime (il codice) e produce un prodotto (la pagina web).

![Codice HTML, CSS e JavaScript si uniscono per creare contenuti nella scheda del browser quando la pagina viene caricata](execution.png)

Un uso molto comune di JavaScript è quello di modificare dinamicamente HTML e CSS per aggiornare un'interfaccia utente, tramite l'API del Document Object Model (come menzionato sopra).

### Sicurezza del browser

Ogni scheda del browser ha il proprio contenitore separato per l'esecuzione del codice (questi contenitori sono chiamati "ambienti di esecuzione" in termini tecnici) — questo significa che, nella maggior parte dei casi, il codice in ciascuna scheda è eseguito completamente separatamente, e il codice in una scheda non può influire direttamente sul codice in un'altra scheda — o su un altro sito web.
Questa è una buona misura di sicurezza — se così non fosse, i pirati potrebbero iniziare a scrivere codice per rubare informazioni da altri siti web, e altre cose brutte del genere.

> [!NOTE]
> Esistono modi per inviare codice e dati tra diversi siti web/schede in modo sicuro, ma queste sono tecniche avanzate che non tratteremo in questo corso.

### Ordine di esecuzione di JavaScript

Quando il browser incontra un blocco di JavaScript, generalmente lo esegue in ordine, dall'alto verso il basso.
Questo significa che deve fare attenzione all'ordine in cui dispone le cose.
Ad esempio, torniamo al blocco di JavaScript che abbiamo visto nel nostro primo esempio:

```js
function updateName() {
  const name = prompt("Enter a new name");
  button.textContent = `Player 1: ${name}`;
}

const button = document.querySelector("button");

button.addEventListener("click", updateName);
```

Qui definiamo per primo un blocco di codice chiamato `updateName()` (questi tipi di blocchi di codice riutilizzabili sono chiamati **funzioni**), che chiede all'utente un nuovo nome e inserisce quel nome nel testo di un pulsante. Successivamente, conserviamo un riferimento a un pulsante utilizzando `document.querySelector` e alleghiamo un listener di eventi utilizzando `addEventListener` in modo che quando il pulsante viene cliccato, la funzione `updateName()` venga eseguita.

Se si invertisse l'ordine delle righe `const button = ...` e `button.addEventListener(...)`, il codice smetterebbe di funzionare — invece, verrebbe restituito un errore nella [console degli sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) — `Uncaught ReferenceError: Cannot access 'button' before initialization`.
Ciò significa che l'oggetto `button` non è ancora stato inizializzato, quindi non possiamo aggiungergli un listener di eventi.

> [!NOTE]
> Non è sempre vero che JavaScript viene eseguito esattamente nell'ordine, dall'alto verso il basso, a causa di comportamenti come il {{Glossary("Hoisting", "hoisting")}}, ma per ora, tenga presente che in genere gli elementi devono essere definiti prima che possa usarli. Questo è una fonte comune di errori.

### Codice interpretato vs compilato

Potrebbe sentire i termini **interpretato** e **compilato** nel contesto della programmazione.
Nei linguaggi interpretati, il codice viene eseguito dall'alto verso il basso e il risultato della sua esecuzione viene immediatamente restituito.
Non deve trasformare il codice in una forma diversa prima che il browser lo esegua.
Il codice viene ricevuto nella sua forma testuale adatta ai programmatori ed elaborato direttamente da lì.

I linguaggi compilati, d'altro canto, vengono trasformati (compilati) in un'altra forma prima di essere eseguiti dal computer.
Ad esempio, C/C++ vengono compilati in codice macchina che viene poi eseguito dal computer.
Il programma è eseguito da un formato binario, che è stato generato dal codice sorgente originale del programma.

JavaScript è un linguaggio di programmazione leggero e interpretato.
Il browser web riceve il codice JavaScript nella sua forma testuale originale ed esegue lo script da quella.
Da un punto di vista tecnico, la maggior parte degli interpreti JavaScript moderni utilizza in realtà una tecnica chiamata **compilazione just-in-time** per migliorare le prestazioni; il codice sorgente JavaScript viene compilato in un formato binario più veloce mentre lo script viene utilizzato, in modo da poter essere eseguito il più rapidamente possibile.
Tuttavia, JavaScript è ancora considerato un linguaggio interpretato, dato che la compilazione viene gestita al momento dell'esecuzione, piuttosto che in anticipo.

Ci sono vantaggi in entrambi i tipi di linguaggio, ma non li discuteremo ora.

### Codice lato server vs lato client

Potrebbe anche sentire i termini **codice lato server** e **codice lato client**, specialmente nel contesto dello sviluppo web.
Il codice lato client è codice eseguito sul computer dell'utente — quando una pagina web viene visualizzata, il codice lato client viene scaricato, quindi eseguito e visualizzato dal browser.
In questo modulo stiamo parlando esplicitamente di **JavaScript lato client**.

Il codice lato server, d'altro canto, viene eseguito sul server, quindi i suoi risultati vengono scaricati e visualizzati nel browser.
Esempi di linguaggi web popolari lato server includono PHP, Python, Ruby, C#, e persino JavaScript!
JavaScript può anche essere usato come linguaggio lato server, ad esempio nell'ambiente popolare Node.js — può scoprire di più su JavaScript lato server nel nostro argomento [Siti web dinamici – Programmazione lato server](/it/docs/Learn_web_development/Extensions/Server-side).

### Codice dinamico vs statico

La parola **dinamico** è usata per descrivere sia JavaScript lato client, che linguaggi lato server — si riferisce alla capacità di aggiornare la visualizzazione di una pagina/app web per mostrare cose diverse in circostanze diverse, generando nuovo contenuto secondo necessità.
Il codice lato server genera dinamicamente nuovo contenuto sul server, ad esempio, estraendo dati da un database, mentre JavaScript lato client genera dinamicamente nuovo contenuto all'interno del browser sul client, ad esempio, creando una nuova tabella HTML, riempiendola con dati richiesti dal server, quindi visualizzando la tabella in una pagina web mostrata all'utente.
Il significato è leggermente diverso nei due contesti, ma correlato, e entrambi gli approcci (lato server e lato client) generalmente lavorano insieme.

Una pagina web senza contenuto che si aggiorna dinamicamente è definita **statica** — mostra sempre lo stesso contenuto.

## Come si aggiunge JavaScript alla pagina?

JavaScript viene applicato alla pagina HTML in modo simile al CSS.
Mentre CSS utilizza gli elementi {{htmlelement("link")}} per applicare stili esterni e gli elementi {{htmlelement("style")}} per applicare stili interni all'HTML, JavaScript ha bisogno di un solo amico nel mondo HTML — l'elemento {{htmlelement("script")}}. Vediamo come funziona.

### JavaScript interno

1. Per prima cosa, faccia una copia locale del nostro file di esempio [apply-javascript.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript.html). Lo salvi in una directory sensata.
2. Apra il file nel suo browser web e nel suo editor di testo. Vedrà che l'HTML crea una semplice pagina web contenente un pulsante cliccabile.
3. Successivamente, vada al suo editor di testo e aggiunga il seguente codice alla fine del suo corpo — proprio prima del tag di chiusura `</body>`:

   ```html
   <script>
     // JavaScript goes here
   </script>
   ```

   Nota che il codice nei suoi documenti web viene generalmente caricato e eseguito nell'ordine in cui appare sulla pagina. Posizionando il JavaScript in fondo, ci assicuriamo che tutti gli elementi HTML siano caricati. (Veda anche [Strategie di caricamento degli script](#strategie_di_caricamento_degli_script) sotto.)

4. Ora aggiungeremo del JavaScript all'interno del nostro elemento {{htmlelement("script")}} per fare in modo che la pagina faccia qualcosa di più interessante — aggiunga il seguente codice appena sotto la riga "// JavaScript goes here":

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

5. Salvi il file e aggiorni il browser — ora dovrebbe vedere che quando fa clic sul pulsante, viene generato un nuovo paragrafo e posizionato sotto.

> [!NOTE]
> Se il suo esempio non sembra funzionare, ripassi i passaggi e controlli che abbia fatto tutto correttamente.
> Ha salvato la sua copia locale del codice iniziale come file `.html`?
> Ha aggiunto il suo elemento {{htmlelement("script")}} appena prima del tag `</body>`?
> Ha inserito il JavaScript esattamente come mostrato? **JavaScript è case sensitive e molto pignolo, quindi deve inserire la sintassi esattamente come mostrato, altrimenti potrebbe non funzionare.**

> [!NOTE]
> Può vedere questa versione su GitHub come [apply-javascript-internal.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html) ([vedila anche live](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html)).

### JavaScript esterno

Questo funziona benissimo, ma cosa succede se volessimo mettere il nostro JavaScript in un file esterno? Esploriamo questo ora.

1. Per prima cosa, crei un nuovo file nella stessa directory del suo file HTML di esempio. Lo chiami `script.js` — si assicuri che abbia quell'estensione di file .js, dato che è così che viene riconosciuto come JavaScript.
2. Rimuova il suo attuale elemento {{htmlelement("script")}} in fondo al `</body>` e aggiunga il seguente codice appena prima del tag di chiusura `</head>` (in modo che il browser possa iniziare a caricare il file prima rispetto a quando è in fondo):

   ```html
   <script type="module" src="script.js"></script>
   ```

3. All'interno di `script.js`, aggiunga il seguente script:

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

4. Salvi e aggiorni il browser. Scoprirà che cliccando sul pulsante non accade nulla, e se controlla la console del browser, vedrà un errore del tipo `Cross-origin request blocked`. Questo perché, come molte risorse esterne, i moduli JavaScript devono essere caricati dall'[origine stessa](/it/docs/Web/Security/Same-origin_policy) dell'HTML, e gli URL `file://` non si qualificano. Ci sono due soluzioni per risolvere questo problema:
   - La nostra soluzione raccomandata è [impostare un server di testing locale](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server). Con il programma server in esecuzione e che serve i file `apply-javascript-external.html` e `script.js` sulla porta `8000`, apra il suo browser e vada su `http://localhost:8000`.
   - Se non può eseguire un server locale, può anche usare `<script defer src="script.js"></script>` invece di `<script type="module" src="script.js"></script>`. Veda [Strategie di caricamento degli script](#strategie_di_caricamento_degli_script) sotto per ulteriori informazioni. Ma tenga presente che le funzionalità che utilizziamo in altre parti del tutorial potrebbero comunque richiedere un server HTTP locale.
5. Ora il sito funziona esattamente come prima, ma ora abbiamo il nostro JavaScript in un file esterno.
   Questo è generalmente un bene in termini di organizzazione del codice e per renderlo riutilizzabile tra molteplici file HTML.
   Inoltre, l'HTML è più leggibile senza enormi blocchi di script.

> [!NOTE]
> Può vedere questa versione su GitHub come [apply-javascript-external.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html) e [script.js](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/script.js) ([vedila anche live](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html)).

### Gestori JavaScript inline

Si noti che talvolta incontrerà frammenti di vero codice JavaScript all'interno di HTML.
Potrebbe sembrare qualcosa di simile a questo:

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

Può provare questa versione del nostro demo qui sotto.

{{ EmbedLiveSample('Inline_JavaScript_handlers', '100%', 150) }}

Questa demo ha esattamente la stessa funzionalità delle due sezioni precedenti, tranne che l'elemento {{htmlelement("button")}} include un gestore `onclick` inline per far eseguire la funzione quando il pulsante viene premuto.

**Tuttavia, per favore, non lo faccia.** È una cattiva pratica contaminare l'HTML con JavaScript ed è inefficace — dovrebbe includere l'attributo `onclick="createParagraph()"` su ogni pulsante a cui desidera applicare il JavaScript.

### Usare addEventListener invece

Invece di includere JavaScript nel suo HTML, utilizzi un costrutto puro JavaScript.
La funzione `querySelectorAll()` le permette di selezionare tutti i pulsanti su una pagina.
Può quindi scorrere i pulsanti, assegnando un gestore per ciascuno usando `addEventListener()`.
Il codice per farlo è mostrato di seguito:

```js
const buttons = document.querySelectorAll("button");

for (const button of buttons) {
  button.addEventListener("click", createParagraph);
}
```

Questo potrebbe essere un po' più lungo dell'attributo `onclick`, ma funzionerà per tutti i pulsanti — non importa quanti siano sulla pagina, né quanti ne siano aggiunti o rimossi.
Il JavaScript non deve essere cambiato.

> [!NOTE]
> Provi a modificare la sua versione di `apply-javascript.html` e aggiunga qualche altro pulsante nel file.
> Quando ricarica, dovrebbe scoprire che tutti i pulsanti, quando cliccati, creeranno un paragrafo.
> Carino, no?

### Strategie di caricamento degli script

Tutto l'HTML di una pagina viene caricato nell'ordine in cui appare.
Se sta usando JavaScript per manipolare elementi della pagina (o più precisamente, del [Document Object Model](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#the_document_object_model)), il suo codice non funzionerà se il JavaScript viene caricato e analizzato prima dell'HTML a cui sta cercando di fare qualcosa.

Ci sono diverse strategie per assicurarsi che il suo JavaScript venga eseguito solo dopo che l'HTML è stato analizzato:

- Nell'esempio di JavaScript interno sopra, l'elemento script è posizionato in fondo al corpo del documento e quindi viene eseguito solo dopo che il resto del corpo HTML è stato analizzato.
- Nell'esempio di JavaScript esterno sopra, l'elemento script è posizionato nell'intestazione del documento, prima che il corpo HTML venga analizzato. Ma poiché stiamo usando `<script type="module">`, il codice viene trattato come un [modulo](/it/docs/Web/JavaScript/Guide/Modules) e il browser attende che tutto l'HTML sia processato prima di eseguire i moduli JavaScript. (Potrebbe anche posizionare script esterni in fondo al corpo. Ma se c'è molto HTML e la rete è lenta, potrebbe richiedere molto tempo prima che il browser possa persino iniziare a recuperare e caricare lo script, quindi in genere è meglio posizionare gli script esterni nell'intestazione.)
- Se desidera ancora utilizzare script non di modulo nell'intestazione del documento, che potrebbero bloccare l'intera pagina dall'essere mostrata e potrebbero causare errori poiché viene eseguito prima che l'HTML venga analizzato:

  - Per gli script esterni, dovrebbe aggiungere l'attributo `defer` (o se non ha bisogno che l'HTML sia pronto, l'attributo `async`) sull'elemento {{htmlelement("script")}}.
  - Per gli script interni, dovrebbe avvolgere il codice in un [listener di eventi `DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event).

  Questo è al di là dell'attuale scopo del tutorial, ma a meno che non debba supportare browser molto vecchi, non deve farlo e può semplicemente utilizzare `<script type="module">` invece.

## Commenti

Come con HTML e CSS, è possibile scrivere commenti nel suo codice JavaScript che verranno ignorati dal browser ed esistono per fornire istruzioni ai suoi colleghi sviluppatori su come funziona il codice (e a lei stesso, se torna al suo codice dopo sei mesi e non ricorda cosa ha fatto).
I commenti sono molto utili e dovrebbero essere usati spesso, specialmente per applicazioni più grandi.
Ci sono due tipi:

- Un commento su una singola riga viene scritto dopo una doppia barra obliqua (`//`), ad esempio.

  ```js
  // I am a comment
  ```

- Un commento su più righe viene scritto tra le stringhe `/*` e `*/`, ad esempio.

  ```js
  /*
    I am also
    a comment
  */
  ```

Quindi, ad esempio, potremmo annotare il JavaScript dell'ultimo demo con commenti in questo modo:

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
> In generale, più commenti sono solitamente meglio che meno, ma dovrebbe fare attenzione se si trova ad aggiungere molti commenti per spiegare cosa sono le variabili (forse i suoi nomi di variabili dovrebbero essere più intuitivi), o per spiegare operazioni molto semplici (forse il suo codice è troppo complicato).

## Sommario

Ecco fatto, il suo primo passo nel mondo di JavaScript.
Abbiamo iniziato solo con la teoria, per cominciare a farle capire perché userebbe JavaScript e che tipo di cose può fare con esso.
Lungo la strada, ha visto alcuni esempi di codice e ha imparato come JavaScript si integra con il resto del codice sul suo sito web, tra le altre cose.

JavaScript potrebbe sembrare un po' scoraggiante in questo momento, ma non si preoccupi — in questo corso la guideremo attraverso semplici passi che avranno senso andando avanti.
Nel prossimo articolo, ci tufferemo direttamente nel pratico, facendola immergere direttamente nella costruzione dei suoi esempi JavaScript.

{{NextMenu("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting")}}
