---
title: Dagli object agli iframe — tecnologie di incorporamento generiche
short-title: Tecnologie di incorporamento
slug: Learn_web_development/Core/Structuring_content/General_embedding_technologies
l10n:
  sourceCommit: f08b3d623c43e0256072013372ba393b5bd1a5a0
---

Gli sviluppatori pensano comunemente all'incorporamento di contenuti multimediali come immagini, video e audio nelle pagine web. In questo articolo facciamo un passo leggermente laterale, esaminando alcuni elementi che consentono di incorporare un'ampia varietà di tipi di contenuto nelle pagine web: gli elementi {{htmlelement("iframe")}}, {{htmlelement("embed")}} e {{htmlelement("object")}}. Gli `<iframe>` servono per incorporare altre pagine web, mentre gli altri due consentono di incorporare risorse esterne come file PDF.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, conoscenza di base del
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Dealing_with_files"
          >lavoro con i file</a
        >, familiarità con i <a href="/it/docs/Learn_web_development/Core/Structuring_content"
          >fondamenti di HTML</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare a incorporare elementi nelle pagine web usando
        {{htmlelement("object")}}, {{htmlelement("embed")}} e
        {{htmlelement("iframe")}}, come documenti PDF e altre pagine web.
      </td>
    </tr>
  </tbody>
</table>

## Una breve storia dell'incorporamento

Molto tempo fa sul Web, era diffuso l'uso dei **frame** per creare siti web: piccole parti di un sito web archiviate in singole pagine HTML. Queste venivano incorporate in un documento principale chiamato **frameset**, che consentiva di specificare l'area dello schermo occupata da ciascun frame, in modo simile al dimensionamento di colonne e righe di una tabella. Erano considerati il massimo della modernità tra la metà e la fine degli anni Novanta, e vi erano prove che suddividere una pagina web in parti più piccole fosse vantaggioso per la velocità di download, aspetto particolarmente evidente dato che le connessioni di rete erano molto lente all'epoca. Tuttavia, avevano molti problemi, che superavano di gran lunga ogni vantaggio con l'aumento della velocità delle reti, quindi non vengono più utilizzati.

Poco tempo dopo, tra la fine degli anni Novanta e l'inizio degli anni Duemila, le tecnologie di plugin divennero molto popolari, come le {{Glossary("Java", "Java Applet")}} e {{Glossary("Adobe_Flash", "Flash")}}: consentivano agli sviluppatori web di incorporare nelle pagine web contenuti avanzati come video e animazioni, che non erano disponibili con il solo HTML. L'incorporamento di queste tecnologie avveniva tramite elementi come {{htmlelement("object")}} e il meno usato {{htmlelement("embed")}}, ed erano molto utili all'epoca. Da allora sono caduti in disuso a causa di molti problemi, tra cui accessibilità, sicurezza, dimensione dei file e altro. Oggi i principali browser hanno smesso di supportare plugin come Flash.

Infine apparve l'elemento {{htmlelement("iframe")}} (insieme ad altri modi per incorporare contenuti, come {{htmlelement("canvas")}}, {{htmlelement("video")}}, ecc.). Questo fornisce un modo per incorporare un intero documento web all'interno di un altro, come se fosse un elemento {{htmlelement("img")}} o simile, ed è regolarmente utilizzato ancora oggi.

Terminata la lezione di storia, proseguiamo e vediamo come usare alcuni di questi elementi.

## Sperimentare gli usi classici dell'incorporamento

In questo articolo passeremo direttamente a un esercizio, per offrire subito un'idea di ciò per cui sono utili le tecnologie di incorporamento. Il mondo online conosce molto bene [YouTube](https://www.youtube.com/), ma molte persone non conoscono alcune delle funzionalità di condivisione disponibili.

1. Innanzitutto, aprire [MDN Playground](/en-US/play).
2. Ora vedremo come YouTube consenta di incorporare un video in qualsiasi pagina desiderata usando un {{htmlelement("iframe")}}.
   1. Andare su YouTube e trovare un video.
   2. Sotto il video si trova un pulsante _Condividi_: selezionarlo per visualizzare le opzioni di condivisione.
   3. Selezionare il pulsante _Incorpora_ e verrà fornito del codice `<iframe>`: copiarlo.
   4. Incollarlo nel pannello _HTML_ del Playground e osservare il risultato nell'output.
3. Come esercizio aggiuntivo, si può anche provare a incorporare una [Google Map](https://www.google.com/maps/) nel Playground:
   1. Andare su Google Maps e trovare una mappa.
   2. Fare clic sul "menu hamburger" (tre linee orizzontali) in alto a sinistra nell'interfaccia utente.
   3. Selezionare l'opzione _Condividi o incorpora mappa_.
   4. Selezionare l'opzione _Incorpora una mappa_, che fornirà del codice `<iframe>`: copiarlo.
   5. Incollarlo nel pannello _HTML_ del Playground e osservare il risultato nell'output.

In caso di errore, è sempre possibile ripristinare il contenuto usando il pulsante _Reset_ nel Playground.

## iframe in dettaglio

È stato semplice e divertente, vero? Gli elementi {{htmlelement("iframe")}} sono progettati per consentire l'incorporamento di altri documenti web nel documento corrente. Questo è ottimo per integrare nel proprio sito web contenuti di terze parti sui quali non si ha il controllo diretto e per i quali non si desidera implementare una versione propria, come video di fornitori di video online, sistemi di commento come [Disqus](https://disqus.com/), mappe di fornitori di mappe online, banner pubblicitari e così via. Persino gli esempi modificabili dal vivo usati durante questo corso sono implementati usando `<iframe>`.

Prima di approfondire l'uso degli elementi `<iframe>`, è necessario conoscere alcune questioni di sicurezza.
Supponiamo di voler includere il glossario MDN in una delle proprie pagine web usando l'elemento {{htmlelement("iframe")}}: si potrebbe provare qualcosa come il seguente esempio di codice.
Se il codice seguente venisse aggiunto a una delle proprie pagine, potrebbe sorprendere la visualizzazione di un messaggio di errore anziché della pagina del glossario:

```html
<iframe
  src="https://developer.mozilla.org/en-US/docs/Glossary"
  width="100%"
  height="500"
  allowfullscreen
  sandbox>
</iframe>
```

```css
iframe {
  border: none;
}
```

Osservando la console del browser, verrà visualizzato un messaggio di errore simile al seguente:

```plain
Refused to display 'https://developer.mozilla.org/' in a frame because it set 'X-Frame-Options' to 'deny'.
```

La sezione [Sicurezza](#sicurezza) qui sotto spiega più in dettaglio perché viene visualizzato questo errore, ma prima vediamo cosa fa il codice.

L'esempio include gli elementi essenziali necessari per usare un `<iframe>`:

- [`border: none`](/it/docs/Web/CSS/Reference/Properties/border)
  - : Se usato, l'`<iframe>` viene visualizzato senza un bordo circostante. Altrimenti, per impostazione predefinita, i browser visualizzano l'`<iframe>` con un bordo circostante, generalmente indesiderabile.
- [`allowfullscreen`](/it/docs/Web/HTML/Reference/Elements/iframe#allowfullscreen)
  - : Se impostato, l'`<iframe>` può essere posto in modalità schermo intero usando la [Fullscreen API](/it/docs/Web/API/Fullscreen_API), argomento che va leggermente oltre lo scopo di questo articolo.
- [`src`](/it/docs/Web/HTML/Reference/Elements/iframe#src)
  - : Questo attributo, come per {{htmlelement("video")}}/{{htmlelement("img")}}, contiene un percorso che punta all'URL del documento da incorporare.
- [`width`](/it/docs/Web/HTML/Reference/Elements/iframe#width) e [`height`](/it/docs/Web/HTML/Reference/Elements/iframe#height)
  - : Questi attributi specificano la larghezza e l'altezza desiderate per l'iframe.
- [`sandbox`](/it/docs/Web/HTML/Reference/Elements/iframe#sandbox)
  - : Questo attributo, che funziona in browser leggermente più recenti rispetto alle altre funzionalità di `<iframe>` (ad esempio IE 10 e versioni successive), richiede impostazioni di sicurezza più rigorose; ne parleremo nella prossima sezione.

> [!NOTE]
> Per migliorare la velocità, è consigliabile impostare l'attributo `src` dell'iframe con JavaScript dopo il completamento del caricamento del contenuto principale. In questo modo la pagina diventa utilizzabile prima e si riduce il tempo ufficiale di caricamento della pagina, un'importante metrica {{Glossary("SEO", "SEO")}}.

### Sicurezza

Abbiamo menzionato sopra alcune questioni di sicurezza: approfondiamole ora. Non è previsto che tutto questo contenuto venga compreso perfettamente alla prima lettura; l'obiettivo è soltanto rendere consapevoli di questo aspetto e fornire un riferimento a cui tornare man mano che si acquisisce esperienza e si inizia a considerare l'uso degli `<iframe>` nei propri esperimenti e progetti. Non c'è nemmeno bisogno di temere gli `<iframe>` e non usarli: occorre soltanto prestare attenzione. Continuare a leggere…

I produttori di browser e gli sviluppatori Web hanno imparato a proprie spese che gli iframe sono un bersaglio comune, termine ufficiale: **vettore di attacco**, per persone malintenzionate sul Web, spesso chiamate **hacker**, o più precisamente **cracker**, che cercano di modificare in modo dannoso una pagina web oppure di indurre le persone a fare qualcosa che non desiderano, come rivelare informazioni sensibili quali nomi utente e password. Per questo motivo, gli ingegneri delle specifiche e gli sviluppatori di browser hanno creato diversi meccanismi di sicurezza per rendere gli `<iframe>` più sicuri, ed esistono anche buone pratiche da considerare: ne verranno trattate alcune qui sotto.

> [!NOTE]
> Il [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) è un tipo comune di attacco tramite iframe in cui gli hacker incorporano un iframe invisibile nel documento, oppure incorporano il documento nel proprio sito web dannoso, e lo usano per acquisire le interazioni degli utenti. Questo è un modo comune per ingannare gli utenti o rubare dati sensibili.

Prima, però, un rapido esempio: provare a caricare nel browser l'esempio precedente mostrato sopra. È possibile [trovarlo su GitHub](https://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html) e anche [vedere il codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/other-embedding-technologies/iframe-detail.html). Al posto della pagina prevista, probabilmente verrà visualizzato un messaggio simile a "Impossibile aprire questa pagina"; inoltre, osservando la _Console_ negli [strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools), verrà visualizzato un messaggio che ne spiega il motivo. In Firefox, verrà mostrato un messaggio simile a _Il caricamento di "https\://developer.mozilla.org/it/docs/Glossary" in un frame è negato dalla direttiva "X-Frame-Options" impostata su "DENY"_. Questo accade perché gli sviluppatori che hanno creato MDN hanno incluso un'impostazione sul server che fornisce le pagine del sito web per impedire che vengano incorporate negli `<iframe>` (vedere [Configurare le direttive CSP](#configurare_le_direttive_csp), qui sotto). Ha senso: incorporare un'intera pagina MDN in altre pagine non ha realmente senso, a meno che non si voglia incorporarla nel proprio sito e rivendicarla come propria, oppure tentare di rubare dati tramite [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking), entrambe azioni decisamente da evitare. Inoltre, se tutti iniziassero a farlo, tutta la larghezza di banda aggiuntiva inizierebbe a costare molto denaro a Mozilla.

#### Incorporare solo quando necessario

A volte ha senso incorporare contenuti di terze parti, come video di YouTube e mappe, ma si possono evitare molti problemi incorporando contenuti di terze parti solo quando strettamente necessario. Una buona regola per la sicurezza web è: _"Non si è mai troppo prudenti. Se è stato creato internamente, va comunque ricontrollato. Se è stato creato da qualcun altro, deve essere considerato pericoloso finché non venga dimostrato il contrario."_

Oltre alla sicurezza, è necessario considerare anche le questioni relative alla proprietà intellettuale. La maggior parte dei contenuti è protetta da copyright, offline e online, persino contenuti che potrebbero non sembrare tali, come la maggior parte delle immagini su [Wikimedia Commons](https://commons.wikimedia.org/wiki/Main_Page). Non visualizzare mai contenuti sulla propria pagina web a meno che non se ne possiedano i diritti o i proprietari abbiano concesso un'autorizzazione scritta e inequivocabile. Le sanzioni per violazione del copyright sono severe. Ancora una volta, non si è mai troppo prudenti.

Se il contenuto è concesso in licenza, è necessario rispettarne i termini. Ad esempio, il contenuto su MDN è [concesso in licenza CC-BY-SA](/it/docs/MDN/Writing_guidelines/Attrib_copyright_license#documentation). Ciò significa che è necessario [attribuirci correttamente il merito](https://wiki.creativecommons.org/wiki/Best_practices_for_attribution) quando si cita il nostro contenuto, anche nel caso in cui vengano apportate modifiche sostanziali.

#### Usare HTTPS

{{Glossary("HTTPS", "HTTPS")}} è la versione crittografata di {{Glossary("HTTP", "HTTP")}}. I siti web dovrebbero essere pubblicati usando HTTPS ogni volta che possibile:

1. HTTPS riduce la possibilità che il contenuto remoto venga manomesso durante il trasferimento.
2. HTTPS impedisce al contenuto incorporato di accedere al contenuto nel documento padre, e viceversa.

L'abilitazione di HTTPS per un sito richiede l'installazione di uno speciale certificato di sicurezza. Molti provider di hosting offrono hosting con HTTPS abilitato senza che sia necessario configurare autonomamente un certificato. Tuttavia, se è necessario configurare autonomamente il supporto HTTPS per il proprio sito, [Let's Encrypt](https://letsencrypt.org/) fornisce strumenti e istruzioni utilizzabili per creare e installare automaticamente il certificato necessario, con supporto integrato per i server web più diffusi, tra cui Apache, Nginx e altri. Gli strumenti di Let's Encrypt sono progettati per rendere il processo il più semplice possibile, quindi non esiste alcun buon motivo per evitare di usarli o di usare altri mezzi disponibili per abilitare HTTPS sul proprio sito.

> [!NOTE]
> [GitHub Pages](/it/docs/Learn_web_development/Howto/Tools_and_setup/Using_GitHub_pages) consente di pubblicare contenuti tramite HTTPS per impostazione predefinita.
> Se viene usato un provider di hosting diverso, verificare quale supporto offre per pubblicare contenuti tramite HTTPS.

#### Usare sempre l'attributo `sandbox`

È opportuno dare agli aggressori meno potere possibile per compiere azioni dannose sul sito web; pertanto, al contenuto incorporato devono essere date _solo le autorizzazioni necessarie per svolgere il proprio compito_. Naturalmente, questo vale anche per i contenuti propri. Un contenitore per codice in cui questo possa essere usato in modo appropriato, oppure per i test, ma non possa danneggiare il resto della base di codice, accidentalmente o intenzionalmente, viene chiamato [sandbox](<https://en.wikipedia.org/wiki/Sandbox_(computer_security)>).

Il contenuto che non è in sandbox potrebbe essere in grado di eseguire JavaScript, inviare moduli, attivare finestre popup e così via. Per impostazione predefinita, dovrebbero essere imposte tutte le restrizioni disponibili usando l'attributo `sandbox` senza parametri, come mostrato nell'esempio precedente.

Se assolutamente necessario, è possibile aggiungere nuovamente le autorizzazioni una alla volta, all'interno del valore dell'attributo `sandbox=""`: consultare la voce di riferimento [`sandbox`](/it/docs/Web/HTML/Reference/Elements/iframe#sandbox) per tutte le opzioni disponibili. Una nota importante: non aggiungere _mai_ sia `allow-scripts` sia `allow-same-origin` all'attributo `sandbox`; in quel caso, il contenuto incorporato potrebbe aggirare la {{Glossary("Same-origin_policy", "Same-origin policy")}}, che impedisce ai siti di eseguire script, e usare JavaScript per disattivare completamente il sandboxing.

> [!NOTE]
> Il sandboxing non fornisce protezione se gli aggressori riescono a indurre le persone a visitare direttamente contenuti dannosi, al di fuori di un `iframe`. Se esiste la possibilità che determinati contenuti siano dannosi, ad esempio contenuti generati dagli utenti, pubblicarli da un {{Glossary("domain", "dominio")}} diverso rispetto al sito principale.

#### Configurare le direttive CSP

{{Glossary("CSP", "CSP")}} significa **[content security policy](/it/docs/Web/HTTP/Guides/CSP)** e fornisce [un insieme di header HTTP](/it/docs/Web/HTTP/Reference/Headers/Content-Security-Policy), ossia metadati inviati insieme alle pagine web quando vengono pubblicate da un server web, progettati per migliorare la sicurezza del documento HTML. Per quanto riguarda la protezione degli `<iframe>`, è possibile _[configurare il server affinché invii un header `X-Frame-Options` appropriato.](/it/docs/Web/HTTP/Reference/Headers/X-Frame-Options)_ Questo può impedire ad altri siti web di incorporare il proprio contenuto nelle loro pagine web, cosa che consentirebbe il [clickjacking](/it/docs/Web/Security/Attacks/Clickjacking) e numerosi altri attacchi. Questo è esattamente ciò che hanno fatto gli sviluppatori di MDN, come visto in precedenza.

> [!NOTE]
> Per maggiori informazioni di contesto su questo argomento, è possibile leggere l'articolo di Frederik Braun [On the X-Frame-Options Security Header](https://blog.mozilla.org/security/2013/12/12/on-the-x-frame-options-security-header/). Naturalmente, una spiegazione completa va piuttosto oltre lo scopo di questo articolo.

## Gli elementi \<embed> e `<object>`

Gli elementi {{htmlelement("embed")}} e {{htmlelement("object")}} svolgono una funzione diversa da {{htmlelement("iframe")}}: sono strumenti di incorporamento generici per incorporare contenuti esterni, come i PDF.

Tuttavia, è improbabile che questi elementi vengano usati molto spesso. Se è necessario visualizzare PDF, di solito è meglio creare collegamenti ad essi, anziché incorporarli nella pagina.

Storicamente, questi elementi sono stati usati anche per incorporare contenuti gestiti da {{Glossary("Plugin", "plugin")}} del browser come {{Glossary("Adobe_Flash", "Adobe Flash")}}, ma questa tecnologia è ora obsoleta e non è supportata dai browser moderni.

Se fosse necessario incorporare contenuto di plugin, queste sono le informazioni necessarie, come minimo:

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
        <em>Tipo </em>{{Glossary("MIME_type", "MIME")}}
        accurato del contenuto incorporato
      </td>
      <td><a href="/it/docs/Web/HTML/Reference/Elements/embed#type"><code>type</code></a></td>
      <td><a href="/it/docs/Web/HTML/Reference/Elements/object#type"><code>type</code></a></td>
    </tr>
    <tr>
      <td>
        Altezza e larghezza, in pixel CSS, della casella controllata dal plugin
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

Vediamo un esempio di `<object>` che incorpora un PDF in una pagina; consultare l'[esempio dal vivo](https://mdn.github.io/learning-area/html/multimedia-and-embedding/other-embedding-technologies/object-pdf.html) e il [codice sorgente](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/other-embedding-technologies/object-pdf.html):

```html
<object data="my-pdf.pdf" type="application/pdf" width="800" height="1200">
  <p>
    You don't have a PDF plugin, but you can
    <a href="my-pdf.pdf">download the PDF file. </a>
  </p>
</object>
```

I PDF sono stati un necessario passaggio intermedio tra la carta e il digitale, ma pongono molte [sfide di accessibilità](https://webaim.org/techniques/acrobat/acrobat) e possono essere difficili da leggere sugli schermi piccoli. Tendono ancora a essere popolari in alcuni contesti, ma è molto meglio collegarli in modo che possano essere scaricati o letti in una pagina separata, anziché incorporarli in una pagina web.

## Riepilogo

L'argomento dell'incorporamento di altri contenuti nei documenti web può diventare rapidamente molto complesso; in questo articolo abbiamo quindi cercato di introdurlo in modo semplice e familiare, che sembri immediatamente pertinente, accennando comunque ad alcune funzionalità più avanzate delle tecnologie coinvolte. Per iniziare, è improbabile che l'incorporamento venga usato per molto più che includere nelle pagine contenuti di terze parti, come mappe e video. Con l'acquisizione di maggiore esperienza, tuttavia, si inizieranno probabilmente a trovare altri usi.

Esistono molte altre tecnologie che implicano l'incorporamento di contenuti esterni, oltre a quelle discusse qui. Alcune sono state viste in articoli precedenti, come {{htmlelement("video")}}, {{htmlelement("audio")}} e {{htmlelement("img")}}, ma ce ne sono altre da scoprire, come {{htmlelement("canvas")}} per la grafica 2D e 3D generata con JavaScript e {{SVGElement("svg")}} per incorporare grafica vettoriale.
