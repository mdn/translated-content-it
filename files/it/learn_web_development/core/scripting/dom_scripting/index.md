---
title: Introduzione al DOM scripting
short-title: DOM scripting
slug: Learn_web_development/Core/Scripting/DOM_scripting
l10n:
  sourceCommit: eec8e74ebe88d4fb05e74ebce6e471f1e3da5c6d
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Object_basics","Learn_web_development/Core/Scripting/Network_requests", "Learn_web_development/Core/Scripting")}}

Quando si scrivono pagine web e app, una delle cose più comuni che si vorrà fare è modificare la struttura del documento in qualche modo. Questo è solitamente fatto manipolando il Document Object Model (DOM) attraverso un set di API del browser incorporate per controllare informazioni HTML e di stile. In questo articolo vi introdurremo al **DOM scripting**.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi di CSS</a>, familiarità con i fondamenti di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è il DOM — la rappresentazione interna da parte del browser della struttura HTML del documento come un'gerarchia di oggetti.</li>
          <li>Le parti importanti di un browser web come rappresentate in JavaScript — <code>Navigator</code>, <code>Window</code>, e <code>Document</code>.</li>
          <li>Come i nodi del DOM esistono relativi l'uno all'altro nell'albero del DOM — radice, genitore, figlio, fratello, e discendente.</li>
          <li>Ottenere riferimenti a nodi del DOM, creare nuovi nodi, aggiungere e rimuovere nodi e attributi.</li>
          <li>Manipolare stili CSS con JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Le parti importanti di un browser web

I browser web sono pezzi di software molto complicati con molte parti mobili, molte delle quali non possono essere controllate o manipolate da uno sviluppatore web usando JavaScript. Potrebbe pensare che tali limitazioni siano una cosa negativa, ma i browser sono bloccati per buone ragioni, principalmente centrate sulla sicurezza. Immagini se un sito web potesse accedere alle sue password memorizzate o ad altre informazioni sensibili, e accedere ai siti web come se fosse lei.

Nonostante le limitazioni, le API Web ci danno comunque accesso a molta funzionalità che ci permettono di fare molte cose con le pagine web. Ci sono alcuni pezzi davvero ovvi che farà riferimento regolarmente nel suo codice — consideri il seguente diagramma, che rappresenta le principali parti di un browser direttamente coinvolte nella visualizzazione di pagine web:

![Parti importanti del browser web; il documento è la pagina web. La finestra include l'intero documento e anche la scheda. Il navigatore è il browser, che include la finestra (che include il documento) e tutte le altre finestre.](document-window-navigator.png)

- La finestra è la scheda del browser in cui una pagina web è caricata; questo è rappresentato in JavaScript dall'oggetto [`Window`](/it/docs/Web/API/Window). Utilizzando i metodi disponibili su questo oggetto può fare cose come restituire la dimensione della finestra (veda [`Window.innerWidth`](/it/docs/Web/API/Window/innerWidth) e [`Window.innerHeight`](/it/docs/Web/API/Window/innerHeight)), manipolare il documento caricato in quella finestra, memorizzare dati specifici a quel documento sul lato client (per esempio usando un database locale o un altro meccanismo di archiviazione), allegare un [gestore degli eventi](/it/docs/Learn_web_development/Core/Scripting/Events) alla finestra corrente, e altro ancora.
- Il navigatore rappresenta lo stato e l'identità del browser (cioè l'user-agent) come esiste sul web. In JavaScript, questo è rappresentato dall'oggetto [`Navigator`](/it/docs/Web/API/Navigator). Si può usare questo oggetto per recuperare informazioni come la lingua preferita dall'utente, un flusso multimediale dalla webcam dell'utente, ecc.
- Il documento (rappresentato dal DOM nei browser) è la pagina effettiva caricata nella finestra, ed è rappresentato in JavaScript dall'oggetto [`Document`](/it/docs/Web/API/Document). Si può usare questo oggetto per restituire e manipolare informazioni sull'HTML e CSS che compongono il documento, per esempio ottenere un riferimento a un elemento nel DOM, cambiare il suo contenuto di testo, applicare nuovi stili ad esso, creare nuovi elementi e aggiungerli all'elemento corrente come figli, o persino eliminarlo del tutto.

In questo articolo ci concentreremo principalmente sulla manipolazione del documento, ma mostreremo anche altri elementi utili.

## Il modello a oggetti del documento

Facciamo un breve riassunto sul Document Object Model (DOM), che abbiamo anche visto in precedenza nel corso. Il documento attualmente caricato in ciascuna delle sue schede del browser è rappresentato da un DOM. Questa è una rappresentazione "a struttura ad albero" creata dal browser che permette alla struttura HTML di essere facilmente accessibile dai linguaggi di programmazione — per esempio il browser stesso la utilizza per applicare stili e altre informazioni agli elementi corretti durante il rendering di una pagina, e sviluppatori come lei possono manipolare il DOM con JavaScript dopo che la pagina è stata renderizzata.

Abbiamo creato una pagina esempio su [dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example.html) ([veda anche dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/dom-example.html)). Provi ad aprirla nel suo browser — è una pagina molto semplice contenente un elemento {{htmlelement("section")}} all'interno del quale si trova un'immagine, e un paragrafo con un link all'interno. Il codice sorgente HTML appare così:

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

![Rappresentazione a struttura ad albero del Document Object Model: Il nodo in alto è il doctype e l'elemento HTML. I nodi figli di HTML includono head e body. Ogni elemento figlio è un ramo. Tutti i testi, anche gli spazi bianchi, sono mostrati anch'essi.](dom-screenshot.png)

> [!NOTE]
> Questo diagramma dell'albero DOM è stato creato usando il [Live DOM viewer](https://software.hixie.ch/utilities/js/live-dom-viewer/) di Ian Hickson.

Ogni voce nell'albero è chiamata **nodo**. Si può vedere nel diagramma sopra che alcuni nodi rappresentano elementi (identificati come `HTML`, `HEAD`, `META` e così via) e altri rappresentano testo (identificato come `#text`). Ci sono [altri tipi di nodi](/it/docs/Web/API/Node/nodeType), ma questi sono i principali che incontrerà.

I nodi sono anche riferiti dalla loro posizione nell'albero relativa ad altri nodi:

- **Nodo radice**: Il nodo più alto nell'albero, che nel caso dell'HTML è sempre il nodo `HTML` (altri vocabolari di markup come SVG e XML personalizzati avranno elementi radice differenti).
- **Nodo figlio**: Un nodo _direttamente_ dentro un altro nodo. Ad esempio, `IMG` è un figlio di `SECTION` nell'esempio sopra.
- **Nodo discendente**: Un nodo _in qualsiasi punto_ dentro un altro nodo. Ad esempio, `IMG` è un figlio di `SECTION` nell'esempio sopra, ed è anche un discendente. `IMG` non è un figlio di `BODY`, in quanto è due livelli sotto di esso nell'albero, ma è un discendente di `BODY`.
- **Nodo padre**: Un nodo che ha un altro nodo dentro di esso. Ad esempio, `BODY` è il nodo padre di `SECTION` nell'esempio sopra.
- **Nodi fratelli**: Nodi che si trovano allo stesso livello sotto lo stesso nodo padre nell'albero del DOM. Ad esempio, `IMG` e `P` sono fratelli nell'esempio sopra.

È utile familiarizzare con questa terminologia prima di lavorare con il DOM, poiché alcuni termini di codice che incontrerà fanno uso di essa. Li incontrerà anche in CSS (ad es., selettore discendente, selettore figlio).

## Apprendimento attivo: Manipolazione base del DOM

Per iniziare a imparare la manipolazione del DOM, iniziamo con un esempio pratico.

1. Prenda una copia locale della pagina [dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example.html) e dell'[immagine](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dinosaur.png) associata.
2. Aggiungere un elemento `<script></script>` appena sopra il tag di chiusura `</body>`.
3. Per manipolare un elemento all'interno del DOM, deve prima selezionarlo e memorizzare un riferimento ad esso all'interno di una variabile. All'interno del suo elemento script, aggiungere la seguente riga:

   ```js
   const link = document.querySelector("a");
   ```

4. Ora che abbiamo memorizzato il riferimento all'elemento in una variabile, possiamo iniziare a manipolarlo usando proprietà e metodi disponibili (questi sono definiti su interfacce come [`HTMLAnchorElement`](/it/docs/Web/API/HTMLAnchorElement) nel caso dell'elemento {{htmlelement("a")}}, la sua interfaccia genitore più generale [`HTMLElement`](/it/docs/Web/API/HTMLElement), e [`Node`](/it/docs/Web/API/Node) — che rappresenta tutti i nodi in un DOM). Innanzitutto, modifichiamo il testo all'interno del link aggiornando il valore della proprietà [`Node.textContent`](/it/docs/Web/API/Node/textContent). Aggiungere la seguente riga sotto la precedente:

   ```js
   link.textContent = "Mozilla Developer Network";
   ```

5. Dovremmo anche cambiare l'URL a cui il link sta puntando, in modo che non vada nel posto sbagliato quando viene cliccato. Aggiungere la seguente riga, di nuovo in fondo:

   ```js
   link.href = "https://developer.mozilla.org";
   ```

Nota che, come molte cose in JavaScript, ci sono molti modi per selezionare un elemento e memorizzare un riferimento ad esso in una variabile. [`Document.querySelector()`](/it/docs/Web/API/Document/querySelector) è l'approccio moderno consigliato. È conveniente perché permette di selezionare gli elementi usando selettori CSS. La chiamata `querySelector()` sopra corrisponderà al primo elemento {{htmlelement("a")}} che appare nel documento. Se volesse corrispondere e fare cose a più elementi, potrebbe usare [`Document.querySelectorAll()`](/it/docs/Web/API/Document/querySelectorAll), che corrisponde a ogni elemento nel documento che corrisponde al selettore, e memorizza riferimenti a loro in un oggetto simile a [array](/it/docs/Learn_web_development/Core/Scripting/Arrays) chiamato [`NodeList`](/it/docs/Web/API/NodeList).

Ci sono metodi più vecchi disponibili per afferrare riferimenti agli elementi, come:

- [`Document.getElementById()`](/it/docs/Web/API/Document/getElementById), che seleziona un elemento con un determinato valore di attributo `id`, ad esempio `<p id="myId">Il mio paragrafo</p>`. L'ID viene passato alla funzione come parametro, ad es., `const elementRef = document.getElementById('myId')`.
- [`Document.getElementsByTagName()`](/it/docs/Web/API/Document/getElementsByTagName), che restituisce un oggetto simile a un array contenente tutti gli elementi sulla pagina di un determinato tipo, ad esempio `<p>`, `<a>`, ecc. Il tipo di elemento viene passato alla funzione come parametro, ad es., `const elementRefArray = document.getElementsByTagName('p')`.

Questi due funzionano meglio nei browser più vecchi rispetto ai metodi moderni come `querySelector()`, ma non sono così convenienti. Dia un'occhiata e veda quali altri può trovare!

### Creare e posizionare nuovi nodi

Il sopra riportato le ha dato un piccolo assaggio di ciò che può fare, ma andiamo oltre e vediamo come possiamo creare nuovi elementi.

1. Tornando all'esempio corrente, iniziamo afferrando un riferimento al nostro elemento {{htmlelement("section")}} — aggiungere il seguente codice alla fine dello script esistente (faccia lo stesso con le altre righe):

   ```js
   const sect = document.querySelector("section");
   ```

2. Ora creiamo un nuovo paragrafo usando [`Document.createElement()`](/it/docs/Web/API/Document/createElement) e diamogli un contenuto di testo allo stesso modo di prima:

   ```js
   const para = document.createElement("p");
   para.textContent = "We hope you enjoyed the ride.";
   ```

3. Si può ora aggiungere il nuovo paragrafo alla fine della sezione usando [`Node.appendChild()`](/it/docs/Web/API/Node/appendChild):

   ```js
   sect.appendChild(para);
   ```

4. Infine per questa parte, aggiungiamo un nodo di testo al paragrafo che contiene il link, per terminare la frase in modo piacevole. Per prima cosa creeremo il nodo di testo usando [`Document.createTextNode()`](/it/docs/Web/API/Document/createTextNode):

   ```js
   const text = document.createTextNode(
     " — the premier source for web development knowledge.",
   );
   ```

5. Ora otterremo un riferimento al paragrafo che contiene il link, e aggiungeremo il nodo di testo ad esso:

   ```js
   const linkPara = document.querySelector("p");
   linkPara.appendChild(text);
   ```

Questo è la maggior parte di ciò che serve per aggiungere nodi al DOM — farà un grande uso di questi metodi quando costruisce interfacce dinamiche (vedremo alcuni esempi più avanti).

### Spostare e rimuovere elementi

Ci possono essere momenti in cui vuole spostare nodi, o eliminarli completamente dal DOM. Questo è perfettamente possibile.

Se volessimo spostare il paragrafo con il link al suo interno in fondo alla sezione, potremmo fare così:

```js
sect.appendChild(linkPara);
```

Questo sposta il paragrafo in fondo alla sezione. Potrebbe aver pensato che avrebbe creato una seconda copia di esso, ma non è il caso — `linkPara` è un riferimento alla sola e unica copia di quel paragrafo. Se volesse fare una copia e aggiungerla anche, dovrebbe usare [`Node.cloneNode()`](/it/docs/Web/API/Node/cloneNode) invece.

Rimuovere un nodo è abbastanza semplice anche, almeno quando ha un riferimento al nodo da rimuovere e al suo genitore. Nel nostro caso attuale, usiamo solo [`Node.removeChild()`](/it/docs/Web/API/Node/removeChild), così:

```js
sect.removeChild(linkPara);
```

Quando vuole rimuovere un nodo solo in base a un riferimento a sé stesso, cosa abbastanza comune, può usare [`Element.remove()`](/it/docs/Web/API/Element/remove):

```js
linkPara.remove();
```

Questo metodo non è supportato nei browser più vecchi. Non hanno alcun metodo per dire a un nodo di rimuovere sé stesso, quindi dovrebbe fare quanto segue:

```js
linkPara.parentNode.removeChild(linkPara);
```

Provi ad aggiungere le righe sopra al suo codice.

### Manipolare gli stili

È possibile manipolare gli stili CSS tramite JavaScript in una varietà di modi.

Per iniziare, può ottenere una lista di tutti i fogli di stile allegati a un documento usando [`Document.styleSheets`](/it/docs/Web/API/Document/styleSheets), che restituisce un oggetto simile a un array con oggetti [`CSSStyleSheet`](/it/docs/Web/API/CSSStyleSheet). Può quindi aggiungere/rimuovere stili come desidera. Tuttavia, non espanderemo su quelle funzionalità perché sono un modo alquanto arcaico e difficile per manipolare lo stile. Ci sono modi molto più semplici.

Il primo modo è aggiungere stili inline direttamente sugli elementi che vuole stilizzare dinamicamente. Questo è fatto con la proprietà [`HTMLElement.style`](/it/docs/Web/API/HTMLElement/style), che contiene informazioni di stile inline per ciascun elemento nel documento. Può impostare le proprietà di questo oggetto per aggiornare direttamente gli stili degli elementi.

1. Come esempio, provi ad aggiungere queste righe al nostro esempio in corso:

   ```js
   para.style.color = "white";
   para.style.backgroundColor = "black";
   para.style.padding = "10px";
   para.style.width = "250px";
   para.style.textAlign = "center";
   ```

2. Ricarichi la pagina e vedrà che gli stili sono stati applicati al paragrafo. Se guarda quel paragrafo nel [Page Inspector/DOM inspector](https://firefox-source-docs.mozilla.org/devtools-user/page_inspector/index.html) del suo browser, vedrà che queste righe stanno effettivamente aggiungendo stile inline al documento:

   ```html
   <p
     style="color: white; background-color: black; padding: 10px; width: 250px; text-align: center;">
     We hope you enjoyed the ride.
   </p>
   ```

> [!NOTE]
> Noti come le versioni delle proprietà JavaScript degli stili CSS sono scritte in {{Glossary("camel_case", "lower camel case")}} mentre le versioni CSS sono trattini (kebab-case) ({{Glossary("kebab_case", "kebab-case")}}) (ad es., `backgroundColor` versus `background-color`). Si assicuri di non confondersi, altrimenti non funzionerà.

C'è un altro comune modo per manipolare dinamicamente gli stili nel suo documento, che vedremo ora.

1. Elimini le precedenti cinque righe che ha aggiunto al JavaScript.
2. Aggiungere il seguente codice all'interno del suo {{htmlelement("head")}} HTML:

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

3. Ora passeremo a un metodo molto utile per la manipolazione generale dell'HTML — [`Element.setAttribute()`](/it/docs/Web/API/Element/setAttribute) — questo prende due argomenti, l'attributo che si desidera impostare sull'elemento e il valore che si desidera impostare. In questo caso imposteremo un nome di classe di highlight sul nostro paragrafo:

   ```js
   para.setAttribute("class", "highlight");
   ```

4. Ricarichi la sua pagina, e non vedrà alcun cambiamento — il CSS è ancora applicato al paragrafo, ma questa volta dando ad esso una classe che è selezionata dalla nostra regola CSS, non come stili CSS inline.

Quale metodo sceglie è una sua scelta; entrambi hanno i loro vantaggi e svantaggi. Il primo metodo richiede meno configurazione ed è buono per usi semplici, mentre il secondo metodo è più purista (nessuna miscelazione tra CSS e JavaScript, nessun stile inline, che sono visti come una cattiva pratica). Man mano che inizia a costruire app più grandi e più elaborate, probabilmente inizierà a usare di più il secondo metodo, ma è davvero una sua scelta.

A questo punto, non abbiamo ancora fatto nulla di utile! Non ha senso usare JavaScript per creare contenuti statici — tanto vale scriverlo nel suo HTML e non usare JavaScript. È più complesso rispetto all'HTML, e creare i suoi contenuti con JavaScript ha anche altre problematiche associate (come non essere leggibili dai motori di ricerca).

Nella prossima sezione esamineremo un uso più pratico delle API DOM.

> [!NOTE]
> Può trovare la nostra [versione finita di dom-example.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/dom-example-manipulated.html) demo su GitHub ([veda dal vivo anche](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/dom-example-manipulated.html)).

## Apprendimento attivo: Una lista della spesa dinamica

In questa sfida vogliamo fare un semplice esempio di lista della spesa che permetta di aggiungere dinamicamente articoli alla lista utilizzando un input del modulo e un pulsante. Quando aggiunge un elemento all'input e preme il pulsante:

- L'elemento dovrebbe apparire nella lista.
- Ogni elemento dovrebbe avere un pulsante che può essere premuto per eliminare quell'elemento dalla lista.
- L'input dovrebbe essere svuotato e focalizzato pronto per inserire un altro elemento.

Il demo completato avrà un aspetto simile a questo:

![Layout demo di una lista della spesa. Un'intestazione 'la mia lista della spesa' seguita da 'Inserisci un nuovo elemento' con un campo di input e un pulsante 'aggiungi elemento'. La lista degli elementi già aggiunti è sotto, ciascuno con un corrispondente pulsante di eliminazione.](shopping-list.png)

Per completare l'esercizio, segua i passaggi di seguito, e assicurarsi che la lista si comporti come descritto sopra.

1. Per iniziare, scarichi una copia del nostro [file shopping-list.html](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/shopping-list.html) di partenza e faccia una copia da qualche parte. Vedrà che ha del CSS minimo, un div con un'etichetta, un input e un pulsante, e una lista vuota ed elemento {{htmlelement("script")}}. Farà tutte le sue aggiunte all'interno dello script.
2. Crei tre variabili che tengano i riferimenti alla lista ({{htmlelement("ul")}}), {{htmlelement("input")}}, e {{htmlelement("button")}}.
3. Crei una [funzione](/it/docs/Learn_web_development/Core/Scripting/Functions) che si eseguirà in risposta al clic del pulsante.
4. All'interno del corpo della funzione, inizi memorizzando il valore corrente [value](/it/docs/Web/API/HTMLInputElement/value) dell'elemento input in una variabile.
5. Successivamente, svuoti l'elemento input impostando il suo valore su una stringa vuota — `''`.
6. Crei tre nuovi elementi — un elemento lista ({{htmlelement('li')}}), {{htmlelement('span')}}, e {{htmlelement('button')}}, e li memorizzi in variabili.
7. Appenda lo span e il pulsante come figli dell'elemento lista.
8. Imposti il contenuto di testo dello span al valore dell'elemento input che ha salvato precedentemente, e il contenuto di testo del pulsante a 'Delete'.
9. Appenda l'elemento lista come figlio della lista.
10. Allega un gestore degli eventi al pulsante di eliminazione in modo che, quando cliccato, elimini l'intero elemento della lista (`<li>...</li>`).
11. Infine, utilizzi il metodo [`focus()`](/it/docs/Web/API/HTMLElement/focus) per focalizzare l'elemento input pronto per l'inserimento del prossimo elemento della lista della spesa.

> [!NOTE]
> Se si blocca davvero, dia un'occhiata alla nostra [lista della spesa finita](https://github.com/mdn/learning-area/blob/main/javascript/apis/document-manipulation/shopping-list-finished.html) ([veda anche in esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/apis/document-manipulation/shopping-list-finished.html)).

## Sommario

Abbiamo raggiunto la fine del nostro studio della manipolazione del documento e del DOM. A questo punto dovrebbe capire quali sono le parti importanti di un browser web rispetto al controllo dei documenti e altri aspetti dell'esperienza web dell'utente. Soprattutto, dovrebbe capire cos'è il Document Object Model, e come manipolarlo per creare funzionalità utili.

## Vedi anche

- Ci sono molte più funzionalità che può utilizzare per manipolare i suoi documenti. Controlli alcune delle nostre referenze e veda cosa può scoprire:
  - [`Document`](/it/docs/Web/API/Document)
  - [`Window`](/it/docs/Web/API/Window)
  - [`Node`](/it/docs/Web/API/Node)
  - [`HTMLElement`](/it/docs/Web/API/HTMLElement), [`HTMLInputElement`](/it/docs/Web/API/HTMLInputElement), [`HTMLImageElement`](/it/docs/Web/API/HTMLImageElement), ecc.
- [DOM Scripting](https://explainers.dev/dom-scripting/), explainers.dev

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Object_basics","Learn_web_development/Core/Scripting/Network_requests", "Learn_web_development/Core/Scripting")}}
