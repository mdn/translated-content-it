---
title: Come i browser caricano i siti web
slug: Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites
l10n:
  sourceCommit: 7edaecc85b639bb2b75cdb7722312fa0b0137476
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Soft_skills", "Learn_web_development/Getting_started/Web_standards")}}

Nell'articolo precedente, abbiamo esaminato una [panoramica delle tecnologie](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#overview_of_modern_web_technologies) di cui sono composti i siti web. In questo articolo analizziamo il processo attraverso il quale queste tecnologie vengono renderizzate — quando un browser ha ricevuto i file di codice e altri asset che compongono una pagina web (come trattato in [Come funziona il web](/it/docs/Learn_web_development/Getting_started/Web_standards/How_the_web_works)), come vengono assemblati per creare l'esperienza finale con cui l'utente interagisce?

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del proprio computer, i browser web e le tecnologie web.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>I diversi tipi di asset che sono restituiti in una risposta HTTP.</li>
          <li>Come i diversi file sono assemblati dal browser per renderizzare una pagina web che viene poi visualizzata dall'utente.</li>
          <li>Perché il browser è a volte considerato un ambiente di programmazione ostile, ma anche un ambiente di programmazione eccezionale.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quali file vengono restituiti nelle risposte HTTP?

Per riassumere la [panoramica delle tecnologie web](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#overview_of_modern_web_technologies) che abbiamo visto nell'ultimo articolo, le risposte HTTP (alle richieste per una pagina web) generalmente contengono alcuni dei seguenti tipi di file:

- File HTML, che specificano il contenuto della pagina web e la sua struttura.
- File CSS, che specificano le informazioni di stile e layout.
- File JavaScript, che specificano il comportamento delle parti interattive della pagina web.
- Asset multimediali come immagini, video, file audio, {{Glossary("PDF", "PDF")}} e {{Glossary("SVG", "SVG")}}, che sono incorporati nelle pagine web o altrimenti visualizzati dal browser.
- Altri tipi di file che il browser non può gestire nativamente e quindi passa a un'applicazione pertinente sul dispositivo per renderizzarli, ad esempio documenti Word o Pages, presentazioni PowerPoint e file Open Office.

## Renderizzazione della pagina web

Quando l'utente naviga verso una nuova pagina web (cliccando su un link o inserendo un indirizzo web nella barra degli indirizzi del browser), vengono inviati diversi richieste HTTP, e diversi file sono inviati indietro nelle risposte HTTP. I file ricevuti in queste risposte vengono processati dal browser e assemblati in una pagina web con cui l'utente può interagire. Questo processo di assemblaggio dei pezzi in una pagina web è chiamato **renderizzazione**.

Le sezioni seguenti forniscono una spiegazione ad alto livello di come un browser renderizza una pagina web. Ricordiamo che questa è una descrizione semplificata, e che diversi browser gestiranno il processo in modi diversi. Tuttavia, questo vi darà comunque un'idea di come funziona.

## Gestire l'HTML

Per cominciare, il file HTML che contiene il contenuto della pagina web e definisce la sua struttura viene ricevuto dal browser e analizzato. Il browser lo converte in una struttura ad albero chiamata **DOM tree** (**Document Object Model**). Il DOM rappresenta la struttura del documento HTML nella memoria del computer. Prendiamo come esempio questo frammento di HTML di base:

```html
<p>
  Let's use:
  <span>HTML</span>
  <span>CSS</span>
  <span>JavaScript</span>
</p>
```

Ogni elemento, attributo e parte di testo nell'HTML diventa un **nodo DOM** nella struttura ad albero. I nodi sono definiti dalla loro relazione con altri nodi DOM. Alcuni elementi sono genitori di nodi figli, e i nodi figli hanno fratelli. Il browser analizzerà questo HTML e creerà il seguente albero DOM da esso:

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

{{EmbedLiveSample('Gestione dell\'HTML', '100%', 55)}}

```css hidden
p {
  margin: 0;
}
```

Certi elementi HTML, quando analizzati, attiveranno ulteriori richieste HTTP:

- Gli elementi {{htmlelement("link")}} che referenziano fogli di stile [CSS](/it/docs/Learn_web_development/Core/Styling_basics) esterni.
- Gli elementi {{htmlelement("script")}} che referenziano file [JavaScript](/it/docs/Learn_web_development/Core/Scripting) esterni.
- Elementi come {{htmlelement("img")}}, {{htmlelement("video")}}, e {{htmlelement("audio")}}, che referenziano file multimediali che si vogliono incorporare nella pagina web.

## Analisi del CSS e renderizzazione della pagina

Vediamo ora come viene gestito il CSS.

1. Il browser analizza il CSS trovato sulla pagina (incluso nel file HTML o recuperato da fogli di stile esterni), e ordina le diverse regole di stile CSS in diversi "contenitori" a seconda degli elementi HTML (rappresentati nel DOM come elementi chiamati **nodi**) a cui verranno applicati. Il browser quindi collega gli stili ai diversi elementi come richiesto (questo passo intermedio è chiamato albero di rendering).
2. L'albero di rendering è disposto nella struttura in cui dovrebbe apparire dopo che le regole sono state applicate. Questo include qualsiasi immagine e altro file multimediale che deve essere incorporato nella pagina.
3. La visualizzazione della pagina viene mostrata sullo schermo (questa fase è chiamata pittura).

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

L'unica regola disponibile nel CSS ha un selettore `span`, quindi il browser è in grado di ordinare il CSS molto rapidamente! Applica quella regola a ciascuno dei tre nodi SPAN nell'albero DOM, dando loro un bordo nero e un sfondo verde lime, quindi dipinge la rappresentazione visiva finale sullo schermo.

L'output aggiornato è il seguente:

{{EmbedLiveSample('Analisi del CSS e renderizzazione della pagina', '100%', 90)}}

## Gestione di JavaScript

Qualsiasi JavaScript trovato sulla pagina (incluso nel file HTML o recuperato da file di script esterni) viene analizzato, interpretato, compilato ed eseguito. Questo avviene a un certo punto prima che la renderizzazione finale della pagina sia completata — dopo tutto, alcuni JavaScript possono influenzare il rendering, ad esempio aggiungendo nodi al DOM o modificando quelli esistenti.

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

Non è necessario comprendere esattamente come funziona questo JavaScript, ma a un livello alto, trova ogni nodo SPAN nel DOM e inverte l'ordine dei caratteri nei loro nodi di testo figli.

L'output finale è il seguente:

{{EmbedLiveSample('Gestione del JavaScript', '100%', 90)}}

## Quali altri passaggi di rendering ci sono?

Diversi altri eventi si verificano durante la renderizzazione della pagina, ma non li discuteremo tutti qui. Un evento aggiuntivo degno di nota è che viene costruito un albero di accessibilità, basato sul DOM, per le tecnologie assistive (ad esempio lettori di schermo) in modo da consentire alle persone che non sono in grado di vedere il contenuto renderizzato di interagire con esso.

Imparerai di più su questo in seguito, nel nostro modulo [Accessibilità](/it/docs/Learn_web_development/Core/Accessibility).

## Il browser: un ambiente di programmazione ostile _e_ eccezionale

Lo sviluppo web front-end può a volte essere frustrante e alcune persone considerano il browser un ambiente di programmazione ostile. Questo perché, a differenza di altri ambienti di programmazione, è molto più difficile fare garanzie sull'ambiente in cui il tuo codice verrà eseguito. Non puoi sapere in anticipo tutte le diverse combinazioni di sistema operativo, browser, lingua, posizione, connessione di rete, CPU, GPU, memoria, durata della batteria, ecc., che i tuoi utenti avranno, quindi non puoi garantire un'esperienza utente perfetta per tutti loro.

I browser moderni tendono a implementare gli standard web in modo abbastanza coerente, ma c'è ancora molta incertezza da affrontare. Come sviluppatore web, dovrai abbracciare questa incertezza, programmando in modo difensivo ed essendo conservativo con le funzionalità che utilizzi. Questo si basa sull'adesione alle [migliori pratiche](/it/docs/Learn_web_development/Getting_started/Web_standards/The_web_standards_model#web_best_practices) delineate nell'articolo precedente.

D'altro canto, il web è anche un ambiente di programmazione eccezionale, per molte ragioni.

- Innanzitutto, è progettato con l'accesso universale in mente. La condizione di base del web è accessibile e collegabile. Alcuni di questi aspetti di base sono più difficili da ottenere in altri ambienti.
- La distribuzione delle app attraverso il web è semplice e potente. Non è necessario far passare gli utenti attraverso un processo di installazione complicato: basta indirizzarli a un indirizzo web e via.
- Gli aggiornamenti delle app sono di solito semplici. In molti casi, i visitatori possono vedere nuove versioni di un'applicazione quando ricaricano la scheda del browser. Non è necessario preoccuparsi di far scaricare e installare regolarmente aggiornamenti software agli utenti.
- La comunità del web è vivace e utile. Come discuteremo in seguito nel nostro articolo [Ricerca e apprendimento](/it/docs/Learn_web_development/Getting_started/Soft_skills/Research_and_learning), ci sono molti posti dove puoi andare per chiedere aiuto e ottime risorse disponibili da cui imparare.

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/The_web_standards_model", "Learn_web_development/Getting_started/Soft_skills", "Learn_web_development/Getting_started/Web_standards")}}
