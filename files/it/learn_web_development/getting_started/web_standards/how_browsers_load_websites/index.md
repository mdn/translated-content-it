---
title: Come i browser caricano i siti web
slug: Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites
l10n:
  sourceCommit: 7edaecc85b639bb2b75cdb7722312fa0b0137476
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Soft_skills", "Learn_web_development/Getting_started/Web_standards")}}

Nell'articolo precedente, abbiamo esaminato una [panoramica delle tecnologie](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#overview_of_modern_web_technologies) su cui sono costruiti i siti web. In questo articolo esploriamo il processo tramite il quale queste tecnologie vengono renderizzate — quando un browser ha ricevuto i file di codice e altri asset che compongono una pagina web (come trattato in [Come funziona il web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)), come vengono assemblati per creare l'esperienza finale con cui l'utente interagisce?

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del suo computer, i browser web e le tecnologie web.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>I diversi tipi di asset che vengono restituiti in una risposta HTTP.</li>
          <li>Come i diversi file vengono assemblati dal browser per renderizzare una pagina web che viene poi visualizzata dall'utente.</li>
          <li>Perché a volte il browser è visto come un ambiente di programmazione ostile, ma anche come un ambiente di programmazione straordinario.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quali file sono restituiti nelle risposte HTTP?

Per riassumere la [panoramica delle tecnologie web](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#overview_of_modern_web_technologies) esaminata nell'ultimo articolo, le risposte HTTP (alle richieste di una pagina web) generalmente contengono alcuni dei seguenti tipi di file:

- File HTML, che specificano il contenuto della pagina web e la sua struttura.
- File CSS, che specificano le informazioni di stile e layout.
- File JavaScript, che specificano il comportamento delle parti interattive della pagina web.
- Asset multimediali come immagini, video, file audio, {{Glossary("PDF", "PDF")}} e {{Glossary("SVG", "SVG")}}, che sono incorporati nelle pagine web o altrimenti visualizzati dal browser.
- Altri tipi di file che il browser non può gestire nativamente e quindi passa a un'applicazione pertinente sul dispositivo per la visualizzazione, ad esempio documenti di Word o Pages, presentazioni PowerPoint e file di Open Office.

## Renderizzazione della pagina web

Quando l'utente naviga verso una nuova pagina web (cliccando un link, o inserendo un indirizzo web nella barra degli indirizzi del browser), vengono inviati diversi richieste HTTP, e diversi file vengono restituiti in risposte HTTP. I file ricevuti in queste risposte vengono elaborati dal browser e assemblati in una pagina web con cui l'utente può interagire. Questo processo di assemblaggio dei pezzi in una pagina web è chiamato **renderizzazione**.

Le sezioni seguenti forniscono una spiegazione ad alto livello di come un browser renderizza una pagina web. Tenga presente che questa è una descrizione semplificata, e che diversi browser gestiranno il processo in modi diversi. Tuttavia, questo darà comunque un'idea di come funzionano le cose.

## Gestione dell'HTML

Per iniziare, il file HTML che contiene il contenuto della pagina web e definisce la sua struttura viene ricevuto dal browser e analizzato. Il browser lo converte in una struttura ad albero chiamata **albero DOM** (**Document Object Model**). Il DOM rappresenta la struttura del documento HTML nella memoria del computer. Prenda come esempio questo semplice snippet HTML:

```html
<p>
  Let's use:
  <span>HTML</span>
  <span>CSS</span>
  <span>JavaScript</span>
</p>
```

Ogni elemento, attributo e pezzo di testo nell'HTML diventa un **nodo DOM** nella struttura ad albero. I nodi sono definiti dalla loro relazione con altri nodi DOM. Alcuni elementi sono genitori di nodi figli, e i nodi figli hanno fratelli. Il browser analizzerà questo HTML e creerà il seguente albero DOM da esso:

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

In questo albero DOM, il nodo corrispondente al nostro elemento `<p>` è un genitore. I suoi figli includono un nodo di testo e i tre nodi corrispondenti ai nostri elementi `<span>`. I nodi `SPAN` sono anche genitori, con nodi di testo come loro figli. Quando il browser renderizza questo albero DOM, apparirà così:

{{EmbedLiveSample('Handling the HTML', '100%', 55)}}

```css hidden
p {
  margin: 0;
}
```

Certi elementi HTML, quando analizzati, attiveranno ulteriori richieste HTTP:

- Elementi {{htmlelement("link")}} che fanno riferimento a fogli di stile [CSS](/it/docs/Learn_web_development/Core/Styling_basics) esterni.
- Elementi {{htmlelement("script")}} che fanno riferimento a file [JavaScript](/it/docs/Learn_web_development/Core/Scripting) esterni.
- Elementi come {{htmlelement("img")}}, {{htmlelement("video")}} e {{htmlelement("audio")}}, che fanno riferimento a file multimediali che si desidera incorporare nella pagina web.

## Analisi del CSS e rendering della pagina

Successivamente, vediamo come viene gestito il CSS.

1. Il browser analizza il CSS trovato sulla pagina (incluso nel file HTML o recuperato da fogli di stile esterni), e ordina le diverse regole di stile CSS in diversi "contenitori" in base a quali elementi HTML (rappresentati nel DOM come elementi chiamati **nodi**) verranno applicati. Il browser quindi allega stili a diversi elementi secondo necessità (questo passaggio intermedio è chiamato albero di rendering).
2. L'albero di rendering è disposto nella struttura in cui dovrebbe apparire dopo che le regole sono state applicate. Questo include eventuali immagini e altri file multimediali che devono essere incorporati nella pagina.
3. La visualizzazione della pagina è mostrata sullo schermo (questa fase è chiamata pittura).

Il diagramma seguente offre una visualizzazione del processo di cui abbiamo parlato finora:

![Panoramica del processo di rendering](rendering.svg)

Tornando al nostro esempio, supponiamo che il seguente CSS sia trovato nel file HTML:

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

L'unica regola disponibile nel CSS ha un selettore `span`, quindi il browser può ordinare il CSS molto rapidamente! Applica tale regola a ciascuno dei tre nodi SPAN nell'albero DOM, dando loro un bordo nero e uno sfondo verde lime, quindi dipinge la rappresentazione visiva finale sullo schermo.

L'output aggiornato è il seguente:

{{EmbedLiveSample('Parsing the CSS, and rendering the page', '100%', 90)}}

## Gestione del JavaScript

Qualsiasi JavaScript trovato sulla pagina (incluso nel file HTML o recuperato da file di script esterni) viene analizzato, interpretato, compilato ed eseguito. Questo accade in un momento precedente al completamento del rendering finale della pagina — dopotutto, alcuni JavaScript possono influenzare il rendering, ad esempio aggiungendo nodi al DOM o modificando quelli esistenti.

Ritornando al nostro esempio, supponiamo che il seguente JavaScript sia trovato nel file HTML:

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

Non è necessario comprendere esattamente come funziona questo JavaScript, ma a livello generale, trova ogni nodo SPAN nel DOM e inverte l'ordine dei caratteri nei loro nodi di testo figlio.

Il risultato finale è il seguente:

{{EmbedLiveSample('Handling the JavaScript', '100%', 90)}}

## Quali altri passaggi di rendering ci sono?

Durante il rendering della pagina accadono diverse altre cose, ma non le discuteremo tutte qui. Un'importante ulteriore occorrenza degna di nota è che un albero di accessibilità viene costruito, basato sul DOM, in modo che le tecnologie assistive (ad esempio i lettori di schermo) possano agganciarsi, consentendo alle persone che non sono in grado di vedere il contenuto renderizzato di interagire con esso.

Imparerà di più su questo più avanti, nel nostro modulo [Accessibilità](/it/docs/Learn_web_development/Core/Accessibility).

## Il browser: un ambiente di programmazione ostile _e_ straordinario

Lo sviluppo web front-end può talvolta essere frustrante, e alcune persone considerano il browser un ambiente di programmazione ostile. Questo perché, a differenza di altri ambienti di programmazione, è molto più difficile fare garanzie sull'ambiente in cui il suo codice verrà eseguito. Non può sapere in anticipo tutte le diverse combinazioni di sistema operativo, browser, lingua, posizione, connessione di rete, CPU, GPU, memoria, durata della batteria, ecc., che i suoi utenti avranno, quindi non può garantire un'esperienza utente perfetta per tutti loro.

I browser moderni tendono a implementare gli standard web in modo abbastanza coerente, ma c'è ancora molta incertezza da affrontare. Come sviluppatore web, dovrà abbracciare tale incertezza, programmando in modo difensivo ed essendo prudente con le funzionalità che utilizza. Questo si basa sull'aderenza alle [best practices](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#web_best_practices) descritte nell'articolo precedente.

D'altra parte, il web è anche un ambiente di programmazione straordinario, per molte ragioni.

- Per cominciare, è progettato con in mente l'accesso universale. Lo stato di base del web è accessibile e collegabile. Alcuni di questi aspetti di base sono più difficili da raggiungere in altri ambienti.
- La distribuzione delle applicazioni attraverso il web è semplice e potente. Non è necessario portare i suoi utenti attraverso un processo di installazione complicato: basta indicar loro un indirizzo web e via.
- Gli aggiornamenti delle applicazioni sono generalmente semplici. In molti casi, i visitatori possono vedere nuove versioni di un'applicazione quando ricaricano la scheda del browser. Non è necessario preoccuparsi di far scaricare e installare regolarmente agli utenti gli aggiornamenti software.
- La comunità web è vibrante e utile. Come discuteremo più avanti nel nostro articolo [Ricerca e apprendimento](/it/docs/Learn_web_development/Getting_started/Soft_skills/Research_and_learning), ci sono molti posti dove può chiedere aiuto e ottime risorse da cui imparare.

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Soft_skills", "Learn_web_development/Getting_started/Web_standards")}}
