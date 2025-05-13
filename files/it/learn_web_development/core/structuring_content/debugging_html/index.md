---
title: Debugging HTML
slug: Learn_web_development/Core/Structuring_content/Debugging_HTML
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Styling_basics", "Learn_web_development/Core/Structuring_content")}}

Scrivere codice HTML va bene, ma cosa succede se qualcosa va storto e non riesci a capire dove si trova l'errore nel codice? Questo articolo ti presenterà alcuni strumenti che possono aiutarti a trovare e correggere errori in HTML.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi di base HTML</a
        >. Semantica a livello di testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >titoli e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >. <a href="/it/docs/Learn_web_development/Core/Structuring_content/Structuring_documents"
          >HTML strutturale</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Il background chiave sulla debugging HTML</li>
          <li>Utilizzare l'inspector DOM nei DevTools del browser per approfondire il codice HTML.</li>
          <li>Esplorare i tipi comuni di errori HTML.</li>
          <li>Usare il <a href="https://validator.w3.org/">validatore HTML</a> per rilevare errori HTML.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Il debugging non è spaventoso

Quando si scrive del codice di qualche tipo, va tutto bene fino a quel momento temuto in cui si verifica un errore — si è commesso un errore, quindi il codice non funziona — o non funziona affatto, o non proprio come si desiderava. Ad esempio, il seguente esempio mostra un errore riportato quando si tenta di {{Glossary("compile", "compilare")}} un semplice programma scritto nel linguaggio [Rust](https://www.rust-lang.org/).

![Una finestra della console che mostra il risultato del tentativo di compilare un programma Rust con una mancanza di virgolette attorno a una stringa in un'istruzione di stampa. Il messaggio di errore riportato è: errore: stringa di doppie virgolette non terminata.](error-message.png)

Qui, il messaggio di errore è relativamente facile da capire — "unterminated double quote string". Se si guarda l'elenco, si può probabilmente vedere come `println!(Hello, world!");` possa logicamente mancare di una doppia virgolette. Tuttavia, i messaggi di errore possono rapidamente diventare più complicati e meno facili da interpretare man mano che i programmi crescono, e anche i casi semplici possono sembrare un po' intimidatori a qualcuno che non sa nulla di Rust.

Il debugging però non deve essere spaventoso — la chiave per sentirsi a proprio agio con la scrittura e il debugging di qualsiasi codice è la familiarità con entrambi il linguaggio e gli strumenti associati.

## HTML e il debugging

HTML non è così complicato da capire come Rust. HTML non è compilato in una forma diversa prima del parsing (è _interpretato_, non _compilato_). E la sintassi degli {{Glossary("element", "elementi")}} HTML è probabilmente molto più facile da comprendere rispetto a un "vero linguaggio di programmazione" come Rust, {{Glossary("JavaScript", "JavaScript")}} o {{Glossary("Python", "Python")}}.

Il modo in cui i browser fanno il parsing di HTML è molto più **permissivo** rispetto a come vengono analizzati la maggior parte dei linguaggi di programmazione, il che è sia un vantaggio che uno svantaggio.

Ma prima di tutto, cosa intendiamo con permissivo? Ebbene, generalmente quando fai qualcosa di sbagliato nel codice, ci sono due principali tipi di errori che si possono incontrare:

- **Errori di sintassi**: Questi sono errori di battitura nel codice che impediscono al programma di funzionare, come l'errore di Rust mostrato in precedenza. Di solito sono facili da correggere a condizione che si abbia familiarità con la sintassi del linguaggio e si sappia cosa significano i messaggi di errore.
- **Errori logici**: Questi sono errori in cui la sintassi è corretta, ma il codice non fa quello che si intendeva, il che significa che il programma funziona in modo non corretto. Sono spesso più difficili da correggere rispetto agli errori di sintassi, poiché non c'è un messaggio di errore che indichi la fonte dell'errore.

HTML in sé non soffre di errori di sintassi perché i browser lo interpretano in modo permissivo, il che significa che la pagina viene comunque visualizzata anche se ci sono errori di sintassi nel codice sorgente. I browser hanno regole integrate su come interpretare il markup HTML scritto in modo errato (spesso chiamato **markup non valido** o **malformato**), cambiandolo automaticamente in un markup valido.

Ad esempio, il seguente frammento di codice HTML contiene elementi annidati in modo errato:

```html example-bad
<p>I didn't expect to find the <em>next-door neighbor's <strong>cat</em></strong> here!</p>
```

Il tag di chiusura `</strong>` dovrebbe trovarsi prima del tag di chiusura `</em>`, ma non è così — è dopo.

Se carichi questo HTML in un browser e poi guardi il [DOM renderizzato](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites#handling_html), vedrai che l'annidamento è stato corretto dal browser:

```html example-good
<p>
  I didn't expect to find the
  <em>next-door neighbor's <strong>cat</strong></em> here!
</p>
```

Quindi perché questo è sia un bene che un male? Bene, in questo caso il browser ha creato il risultato previsto, ma come vedrai [più avanti](#active_learning_studying_html_using_the_dom_inspector), non è sempre così. Otterrai sempre _qualcosa_ in esecuzione, ma il browser non sempre lo interpreta correttamente, il che può causare problemi. È meglio scrivere correttamente il markup fin dall'inizio.

> [!NOTE]
> L'HTML è analizzato in modo permissivo perché quando il web è stato creato inizialmente, è stato deciso che pubblicare contenuti fosse più importante che garantire che la sintassi fosse assolutamente corretta. Il web probabilmente non sarebbe così popolare come lo è oggi se fosse stato più rigido fin dall'inizio.

Allora come si trova un errore di markup? Più avanti ti mostreremo come trovare errori in HTML usando uno strumento chiamato [validatore HTML](#validazione_html), ma prima ti mostreremo come ispezionare manualmente il tuo HTML utilizzando un **inspector DOM**, ed esplorare quali tipi di errori di markup potresti cercare e come il browser potrebbe interpretarli.

## Usare un inspector DOM

Tutti i browser moderni hanno una serie di [strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) (devtools) integrati, che forniscono una serie di funzionalità per esaminare la pagina web caricata nella scheda corrente. Questi possono mostrare quale HTML è renderizzato nella pagina, quale CSS è applicato a ciascun nodo DOM, quale JavaScript è in esecuzione nella pagina e altro. Consentono inoltre di modificare il codice in esecuzione e vedere l'effetto direttamente sulla pagina.

È possibile aprire i devtools in modo simile in ogni browser — vedere [Come aprire i devtools nel tuo browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#how_to_open_the_devtools_in_your_browser) per imparare come fare.

Per questo articolo, l'unica funzione dei devtools rilevante è l'**inspector DOM**, che mostra il DOM HTML renderizzato attualmente e ti consente di modificarlo. Diamo un'occhiata ora:

1. Apri i devtools nel tuo browser.
2. Apri l'inspector DOM. È nella stessa posizione in ogni browser — la prima scheda nei devtools all'inizio della riga. In Firefox è etichettato come _Inspector_, mentre in Safari, Edge e Chrome è etichettato come _Elements_. Questa dovrebbe essere la scheda selezionata per impostazione predefinita quando apri per la prima volta i devtools, ma selezionala se non lo è.
3. Esamina la struttura dell'albero del DOM mostrata nella scheda, e nota come puoi fare clic sulle piccole frecce di espansione all'inizio di ciascun nodo DOM per espanderli e contrarli e rivelare i loro nodi discendenti. Puoi anche utilizzare i tasti cursore su e giù per spostarti su e giù per i nodi, e i tasti cursore a destra e a sinistra per espandere e contrarre i nodi.
4. Prova inoltre a passare con il mouse sopra i nodi (o selezionarli con i tasti cursore) e nota come l'elemento attualmente selezionato (o evidenziato) è evidenziato nel viewport.
5. Puoi anche modificare il DOM renderizzato. Non utilizzeremo la funzionalità di modifica in questo articolo, ma prenditi del tempo per capire come fare questo se sei curioso.

## Apprendimento attivo: Studiare HTML utilizzando l'inspector DOM

È tempo di studiare del codice HTML utilizzando l'inspector DOM, e vedere come il browser gestisce gli errori comuni di markup.

1. Prima di tutto, salva il seguente elenco di file HTML come `debug-example.html`, da qualche parte sul tuo computer locale. Questo demo è scritto deliberatamente con alcuni errori incorporati per esplorarli.

   ```html-nolint
   <!DOCTYPE html>
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

2. Successivamente, aprilo in un browser. Vedrai qualcosa di simile a questo:![Un semplice documento HTML con un titolo di esempi di debugging HTML, e alcune informazioni sugli errori HTML comuni, come elementi non chiusi, elementi annidati male e attributi non chiusi. ](badly-formed-html.png)
3. Questo non sembra immediatamente così bello; guardiamo al codice sorgente per vedere se possiamo capire il perché (sono mostrati solo i contenuti del body):

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

   - Gli elementi {{htmlelement("p","paragraph")}} e {{htmlelement("li","list item")}} non hanno tag di chiusura. Guardando l'immagine sopra, questo non sembra aver influenzato troppo il rendering del markup, poiché è facile dedurre dove dovrebbe finire un elemento e iniziarne un altro.
   - Il primo elemento {{htmlelement("strong")}} non ha un tag di chiusura. Questo è un po' più problematico, poiché non è facile capire dove l'elemento dovrebbe terminare. Infatti, tutto il resto del testo è stato renderizzato in grassetto.
   - Questa sezione è annidata male: `<strong>strong <em>strong emphasized?</strong> what is this?</em>`. Non è facile capire come sia stata interpretata a causa del problema precedente.
   - Il valore dell'attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) manca di una virgolette doppia di chiusura. Questo sembra aver causato il problema più grande — il link non è stato renderizzato affatto.

5. Ora esaminiamo il DOM renderizzato, al contrario del codice sorgente. Per fare questo, apri l'inspector DOM del tuo browser. Vedrai una rappresentazione del markup renderizzato:![L'inspector HTML in Firefox, con evidenziato il paragrafo del nostro esempio, che mostra il testo "What causes errors in HTML?" Qui puoi vedere che l'elemento paragrafo è stato chiuso dal browser.](html-inspector.png)
6. Guarda come il browser ha cercato di correggere i nostri errori HTML (abbiamo condotto la revisione in Firefox; altri browser moderni _dovrebbero_ dare lo stesso risultato):

   - Ai paragrafi e agli elementi lista sono stati dati i tag di chiusura.
   - Non è chiaro dove dovrebbe essere chiuso il primo `<strong>`, quindi il browser ha avvolto ciascun blocco separato di testo nel proprio elemento `<strong>`, fino al fondo del documento!
   - Il nesting errato è stato corretto dal browser come mostrato qui:

     ```html
     <strong>
       strong
       <em>strong emphasized?</em>
     </strong>
     <em> what is this?</em>
     ```

   - Il link con la virgolette mancante è stato eliminato del tutto. L'ultimo elemento lista si presenta così:

     ```html
     <li>
       <strong>
         Unclosed attributes: Another common source of HTML problems. Let's look
         at an example:
       </strong>
     </li>
     ```

## Validazione HTML

Si può vedere dall'esempio sopra che è davvero importante assicurarsi che il proprio HTML sia ben formato! Ma come fare? In un piccolo esempio come quello visto sopra, è facile cercare tra le righe e trovare gli errori, ma che dire di un enorme e complesso documento HTML?

Lo strumento per questo lavoro è il [Markup Validation Service](https://validator.w3.org/) (o **validatore HTML**), che è creato e mantenuto dal W3C (che hai conosciuto nel [Modello degli standard web](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model)). Il validatore prende un documento HTML come input, lo attraversa, e ti dà un report per dirti cosa c'è di sbagliato nel tuo HTML.

![La homepage del validatore HTML](validator.png)

Per specificare l'HTML da validare, puoi fornire un indirizzo web, caricare un file HTML, o inserire direttamente del codice HTML.

## Apprendimento attivo: Validare un documento HTML

Proviamo con il nostro [documento di esempio](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/debugging-html/debug-example.html).

1. Prima di tutto, carica il [Markup Validation Service](https://validator.w3.org/) in una nuova scheda del browser, se non è già aperto.
2. Passa alla scheda [Validate by Direct Input](https://validator.w3.org/#validate_by_input).
3. Copia tutto il codice del documento di esempio (non solo il body) e incollalo nella grande area di testo mostrata nel Markup Validation Service.
4. Premi il pulsante _Check_.

Questo dovrebbe fornire un elenco di errori e altre informazioni.

![Un elenco di risultati della validazione HTML dal servizio di validazione del markup W3C](validation-results.png)

### Interpretare i messaggi di errore

I messaggi di errore sono di solito utili, ma a volte non sono così facili da capire. Con un po' di pratica, puoi imparare a interpretarli per correggere il tuo codice. Passiamo attraverso i messaggi di errore e vediamo cosa significano. Noterai che ogni messaggio viene fornito con un numero di riga e colonna per aiutarti a localizzare facilmente l'errore.

- "End tag `li` implied, but there were open elements" (2 istanze): Questi messaggi indicano che un elemento è aperto e dovrebbe essere chiuso. Il tag di chiusura è implicito, ma non effettivamente presente. L'informazione su riga/colonna punta alla prima linea dopo la linea dove dovrebbe effettivamente trovarsi il tag di chiusura, ma questo è un buon indizio per vedere cosa non va.
- "Unclosed element `strong`": Questo è davvero facile da capire — un elemento {{htmlelement("strong")}} non è chiuso, e l'informazione su riga/colonna punta esattamente a dove si trova.
- "End tag `strong` violates nesting rules": Questo evidenzia gli elementi annidati in modo errato, e l'informazione su riga/colonna indica dove si trovano.
- "End of file reached when inside an attribute value. Ignoring tag": Questo è piuttosto criptico; si riferisce al fatto che c'è un valore di attributo non formato correttamente da qualche parte, possibilmente vicino alla fine del file perché la fine del file appare all'interno del valore dell'attributo. Il fatto che il browser non renderizzi il link dovrebbe darci un buon indizio su quale elemento è colpevole.
- "End of file seen and there were open elements": Questo è un po' ambiguo, ma si riferisce al fatto che ci sono elementi aperti che devono essere chiusi correttamente. I numeri di riga puntano alle ultime righe del file, e questo messaggio di errore viene con una linea di codice che indica un esempio di elemento aperto:

  ```plain
  example: <a href="https://www.mozilla.org/>link to Mozilla homepage</a> ↩ </ul>↩ </body>↩</html>
  ```

  > [!NOTE]
  > Un attributo che manca di una virgolette di chiusura può portare a un elemento aperto perché il resto del documento viene interpretato come contenuto dell'attributo.

- "Unclosed element `ul`": Questo non è molto utile, dato che l'elemento {{htmlelement("ul")}} _è_ chiuso correttamente. Questo errore si verifica perché l'elemento {{htmlelement("a")}} non è chiuso, a causa del carattere di chiusura mancante delle virgolette.

Se non riesci a capire cosa significa ogni messaggio di errore, non preoccuparti. Una buona strategia è correggere alcuni errori alla volta, quindi validare nuovamente il tuo HTML dopo ogni serie di correzioni per vedere quali errori rimangono. A volte, correggere un errore precedente eliminerà anche altri messaggi di errore — diversi errori possono spesso essere causati da un solo problema, in un effetto domino.

Saprai quando tutti i tuoi errori sono corretti quando vedrai un bel banner verde che ti dice che non ci sono errori da segnalare. Al momento della scrittura, diceva "Controllo del documento completato. Nessun errore o avviso da mostrare."

## Riassunto

Ecco quindi un'introduzione al debugging HTML, che dovrebbe darti alcune abilità utili su cui contare quando fai il debugging di HTML, ma anche di codice CSS e JavaScript più avanti nel corso. Questo segna anche la fine del modulo _Strutturare i contenuti con HTML_.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Styling_basics", "Learn_web_development/Core/Structuring_content")}}
