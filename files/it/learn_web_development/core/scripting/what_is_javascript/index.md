---
title: Che cos'è JavaScript?
slug: Learn_web_development/Core/Scripting/What_is_JavaScript
l10n:
  sourceCommit: f4c14731a1a157fc8d8f7357ac4d74d14a7d7fb5
---

{{NextMenu("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting")}}

Benvenuti al corso JavaScript per principianti di MDN!
In questo articolo verrà esaminato JavaScript a un livello generale, rispondendo a domande come «Che cos'è?» e «Che cosa si può fare con esso?», e chiarendo lo scopo di JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Che cos'è JavaScript e come si integra in un sito web.</li>
          <li>Che cosa si può fare con JavaScript.</li>
          <li>Aggiungere JavaScript a una pagina web.</li>
          <li>Scrivere commenti all'interno di JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Una definizione generale

JavaScript è un linguaggio di scripting o di programmazione che consente di implementare funzionalità complesse nelle pagine web: ogni volta che una pagina web fa più che restare ferma a mostrare informazioni statiche — visualizzando aggiornamenti tempestivi dei contenuti, mappe interattive, grafica 2D/3D animata, jukebox video a scorrimento e così via — è probabile che JavaScript sia coinvolto.
Rappresenta il terzo strato della torta a strati delle tecnologie web standard, due delle quali ([HTML](/it/docs/Learn_web_development/Core/Structuring_content) e [CSS](/it/docs/Learn_web_development/Core/Styling_basics)) sono state trattate molto più nel dettaglio in altre parti dell'area di apprendimento.

![I tre livelli delle tecnologie web standard: HTML, CSS e JavaScript](cake.png)

- {{Glossary("HTML", "HTML")}} è il linguaggio di markup usato per strutturare e dare significato ai contenuti web, ad esempio definendo paragrafi, intestazioni e tabelle di dati, oppure incorporando immagini e video nella pagina.
- {{Glossary("CSS", "CSS")}} è un linguaggio di regole di stile usato per applicare stili ai contenuti HTML, ad esempio impostando colori di sfondo e font, e disponendo i contenuti in più colonne.
- {{Glossary("JavaScript", "JavaScript")}} è un linguaggio di scripting che consente di creare contenuti aggiornati dinamicamente, controllare contenuti multimediali, animare immagini e praticamente qualsiasi altra cosa. (Be', non proprio tutto, ma è sorprendente ciò che si può ottenere con poche righe di codice JavaScript.)

I tre strati si basano bene l'uno sull'altro. Prendiamo come esempio un pulsante. È possibile definirlo con HTML per dargli struttura e scopo:

```css hidden live-sample___string-concat-name-html live-sample___string-concat-name-css live-sample___string-concat-name-js
html {
  height: 100%;
}

body {
  height: inherit;
  display: flex;
  align-items: center;
  justify-content: center;
}

button {
  font-size: 1.4em;
}
```

```html live-sample___string-concat-name-html live-sample___string-concat-name-css live-sample___string-concat-name-js
<button>Player 1: Chris</button>
```

{{EmbedLiveSample('string-concat-name-html', , '80')}}

Quindi è possibile aggiungere del CSS per renderlo gradevole:

```css live-sample___string-concat-name-css live-sample___string-concat-name-js
button {
  font-family: "Helvetica Neue", "Helvetica", sans-serif;
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

{{EmbedLiveSample('string-concat-name-css', , '80')}}

Infine, è possibile aggiungere JavaScript per implementare un comportamento dinamico:

```js live-sample___string-concat-name-js
function updateName() {
  const name = prompt("Enter a new name");
  button.textContent = `Player 1: ${name}`;
}

const button = document.querySelector("button");

button.addEventListener("click", updateName);
```

Provare a fare clic sull'etichetta di testo, inserire un nome nella finestra di dialogo che si apre e premere il pulsante OK.

{{EmbedLiveSample('string-concat-name-js', , '80', , , , , 'allow-modals')}}

JavaScript può fare molto di più: esploriamo più nel dettaglio che cosa.

> [!NOTE]
> Prima di proseguire, perché non cimentarsi subito in una sfida di Scrimba? Consultare [Render a welcome message](https://scrimba.com/learn-javascript-c0v/~0n?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>. Se non si sa come scrivere questo codice, non c'è alcun problema; si può provare a effettuare ricerche sul web per trovare alcune risposte oppure visualizzare la soluzione alla fine dello scrim.

## Che cosa può fare davvero?

Il linguaggio JavaScript lato client di base è costituito da alcune funzionalità di programmazione comuni che consentono di fare cose come:

- Memorizzare valori utili all'interno di variabili. Nell'esempio precedente, ad esempio, viene richiesto l'inserimento di un nuovo nome, che viene poi memorizzato in una variabile chiamata `name`.
- Eseguire operazioni su porzioni di testo (note in programmazione come "stringhe"). Nell'esempio precedente, viene presa la stringa "Player 1: " e unita alla variabile `name` per creare l'etichetta di testo completa, ad esempio "Player 1: Chris".
- Eseguire codice in risposta al verificarsi di determinati eventi in una pagina web. Nell'esempio precedente è stato usato un evento [`click`](/it/docs/Web/API/Element/click_event) per rilevare quando viene fatto clic sul pulsante ed eseguire quindi il codice che aggiorna l'etichetta di testo.
- E molto altro ancora!

Ciò che è ancora più interessante, tuttavia, è la funzionalità costruita sopra il linguaggio JavaScript lato client. Le cosiddette **Application Programming Interfaces** (**API**) forniscono superpoteri aggiuntivi da usare nel codice JavaScript.

Le API sono insiemi già pronti di blocchi di codice che consentono allo sviluppatore di implementare programmi che altrimenti sarebbero difficili o impossibili da implementare.
Fanno per la programmazione ciò che i kit di mobili già pronti fanno per l'arredamento della casa: è molto più facile prendere pannelli già tagliati e avvitarli insieme per realizzare una libreria piuttosto che progettare tutto da soli, trovare il legno corretto, tagliare tutti i pannelli nelle dimensioni e forme giuste, trovare viti della dimensione corretta e _poi_ assemblare tutto per realizzare una libreria.

In genere rientrano in due categorie.

![Due categorie di API; le API di terze parti sono mostrate accanto al browser e le API del browser sono nel browser](browser.png)

Le **API del browser** sono integrate nel browser web e sono in grado di esporre dati dall'ambiente informatico circostante o di eseguire operazioni complesse utili. Ad esempio:

- L'[API DOM (Document Object Model)](/it/docs/Web/API/Document_Object_Model) consente di manipolare HTML e CSS, creando, rimuovendo e modificando HTML, applicando dinamicamente nuovi stili alla pagina e così via.
  Ogni volta che si vede comparire una finestra popup su una pagina, oppure viene visualizzato un nuovo contenuto (come visto nella semplice demo precedente), è il DOM in azione.
- L'[API Geolocation](/it/docs/Web/API/Geolocation_API) recupera informazioni geografiche.
  È così che [Google Maps](https://www.google.com/maps) riesce a trovare la posizione e a rappresentarla su una mappa.
- Le API [Canvas](/it/docs/Web/API/Canvas_API) e [WebGL](/it/docs/Web/API/WebGL_API) consentono di creare grafica 2D e 3D animata.
  Con queste tecnologie web vengono realizzate cose sorprendenti: vedere [Chrome Experiments](https://experiments.withgoogle.com/collection/chrome) e [webglsamples](https://webglsamples.org/).
- Le [API audio e video](/it/docs/Web/Media/Guides/Audio_and_video_delivery), come [`HTMLMediaElement`](/it/docs/Web/API/HTMLMediaElement) e [WebRTC](/it/docs/Web/API/WebRTC_API), consentono di fare cose davvero interessanti con i contenuti multimediali, come riprodurre audio e video direttamente in una pagina web, oppure acquisire video dalla webcam e mostrarlo sul computer di un'altra persona (provare la semplice [demo Snapshot](https://chrisdavidmills.github.io/snapshot/) per comprenderne l'idea).

Le **API di terze parti** non sono integrate nel browser per impostazione predefinita e in genere occorre ottenere il relativo codice e le informazioni da qualche parte sul Web. Ad esempio:

- L'[API Bluesky](https://bsky.network/) consente di fare cose come visualizzare gli ultimi post sul proprio sito web.
- L'[API Google Maps](https://developers.google.com/maps/) e l'[API OpenStreetMap](https://wiki.openstreetmap.org/wiki/API) consentono di incorporare mappe personalizzate nel proprio sito web e offrono altre funzionalità simili.

> [!NOTE]
> Queste API sono avanzate e non verranno trattate in questo modulo. È possibile approfondirle nel nostro [modulo sulle API web lato client](/it/docs/Learn_web_development/Extensions/Client-side_APIs).

C'è molto altro disponibile! Tuttavia, non bisogna entusiasmarti troppo presto. Non sarà possibile costruire il prossimo Facebook, Google Maps o Instagram dopo aver studiato JavaScript per 24 ore: prima ci sono molte basi da trattare. Ed è per questo che si è qui: proseguiamo!

## Che cosa fa JavaScript nella pagina?

Qui inizieremo effettivamente a esaminare del codice e, nel farlo, esploreremo ciò che accade realmente quando si esegue JavaScript nella pagina.

Ricapitoliamo brevemente ciò che accade quando si carica una pagina web in un browser (argomento trattato per la prima volta nell'articolo [Che cos'è CSS?](/it/docs/Learn_web_development/Core/Styling_basics/What_is_CSS#how_is_css_applied_to_html)). Quando si carica una pagina web nel browser, il codice (HTML, CSS e JavaScript) viene eseguito all'interno di un ambiente di esecuzione (la scheda del browser). È come una fabbrica che riceve materie prime (il codice) e produce un prodotto (la pagina web).

![Il codice HTML, CSS e JavaScript si unisce per creare il contenuto nella scheda del browser quando la pagina viene caricata](execution.png)

Un uso molto comune di JavaScript consiste nel modificare dinamicamente HTML e CSS per aggiornare un'interfaccia utente, tramite l'API Document Object Model (come menzionato sopra).

### Sicurezza del browser

Ogni scheda del browser dispone di un proprio contenitore separato in cui eseguire il codice (in termini tecnici questi contenitori sono chiamati "ambienti di esecuzione"): ciò significa che nella maggior parte dei casi il codice in ogni scheda viene eseguito completamente separatamente e il codice in una scheda non può influire direttamente sul codice in un'altra scheda o in un altro sito web.
Questa è una buona misura di sicurezza: se non fosse così, i pirati potrebbero iniziare a scrivere codice per rubare informazioni da altri siti web e compiere altre azioni dannose.

> [!NOTE]
> Esistono modi per inviare codice e dati tra siti web o schede diverse in modo sicuro, ma si tratta di tecniche avanzate che non verranno trattate in questo corso.

### Ordine di esecuzione di JavaScript

Quando il browser incontra un blocco di JavaScript, in genere lo esegue nell'ordine in cui appare, dall'alto verso il basso.
Ciò significa che occorre prestare attenzione all'ordine in cui vengono inserite le varie istruzioni.
Ad esempio, torniamo al blocco JavaScript visto nel primo esempio:

```js
function updateName() {
  const name = prompt("Enter a new name");
  button.textContent = `Player 1: ${name}`;
}

const button = document.querySelector("button");

button.addEventListener("click", updateName);
```

Qui viene prima definito un blocco di codice chiamato `updateName()` (questi tipi di blocchi di codice riutilizzabili sono chiamati **funzioni**), che chiede all'utente un nuovo nome e inserisce quel nome nel testo di un pulsante. Viene quindi memorizzato un riferimento a un pulsante mediante `document.querySelector` e viene collegato a esso un event listener mediante `addEventListener`, in modo che, quando si fa clic sul pulsante, venga eseguita la funzione `updateName()`.

Se si invertisse l'ordine delle righe `const button = ...` e `button.addEventListener(...)`, il codice non funzionerebbe più: verrebbe invece restituito un errore nella [console degli strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools): `Uncaught ReferenceError: Cannot access 'button' before initialization`.
Ciò significa che l'oggetto `button` non è ancora stato inizializzato, quindi non è possibile aggiungervi un event listener.

> [!NOTE]
> Non è sempre vero che JavaScript viene eseguito esattamente dall'alto verso il basso, a causa di comportamenti come l'{{Glossary("Hoisting", "hoisting")}}, ma per ora è importante ricordare che in genere gli elementi devono essere definiti prima di poterli usare. Questa è una fonte comune di errori.

### Codice interpretato rispetto a codice compilato

Nel contesto della programmazione si potrebbero sentire i termini **interpretato** e **compilato**.
Nei linguaggi interpretati, il codice viene eseguito dall'alto verso il basso e il risultato dell'esecuzione viene restituito immediatamente.
Non è necessario trasformare il codice in una forma diversa prima che il browser lo esegua.
Il codice viene ricevuto nella sua forma testuale adatta ai programmatori ed elaborato direttamente da essa.

I linguaggi compilati, invece, vengono trasformati (compilati) in un'altra forma prima di essere eseguiti dal computer.
Ad esempio, C/C++ viene compilato in codice macchina, che viene poi eseguito dal computer.
Il programma viene eseguito da un formato binario generato dal codice sorgente originale del programma.

JavaScript è un linguaggio di programmazione interpretato leggero.
Il browser web riceve il codice JavaScript nella sua forma testuale originale ed esegue lo script a partire da essa.
Da un punto di vista tecnico, la maggior parte degli interpreti JavaScript moderni utilizza in realtà una tecnica chiamata **just-in-time compiling** per migliorare le prestazioni: il codice sorgente JavaScript viene compilato in un formato binario più veloce mentre lo script viene utilizzato, affinché possa essere eseguito il più rapidamente possibile.
Tuttavia, JavaScript è ancora considerato un linguaggio interpretato, poiché la compilazione viene gestita in fase di esecuzione, anziché in anticipo.

Entrambi i tipi di linguaggio hanno vantaggi, ma non verranno discussi ora.

### Codice lato server rispetto a codice lato client

Si potrebbero anche sentire i termini codice **lato server** e **lato client**, specialmente nel contesto dello sviluppo web.
Il codice lato client è il codice eseguito sul computer dell'utente: quando viene visualizzata una pagina web, il codice lato client della pagina viene scaricato, quindi eseguito e visualizzato dal browser.
In questo modulo si parla esplicitamente di **JavaScript lato client**.

Il codice lato server, invece, viene eseguito sul server, quindi i suoi risultati vengono scaricati e visualizzati nel browser.
Esempi di linguaggi web lato server popolari includono PHP, Python, Ruby, C# e perfino JavaScript!
JavaScript può anche essere usato come linguaggio lato server, ad esempio nel popolare ambiente Node.js: è possibile approfondire JavaScript lato server nell'argomento [Siti web dinamici – Programmazione lato server](/it/docs/Learn_web_development/Extensions/Server-side).

### Codice dinamico rispetto a statico

La parola **dinamico** viene usata per descrivere sia JavaScript lato client sia i linguaggi lato server: si riferisce alla capacità di aggiornare la visualizzazione di una pagina web/app per mostrare cose diverse in circostanze diverse, generando nuovi contenuti secondo necessità.
Il codice lato server genera dinamicamente nuovi contenuti sul server, ad esempio recuperando dati da un database, mentre JavaScript lato client genera dinamicamente nuovi contenuti nel browser sul client, ad esempio creando una nuova tabella HTML, riempiendola con dati richiesti al server e visualizzando quindi la tabella in una pagina web mostrata all'utente.
Il significato è leggermente diverso nei due contesti, ma correlato, e i due approcci (lato server e lato client) di solito lavorano insieme.

Una pagina web senza contenuti che si aggiornano dinamicamente viene definita **statica**: mostra semplicemente sempre lo stesso contenuto.

## Come si aggiunge JavaScript alla pagina?

JavaScript viene applicato alla pagina HTML in modo simile a CSS.
Mentre CSS usa elementi {{htmlelement("link")}} per applicare fogli di stile esterni ed elementi {{htmlelement("style")}} per applicare fogli di stile interni a HTML, JavaScript ha bisogno di un solo alleato nel mondo di HTML: l'elemento {{htmlelement("script")}}. Vediamo come funziona.

> [!NOTE]
> Il tutorial interattivo di Scrimba [Setting up our JavaScript file](https://scrimba.com/learn-javascript-c0v/~03?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> illustra diversi modi per aggiungere JavaScript al proprio HTML.

### JavaScript interno

1. Prima di tutto, creare una copia locale del file di esempio [apply-javascript.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript.html). Salvarla in una directory appropriata.
2. Aprire il file nel browser web e nell'editor di testo. Si vedrà che l'HTML crea una semplice pagina web contenente un pulsante selezionabile.
3. Quindi, nell'editor di testo, aggiungere quanto segue alla fine del body, appena prima del tag di chiusura `</body>`:

   ```html
   <script>
     // JavaScript goes here
   </script>
   ```

   Notare che il codice nei documenti web viene generalmente caricato ed eseguito nell'ordine in cui appare nella pagina. Posizionando JavaScript in fondo, si garantisce che tutti gli elementi HTML siano caricati. (Vedere anche [Strategie di caricamento degli script](#strategie_di_caricamento_degli_script) più avanti.)

4. Ora verrà aggiunto del JavaScript all'interno dell'elemento {{htmlelement("script")}} per rendere la pagina più interessante: aggiungere il codice seguente subito sotto la riga "// JavaScript goes here":

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

5. Salvare il file e aggiornare il browser: ora, quando si fa clic sul pulsante, dovrebbe essere generato un nuovo paragrafo e inserito sotto di esso.

> [!NOTE]
> Se l'esempio non sembra funzionare, ripercorrere i passaggi e verificare che tutto sia stato eseguito correttamente.
> La copia locale del codice iniziale è stata salvata come file `.html`?
> L'elemento {{htmlelement("script")}} è stato aggiunto appena prima del tag `</body>`?
> Il codice JavaScript è stato inserito esattamente come mostrato? **JavaScript distingue tra maiuscole e minuscole ed è molto rigoroso, quindi occorre inserire la sintassi esattamente come mostrato, altrimenti potrebbe non funzionare.**

> [!NOTE]
> Questa versione è disponibile su GitHub come [apply-javascript-internal.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html) ([vederla anche in esecuzione](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html)).

### JavaScript esterno

Funziona bene, ma cosa accadrebbe se si volesse inserire JavaScript in un file esterno? Esploriamolo ora.

1. Per prima cosa, creare un nuovo file nella stessa directory del file HTML di esempio. Chiamarlo `script.js`: assicurarsi che abbia l'estensione .js, poiché è così che viene riconosciuto come JavaScript.
2. Rimuovere l'elemento {{htmlelement("script")}} corrente alla fine di `</body>` e aggiungere quanto segue appena prima del tag di chiusura `</head>` (in questo modo il browser può iniziare a caricare il file prima rispetto a quando si trova in fondo):

   ```html
   <script type="module" src="script.js"></script>
   ```

3. All'interno di `script.js`, aggiungere lo script seguente:

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

4. Salvare e aggiornare il browser. Si scoprirà che fare clic sul pulsante non ha effetto e, controllando la console del browser, verrà visualizzato un errore simile a `Cross-origin request blocked`. Questo perché, come molte risorse esterne, i moduli JavaScript devono essere caricati dalla [stessa origine](/it/docs/Web/Security/Defenses/Same-origin_policy) dell'HTML e gli URL `file://` non soddisfano questo requisito. Esistono due soluzioni per risolvere il problema:
   - La soluzione consigliata consiste nel [configurare un server di test locale](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server). Con il programma server in esecuzione che serve i file `apply-javascript-external.html` e `script.js` sulla porta `8000`, aprire il browser e andare a `http://localhost:8000`.
   - Se non è possibile eseguire un server locale, si può anche usare `<script defer src="script.js"></script>` invece di `<script type="module" src="script.js"></script>`. Vedere [Strategie di caricamento degli script](#strategie_di_caricamento_degli_script) più avanti per ulteriori informazioni. Tuttavia, tenere presente che le funzionalità usate in altre parti del tutorial potrebbero comunque richiedere un server HTTP locale.
5. Ora il sito web funziona esattamente come prima, ma JavaScript si trova in un file esterno.
   In genere questa è una buona soluzione per organizzare il codice e renderlo riutilizzabile in più file HTML.
   Inoltre, l'HTML è più facile da leggere senza enormi blocchi di script inseriti al suo interno.

> [!NOTE]
> Questa versione è disponibile su GitHub come [apply-javascript-external.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html) e [script.js](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/what-is-js/script.js) ([vederla anche in esecuzione](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html)).

### Gestori JavaScript inline

Notare che talvolta si incontrano frammenti di codice JavaScript effettivo all'interno dell'HTML.
Potrebbero avere un aspetto simile al seguente:

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

È possibile provare questa versione della demo qui sotto.

{{ EmbedLiveSample('Inline_JavaScript_handlers', '100%', 150) }}

Questa demo ha esattamente la stessa funzionalità delle due sezioni precedenti, tranne per il fatto che l'elemento {{htmlelement("button")}} include un gestore `onclick` inline per eseguire la funzione quando viene premuto il pulsante.

**Tuttavia, non farlo.** È una cattiva pratica inquinare l'HTML con JavaScript ed è inefficiente: bisognerebbe includere l'attributo `onclick="createParagraph()"` in ogni pulsante a cui si desidera applicare JavaScript.

### Usare invece addEventListener

Invece di includere JavaScript nell'HTML, usare un costrutto JavaScript puro.
La funzione `querySelectorAll()` consente di selezionare tutti i pulsanti in una pagina.
È quindi possibile iterare sui pulsanti assegnando un gestore a ciascuno mediante `addEventListener()`.
Il codice per farlo è mostrato di seguito:

```js
const buttons = document.querySelectorAll("button");

for (const button of buttons) {
  button.addEventListener("click", createParagraph);
}
```

Potrebbe essere un po' più lungo dell'attributo `onclick`, ma funzionerà per tutti i pulsanti, indipendentemente da quanti ce ne siano nella pagina o da quanti vengano aggiunti o rimossi.
Non è necessario modificare JavaScript.

> [!NOTE]
> Provare a modificare la propria versione di `apply-javascript.html` e aggiungere qualche altro pulsante nel file.
> Al ricaricamento, tutti i pulsanti dovrebbero creare un paragrafo quando vengono selezionati.
> Bello, vero?

### Strategie di caricamento degli script

Tutto l'HTML di una pagina viene caricato nell'ordine in cui appare.
Se si usa JavaScript per manipolare gli elementi della pagina (o, più precisamente, il [Document Object Model](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting#the_document_object_model)), il codice non funzionerà se JavaScript viene caricato e analizzato prima dell'HTML su cui si sta cercando di operare.

Esistono diverse strategie per assicurarsi che JavaScript venga eseguito solo dopo che l'HTML è stato analizzato:

- Nell'esempio di JavaScript interno sopra, l'elemento script è collocato alla fine del body del documento e viene quindi eseguito solo dopo che il resto del body HTML è stato analizzato.
- Nell'esempio di JavaScript esterno sopra, l'elemento script è collocato nell'head del documento, prima che il body HTML venga analizzato. Tuttavia, poiché viene usato `<script type="module">`, il codice viene trattato come un [modulo](/it/docs/Web/JavaScript/Guide/Modules) e il browser attende che tutto l'HTML venga elaborato prima di eseguire i moduli JavaScript. (Si potrebbero anche inserire gli script esterni alla fine del body. Tuttavia, se c'è molto HTML e la rete è lenta, potrebbe trascorrere molto tempo prima che il browser possa persino iniziare a recuperare e caricare lo script, perciò in genere è preferibile inserire gli script esterni nell'head.)
- Se si desidera ancora usare script non modulari nell'head del documento, che potrebbero bloccare la visualizzazione dell'intera pagina e causare errori perché vengono eseguiti prima che l'HTML sia stato analizzato:
  - Per gli script esterni, aggiungere l'attributo `defer` (oppure `async`, se non è necessario che l'HTML sia pronto) all'elemento {{htmlelement("script")}}.
  - Per gli script interni, racchiudere il codice in un [event listener `DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event).

  Questo va oltre lo scopo del tutorial a questo punto, ma a meno che non sia necessario supportare browser molto vecchi, non occorre farlo e si può semplicemente usare `<script type="module">`.

## Commenti

Come per HTML e CSS, è possibile scrivere commenti nel codice JavaScript che verranno ignorati dal browser e che servono a fornire istruzioni agli altri sviluppatori su come funziona il codice (e anche a chi scrive il codice, se vi torna dopo sei mesi e non ricorda più cosa aveva fatto).
I commenti sono molto utili e dovrebbero essere usati spesso, in particolare per applicazioni più grandi.
Esistono due tipi:

- Un commento su una sola riga viene scritto dopo una doppia barra (`//`), ad esempio:

  ```js
  // I am a comment
  ```

- Un commento su più righe viene scritto tra le stringhe `/*` e `*/`, ad esempio:

  ```js
  /*
    I am also
    a comment
  */
  ```

Quindi, ad esempio, si potrebbe annotare il JavaScript dell'ultima demo con commenti nel modo seguente:

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
> In generale, più commenti sono spesso meglio di meno commenti, ma occorre fare attenzione se si aggiungono molti commenti per spiegare che cosa sono le variabili (forse i nomi delle variabili dovrebbero essere più intuitivi) o per spiegare operazioni molto semplici (forse il codice è eccessivamente complicato).

## Riepilogo

Ecco quindi il primo passo nel mondo di JavaScript.
Si è iniziato soltanto con la teoria, per acquisire familiarità con il motivo per cui si userebbe JavaScript e con il tipo di cose che è possibile fare con esso.
Durante il percorso sono stati mostrati alcuni esempi di codice ed è stato appreso, tra le altre cose, come JavaScript si integra con il resto del codice del sito web.

JavaScript potrebbe sembrare un po' scoraggiante in questo momento, ma non c'è da preoccuparsi: in questo corso verrà affrontato in semplici passaggi che avranno sempre più senso man mano che si prosegue.
Nel prossimo articolo si passerà direttamente alla pratica, iniziando a costruire esempi JavaScript personali.

{{NextMenu("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting")}}
