---
title: "HTML: Una buona base per l'accessibilità"
short-title: HTML accessibile
slug: Learn_web_development/Core/Accessibility/HTML
l10n:
  sourceCommit: b2c8dcdae36907a87d1d1b9393ca4a35ebc765d6
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Tooling","Learn_web_development/Core/Accessibility/CSS_and_JavaScript", "Learn_web_development/Core/Accessibility")}}

Gran parte del contenuto web può essere reso accessibile assicurandosi di utilizzare i corretti elementi di Hypertext Markup Language per il corretto scopo in ogni momento. Questo articolo analizza in dettaglio come HTML può essere utilizzato per garantire la massima accessibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di base di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, una <a href="/it/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">comprensione di base dei concetti di accessibilità</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Utilizzare HTML semantico, noto anche come "L'elemento giusto per il lavoro giusto", perché il browser fornisce molti 'hook' di accessibilità integrati.</li>
          <li>Migliori pratiche accessibili come testo alternativo, buone pratiche di collegamento, etichette di modulo e intestazioni di righe e colonne di tabelle e definizione dell'ambito.</li>
          <li>Utilizzare un linguaggio semplice e chiaro, evitando gergo e abbreviazioni quando possibile, e fornendo definizioni quando non è possibile.</li>
          <li>Il concetto e la pratica dell'accessibilità tramite tastiera.</li>
          <li>L'importanza dell'ordine della sorgente.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## HTML e accessibilità

Man mano che impara di più su HTML — legga più risorse, guardi più esempi, ecc. — continuerà a vedere un tema comune: l'importanza dell'uso di HTML semantico (a volte chiamato POSH, o Plain Old Semantic HTML). Ciò significa utilizzare gli elementi HTML corretti per il loro scopo previsto il più possibile.

Potrebbe chiedersi perché questo sia così importante. Dopo tutto, può utilizzare una combinazione di CSS e JavaScript per far comportare qualsiasi elemento HTML nel modo che desidera. Ad esempio, un pulsante di controllo per riprodurre un video sul suo sito potrebbe essere marcato in questo modo:

```html
<div>Play video</div>
```

Ma come vedrete più in dettaglio in seguito, ha senso utilizzare l'elemento corretto per il lavoro:

```html
<button>Play video</button>
```

Non solo i `<button>` HTML hanno uno stile adatto applicato per impostazione predefinita (che probabilmente vorrà sovrascrivere), ma hanno anche l'accessibilità tramite tastiera integrata: gli utenti possono navigare tra i pulsanti usando il tasto <kbd>Tab</kbd> e attivare la loro selezione usando <kbd>Space</kbd>, <kbd>Return</kbd> o <kbd>Enter</kbd>.

Scrivere HTML semantico non richiede più tempo rispetto a una marcatura non semantica (cattiva) se si inizia a farlo in modo coerente dall'inizio del progetto. Ancora meglio, la marcatura semantica offre altri vantaggi oltre all'accessibilità:

1. **Più facile da sviluppare** — come detto prima, si ottiene una certa funzionalità gratuitamente, e inoltre è innegabilmente più facile da capire.
2. **Migliore sui dispositivi mobili** — HTML semantico è probabilmente più leggero in termini di dimensioni dei file rispetto a un codice spaghetti non semantico e più facile da rendere reattivo.
3. **Buono per la SEO** — i motori di ricerca danno maggiore importanza alle parole chiave all'interno di intestazioni, link, ecc. rispetto alle parole chiave incluse in `<div>` non semantici, ecc., quindi i documenti saranno più facilmente trovabili dai clienti.

Procediamo ed esaminiamo in dettaglio HTML accessibile.

## Buona semantica

Abbiamo già parlato dell'importanza della corretta semantica e del motivo per cui dovremmo usare l'elemento HTML giusto per il lavoro. Questo non può essere ignorato, poiché è uno dei principali motivi per cui l'accessibilità è gravemente compromessa se non gestita correttamente.

Lì fuori sul Web, la verità è che le persone fanno cose molto strane con il markup HTML. Spesso, l'uso improprio di HTML è dovuto a pratiche obsolete che non sono ancora scomparse, ma a volte si verifica perché gli autori non ne sanno di più. Qualunque sia il caso, dovrebbe sostituire il codice cattivo con un buon markup semantico, ove possibile, sia nelle pagine HTML statiche che nell'HTML generato dinamicamente dal codice [server-side](/it/docs/Learn_web_development/Extensions/Server-side) o dai [framework JavaScript client-side](/it/docs/Learn_web_development/Core/Frameworks_libraries) come React.

A volte non ci si trova nella posizione di eliminare un markup scadente — le sue pagine potrebbero dipendere da codice server-side o componenti web/framework su cui non ha alcun controllo, oppure potrebbe avere contenuti di terze parti sulla sua pagina (come banner pubblicitari).

L'obiettivo non è "tutto o niente"; ogni miglioramento che può fare aiuta la causa dell'accessibilità.

### Utilizzare un contenuto di testo ben strutturato

Uno dei migliori aiuti all'accessibilità che un utente di screen reader può avere è un'ottima struttura del testo con intestazioni, paragrafi, elenchi, ecc. Un buon esempio semantico potrebbe apparire come il seguente:

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

Abbiamo preparato una versione con testo più lungo che può provare con un lettore dello schermo (vedi [good-semantics.html](https://mdn.github.io/learning-area/accessibility/html/good-semantics.html)). Se prova a navigare attraverso questo, vedrà che è piuttosto facile da navigare:

1. Il lettore dello schermo legge ogni intestazione mentre procede attraverso il contenuto, notificando ciò che è un'intestazione, cos'è un paragrafo, ecc.
2. Si ferma dopo ogni elemento, lasciandole andare a qualunque ritmo le sia più comodo.
3. Può passare all'intestazione successiva/precedente in molti lettori di schermo.
4. Può anche richiamare un elenco di tutte le intestazioni in molti lettori di schermo, permettendole di usarle come una comoda tabella dei contenuti per trovare contenuti specifici.

Le persone a volte scrivono intestazioni, paragrafi, ecc., usando interruzioni di riga e aggiungendo elementi HTML solo per lo stile, qualcosa del genere:

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

Se prova la nostra versione più lunga con un lettore dello schermo (vedi [bad-semantics.html](https://mdn.github.io/learning-area/accessibility/html/bad-semantics.html)), non avrà una buona esperienza: il lettore dello schermo non ha nulla da usare come punti di riferimento, quindi non può recuperare una tabella dei contenuti utile, e l'intera pagina viene vista come un unico blocco gigante, quindi viene letta tutta in una volta senza interruzioni.

Ci sono anche altri problemi oltre all'accessibilità: è più difficile stilizzare il contenuto usando CSS, o manipolarlo con JavaScript, per esempio, perché non ci sono elementi da usare come selettori.

### Usare un linguaggio chiaro

Il linguaggio che usa può anche influire sull'accessibilità. In generale, dovrebbe utilizzare un linguaggio chiaro che non sia eccessivamente complesso e non utilizzi gergo o termini di slang non necessari. Questo non solo va a beneficio delle persone con disabilità cognitive o altre; va a beneficio dei lettori per i quali il testo non è scritto nella loro lingua madre, delle persone più giovani… di tutti, in realtà! A parte questo, dovrebbe cercare di evitare di utilizzare linguaggio e caratteri che non vengono letti chiaramente dal lettore dello schermo. Ad esempio:

- Non usi i trattini se può evitarlo. Invece di scrivere 5–7, scriva 5 a 7.
- Espanda le abbreviazioni — invece di scrivere Gen, scriva Gennaio.
- Espanda gli acronimi, almeno una o due volte, quindi utilizzi il tag [`<abbr>`](/it/docs/Web/HTML/Reference/Elements/abbr) per descriverli.

### Strutturare le sezioni della pagina in modo logico

Dovrebbe utilizzare opportuni [elementi di sezionamento](/it/docs/Web/HTML/Reference/Elements#content_sectioning) per strutturare le sue pagine web, ad esempio navigazione ({{htmlelement("nav")}}), piè di pagina ({{htmlelement("footer")}}), e unità di contenuto ripetute ({{htmlelement("article")}}). Questi forniscono ulteriore semantica ai lettori di schermi (e altri strumenti) per fornire agli utenti ulteriori indizi sul contenuto che stanno navigando.

Ad esempio, una struttura di contenuto moderna potrebbe apparire così:

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

Può trovare un [esempio completo qui](https://mdn.github.io/learning-area/html/introduction-to-html/document_and_website_structure/).

Oltre ad avere una buona semantica e un layout attraente, il suo contenuto dovrebbe avere un senso logico nell'ordine sorgente — può sempre posizionarlo dove desidera usando CSS in seguito, ma dovrebbe ottenere l'ordine sorgente corretto all'inizio, in modo che ciò che gli utenti dei lettori di schermi sentano abbia un senso.

### Utilizzare controlli dell'interfaccia utente semantici ove possibile

Per controlli dell'interfaccia utente, intendiamo le principali parti dei documenti web con cui gli utenti interagiscono — più comunemente pulsanti, link e controlli dei moduli. In questa sezione, esamineremo le preoccupazioni di accessibilità di base da tenere a mente quando si creano tali controlli. Articoli successivi su WAI-ARIA e multimedia esamineranno altri aspetti dell'accessibilità dell'interfaccia utente.

Un aspetto chiave dell'accessibilità dei controlli dell'interfaccia utente è che per impostazione predefinita, i browser consentono di manipolarli tramite tastiera. Può provarlo usando il nostro esempio [native-keyboard-accessibility.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html) (vedi il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html)). Apra ciò in una nuova scheda e provi a premere il tasto Tab; dopo alcuni pressioni, dovrebbe vedere che il focus sul tab inizia a spostarsi tra i diversi elementi focalizzabili. Gli elementi focalizzati ricevono uno stile predefinito evidenziato in ogni browser (differisce leggermente tra i vari browser) in modo tale che possa capire quale elemento è focalizzato.

Tre pulsanti con il testo "Cliqui qui!", "Cliqui anche me!", e "E me!" all'interno rispettivamente. Il terzo pulsante ha un contorno blu intorno per indicare il focus attuale del tab.

> [!NOTE]
> È possibile abilitare un overlay che mostra l'ordine di tabulazione della pagina nei suoi strumenti per sviluppatori. Per ulteriori informazioni, veda: [Accessibility Inspector > Show web page tabbing order](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html#show-web-page-tabbing-order).

Può quindi premere Invio/Enter per seguire un link focalizzato o premere un pulsante (abbiamo incluso alcuni JavaScript per far sì che i pulsanti visualizzino un messaggio), o iniziare a digitare per inserire testo in un input di testo. Altri elementi del modulo hanno controlli diversi; ad esempio, l'elemento {{htmlelement("select")}} può avere le sue opzioni visualizzate e ciclare tra loro usando i tasti freccia su e giù.

Ottiene essenzialmente questo comportamento gratuitamente, solo utilizzando gli elementi appropriati, ad esempio:

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

Ciò significa utilizzare link, pulsanti, elementi di modulo ed etichette in modo appropriato (incluso l'elemento {{htmlelement("label")}} per i controlli del modulo).

Tuttavia, questo è un altro caso in cui le persone a volte fanno cose strane con HTML. Ad esempio, a volte si vedono pulsanti marcati usando i {{htmlelement("div")}}, per esempio:

```html example-bad
<div data-message="This is from the first button">Click me!</div>
<div data-message="This is from the second button">Click me too!</div>
<div data-message="This is from the third button">And me!</div>
```

Ma usare tale codice non è consigliato — si perde immediatamente l'accessibilità tramite tastiera nativa che avrebbe avuto semplicemente usando gli elementi {{htmlelement("button")}}, oltre a non ottenere alcuna delle stilizzazioni CSS predefinite che i pulsanti ottengono. Nel raro o inesistente caso in cui necessita utilizzare un elemento non pulsante per un pulsante, utilizzi il [`button` role](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) e implementi tutti i comportamenti del pulsante predefiniti, inclusi il supporto per tastiera e mouse.

#### Ricostruire l'accessibilità tramite tastiera

Aggiungere tali vantaggi richiede un po' di lavoro (può vedere un esempio nel nostro esempio [fake-div-buttons.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) — veda anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html)). Qui abbiamo dato ai nostri finti pulsanti `<div>` la capacità di essere focalizzati (incluso tramite tab) assegnando a ciascuno l'attributo `tabindex="0"`. Includiamo anche `role="button"` in modo che gli utenti di lettori di schermi sappiano di poter focalizzare e interagire con l'elemento:

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

Fondamentalmente, l'attributo [`tabindex`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex) è principalmente inteso a consentire agli elementi tabbabili di avere un ordine di tabulazione personalizzato (specificato in ordine numerico positivo), invece di essere tabbati nel loro ordine sorgente predefinito. Questo è quasi sempre un'idea sbagliata, poiché può causare grande confusione. Lo utilizzi solo se ne ha veramente bisogno, ad esempio, se il layout mostra le cose in un ordine visivo molto diverso rispetto al codice sorgente, e desidera rendere le cose più logiche. Ci sono altre due opzioni per `tabindex`:

- `tabindex="0"` — come indicato sopra, questo valore consente agli elementi che normalmente non sono tabbabili di diventare tabbabili. Questo è il valore più utile di `tabindex`.
- `tabindex="-1"` — questo permette agli elementi normalmente non tabbabili di ricevere focus programmaticamente, ad esempio tramite JavaScript, o come target di link.

Mentre l'aggiunta sopra le consente di tabbinare sui pulsanti, non le permette di attivarli tramite il tasto <kbd>Enter</kbd>/<kbd>Return</kbd>. Per farlo, abbiamo dovuto aggiungere il seguente frammento di JavaScript:

```js
document.onkeydown = (e) => {
  // The Enter/Return key
  if (e.key === "Enter") {
    document.activeElement.click();
  }
};
```

Qui aggiungiamo un listener all'oggetto `document` per rilevare quando un pulsante è stato premuto sulla tastiera. Verifichiamo quale pulsante è stato premuto tramite la proprietà [`key`](/it/docs/Web/API/KeyboardEvent/key) dell'oggetto evento; se il pulsante premuto è <kbd>Enter</kbd>/<kbd>Return</kbd>, eseguiamo la funzione memorizzata nel gestore `onclick` del pulsante utilizzando `document.activeElement.click()`. [`activeElement`](/it/docs/Web/API/Document/activeElement) ci dà l'elemento che è attualmente focalizzato sulla pagina.

Questo è un bel po' di lavoro extra per costruire la funzionalità. E ci saranno probabilmente altri problemi con esso. **Meglio utilizzare semplicemente l'elemento giusto per il lavoro giusto fin dall'inizio.**

#### Utilizzare etichette di testo significative

Le etichette di testo dei controlli dell'interfaccia utente sono molto utili per tutti gli utenti, ma ottenere giuste è particolarmente importante per gli utenti con disabilità.

Dovrebbe assicurarsi che il testo dei pulsanti e delle etichette dei link sia comprensibile e distintivo. Non utilizzi semplicemente "Clicca qui" per le tue etichette, poiché gli utenti di screen reader a volte ottengono un elenco di pulsanti e controlli di modulo. Il seguente screenshot mostra i nostri controlli elencati da VoiceOver su Mac.

![Elenco di etichette dei form input elencate dal software VoiceOver su Mac. Questo elenco contiene etichette prive di significato come 'happy menu button` assegnate a vari controlli del modulo come pulsante, campo di testo e link](voiceover-formcontrols.png)

Assicurarsi che le sue etichette abbiano un senso fuori contesto, lette per conto proprio, oltre che nel contesto del paragrafo in cui si trovano. Ad esempio, il seguente mostra un esempio di buon testo di link:

```html example-good
<p>
  Whales are really awesome creatures.
  <a href="whales.html">Find out more about whales</a>.
</p>
```

ma questo è un testo di link negativo:

```html example-bad
<p>
  Whales are really awesome creatures. To find out more about whales,
  <a href="whales.html">click here</a>.
</p>
```

> [!NOTE]
> Può trovare molto di più sull'implementazione dei link e le migliori pratiche nel nostro articolo [Creazione di link](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links). Può anche vedere alcuni buoni e cattivi esempi su [good-links.html](https://mdn.github.io/learning-area/accessibility/html/good-links.html) e [bad-links.html](https://mdn.github.io/learning-area/accessibility/html/bad-links.html).

Le etichette dei moduli sono anche importanti per fornire un indizio su cosa deve inserire in ogni input di modulo. Quanto segue sembra un esempio abbastanza ragionevole:

```html example-bad
Fill in your name: <input type="text" id="name" name="name" />
```

Tuttavia, questo non è molto utile per gli utenti disabili. Non c'è nulla nell'esempio sopra per associare l'etichetta in modo inequivocabile all'input del modulo e renderlo chiaro su come compilarlo se non può vederlo. Se accede a ciò con alcuni screen reader, potrebbe ricevere solo una descrizione del tipo "modifica testo."

Il seguente è un esempio molto migliore:

```html example-good
<div>
  <label for="name">Fill in your name:</label>
  <input type="text" id="name" name="name" />
</div>
```

Con codice come questo, l'etichetta sarà chiaramente associata all'input; la descrizione sarà più simile a "Compili il suo nome: modifica testo."

![Una buona etichetta di modulo che recita 'Compili il suo nome' è assegnata a un controllo del modulo di input di testo. ](voiceover-good-form-label.png)

Come bonus aggiuntivo, nella maggior parte dei browser associare un'etichetta con un input di modulo significa che può fare clic sull'etichetta per selezionare o attivare l'elemento del modulo. Questo offre all'input un'area di colpo più ampia, rendendolo più facile da selezionare.

> [!NOTE]
> Può vedere alcuni buoni e cattivi esempi di modulo in [good-form.html](https://mdn.github.io/learning-area/accessibility/html/good-form.html) e [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html).

Può trovare una bella spiegazione dell'importanza delle etichette di testo corrette e come investigare sui problemi delle etichette di testo utilizzando il [Firefox Accessibility Inspector](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html), nel seguente video:

{{EmbedYouTube("YhlAVlfH0rQ")}}

## Tabelle dati accessibili

Una tabella dati di base può essere scritta con un markup molto semplice, ad esempio:

```html
<table>
  <tr>
    <td>Name</td>
    <td>Age</td>
    <td>Pronouns</td>
  </tr>
  <tr>
    <td>Gabriel</td>
    <td>13</td>
    <td>he/him</td>
  </tr>
  <tr>
    <td>Elva</td>
    <td>8</td>
    <td>she/her</td>
  </tr>
  <tr>
    <td>Freida</td>
    <td>5</td>
    <td>she/her</td>
  </tr>
</table>
```

Ma questo presenta dei problemi — non c'è modo per un utente di un lettore di schermo di associare righe o colonne insieme come gruppi di dati. Per fare ciò, deve sapere quali sono le righe di intestazione e se sono intestazioni per righe, colonne, ecc. Questo può essere fatto solo visivamente per la tabella sopra (vedi [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html) e provi l'esempio da solo).

Ora guardi il nostro esempio [punk bands table](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/styling-tables/punk-bands-complete.html) — può vedere alcuni aiuti all'accessibilità in azione qui:

- Le intestazioni delle tabelle sono definite utilizzando elementi {{htmlelement("th")}} — può anche specificare se sono intestazioni per righe o colonne utilizzando l'attributo `scope`. Questo le dà gruppi completi di dati che possono essere consumati da screen reader come unità singole.
- L'elemento {{htmlelement("caption")}} e l'attributo `summary` dell'elemento `<table>` svolgono entrambi lavori simili — agiscono come testo alternativo per una tabella, fornendo agli utenti di screen reader un utile sommario rapido del contenuto della tabella. L'elemento `<caption>` è generalmente preferito poiché rende il suo contenuto accessibile anche agli utenti vedenti, che potrebbero trovarlo utile. Non ha davvero bisogno di entrambi.

> [!NOTE]
> Veda il nostro articolo [Accessibilità delle tabelle HTML](/it/docs/Learn_web_development/Core/Structuring_content/Table_accessibility) per ulteriori dettagli sulle tabelle dati accessibili.

## Alternative testuali

Mentre il contenuto testuale è intrinsecamente accessibile, lo stesso non si può necessariamente dire per il contenuto multimediale — le immagini e i video non possono essere visti dalle persone con disabilità visive, e il contenuto audio non può essere ascoltato dalle persone con disabilità uditive. Parleremo in dettaglio del contenuto video e audio nella [Multimedia accessibile](/it/docs/Learn_web_development/Core/Accessibility/Multimedia), ma per questo articolo esamineremo l'accessibilità per il modesto elemento {{htmlelement("img")}}.

Abbiamo scritto un semplice esempio, [accessible-image.html](https://mdn.github.io/learning-area/accessibility/html/accessible-image.html), che presenta quattro copie della stessa immagine:

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

La prima immagine, quando vista da un lettore di schermo, non offre realmente molto aiuto all'utente — ad esempio VoiceOver legge "/dinosaur.png, immagine". Legge il nome del file per cercare di fornire aiuto. In questo esempio l'utente almeno saprà che si tratta di un qualche tipo di dinosauro, ma spesso i file potrebbero essere caricati con nomi generati da una macchina (ad esempio, da una fotocamera digitale) e questi nomi di file probabilmente non fornirebbero alcun contesto al contenuto dell'immagine.

> [!NOTE]
> Ecco perché non dovrebbe mai includere contenuti testuali all'interno di un'immagine — i lettori di schermi non possono accedervi. Ci sono anche altri svantaggi — non può selezionarli e copiarli/incollarli. Semplicemente non lo faccia!

Quando un lettore di schermi incontra la seconda immagine, legge interamente l'attributo alt — "Un Tyrannosaurus Rex rosso: Un dinosauro a due zampe in piedi eretto come un essere umano, con piccole braccia e una grande testa con molti denti affilati.".

Questo evidenzia l'importanza non solo di utilizzare nomi di file significativi nel caso in cui il cosiddetto **alt text** non sia disponibile, ma anche di assicurarsi che il testo alternativo venga fornito negli attributi `alt` ove possibile.

Si noti che i contenuti dell'attributo `alt` dovrebbero sempre fornire una rappresentazione diretta dell'immagine e di ciò che essa trasmette visivamente. L'alt dovrebbe essere breve e conciso e includere tutte le informazioni trasmesse nell'immagine che non sono duplicate nel testo circostante.

Il contenuto dell'attributo `alt` per una singola immagine varia a seconda del contesto. Ad esempio, se la foto di Fluffy è un avatar accanto a una recensione per cibo per cani Yuckymeat, `alt="Fluffy"` è appropriato. Se la foto fa parte della pagina di adozione di Fluffy per la società di soccorso animale, le informazioni trasmesse nell'immagine che sono rilevanti per un potenziale genitore di cani che non sono duplicate nel testo circostante dovevano essere incluse. Una descrizione più lunga, come `alt="Fluffy, un terrier a tre colori con peli molto corti, con una palla da tennis in bocca."` è appropriata. Poiché il testo circostante probabilmente include la taglia e la razza di Fluffy, queste non sono incluse nell'`alt`. Tuttavia, poiché la biografia del cane probabilmente non include la lunghezza dei peli, i colori o le preferenze dei giocattoli, che il potenziale genitore necessita sapere, essi vengono inclusi. L'immagine è all'aperto, o Fluffy ha un collare rosso con un guinzaglio blu? Non è importante in termini di adozione dell'animale e quindi non viene incluso. Tutte le informazioni che l'immagine trasmette che un utente vedente può accedere e sono rilevanti per il contesto sono quelle che devono essere trasmesse; niente di più. Mantenilo breve, preciso e utile.

Qualsiasi conoscenza personale o descrizione extra non dovrebbe essere inclusa qui, poiché non è utile per le persone che non hanno mai visto l'immagine prima. Se la palla è il giocattolo preferito di Fluffy o se un utente vedente non può sapere questo dall'immagine, allora non includerlo.

Una cosa da considerare è se le sue immagini hanno significato nel suo contenuto, o se sono puramente per decorazione visiva, e quindi non hanno significato. Se sono decorative, è meglio scrivere un testo vuoto come valore per l'attributo `alt` (vedi [Attributi alt vuoti](#attributi_alt_vuoti)) o semplicemente includerli nella pagina come immagini di sfondo CSS.

> [!NOTE]
> Legga [Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images) e [Immagini responsive](/it/docs/Web/HTML/Guides/Responsive_images) per molte più informazioni sull'implementazione delle immagini e le migliori pratiche.
> Può anche verificare [An alt Decision Tree](https://www.w3.org/WAI/tutorials/images/decision-tree/) per imparare come utilizzare un attributo alt per le immagini in varie situazioni.

Se desidera fornire informazioni contestuali extra, dovrebbe inserirle nel testo circostante l'immagine, o all'interno di un attributo `title`, come mostrato sopra. In questo caso, la maggior parte dei lettori di schermi leggerà l'alt text, l'attributo title e il nome del file. Inoltre, i browser visualizzano il testo del titolo come suggerimenti quando si passa sopra con il mouse.

![Screenshot di un Tyrannosaurus Rex rosso con il testo "Il dinosauro rosso di Mozilla" visualizzato come suggerimento al passaggio del mouse.](title-attribute.png)

Diamo un'altra rapida occhiata al quarto metodo:

```html
<img src="dinosaur.png" aria-labelledby="dino-label" />

<p id="dino-label">The Mozilla red Tyrannosaurus…</p>
```

In questo caso, non stiamo utilizzando l'attributo `alt` — al contrario, abbiamo presentato la descrizione dell'immagine come un paragrafo di testo normale, gli abbiamo dato un `id`, e quindi abbiamo utilizzato l'attributo `aria-labelledby` per riferirci a quell'`id`, che causa ai lettori di schermi di utilizzare tale paragrafo come alt text/etichetta per quell'immagine. Questo è particolarmente utile se desidera utilizzare lo stesso testo come etichetta per più immagini — qualcosa che non è possibile con `alt`.

> **Nota:** [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) fa parte della specifica [WAI-ARIA](https://www.w3.org/TR/wai-aria-1.1/), che permette agli sviluppatori di aggiungere ulteriore semantica al loro markup per migliorare l'accessibilità del lettore di schermi, se necessario.

### Figure e didascalie delle figure

HTML include due elementi — {{htmlelement("figure")}} e {{htmlelement("figcaption")}} — che associano una figura di qualche tipo (potrebbe essere qualsiasi cosa, non necessariamente un'immagine) con una didascalia:

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

Mentre c'è un supporto misto degli screen reader per associare le didascalie delle figure con le loro figure, l'inclusione di [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) o [`aria-describedby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby) crea l'associazione se non presente. Detto questo, la struttura dell'elemento è utile per la stilizzazione CSS, oltre a fornire un modo per posizionare una descrizione dell'immagine accanto a essa nel codice sorgente.

### Attributi alt vuoti

```html
<h3>
  <img src="article-icon.png" alt="" />
  Tyrannosaurus Rex: the king of the dinosaurs
</h3>
```

Ci potrebbero essere momenti in cui un'immagine è inclusa nel design di una pagina, ma il suo scopo principale è per la decorazione visiva. Noterà nell'esempio di codice sopra che l'attributo `alt` dell'immagine è vuoto — questo serve a far riconoscere l'immagine ai lettori di schermi, ma non a tentare di descrivere l'immagine (invece direbbero solo "immagine", o simile).

La ragione per utilizzare un `alt` vuoto invece di non includerlo è che molti lettori di schermi annunciano l'intero URL dell'immagine se non viene fornito un `alt`. Nell'esempio sopra, l'immagine funge da decorazione visiva per l'intestazione a cui è associata. In casi come questo e in casi in cui un'immagine è solo decorativa e non ha valore di contenuto, dovrebbe includere un `alt` vuoto nei suoi elementi `img`. Un'alternativa è utilizzare l'attributo [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) di aria [`role="presentation"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/presentation_role) poiché anche questo impedisce ai lettori di schermi di leggere testo alternativo.

> [!NOTE]
> Se possibile, dovrebbe utilizzare il CSS per visualizzare immagini che sono solo decorative.

## Ulteriori informazioni sui link

I link (l'elemento [`<a>`](/it/docs/Web/HTML/Reference/Elements/a) con un attributo `href`), a seconda di come vengono usati, possono aiutare o danneggiare l'accessibilità. Per impostazione predefinita, i link sono accessibili nell'aspetto. Possono migliorare l'accessibilità aiutando l'utente a navigare rapidamente verso diverse sezioni di un documento. Possono anche danneggiare l'accessibilità se il loro stile accessibile viene rimosso o se JavaScript causa loro di comportarsi in modi inaspettati.

### Stile dei link

Per impostazione predefinita, i link sono visivamente diversi dal resto del testo, sia nel colore che nella [text-decoration](/it/docs/Web/CSS/text-decoration), con i link di colore blu e sottolineati per impostazione predefinita, di colore viola e sottolineati se visitati, e con un [focus-ring](/it/docs/Web/CSS/:focus) quando ricevono il focus da tastiera.

Il colore non dovrebbe essere utilizzato come unico metodo per distinguere i link dal contenuto non collegabile. Il colore del testo dei link, come tutto il testo, deve essere significativamente diverso dal colore di sfondo ([un contrasto di 4.5:1](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast)). Inoltre, i link dovrebbero essere visivamente significativamente diversi dal testo non collegabile, con un requisito minimo di contrasto di 3:1 tra il testo dei link e il testo circostante e tra gli stati predefiniti, visitati e di focus/attivi e un contrasto di 4.5:1 tra tutti quei colori di stato e il colore di sfondo.

### Eventi `onclick`

I tag di ancoraggio sono spesso abusati con l'evento `onclick` per creare pseudo-pulsanti impostando **href** su `"#"` o `"javascript:void(0)"` per evitare che la pagina si aggiorni.

Questi valori causano comportamenti inaspettati quando si copiano o trascinano link, si aprono link in una nuova scheda o finestra, si aggiungono ai segnalibri e quando JavaScript è ancora in fase di download, causa errori o è disabilitato. Questo trasmette anche semantica errata alle tecnologie assistive (ad esempio, lettori di schermi). In questi casi, è consigliato utilizzare un {{HTMLElement("button")}} invece. In generale, dovrebbe utilizzare un'ancora solo per la navigazione utilizzando un URL adeguato.

### Link esterni e collegamenti a risorse non-HTML

I link che si aprono in una nuova scheda o finestra tramite la dichiarazione `target="_blank"` e i link il cui valore del `href` punta a una risorsa di file dovrebbero includere un indicatore sul comportamento che si verificherà quando il link viene attivato.

Le persone che sperimentano condizioni di visione ridotta, che navigano con l'aiuto di tecnologie assistive di lettura dello schermo, o che hanno preoccupazioni cognitive possono confondersi quando una nuova scheda, finestra o applicazione viene aperta inaspettatamente. Le versioni più vecchie del software di lettura dello schermo potrebbero non annunciare nemmeno il comportamento.

#### Collegamento che apre una nuova scheda o finestra

```html
<a target="_blank" href="https://www.wikipedia.org/"
  >Wikipedia (opens in a new window)</a
>
```

#### Collegamento a una risorsa non-HTML

```html
<a target="_blank" href="2017-annual-report.ppt"
  >2017 Annual Report (PowerPoint)</a
>
```

Se viene utilizzata un'icona al posto del testo per significare il comportamento di questo tipo di link, assicurarsi che includa una [descrizione alternativa](/it/docs/Web/HTML/Reference/Elements/img#alt).

- [WebAIM: Links and Hypertext - Hypertext Links](https://webaim.org/techniques/hypertext/hypertext_links)
- [MDN Understanding WCAG, Guideline 3.2 explanations](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Understandable#guideline_3.2_—_predictable_make_web_pages_appear_and_operate_in_predictable_ways)
- [G200: Opening new windows and tabs from a link only when necessary | W3C Techniques for WCAG 2.0](https://www.w3.org/TR/WCAG20-TECHS/G200.html)
- [G201: Giving users advanced warning when opening a new window | W3C Techniques for WCAG 2.0](https://www.w3.org/TR/WCAG20-TECHS/G201.html)

### Link di salto

Un link di salto, noto anche come skipnav, è un `a` element posizionato il più vicino possibile all'elemento di apertura {{HTMLElement("body")}} che collega all'inizio del contenuto principale della pagina. Questo link consente alle persone di bypassare il contenuto ripetuto su più pagine di un sito web, come l'intestazione e la navigazione principale di un sito web.

I link di salto sono particolarmente utili per le persone che navigano con l'aiuto della tecnologia assistiva come controllo a pulsante, comandi vocali, o bastoncini/mestoli per la testa, dove l'atto di muoversi tra link ripetitivi può essere un compito laborioso.

- [WebAIM: "Skip Navigation" Links](https://webaim.org/techniques/skipnav/)
- [How–to: Use Skip Navigation links - The A11Y Project](https://www.a11yproject.com/posts/skip-nav-links/)
- [MDN Understanding WCAG, Guideline 2.4 explanations](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Operable#guideline_2.4_%e2%80%94_navigable_provide_ways_to_help_users_navigate_find_content_and_determine_where_they_are)
- [Understanding Success Criterion 2.4.1 | W3C Understanding WCAG 2.0](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-skip.html)

### Prossimità

Grandi quantità di contenuto interattivo — compresi i tag di ancoraggio — posizionati in stretta prossimità visiva tra di loro dovrebbero avere spazio inserito per separarli. Questo spazio è utile per le persone che soffrono di problemi di controllo motorio fine e che accidentalmente possono attivare il contenuto interattivo sbagliato durante la navigazione.

Lo spazio può essere creato utilizzando le proprietà CSS come {{CSSxRef("margin")}}.

- [Hand tremors and the giant-button-problem - Axess Lab](https://axesslab.com/hand-tremors/)

## Test di abilità

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Veda [Test di abilità: Accessibilità HTML](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/HTML) per verificare se ha mantenuto queste informazioni prima di procedere.

## Riepilogo

Ora dovrebbe essere ben versato nello scrivere HTML accessibile per la maggior parte delle occasioni. Il nostro articolo sui concetti di base di WAI-ARIA aiuterà a colmare le lacune di questa conoscenza, ma questo articolo si è preso cura delle basi. Prossimamente esploreremo CSS e JavaScript, e come l'accessibilità è influenzata dal loro buon o cattivo uso.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Tooling","Learn_web_development/Core/Accessibility/CSS_and_JavaScript", "Learn_web_development/Core/Accessibility")}}
