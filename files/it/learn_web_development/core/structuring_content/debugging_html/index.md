---
title: Debug del codice HTML
slug: Learn_web_development/Core/Structuring_content/Debugging_HTML
l10n:
  sourceCommit: 2066cc916dfdcbb782340bf0ce562b230e947cba
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Forms_challenge", "Learn_web_development/Core/Styling_basics", "Learn_web_development/Core/Structuring_content")}}

Scrivere HTML va bene, ma cosa succede se qualcosa va storto e non si riesce a capire dove si trova l'errore nel codice? Questo articolo introdurrà alcuni strumenti che possono aiutare a trovare e correggere gli errori in HTML.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base di HTML, come descritto in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica a livello di testo, come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > ed <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >. <a href="/it/docs/Learn_web_development/Core/Structuring_content/Structuring_documents"
          >HTML strutturale</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Le informazioni di base principali sul debug di HTML</li>
          <li>Uso dell'ispettore DOM nei DevTools del browser per approfondire il codice HTML.</li>
          <li>Esplorazione dei tipi comuni di errori HTML.</li>
          <li>Uso del <a href="https://validator.w3.org/">validatore HTML</a> per rilevare gli errori HTML.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Il debug non è spaventoso

Quando si scrive un qualsiasi tipo di codice, tutto va bene fino a quel temuto momento in cui si verifica un errore: è stato fatto qualcosa di sbagliato, quindi il codice non funziona, né del tutto né nel modo desiderato. Ad esempio, quanto segue mostra un errore segnalato durante il tentativo di {{Glossary("compile", "compilare")}} un semplice programma scritto nel linguaggio [Rust](https://rust-lang.org/).

![Una finestra della console che mostra il risultato del tentativo di compilare un programma Rust con una virgoletta mancante attorno a una stringa in un'istruzione di stampa. Il messaggio di errore segnalato è error: unterminated double quote string.](error-message.png)

In questo caso, il messaggio di errore è relativamente facile da comprendere: "unterminated double quote string". Osservando il listato, probabilmente si può capire come a `println!(Hello, world!");` possa logicamente mancare una doppia virgoletta. Tuttavia, i messaggi di errore possono diventare rapidamente più complicati e meno facili da interpretare man mano che i programmi diventano più grandi, e persino i casi semplici possono apparire un po' intimidatori a chi non sa nulla di Rust.

Il debug non deve però essere spaventoso: la chiave per sentirsi a proprio agio nello scrivere ed eseguire il debug di qualsiasi codice è avere familiarità sia con il linguaggio sia con gli strumenti associati.

## HTML e debug

HTML non è complicato da comprendere quanto Rust. HTML non viene compilato in una forma diversa prima dell'analisi (viene _interpretato_, non _compilato_). Inoltre, la sintassi degli {{Glossary("element", "elementi")}} HTML è probabilmente molto più semplice da comprendere rispetto a un "vero linguaggio di programmazione" come Rust, {{Glossary("JavaScript", "JavaScript")}} o {{Glossary("Python", "Python")}}.

Il modo in cui i browser analizzano HTML è molto più **permissivo** rispetto a quello in cui viene analizzata la maggior parte dei linguaggi di programmazione, e questo è sia un vantaggio sia uno svantaggio.

Ma prima di tutto, cosa si intende per permissivo? In generale, quando si fa qualcosa di sbagliato nel codice, si incontrano due tipi principali di errori:

- **Errori di sintassi**: sono errori di battitura nel codice che impediscono l'esecuzione del programma, come l'errore Rust mostrato in precedenza. Di solito sono facili da correggere, purché si conosca la sintassi del linguaggio e si sappia cosa significano i messaggi di errore.
- **Errori logici**: sono errori in cui la sintassi è effettivamente corretta, ma il codice non fa ciò che era previsto, quindi il programma viene eseguito in modo non corretto. Spesso sono più difficili da correggere degli errori di sintassi, poiché non esiste un messaggio di errore che indirizzi alla fonte dell'errore.

HTML non presenta errori di sintassi perché i browser lo analizzano in modo permissivo, ovvero la pagina viene comunque visualizzata anche se nel codice sorgente sono presenti errori di sintassi. I browser dispongono di regole integrate che specificano come interpretare il markup HTML scritto in modo errato (spesso chiamato markup **non valido** o **malformato**), modificandolo automaticamente in markup valido.

Ad esempio, il seguente frammento HTML contiene elementi annidati in modo non corretto:

```html example-bad
<p>I didn't expect to find the <em>next-door neighbor's <strong>cat</em></strong> here!</p>
```

Il tag di chiusura `</strong>` dovrebbe trovarsi prima del tag di chiusura `</em>`, ma non è così: si trova dopo.

Se si carica questo HTML in un browser e poi si osserva il [DOM sottoposto a rendering](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites#handling_html), si vedrà che il browser ha corretto l'annidamento:

```html example-good
<p>
  I didn't expect to find the
  <em>next-door neighbor's <strong>cat</strong></em> here!
</p>
```

Perché questo è sia un vantaggio sia uno svantaggio? In questo caso il browser ha creato il risultato previsto, ma, come si vedrà [più avanti](#your_turn_studying_html_using_the_dom_inspector), non è sempre così. Si otterrà sempre _qualcosa_ in esecuzione, ma il browser non interpreta sempre tutto correttamente, e questo può causare problemi. È meglio scrivere markup corretto fin dall'inizio.

> [!NOTE]
> HTML viene analizzato in modo permissivo perché, quando il web è stato creato, si è deciso che pubblicare i contenuti fosse più importante che assicurarsi che la sintassi fosse assolutamente corretta. Il web probabilmente non sarebbe popolare come lo è oggi se fosse stato più rigido fin dall'inizio.

Come si trovano quindi gli errori di markup? Più avanti verrà mostrato come trovare gli errori in HTML utilizzando uno strumento chiamato [validatore HTML](#validazione_html), ma prima verrà illustrato come ispezionare manualmente l'HTML usando un **ispettore DOM**, per poi esplorare i tipi di errori di markup che si potrebbero cercare e il modo in cui il browser potrebbe interpretarli.

## Uso dell'ispettore DOM

Tutti i browser moderni dispongono di un insieme di [strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) (devtools) integrati, che forniscono varie funzionalità per esaminare la pagina web caricata nella scheda corrente. Possono mostrare quale HTML viene sottoposto a rendering nella pagina, quale CSS viene applicato a ogni nodo DOM, quale JavaScript è in esecuzione nella pagina e altro ancora. Consentono inoltre di modificare il codice attualmente in esecuzione e di vedere l'effetto in tempo reale sulla pagina.

È possibile aprire i devtools in modo simile in ogni browser: consultare [Come aprire i devtools nel browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#how_to_open_the_devtools_in_your_browser) per scoprire come fare.

Per questo articolo, l'unica funzione dei devtools rilevante è l'**ispettore DOM**, che mostra il DOM HTML attualmente sottoposto a rendering e consente di modificarlo. Vediamolo ora:

1. Aprire i devtools nel browser.
2. Aprire l'ispettore DOM. Si trova nello stesso posto in ogni browser: la prima scheda nei devtools, all'inizio della riga. In Firefox è denominata _Inspector_, mentre in Safari, Edge e Chrome è denominata _Elements_. Questa dovrebbe essere la scheda selezionata per impostazione predefinita quando si aprono per la prima volta i devtools, ma occorre selezionarla se non lo è.
3. Esaminare la struttura ad albero del DOM mostrata nella scheda e notare come sia possibile fare clic sulle piccole frecce di espansione all'inizio di ciascun nodo DOM per espanderlo e comprimerlo, rivelando i nodi discendenti. È anche possibile usare i tasti freccia su e giù per spostarsi tra i nodi e i tasti freccia destra e sinistra per espandere e comprimere i nodi.
4. Provare anche a passare il puntatore sui nodi, oppure selezionarli con i tasti freccia, e notare come l'elemento su cui si trova attualmente il puntatore, o che è selezionato, venga evidenziato nella viewport.
5. È anche possibile modificare il DOM sottoposto a rendering. In questo articolo non verrà utilizzata la funzionalità di modifica, ma è possibile approfondire come usarla se suscita interesse.

## Prova: studiare HTML usando l'ispettore DOM

In questa sezione verrà studiato del codice usando l'ispettore DOM e si vedrà come il browser gestisce gli errori di markup comuni.

1. Per prima cosa, salvare il seguente listato di file HTML come `debug-example.html` in una posizione qualsiasi sul computer locale. Questa demo è stata deliberatamente scritta con alcuni errori integrati da esplorare.

   ```html-nolint
   <!doctype html>
   <html lang="en-US">
     <head>
       <meta charset="utf-8">
       <title>HTML debugging examples</title>
     </head>

     <body>
       <h1>HTML debugging examples</h1>
       <p>What causes errors in HTML?
       <ul>
         <li>Unclosed elements: If an element is <strong>not closed properly,then its effect can spread to areas you didn't intend
         <li>Badly nested elements: Nesting elements properly is also very important for code behaving correctly. <strong>strong <em>strong emphasized?</strong> what is this?</em>
         <li>Unclosed attributes: Another common source of HTML problems. Let's look at an example: <a href="https://www.mozilla.org/>link to Mozilla homepage</a>
       </ul>
     </body>
   </html>
   ```

2. Quindi, aprirlo in un browser. Verrà visualizzato qualcosa di simile:![Un semplice documento HTML con il titolo HTML debugging examples e alcune informazioni su errori HTML comuni, come elementi non chiusi, elementi annidati in modo errato e attributi non chiusi.](badly-formed-html.png)
3. L'aspetto non è subito ottimale; osserviamo il codice sorgente per capire perché (viene mostrato solo il contenuto del body):

   ```html
   <h1>HTML debugging examples</h1>

   <p>What causes errors in HTML?

   <ul>
     <li>Unclosed elements: If an element is <strong>not closed properly,
         then its effect can spread to areas you didn't intend

     <li>Badly nested elements: Nesting elements properly is also very important
         for code behaving correctly. <strong>strong <em>strong emphasized?</strong>
         what is this?</em>

     <li>Unclosed attributes: Another common source of HTML problems. Let's
         look at an example: <a href="https://www.mozilla.org/>link to Mozilla
         homepage</a>
   </ul>
   ```

4. Rivediamo i problemi:
   - Gli elementi {{htmlelement("p","paragrafo")}} e {{htmlelement("li","elemento dell'elenco")}} non hanno tag di chiusura. Osservando l'immagine sopra, non sembra che questo abbia compromesso troppo il rendering del markup, poiché è facile dedurre dove un elemento dovrebbe terminare e un altro iniziare.
   - Il primo elemento {{htmlelement("strong")}} non ha un tag di chiusura. Questo è un po' più problematico, perché non è facile capire dove l'elemento dovrebbe terminare. In effetti, tutto il resto del testo è stato sottoposto a rendering in grassetto.
   - Questa sezione è annidata in modo errato: `<strong>strong <em>strong emphasized?</strong> what is this?</em>`. Non è facile capire come sia stata interpretata a causa del problema precedente.
   - Al valore dell'attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) manca una doppia virgoletta di chiusura. Questo sembra aver causato il problema più grande: il link non è stato sottoposto a rendering.

5. Ora esaminiamo il DOM sottoposto a rendering, invece del codice sorgente. Per farlo, aprire l'ispettore DOM del browser. Verrà visualizzata una rappresentazione del markup sottoposto a rendering: ![L'ispettore HTML in Firefox, con il paragrafo dell'esempio evidenziato, che mostra il testo "What causes errors in HTML?" Qui è possibile vedere che l'elemento paragrafo è stato chiuso dal browser.](html-inspector.png)
6. Osservare come il browser abbia cercato di correggere gli errori HTML (la revisione è stata effettuata in Firefox; gli altri browser moderni _dovrebbero_ fornire lo stesso risultato):
   - Ai paragrafi e agli elementi dell'elenco sono stati aggiunti tag di chiusura.
   - Non è chiaro dove il primo elemento `<strong>` debba essere chiuso, quindi il browser ha racchiuso ogni blocco di testo separato nel proprio elemento `<strong>`, fino in fondo al documento.
   - L'annidamento errato è stato corretto dal browser come mostrato qui:

     ```html
     <strong>
       strong
       <em>strong emphasized?</em>
     </strong>
     <em> what is this?</em>
     ```

   - Il link con la doppia virgoletta mancante è stato eliminato completamente. L'ultimo elemento dell'elenco appare così:

     ```html
     <li>
       <strong>
         Unclosed attributes: Another common source of HTML problems. Let's look
         at an example:
       </strong>
     </li>
     ```

## Validazione HTML

Dall'esempio precedente si può vedere quanto sia importante assicurarsi che l'HTML sia ben formato. Ma come fare? In un piccolo esempio come quello visto sopra, è facile cercare tra le righe e trovare gli errori, ma cosa accade con un documento HTML enorme e complesso?

Lo strumento adatto a questo compito è il [Markup Validation Service](https://validator.w3.org/) (o **validatore HTML**), creato e mantenuto dal W3C, di cui si è appreso nel modulo [Il modello degli standard web](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model). Il validatore accetta un documento HTML come input, lo analizza e fornisce un rapporto che indica cosa non va nell'HTML.

![La homepage del validatore HTML](validator.png)

Per specificare l'HTML da validare, è possibile fornire un indirizzo web, caricare un file HTML oppure inserire direttamente del codice HTML.

## Validare un documento HTML

In questa attività verrà provato il validatore HTML. Verrà validato lo stesso HTML studiato in precedenza con l'ispettore DOM.

1. Per prima cosa, caricare il [Markup Validation Service](https://validator.w3.org/) in una nuova scheda del browser, se non è già aperto.
2. Passare alla scheda [Validate by Direct Input](https://validator.w3.org/#validate_by_input).
3. Copiare il [documento di esempio](#your_turn_studying_html_using_the_dom_inspector) e incollarlo nella grande area di testo mostrata nel Markup Validation Service. Incollare l'intera struttura del documento, non soltanto il contenuto di `<body>`.
4. Premere il pulsante _Check_.

Dovrebbe essere visualizzato un elenco di errori e altre informazioni.

![Un elenco di risultati della validazione HTML dal servizio di validazione del markup del W3C](validation-results.png)

### Interpretare i messaggi di errore

I messaggi di errore sono solitamente utili, ma talvolta non sono così facili da comprendere. Con un po' di pratica, è possibile capire come interpretarli per correggere il codice. Esaminiamo i messaggi di errore e vediamo cosa significano. Ogni messaggio è accompagnato da un numero di riga e di colonna per aiutare a individuare facilmente l'errore.

- "End tag `li` implied, but there were open elements" (2 occorrenze): questi messaggi indicano che è aperto un elemento che dovrebbe essere chiuso. Il tag di chiusura è implicito, ma non è effettivamente presente. Le informazioni su riga e colonna puntano alla prima riga successiva a quella in cui il tag di chiusura dovrebbe realmente trovarsi, ma questo è un indizio sufficiente per capire cosa non va.
- "Unclosed element `strong`": questo è più facile da comprendere: un elemento {{htmlelement("strong")}} non è chiuso e le informazioni su riga e colonna indicano esattamente dove si trova.
- "End tag `strong` violates nesting rules": questo segnala gli elementi annidati in modo errato e le informazioni su riga e colonna indicano dove si trovano.
- "End of file reached when inside an attribute value. Ignoring tag": questo messaggio è piuttosto criptico; si riferisce al fatto che da qualche parte è presente un valore di attributo non formato correttamente, probabilmente vicino alla fine del file perché la fine del file appare all'interno del valore dell'attributo. Il fatto che il browser non sottoponga a rendering il link dovrebbe fornire un buon indizio sull'elemento responsabile.
- "End of file seen and there were open elements": questo è un po' ambiguo, ma sostanzialmente si riferisce al fatto che ci sono elementi aperti che devono essere chiusi correttamente. I numeri di riga indicano le ultime righe del file e questo messaggio di errore include una riga di codice che mostra un esempio di elemento aperto:

  ```plain
  example: <a href="https://www.mozilla.org/>link to Mozilla homepage</a> ↩ </ul>↩ </body>↩</html>
  ```

  > [!NOTE]
  > Un attributo privo della virgoletta di chiusura può generare un elemento aperto perché il resto del documento viene interpretato come contenuto dell'attributo.

- "Unclosed element `ul`": questo non è molto utile, poiché l'elemento {{htmlelement("ul")}} _è_ chiuso correttamente. L'errore si verifica perché l'elemento {{htmlelement("a")}} non è chiuso, a causa della virgoletta di chiusura mancante.

Non c'è da preoccuparsi se non si riesce a capire il significato di ogni messaggio di errore. Una buona strategia consiste nel correggere pochi errori alla volta, quindi rivalidare l'HTML dopo ogni insieme di correzioni per vedere quali errori rimangono. A volte, correggere un errore precedente eliminerà anche altri messaggi di errore: spesso diversi errori possono essere causati da un singolo problema, con un effetto domino.

Si saprà che tutti gli errori sono stati corretti quando apparirà un bel banner verde che indica che non ci sono errori da segnalare. Al momento della scrittura, riportava: "Document checking completed. No errors or warnings to show."

## Riepilogo

Ecco quindi un'introduzione al debug di HTML, che dovrebbe fornire alcune competenze utili su cui fare affidamento durante il debug di HTML, ma anche di codice CSS e JavaScript più avanti nel corso. Questo segna anche la fine del modulo _Strutturare i contenuti con HTML_.

Il passo successivo è iniziare a imparare lo stile del web nel modulo [Fondamenti dello stile CSS](/it/docs/Learn_web_development/Core/Styling_basics).

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/Forms_challenge", "Learn_web_development/Core/Styling_basics", "Learn_web_development/Core/Structuring_content")}}
