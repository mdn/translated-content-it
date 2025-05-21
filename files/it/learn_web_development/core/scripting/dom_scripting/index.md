---
title: Introduzione al DOM scripting
short-title: DOM scripting
slug: Learn_web_development/Core/Scripting/DOM_scripting
l10n:
  sourceCommit: eec8e74ebe88d4fb05e74ebce6e471f1e3da5c6d
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Object_basics","Learn_web_development/Core/Scripting/Network_requests", "Learn_web_development/Core/Scripting")}}

Quando si scrivono pagine web e app, una delle cose più comuni che si desidera fare è modificare la struttura del documento in qualche modo. Questo di solito viene fatto manipolando il Document Object Model (DOM) tramite un set di API del browser integrate per controllare le informazioni HTML e di stile. In questo articolo ti introdurremo al **DOM scripting**.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprendere l'<a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e le <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenta del CSS</a>, familiarità con le basi di JavaScript come trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Che cosa è il DOM — la rappresentazione interna del browser della struttura HTML del documento come una gerarchia di oggetti.</li>
          <li>Le parti importanti di un browser web come rappresentate in JavaScript — <code>Navigator</code>, <code>Window</code> e <code>Document</code>.</li>
          <li>Come i nodi DOM esistono relativamente l'uno all'altro nell'albero DOM — radice, genitore, figlio, fratello e discendente.</li>
          <li>Ottenere riferimenti ai nodi DOM, creare nuovi nodi, aggiungere e rimuovere nodi e attributi.</li>
          <li>Manipolare stili CSS con JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Le parti importanti di un browser web

I browser web sono software molto complessi con molte parti in movimento, molte delle quali non possono essere controllate o manipolate da uno sviluppatore web usando JavaScript. Potresti pensare che tali limitazioni siano una cosa negativa, ma i browser sono bloccati per buone ragioni, principalmente legate alla sicurezza. Immagina se un sito web potesse accedere alle tue password memorizzate o ad altre informazioni sensibili, e accedere ai siti web come se fossi tu?

Nonostante le limitazioni, le API Web ci danno comunque accesso a molte funzionalità che ci permettono di fare molte cose con le pagine web. Ci sono alcune parti davvero ovvie a cui farai riferimento regolarmente nel tuo codice — considera il diagramma seguente, che rappresenta le principali parti di un browser direttamente coinvolte nella visualizzazione delle pagine web:

![Parti importanti del browser web; il documento è la pagina web. La finestra include l'intero documento e anche la scheda. Il navigatore è il browser, che include la finestra (che include il documento) e tutte le altre finestre.](document-window-navigator.png)

- La finestra è la scheda del browser in cui viene caricata una pagina web; questa è rappresentata in JavaScript dall'oggetto [`Window`](/it/docs/Web/API/Window). Usando i metodi disponibili su questo oggetto puoi fare cose come restituire la dimensione della finestra (vedi [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) e [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight)), manipolare il documento caricato in quella finestra, memorizzare dati specifici per quel documento sul lato client (per esempio usando un database locale o un altro meccanismo di archiviazione), agganciare un [gestore degli eventi](/it/docs/Learn_web_development/Core/Scripting/Events) alla finestra corrente, e altro ancora.
- Il navigatore rappresenta lo stato e l'identità del browser (cioè l'user-agent) mentre esiste sul web. In JavaScript, questo è rappresentato dall'oggetto [`Navigator`](/it/docs/Web/API/Navigator). Puoi usare questo oggetto per recuperare cose come la lingua preferita dell'utente, un flusso video dalla webcam dell'utente, ecc.
- Il documento (rappresentato dal DOM nei browser) è la pagina effettiva caricata nella finestra, ed è rappresentato in JavaScript dall'oggetto [`Document`](/it/docs/Web/API/Document). Puoi usare questo oggetto per restituire e manipolare informazioni sull'HTML e il CSS che compongono il documento, per esempio ottenere un riferimento a un elemento nel DOM, cambiare il suo contenuto di testo, applicare nuovi stili ad esso, creare nuovi elementi e aggiungerli all'elemento corrente come figli, oppure anche eliminarlo del tutto.

In questo articolo ci concentreremo principalmente sulla manipolazione del documento, ma mostreremo anche alcune altre parti utili.

## Il modello a oggetti del documento

Facciamo un breve riepilogo sul Document Object Model (DOM), di cui abbiamo già parlato nel corso precedente. Il documento attualmente caricato in ciascuna delle schede del tuo browser è rappresentato da un DOM. Questa è una rappresentazione a "struttura ad albero" creata dal browser che consente all'HTML di essere facilmente accessibile dai linguaggi di programmazione — per esempio il browser stesso lo utilizza per applicare stili e altre informazioni agli elementi corretti mentre rende una pagina, e sviluppatori come te possono manipolare il DOM con JavaScript dopo che la pagina è stata resa.

Abbiamo creato una pagina di esempio su [dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example.html) ([vedi anche dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/dom-example.html)). Prova ad aprirla nel tuo browser — è una pagina molto semplice contenente un elemento {{htmlelement("section")}} dentro il quale puoi trovare un'immagine e un paragrafo con un collegamento all'interno. Il codice sorgente HTML appare così:

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

![Rappresentazione a struttura ad albero del Document Object Model: il nodo superiore è il doctype e l'elemento HTML. I nodi figli di HTML includono head e body. Ogni elemento figlio è un ramo. Tutti i testi, compresi gli spazi bianchi, sono mostrati.](dom-screenshot.png)

> [!NOTE]
> Questo diagramma dell'albero DOM è stato creato usando il [Live DOM viewer](https://software.hixie.ch/utilities/js/live-dom-viewer) di Ian Hickson.

Ogni voce nell'albero è chiamata un **nodo**. Puoi vedere nel diagramma sopra che alcuni nodi rappresentano elementi (identificati come `HTML`, `HEAD`, `META` e così via) e altri rappresentano testo (identificati come `#text`). Ci sono [altri tipi di nodi](/it/docs/Web/API/Node/nodeType) ma questi sono i principali che incontrerai.

I nodi sono anche riferiti in base alla loro posizione nell'albero rispetto ad altri nodi:

- **Nodo radice**: Il nodo superiore nell'albero, che nel caso dell'HTML è sempre il nodo `HTML` (altri vocabolari di markup come SVG e XML personalizzati avranno elementi radice diversi).
- **Nodo figlio**: Un nodo _direttamente_ all'interno di un altro nodo. Per esempio, `IMG` è un figlio di `SECTION` nell'esempio sopra.
- **Nodo discendente**: Un nodo _ovunque_ all'interno di un altro nodo. Per esempio, `IMG` è un figlio di `SECTION` nell'esempio sopra, ed è anche un discendente. `IMG` non è un figlio di `BODY`, poiché si trova due livelli sotto di esso nell'albero, ma è un discendente di `BODY`.
- **Nodo genitore**: Un nodo che ha un altro nodo al suo interno. Per esempio, `BODY` è il nodo genitore di `SECTION` nell'esempio sopra.
- **Nodi fratelli**: Nodi che si trovano allo stesso livello sotto lo stesso nodo genitore nell'albero DOM. Per esempio, `IMG` e `P` sono fratelli nell'esempio sopra.

È utile familiarizzare con questa terminologia prima di lavorare con il DOM, poiché molti dei termini di codice che incontrerai li utilizzano. Li troverai anche nel CSS (ad esempio, selettore discendente, selettore figlio).

## Apprendimento attivo: manipolazione base del DOM

Per iniziare ad apprendere sulla manipolazione del DOM, iniziamo con un esempio pratico.

1. Prendi una copia locale della [pagina dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example.html) e dell'[immagine](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dinosaur.png) che la accompagna.
2. Aggiungi un elemento `<script></script>` appena sopra il tag di chiusura `</body>`.
3. Per manipolare un elemento all'interno del DOM, devi prima selezionarlo e memorizzare un riferimento ad esso all'interno di una variabile. All'interno dell'elemento script, aggiungi la seguente riga:

   ```js
   const link = document.querySelector("a");
   ```

4. Ora abbiamo il riferimento all'elemento memorizzato in una variabile, possiamo iniziare a manipolarlo usando le proprietà e i metodi disponibili per esso (questi sono definiti su interfacce come [`HTMLAnchorElement`](/it/docs/Web/API/HTMLAnchorElement) nel caso dell'elemento {{htmlelement("a")}}, la sua interfaccia genitore più generale [`HTMLElement`](/it/docs/Web/API/HTMLElement) e [`Node`](/it/docs/Web/API/Node) — che rappresenta tutti i nodi in un DOM). Prima di tutto, cambiamo il testo all'interno del collegamento aggiornando il valore della proprietà [`Node.textContent`](/it/docs/Web/API/Node/textContent). Aggiungi la seguente riga sotto la precedente:

   ```js
   link.textContent = "Mozilla Developer Network";
   ```

5. Dovremmo anche cambiare l'URL a cui punta il collegamento, in modo che non vada nel posto sbagliato quando viene cliccato. Aggiungi la seguente riga, ancora una volta in fondo:

   ```js
   link.href = "https://developer.mozilla.org";
   ```

Nota che, come per molte cose in JavaScript, ci sono molti modi per selezionare un elemento e memorizzare un riferimento ad esso in una variabile. [`Document.querySelector()`](/it/docs/Web/API/Document/querySelector) è l'approccio moderno consigliato. È conveniente perché ti permette di selezionare elementi usando selettori CSS. La chiamata `querySelector()` sopra corrisponderà al primo elemento {{htmlelement("a")}} che appare nel documento. Se desideri corrispondere e fare cose a più elementi, puoi usare [`Document.querySelectorAll()`](/it/docs/Web/API/Document/querySelectorAll), che corrisponde a ogni elemento nel documento che corrisponde al selettore, e memorizza i riferimenti ad essi in un oggetto simile a una [array](/it/docs/Learn_web_development/Core/Scripting/Arrays) chiamato [`NodeList`](/it/docs/Web/API/NodeList).

Ci sono metodi più vecchi disponibili per ottenere riferimenti a elementi, come:

- [`Document.getElementById()`](/it/docs/Web/API/Document/getElementById), che seleziona un elemento con un dato valore di `id` attributo, ad esempio, `<p id="myId">Mio paragrafo</p>`. L'ID viene passato alla funzione come parametro, vale a dire, `const elementRef = document.getElementById('myId')`.
- [`Document.getElementsByTagName()`](/it/docs/Web/API/Document/getElementsByTagName), che restituisce un oggetto simile a una array contenente tutti gli elementi sulla pagina di un dato tipo, per esempio `<p>`, `<a>`, ecc. Il tipo di elemento è passato alla funzione come parametro, cioè, `const elementRefArray = document.getElementsByTagName('p')`.

Questi funzionano meglio nei browser più vecchi rispetto ai metodi più moderni come `querySelector()`, ma non sono così convenienti. Guarda e scopri quali altri puoi trovare!

### Creare e posizionare nuovi nodi

Quanto sopra ti ha dato un piccolo assaggio di quello che puoi fare, ma andiamo oltre e vediamo come possiamo creare nuovi elementi.

1. Tornando all'esempio corrente, iniziamo catturando un riferimento al nostro elemento {{htmlelement("section")}} — aggiungi il seguente codice in fondo al tuo script esistente (fa lo stesso con le altre linee):

   ```js
   const sect = document.querySelector("section");
   ```

2. Ora creiamo un nuovo paragrafo usando [`Document.createElement()`](/it/docs/Web/API/Document/createElement) e dargli un po' di contenuto di testo nello stesso modo di prima:

   ```js
   const para = document.createElement("p");
   para.textContent = "We hope you enjoyed the ride.";
   ```

3. Ora puoi appendere il nuovo paragrafo alla fine della sezione usando [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild):

   ```js
   sect.appendChild(para);
   ```

4. Infine per questa parte, aggiungiamo un nodo di testo al paragrafo in cui si trova il collegamento, per chiudere bene la frase. Prima creeremo il nodo di testo usando [`Document.createTextNode()`](/it/docs/Web/API/Document/createTextNode):

   ```js
   const text = document.createTextNode(
     " — the premier source for web development knowledge.",
   );
   ```

5. Ora cattureremo un riferimento al paragrafo in cui si trova il collegamento e appenderemo il nodo di testo ad esso:

   ```js
   const linkPara = document.querySelector("p");
   linkPara.appendChild(text);
   ```

Questo è la maggior parte di ciò di cui hai bisogno per aggiungere nodi al DOM — farai molto uso di questi metodi quando costruisci interfacce dinamiche (vedremo alcuni esempi più avanti).

### Spostare e rimuovere elementi

Ci possono essere volte in cui vuoi spostare nodi, o eliminarli completamente dal DOM. Questo è perfettamente possibile.

Se volessimo spostare il paragrafo con il collegamento all'interno in fondo alla sezione, potremmo fare questo:

```js
sect.appendChild(linkPara);
```

Questo sposta il paragrafo fino al fondo della sezione. Potresti aver pensato che creerebbe una seconda copia di esso, ma non è così — `linkPara` è un riferimento all'unica copia di quel paragrafo. Se volessi creare una copia e aggiungerla anche, dovresti usare [`Node.cloneNode()`](/it/docs/Web/API/Node/cloneNode) invece.

Rimuovere un nodo è abbastanza semplice, almeno quando hai un riferimento al nodo da rimuovere e al suo genitore. Nel nostro caso attuale, usiamo semplicemente [`Node.removeChild()`](/it/docs/Web/API/Node/removeChild), in questo modo:

```js
sect.removeChild(linkPara);
```

Quando vuoi rimuovere un nodo basato solo su un riferimento a se stesso, cosa abbastanza comune, puoi usare [`Element.remove()`](/it/docs/Web/API/Element/remove):

```js
linkPara.remove();
```

Questo metodo non è supportato nei browser più vecchi. Non hanno alcun metodo per dire a un nodo di rimuovere se stesso, quindi dovresti fare quanto segue:

```js
linkPara.parentNode.removeChild(linkPara);
```

Prova ad aggiungere queste righe al tuo codice.

### Manipolazione degli stili

È possibile manipolare gli stili CSS tramite JavaScript in diversi modi.

Innanzitutto, puoi ottenere un elenco di tutti gli stylesheet allegati a un documento usando [`Document.styleSheets`](/it/docs/Web/API/Document/styleSheets), che restituisce un oggetto simile a una array con oggetti [`CSSStyleSheet`](/it/docs/Web/API/CSSStyleSheet). Puoi quindi aggiungere/rimuovere stili come desideri. Tuttavia, non espanderemo quelle caratteristiche perché sono un modo alquanto arcaico e difficile per manipolare lo stile. Ci sono modi molto più semplici.

Il primo modo è aggiungere stili inline direttamente sugli elementi che vuoi stilizzare dinamicamente. Questo è fatto con la proprietà [`HTMLElement.style`](/it/docs/Web/API/HTMLElement/style), che contiene informazioni di stile inline per ciascun elemento nel documento. Puoi impostare proprietà di questo oggetto per aggiornare direttamente gli stili degli elementi.

1. Come esempio, prova ad aggiungere queste righe al nostro esempio in corso:

   ```js
   para.style.color = "white";
   para.style.backgroundColor = "black";
   para.style.padding = "10px";
   para.style.width = "250px";
   para.style.textAlign = "center";
   ```

2. Ricarica la pagina e vedrai che gli stili sono stati applicati al paragrafo. Se guardi quel paragrafo nell'[inspecteur della pagina/DOM inspector](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/index.html) del tuo browser, vedrai che queste righe stanno effettivamente aggiungendo stili inline al documento:

   ```html
   <p
     style="color: white; background-color: black; padding: 10px; width: 250px; text-align: center;">
     We hope you enjoyed the ride.
   </p>
   ```

> [!NOTE]
> Nota come le versioni JavaScript delle proprietà CSS sono scritte in {{Glossary("camel_case", "lower camel case")}} mentre le versioni CSS sono trattinate ({{Glossary("kebab_case", "kebab-case")}}) (es., `backgroundColor` rispetto a `background-color`). Assicurati di non confonderle, altrimenti non funzionerà.

C'è un altro modo comune per manipolare dinamicamente gli stili sul tuo documento, che ora esamineremo.

1. Cancella le precedenti cinque righe che hai aggiunto al JavaScript.
2. Aggiungi il seguente all'interno del tuo HTML {{htmlelement("head")}}:

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

3. Ora passeremo a un metodo molto utile per la manipolazione generale dell'HTML — [`Element.setAttribute()`](/it/docs/Web/API/Element/setAttribute) — questo prende due argomenti, l'attributo che vuoi impostare sull'elemento e il valore che vuoi impostare. In questo caso impostiamo un nome di classe di highlight sul nostro paragrafo:

   ```js
   para.setAttribute("class", "highlight");
   ```

4. Ricarica la pagina e vedrai che non ci sono cambiamenti — il CSS è ancora applicato al paragrafo, ma questa volta assegnandogli una classe che è selezionata dalla nostra regola CSS, non come stili CSS inline.

Quale metodo tu scelga dipende da te; entrambi hanno i loro vantaggi e svantaggi. Il primo metodo richiede meno configurazione ed è buono per usi semplici, mentre il secondo metodo è più purista (nessuna mescolanza tra CSS e JavaScript, nessuno stile inline, che è considerato una cattiva pratica). Man mano che inizi a costruire app più grandi e coinvolgenti, probabilmente inizierai a usare di più il secondo metodo, ma dipende davvero da te.

A questo punto, non abbiamo fatto nulla di utile! Non ha senso usare JavaScript per creare contenuti statici — sarebbe meglio scriverlo nel tuo HTML e non usare JavaScript. È più complesso dell'HTML e creare il tuo contenuto con JavaScript ha anche altri problemi legati (come non essere leggibile dai motori di ricerca).

Nella prossima sezione esamineremo un uso più pratico delle API DOM.

> [!NOTE]
> Puoi trovare la nostra [versione finale dell'esempio dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example-manipulated.html) su GitHub ([vedi anche dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/dom-example-manipulated.html)).

## Apprendimento attivo: Una lista della spesa dinamica

In questa sfida vogliamo creare un esempio semplice di lista della spesa che ti permetta di aggiungere dinamicamente elementi alla lista usando un modulo di input e un pulsante. Quando aggiungi un elemento all'input e premi il pulsante:

- L'elemento dovrebbe apparire nella lista.
- Ogni elemento dovrebbe avere un pulsante che possa essere premuto per eliminare quell'elemento dalla lista.
- L'input dovrebbe essere svuotato e focalizzato in modo da essere pronto per immettere un altro elemento.

La demo finale apparirà qualcosa di simile a questo:

![Layout demo di una lista della spesa. Un'intestazione 'la mia lista della spesa' seguita da 'Inserisci un nuovo elemento' con un campo di input e un pulsante 'aggiungi elemento'. La lista degli elementi già aggiunti è sotto, ciascuno con un pulsante elimina corrispondente. ](shopping-list.png)

Per completare l'esercizio, segui i passaggi seguenti e assicurati che la lista si comporti come descritto sopra.

1. Inizia scaricando una copia della nostra [pagina iniziale shopping-list.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/shopping-list.html) e fanne una copia da qualche parte. Vedrai che ha un po' di CSS minimale, un div con un'etichetta, input e pulsante, e una lista vuota e un elemento {{htmlelement("script")}}. Farai tutte le tue aggiunte all'interno dello script.
2. Crea tre variabili che contengano riferimenti all'elenco ({{htmlelement("ul")}}), {{htmlelement("input")}}, e elementi {{htmlelement("button")}}.
3. Crea una [funzione](/it/docs/Learn_web_development/Core/Scripting/Functions) che verrà eseguita in risposta al clic sul pulsante.
4. All'interno del corpo della funzione, inizia memorizzando il valore corrente dell'elemento input in una variabile.
5. Successivamente, svuota l'elemento input impostando il suo valore su una stringa vuota — `''`.
6. Crea tre nuovi elementi — un elemento di lista ({{htmlelement('li')}}), {{htmlelement('span')}} e {{htmlelement('button')}}, e memorizzali in variabili.
7. Appendi lo span e il pulsante come figli dell'elemento di lista.
8. Imposta il contenuto di testo dello span al valore dell'elemento input che hai salvato in precedenza, e il contenuto di testo del pulsante a 'Elimina'.
9. Appendi l'elemento di lista come figlio della lista.
10. Allegare un gestore degli eventi al pulsante elimina in modo che, quando viene cliccato, elimini l'intero elemento di lista (`<li>...</li>`).
11. Infine, usa il metodo [`focus()`](/it/docs/Web/API/HTMLElement/focus) per mettere a fuoco l'elemento input pronto per l'inserimento del prossimo elemento della lista della spesa.

> [!NOTE]
> Se ti blocchi davvero, dai un'occhiata alla nostra [lista della spesa finita](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/shopping-list-finished.html) ([vedi anche dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/shopping-list-finished.html)).

## Riassunto

Siamo giunti alla fine del nostro studio sulla manipolazione dei documenti e del DOM. A questo punto dovresti capire quali sono le parti importanti di un browser web rispetto al controllo dei documenti e altri aspetti dell'esperienza web dell'utente. Soprattutto, dovresti capire cos'è il Document Object Model e come manipolarlo per creare funzionalità utili.

## Vedi anche

- Ci sono molte altre funzionalità che puoi usare per manipolare i tuoi documenti. Dai un'occhiata ad alcuni dei nostri riferimenti e scopri cosa puoi scoprire:
  - [`Document`](/it/docs/Web/API/Document)
  - [`Window`](/it/docs/Web/API/Window)
  - [`Node`](/it/docs/Web/API/Node)
  - [`HTMLElement`](/it/docs/Web/API/HTMLElement), [`HTMLInputElement`](/it/docs/Web/API/HTMLInputElement), [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement), ecc.
- [DOM Scripting](https://explainers.dev/dom-scripting/), explainers.dev

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Object_basics","Learn_web_development/Core/Scripting/Network_requests", "Learn_web_development/Core/Scripting")}}
