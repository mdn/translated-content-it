---
title: Introduzione alla DOM scripting
short-title: DOM scripting
slug: Learn_web_development/Core/Scripting/DOM_scripting
l10n:
  sourceCommit: 0915a5e602d475bd1a1a57d905f0bac1b7ed57b8
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Object_basics","Learn_web_development/Core/Scripting/Network_requests", "Learn_web_development/Core/Scripting")}}

Quando si scrivono pagine web e app, una delle cose più comuni che si desidera fare è cambiare la struttura del documento in qualche modo. Questo viene solitamente fatto manipolando il Document Object Model (DOM) tramite un set di API integrate nei browser per controllare l'HTML e le informazioni di stile. In questo articolo ti introdurremo alla **DOM scripting**.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è il DOM — la rappresentazione interna del browser della struttura HTML del documento come gerarchia di oggetti.</li>
          <li>Le parti importanti di un browser web come rappresentate in JavaScript — <code>Navigator</code>, <code>Window</code>, e <code>Document</code>.</li>
          <li>Come i nodi del DOM esistono in relazione l'uno all'altro nell'albero DOM — radice, genitore, figlio, fratello e discendente.</li>
          <li>Ottenere riferimenti ai nodi del DOM, creare nuovi nodi, aggiungere e rimuovere nodi e attributi.</li>
          <li>Manipolare gli stili CSS con JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Le parti importanti di un browser web

I browser web sono software molto complessi con molte parti in movimento, molte delle quali non possono essere controllate o manipolate da uno sviluppatore web usando JavaScript. Potresti pensare che tali limitazioni siano una cosa negativa, ma i browser sono bloccati per buone ragioni, principalmente incentrate sulla sicurezza. Immagina se un sito web potesse accedere alle tue password memorizzate o ad altre informazioni sensibili, e accedere ai siti web come se fossi tu?

Nonostante le limitazioni, le API Web ci danno comunque accesso a molte funzionalità che ci permettono di fare molte cose con le pagine web. Ci sono alcune parti davvero ovvie che farai riferimento regolarmente nel tuo codice — considera il seguente diagramma, che rappresenta le principali parti di un browser direttamente coinvolte nella visualizzazione delle pagine web:

![Parti importanti del browser web; il documento è la pagina web. La finestra include l'intero documento e anche la scheda. Il navigatore è il browser, che include la finestra (che include il documento) e tutte le altre finestre.](document-window-navigator.png)

- La finestra è la scheda del browser in cui è caricata una pagina web; questa è rappresentata in JavaScript dall'oggetto [`Window`](/it/docs/Web/API/Window). Utilizzando i metodi disponibili su questo oggetto puoi fare cose come restituire la dimensione della finestra (vedi [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) e [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight)), manipolare il documento caricato in quella finestra, memorizzare dati specifici di quel documento lato client (ad esempio usando un database locale o un altro meccanismo di archiviazione), allegare un [gestore eventi](/it/docs/Learn_web_development/Core/Scripting/Events) alla finestra corrente, e altro ancora.
- Il navigatore rappresenta lo stato e l'identità del browser (cioè lo user-agent) così come esiste sul web. In JavaScript, questo è rappresentato dall'oggetto [`Navigator`](/it/docs/Web/API/Navigator). Puoi utilizzare questo oggetto per recuperare cose come la lingua preferita dell'utente, un flusso multimediale dalla webcam dell'utente, ecc.
- Il documento (rappresentato dal DOM nei browser) è la pagina attuale caricata nella finestra, ed è rappresentato in JavaScript dall'oggetto [`Document`](/it/docs/Web/API/Document). Puoi usare questo oggetto per restituire e manipolare informazioni sull'HTML e il CSS che compongono il documento, ad esempio ottenere un riferimento a un elemento nel DOM, cambiare il suo contenuto di testo, applicare nuovi stili ad esso, creare nuovi elementi e aggiungerli all'elemento corrente come figli, o addirittura eliminarlo completamente.

In questo articolo ci concentreremo principalmente sulla manipolazione del documento, ma mostreremo anche alcune altre parti utili.

## Il modello a oggetti documento

Facciamo un breve riepilogo sul Document Object Model (DOM), che abbiamo già esaminato all'inizio del corso. Il documento attualmente caricato in ciascuna delle tue schede del browser è rappresentato da un DOM. Questa è una rappresentazione a "struttura ad albero" creata dal browser che permette alla struttura HTML di essere facilmente accessibile dai linguaggi di programmazione — ad esempio il browser stesso la utilizza per applicare gli stili e altre informazioni agli elementi corretti mentre rende una pagina, e sviluppatori come te possono manipolare il DOM con JavaScript dopo che la pagina è stata resa.

> [!NOTE] > [The Document Object Model](https://scrimba.com/learn-javascript-c0v/~0g?via=mdn) su Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce una comoda guida al termine "DOM" e a cosa significa.

Abbiamo creato una pagina di esempio su [dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example.html) ([vedi anche dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/dom-example.html)). Prova ad aprirla nel tuo browser — è una pagina molto semplice che contiene un elemento {{htmlelement("section")}} all'interno del quale puoi trovare un'immagine, e un paragrafo con un link all'interno. Il codice sorgente HTML appare così:

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <title>Simple DOM example</title>
  </head>
  <body>
    <section>
      <img
        src="dinosaur.png"
        alt="A red Tyrannosaurus Rex: A two legged dinosaur standing upright like a human, with small arms, and a large head with lots of sharp teeth." />
      <p>
        Here we will add a link to the
        <a href="https://www.mozilla.org/">Mozilla homepage</a>
      </p>
    </section>
  </body>
</html>
```

Il DOM invece appare così:

![Rappresentazione a struttura ad albero del Document Object Model: Il nodo più alto è il doctype e l'elemento HTML. I nodi figli dell'HTML includono testa e corpo. Ogni elemento figlio è un ramo. Tutto il testo, anche lo spazio bianco, è mostrato anch'esso.](dom-screenshot.png)

> [!NOTE]
> Questo diagramma dell'albero DOM è stato creato utilizzando il [Live DOM viewer](https://software.hixie.ch/utilities/js/live-dom-viewer/) di Ian Hickson.

Ogni voce nell'albero è chiamata **nodo**. Puoi vedere nel diagramma sopra che alcuni nodi rappresentano elementi (identificati come `HTML`, `HEAD`, `META` e così via) e altri rappresentano testo (identificati come `#text`). Ci sono [altri tipi di nodi](/it/docs/Web/API/Node/nodeType), ma questi sono i principali che incontrerai.

I nodi vengono anche indicati con la loro posizione nell'albero relativamente ad altri nodi:

- **Nodo radice**: Il nodo più alto nell'albero, che nel caso dell'HTML è sempre il nodo `HTML` (altri vocabolari di markup come SVG e XML personalizzati avranno elementi radice diversi).
- **Nodo figlio**: Un nodo _direttamente_ all'interno di un altro nodo. Ad esempio, `IMG` è un figlio di `SECTION` nell'esempio sopra.
- **Nodo discendente**: Un nodo _ovunque_ all'interno di un altro nodo. Ad esempio, `IMG` è un figlio di `SECTION` nell'esempio sopra, ed è anche un discendente. `IMG` non è un figlio di `BODY`, poiché è due livelli al di sotto nell'albero, ma è un discendente di `BODY`.
- **Nodo padre**: Un nodo che ha un altro nodo al suo interno. Ad esempio, `BODY` è il nodo padre di `SECTION` nell'esempio sopra.
- **Nodi fratelli**: Nodi che si trovano sullo stesso livello sotto lo stesso nodo padre nell'albero DOM. Ad esempio, `IMG` e `P` sono fratelli nell'esempio sopra.

È utile familiarizzare con questa terminologia prima di lavorare con il DOM, poiché un certo numero di termini nel codice che incontrerai li utilizza. Li incontrerai anche nel CSS (ad esempio, selettore di discendenti, selettore di figli).

## Apprendimento attivo: manipolazione di base del DOM

Per iniziare a imparare sulla manipolazione del DOM, cominciamo con un esempio pratico.

1. Prendi una copia locale della [pagina dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example.html) e dell'[immagine](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dinosaur.png) che viene fornita insieme ad essa.
2. Aggiungi un elemento `<script></script>` appena sopra il tag di chiusura `</body>`.
3. Per manipolare un elemento all'interno del DOM, devi prima selezionarlo e memorizzare un riferimento ad esso all'interno di una variabile. All'interno del tuo elemento script, aggiungi la seguente riga:

   ```js
   const link = document.querySelector("a");
   ```

4. Ora che abbiamo il riferimento all'elemento memorizzato in una variabile, possiamo iniziare a manipolarlo utilizzando le proprietà e i metodi disponibili su di esso (questi sono definiti su interfacce come [`HTMLAnchorElement`](/it/docs/Web/API/HTMLAnchorElement) nel caso dell'elemento {{htmlelement("a")}}, la sua interfaccia padre più generale [`HTMLElement`](/it/docs/Web/API/HTMLElement), e [`Node`](/it/docs/Web/API/Node) — che rappresenta tutti i nodi in un DOM). Prima di tutto, cambiamo il testo all'interno del link aggiornando il valore della proprietà [`Node.textContent`](/it/docs/Web/API/Node/textContent). Aggiungi la seguente riga sotto la precedente:

   ```js
   link.textContent = "Mozilla Developer Network";
   ```

5. Dovremmo anche cambiare l'URL a cui fa riferimento il link, in modo che non porti al posto sbagliato quando viene cliccato. Aggiungi la seguente riga, di nuovo in fondo:

   ```js
   link.href = "https://developer.mozilla.org";
   ```

Nota che, come in molte altre situazioni in JavaScript, ci sono molti modi per selezionare un elemento e memorizzare un riferimento ad esso in una variabile. [`Document.querySelector()`](/it/docs/Web/API/Document/querySelector) è l'approccio moderno raccomandato. È conveniente perché permette di selezionare elementi usando i selettori CSS. La chiamata `querySelector()` sopra corrisponderà al primo elemento {{htmlelement("a")}} che appare nel documento. Se si desidera corrispondere e fare cose a vari elementi, si potrebbe utilizzare [`Document.querySelectorAll()`](/it/docs/Web/API/Document/querySelectorAll), che corrisponde a ogni elemento nel documento che corrisponde al selettore, e memorizza i riferimenti ad essi in un oggetto simile a un [array](/it/docs/Learn_web_development/Core/Scripting/Arrays) chiamato [`NodeList`](/it/docs/Web/API/NodeList).

Ci sono metodi più vecchi disponibili per ottenere riferimenti agli elementi, come:

- [`Document.getElementById()`](/it/docs/Web/API/Document/getElementById), che seleziona un elemento con un dato valore di attributo `id`, ad esempio `<p id="myId">Il mio paragrafo</p>`. L'ID viene passato alla funzione come parametro, cioè `const elementRef = document.getElementById('myId')`.
- [`Document.getElementsByTagName()`](/it/docs/Web/API/Document/getElementsByTagName), che restituisce un oggetto simile a un array contenente tutti gli elementi sulla pagina di un dato tipo, ad esempio `<p>`, `<a>`, ecc. Il tipo di elemento viene passato alla funzione come parametro, cioè `const elementRefArray = document.getElementsByTagName('p')`.

Questi due funzionano meglio in browser più vecchi rispetto ai metodi moderni come `querySelector()`, ma non sono altrettanto convenienti. Dai un'occhiata e vedi cosa puoi trovare!

### Creazione e posizionamento di nuovi nodi

Quanto detto sopra ti ha dato un piccolo assaggio di ciò che puoi fare, ma andiamo oltre e vediamo come possiamo creare nuovi elementi.

1. Tornando all'esempio corrente, iniziamo con l'ottenere un riferimento al nostro elemento {{htmlelement("section")}} — aggiungi il seguente codice alla fine del tuo script esistente (fai lo stesso con le altre righe):

   ```js
   const sect = document.querySelector("section");
   ```

2. Ora creiamo un nuovo paragrafo usando [`Document.createElement()`](/it/docs/Web/API/Document/createElement) e diamogli un po' di contenuto di testo nello stesso modo di prima:

   ```js
   const para = document.createElement("p");
   para.textContent = "We hope you enjoyed the ride.";
   ```

3. Puoi ora aggiungere il nuovo paragrafo alla fine della sezione usando [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild):

   ```js
   sect.appendChild(para);
   ```

4. Infine per questa parte, aggiungiamo un nodo di testo al paragrafo in cui il link è inserito, per completare la frase in modo armonico. Per prima cosa creeremo il nodo di testo usando [`Document.createTextNode()`](/it/docs/Web/API/Document/createTextNode):

   ```js
   const text = document.createTextNode(
     " — the premier source for web development knowledge.",
   );
   ```

5. Ora otterremo un riferimento al paragrafo in cui è inserito il link e aggiungeremo il nodo di testo:

   ```js
   const linkPara = document.querySelector("p");
   linkPara.appendChild(text);
   ```

Questo è la maggior parte di ciò di cui hai bisogno per aggiungere nodi al DOM — farai molto uso di questi metodi quando costruisci interfacce dinamiche (guarderemo alcuni esempi più avanti).

### Spostare e rimuovere elementi

Ci possono essere volte in cui si desidera spostare nodi o eliminarli completamente dal DOM. Questo è perfettamente possibile.

Se volessimo spostare il paragrafo con il link all'interno in fondo alla sezione, potremmo fare questo:

```js
sect.appendChild(linkPara);
```

Questo sposta il paragrafo in fondo alla sezione. Potresti aver pensato che avrebbe fatto una seconda copia di esso, ma non è così — `linkPara` è un riferimento all'unica copia di quel paragrafo. Se volevi fare una copia e aggiungerla, avresti bisogno di usare [`Node.cloneNode()`](/it/docs/Web/API/Node/cloneNode) invece.

Rimuovere un nodo è abbastanza semplice, almeno quando hai un riferimento al nodo da rimuovere e al suo genitore. Nel nostro caso corrente, usiamo semplicemente [`Node.removeChild()`](/it/docs/Web/API/Node/removeChild), in questo modo:

```js
sect.removeChild(linkPara);
```

Quando vuoi rimuovere un nodo basandoti solo su un riferimento a se stesso, il che è abbastanza comune, puoi usare [`Element.remove()`](/it/docs/Web/API/Element/remove):

```js
linkPara.remove();
```

Questo metodo non è supportato nei browser più vecchi. Non hanno un metodo per dire a un nodo di rimuovere se stesso, quindi dovresti fare quanto segue:

```js
linkPara.parentNode.removeChild(linkPara);
```

Prova ad aggiungere le righe sopra al tuo codice.

### Manipolazione degli stili

È possibile manipolare gli stili CSS tramite JavaScript in vari modi.

Per iniziare, puoi ottenere un elenco di tutti gli stili allegati a un documento usando [`Document.styleSheets`](/it/docs/Web/API/Document/styleSheets), che restituisce un oggetto simile a un array con oggetti [`CSSStyleSheet`](/it/docs/Web/API/CSSStyleSheet). Puoi poi aggiungere/rimuovere stili a piacere. Tuttavia, non espanderemo queste caratteristiche perché sono un modo alquanto arcaico e difficile per manipolare gli stili. Ci sono modi molto più facili.

Il primo modo è aggiungere stili inline direttamente sugli elementi che vuoi stilizzare dinamicamente. Questo si fa con la proprietà [`HTMLElement.style`](/it/docs/Web/API/HTMLElement/style), che contiene informazioni sullo stile inline per ogni elemento nel documento. Puoi impostare le proprietà di questo oggetto per aggiornare direttamente gli stili degli elementi.

1. Ad esempio, prova ad aggiungere queste righe al nostro esempio in corso:

   ```js
   para.style.color = "white";
   para.style.backgroundColor = "black";
   para.style.padding = "10px";
   para.style.width = "250px";
   para.style.textAlign = "center";
   ```

2. Ricarica la pagina e vedrai che gli stili sono stati applicati al paragrafo. Se guardi quel paragrafo nel [Page Inspector/DOM inspector](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/index.html) del tuo browser, vedrai che queste righe stanno effettivamente aggiungendo stili inline al documento:

   ```html
   <p
     style="color: white; background-color: black; padding: 10px; width: 250px; text-align: center;">
     We hope you enjoyed the ride.
   </p>
   ```

> [!NOTE]
> Nota come le versioni delle proprietà JavaScript degli stili CSS sono scritte in {{Glossary("camel_case", "lower camel case")}} mentre le versioni CSS sono separate da trattini ({{Glossary("kebab_case", "kebab-case")}}) (ad esempio, `backgroundColor` contro `background-color`). Assicurati di non confonderle, altrimenti non funzionerà.

C'è un altro modo comune per manipolare dinamicamente gli stili sul tuo documento, che guarderemo ora.

1. Elimina le cinque righe precedenti che hai aggiunto al JavaScript.
2. Aggiungi il seguente codice all'interno del tuo {{htmlelement("head")}} HTML:

   ```html
   <style>
     .highlight {
       color: white;
       background-color: black;
       padding: 10px;
       width: 250px;
       text-align: center;
     }
   </style>
   ```

3. Ora passeremo a un metodo molto utile per la manipolazione generale dell'HTML — [`Element.setAttribute()`](/it/docs/Web/API/Element/setAttribute) — questo prende due argomenti, l'attributo che vuoi impostare sull'elemento, e il valore a cui vuoi impostarlo. In questo caso imposteremo un nome di classe highlight sul nostro paragrafo:

   ```js
   para.setAttribute("class", "highlight");
   ```

4. Aggiorna la tua pagina, e non vedrai nessun cambiamento — il CSS è ancora applicato al paragrafo, ma questa volta dandogli una classe che viene selezionata dalla nostra regola CSS, non come stili CSS inline.

Quale metodo scegli è a tua discrezione; entrambi hanno i loro vantaggi e svantaggi. Il primo metodo richiede meno impostazioni ed è buono per usi semplici, mentre il secondo metodo è più purista (nessun mixing di CSS e JavaScript, nessuno stile inline, che è visto come una cattiva pratica). Mentre inizi a costruire app più grandi e coinvolte, probabilmente inizierai a usare di più il secondo metodo, ma dipende davvero da te.

A questo punto, in realtà non abbiamo fatto nulla di utile! Non ha senso usare JavaScript per creare contenuti statici — potresti altrettanto scriverli nell'HTML e non usare JavaScript. È più complesso dell'HTML, e creare i tuoi contenuti con JavaScript comporta anche altri problemi (come non essere leggibile dai motori di ricerca).

Nella prossima sezione esamineremo un uso più pratico delle API del DOM.

> [!NOTE]
> Puoi trovare la nostra [versione finita della demo dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example-manipulated.html) su GitHub ([vedi anche dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/dom-example-manipulated.html)).

## Apprendimento attivo: una lista della spesa dinamica

In questa sfida vogliamo creare un semplice esempio di lista della spesa che ti permetta di aggiungere dinamicamente elementi alla lista usando un input del form e un pulsante. Quando aggiungi un elemento all'input e premi il pulsante:

- L'elemento dovrebbe apparire nella lista.
- Ogni elemento dovrebbe essere dotato di un pulsante che può essere premuto per cancellare quell'elemento dalla lista.
- L'input dovrebbe essere svuotato e focalizzato, pronto per inserire un altro elemento.

La demo finale sembrerà qualcosa di simile a questo:

![Layout demo di una lista della spesa. Un'intestazione "la mia lista della spesa" seguita da "Inserisci un nuovo elemento" con un campo di input e pulsante "aggiungi elemento". La lista degli elementi già aggiunti è sotto, ciascuno con un corrispondente pulsante di eliminazione.](shopping-list.png)

Per completare l'esercizio, segui le istruzioni qui sotto e assicurati che la lista si comporti come descritto sopra.

1. Per iniziare, scarica una copia del nostro file di partenza [shopping-list.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/shopping-list.html) e copialo da qualche parte. Vedrai che ha un po' di CSS minimale, un div con un'etichetta, input, e pulsante, una lista vuota e un elemento {{htmlelement("script")}}. Farai tutte le tue aggiunte all'interno dello script.
2. Crea tre variabili che contengano riferimenti alla lista ({{htmlelement("ul")}}), {{htmlelement("input")}}, e {{htmlelement("button")}}.
3. Crea una [funzione](/it/docs/Learn_web_development/Core/Scripting/Functions) che verrà eseguita in risposta alla pressione del pulsante.
4. All'interno del corpo della funzione, inizia memorizzando il [valore](/it/docs/Web/API/HTMLInputElement/value) corrente dell'elemento input in una variabile.
5. Successivamente, svuota l'elemento input impostando il suo valore su una stringa vuota — `''`.
6. Crea tre nuovi elementi — un elemento della lista ({{htmlelement('li')}}), {{htmlelement('span')}}, e {{htmlelement('button')}}, e memorizzali in variabili.
7. Allegare lo span e il pulsante come figli dell'elemento della lista.
8. Imposta il contenuto di testo dello span al valore dell'elemento input che hai salvato in precedenza, e il contenuto di testo del pulsante su 'Delete'.
9. Aggiungi l'elemento della lista come figlio della lista.
10. Collega un gestore eventi al pulsante di eliminazione in modo che, quando viene cliccato, elimini l'intero elemento della lista (`<li>...</li>`).
11. Infine, usa il metodo [`focus()`](/it/docs/Web/API/HTMLElement/focus) per focalizzare l'elemento input pronto per inserire il prossimo elemento della lista della spesa.

> [!NOTE]
> Se sei veramente bloccato, dai un'occhiata alla nostra [lista della spesa finita](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/shopping-list-finished.html) ([vedi anche in esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/shopping-list-finished.html)).

## Sommario

Abbiamo raggiunto la fine del nostro studio sulla manipolazione dei documenti e del DOM. A questo punto dovresti comprendere quali sono le parti importanti di un browser web rispetto al controllo dei documenti e ad altri aspetti dell'esperienza web dell'utente. Soprattutto, dovresti comprendere cos'è il Document Object Model e come manipolarlo per creare funzionalità utili.

## Vedi anche

- Ci sono molte altre funzionalità che puoi usare per manipolare i tuoi documenti. Controlla alcuni dei nostri riferimenti e vedi cosa puoi scoprire:
  - [`Document`](/it/docs/Web/API/Document)
  - [`Window`](/it/docs/Web/API/Window)
  - [`Node`](/it/docs/Web/API/Node)
  - [`HTMLElement`](/it/docs/Web/API/HTMLElement), [`HTMLInputElement`](/it/docs/Web/API/HTMLInputElement), [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement), ecc.
- [DOM Scripting](https://explainers.dev/dom-scripting/), explainers.dev

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Object_basics","Learn_web_development/Core/Scripting/Network_requests", "Learn_web_development/Core/Scripting")}}
