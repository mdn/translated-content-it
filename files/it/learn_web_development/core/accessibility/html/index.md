---
title: "HTML: una buona base per l'accessibilità"
short-title: HTML accessibile
slug: Learn_web_development/Core/Accessibility/HTML
l10n:
  sourceCommit: 1b7c3c1e03f14c3878e4d8518b0f1a89bedfdc9c
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Tooling","Learn_web_development/Core/Accessibility/Test_your_skills/HTML", "Learn_web_development/Core/Accessibility")}}

Una grande quantità di contenuti web può essere resa accessibile semplicemente assicurandosi che vengano sempre usati gli elementi Hypertext Markup Language corretti per lo scopo corretto. Questo articolo esamina in dettaglio come HTML possa essere usato per garantire la massima accessibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, una <a href="/it/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">comprensione di base dei concetti di accessibilità</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Usare HTML semantico, ovvero "l'elemento giusto per il compito giusto", poiché il browser fornisce così tanti meccanismi di accessibilità integrati.</li>
          <li>Buone pratiche di accessibilità quali testo alternativo, buon testo dei link, etichette dei moduli e intestazioni e ambiti di righe e colonne delle tabelle.</li>
          <li>Usare un linguaggio semplice e chiaro, evitando per quanto possibile gergo e abbreviazioni, e fornendo definizioni quando non è possibile evitarli.</li>
          <li>Il concetto e la pratica dell'accessibilità tramite tastiera.</li>
          <li>L'importanza dell'ordine nel sorgente.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## HTML e accessibilità

Man mano che si approfondisce la conoscenza di HTML — leggendo più risorse, osservando più esempi e così via — continuerà a emergere un tema comune: l'importanza dell'uso di HTML semantico (talvolta chiamato POSH, o Plain Old Semantic HTML). Ciò significa usare, per quanto possibile, gli elementi HTML corretti per lo scopo per cui sono previsti.

Ci si potrebbe chiedere perché questo sia così importante. Dopotutto, è possibile usare una combinazione di CSS e JavaScript per fare comportare praticamente qualsiasi elemento HTML nel modo desiderato. Ad esempio, un pulsante di controllo per riprodurre un video sul proprio sito potrebbe essere contrassegnato in questo modo:

```html
<div>Play video</div>
```

Ma, come verrà illustrato più dettagliatamente in seguito, ha senso usare l'elemento corretto per il compito:

```html
<button>Play video</button>
```

Non solo gli elementi HTML `<button>` hanno per impostazione predefinita alcuni stili appropriati applicati (che probabilmente si vorrà sovrascrivere), ma dispongono anche di accessibilità tramite tastiera integrata: gli utenti possono navigare tra i pulsanti usando il tasto <kbd>Tab</kbd> e attivare la selezione usando <kbd>Space</kbd>, <kbd>Return</kbd> o <kbd>Enter</kbd>.

L'HTML semantico non richiede più tempo da scrivere rispetto al markup non semantico (errato), se viene usato in modo coerente fin dall'inizio del progetto. Ancora meglio, il markup semantico offre altri vantaggi oltre all'accessibilità:

1. **Più facile da sviluppare** — come già menzionato, offre alcune funzionalità gratuitamente e, probabilmente, è anche più facile da comprendere.
2. **Migliore sui dispositivi mobili** — l'HTML semantico ha probabilmente una dimensione del file inferiore rispetto al codice spaghetti non semantico ed è più facile da rendere responsive.
3. **Utile per la SEO** — i motori di ricerca attribuiscono più importanza alle parole chiave all'interno di titoli, link e così via rispetto alle parole chiave incluse in `<div>` non semantici e così via, quindi i documenti saranno più facilmente reperibili dai clienti.

Procediamo quindi esaminando più in dettaglio l'HTML accessibile.

## Buona semantica

Abbiamo già parlato dell'importanza di una semantica corretta e del motivo per cui occorre usare l'elemento HTML giusto per il compito giusto. Questo aspetto non può essere ignorato, poiché è uno dei principali punti in cui l'accessibilità viene gravemente compromessa se non gestita correttamente.

Sul web, la verità è che le persone fanno alcune cose davvero strane con il markup HTML. Spesso l'uso scorretto di HTML è dovuto a pratiche obsolete che non sono ancora scomparse, ma talvolta avviene perché gli autori non ne sanno di più. In ogni caso, è opportuno sostituire il codice errato con un buon markup semantico ovunque possibile, sia nelle pagine HTML statiche sia nell'HTML generato dinamicamente da codice [lato server](/it/docs/Learn_web_development/Extensions/Server-side) o da [framework JavaScript lato client](/it/docs/Learn_web_development/Core/Frameworks_libraries) come React.

Talvolta non è possibile eliminare markup scadente: le pagine potrebbero dipendere da codice lato server o componenti web/framework sui quali non si ha alcun controllo, oppure potrebbero contenere contenuti di terze parti (come banner pubblicitari).

L'obiettivo non è "tutto o niente"; ogni miglioramento possibile aiuterà la causa dell'accessibilità.

### Usare contenuti testuali ben strutturati

Uno dei migliori aiuti all'accessibilità per un utente di screen reader è un'eccellente struttura del testo con titoli, paragrafi, elenchi e così via. Un buon esempio semantico potrebbe apparire più o meno così:

```html example-good
<h1>My heading</h1>

<p>This is the first section of my document.</p>

<p>I'll add another paragraph here too.</p>

<ol>
  <li>Here is</li>
  <li>a list for</li>
  <li>you to read</li>
</ol>

<h2>My subheading</h2>

<p>
  This is the first subsection of my document. I'd love people to be able to
  find this content!
</p>

<h2>My 2nd subheading</h2>

<p>
  This is the second subsection of my content, which I think is more interesting
  than the last one.
</p>
```

È stata preparata una versione con testo più lungo da provare con uno screen reader (vedere [good-semantics.html](https://mdn.github.io/learning-area/accessibility/html/good-semantics.html)). Provando a navigarla, si noterà che è piuttosto facile da esplorare:

1. Lo screen reader legge ogni intestazione man mano che si procede nel contenuto, comunicando cosa sia un titolo, cosa sia un paragrafo e così via.
2. Si ferma dopo ogni elemento, consentendo di procedere alla velocità più confortevole.
3. In molti screen reader è possibile passare al titolo successivo/precedente.
4. In molti screen reader è anche possibile visualizzare un elenco di tutti i titoli, consentendo di usarli come un pratico sommario per trovare contenuti specifici.

Talvolta le persone scrivono titoli, paragrafi e così via usando interruzioni di riga e aggiungendo elementi HTML esclusivamente per lo stile, in modo simile al seguente:

```html example-bad
<span style="font-size: 3em">My heading</span> <br /><br />
This is the first section of my document.
<br /><br />
I'll add another paragraph here too.
<br /><br />
1. Here is
<br /><br />
2. a list for
<br /><br />
3. you to read
<br /><br />
<span style="font-size: 2.5em">My subheading</span>
<br /><br />
This is the first subsection of my document. I'd love people to be able to find
this content!
<br /><br />
<span style="font-size: 2.5em">My 2nd subheading</span>
<br /><br />
This is the second subsection of my content. I think is more interesting than
the last one.
```

Provando la versione più lunga con uno screen reader (vedere [bad-semantics.html](https://mdn.github.io/learning-area/accessibility/html/bad-semantics.html)), l'esperienza non sarà molto buona: lo screen reader non ha nulla da usare come punti di riferimento, quindi non è possibile recuperare un sommario utile e l'intera pagina viene vista come un unico blocco gigantesco, quindi viene letta tutta insieme in un'unica volta.

Esistono anche altri problemi oltre all'accessibilità: ad esempio, è più difficile applicare stili al contenuto tramite CSS o manipolarlo con JavaScript, perché non esistono elementi da usare come selettori.

### Usare un linguaggio chiaro

Anche il linguaggio usato può influire sull'accessibilità. In generale, occorre usare un linguaggio chiaro, non eccessivamente complesso e privo di gergo o espressioni colloquiali non necessarie. Questo non va a vantaggio soltanto delle persone con disabilità cognitive o di altro tipo; va a vantaggio dei lettori per cui il testo non è scritto nella lingua madre, delle persone più giovani… di tutti, in effetti. Inoltre, occorre cercare di evitare linguaggio e caratteri che non vengono letti chiaramente dallo screen reader. Ad esempio:

- Non usare trattini se è possibile evitarlo. Invece di scrivere 5–7, scrivere da 5 a 7.
- Espandere le abbreviazioni: invece di scrivere gen, scrivere gennaio.
- Espandere gli acronimi, almeno una o due volte, quindi usare il tag [`<abbr>`](/it/docs/Web/HTML/Reference/Elements/abbr) per descriverli.

### Strutturare logicamente le sezioni della pagina

Occorre usare gli appropriati [elementi di sezionamento](/it/docs/Web/HTML/Reference/Elements#content_sectioning) per strutturare le pagine web, ad esempio navigazione ({{htmlelement("nav")}}), piè di pagina ({{htmlelement("footer")}}) e unità di contenuto ripetute ({{htmlelement("article")}}). Questi forniscono semantica aggiuntiva agli screen reader (e ad altri strumenti), offrendo agli utenti ulteriori indizi sui contenuti che stanno navigando.

Ad esempio, una moderna struttura del contenuto potrebbe apparire più o meno così:

```html
<header>
  <h1>Header</h1>
</header>

<nav>
  <!-- main navigation in here -->
</nav>

<!-- Here is our page's main content -->
<main>
  <!-- It contains an article -->
  <article>
    <h2>Article heading</h2>

    <!-- article content in here -->
  </article>

  <aside>
    <h2>Related</h2>

    <!-- aside content in here -->
  </aside>
</main>

<!-- And here is our main footer that is used across all the pages of our website -->

<footer>
  <!-- footer content in here -->
</footer>
```

Un [esempio completo è disponibile qui](https://mdn.github.io/learning-area/html/introduction-to-html/document_and_website_structure/).

Oltre ad avere una buona semantica e un layout accattivante, il contenuto dovrebbe avere senso logico nel suo ordine sorgente: in seguito sarà sempre possibile posizionarlo dove si desidera usando CSS, ma occorre iniziare con il giusto ordine sorgente, affinché ciò che viene letto agli utenti di screen reader abbia senso.

### Usare controlli UI semantici quando possibile

Per controlli UI si intendono le parti principali dei documenti web con cui gli utenti interagiscono, più comunemente pulsanti, link e controlli dei moduli. In questa sezione verranno esaminate le preoccupazioni di base relative all'accessibilità da considerare quando si creano tali controlli. Gli articoli successivi su WAI-ARIA e contenuti multimediali esamineranno altri aspetti dell'accessibilità della UI.

Un aspetto fondamentale dell'accessibilità dei controlli UI è che, per impostazione predefinita, i browser consentono di manipolarli tramite tastiera. È possibile provarlo usando l'esempio [native-keyboard-accessibility.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html)). Aprirlo in una nuova scheda e provare a premere il tasto Tab; dopo alcune pressioni, il focus della tabulazione dovrebbe iniziare a spostarsi tra i diversi elementi selezionabili. Gli elementi con focus ricevono uno stile predefinito evidenziato in ogni browser (che differisce leggermente tra browser diversi), così da poter riconoscere quale elemento ha il focus.

![Tre pulsanti contenenti rispettivamente il testo "Click me!", "Click me too!" e "And me!". Il terzo pulsante ha un contorno blu attorno per indicare il focus corrente della tabulazione.](button-focused-unfocused.png)

> [!NOTE]
> È possibile abilitare negli strumenti per sviluppatori una sovrapposizione che mostra l'ordine di tabulazione della pagina. Per ulteriori informazioni, vedere: [Accessibility Inspector > Show web page tabbing order](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html#show-web-page-tabbing-order).

È quindi possibile premere Enter/Return per seguire un link con focus o premere un pulsante (è stato incluso del JavaScript per fare visualizzare ai pulsanti un messaggio di avviso), oppure iniziare a digitare per inserire testo in un input di testo. Altri elementi dei moduli hanno controlli diversi; ad esempio, l'elemento {{htmlelement("select")}} può visualizzare le proprie opzioni e consentire di scorrerle usando i tasti freccia su e freccia giù.

Questo comportamento viene essenzialmente ottenuto gratuitamente, semplicemente usando gli elementi appropriati, ad esempio:

```html example-good
<h1>Links</h1>

<p>This is a link to <a href="https://www.mozilla.org">Mozilla</a>.</p>

<p>
  Another link, to the
  <a href="https://developer.mozilla.org">Mozilla Developer Network</a>.
</p>

<h2>Buttons</h2>

<p>
  <button data-message="This is from the first button">Click me!</button>
  <button data-message="This is from the second button">Click me too!</button>
  <button data-message="This is from the third button">And me!</button>
</p>

<h2>Form</h2>

<form>
  <div>
    <label for="name">Fill in your name:</label>
    <input type="text" id="name" name="name" />
  </div>
  <div>
    <label for="age">Enter your age:</label>
    <input type="text" id="age" name="age" />
  </div>
  <div>
    <label for="mood">Choose your mood:</label>
    <select id="mood" name="mood">
      <option>Happy</option>
      <option>Sad</option>
      <option>Angry</option>
      <option>Worried</option>
    </select>
  </div>
</form>
```

Ciò significa usare appropriatamente link, pulsanti, elementi dei moduli ed etichette (incluso l'elemento {{htmlelement("label")}} per i controlli dei moduli).

Tuttavia, questo è un altro caso in cui le persone talvolta fanno cose strane con HTML. Ad esempio, talvolta si vedono pulsanti contrassegnati usando elementi {{htmlelement("div")}}, ad esempio:

```html example-bad
<div data-message="This is from the first button">Click me!</div>
<div data-message="This is from the second button">Click me too!</div>
<div data-message="This is from the third button">And me!</div>
```

Ma l'uso di tale codice non è consigliato: si perde immediatamente l'accessibilità nativa tramite tastiera che si sarebbe ottenuta usando semplicemente elementi {{htmlelement("button")}}, oltre a non ottenere gli stili CSS predefiniti assegnati ai pulsanti. Nel raro, se non inesistente, caso in cui sia necessario usare un elemento non pulsante per un pulsante, usare il [`role` `button`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) e implementare tutti i comportamenti predefiniti del pulsante, incluso il supporto per tastiera e pulsante del mouse.

#### Ripristinare l'accessibilità tramite tastiera

Riaggiungere tali vantaggi richiede un po' di lavoro (è possibile vedere un esempio in [fake-div-buttons.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html); vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html)). Qui è stata data ai falsi pulsanti `<div>` la possibilità di ricevere il focus (anche tramite tabulazione) assegnando a ciascuno l'attributo `tabindex="0"`. È incluso anche `role="button"`, affinché gli utenti di screen reader sappiano di poter mettere a fuoco e interagire con l'elemento:

```html
<div data-message="This is from the first button" tabindex="0" role="button">
  Click me!
</div>
<div data-message="This is from the second button" tabindex="0" role="button">
  Click me too!
</div>
<div data-message="This is from the third button" tabindex="0" role="button">
  And me!
</div>
```

In sostanza, l'attributo [`tabindex`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex) è destinato principalmente a consentire agli elementi tabulabili di avere un ordine di tabulazione personalizzato (specificato in ordine numerico positivo), anziché essere attraversati tramite tabulazione nel loro ordine sorgente predefinito. Questa è quasi sempre una cattiva idea, poiché può causare grande confusione. Va usato solo quando è davvero necessario, ad esempio se il layout mostra gli elementi in un ordine visivo molto diverso dal codice sorgente e si desidera far funzionare le cose in modo più logico. Esistono altre due opzioni per `tabindex`:

- `tabindex="0"` — come indicato sopra, questo valore consente agli elementi normalmente non tabulabili di diventare tabulabili. È il valore più utile di `tabindex`.
- `tabindex="-1"` — consente a elementi normalmente non tabulabili di ricevere il focus in modo programmatico, ad esempio tramite JavaScript o come destinazione di link.

Sebbene l'aggiunta precedente consenta di raggiungere i pulsanti tramite tabulazione, non permette di attivarli con il tasto <kbd>Enter</kbd>/<kbd>Return</kbd>. Per farlo, è stato necessario aggiungere il seguente codice JavaScript:

```js
document.onkeydown = (e) => {
  // The Enter/Return key
  if (e.key === "Enter") {
    document.activeElement.click();
  }
};
```

Qui viene aggiunto un listener all'oggetto `document` per rilevare quando è stato premuto un pulsante sulla tastiera. Viene verificato quale pulsante sia stato premuto tramite la proprietà [`key`](/it/docs/Web/API/KeyboardEvent/key) dell'oggetto evento; se il tasto premuto è <kbd>Enter</kbd>/<kbd>Return</kbd>, viene eseguita la funzione memorizzata nel gestore `onclick` del pulsante usando `document.activeElement.click()`. [`activeElement`](/it/docs/Web/API/Document/activeElement) fornisce l'elemento attualmente con focus nella pagina.

È molto lavoro aggiuntivo per ripristinare questa funzionalità. E sicuramente vi saranno altri problemi. **È molto meglio usare fin dall'inizio l'elemento giusto per il compito giusto.**

#### Usare etichette testuali significative

Le etichette testuali dei controlli UI sono molto utili per tutti gli utenti, ma definirle correttamente è particolarmente importante per gli utenti con disabilità.

Occorre assicurarsi che le etichette testuali di pulsanti e link siano comprensibili e distintive. Non usare semplicemente "Fare clic qui" come etichetta, poiché gli utenti di screen reader talvolta visualizzano un elenco di pulsanti e controlli dei moduli. La schermata seguente mostra i controlli elencati da VoiceOver su Mac.

![Elenco delle etichette degli input dei moduli visualizzato dal software VoiceOver su Mac. Questo elenco contiene etichette prive di significato come 'happy menu button' assegnate a vari controlli dei moduli, quali button, textfield e link.](voiceover-formcontrols.png)

Assicurarsi che le etichette abbiano senso fuori contesto, lette da sole, oltre che nel contesto del paragrafo in cui si trovano. Ad esempio, il seguente mostra un esempio di buon testo per un link:

```html example-good
<p>
  Whales are really awesome creatures.
  <a href="whales.html">Find out more about whales</a>.
</p>
```

ma questo è un cattivo testo per un link:

```html example-bad
<p>
  Whales are really awesome creatures. To find out more about whales,
  <a href="whales.html">click here</a>.
</p>
```

> [!NOTE]
> È possibile trovare molte più informazioni sull'implementazione dei link e sulle buone pratiche nell'articolo [Creazione di link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links). È inoltre possibile vedere alcuni esempi buoni e cattivi in [good-links.html](https://mdn.github.io/learning-area/accessibility/html/good-links.html) e [bad-links.html](https://mdn.github.io/learning-area/accessibility/html/bad-links.html).

Anche le etichette dei moduli sono importanti per fornire un'indicazione su ciò che deve essere inserito in ciascun input del modulo. Il seguente sembra un esempio abbastanza ragionevole:

```html example-bad
Fill in your name: <input type="text" id="name" name="name" />
```

Tuttavia, non è altrettanto utile per gli utenti con disabilità. Nell'esempio precedente non vi è nulla che associ in modo inequivocabile l'etichetta all'input del modulo e che chiarisca come compilarlo se non è possibile vederlo. Accedendo a questo contenuto con alcuni screen reader, si potrebbe ricevere soltanto una descrizione simile a "modifica testo".

Il seguente è un esempio molto migliore:

```html example-good
<div>
  <label for="name">Fill in your name:</label>
  <input type="text" id="name" name="name" />
</div>
```

Con codice di questo tipo, l'etichetta sarà chiaramente associata all'input; la descrizione sarà più simile a "Inserisci il tuo nome: modifica testo".

![Una buona etichetta del modulo che recita "Inserisci il tuo nome" viene assegnata a un controllo del modulo di input di testo.](voiceover-good-form-label.png)

Come vantaggio aggiuntivo, nella maggior parte dei browser associare un'etichetta a un input del modulo significa che è possibile fare clic sull'etichetta per selezionare o attivare l'elemento del modulo. Questo offre all'input un'area selezionabile più grande, rendendolo più facile da selezionare.

> [!NOTE]
> È possibile vedere alcuni esempi di moduli buoni e cattivi in [good-form.html](https://mdn.github.io/learning-area/accessibility/html/good-form.html) e [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html).

Una buona spiegazione dell'importanza di etichette testuali corrette e di come analizzare i problemi delle etichette testuali usando [Firefox Accessibility Inspector](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html) è disponibile nel seguente video:

{{EmbedYouTube("YhlAVlfH0rQ")}}

## Tabelle di dati accessibili

Una tabella di dati di base può essere scritta con markup molto semplice, ad esempio:

```html
<table>
  <tr>
    <td>Name</td>
    <td>Age</td>
    <td>Pronouns</td>
  </tr>
  <tr>
    <td>Xavier</td>
    <td>23</td>
    <td>he/him</td>
  </tr>
  <tr>
    <td>Tina</td>
    <td>8</td>
    <td>she/her</td>
  </tr>
  <tr>
    <td>Sam</td>
    <td>17</td>
    <td>she/her</td>
  </tr>
</table>
```

Ma questo presenta problemi: non esiste un modo per un utente di screen reader di associare righe o colonne come raggruppamenti di dati. Per farlo, è necessario sapere quali siano le righe di intestazione e se fungano da intestazione per righe, colonne e così via. Questo può essere fatto solo visivamente per la tabella precedente (vedere [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html) e provare personalmente l'esempio).

Ora osservare l'[esempio di tabella delle band punk](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/styling-tables/punk-bands-complete.html): qui sono visibili alcuni strumenti di accessibilità in azione:

- Le intestazioni della tabella vengono definite usando elementi {{htmlelement("th")}}; è inoltre possibile specificare se sono intestazioni per righe o colonne usando l'attributo `scope`. Questo fornisce gruppi completi di dati che possono essere elaborati dagli screen reader come singole unità.
- L'elemento {{htmlelement("caption")}} e l'attributo `summary` dell'elemento `<table>` svolgono compiti simili: fungono da testo alternativo per una tabella, offrendo a un utente di screen reader un utile riepilogo rapido dei contenuti della tabella. L'elemento `<caption>` è generalmente preferito poiché rende il suo contenuto accessibile anche agli utenti vedenti, che potrebbero trovarlo utile. Non sono realmente necessari entrambi.

> [!NOTE]
> Per maggiori dettagli sulle tabelle di dati accessibili, vedere l'articolo [Accessibilità delle tabelle HTML](/it/docs/Learn_web_development/Core/Structuring_content/Table_accessibility).

## Alternative testuali

Mentre il contenuto testuale è intrinsecamente accessibile, non si può necessariamente dire lo stesso per il contenuto multimediale: il contenuto di immagini e video non può essere visto dalle persone con disabilità visive, mentre il contenuto audio non può essere ascoltato dalle persone con disabilità uditive. Il contenuto video e audio viene trattato in dettaglio nella [Guida ai contenuti multimediali accessibili](/it/docs/Learn_web_development/Core/Accessibility/Multimedia), ma in questo articolo verrà esaminata l'accessibilità per l'umile elemento {{htmlelement("img")}}.

È stato preparato un semplice esempio, [accessible-image.html](https://mdn.github.io/learning-area/accessibility/html/accessible-image.html), che presenta quattro copie della stessa immagine:

```html
<img src="dinosaur.png" />

<img
  src="dinosaur.png"
  alt="A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a human, with small arms, and a large head with lots of sharp teeth." />

<img
  src="dinosaur.png"
  alt="A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a human, with small arms, and a large head with lots of sharp teeth."
  title="The Mozilla red dinosaur" />

<img src="dinosaur.png" aria-labelledby="dino-label" />

<p id="dino-label">
  The Mozilla red Tyrannosaurus Rex: A two legged dinosaur standing upright like
  a human, with small arms, and a large head with lots of sharp teeth.
</p>
```

La prima immagine, quando viene visualizzata da uno screen reader, non offre realmente molto aiuto all'utente: VoiceOver, ad esempio, legge "/dinosaur.png, immagine". Legge il nome del file per cercare di fornire un aiuto. In questo esempio l'utente saprà almeno che si tratta di un qualche tipo di dinosauro, ma spesso i file possono essere caricati con nomi generati automaticamente (ad esempio, da una fotocamera digitale) e tali nomi probabilmente non fornirebbero alcun contesto sul contenuto dell'immagine.

> [!NOTE]
> Questo è il motivo per cui non si dovrebbe mai includere contenuto testuale all'interno di un'immagine: gli screen reader non possono accedervi. Ci sono anche altri svantaggi: non è possibile selezionarlo e copiarlo/incollarlo. Semplicemente, non farlo.

Quando uno screen reader incontra la seconda immagine, legge l'intero attributo alt: "Un Tyrannosaurus Rex rosso: un dinosauro bipede che sta in piedi come un essere umano, con braccia piccole e una grande testa con molti denti affilati.".

Questo evidenzia l'importanza non solo di usare nomi di file significativi nel caso in cui il cosiddetto **testo alternativo** non sia disponibile, ma anche di assicurarsi che il testo alternativo venga fornito negli attributi `alt` ovunque possibile.

Si noti che il contenuto dell'attributo `alt` dovrebbe sempre fornire una rappresentazione diretta dell'immagine e di ciò che comunica visivamente. Il testo alternativo dovrebbe essere breve e conciso e includere tutte le informazioni trasmesse nell'immagine che non siano duplicate nel testo circostante.

Il contenuto dell'attributo `alt` per una singola immagine differisce in base al contesto. Ad esempio, se la foto di Fluffy è un avatar accanto a una recensione del cibo per cani Yuckymeat, `alt="Fluffy"` è appropriato. Se la foto fa parte della pagina di adozione di Fluffy per un'associazione di salvataggio animali, dovrebbero essere incluse le informazioni trasmesse nell'immagine rilevanti per una potenziale persona adottante che non siano duplicate nel testo circostante. È appropriata una descrizione più lunga, come `alt="Fluffy, un terrier tricolore dal pelo molto corto, con una pallina da tennis in bocca."`. Poiché il testo circostante probabilmente indica già la taglia e la razza di Fluffy, tali informazioni non sono incluse in `alt`. Tuttavia, poiché la biografia del cane probabilmente non include lunghezza del pelo, colori o preferenze sui giocattoli, che la potenziale persona adottante deve conoscere, tali informazioni vengono incluse. L'immagine è all'aperto oppure Fluffy indossa un collare rosso con un guinzaglio blu? Non è importante ai fini dell'adozione dell'animale e pertanto non viene incluso. Devono essere trasmesse tutte le informazioni che l'immagine comunica, accessibili a un utente vedente e rilevanti per il contesto; nulla di più. Il testo deve rimanere breve, preciso e utile.

Non dovrebbero essere incluse conoscenze personali o descrizioni aggiuntive, poiché non sono utili per le persone che non hanno mai visto l'immagine. Se la pallina è il giocattolo preferito di Fluffy oppure se un utente vedente non può saperlo dall'immagine, non includerlo.

Un aspetto da considerare è se le immagini abbiano un significato all'interno del contenuto oppure se siano puramente decorative dal punto di vista visivo e dunque prive di significato. Se sono decorative, è meglio scrivere un testo vuoto come valore dell'attributo `alt` (vedere [Attributi alt vuoti](#attributi_alt_vuoti)) oppure includerle nella pagina come immagini di sfondo CSS.

> [!NOTE]
> Leggere [Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images) e [Immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images) per molte più informazioni sull'implementazione delle immagini e sulle buone pratiche.
> È inoltre possibile consultare [An alt Decision Tree](https://www.w3.org/WAI/tutorials/images/decision-tree/) per imparare a usare un attributo alt per le immagini in varie situazioni.

Se si desidera fornire informazioni contestuali aggiuntive, occorre inserirle nel testo che circonda l'immagine oppure all'interno di un attributo `title`, come mostrato sopra. In questo caso, la maggior parte degli screen reader leggerà il testo alternativo, l'attributo title e il nome del file. Inoltre, i browser visualizzano il testo title come tooltip al passaggio del mouse.

![Schermata di un Tyrannosaurus Rex rosso con il testo "The mozilla red dinosaur" visualizzato come tooltip al passaggio del mouse.](title-attribute.png)

Esaminiamo rapidamente il quarto metodo:

```html
<img src="dinosaur.png" aria-labelledby="dino-label" />

<p id="dino-label">The Mozilla red Tyrannosaurus…</p>
```

In questo caso, non viene usato affatto l'attributo `alt`: invece, la descrizione dell'immagine viene presentata come un normale paragrafo di testo, a cui viene assegnato un `id`, e quindi viene usato l'attributo `aria-labelledby` per fare riferimento a tale `id`, facendo sì che gli screen reader usino quel paragrafo come testo alternativo/etichetta per l'immagine. Ciò è particolarmente utile se si desidera usare lo stesso testo come etichetta per più immagini, cosa non possibile con `alt`.

> [!NOTE]
> [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) fa parte della specifica [WAI-ARIA](https://w3c.github.io/aria/), che consente agli sviluppatori di aggiungere semantica aggiuntiva al markup per migliorare l'accessibilità con gli screen reader dove necessario.

### Figure e didascalie

HTML include due elementi, {{htmlelement("figure")}} e {{htmlelement("figcaption")}}, che associano una figura di qualsiasi tipo (potrebbe essere qualsiasi cosa, non necessariamente un'immagine) a una didascalia:

```html
<figure>
  <img
    src="dinosaur.png"
    alt="The Mozilla Tyrannosaurus"
    aria-describedby="dinodescr" />
  <figcaption id="dinodescr">
    A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a
    human, with small arms, and a large head with lots of sharp teeth.
  </figcaption>
</figure>
```

Sebbene il supporto degli screen reader per l'associazione delle didascalie alle relative figure sia variabile, includere [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) o [`aria-describedby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby) crea l'associazione qualora non sia presente. Detto questo, la struttura degli elementi è utile per lo stile CSS e fornisce inoltre un modo per posizionare una descrizione dell'immagine accanto a essa nel sorgente.

### Attributi alt vuoti

```html
<h3>
  <img src="article-icon.png" alt="" />
  Tyrannosaurus Rex: the king of the dinosaurs
</h3>
```

Può capitare che un'immagine sia inclusa nel design di una pagina, ma che il suo scopo principale sia la decorazione visiva. Nel precedente esempio di codice, l'attributo `alt` dell'immagine è vuoto: ciò fa sì che gli screen reader riconoscano l'immagine, ma non tentino di descriverla (direbbero invece solo "immagine", o qualcosa di simile).

Il motivo per usare un `alt` vuoto anziché ometterlo è che molti screen reader annunciano l'intero URL dell'immagine se non viene fornito alcun `alt`. Nell'esempio precedente, l'immagine funge da decorazione visiva per il titolo a cui è associata. In casi come questo e nei casi in cui un'immagine sia soltanto decorativa e non abbia valore di contenuto, occorre includere un `alt` vuoto negli elementi `img`. Un'altra alternativa consiste nell'usare l'attributo aria [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) [`role="presentation"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/presentation_role), poiché anche questo impedisce agli screen reader di leggere il testo alternativo.

> [!NOTE]
> Se possibile, usare CSS per visualizzare immagini esclusivamente decorative.

## Altre informazioni sui link

I link (l'elemento [`<a>`](/it/docs/Web/HTML/Reference/Elements/a) con un attributo `href`), a seconda di come vengono usati, possono aiutare o danneggiare l'accessibilità. Per impostazione predefinita, i link sono accessibili nell'aspetto. Possono migliorare l'accessibilità aiutando un utente a navigare rapidamente verso diverse sezioni di un documento. Possono anche danneggiare l'accessibilità se ne viene rimosso lo stile accessibile o se JavaScript ne causa comportamenti imprevisti.

### Stile dei link

Per impostazione predefinita, i link sono visivamente diversi dagli altri testi sia nel colore sia in [text-decoration](/it/docs/Web/CSS/Reference/Properties/text-decoration): per impostazione predefinita sono blu e sottolineati, viola e sottolineati se visitati, e hanno un [focus-ring](/it/docs/Web/CSS/Reference/Selectors/:focus) quando ricevono il focus tramite tastiera.

Il colore non dovrebbe essere usato come unico metodo per distinguere i link dai contenuti non collegati. Il colore del testo dei link, come tutto il testo, deve essere significativamente diverso dal colore di sfondo ([un contrasto di 4,5:1](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast)). Inoltre, i link dovrebbero essere visivamente significativamente diversi dal testo non collegato, con un requisito minimo di contrasto di 3:1 tra il testo del link e il testo circostante e tra gli stati predefinito, visitato e focus/attivo, e un contrasto di 4,5:1 tra tutti i colori di tali stati e il colore di sfondo.

### Eventi `onclick`

I tag di ancoraggio sono spesso usati impropriamente con l'evento `onclick` per creare pseudo-pulsanti impostando **href** su `"#"` o `"javascript:void(0)"` per impedire l'aggiornamento della pagina.

Questi valori causano comportamenti imprevisti durante la copia o il trascinamento dei link, l'apertura dei link in una nuova scheda o finestra, l'aggiunta ai segnalibri e quando JavaScript è ancora in download, genera un errore o è disabilitato. Trasmettono inoltre una semantica errata alle tecnologie assistive, come gli screen reader. In questi casi, è consigliato usare invece un {{HTMLElement("button")}}. In generale, un'ancora dovrebbe essere usata solo per la navigazione tramite un URL corretto.

### Link esterni e link a risorse non HTML

I link che aprono una nuova scheda o finestra tramite la dichiarazione `target="_blank"` e i link il cui valore `href` punta a una risorsa file dovrebbero includere un indicatore del comportamento che si verificherà quando il link verrà attivato.

Le persone con ipovisione, che navigano con l'ausilio della tecnologia di lettura dello schermo o che hanno difficoltà cognitive potrebbero confondersi quando una nuova scheda, finestra o applicazione viene aperta inaspettatamente. Le versioni meno recenti del software di lettura dello schermo potrebbero persino non annunciare il comportamento.

#### Link che apre una nuova scheda o finestra

```html
<a target="_blank" href="https://www.wikipedia.org/"
  >Wikipedia (opens in a new window)</a
>
```

#### Link a una risorsa non HTML

```html
<a target="_blank" href="2017-annual-report.ppt"
  >2017 Annual Report (PowerPoint)</a
>
```

Se viene usata un'icona al posto del testo per indicare questo tipo di comportamento del link, assicurarsi che includa una [descrizione alternativa](/it/docs/Web/HTML/Reference/Elements/img#alt).

- [WebAIM: link e ipertesto - link ipertestuali](https://webaim.org/techniques/hypertext/hypertext_links)
- [MDN Understanding WCAG, spiegazioni della linea guida 3.2](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Understandable#guideline_3.2_—_predictable_make_web_pages_appear_and_operate_in_predictable_ways)
- [G200: apertura di nuove finestre e schede da un link solo quando necessario | Tecniche W3C per WCAG 2.0](https://www.w3.org/TR/WCAG20-TECHS/G200.html)
- [G201: avvertire anticipatamente gli utenti quando si apre una nuova finestra | Tecniche W3C per WCAG 2.0](https://www.w3.org/TR/WCAG20-TECHS/G201.html)

### Link per saltare contenuti

Un link per saltare contenuti, noto anche come skipnav, è un elemento `a` posizionato il più vicino possibile all'elemento di apertura {{HTMLElement("body")}} e che collega all'inizio del contenuto principale della pagina. Questo link consente alle persone di ignorare i contenuti ripetuti in più pagine di un sito web, come l'intestazione e la navigazione primaria del sito.

I link per saltare contenuti sono particolarmente utili per le persone che navigano con l'ausilio di tecnologie assistive come controllo tramite interruttore, comandi vocali o bastoncini orali/bacchette per la testa, per le quali spostarsi attraverso link ripetitivi può essere un compito laborioso.

- [WebAIM: link "Skip Navigation"](https://webaim.org/techniques/skipnav/)
- [Come fare: usare i link Skip Navigation - The A11Y Project](https://www.a11yproject.com/posts/skip-nav-links/)
- [MDN Understanding WCAG, spiegazioni della linea guida 2.4](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Operable#guideline_2.4_%e2%80%94_navigable_provide_ways_to_help_users_navigate_find_content_and_determine_where_they_are)
- [Comprendere il criterio di successo 2.4.1 | W3C Understanding WCAG 2.0](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-skip.html)

### Prossimità

Grandi quantità di contenuto interattivo, incluse le ancore, poste in stretta prossimità visiva tra loro dovrebbero avere dello spazio per separarle. Questa spaziatura è utile per le persone che hanno problemi di controllo motorio fine e potrebbero attivare accidentalmente il contenuto interattivo errato durante la navigazione.

La spaziatura può essere creata usando proprietà CSS come {{CSSxRef("margin")}}.

- [Tremori alle mani e il problema dei pulsanti giganti - Axess Lab](https://axesslab.com/hand-tremors/)

## Riepilogo

A questo punto si dovrebbe avere una buona padronanza della scrittura di HTML accessibile nella maggior parte delle occasioni. Nel prossimo articolo verranno proposti alcuni test che consentono di verificare quanto bene siano state comprese e ricordate tutte queste informazioni.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Tooling","Learn_web_development/Core/Accessibility/Test_your_skills/HTML", "Learn_web_development/Core/Accessibility")}}
