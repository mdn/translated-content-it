---
title: Da oggetto a iframe — tecnologie generali di incorporamento
short-title: Tecnologie di incorporamento
slug: Learn_web_development/Core/Structuring_content/General_embedding_technologies
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Gli sviluppatori spesso pensano all'incorporare media come immagini, video e audio nelle pagine web. In questo articolo facciamo un passo di lato, esaminando alcuni elementi che permettono di incorporare una vasta gamma di tipi di contenuti nelle vostre pagine web: gli elementi {{htmlelement("iframe")}}, {{htmlelement("embed")}} e {{htmlelement("object")}}. Gli `<iframe>` sono usati per incorporare altre pagine web, mentre gli altri due permettono di incorporare risorse esterne come file PDF.

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
        >, familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content/"
          >fondamenti di HTML</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare come incorporare elementi nelle pagine web utilizzando
        {{htmlelement("object")}}, {{htmlelement("embed")}} e
        {{htmlelement("iframe")}}, come documenti PDF e altre pagine web.
      </td>
    </tr>
  </tbody>
</table>

## Una breve storia dell'incorporamento

Tanto tempo fa sul Web, era comune usare i **frames** per creare siti web — piccole parti di un sito web memorizzate in singole pagine HTML. Queste erano incorporate in un documento principale chiamato **frameset**, che permetteva di specificare l'area sullo schermo che ogni frame riempiva, similmente a ridimensionare colonne e righe di una tabella. Questi erano considerati il massimo della modernità nella metà degli anni '90, e c'era evidenza che avere una pagina web suddivisa in piccoli blocchi migliorasse la velocità di download — soprattutto evidente con le connessioni di rete che erano così lente all'epoca. Tuttavia, avevano molti problemi che superavano eventuali lati positivi quando le velocità di rete diventarono più rapide, quindi non si vedono più in uso.

Un po' più tardi (fine anni '90, inizio 2000), divennero molto popolari le tecnologie di plugin, come {{Glossary("Java", "Applet Java")}} e {{Glossary("Adobe_Flash", "Flash")}} — queste permettevano agli sviluppatori web di incorporare contenuti ricchi nelle pagine web come video e animazioni, che semplicemente non erano disponibili attraverso HTML da solo. L'incorporamento di queste tecnologie si realizzava attraverso elementi come {{htmlelement("object")}}, e il meno utilizzato {{htmlelement("embed")}}, ed erano molto utili all'epoca. Tuttavia, sono caduti in disuso a causa di molti problemi, inclusi accessibilità, sicurezza, dimensione dei file, e altro ancora. Oggi i principali browser hanno smesso di supportare plugin come Flash.

Infine, apparve l'elemento {{htmlelement("iframe")}} (insieme ad altri modi per incorporare contenuti, quali {{htmlelement("canvas")}}, {{htmlelement("video")}}, ecc.) Questo fornisce un modo per incorporare un intero documento web all'interno di un altro, come se fosse un {{htmlelement("img")}} o altro elemento simile, ed è usato regolarmente oggi.

Con la lezione di storia fuori dai piedi, passiamo oltre e vediamo come usare alcuni di questi.

## Apprendimento attivo: usi classici dell'incorporamento

In questo articolo ci tuffiamo direttamente in una sezione di apprendimento attivo, per darvi subito un'idea reale di quanto siano utili le tecnologie di incorporamento. Il mondo online è molto familiare con [YouTube](https://www.youtube.com/), ma molte persone non conoscono alcune delle funzioni di condivisione disponibili. Vediamo come YouTube ci permette di incorporare un video in qualsiasi pagina desideriamo usando un {{htmlelement("iframe")}}.

1. Innanzitutto, andate su YouTube e trovate un video che vi piace.
2. Sotto il video, troverete un pulsante _Condividi_ — selezionatelo per visualizzare le opzioni di condivisione.
3. Selezionate il bottone _Incorpora_ e vi verrà fornito del codice `<iframe>` — copiatelo.
4. Inseritelo nella casella _Input_ qui sotto, e vedete quale sarà il risultato nella casella _Output_.

Per punti extra, potreste provare anche a incorporare una [Mappa di Google](https://www.google.com/maps/) nell'esempio:

1. Andate su Google Maps e trovate una mappa che vi piace.
2. Cliccate sul "Menu Hamburger" (tre linee orizzontali) in alto a sinistra dell'interfaccia utente.
3. Selezionate l'opzione _Condividi o incorpora mappa_.
4. Selezionate l'opzione Incorpora mappa, vi darà del codice `<iframe>` — copiatelo.
5. Inseritelo nella casella _Input_ qui sotto, e vedete quale sarà il risultato nella casella _Output_.

Se fate un errore, potete sempre resettarlo usando il pulsante _Reset_. Se siete davvero bloccatі, premete il pulsante _Mostra soluzione_ per vedere una risposta.

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

## iframes in dettaglio

È stato facile e divertente, vero? Gli elementi {{htmlelement("iframe")}} sono progettati per permettere di incorporare altri documenti web nel documento corrente. Questo è ottimo per incorporare contenuti di terzi nel vostro sito web su cui potete non avere un controllo diretto e che non volete dover implementare per conto vostro — come video da fornitori di video online, sistemi di commenti come [Disqus](https://disqus.com/), mappe da fornitori di mappe online, banner pubblicitari, ecc. Anche gli esempi modificabili live che avete utilizzato attraverso questo corso sono implementati usando `<iframe>`.

Prima di immergersi nell'uso degli elementi `<iframe>`, ci sono alcune preoccupazioni di sicurezza di cui essere a conoscenza.
Diciamo che volegliate includere il glossario MDN in una delle vostre pagine web usando l'elemento {{htmlelement("iframe")}}, potreste provare qualcosa come il prossimo esempio di codice.
Se aggiungeste il codice sotto in una delle vostre pagine, potreste essere sorpresi di vedere un messaggio di errore invece della pagina del glossario:

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

Se date un'occhiata alla console del vostro browser, vedrete un messaggio di errore simile a questo:

```plain
Refused to display 'https://developer.mozilla.org/' in a frame because it set 'X-Frame-Options' to 'deny'.
```

La sezione [Sicurezza](#preoccupazioni_di_sicurezza) sotto spiega in dettaglio perché vedete questo errore, ma prima, diamo un'occhiata a cosa sta facendo il nostro codice.

L'esempio include l'indispensabile necessario per usare un `<iframe>`:

- [`border: none`](/it/docs/Web/CSS/border)
  - : Se usato, l'`<iframe>` viene visualizzato senza bordo circostante. Altrimenti, per impostazione predefinita, i browser mostrano l'`<iframe>` con un bordo circostante (cosa generalmente indesiderata).
- [`allowfullscreen`](/it/docs/Web/HTML/Reference/Elements/iframe#allowfullscreen)
  - : Se impostato, l'`<iframe>` può essere posto in modalità schermo intero usando la [Fullscreen API](/it/docs/Web/API/Fullscreen_API) (leggermente al di fuori del campo di questo articolo).
- [`src`](/it/docs/Web/HTML/Reference/Elements/iframe#src)
  - : Questo attributo, come nel caso di {{htmlelement("video")}}/{{htmlelement("img")}}, contiene un percorso che punta all'URL del documento da incorporare.
- [`width`](/it/docs/Web/HTML/Reference/Elements/iframe#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/iframe#height)
  - : Questi attributi specificano la larghezza e l'altezza che si desidera che l'`<iframe>` abbia.
- [`sandbox`](/it/docs/Web/HTML/Reference/Elements/iframe#sandbox)
  - : Questo attributo, che funziona in browser leggermente più moderni rispetto al resto delle funzionalità `<iframe>` (ad esempio, IE 10 e successivi) richiede impostazioni di sicurezza elevate; ne parleremo di più nella sezione successiva.

> [!NOTE]
> Per migliorare la velocità, è una buona idea impostare l'attributo `src` dell'iframe con JavaScript dopo che il contenuto principale ha terminato il caricamento. Questo rende la pagina utilizzabile prima e diminuisce il tempo di caricamento ufficiale della pagina (un importante metrica {{Glossary("SEO", "SEO")}}).

### Preoccupazioni di sicurezza

Sopra abbiamo menzionato le preoccupazioni di sicurezza — vediamo ora di esplorarle in maggiore dettaglio. Non ci aspettiamo che comprendiate perfettamente tutti questi contenuti la prima volta; vogliamo solo rendervi consapevoli di questa preoccupazione, e fornire un riferimento a cui tornare man mano che acquisirete più esperienza e inizierete a considerare di usare `<iframe>` nei vostri esperimenti e lavori. Inoltre, non c'è bisogno di avere paura e non usare `<iframe>` — basta essere cauti. Continuate a leggere...

I produttori di browser e gli sviluppatori web hanno imparato a loro spese che gli iframes sono un obiettivo comune (termine ufficiale: **vettore d'attacco**) per le persone malintenzionate sul Web (spesso chiamate **hacker**, o più accuratamente, **cracker**) che cercano di modificare in modo dannoso la vostra pagina web, o di ingannare le persone per fare qualcosa che non vogliono fare, come rivelare informazioni sensibili come nomi utente e password. A causa di ciò, gli ingegneri delle specifiche e gli sviluppatori dei browser hanno sviluppato vari meccanismi di sicurezza per rendere gli `<iframe>` più sicuri, e ci sono anche buone pratiche da considerare — copriremo alcune di queste di seguito.

> **Nota:** Il [Clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) è un tipo comune di attacco iframe in cui i hacker incorporano un iframe invisibile nel vostro documento (o incorporano il vostro documento nel loro sito web dannoso) e lo usano per catturare le interazioni degli utenti. Questo è un modo comune per fuorviare gli utenti o rubare dati sensibili.

Un esempio rapido però — provate a caricare l'esempio precedente che abbiamo mostrato sopra nel vostro browser — potete [trovarlo live su GitHub](https://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html) ([vedetene il codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html) anche.) Invece della pagina che vi aspettavate, probabilmente vedrete un qualche messaggio del tipo "Non posso aprire questa pagina", e se guardate nella _Console_ degli [strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools), vedrete un messaggio che vi spiega perché. In Firefox, vi sarà comunicato qualcosa del tipo _Il caricamento di "https\://developer.mozilla.org/it/docs/Glossary" in un frame è negato dalla direttiva "X-Frame-Options" impostata a "DENY"_. Questo perché gli sviluppatori che hanno costruito MDN hanno incluso un'impostazione sul server che serve le pagine del sito per impedire che esse vengano incorporate all'interno di `<iframe>` (vedi [Configura direttive CSP](#configurare_le_direttive_csp), sotto.) Questo ha senso — un'intera pagina MDN non ha davvero senso essere incorporata in altre pagine a meno che non vogliate fare qualcosa come incorporarla nel vostro sito e reclamarla come vostra — o tentare di rubare dati tramite [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking), cose che entrambe sono davvero cattive da fare. Inoltre, se tutti iniziassero a fare questo, tutta la banda larga aggiuntiva comincerebbe a costare molto a Mozilla.

#### Incorpora solo quando necessario

A volte ha senso incorporare contenuto terziario — come video di youtube e mappe — ma potete risparmiarvi molti mal di testa se incorporate contenuto terziario solo quando completamente necessario. Una buona regola per la sicurezza web è _"Non si è mai troppo cauti. Se lo avete fatto, ricontrollatelo comunque. Se lo ha fatto qualcun altro, presupponete che sia pericoloso fino a prova contraria."_

Oltre alla sicurezza, dovreste anche essere consapevoli delle questioni relative alla proprietà intellettuale. La maggior parte dei contenuti è protetta da copyright, offline e online, anche contenuti che potreste non aspettarvi (per esempio, la maggior parte delle immagini su [Wikimedia Commons](https://commons.wikimedia.org/wiki/Main_Page)). Non visualizzate mai contenuti sul vostro sito web a meno che non ne siate i proprietari o i proprietari vi abbiano dato un permesso scritto, inequivocabile. Le penalità per violazione del copyright sono severe. Di nuovo, non si può mai essere troppo cauti.

Se il contenuto è licenziato, è necessario rispettare i termini della licenza. Per esempio, il contenuto su MDN è [concessa sotto licenza CC-BY-SA](/it/docs/MDN/Writing_guidelines/Attrib_copyright_license#documentation). Ciò significa che si deve [darci credito adeguato](https://wiki.creativecommons.org/wiki/Best_practices_for_attribution) quando citate il nostro contenuto, anche se apportate modifiche sostanziali.

#### Utilizza HTTPS

{{Glossary("HTTPS", "HTTPS")}} è la versione crittografata di {{Glossary("HTTP", "HTTP")}}. Dovreste servire i vostri siti web usando HTTPS ogni volta che possibile:

1. HTTPS riduce le probabilità che il contenuto remoto sia stato manomesso durante il transito.
2. HTTPS impedisce che il contenuto incorporato acceda al contenuto nel vostro documento principale, e viceversa.

Abilitare HTTPS sul vostro sito richiede l'installazione di un certificato di sicurezza speciale. Molti fornitori di hosting offrono un hosting abilitato HTTPS senza bisogno che facciate alcuna configurazione per mettere in atto un certificato. Ma se _dovete_ configurare il supporto HTTPS per il vostro sito da soli, [Let's Encrypt](https://letsencrypt.org/) fornisce strumenti e istruzioni che potete usare per creare e installare automaticamente il certificato necessario — con supporto integrato per i server web più ampiamente utilizzati, incluso il server web Apache, Nginx, e altri. Gli strumenti Let's Encrypt sono progettati per rendere il processo il più semplice possibile, quindi non c'è davvero alcuna buona ragione per evitare di usarlo o altri mezzi disponibili per abilitare HTTPS sul vostro sito.

> **Nota:** [GitHub pages](/it/docs/Learn_web_development/Howto/Tools_and_setup/Using_GitHub_pages) consente di servire contenuti tramite HTTPS per impostazione predefinita.
> Se si utilizza un provider di hosting diverso si dovrebbe controllare quale supporto forniscono per servire contenuti con HTTPS.

#### Utilizzare sempre l'attributo `sandbox`

Vorrete dare agli attaccanti il meno potere possibile per fare cose dannose al vostro sito web, quindi dovreste dare ai contenuti incorporati _solo i permessi necessari per svolgere il loro compito._ Naturalmente, questo vale anche per i vostri contenuti. Un contenitore per il codice dove può essere usato in modo appropriato — o per test — ma non può causare alcun danno al resto del codice (sia accidentale che doloso) è chiamato [sandbox](<https://en.wikipedia.org/wiki/Sandbox_(computer_security)>).

I contenuti non segregati potrebbero essere in grado di eseguire JavaScript, inviare moduli, generare finestre popup, ecc. Per impostazione predefinita, dovreste imporre tutte le restrizioni disponibili usando l'attributo `sandbox` senza parametri, come mostrato nel nostro esempio precedente.

Se strettamente necessario, potete aggiungere permessi uno per uno (all'interno del valore dell'attributo `sandbox=""`) — vedi la voce di riferimento [`sandbox`](/it/docs/Web/HTML/Reference/Elements/iframe#sandbox) per tutte le opzioni disponibili. Una nota importante è che non si dovrebbe mai aggiungere sia `allow-scripts` che `allow-same-origin` al proprio attributo `sandbox` — in quel caso, il contenuto incorporato potrebbe bypassare la {{Glossary("Same-origin_policy", "Same-origin policy")}} che impedisce ai siti di eseguire script, e utilizzare JavaScript per disattivare il sandboxing del tutto.

> [!NOTE]
> Il sandboxing non fornisce protezione se gli attaccanti riescono a ingannare le persone affinché visitino contenuti dannosi direttamente (al di fuori di un `iframe`). Se c'è qualche possibilità che un certo contenuto possa essere dannoso (ad esempio, contenuto generato dagli utenti), si prega di servirlo da un diverso {{Glossary("domain", "dominio")}} rispetto al vostro sito principale.

#### Configurare le direttive CSP

{{Glossary("CSP", "CSP")}} sta per **[politica di sicurezza dei contenuti](/it/docs/Web/HTTP/Guides/CSP)** e fornisce [un insieme di intestazioni HTTP](/it/docs/Web/HTTP/Reference/Headers/Content-Security-Policy) (metadati inviati insieme alle vostre pagine web quando sono servite da un server web) progettate per migliorare la sicurezza del vostro documento HTML. Quando si tratta di proteggere gli `<iframe>`, potete _[configurare il vostro server per inviare una intestazione `X-Frame-Options` appropriata.](/it/docs/Web/HTTP/Reference/Headers/X-Frame-Options)_ Ciò può impedire ad altri siti web di incorporare i vostri contenuti nelle loro pagine web (che abiliterebbe il [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) e una serie di altri attacchi), che è esattamente ciò che gli sviluppatori MDN hanno fatto, come abbiamo visto prima.

> [!NOTE]
> Potete leggere il post di Frederik Braun [Sulla intestazione di sicurezza X-Frame-Options](https://blog.mozilla.org/security/2013/12/12/on-the-x-frame-options-security-header/) per ulteriori informazioni su questo argomento. Ovviamente, è piuttosto fuori tema per una spiegazione completa in questo articolo.

## Gli elementi \<embed> e \<object>

Gli elementi {{htmlelement("embed")}} e {{htmlelement("object")}} servono una funzione diversa rispetto a {{htmlelement("iframe")}} — questi elementi sono strumenti di incorporamento generali per includere contenuti esterni, come i PDF.

Tuttavia, è improbabile che usiate molto questi elementi. Se avete bisogno di visualizzare PDF, è generalmente meglio collegarli, piuttosto che incorporarli nella pagina.

Storicamente questi elementi sono stati usati anche per incorporare contenuti gestiti dai {{Glossary("Plugin", "plugin")}} del browser come {{Glossary("Adobe_Flash", "Adobe Flash")}}, ma questa tecnologia è ormai obsoleta e non è supportata dai browser moderni.

Se vi trovate nella necessità di incorporare contenuti di plugin, questo è il tipo di informazioni di cui avrete bisogno, almeno in minima parte:

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
        <em>Tipo multimediale </em>{{Glossary("MIME_type", "corretto")}}
        del contenuto incorporato
      </td>
      <td><a href="/it/docs/Web/HTML/Reference/Elements/embed#type"><code>type</code></a></td>
      <td><a href="/it/docs/Web/HTML/Reference/Elements/object#type"><code>type</code></a></td>
    </tr>
    <tr>
      <td>
        Altezza e larghezza (in pixel CSS) della casella controllata dal plugin
      </td>
      <td>
         <a href="/it/docs/Web/HTML/Reference/Elements/embed#height"><code>height</code></a><br /><a href="/it/docs/Web/HTML/Reference/Elements/embed#width"><code>width</code></a>
      </td>
      <td>
         <a href="/it/docs/Web/HTML/Reference/Elements/object#height"><code>height</code></a><br /><a href="/it/docs/Web/HTML/Reference/Elements/object#width"><code>width</code></a>
      </td>
    </tr>
    <tr>
      <td>Contenuto HTML indipendente come ripiego per una risorsa non disponibile</td>
      <td>Non supportato (<code>&#x3C;noembed></code> è obsoleto)</td>
      <td>
        Contenuto all'interno dei tag di apertura e chiusura
        <code>&#x3C;object></code>
      </td>
    </tr>
  </tbody>
</table>

Vediamo un esempio di `<object>` che incorpora un PDF in una pagina (vedi l'[esempio live](https://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/object-pdf.html) e il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/other-embedding-technologies/object-pdf.html)):

```html
<object data="my-pdf.pdf" type="application/pdf" width="800" height="1200">
  <p>
    You don't have a PDF plugin, but you can
    <a href="my-pdf.pdf">download the PDF file. </a>
  </p>
</object>
```

I PDF sono stati un trampolino necessario tra carta e digitale, ma presentano molte [sfide di accessibilità](https://webaim.org/techniques/acrobat/acrobat) e possono essere difficili da leggere su schermi ridotti. Tendono ancora ad essere popolari in alcuni ambiti, ma è molto meglio collegarli così che possano essere scaricati o letti su una pagina separata, piuttosto che incorporarli in una pagina web.

## Sommario

L'argomento dell'incorporamento di altri contenuti nei documenti web può rapidamente diventare molto complesso, quindi in questo articolo abbiamo cercato di introdurlo in un modo semplice e familiare che sembrerà subito pertinente, pur accennando a alcune delle funzionalità più avanzate delle tecnologie coinvolte. All'inizio, è improbabile che utilizziate l'incorporamento oltre l'inclusione di contenuti di terzi come mappe e video sulle vostre pagine. Man mano che diventate più esperti, tuttavia, è probabile che inizierete a trovare più usi per essi.

Ci sono molte altre tecnologie che coinvolgono l'incorporamento di contenuti esterni oltre a quelli discussi qui. Ne abbiamo visti alcuni in articoli precedenti, come {{htmlelement("video")}}, {{htmlelement("audio")}}, e {{htmlelement("img")}}, ma ci sono altri da scoprire, come {{htmlelement("canvas")}} per la generazione di grafica 2D e 3D con JavaScript, e {{SVGElement("svg")}} per l'incorporamento di grafica vettoriale.
