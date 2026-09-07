---
title: Come i browser caricano i siti web
slug: Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites
l10n:
  sourceCommit: cab1109a0c225299a9fb2b3402bcd4a1931b8ab7
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Soft_skills", "Learn_web_development/Getting_started/Web_standards")}}

Nell'articolo precedente è stata esaminata una [panoramica delle tecnologie](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#overview_of_modern_web_technologies) utilizzate per costruire i siti web. In questo articolo viene illustrato il processo con cui tali tecnologie vengono renderizzate: quando un browser ha ricevuto i file di codice e le altre risorse che costituiscono una pagina web (come trattato in [Come funziona il Web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)), come vengono assemblati per creare l'esperienza finale con cui l'utente interagisce?

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, i browser web e le tecnologie web.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>I diversi tipi di risorse restituiti in una risposta HTTP.</li>
          <li>Come i diversi file vengono assemblati dal browser per renderizzare una pagina web che viene poi visualizzata all'utente.</li>
          <li>Perché il browser viene talvolta considerato un ambiente di programmazione ostile, ma anche un ambiente di programmazione eccellente.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quali file vengono restituiti nelle risposte HTTP?

Per riassumere la [panoramica delle tecnologie web](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#overview_of_modern_web_technologies) esaminata nell'ultimo articolo, le risposte HTTP (alle richieste di una pagina web) conterranno generalmente alcuni dei seguenti tipi di file:

- File HTML, che specificano il contenuto e la struttura della pagina web.
- File CSS, che specificano le informazioni di stile e layout.
- File JavaScript, che specificano il comportamento delle parti interattive della pagina web.
- Risorse multimediali come immagini, video, file audio, {{Glossary("PDF", "PDF")}} e {{Glossary("SVG", "SVG")}}, incorporati nelle pagine web o altrimenti visualizzati dal browser.
- Altri tipi di file che il browser non può gestire nativamente e che quindi passa a un'app pertinente sul dispositivo per il rendering, ad esempio documenti Word o Pages, presentazioni PowerPoint e file Open Office.

## Rendering della pagina web

Quando l'utente passa a una nuova pagina web, facendo clic su un link o inserendo un indirizzo web nella barra degli indirizzi del browser, vengono inviate diverse richieste HTTP e diversi file vengono restituiti nelle risposte HTTP. I file ricevuti in tali risposte vengono elaborati dal browser e assemblati in una pagina web con cui l'utente può interagire. Questo processo di assemblaggio dei vari elementi in una pagina web è chiamato **rendering**.

Le sezioni seguenti forniscono una spiegazione generale di come un browser renderizza una pagina web. Occorre tenere presente che si tratta di una descrizione semplificata e che browser diversi gestiranno il processo in modi diversi. Tuttavia, consente comunque di farsi un'idea di come funziona.

## Gestione dell'HTML

Per iniziare, il browser riceve e analizza il file HTML che contiene il contenuto della pagina web e ne definisce la struttura. Il browser lo converte in una struttura ad albero chiamata **albero DOM** (**Document Object Model**). Il DOM rappresenta la struttura del documento HTML nella memoria del computer. Si consideri come esempio questo frammento HTML di base:

```html
<p>
  Let's use:
  <span>HTML</span>
  <span>CSS</span>
  <span>JavaScript</span>
</p>
```

Ogni elemento, attributo e porzione di testo nell'HTML diventa un **nodo DOM** nella struttura ad albero. I nodi sono definiti dalla loro relazione con altri nodi DOM. Alcuni elementi sono genitori di nodi figli e i nodi figli hanno nodi fratelli. Il browser analizzerà questo HTML e creerà da esso il seguente albero DOM:

```plain
P
├─ "Let's use:"
├─ SPAN
|  └─ "HTML"
├─ SPAN
|  └─ "CSS"
└─ SPAN
    └─ "JavaScript"
```

In questo albero DOM, il nodo corrispondente all'elemento `<p>` è un genitore. I suoi figli includono un nodo di testo e i tre nodi corrispondenti agli elementi `<span>`. Anche i nodi `SPAN` sono genitori, con nodi di testo come figli. Quando il browser renderizza questo albero DOM, il risultato sarà il seguente:

{{EmbedLiveSample('Handling the HTML', '100%', 55)}}

```css hidden
p {
  margin: 0;
}
```

Alcuni elementi HTML, quando vengono analizzati, attivano ulteriori richieste HTTP:

- Elementi {{htmlelement("link")}} che fanno riferimento a fogli di stile [CSS](/it/docs/Learn_web_development/Core/Styling_basics) esterni.
- Elementi {{htmlelement("script")}} che fanno riferimento a file [JavaScript](/it/docs/Learn_web_development/Core/Scripting) esterni.
- Elementi come {{htmlelement("img")}}, {{htmlelement("video")}} e {{htmlelement("audio")}}, che fanno riferimento a file multimediali da incorporare nella pagina web.

## Analisi del CSS e rendering della pagina

Successivamente viene gestito il CSS.

1. Il browser analizza il CSS presente nella pagina, incluso nel file HTML oppure recuperato da fogli di stile esterni, e suddivide le diverse regole di stile CSS in diversi "contenitori" in base agli elementi HTML, rappresentati nel DOM come elementi chiamati **nodi**, ai quali saranno applicate. Il browser associa quindi gli stili ai diversi elementi secondo necessità. Questo passaggio intermedio è chiamato render tree.
2. Il render tree viene disposto nella struttura che dovrebbe avere dopo l'applicazione delle regole. Ciò include eventuali immagini e altri file multimediali da incorporare nella pagina.
3. La visualizzazione della pagina viene mostrata sullo schermo. Questa fase è chiamata painting.

Il diagramma seguente offre una visualizzazione del processo descritto finora:

![Panoramica del processo di rendering](rendering.svg)

Tornando all'esempio, si supponga che nel file HTML sia presente il seguente CSS:

```html hidden
<p>
  Let's use:
  <span>HTML</span>
  <span>CSS</span>
  <span>JavaScript</span>
</p>
```

```css
span {
  border: 1px solid black;
  background-color: lime;
}
```

L'unica regola disponibile nel CSS ha un selettore `span`, quindi il browser riesce a ordinare il CSS molto rapidamente. Applica questa regola a ciascuno dei tre nodi SPAN nell'albero DOM, assegnando loro un bordo nero e uno sfondo verde lime, quindi disegna sullo schermo la rappresentazione visiva finale.

L'output aggiornato è il seguente:

{{EmbedLiveSample('Parsing the CSS, and rendering the page', '100%', 90)}}

## Gestione di JavaScript

Dopo che il CSS è stato gestito, qualsiasi JavaScript presente nella pagina, incluso nel file HTML oppure recuperato da file script esterni, viene analizzato, interpretato, compilato ed eseguito. Ciò avviene a un certo punto prima del completamento del rendering finale della pagina: del resto, parte del JavaScript può influire sul rendering, ad esempio aggiungendo nodi al DOM o modificando quelli esistenti.

Tornando all'esempio, si supponga che nel file HTML sia presente il seguente JavaScript:

```html hidden
<p>
  Let's use:
  <span>HTML</span>
  <span>CSS</span>
  <span>JavaScript</span>
</p>
```

```css hidden
span {
  border: 1px solid black;
  background-color: lime;
}
```

```js
const spans = document.querySelectorAll("span");
spans.forEach((span) => {
  const reversedText = span.textContent.split("").reverse().join("");
  span.textContent = reversedText;
});
```

Non è necessario comprendere esattamente come funziona questo JavaScript, ma a grandi linee individua ogni nodo SPAN nel DOM e inverte l'ordine dei caratteri nei relativi nodi di testo figli.

L'output finale è il seguente:

{{EmbedLiveSample('Handling the JavaScript', '100%', 90)}}

## Quali altri passaggi di rendering esistono?

Durante il rendering della pagina avvengono molte altre operazioni, ma non verranno trattate tutte qui. Un evento aggiuntivo importante da menzionare è la creazione di un albero di accessibilità, basato sul DOM, al quale le tecnologie assistive, ad esempio i lettori di schermo, possono collegarsi. Ciò consente alle persone che non possono vedere il contenuto renderizzato di interagire con esso.

Questo argomento verrà approfondito in seguito, nel modulo [Accessibilità](/it/docs/Learn_web_development/Core/Accessibility).

## Il browser: un ambiente di programmazione ostile _e_ straordinario

Lo sviluppo web front-end può talvolta risultare frustrante e alcune persone considerano il browser un ambiente di programmazione ostile. Questo perché, a differenza di altri ambienti di programmazione, è molto più difficile fornire garanzie sull'ambiente in cui verrà eseguito il codice. Non è possibile conoscere in anticipo tutte le diverse combinazioni di sistema operativo, browser, lingua, località, connessione di rete, CPU, GPU, memoria, durata della batteria e così via che gli utenti avranno; pertanto, non è possibile garantire un'esperienza utente perfetta per tutti.

I browser moderni tendono a implementare gli standard web in modo abbastanza coerente, ma rimane comunque molta incertezza da affrontare. Come sviluppatore web, sarà necessario accettare tale incertezza, programmando in modo difensivo e adottando un approccio conservativo rispetto alle funzionalità utilizzate. Ciò richiede l'adesione alle [buone pratiche](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#web_best_practices) delineate nell'articolo precedente.

Il Web, tuttavia, è anche un ambiente di programmazione straordinario, per molte ragioni.

- Innanzitutto, è progettato tenendo presente l'accesso universale. Lo stato di base del Web è accessibile e collegabile tramite link. Alcuni di questi aspetti fondamentali sono più difficili da ottenere in altri ambienti.
- La distribuzione di app tramite il Web è semplice e potente. Non è necessario guidare gli utenti attraverso un processo di installazione complicato: basta indirizzarli a un indirizzo web e l'app è pronta all'uso.
- Gli aggiornamenti delle app sono generalmente semplici. In molti casi, i visitatori possono vedere le nuove versioni di un'applicazione quando ricaricano la scheda del browser. Non è necessario preoccuparsi di far scaricare e installare regolarmente gli aggiornamenti software ai visitatori.
- La comunità del Web è vivace e disponibile. Come viene illustrato in seguito nell'articolo [Ricerca e apprendimento](/it/docs/Learn_web_development/Getting_started/Soft_skills/Research_and_learning), esistono molti luoghi in cui chiedere aiuto e ottime risorse da cui imparare.

## Vedi anche

- [Quando e come segnalare bug nei browser](/it/docs/Learn_web_development/Howto/Web_mechanics/File_browser_bugs)
  - : Se qualcosa non funziona come previsto in un browser, potrebbe trattarsi di un bug del browser. Questo articolo spiega come capire se è così e, in tal caso, come inviare una segnalazione di bug.

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Soft_skills", "Learn_web_development/Getting_started/Web_standards")}}
