---
title: "HTML: Una buona base per l'accessibilità"
short-title: HTML Accessibile
slug: Learn_web_development/Core/Accessibility/HTML
l10n:
  sourceCommit: b2c8dcdae36907a87d1d1b9393ca4a35ebc765d6
---

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Tooling","Learn_web_development/Core/Accessibility/CSS_and_JavaScript", "Learn_web_development/Core/Accessibility")}}

Una gran parte dei contenuti web può essere resa accessibile semplicemente assicurandosi che gli elementi del linguaggio di marcatura Hypertext Markup Language corretti siano usati per il giusto scopo in ogni momento. Questo articolo esamina in dettaglio come l'HTML può essere utilizzato per garantire la massima accessibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Familiarità con <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, una <a href="/it/docs/Learn_web_development/Core/Accessibility/What_is_accessibility">comprensione di base dei concetti di accessibilità</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Utilizzare HTML semantico, alias "L'elemento giusto per il lavoro giusto", poiché il browser fornisce numerose funzioni di accessibilità integrate.</li>
          <li>Migliori pratiche accessibili come testi alternativi, buone pratiche per i collegamenti, etichette dei moduli, intestazioni e suddivisioni di righe e colonne delle tabelle.</li>
          <li>Utilizzare un linguaggio semplice e chiaro, evitando gergalità e abbreviazioni ove possibile e fornendo definizioni laddove non sia possibile.</li>
          <li>Il concetto e la pratica dell'accessibilità tramite tastiera.</li>
          <li>L'importanza dell'ordine di origine.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## HTML e accessibilità

Mentre impari di più sull'HTML — leggi più risorse, guarda più esempi, ecc. — vedrai ricorrere un tema comune: l'importanza di utilizzare HTML semantico (a volte chiamato POSH, o Plain Old Semantic HTML). Questo significa utilizzare gli elementi HTML corretti per il loro scopo previsto il più possibile.

Potresti chiederti perché ciò sia così importante. Dopotutto, puoi utilizzare una combinazione di CSS e JavaScript per far comportare qualsiasi elemento HTML nel modo che desideri. Ad esempio, un pulsante di controllo per riprodurre un video sul tuo sito potrebbe essere marcato in questo modo:

```html
<div>Play video</div>
```

Ma come vedrai in modo più approfondito in seguito, ha senso utilizzare l'elemento corretto per il lavoro:

```html
<button>Play video</button>
```

Non solo i `<button>` di HTML hanno qualche stile adeguato applicato di default (che probabilmente vorrai sovrascrivere), ma hanno anche accessibilità da tastiera integrata — gli utenti possono navigare tra i bottoni usando il tasto <kbd>Tab</kbd> e attivare la loro selezione usando <kbd>Spazio</kbd>, <kbd>Return</kbd> o <kbd>Invio</kbd>.

L'HTML semantico non richiede più tempo per essere scritto rispetto alla marcatura non semantica (cattiva) se lo fai in modo coerente dall'inizio del tuo progetto. Ancora meglio, la marcatura semantica ha altri benefici oltre all'accessibilità:

1. **Più facile da sviluppare** — come menzionato sopra, ottieni alcune funzioni gratuitamente, oltre ad essere probabilmente più facile da comprendere.
2. **Migliore sui dispositivi mobile** — l'HTML semantico è probabilmente più leggero in termini di dimensioni del file rispetto al codice spaghetti non semantico e più facile da rendere responsivo.
3. **Buono per la SEO** — i motori di ricerca danno più importanza alle parole chiave all'interno delle intestazioni, collegamenti, ecc., rispetto alle parole chiave incluse in `<div>` non semantici, ecc., quindi i tuoi documenti saranno più facilmente trovabili dai clienti.

Procediamo e guardiamo più da vicino l'HTML accessibile.

## Buona semantica

Abbiamo già parlato dell'importanza della semantica corretta e del perché dovremmo usare l'elemento HTML giusto per il lavoro. Questo non può essere ignorato, poiché è uno dei principali luoghi in cui l'accessibilità è gravemente compromessa se non gestita correttamente.

Là fuori sul web, la verità è che le persone fanno cose molto strane con la marcatura HTML. Spesso, l'uso improprio dell'HTML è dovuto a pratiche legacy che non sono ancora state rimosse, ma a volte accade perché gli autori non sanno meglio. Qualunque sia il caso, dovresti sostituire il codice cattivo con una marcatura semantica buona ovunque possibile, sia nelle pagine HTML statiche che nell'HTML generato dinamicamente dal codice [server-side](/it/docs/Learn_web_development/Extensions/Server-side) o dai [framework JavaScript client-side](/it/docs/Learn_web_development/Core/Frameworks_libraries) come React.

A volte non sei in grado di eliminare il cattivo markup — le tue pagine potrebbero dipendere dal codice server-side o da componenti web/framework che non puoi controllare, o potresti avere contenuti di terze parti sulla tua pagina (come banner pubblicitari).

L'obiettivo non è "tutto o niente"; ogni miglioramento che puoi fare aiuterà la causa dell'accessibilità.

### Utilizzare contenuti di testo ben strutturati

Uno dei migliori aiuti all'accessibilità che un utente di screen reader può avere è un'eccellente struttura di testo con intestazioni, paragrafi, elenchi, ecc. Un buon esempio semantico potrebbe apparire come segue:

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

Abbiamo preparato una versione con testo più lungo che puoi provare con uno screen reader (vedi [good-semantics.html](https://mdn.github.io/learning-area/accessibility/html/good-semantics.html)). Se provi a navigare attraverso questo, vedrai che è abbastanza facile da navigare:

1. Lo screen reader legge ogni intestazione mentre procedi attraverso il contenuto, notificandoti cos'è un'intestazione, cos'è un paragrafo, ecc.
2. Si ferma dopo ciascun elemento, permettendoti di andare al ritmo che ti è comodo.
3. Puoi saltare all'intestazione successiva/precedente in molti screen reader.
4. Puoi anche visualizzare un elenco di tutte le intestazioni in molti screen reader, permettendoti di usarle come una pratica tabella dei contenuti per trovare contenuti specifici.

Le persone a volte scrivono intestazioni, paragrafi, ecc. utilizzando interruzioni di riga e aggiungendo elementi HTML solo per lo stile, qualcosa come il seguente:

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

Se provi la nostra versione più lunga con uno screen reader (vedi [bad-semantics.html](https://mdn.github.io/learning-area/accessibility/html/bad-semantics.html)), non avrai una buona esperienza — lo screen reader non ha nulla da utilizzare come punti di riferimento, quindi non puoi recuperare una tabella dei contenuti utile, e l'intera pagina è vista come un blocco gigante, quindi viene letta in una volta sola, tutta in una volta.

Ci sono anche altri problemi oltre l'accessibilità — è più difficile stilizzare il contenuto usando CSS o manipolarlo con JavaScript, ad esempio, perché non ci sono elementi da usare come selettori.

### Utilizzare un linguaggio chiaro

Il linguaggio che usi può anche influenzare l'accessibilità. In generale, dovresti usare un linguaggio chiaro che non sia eccessivamente complesso e non usi gergo o termini gergali non necessari. Questo non solo avvantaggia le persone con disabilità cognitive o altre disabilità; avvantaggia i lettori per i quali il testo non è scritto nella loro lingua madre, i giovani... tutti, in realtà! Inoltre, dovresti cercare di evitare di usare linguaggio e caratteri che non vengono letti chiaramente dallo screen reader. Ad esempio:

- Non usare trattini se puoi evitarlo. Invece di scrivere 5–7, scrivi da 5 a 7.
- Espandi le abbreviazioni — invece di scrivere Jan, scrivi Gennaio.
- Espandi gli acronimi, almeno una o due volte, poi usa il tag [`<abbr>`](/it/docs/Web/HTML/Reference/Elements/abbr) per descriverli.

### Strutturare logicamente le sezioni della pagina

Dovresti utilizzare gli appropriati [elementi di divisione](/it/docs/Web/HTML/Reference/Elements#content_sectioning) per strutturare le tue pagine web, ad esempio la navigazione ({{htmlelement("nav")}}), il piè di pagina ({{htmlelement("footer")}}) e le unità di contenuto ripetute ({{htmlelement("article")}}). Questi forniscono ulteriore semantica per gli screen reader (e altri strumenti) per dare agli utenti ulteriori indizi sul contenuto che stanno navigando.

Ad esempio, una struttura di contenuti moderna potrebbe apparire come segue:

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

Puoi trovare un [esempio completo qui](https://mdn.github.io/learning-area/html/introduction-to-html/document_and_website_structure/).

Oltre a avere una buona semantica e un layout attraente, il tuo contenuto dovrebbe avere senso logico nel suo ordine di origine — puoi sempre posizionarlo dove vuoi usando CSS in seguito, ma dovresti ottenere l'ordine di origine corretto all'inizio, in modo che ciò che viene letto loro dagli screen readers abbia senso.

### Utilizzare controlli UI semantici ove possibile

Per controlli UI intendiamo le parti principali dei documenti web con cui gli utenti interagiscono — più comunemente bottoni, link, e controlli dei moduli. In questa sezione, esamineremo le principali preoccupazioni relative all'accessibilità di tali controlli. Articoli successivi su WAI-ARIA e multimedia esamineranno altri aspetti dell'accessibilità UI.

Un aspetto chiave dell'accessibilità dei controlli UI è che, per impostazione predefinita, i browser consentono di manipolarli tramite tastiera. Puoi provare questo usando il nostro esempio [native-keyboard-accessibility.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html) (vedi il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html)). Aprilo in una nuova scheda e prova a premere il tasto tab; dopo alcuni pressioni, dovresti vedere il focus del tab cominciare a muoversi attraverso i diversi elementi focalizzabili. Gli elementi focalizzati ricevono uno stile di evidenziazione predefinito in ogni browser (differisce leggermente tra diversi browser), in modo che tu possa capire quale elemento è focalizzato.

![Tre pulsanti con il testo "Click me!", "Click me too!", e "And me!" al loro interno rispettivamente. Il terzo pulsante ha un contorno blu attorno ad esso per indicare il focus attuale del tab.](button-focused-unfocused.png)

> [!NOTE]
> Puoi abilitare una sovrapposizione che mostra l'ordine del tab della pagina negli strumenti per sviluppatori. Per maggiori informazioni, vedi: [Accessibility Inspector > Show web page tabbing order](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html#show-web-page-tabbing-order).

Puoi quindi premere Enter/Return per seguire un link focalizzato o premere un pulsante (abbiamo incluso del JavaScript per fare in modo che i pulsanti mostrino un messaggio di avviso), o iniziare a digitare per inserire testo in un input di testo. Altri elementi di modulo hanno controlli diversi; ad esempio, l'elemento {{htmlelement("select")}} può mostrarti le sue opzioni e permetterti di navigare tra esse utilizzando i tasti freccia su e giù.

Ottieni essenzialmente questo comportamento gratuitamente, semplicemente usando gli elementi appropriati, ad esempio:

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

Ciò significa usare collegamenti, bottoni, elementi di modulo e etichette in modo appropriato (incluso l'elemento {{htmlelement("label")}} per i controlli dei moduli).

Tuttavia, questo è un altro caso in cui le persone a volte fanno cose strane con l'HTML. Ad esempio, a volte vedi i bottoni contrassegnati usando {{htmlelement("div")}}, ad esempio:

```html example-bad
<div data-message="This is from the first button">Click me!</div>
<div data-message="This is from the second button">Click me too!</div>
<div data-message="This is from the third button">And me!</div>
```

Ma usare un tale codice non è consigliato — perdi immediatamente l'accessibilità nativa attraverso la tastiera che avresti avuto se avessi semplicemente usato elementi {{htmlelement("button")}}, inoltre non ottieni nessuno degli stili CSS predefiniti che i bottoni ottengono. Nei rari casi in cui è necessario utilizzare un elemento non-button per un bottone, utilizza il [`ruolo button`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/button_role) e implementa tutti i comportamenti predefiniti del bottone, incluso il supporto per tastiera e mouse.

#### Ricostruire l'accessibilità tramite tastiera

Riaggiungere tali vantaggi richiede un po' di lavoro (puoi vedere un esempio nel nostro esempio [fake-div-buttons.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html) — vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/accessibility/fake-div-buttons.html)). Qui abbiamo dato ai nostri fake `<div>` bottoni la capacità di essere focalizzati (incluso tramite tab) attribuendo a ciascuno il attributo `tabindex="0"`. Includiamo anche `role="button"` affinché gli utenti di screen reader sappiano che possono focalizzarsi ed interagire con l'elemento:

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

Fondamentalmente, l'attributo [`tabindex`](/it/docs/Web/HTML/Reference/Global_attributes/tabindex) è inteso principalmente per permettere agli elementi focusabili di avere un ordine di tab personalizzato (specificato in ordine numerico positivo), invece di essere semplicemente tabulati nell'ordine di origine predefinito. Questo è quasi sempre una cattiva idea, poiché può causare notevole confusione. Usalo solo se realmente necessario, ad esempio, se il layout mostra le cose in un ordine visivo molto diverso dal codice sorgente, e vuoi far funzionare le cose in modo più logico. Ci sono altre due opzioni per `tabindex`:

- `tabindex="0"` — come indicato sopra, questo valore permette agli elementi che normalmente non sono tabbabili di diventare tabbabili. Questo è il valore più utile di `tabindex`.
- `tabindex="-1"` — questo permette agli elementi che normalmente non sono tabbabili di ricevere focus programmaticamente, ad esempio, tramite JavaScript, o come target di collegamenti.

Anche se l'aggiunta sopra ci permette di tabulare ai bottoni, non ci permette di attivarli tramite il tasto <kbd>Invio</kbd>/<kbd>Return</kbd>. Per fare ciò, abbiamo dovuto aggiungere il seguente pezzo di JavaScript:

```js
document.onkeydown = (e) => {
  // The Enter/Return key
  if (e.key === "Enter") {
    document.activeElement.click();
  }
};
```

Qui aggiungiamo un listener all'oggetto `document` per rilevare quando un bottone è stato premuto sulla tastiera. Controlliamo quale bottone è stato premuto tramite la proprietà [`key`](/it/docs/Web/API/KeyboardEvent/key) dell'oggetto evento; se il tasto premuto è <kbd>Invio</kbd>/<kbd>Return</kbd>, eseguiamo la funzione memorizzata nel gestore `onclick` del bottone utilizzando `document.activeElement.click()`. [`activeElement`](/it/docs/Web/API/Document/activeElement) ci dà l'elemento attualmente focalizzato sulla pagina.

Questo è un sacco di sbattimento in più per ricostruire la funzionalità. E c'è la probabilità che ci siano altri problemi con essa. **Meglio usare semplicemente l'elemento giusto per il lavoro giusto fin dall'inizio.**

#### Utilizzare etichette di testo significative

Le etichette di testo dei controlli UI sono molto utili per tutti gli utenti, ma ottenere che siano corrette è particolarmente importante per gli utenti con disabilità.

Dovresti assicurarti che le etichette di testo su bottoni e collegamenti siano comprensibili e distintive. Non usare semplicemente "Clicca qui" per le tue etichette, poiché gli utenti di screen reader talvolta estraggono un elenco di bottoni e controlli moduli. Lo screenshot seguente mostra i nostri controlli elencati da VoiceOver su Mac.

![Elenco di etichette di input modulo elencate dal software VoiceOver su Mac. Questo elenco contiene etichette prive di senso come 'menu felice bottone` date a vari controlli modulo come bottone, campo di testo e collegamento.](voiceover-formcontrols.png)

Assicurati che le tue etichette abbiano senso fuori dal contesto, lette da sole, e anche nel contesto del paragrafo in cui si trovano. Ad esempio, il seguente mostra un esempio di buon testo per il collegamento:

```html example-good
<p>
  Whales are really awesome creatures.
  <a href="whales.html">Find out more about whales</a>.
</p>
```

ma questo è un cattivo testo per il collegamento:

```html example-bad
<p>
  Whales are really awesome creatures. To find out more about whales,
  <a href="whales.html">click here</a>.
</p>
```

> [!NOTE]
> Puoi trovare molte più informazioni sull'implementazione dei collegamenti e sulle migliori pratiche nel nostro articolo [Creare collegamenti](/it/docs/Learn_web_development/Core/Structuring_content/Creating_links). Puoi anche vedere alcuni buoni e cattivi esempi in [good-links.html](https://mdn.github.io/learning-area/accessibility/html/good-links.html) e [bad-links.html](https://mdn.github.io/learning-area/accessibility/html/bad-links.html).

Le etichette di modulo sono anche importanti per darti un'idea di quello che devi inserire in ciascun input del modulo. Il seguente potrebbe sembrare un esempio ragionevolmente buono:

```html example-bad
Fill in your name: <input type="text" id="name" name="name" />
```

Tuttavia, questo non è molto utile per gli utenti disabili. Non c'è nulla nell'esempio sopra per associare chiaramente l'etichetta all'input del modulo e renderlo chiaro su come compilarlo se non puoi vederlo. Se accedi a questo con alcuni screen reader, potresti ricevere solo una descrizione del tipo "testo da modificare".

Il seguente è un esempio molto migliore:

```html example-good
<div>
  <label for="name">Fill in your name:</label>
  <input type="text" id="name" name="name" />
</div>
```

Con il codice come questo, l'etichetta sarà chiaramente associata all'input; la descrizione sarà più simile a "Inserisci il tuo nome: testo da modificare."

![Una buona etichetta di modulo che legge 'Inserisci il tuo nome' è data a un controllo modulo di input di testo.](voiceover-good-form-label.png)

Come bonus aggiuntivo, nella maggior parte dei browser associare un'etichetta a un input del modulo significa che puoi cliccare sull'etichetta per selezionare o attivare l'elemento del modulo. Questo dà all'input un'area di colpo più grande, rendendolo più facile da selezionare.

> [!NOTE]
> Puoi vedere alcuni buoni e cattivi esempi di modulo in [good-form.html](https://mdn.github.io/learning-area/accessibility/html/good-form.html) e [bad-form.html](https://mdn.github.io/learning-area/accessibility/html/bad-form.html).

Puoi trovare una buona spiegazione dell'importanza delle etichette di testo corrette e su come investigare i problemi delle etichette di testo usando il [Firefox Accessibility Inspector](https://firefox-source-docs.mozilla.org/devtools-user/accessibility_inspector/index.html), nel seguente video:

{{EmbedYouTube("YhlAVlfH0rQ")}}

## Tabelle di dati accessibili

Una tabella di dati di base può essere scritta con una marcatura molto semplice, ad esempio:

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

Ma questo ha dei problemi — non c'è modo per un utente di screen reader di associare righe o colonne come raggruppamenti di dati. Per farlo, devi sapere quali sono le righe di intestazione e se stanno guidando righe, colonne, ecc. Questo può essere fatto solo visivamente per la tabella sopra (vedi [bad-table.html](https://mdn.github.io/learning-area/accessibility/html/bad-table.html) e prova l'esempio tu stesso).

Ora dai un'occhiata al nostro [esempio di tabella delle band punk](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/styling-tables/punk-bands-complete.html) — puoi vedere alcuni aiuti all'accessibilità al lavoro qui:

- Le intestazioni di tabella sono definite usando elementi {{htmlelement("th")}} — puoi anche specificare se sono intestazioni per righe o colonne utilizzando l'attributo `scope`. Questo ti dà gruppi completi di dati che possono essere consumati dai lettori di schermo come unità singole.
- L'elemento {{htmlelement("caption")}} e l'attributo `<table>` `summary` svolgono compiti simili — agiscono come alt text per una tabella, fornendo un utile riassunto rapido del contenuto della tabella ad un utente di screen reader. L'elemento `<caption>` è generalmente preferito poiché rende il suo contenuto accessibile anche agli utenti vedenti, che potrebbero trovarlo utile. Non hai davvero bisogno di entrambi.

> [!NOTE]
> Vedi il nostro articolo [Accessibilità delle tabelle HTML](/it/docs/Learn_web_development/Core/Structuring_content/Table_accessibility) per ulteriori dettagli sulle tabelle di dati accessibili.

## Alternative testuali

Mentre i contenuti testuali sono intrinsecamente accessibili, lo stesso non può necessariamente essere detto per i contenuti multimediali — i contenuti d'immagine e video non possono essere visti dalle persone con disabilità visive, e i contenuti audio non possono essere uditi da persone con disabilità uditive. Copriamo i contenuti video e audio in dettaglio nel [Multimedia accessibile](/it/docs/Learn_web_development/Core/Accessibility/Multimedia), ma per questo articolo esamineremo l'accessibilità per il semplice elemento {{htmlelement("img")}}.

Abbiamo preparato un semplice esempio, [accessible-image.html](https://mdn.github.io/learning-area/accessibility/html/accessible-image.html), che presenta quattro copie della stessa immagine:

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

La prima immagine, quando viene vista da un lettore di schermo, non offre realmente agli utenti molto aiuto — VoiceOver, ad esempio, legge "/dinosaur.png, immagine". Legge il nome del file per fornire qualche aiuto. In questo esempio l'utente saprà almeno che si tratta di un dinosauro di qualche tipo, ma spesso i file possono essere caricati con nomi di file generati in modo automatico (ad esempio, da una fotocamera digitale) e questi nomi di file probabilmente non fornirebbero alcun contesto al contenuto dell'immagine.

> [!NOTE]
> Questo è il motivo per cui non dovresti mai includere contenuti testuali all'interno di un'immagine — i lettori di schermo non possono accedervi. Ci sono anche altri svantaggi — non puoi selezionarlo e copiarlo/incollarlo. Basta non farlo!

Quando un lettore di schermo incontra la seconda immagine, legge l'intero attributo alt — "Un Tirannosaurus Rex rosso: Un dinosauro a due zampe che sta in piedi come un umano, con piccole braccia e una grande testa con molti denti affilati.".

Questo evidenzia l'importanza non solo di utilizzare nomi di file significativi nel caso in cui il cosiddetto **alt text** non sia disponibile, ma anche di assicurarsi che il testo alternativo sia fornito negli attributi `alt` ove possibile.

Nota che il contenuto dell'attributo `alt` dovrebbe sempre fornire una rappresentazione diretta dell'immagine e di ciò che trasmette visivamente. L'alt dovrebbe essere breve e conciso e includere tutte le informazioni trasmesse dall'immagine che non sono duplicate nel testo circostante.

Il contenuto dell'attributo `alt` per una singola immagine varia in base al contesto. Ad esempio, se la foto di Fluffy è un'immagine avatar accanto a una recensione del cibo per cani Yuckymeat, `alt="Fluffy"` è appropriato. Se la foto fa parte della pagina di adozione di Fluffy presso la società di soccorso animali, le informazioni trasmesse nell'immagine che sono rilevanti per un potenziale genitore di cani e non duplicate nel testo circostante dovrebbero essere incluse. Una descrizione più lunga, come `alt="Fluffy, un terrier tricolore con pelo molto corto, con una pallina da tennis in bocca."` è appropriata. Poiché il testo circostante probabilmente ha le dimensioni e la razza di Fluffy, questo non viene incluso nell'`alt`. Tuttavia, poiché la biografia del cane probabilmente non include lunghezza del pelo, colori, o preferenze di giocattoli, che il potenziale genitore ha bisogno di sapere, è incluso. L'immagine è all'aperto, o Fluffy ha un collare rosso con un guinzaglio blu? Non è importante in termini di adozione dell'animale e quindi non è incluso. Tutte le informazioni che l'immagine trasmette che un utente vedente può accedere e che sono rilevanti per il contesto sono ciò che deve essere trasmesso; nient'altro. Tienilo breve, preciso e utile.

Qualsiasi conoscenza personale o descrizione extra non dovrebbe essere inclusa qui, in quanto non è utile per le persone che non hanno visto l'immagine prima. Se il giocattolo è il preferito di Fluffy o se un utente vedente non può conoscerlo dall'immagine, allora non includerlo.

Una cosa da considerare è se le tue immagini abbiano un significato all'interno del tuo contenuto, o se siano puramente per decorazione visiva, e quindi non abbiano significato. Se sono decorative, è meglio scrivere un testo vuoto come valore per l'attributo `alt` (vedi [Attributi alt vuoti](#attributi_alt_vuoti)) o semplicemente includerle nella pagina come immagini di sfondo CSS.

> [!NOTE]
> Leggi [Immagini HTML](/it/docs/Learn_web_development/Core/Structuring_content/HTML_images) e [Immagini Responsive](/it/docs/Web/HTML/Guides/Responsive_images) per molte più informazioni sull'implementazione delle immagini e sulle migliori pratiche.
> Puoi anche controllare [Un albero delle decisioni alt](https://www.w3.org/WAI/tutorials/images/decision-tree/) per imparare come usare un attributo alt per le immagini in varie situazioni.

Se vuoi fornire ulteriori informazioni contestuali, dovresti inserirle nel testo circostante l'immagine, o all'interno di un attributo `title`, come mostrato sopra. In questo caso, la maggior parte degli screen reader leggerà l'alt text, l'attributo title e il nome del file. Inoltre, i browser visualizzano il testo del titolo come tooltip quando sopra di esse si passa il mouse.

![Screenshot di un Tirannosaurus Rex rosso con il testo "Il dinosauro rosso di mozilla" visualizzato come tooltip al passaggio del mouse.](title-attribute.png)

Diamo un altro rapido sguardo al quarto metodo:

```html
<img src="dinosaur.png" aria-labelledby="dino-label" />

<p id="dino-label">The Mozilla red Tyrannosaurus…</p>
```

In questo caso, non stiamo usando l'attributo `alt` — invece, abbiamo presentato la nostra descrizione dell'immagine come un paragrafo di testo normale, gli abbiamo dato un `id`, e poi abbiamo usato l'attributo `aria-labelledby` per riferirci a quell'`id`, il che fa sì che i lettori di schermo utilizzino quel paragrafo come testo alternativo/etichetta per quell'immagine. Questo è particolarmente utile se vuoi usare lo stesso testo come etichetta per più immagini — qualcosa che non è possibile con `alt`.

> **Nota:** [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) fa parte della specifica [WAI-ARIA](https://www.w3.org/TR/wai-aria-1.1/), che permette agli sviluppatori di aggiungere semantica extra al loro markup per migliorare l'accessibilità dello screen reader dove necessario.

### Figure e didascalie delle figure

HTML include due elementi — {{htmlelement("figure")}} e {{htmlelement("figcaption")}} — che associano una figura di qualche tipo (potrebbe essere qualsiasi cosa, non necessariamente un'immagine) con una didascalia della figura:

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

Sebbene ci sia supporto variabile da parte dei lettori di schermo per l'associazione di didascalie di figure con le loro figure, l'inclusione di [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby) o [`aria-describedby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-describedby) crea l'associazione se non è presente. Detto questo, la struttura dell'elemento è utile per lo styling CSS, oltre a fornire un modo per posizionare una descrizione dell'immagine accanto ad essa nell'origine.

### Attributi alt vuoti

```html
<h3>
  <img src="article-icon.png" alt="" />
  Tyrannosaurus Rex: the king of the dinosaurs
</h3>
```

Ci possono essere momenti in cui un'immagine è inclusa nel design di una pagina, ma il suo scopo principale è per decorazione visiva. Noterai, nell'esempio di codice sopra, che l'attributo `alt` dell'immagine è vuoto — questo serve a far riconoscere l'immagine agli screen reader, ma a non tentare di descrivere l'immagine (invece direbbero solo "immagine", o simili).

Il motivo per usare un `alt` vuoto invece di non includerlo è perché molti screen reader annunciano l'intero URL dell'immagine se non viene fornito un `alt`. Nell'esempio sopra, l'immagine funge da decorazione visiva per l'intestazione a cui è associata. In casi come questo, e in casi in cui un'immagine è solo decorazione e non ha valore di contenuto, dovresti includere un `alt` vuoto nei tuoi elementi `img`. Un'altra alternativa è usare l'attributo aria [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles) [`role="presentation"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/presentation_role) poiché questo blocca anche gli screen reader dal leggere il testo alternativo.

> [!NOTE]
> Se possibile, dovresti usare CSS per visualizzare immagini che sono solo decorative.

## Più sui collegamenti

I collegamenti (l'elemento [`<a>`](/it/docs/Web/HTML/Reference/Elements/a) con un attributo `href`), a seconda di come vengono usati, possono aiutare o danneggiare l'accessibilità. Per impostazione predefinita, i collegamenti sono accessibili nell'aspetto. Possono migliorare l'accessibilità aiutando un utente a navigare rapidamente a diverse sezioni di un documento. Possono anche danneggiare l'accessibilità se il loro stile accessibile è stato rimosso o se JavaScript li fa comportare in modi inattesi.

### Stile dei collegamenti

Per impostazione predefinita, i collegamenti sono visivamente diversi dal resto del testo sia per colore che per [text-decoration](/it/docs/Web/CSS/text-decoration), con i collegamenti di colore blu e sottolineati per impostazione predefinita, viola e sottolineati se visitati, e con un [focus-ring](/it/docs/Web/CSS/:focus) quando ricevono focus da tastiera.

Il colore non dovrebbe essere utilizzato come unico metodo per distinguere i collegamenti dal contenuto non collegante. Colore del testo del collegamento, come tutto il testo, deve essere significativamente diverso dal colore di sfondo ([un contrasto di 4.5:1](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Perceivable/Color_contrast)). Inoltre, i collegamenti dovrebbero essere visivamente significativamente diversi dal testo non collegante, con un requisito minimo di contrasto di 3:1 tra il testo del collegamento e il testo circostante e tra gli stati di default, visitato e focus/attivo e un contrasto di 4.5:1 tra tutti quei colori di stato e il colore di sfondo.

### Eventi `onclick`

I tag di ancoraggio sono spesso abusati con l'evento `onclick` per creare pseudo-pulsanti impostando **href** su `"#"` o `"javascript:void(0)"` per evitare che la pagina si aggiorni.

Questi valori causano un comportamento inatteso quando si copiano o si trascinano i collegamenti, si aprono collegamenti in una nuova scheda o finestra, si aggiungono ai preferiti, e quando JavaScript è ancora in fase di download, si verifica un errore, o è disabilitato. Questo trasmette anche semantiche errate alle tecnologie assistive (ad esempio, screen reader). In questi casi, si consiglia di usare un {{HTMLElement("button")}} invece. In generale, dovresti usare un'ancoraggio solo per la navigazione usando un URL corretto.

### Collegamenti esterni e collegamenti a risorse non HTML

I collegamenti che si aprono in una nuova scheda o finestra tramite la dichiarazione `target="_blank"` e i collegamenti il cui valore `href` punta a una risorsa file dovrebbero includere un'indicazione sul comportamento che si verificherà quando il collegamento viene attivato.

Le persone che sperimentano condizioni di bassa visione, che navigano con l'aiuto di tecnologie di lettura dello schermo, o che hanno preoccupazioni cognitive possono essere confuse quando la nuova scheda, finestra o applicazione viene aperta inaspettatamente. Versioni più vecchie di software di lettura dello schermo potrebbero non annunciare nemmeno il comportamento.

#### Collegamento che apre una nuova scheda o finestra

```html
<a target="_blank" href="https://www.wikipedia.org/"
  >Wikipedia (opens in a new window)</a
>
```

#### Collegamento a una risorsa non HTML

```html
<a target="_blank" href="2017-annual-report.ppt"
  >2017 Annual Report (PowerPoint)</a
>
```

Se viene utilizzata un'icona al posto del testo per significare questo tipo di comportamento del collegamento, assicurati che includa una [descrizione alternativa](/it/docs/Web/HTML/Reference/Elements/img#alt).

- [WebAIM: Collegamenti e Ipertesto - Collegamenti ipertestuali](https://webaim.org/techniques/hypertext/hypertext_links)
- [MDN Understanding WCAG, Guideline 3.2 explanations](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Understandable#guideline_3.2_—_predictable_make_web_pages_appear_and_operate_in_predictable_ways)
- [G200: Aprire nuove finestre e schede da un collegamento solo quando necessario | Tecniche W3C per WCAG 2.0](https://www.w3.org/TR/WCAG20-TECHS/G200.html)
- [G201: Dare agli utenti un avviso avanzato quando si apre una nuova finestra | Tecniche W3C per WCAG 2.0](https://www.w3.org/TR/WCAG20-TECHS/G201.html)

### Collegamenti di salto

Un collegamento di salto, noto anche come skipnav, è un elemento `a` posizionato il più vicino possibile all'apertura dell'elemento {{HTMLElement("body")}} che collega all'inizio del contenuto principale della pagina. Questo collegamento consente alle persone di bypassare il contenuto ripetuto in più pagine su un sito web, come l'intestazione e la navigazione principale di un sito web.

I collegamenti di salto sono particolarmente utili per le persone che navigano con l'aiuto di tecnologie assistive come il controllo a interruttore, il comando vocale, o i bastoni/moletti bocca/testa, dove l'atto di spostarsi attraverso i collegamenti ripetitivi può essere un compito laborioso.

- [WebAIM: "Skip Navigation" Links](https://webaim.org/techniques/skipnav/)
- [Come fare: Usare i collegamenti di Skip Navigation - Il progetto A11Y](https://www.a11yproject.com/posts/skip-nav-links/)
- [MDN Understanding WCAG, Guideline 2.4 explanations](/it/docs/Web/Accessibility/Guides/Understanding_WCAG/Operable#guideline_2.4_%e2%80%94_navigable_provide_ways_to_help_users_navigate_find_content_and_determine_where_they_are)
- [Understanding Success Criterion 2.4.1 | W3C Understanding WCAG 2.0](https://www.w3.org/TR/UNDERSTANDING-WCAG20/navigation-mechanisms-skip.html)

### Prossimità

Grandi quantità di contenuto interattivo — incluso le ancore — posizionate in stretta prossimità visiva tra loro dovrebbe avere spazi inseriti per separarle. Questo spazio è utile per le persone che soffrono di problemi di controllo motorio fine e possono attivare accidentalmente il contenuto interattivo errato durante la navigazione.

Lo spazio può essere creato usando proprietà CSS come {{CSSxRef("margin")}}.

- [Tremori alle mani e il problema del bottone gigante - Axess Lab](https://axesslab.com/hand-tremors/)

## Metti alla prova le tue abilità

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Vedi [Metti alla prova le tue abilità: Accessibilità HTML](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/HTML) per verificare di aver trattenuto queste informazioni prima di procedere.

## Riepilogo

Dovresti ora essere ben informato sulla scrittura di HTML accessibile per la maggior parte delle occasioni. Il nostro articolo sui fondamenti di WAI-ARIA aiuterà a colmare le lacune in questa conoscenza, ma questo articolo si è occupato delle basi. Il prossimo passo esploreremo CSS e JavaScript, e come l'accessibilità è influenzata dal loro buon o cattivo uso.

{{PreviousMenuNext("Learn_web_development/Core/Accessibility/Tooling","Learn_web_development/Core/Accessibility/CSS_and_JavaScript", "Learn_web_development/Core/Accessibility")}}
