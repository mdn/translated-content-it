---
title: Dall'oggetto all'iframe — tecnologie di incorporamento generali
short-title: Tecnologie di incorporamento
slug: Learn_web_development/Core/Structuring_content/General_embedding_technologies
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Gli sviluppatori comunemente pensano all'incorporamento di media come immagini, video e audio nelle pagine web. In questo articolo facciamo un passo laterale, osservando alcuni elementi che permettono di incorporare una vasta gamma di tipi di contenuto nelle tue pagine web: gli elementi {{htmlelement("iframe")}}, {{htmlelement("embed")}} e {{htmlelement("object")}}. Gli `<iframe>` servono per incorporare altre pagine web, mentre gli altri due permettono di incorporare risorse esterne come file PDF.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, conoscenza di base di
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >lavorare con i file</a
        >, familiarità con i <a href="/it/docs/Learn_web_development/Core/Structuring_content/"
          >fondamenti di HTML</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a incorporare oggetti nelle pagine web usando
        {{htmlelement("object")}}, {{htmlelement("embed")}}, e
        {{htmlelement("iframe")}}, come documenti PDF e altre pagine web.
      </td>
    </tr>
  </tbody>
</table>

## Una breve storia dell'incorporamento

Tanto tempo fa sul Web, era popolare usare le **cornici** per creare siti web — piccole parti di un sito web memorizzate in pagine HTML individuali. Queste erano incorporate in un documento principale chiamato **frameset**, che permetteva di specificare l'area sullo schermo che ciascuna cornice riempiva, in modo simile a dimensionare le colonne e le righe di una tabella. Queste erano considerate l'apice del "cool" a metà degli anni '90, e c'era evidenza che avere una pagina web suddivisa in piccoli blocchi come questo fosse migliore per la velocità di download — particolarmente evidente con le connessioni di rete così lente all'epoca. Tuttavia, presentavano molti problemi che superavano di gran lunga i vantaggi man mano che le velocità di rete aumentavano, quindi non si vedono più in uso.

Poco dopo (fine anni '90, inizio 2000), le tecnologie plugin divennero molto popolari, come {{Glossary("Java", "Java Applets")}} e {{Glossary("Adobe_Flash", "Flash")}} — queste consentivano agli sviluppatori web di incorporare contenuti ricchi nelle pagine web, come video e animazioni, che semplicemente non erano disponibili attraverso HTML da solo. L'incorporamento di queste tecnologie è stato realizzato tramite elementi come {{htmlelement("object")}}, e il meno usato {{htmlelement("embed")}}, ed erano molto utili all'epoca. Da allora sono caduti in disuso a causa di molti problemi, inclusi accessibilità, sicurezza, dimensione dei file e altro. Al giorno d'oggi i principali browser hanno smesso di supportare plugin come Flash.

Infine, è apparso l'elemento {{htmlelement("iframe")}} (insieme ad altri modi di incorporare contenuti, come {{htmlelement("canvas")}}, {{htmlelement("video")}}, ecc.) Questo fornisce un modo per incorporare un intero documento web all'interno di un altro, come se fosse un {{htmlelement("img")}} o altri elementi simili, ed è usato regolarmente oggi.

Conclusa la lezione di storia, passiamo a vedere come utilizzare alcuni di questi.

## Apprendimento attivo: usi classici dell'incorporamento

In questo articolo entreremo subito in una sezione di apprendimento attivo, per darti immediatamente un'idea reale di quanto siano utili le tecnologie di incorporamento. Il mondo online è molto familiare con [YouTube](https://www.youtube.com/), ma molte persone non conoscono alcune delle funzionalità di condivisione che ha a disposizione. Vediamo come YouTube ci permette di incorporare un video in qualsiasi pagina ci piaccia usando un {{htmlelement("iframe")}}.

1. Innanzitutto, vai su YouTube e trova un video che ti piace.
2. Sotto il video, troverai un pulsante _Condividi_ — selezionalo per visualizzare le opzioni di condivisione.
3. Seleziona il pulsante _Incorpora_ e ti verrà fornito del codice `<iframe>` — copialo.
4. Inseriscilo nella casella _Input_ qui sotto e vedi quale sarà il risultato nella sezione _Output_.

Per punti extra, potresti anche provare a incorporare una [Google Map](https://www.google.com/maps/) nell'esempio:

1. Vai su Google Maps e trova una mappa che ti piace.
2. Clicca sul "Menu Hamburger" (tre linee orizzontali) in alto a sinistra nell'interfaccia utente.
3. Seleziona l'opzione _Condividi o incorpora mappa_.
4. Seleziona l'opzione Incorpora mappa, che ti darà del codice `<iframe>` — copialo.
5. Inseriscilo nella casella _Input_ qui sotto e vedi quale sarà il risultato nella sezione _Output_.

Se fai un errore, puoi sempre resettarlo usando il pulsante _Resetta_. Se sei veramente bloccato, premi il pulsante _Mostra soluzione_ per vedere una risposta.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 250px;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea
  id="code"
  class="input"
  style="width: 95%;min-height: 100px;"></textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css hidden
html {
  font-family: sans-serif;
}

h2 {
  font-size: 16px;
}

.a11y-label {
  margin: 0;
  text-align: right;
  font-size: 0.7rem;
  width: 98%;
}

body {
  margin: 10px;
  background: #f5f9fa;
}
```

```js hidden
const textarea = document.getElementById("code");
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
const output = document.querySelector(".output");
let code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  output.innerHTML = textarea.value;
}

reset.addEventListener("click", function () {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = htmlSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", function () {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

const htmlSolution =
  '<iframe width="420" height="315" src="https://www.youtube.com/embed/QH2-TGUlwu4" frameborder="0" allowfullscreen>\n</iframe>\n\n<iframe src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d37995.65748333395!2d-2.273568166412784!3d53.473310471916975!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x487bae6c05743d3d%3A0xf82fddd1e49fc0a1!2sThe+Lowry!5e0!3m2!1sen!2suk!4v1518171785211" width="600" height="450" frameborder="0" style="border:0" allowfullscreen>\n</iframe>';
let solutionEntry = htmlSolution;

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = function (e) {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
    textarea.blur();
  }
};

function insertAtCaret(text) {
  const scrollPos = textarea.scrollTop;
  let caretPos = textarea.selectionStart;

  const front = textarea.value.substring(0, caretPos);
  const back = textarea.value.substring(
    textarea.selectionEnd,
    textarea.value.length,
  );
  textarea.value = front + text + back;
  caretPos += text.length;
  textarea.selectionStart = caretPos;
  textarea.selectionEnd = caretPos;
  textarea.focus();
  textarea.scrollTop = scrollPos;
}

// Update the saved userCode every time the user updates the text area code

textarea.onkeyup = function () {
  // We only want to save the state when the user code is being shown,
  // not the solution, so that solution is not saved over the user code
  if (solution.value === "Show solution") {
    userEntry = textarea.value;
  } else {
    solutionEntry = textarea.value;
  }

  updateCode();
};
```

{{ EmbedLiveSample('Active_learning_classic_embedding_uses', 700, 600) }}

## Dettagli sugli iframe

Quindi, è stato facile e divertente, giusto? Gli elementi {{htmlelement("iframe")}} sono progettati per permettere di incorporare altri documenti web nel documento corrente. Questo è ottimo per incorporare contenuti di terze parti nel tuo sito web che potresti non avere controllo diretto su e non vuoi implementare una tua versione — come video da provider di video online, sistemi di commento come [Disqus](https://disqus.com/), mappe da provider di mappe online, banner pubblicitari, ecc. Anche gli esempi modificabili dal vivo che hai utilizzato durante questo corso sono implementati usando `<iframe>`.

Prima di approfondire l'uso degli elementi `<iframe>`, ci sono alcune preoccupazioni sulla sicurezza da tenere presente. Supponiamo che tu voglia includere il glossario MDN in una delle tue pagine web utilizzando l'elemento {{htmlelement("iframe")}}, potresti provare qualcosa come il prossimo esempio di codice. Se dovessi aggiungere il codice qui sotto a una delle tue pagine, potresti essere sorpreso di vedere un messaggio di errore invece della pagina del glossario:

```html
<head>
  <style>
    iframe {
      border: none;
    }
  </style>
</head>
<body>
  <iframe
    src="https://developer.mozilla.org/en-US/docs/Glossary"
    width="100%"
    height="500"
    allowfullscreen
    sandbox>
    <p>
      <a href="/en-US/docs/Glossary">
        Fallback link for browsers that don't support iframes
      </a>
    </p>
  </iframe>
</body>
```

Se dai un'occhiata alla console del tuo browser, vedrai un messaggio di errore come il seguente:

```plain
Refused to display 'https://developer.mozilla.org/' in a frame because it set 'X-Frame-Options' to 'deny'.
```

La sezione [Sicurezza](#preoccupazioni_sulla_sicurezza) qui sotto spiega nel dettaglio perché vedi questo errore, ma prima, diamo un'occhiata a cosa sta facendo il nostro codice.

L'esempio include l'essenziale per usare un `<iframe>`:

- [`border: none`](/it/docs/Web/CSS/border)
  - : Se usato, l`<iframe>` viene visualizzato senza un bordo circostante. Altrimenti, per impostazione predefinita, i browser visualizzano l`<iframe>` con un bordo circostante (che generalmente non è desiderabile).
- [`allowfullscreen`](/it/docs/Web/HTML/Reference/Elements/iframe#allowfullscreen)
  - : Se impostato, l`<iframe>` può essere visualizzato a schermo intero utilizzando la [Fullscreen API](/it/docs/Web/API/Fullscreen_API) (leggermente oltre lo scopo di questo articolo).
- [`src`](/it/docs/Web/HTML/Reference/Elements/iframe#src)
  - : Questo attributo, come con {{htmlelement("video")}}/{{htmlelement("img")}}, contiene un percorso che punta all'URL del documento da incorporare.
- [`width`](/it/docs/Web/HTML/Reference/Elements/iframe#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/iframe#height)
  - : Questi attributi specificano la larghezza e l'altezza che vuoi impostare per l`<iframe>`.
- [`sandbox`](/it/docs/Web/HTML/Reference/Elements/iframe#sandbox)
  - : Questo attributo, che funziona su browser leggermente più moderni rispetto al resto delle funzionalità di `<iframe>` (e.g., IE 10 e successivi), richiede impostazioni di sicurezza elevate; diremo di più su questo nella prossima sezione.

> [!NOTE]
> Per migliorare la velocità, è una buona idea impostare l'attributo `src` dell'iframe con JavaScript una volta terminato il caricamento del contenuto principale. Ciò rende la pagina utilizzabile prima e diminuisce il tempo di caricamento ufficiale della pagina (un importante {{Glossary("SEO", "SEO")}} metric).

### Preoccupazioni sulla sicurezza

Sopra abbiamo menzionato le preoccupazioni sulla sicurezza — entriamo ora in questo argomento con maggiori dettagli. Non ci aspettiamo che tu comprenda perfettamente tutto questo contenuto la prima volta; vogliamo solo renderti consapevole di questa preoccupazione e fornire un riferimento a cui tornare man mano che diventi più esperto e inizi a considerare l'uso degli `<iframe>` nei tuoi esperimenti e nel tuo lavoro. Inoltre, non c'è bisogno di avere paura e non usare gli `<iframe>` — devi solo essere cauto. Continua a leggere…

I produttori di browser e gli sviluppatori web hanno appreso a proprie spese che gli iframe sono un bersaglio comune (termine ufficiale: **vettore di attacco**) per i malintenzionati sul Web (spesso definiti **hacker**, o più accuratamente, **cracker**) per attaccare se stanno cercando di modificare malignamente la tua pagina web, o ingannare le persone per fargli fare qualcosa che non vogliono, come rivelare informazioni sensibili come nomi utente e password. Per questo motivo, gli ingegneri delle specifiche e gli sviluppatori di browser hanno sviluppato vari meccanismi di sicurezza per rendere gli `<iframe>` più sicuri, e ci sono anche buone pratiche da considerare — ne copriremo alcune qui sotto.

> **Nota:** [Il Clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) è un tipo di attacco agli iframe comune in cui gli hacker incorporano un iframe invisibile nel tuo documento (o incorporano il tuo documento nel proprio sito web malevolo) e lo usano per catturare le interazioni degli utenti. Questo è un modo comune per fuorviare gli utenti o rubare dati sensibili.

Un rapido esempio prima però — prova a caricare l'esempio precedente che abbiamo mostrato sopra nel tuo browser — puoi [trovarlo live su GitHub](https://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html)). Invece della pagina che ti aspettavi, potresti vedere un messaggio di tipo "Non riesco ad aprire questa pagina", e se guardi la _Console_ negli [strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools), vedrai un messaggio che ti dice perché. In Firefox, ti verrà detto qualcosa del tipo _The loading of "https\://developer.mozilla.org/it/docs/Glossary" in a frame is denied by "X-Frame-Options" directive set to "DENY"_. Questo perché gli sviluppatori che hanno costruito MDN hanno incluso un'impostazione sul server che fornisce le pagine del sito per disabilitarne l'incorporamento all'interno di `<iframe>` (vedi [Configura direttive CSP](#configura_direttive_csp), sotto). Ciò ha senso — una intera pagina MDN non ha davvero senso essere incorporata in altre pagine a meno che tu non voglia fare qualcosa come incorporarle sul tuo sito e dichiararle tue — o tentare di rubare dati tramite [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking), che sono entrambe cose davvero cattive da fare. Inoltre, se tutti iniziassero a fare questo, tutta la larghezza di banda aggiuntiva inizierebbe a costare un sacco di soldi a Mozilla.

#### Incorpora solo quando necessario

A volte ha senso incorporare contenuti di terze parti — come video di YouTube e mappe — ma puoi risparmiarti molti problemi se incorpori contenuti di terze parti solo quando completamente necessario. Una buona regola per la sicurezza web è _"Non puoi mai essere troppo cauto. Se l'hai fatto tu, ricontrollalo comunque. Se l'ha fatto qualcun altro, presumilo pericoloso fino a prova contraria."_

Oltre alla sicurezza, dovresti anche essere consapevole dei problemi di proprietà intellettuale. La maggior parte dei contenuti è protetta da copyright, sia offline che online, anche contenuti che potresti non aspettarti (ad esempio, la maggior parte delle immagini su [Wikimedia Commons](https://commons.wikimedia.org/wiki/Main_Page)). Non visualizzare mai contenuti sulla tua pagina web a meno che tu non li possieda o i proprietari non ti abbiano dato il permesso scritto ed inequivocabile. Le penalità per violazione del copyright sono severe. Ancora una volta, non puoi mai essere troppo cauto.

Se il contenuto è sotto licenza, devi rispettare i termini della licenza. Ad esempio, i contenuti su MDN sono [sotto licenza CC-BY-SA](/it/docs/MDN/Writing_guidelines/Attrib_copyright_license#documentation). Ciò significa che devi [attribuirci correttamente](https://wiki.creativecommons.org/wiki/Best_practices_for_attribution) quando citi i nostri contenuti, anche se apporti modifiche sostanziali.

#### Usa HTTPS

{{Glossary("HTTPS", "HTTPS")}} è la versione criptata di {{Glossary("HTTP", "HTTP")}}. Dovresti servire i tuoi siti web usando HTTPS ogni qual volta possibile:

1. HTTPS riduce la possibilità che il contenuto remoto sia stato manomesso durante il transito.
2. HTTPS impedisce al contenuto incorporato di accedere al contenuto nel tuo documento principale, e viceversa.

Abilitare il tuo sito con HTTPS richiede che un certificato di sicurezza speciale sia installato. Molti provider di hosting offrono hosting abilitato HTTPS senza bisogno che tu faccia alcun setup da solo per mettere in atto un certificato. Ma se _devi_ impostare il supporto HTTPS per il tuo sito da solo, [Let's Encrypt](https://letsencrypt.org/) fornisce strumenti e istruzioni che puoi usare per creare e installare automaticamente il certificato necessario — con supporto integrato per i server web più ampiamente utilizzati, incluso il server web Apache, Nginx e altri. Gli strumenti Let's Encrypt sono progettati per rendere il processo il più semplice possibile, quindi non c'è davvero una buona ragione per evitare di usarlo o altri mezzi disponibili per abilitare HTTPS al tuo sito.

> **Nota:** [GitHub pages](/it/docs/Learn_web_development/Howto/Tools_and_setup/Using_GitHub_pages) consente di servire contenuti tramite HTTPS per impostazione predefinita.
> Se stai usando un provider di hosting diverso dovresti controllare quale supporto forniscono per servire contenuti con HTTPS.

#### Usa sempre l'attributo `sandbox`

Vuoi dare agli attaccanti il minore potere possibile di fare cose cattive sul tuo sito, quindi dovresti dare ai contenuti incorporati _solo i permessi necessari per svolgere il loro lavoro._ Ovviamente, questo vale anche per i tuoi stessi contenuti. Un contenitore per il codice dove può essere utilizzato appropriatamente — o per testare — ma non può causare alcun danno al resto del codice (sia accidentale che malevolo) è chiamato [sandbox](<https://en.wikipedia.org/wiki/Sandbox_(computer_security)>).

I contenuti che non sono sandboxed potrebbero essere in grado di eseguire JavaScript, inviare moduli, avviare finestre popup, ecc. Per impostazione predefinita, dovresti imporre tutte le restrizioni disponibili usando l'attributo `sandbox` senza parametri, come mostrato nel nostro esempio precedente.

Se assolutamente necessario, puoi aggiungere permessi uno alla volta (all'interno del valore dell'attributo `sandbox=""`) — vedi la voce di riferimento [`sandbox`](/it/docs/Web/HTML/Reference/Elements/iframe#sandbox) per tutte le opzioni disponibili. Un punto importante è che non dovresti _mai_ aggiungere sia `allow-scripts` che `allow-same-origin` al tuo attributo `sandbox` — in quel caso, il contenuto incorporato potrebbe bypassare la {{Glossary("Same-origin_policy", "Same-origin policy")}} che impedisce ai siti di eseguire script, e usare JavaScript per disattivare completamente la sandboxing.

> [!NOTE]
> La sandboxing non fornisce protezione se gli attaccanti possono ingannare le persone a visitare contenuti malevoli direttamente (al di fuori di un `iframe`). Se c'è una qualsiasi possibilità che certi contenuti possano essere malevoli (es. contenuto generato dagli utenti), ti preghiamo di servirlo da un {{Glossary("domain", "dominio")}} diverso dal tuo sito principale.

#### Configura direttive CSP

{{Glossary("CSP", "CSP")}} sta per **[content security policy](/it/docs/Web/HTTP/Guides/CSP)** e fornisce [un insieme di intestazioni HTTP](/it/docs/Web/HTTP/Reference/Headers/Content-Security-Policy) (metadati inviati insieme ai tuoi documenti web quando sono serviti da un server web) progettati per migliorare la sicurezza del tuo documento HTML. Quando si tratta di assicurare gli `<iframe>`, puoi _[configurare il tuo server per inviare un'intestazione `X-Frame-Options` appropriata](/it/docs/Web/HTTP/Reference/Headers/X-Frame-Options)_. Questo può prevenire che altri siti web incorporino il tuo contenuto nelle loro pagine web (il che abilita il [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) e una serie di altri attacchi), che è esattamente ciò che gli sviluppatori di MDN hanno fatto, come abbiamo visto prima.

> [!NOTE]
> Puoi leggere il post di Frederik Braun [On the X-Frame-Options Security Header](https://blog.mozilla.org/security/2013/12/12/on-the-x-frame-options-security-header/) per ulteriori informazioni su questo argomento. Ovviamente, è piuttosto fuori ambito per una spiegazione completa in questo articolo.

## Gli elementi \<embed> e \<object>

Gli elementi {{htmlelement("embed")}} e {{htmlelement("object")}} servono una funzione diversa rispetto a {{htmlelement("iframe")}} — questi elementi sono strumenti di incorporamento generali per incorporare contenuti esterni, come i PDF.

Tuttavia, è improbabile che usi molto questi elementi. Se devi visualizzare i PDF, di solito è meglio collegarsi a loro piuttosto che incorporarli nella pagina.

Storicamente, questi elementi sono stati utilizzati anche per incorporare contenuti gestiti dai {{Glossary("Plugin", "plugin")}} del browser come {{Glossary("Adobe_Flash", "Adobe Flash")}}, ma questa tecnologia è ora obsoleta e non è supportata dai browser moderni.

Se ti ritrovi a dover incorporare contenuti di plugin, questo è il tipo di informazioni di cui avrai bisogno, almeno:

<table class="standard-table no-markdown">
  <thead>
    <tr>
      <th scope="col"></th>
      <th scope="col">{{htmlelement("embed")}}</th>
      <th scope="col">{{htmlelement("object")}}</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>{{Glossary("URL", "URL")}} del contenuto incorporato</td>
      <td><a href="/it/docs/Web/HTML/Reference/Elements/embed#src"><code>src</code></a></td>
      <td><a href="/it/docs/Web/HTML/Reference/Elements/object#data"><code>data</code></a></td>
    </tr>
    <tr>
      <td>
        <em>Tipo di media</em> {{Glossary("MIME_type", "preciso")}}
        del contenuto incorporato
      </td>
      <td><a href="/it/docs/Web/HTML/Reference/Elements/embed#type"><code>type</code></a></td>
      <td><a href="/it/docs/Web/HTML/Reference/Elements/object#type"><code>type</code></a></td>
    </tr>
    <tr>
      <td>
        Altezza e larghezza (in pixel CSS) della scatola controllata dal plugin
      </td>
      <td>
         <a href="/it/docs/Web/HTML/Reference/Elements/embed#height"><code>height</code></a><br /><a href="/it/docs/Web/HTML/Reference/Elements/embed#width"><code>width</code></a>
      </td>
      <td>
         <a href="/it/docs/Web/HTML/Reference/Elements/object#height"><code>height</code></a><br /><a href="/it/docs/Web/HTML/Reference/Elements/object#width"><code>width</code></a>
      </td>
    </tr>
    <tr>
      <td>Contenuto HTML indipendente come fallback per una risorsa non disponibile</td>
      <td>Non supportato (<code>&#x3C;noembed></code> è obsoleto)</td>
      <td>
        Contenuto tra i tag di apertura e chiusura
        <code>&#x3C;object></code>
      </td>
    </tr>
  </tbody>
</table>

Vediamo un esempio di `<object>` che incorpora un PDF in una pagina (vedi l'[esempio dal vivo](https://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/object-pdf.html) e il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/other-embedding-technologies/object-pdf.html)):

```html
<object data="my-pdf.pdf" type="application/pdf" width="800" height="1200">
  <p>
    You don't have a PDF plugin, but you can
    <a href="my-pdf.pdf">download the PDF file. </a>
  </p>
</object>
```

I PDF sono stati un passaggio necessario tra carta e digitale, ma pongono molte [sfide di accessibilità](https://webaim.org/techniques/acrobat/acrobat) e possono essere difficili da leggere su schermi piccoli. Tuttavia, tendono ancora a essere popolari in alcuni ambienti, ma è molto meglio collegarsi a loro così che possano essere scaricati o letti su una pagina separata, piuttosto che incorporarli in una pagina web.

## Sommario

L'argomento dell'incorporamento di altri contenuti nei documenti web può diventare rapidamente molto complesso, quindi in questo articolo abbiamo cercato di introdurlo in un modo semplice e familiare che sembrerà immediatamente rilevante, pur accennando ad alcune delle funzionalità più avanzate delle tecnologie coinvolte. All'inizio, è improbabile che tu usi l'incorporamento per molto oltre l'inclusione di contenuti di terze parti come mappe e video sulle tue pagine. Man mano che diventi più esperto, tuttavia, è probabile che inizi a trovare più usi per loro.

Ci sono molte altre tecnologie che coinvolgono l'incorporamento di contenuti esterni oltre a quelle discusse qui. Ne abbiamo viste alcune in articoli precedenti, come {{htmlelement("video")}}, {{htmlelement("audio")}}, e {{htmlelement("img")}}, ma ci sono altri da scoprire, come {{htmlelement("canvas")}} per la generazione di grafica 2D e 3D con JavaScript, e {{SVGElement("svg")}} per incorporare grafica vettoriale.
