---
title: Correzione del Debugging HTML
slug: Learn_web_development/Core/Structuring_content/Debugging_HTML
l10n:
  sourceCommit: 0c25999e30c50a6e30d66d571b7b4125178b7467
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Styling_basics", "Learn_web_development/Core/Structuring_content")}}

Scrivere HTML è semplice, ma cosa succede se qualcosa va storto e non riesci a individuare l'errore nel codice? Questo articolo ti introdurrà ad alcuni strumenti che possono aiutarti a trovare e correggere errori in HTML.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con HTML, come trattato in
        <a href="/it/docs/Learn_web_development/Core/Structuring_content/Basic_HTML_syntax"
          >Sintassi HTML di base</a
        >. Semantica di livello testo come <a href="/it/docs/Learn_web_development/Core/Structuring_content/Headings_and_paragraphs"
          >intestazioni e paragrafi</a
        > e <a href="/it/docs/Learn_web_development/Core/Structuring_content/Lists"
          >elenchi</a
        >. <a href="/it/docs/Learn_web_development/Core/Structuring_content/Structuring_documents"
          >HTML strutturale</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Conoscenze di base sul debugging HTML</li>
          <li>Utilizzo dell'ispettore DOM nei DevTools del tuo browser per approfondire il tuo codice HTML.</li>
          <li>Esplorazione dei tipi comuni di errori HTML.</li>
          <li>Utilizzo del <a href="https://validator.w3.org/">validator HTML</a> per rilevare errori HTML.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Il debugging non fa paura

Quando si scrive del codice, va tutto bene fino al temuto momento in cui si verifica un errore — hai commesso un errore, quindi il tuo codice non funziona — o non funziona affatto, o non esattamente come desideravi. Ad esempio, di seguito si mostra un errore segnalato quando si tenta di {{Glossary("compile", "compilare")}} un semplice programma scritto nel linguaggio [Rust](https://www.rust-lang.org/).

![Una finestra della console che mostra il risultato del tentativo di compilare un programma rust con una virgoletta mancante attorno a una stringa in una stampa. Il messaggio di errore riportato è error: unterminated double quote string.](error-message.png)

Qui, il messaggio di errore è relativamente facile da capire — "unterminated double quote string". Se si guarda al codice, si può probabilmente vedere come `println!(Hello, world!");` possa logicamente avere una doppia virgoletta mancante. Tuttavia, i messaggi di errore possono rapidamente diventare più complessi e meno facili da interpretare man mano che i programmi crescono, e anche casi semplici possono sembrare un po' intimidatori per qualcuno che non conosce Rust.

Il debugging non deve essere spaventoso comunque — la chiave per sentirsi a proprio agio con la scrittura e il debugging di qualsiasi codice è la familiarità con il linguaggio e gli strumenti associati.

## HTML e debugging

HTML non è così complicato da capire come Rust. HTML non viene compilato in una forma diversa prima di essere analizzato (è _interpretato_, non _compilato_). E la sintassi degli {{Glossary("element", "elementi")}} HTML è probabilmente molto più facile da comprendere rispetto a quella di un "vero linguaggio di programmazione" come Rust, {{Glossary("JavaScript", "JavaScript")}} o {{Glossary("Python", "Python")}}.

Il modo in cui i browser analizzano l'HTML è molto più **permissivo** di come sono analizzati la maggior parte dei linguaggi di programmazione, il che è sia un vantaggio che uno svantaggio.

Ma prima di tutto, cosa intendiamo per permissivo? Bene, in genere quando fai qualcosa di sbagliato in un codice, ti imbatterai in due principali tipi di errore:

- **Errori di sintassi**: Questi sono errori di battitura nel tuo codice che impediscono l'esecuzione del programma, come l'errore di Rust mostrato in precedenza. Questi sono solitamente facili da correggere finché si ha familiarità con la sintassi del linguaggio e si conosce il significato dei messaggi di errore.
- **Errori logici**: Questi sono errori in cui la sintassi è effettivamente corretta, ma il codice non sta facendo quello che intendevi, il che significa che il programma funziona in modo errato. Questi sono spesso più difficili da risolvere rispetto agli errori di sintassi, in quanto non esiste un messaggio di errore che ti indirizzi alla fonte dell'errore.

L'HTML stesso non soffre di errori di sintassi perché i browser lo analizzano in modo permissivo, il che significa che la pagina viene ancora visualizzata anche se ci sono errori di sintassi nel codice sorgente. I browser hanno regole integrate su come interpretare markup HTML scritto in modo errato (spesso definito markup **non valido** o **malformato**), correggendolo automaticamente in un markup valido.

Ad esempio, il seguente frammento HTML contiene elementi nidificati in modo errato:

```html example-bad
<p>I didn't expect to find the <em>next-door neighbor's <strong>cat</em></strong> here!</p>
```

Il tag chiusura `</strong>` dovrebbe essere prima del tag chiusura `</em>`, ma non lo è — si trova dopo.

Se carichi questo HTML in un browser e osservi il [DOM renderizzato](/it/docs/Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites#handling_html), vedrai che la nidificazione è stata corretta dal browser:

```html example-good
<p>
  I didn't expect to find the
  <em>next-door neighbor's <strong>cat</strong></em> here!
</p>
```

Quindi perché questo è sia buono sia cattivo? Bene, in questo caso il browser ha creato il risultato desiderato, ma come vedrai [più avanti](#active_learning_studying_html_using_the_dom_inspector), questo non è sempre il caso. Otterrai sempre _qualcosa_ che funziona, ma il browser non sempre fa tutto bene, il che può causare problemi. È meglio scrivere il markup corretto fin dall'inizio.

> [!NOTE]
> L'HTML viene analizzato in modo permissivo perché quando il web è stato creato, è stato deciso che pubblicare contenuti era più importante che assicurarsi che la sintassi fosse assolutamente corretta. Probabilmente il web non sarebbe così popolare come è oggi se fosse stato più rigoroso fin dall'inizio.

Allora come trovi gli errori nel markup? Più avanti ti mostreremo come trovare errori in HTML usando uno strumento chiamato [validator HTML](#validazione_html), ma prima ti mostreremo come ispezionare manualmente il tuo HTML usando un **ispettore DOM**, e poi esplorare quali tipi di errori del markup potresti cercare e come il browser potrebbe interpretarli.

## Utilizzo di un ispettore DOM

Tutti i browser moderni hanno un set di [strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) (devtools) integrati, che forniscono un insieme di funzionalità per esaminare la pagina web caricata nella scheda corrente. Questi strumenti possono mostrarti l'HTML renderizzato nella pagina, i CSS applicati a ciascun nodo DOM, gli JavaScript in esecuzione nella pagina e altro ancora. Ti consentono anche di modificare il codice attualmente in esecuzione e vedere l'effetto direttamente sulla pagina.

Puoi aprire i devtools in un modo simile in ogni browser — vedi [Come aprire i devtools nel tuo browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools#how_to_open_the_devtools_in_your_browser) per imparare come.

Per questo articolo, l'unica funzione dei devtools rilevante è l'**ispettore DOM**, che mostra l'HTML DOM renderizzato attualmente e consente di modificarlo. Esaminiamo questo ora:

1. Apri i devtools nel tuo browser.
2. Apri l'ispettore DOM. È nello stesso posto in ogni browser — la prima scheda nei devtools all'inizio della riga. In Firefox è etichettato _Inspector_, mentre in Safari, Edge e Chrome è etichettato _Elements_. Questo dovrebbe essere la scheda selezionata per impostazione predefinita quando apri i devtools per la prima volta, ma selezionala se non lo è.
3. Esamina la struttura dell'albero DOM mostrata nella scheda e nota come puoi fare clic sulle piccole frecce di espansione all'inizio di ciascun nodo DOM per espanderle e comprimerle e rivelare i loro nodi discendenti. Puoi anche utilizzare i tasti del cursore su e giù per spostarti su e giù tra i nodi, e i tasti del cursore a destra e sinistra per espandere e comprimere i nodi.
4. Prova anche a passare il mouse sopra i nodi (o selezionarli con i tasti del cursore) e nota come l'elemento attualmente evidenziato (o selezionato) è evidenziato nella viewport.
5. Puoi anche modificare il DOM renderizzato. Non utilizzeremo la funzionalità di modifica in questo articolo, ma prenditi del tempo per cercare come farlo se sei curioso.

## Apprendimento attivo: Studiare HTML usando l'ispettore DOM

È il momento di studiare del codice HTML utilizzando l'ispettore DOM e vedere come il browser gestisce errori comuni di markup.

1. Innanzitutto, salva l'elenco dei file HTML seguente come `debug-example.html`, da qualche parte sul tuo computer locale. Questo esempio è scritto deliberatamente con alcuni errori incorporati da esplorare.

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

2. Successivamente, aprilo in un browser. Vedrai qualcosa di simile a questo:![Un semplice documento HTML con un titolo di Esempi di debugging HTML e alcune informazioni sugli errori comuni HTML, come elementi non chiusi, elementi nidificati male e attributi non chiusi.](badly-formed-html.png)
3. Questo non sembra subito buono; esaminiamo il codice sorgente per vedere se riusciamo a capire perché (solo il contenuto del corpo è mostrato):

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

4. Esaminiamo i problemi:

   - Gli elementi {{htmlelement("p","paragrafo")}} e {{htmlelement("li","elemento lista")}} non hanno tag di chiusura. Guardando l'immagine sopra, questo non sembra aver influenzato troppo la visualizzazione del markup, poiché è facile dedurre dove un elemento dovrebbe terminare e un altro iniziare.
   - Il primo elemento {{htmlelement("strong")}} non ha un tag di chiusura. Questo è un po' più problematico, poiché non è facile capire dove l'elemento dovrebbe terminare. Infatti, tutto il resto del testo è stato reso in grassetto.
   - Questa sezione è nidificata male: `<strong>grassetto <em>grassetto enfatizzato?</strong> cos'è questo?</em>`. Non è facile dire come questo sia stato interpretato a causa del problema precedente.
   - Il valore dell'attributo [`href`](/it/docs/Web/HTML/Reference/Elements/a#href) manca di una doppia virgoletta di chiusura. Questo sembra aver causato il problema più grande — il link non è stato visualizzato affatto.

5. Ora esaminiamo il DOM renderizzato, rispetto al codice sorgente. Per farlo, apri l'ispettore DOM del tuo browser. Vedrai una rappresentazione del markup renderizzato: ![L'ispettore HTML in Firefox, con l'elemento paragrafo del nostro esempio evidenziato, che mostra il testo "Cosa causa errori in HTML?" Qui puoi vedere che l'elemento paragrafo è stato chiuso dal browser.](html-inspector.png)
6. Guarda come il browser ha cercato di correggere i nostri errori HTML (abbiamo fatto la revisione in Firefox; altri browser moderni _dovrebbero_ dare lo stesso risultato):

   - Ai paragrafi e agli elementi di lista sono stati dati tag di chiusura.
   - Non è chiaro dove il primo elemento `<strong>` dovrebbe essere chiuso, quindi il browser ha avvolto ogni blocco di testo separato nel proprio elemento `<strong>`, scendendo fino al fondo del documento!
   - La nidificazione errata è stata corretta dal browser come mostrato qui:

     ```html
     <strong>
       strong
       <em>strong emphasized?</em>
     </strong>
     <em> what is this?</em>
     ```

   - Il link con la virgoletta mancante è stato eliminato del tutto. L'ultimo elemento di lista appare così:

     ```html
     <li>
       <strong>
         Unclosed attributes: Another common source of HTML problems. Let's look
         at an example:
       </strong>
     </li>
     ```

## Validazione HTML

Puoi vedere dall'esempio sopra che davvero vuoi assicurarti che il tuo HTML sia ben formato! Ma come? In un esempio di piccole dimensioni come quello visto sopra, è facile cercare le righe e trovare gli errori, ma che dire di un documento HTML grande e complesso?

Lo strumento per questo lavoro è il [Markup Validation Service](https://validator.w3.org/) (o **validator HTML**), che è creato e mantenuto dal W3C (di cui hai imparato nel [Il modello degli standard del web](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model)). Il validator prende un documento HTML come input, lo esamina e ti fornisce un report per dirti cosa non va nel tuo HTML.

![Homepage del validator HTML](validator.png)

Per specificare l'HTML da validare, puoi fornire un indirizzo web, caricare un file HTML o inserire direttamente del codice HTML.

## Apprendimento attivo: Validare un documento HTML

Proviamo questo con il nostro [documento di esempio](https://github.com/mdn/learning-area/blob/main/html/introduction-to-html/debugging-html/debug-example.html).

1. Prima, carica il [Markup Validation Service](https://validator.w3.org/) in una nuova scheda del browser, se non è già aperto.
2. Passa alla scheda [Validate by Direct Input](https://validator.w3.org/#validate_by_input).
3. Copia tutto il codice del documento di esempio (non solo il corpo) e incollalo nell'area di testo grande mostrata nel Markup Validation Service.
4. Premi il pulsante _Check_.

Questo dovrebbe darti un elenco di errori e altre informazioni.

![Un elenco di risultati di validazione HTML dal servizio di validazione del markup W3C](validation-results.png)

### Interpretare i messaggi di errore

I messaggi di errore sono generalmente utili, ma a volte non sono così facili da capire. Con un po' di pratica, puoi imparare a interpretarli per correggere il tuo codice. Vediamo i messaggi di errore e cosa significano. Noterai che ogni messaggio è accompagnato da un numero di riga e colonna per aiutarti a localizzare facilmente l'errore.

- "End tag `li` implied, but there were open elements" (2 istanze): Questi messaggi indicano che un elemento è aperto e dovrebbe essere chiuso. Il tag finale è implicato, ma non effettivamente presente. Le informazioni di riga/colonna indicano la prima riga dopo la riga in cui il tag di chiusura dovrebbe realmente essere, ma questa è una buona pista per vedere cosa è sbagliato.
- "Unclosed element `strong`": Questo è davvero semplice da capire — un elemento {{htmlelement("strong")}} non è chiuso, e le informazioni di riga/colonna puntano esattamente a dove è.
- "End tag `strong` violates nesting rules": Questo segnala gli elementi nidificati in modo errato, e le informazioni di riga/colonna indicano dove sono.
- "End of file reached when inside an attribute value. Ignoring tag": Questo è piuttosto criptico; si riferisce al fatto che manca la corretta formattazione di un valore di attributo da qualche parte, possibilmente vicino alla fine del file poiché la fine del file appare all'interno del valore dell'attributo. Il fatto che il browser non rende il link dovrebbe dare un buon indizio su quale elemento è responsabile.
- "End of file seen and there were open elements": Questo è un po' ambiguo, ma si riferisce essenzialmente al fatto che ci sono elementi aperti che devono essere chiusi correttamente. I numeri di riga puntano alle ultime righe del file, e questo messaggio di errore è accompagnato da una riga di codice che indica un esempio di elemento aperto:

  ```plain
  example: <a href="https://www.mozilla.org/>link to Mozilla homepage</a> ↩ </ul>↩ </body>↩</html>
  ```

  > [!NOTE]
  > Un attributo mancante di una virgoletta di chiusura può risultare in un elemento aperto poiché il resto del documento è interpretato come contenuto dell'attributo.

- "Unclosed element `ul`": Questo non è molto utile, poiché l'elemento {{htmlelement("ul")}} è chiuso correttamente. Questo errore si verifica perché l'elemento {{htmlelement("a")}} non è chiuso, a causa della virgoletta di chiusura mancante.

Se non riesci a capire cosa significhi ogni messaggio di errore, non preoccuparti. Una buona strategia è correggere alcuni errori alla volta, quindi riverificare il tuo HTML dopo ogni gruppo di correzioni per vedere quali errori rimangono. A volte, correggere un errore precedente eliminerà anche altri messaggi di errore — diversi errori possono spesso essere causati da un singolo problema, in un effetto domino.

Saprai che tutti i tuoi errori sono corretti quando vedrai un bellissimo banner verde che ti dirà che non ci sono errori da segnalare. Al momento della scrittura, diceva "Document checking completed. No errors or warnings to show."

## Riepilogo

Ecco quindi, un'introduzione al debugging HTML, che dovrebbe fornire alcune abilità utili su cui contare quando si esegue il debugging di HTML, ma anche di codice CSS e JavaScript più avanti nel corso. Questo segna anche la fine del modulo _Strutturare i contenuti con HTML_.

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_forms", "Learn_web_development/Core/Styling_basics", "Learn_web_development/Core/Structuring_content")}}
